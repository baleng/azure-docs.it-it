---
title: Esercitazione per la ricerca di dati JSON nell'archivio BLOB di Azure - Ricerca di Azure
description: Questa esercitazione descrive come cercare dati BLOB di Azure semistrutturati tramite Ricerca di Azure.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 1c8ce14dd3961eff33a54a14c2bd0b27650d8a50
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58201348"
---
# <a name="tutorial-search-semi-structured-data-in-azure-cloud-storage"></a>Esercitazione: Cercare dati semistrutturati nell'archiviazione cloud di Azure

Ricerca di Azure consente di indicizzare i documenti e le matrici JSON in archiviazione BLOB di Azure usando un [indicizzatore](search-indexer-overview.md) in grado di leggere dati semistrutturati. I dati semistrutturati contengono tag o contrassegni che separano il contenuto all'interno dei dati. Si differenziano dai dati non strutturati, che devono essere completamente indicizzati, e dai dati strutturati formalmente in base a un modello di dati, ad esempio uno schema di database relazionale, che può essere indicizzato campo per campo.

In questa esercitazione usare le [API REST Ricerca di Azure](https://docs.microsoft.com/rest/api/searchservice/) e un client REST per eseguire le attività seguenti:

> [!div class="checklist"]
> * Configurare un'origine dati di Ricerca di Azure per un contenitore BLOB di Azure
> * Creare un indice di Ricerca di Azure in cui includere contenuto ricercabile
> * Configurare ed eseguire un indicizzatore per leggere il contenitore ed estrarre contenuto ricercabile da archiviazione BLOB di Azure
> * Eseguire una ricerca nell'indice che appena creato

> [!NOTE]
> Questa esercitazione si basa sul supporto della matrice JSON, che è attualmente una anteprima funzionalità di Ricerca di Azure. Non è disponibile nel portale di Azure. Per questo motivo, si usano l'API REST in anteprima che fornisce questa funzionalità e uno strumento client REST per chiamare l'API.

## <a name="prerequisites"></a>Prerequisiti

[Creare un servizio Ricerca di Azure](search-create-service-portal.md) o [trovare un servizio esistente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) nella sottoscrizione corrente. È possibile usare un servizio gratuito per questa esercitazione.

[Creare un account di archiviazione di Azure](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) in cui includere i dati di esempio.

[Usare Postman](https://www.getpostman.com/) o un altro client REST per inviare le richieste. Le istruzioni per configurare una richiesta HTTP in Postman vengono fornite nella sezione successiva.

## <a name="set-up-postman"></a>Configurare Postman

Avviare Postman e configurare una richiesta HTTP. Se non si ha familiarità con questo strumento, vedere [Esplorare le API REST di Ricerca di Azure con Postman](search-fiddler.md) per altre informazioni.

Il metodo di richiesta per ogni chiamata in questa esercitazione è "POST". Le chiavi di intestazione sono "Content-type" e "api-key". I valori delle chiavi di intestazione sono rispettivamente "application/json" e la chiave di amministrazione, ovvero un segnaposto per la chiave primaria di ricerca. Il corpo è l'area in cui si posiziona il contenuto effettivo della chiamata. A seconda del client in uso, la procedura per la creazione della query potrebbe essere leggermente diversa, ma questi sono i criteri di base.

  ![Ricerca su dati semistrutturati](media/search-semi-structured-data/postmanoverview.png)

Per le chiamate REST illustrate in questa esercitazione, la chiave API per la ricerca è obbligatoria. È possibile trovare la chiave API in **Chiavi** all'interno del servizio di ricerca. Questa chiave API deve essere nell'intestazione di ogni chiamata API (sostituire la chiave di amministrazione nello screenshot precedente con questa chiave) che dovrà essere eseguita seguendo le istruzioni in questa esercitazione. Conservare la chiave perché è necessaria per ogni chiamata.

  ![Ricerca su dati semistrutturati](media/search-semi-structured-data/keys.png)

## <a name="prepare-sample-data"></a>Preparare i dati di esempio

1. **Scaricare [clinical-trials-json.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip)** e decomprimerlo nella relativa cartella. I dati provengono da [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results) e sono stati convertiti in JSON per questa esercitazione.

2. Accedere al [portale di Azure](https://portal.azure.com), passare all'account di archiviazione di Azure, aprire il contenitore di **dati** e fare clic su **Carica**.

3. Fare clic su **Avanzate**, immettere "clinical-trials-json" e quindi caricare tutti i file JSON scaricati.

  ![Ricerca su dati semistrutturati](media/search-semi-structured-data/clinicalupload.png)

Dopo aver completato il caricamento, i file dovrebbero essere visualizzati nella rispettiva sottocartella all'interno del contenitore dei dati.

## <a name="connect-your-search-service-to-your-container"></a>Connettere il servizio di ricerca al contenitore

Viene usato Postman per effettuare tre chiamate API al servizio di ricerca per creare un'origine dati, un indice e un indicizzatore. L'origine dati include un puntatore all'account di archiviazione e ai dati JSON. Il servizio di ricerca stabilisce la connessione durante il caricamento dei dati.

La stringa di query deve contenere un'API di anteprima, ad esempio **api-version=2017-11-11-Preview** e ogni chiamata deve restituire **201 Creato**. La versione dell'API disponibile a livello generale non ha ancora la capacità di gestire elementi JSON come jsonArray, funzionalità attualmente disponibile solo nella versione di anteprima dell'API.

Eseguire le tre chiamate dell'API seguenti dal client REST.

## <a name="create-a-data-source"></a>Creare un'origine dati

Un'origine dati è un oggetto di Ricerca di Azure che specifica quali dati indicizzare.

L'endpoint di questa chiamata è `https://[service name].search.windows.net/datasources?api-version=2016-09-01-Preview`. Sostituire `[service name]` con il nome del servizio di ricerca. Per questa chiamata, sono necessari il nome dell'account di archiviazione e la chiave dell'account di archiviazione. La chiave dell'account di archiviazione è reperibile nel portale di Azure tra le **chiavi di accesso** dell'account di archiviazione. La posizione è indicata nell'immagine seguente:

  ![Ricerca su dati semistrutturati](media/search-semi-structured-data/storagekeys.png)

Assicurarsi di sostituire `[storage account name]` e `[storage account key]` nel corpo della chiamata prima di eseguire la chiamata.

```json
{
    "name" : "clinical-trials-json",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=[storage account name];AccountKey=[storage account key];" },
    "container" : { "name" : "data", "query" : "clinical-trials-json" }
}
```

La risposta dovrebbe essere simile alla seguente:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#datasources/$entity",
    "@odata.etag": "\"0x8D505FBC3856C9E\"",
    "name": "clinical-trials-json",
    "description": null,
    "type": "azureblob",
    "subtype": null,
    "credentials": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=[mystorageaccounthere];AccountKey=[[myaccountkeyhere]]];"
    },
    "container": {
        "name": "data",
        "query": "clinical-trials-json"
    },
    "dataChangeDetectionPolicy": null,
    "dataDeletionDetectionPolicy": null
}
```

## <a name="create-an-index"></a>Creare un indice
    
La seconda chiamata dell'API crea un indice di Ricerca di Azure. Un indice specifica tutti i parametri e i relativi attributi.

L'URL per questa chiamata è `https://[service name].search.windows.net/indexes?api-version=2016-09-01-Preview`. Sostituire `[service name]` con il nome del servizio di ricerca.

Sostituire prima di tutto l'URL, quindi copiare e incollare il codice seguente nel corpo ed eseguire la query.

```json
{
  "name": "clinical-trials-json-index",  
  "fields": [
  {"name": "FileName", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "Description", "type": "Edm.String", "searchable": true, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MinimumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Title", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "URL", "type": "Edm.String", "searchable": false, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MyURL", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "Gender", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "MaximumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Summary", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "NCTID", "type": "Edm.String", "key": true, "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "Phase", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Date", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "OverallStatus", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "OrgStudyId", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": false},
  {"name": "HealthyVolunteers", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Keywords", "type": "Collection(Edm.String)", "searchable": true, "retrievable": true, "facetable": true, "filterable": false, "sortable": false},
  {"name": "metadata_storage_last_modified", "type":"Edm.DateTimeOffset", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_storage_size", "type":"Edm.String", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_content_type", "type":"Edm.String", "searchable": true, "retrievable": true, "filterable": true, "sortable": false}
  ],
  "suggesters": [
  {
    "name": "sg",
    "searchMode": "analyzingInfixMatching",
    "sourceFields": ["Title"]
  }
  ]
}
```

La risposta dovrebbe essere simile alla seguente:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexes/$entity",
    "@odata.etag": "\"0x8D505FC00EDD5FA\"",
    "name": "clinical-trials-json-index",
    "fields": [
        {
            "name": "FileName",
            "type": "Edm.String",
            "searchable": false,
            "filterable": false,
            "retrievable": true,
            "sortable": true,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        {
            "name": "Description",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "retrievable": false,
            "sortable": false,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        ...
          "scoringProfiles": [],
    "defaultScoringProfile": null,
    "corsOptions": null,
    "suggesters": [],
    "analyzers": [],
    "tokenizers": [],
    "tokenFilters": [],
    "charFilters": []
}
```

## <a name="create-and-run-an-indexer"></a>Creare ed eseguire un indicizzatore

Un indicizzatore connette l'origine dati, importa i dati nell'indice di ricerca di destinazione e facoltativamente fornisce una pianificazione per automatizzare l'aggiornamento dei dati.

L'URL per questa chiamata è `https://[service name].search.windows.net/indexers?api-version=2016-09-01-Preview`. Sostituire `[service name]` con il nome del servizio di ricerca.

Sostituire prima di tutto l'URL, quindi copiare e incollare il codice seguente nel corpo e inviare la richiesta. La richiesta viene elaborata immediatamente. Quando viene restituita la risposta, si avrà un indice da usare per la ricerca full-text.

```json
{
  "name" : "clinical-trials-json-indexer",
  "dataSourceName" : "clinical-trials-json",
  "targetIndexName" : "clinical-trials-json-index",
  "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
}
```

La risposta dovrebbe essere simile alla seguente:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexers/$entity",
    "@odata.etag": "\"0x8D505FDE143D164\"",
    "name": "clinical-trials-json-indexer",
    "description": null,
    "dataSourceName": "clinical-trials-json",
    "targetIndexName": "clinical-trials-json-index",
    "schedule": null,
    "parameters": {
        "batchSize": null,
        "maxFailedItems": null,
        "maxFailedItemsPerBatch": null,
        "base64EncodeKeys": null,
        "configuration": {
            "parsingMode": "jsonArray"
        }
    },
    "fieldMappings": [],
    "enrichers": [],
    "disabled": null
}
```

## <a name="search-your-json-files"></a>Cercare i file JSON

È possibile inviare query sull'indice. Per questa attività, usare [**Esplora ricerche** ](search-explorer.md)nel portale.

  ![Ricerca su dati non strutturati](media/search-semi-structured-data/indexespane.png)

### <a name="user-defined-metadata-search"></a>Ricerca nei metadati definiti dall'utente

Come in precedenza, è possibile eseguire ricerche nei dati in vari modi: ricerca full-text, proprietà di sistema o metadati definiti dall'utente. Le ricerche nelle proprietà di sistema e nei metadati definiti dall'utente possono essere eseguite solo con il parametro `$select` se questi elementi sono stati contrassegnati come **recuperabili** durante la creazione dell'indice di destinazione. I parametri nell'indice non possono essere modificati dopo la creazione. È comunque possibile aggiungere ulteriori parametri.

Un esempio di query semplice è `$select=Gender,metadata_storage_size`, che limita i risultati a questi due parametri.

  ![Ricerca su dati semistrutturati](media/search-semi-structured-data/lastquery.png)

Un esempio di query più complessa può essere `$filter=MinimumAge ge 30 and MaximumAge lt 75`, che restituisce solo i risultati in cui il parametro MinimumAge è maggiore o uguale a 30 e il parametro MaximumAge è minore di 75.

  ![Ricerca su dati semistrutturati](media/search-semi-structured-data/metadatashort.png)

È possibile sperimentare e provare altre query in autonomia, se lo si desidera. Tenere presente che è possibile usare gli operatori logici (and, or e not) e gli operatori di confronto (eq, ne, gt, lt, ge e le). Per i confronti tra stringhe viene fatta distinzione tra maiuscole e minuscole.

Il parametro `$filter` funziona solo con i metadati contrassegnati come filtrabili al momento della creazione dell'indice.

## <a name="clean-up-resources"></a>Pulire le risorse

Il modo più veloce per pulire le risorse dopo un'esercitazione consiste nell'eliminare il gruppo di risorse contenente il servizio Ricerca di Azure. È possibile eliminare ora il gruppo di risorse per eliminare definitivamente tutti gli elementi in esso contenuti. Nel portale, il nome del gruppo di risorse è indicato nella pagina Panoramica del servizio Ricerca di Azure.

## <a name="next-steps"></a>Passaggi successivi

È possibile collegare gli algoritmi basati su intelligenza artificiale alla pipeline di un indicizzatore. Come passaggio successivo, continuare con l'esercitazione seguente.

> [!div class="nextstepaction"]
> [Indicizzazione di documenti in Archiviazione BLOB di Azure](search-howto-indexing-azure-blob-storage.md)
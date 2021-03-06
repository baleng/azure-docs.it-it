---
title: 'Caricamento: Data Prep SDK per Python'
titleSuffix: Azure Machine Learning service
description: Informazioni sul caricamento dei dati con Azure Machine Learning Data Prep SDK. È possibile caricare diversi tipi di dati di input, specificare i parametri e i tipi di file di dati oppure usare la funzionalità di lettura smart SDK per rilevare automaticamente il tipo di file.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: jmartens
ms.date: 2/22/2019
ms.custom: seodec18
ms.openlocfilehash: 34dd20826928d1ab2ba1fc7980c7d47b796ea663
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2019
ms.locfileid: "58311017"
---
# <a name="load-and-read-data-with-azure-machine-learning"></a>Caricare e leggere dati con Azure Machine Learning

In questo articolo descrive diversi metodi di caricamento dei dati usando il SDK di Azure Machine Learning Data Prep. Per visualizzare la documentazione di riferimento per il SDK, vedere la [Panoramica](https://aka.ms/data-prep-sdk). L'SDK supporta più funzionalità di inserimento dati, tra cui:

* Caricamento da numerosi tipi di file con inferenza dei parametri di analisi (codifica, separatore, intestazioni)
* Conversione del tipo mediante l'inferenza durante il caricamento dei file
* Supporto delle connessioni per Microsoft SQL Server e Azure Data Lake Storage

Nella tabella seguente mostra una selezione delle funzioni utilizzate per il caricamento dei dati da tipi di file comuni.

| Tipo file | Funzione | Collegamento di riferimento |
|-------|-------|-------|
|Qualsiasi|`auto_read_file()`|[reference](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep?view=azure-dataprep-py#auto-read-file-path--filepath--include-path--bool---false-----azureml-dataprep-api-dataflow-dataflow)|
|Text|`read_lines()`|[reference](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep?view=azure-dataprep-py#read-lines-path--filepath--header--azureml-dataprep-api-engineapi-typedefinitions-promoteheadersmode----promoteheadersmode-none--0---encoding--azureml-dataprep-api-engineapi-typedefinitions-fileencoding----fileencoding-utf8--0---skip-rows--int---0--skip-mode--azureml-dataprep-api-engineapi-typedefinitions-skipmode----skipmode-none--0---comment--str---none--include-path--bool---false-----azureml-dataprep-api-dataflow-dataflow)|
|CSV|`read_csv()`|[reference](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep?view=azure-dataprep-py#read-csv-path--filepath--separator--str--------header--azureml-dataprep-api-engineapi-typedefinitions-promoteheadersmode----promoteheadersmode-constantgrouped--3---encoding--azureml-dataprep-api-engineapi-typedefinitions-fileencoding----fileencoding-utf8--0---quoting--bool---false--inference-arguments--azureml-dataprep-api-builders-inferencearguments---none--skip-rows--int---0--skip-mode--azureml-dataprep-api-engineapi-typedefinitions-skipmode----skipmode-none--0---comment--str---none--include-path--bool---false--archive-options--azureml-dataprep-api--archiveoption-archiveoptions---none-----azureml-dataprep-api-dataflow-dataflow)|
|Excel|`read_excel()`|[reference](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep?view=azure-dataprep-py#read-excel-path--filepath--sheet-name--str---none--use-column-headers--bool---false--inference-arguments--azureml-dataprep-api-builders-inferencearguments---none--skip-rows--int---0--include-path--bool---false-----azureml-dataprep-api-dataflow-dataflow)|
|Larghezza fissa|`read_fwf()`|[reference](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep?view=azure-dataprep-py#read-fwf-path--filepath--offsets--typing-list-int---header--azureml-dataprep-api-engineapi-typedefinitions-promoteheadersmode----promoteheadersmode-constantgrouped--3---encoding--azureml-dataprep-api-engineapi-typedefinitions-fileencoding----fileencoding-utf8--0---inference-arguments--azureml-dataprep-api-builders-inferencearguments---none--skip-rows--int---0--skip-mode--azureml-dataprep-api-engineapi-typedefinitions-skipmode----skipmode-none--0---include-path--bool---false-----azureml-dataprep-api-dataflow-dataflow)|
|JSON|`read_json()`|[reference](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep?view=azure-dataprep-py#read-json-path--filepath--encoding--azureml-dataprep-api-engineapi-typedefinitions-fileencoding----fileencoding-utf8--0---flatten-nested-arrays--bool---false--include-path--bool---false-----azureml-dataprep-api-dataflow-dataflow)|

## <a name="load-data-automatically"></a>Caricare automaticamente i dati

Per caricare automaticamente i dati senza specificare il tipo di file, usare la funzione `auto_read_file()`. Il tipo di file e gli argomenti richiesti per leggerlo vengono dedotti automaticamente.

```python
import azureml.dataprep as dprep

dflow = dprep.auto_read_file(path='./data/any-file.txt')
```

Questa funzione è utile per rilevare automaticamente il tipo di file, la codifica e altri argomenti di analisi, tutto da un unico comodo punto di ingresso. La funzione, inoltre, esegue automaticamente i seguenti passaggi comunemente eseguiti quando si caricano dati delimitati:

* Derivare e impostare il delimitatore
* Ignorare le righe vuote nella parte superiore del file
* Derivare e impostare la riga di intestazione

In alternativa, se si conosce il file digitare anticipatamente e si vuole controllare in che modo che viene analizzato in modo esplicito, usare le funzioni specifiche del file.

## <a name="load-text-line-data"></a>Caricamento di dati di righe di testo

Per leggere dati di testo semplice in un flusso di dati, usare `read_lines()` senza specificare parametri facoltativi.

```python
dflow = dprep.read_lines(path='./data/text_lines.txt')
dflow.head(5)
```

||Grafico a linee|
|----|-----|
|0|Data \| \|  Temperatura minima \| \|  Temperatura massima|
|1|01-07-2015 \|\|  -4,1 \|\|  10,0|
|2|02-07-2015 \|\|  -0,8 \|\|  10,8|


Dopo l'inserimento dei dati, eseguire il codice seguente per convertire l'oggetto flusso di dati in un dataframe Pandas.

```python
pandas_df = dflow.to_pandas_dataframe()
```

## <a name="load-csv-data"></a>Caricare dati CSV

Durante la lettura di file delimitati, il runtime sottostante può dedurre i parametri di analisi, ad esempio il separatore, la codifica, l'uso o meno di intestazioni e così via. Eseguire il codice seguente per tentare di leggere un file specificandone solo la posizione.

```python
dflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv?st=2018-06-15T23%3A01%3A42Z&se=2019-06-16T23%3A01%3A00Z&sp=r&sv=2017-04-17&sr=b&sig=ugQQCmeC2eBamm6ynM7wnI%2BI3TTDTM6z9RPKj4a%2FU6g%3D')
dflow.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|-----|
|0|stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|1|ALABAMA|1|101710|Contea Hale|10171002158| |
|2|ALABAMA|1|101710|Contea Hale|10171002162| |


Per escludere righe durante il caricamento, definire il parametro `skip_rows`. Questo parametro non caricherà le righe specificate nel file CSV (usando un indice in base uno).

```python
dflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv',
                          skip_rows=1)
dflow.head(5)
```

| |stnam|fipst|leaid|leanm10|ncessch|MAM_MTH00numvalid_1011|
|-----|-------|---------| -------|------|-----|------|
|0|ALABAMA|1|101710|Contea Hale|10171002158|29|
|1|ALABAMA|1|101710|Contea Hale|10171002162|40 |

Eseguire il codice seguente per visualizzare i tipi di dati delle colonne.

```python
dflow.dtypes
```
Output:

    stnam                     object
    fipst                     object
    leaid                     object
    leanm10                   object
    ncessch                   object
    schnam10                  object
    MAM_MTH00numvalid_1011    object
    dtype: object

Per impostazione predefinita, Azure Machine Learning Data Prep SDK non modifica il tipo di dati. L'origine dati da cui si sta leggendo è un file di testo, pertanto l'SDK legge tutti i valori come stringhe. Per questo esempio le colonne numeriche devono essere analizzate come numeri. Impostare il parametro `inference_arguments` su `InferenceArguments.current_culture()` per dedurre e convertire automaticamente i tipi di colonna durante la lettura del file.

```
dflow = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv',
                          skip_rows=1,
                          inference_arguments=dprep.InferenceArguments.current_culture())
dflow.dtypes
```
Output:

    stnam                      object
    fipst                     float64
    leaid                     float64
    leanm10                    object
    ncessch                   float64
    schnam10                   object
    ALL_MTH00numvalid_1011    float64
    dtype: object


Molte delle colonne sono state rilevate correttamente come numeriche e il relativo tipo è impostato su `float64`.

## <a name="use-excel-data"></a>Usare dati Excel

L'SDK include una funzione `read_excel()` per il caricamento dei file di Excel. Per impostazione predefinita, la funzione carica il primo foglio della cartella di lavoro. Per definire un foglio specifico da caricare, definire il parametro `sheet_name` con il valore di stringa del nome del foglio.

```python
dflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2')
dflow.head(5)
```

| |Colonna1|Colonna2|Colonna3|Colonna4|Colonna5|Colonna6|Colonna7|Colonna8| | |
|-|-------|-------|-------|-------|-------|-------|-------|-------|-|-|
|0|Nessuna|Nessuna|Nessuna|Nessuna|Nessuna|Nessuna|Nessuna|Nessuna|Nessuna| |
|1|Nessuna|Nessuna|Nessuna|Nessuna|Nessuna|Nessuna|Nessuna|Nessuna|Nessuna| |
|2|Nessuna|Nessuna|Nessuna|Nessuna|Nessuna|Nessuna|Nessuna|Nessuna|Nessuna| |
|3|RANK|Title|Studio|In tutto il mondo|Nazionale / %|Colonna1|Oltremare / %|Colonna2|Anno^| |
|4|1|Avatar|Fox|2788|760,5|0,273|2027,5|0,727|2009^|5|

L'output mostra che i dati nel secondo foglio contenevano tre righe vuote prima delle intestazioni. La funzione `read_excel()` contiene parametri facoltativi che indicano di ignorare le righe e usare le intestazioni. Eseguire il codice seguente per ignorare le prime tre righe e usare la quarta riga come riga di intestazioni.

```python
dflow = dprep.read_excel(path='./data/excel.xlsx', sheet_name='Sheet2', use_column_headers=True, skip_rows=3)
```

||RANK|Title|Studio|In tutto il mondo|Nazionale / %|Colonna1|Oltremare / %|Colonna2|Anno^|
|------|------|------|-----|------|-----|-------|----|-----|-----|
|0|1|Avatar|Fox|2788|760,5|0,273|2027,5|0,727|2009^|
|1|2|Titanic|Par.|2186,8|658,7|0,301|1528,1|0,699|1997^|

## <a name="load-fixed-width-data-files"></a>Caricare file di dati a larghezza fissa

Per caricare i file a larghezza fissa, si specifica un elenco degli offset di carattere. Si presuppone sempre che la prima colonna inizi all'offset zero.

```python
dflow = dprep.read_fwf('./data/fixed_width_file.txt', offsets=[7, 13, 43, 46, 52, 58, 65, 73])
dflow.head(5)
```

||010000|99999|BOGUS NORWAY|NO|NO_1|ENRS|Colonna7|Colonna8|Colonna9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010003|99999|BOGUS NORWAY|NO|NO|ENSO||||
|1|010010|99999|JAN MAYEN|NO|JN|ENJA|+70933|-008667|+00090|


Per evitare il rilevamento delle intestazioni e analizzare i dati corretti, passare `PromoteHeadersMode.NONE` al parametro `header`.

```python
dflow = dprep.read_fwf('./data/fixed_width_file.txt',
                          offsets=[7, 13, 43, 46, 52, 58, 65, 73],
                          header=dprep.PromoteHeadersMode.NONE)
```

||Colonna1|Colonna2|Colonna3|Colonna4|Colonna5|Colonna6|Colonna7|Colonna8|Colonna9|
|------|------|------|-----|------|-----|-------|----|-----|----|
|0|010000|99999|BOGUS NORWAY|NO|NO_1|ENRS|Colonna7|Colonna8|Colonna9|
|1|010003|99999|BOGUS NORWAY|NO|NO|ENSO||||


## <a name="load-sql-data"></a>Caricare dati SQL

L'SDK può caricare anche i dati di un'origine SQL. Attualmente, è supportato solo Microsoft SQL Server. Per leggere i dati da un server SQL, creare un [ `MSSQLDataSource` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep.mssqldatasource?view=azure-dataprep-py) oggetto che contiene i parametri di connessione. Il parametro della password di `MSSQLDataSource` accetta un [ `Secret` ](https://docs.microsoft.com/python/api/azureml-dataprep/azureml.dataprep?view=azure-dataprep-py#register-secret-value--str--id--str---none-----azureml-dataprep-api-engineapi-typedefinitions-secret) oggetto. È possibile compilare un oggetto segreto in due modi:

* Registrare il segreto e il relativo valore con il motore di esecuzione.
* Creare il segreto con solo un `id` (se il valore del segreto è già registrato nell'ambiente di esecuzione) usando `dprep.create_secret("[SECRET-ID]")`.

```python
secret = dprep.register_secret(value="[SECRET-PASSWORD]", id="[SECRET-ID]")

ds = dprep.MSSQLDataSource(server_name="[SERVER-NAME]",
                           database_name="[DATABASE-NAME]",
                           user_name="[DATABASE-USERNAME]",
                           password=secret)
```

Dopo aver creato un oggetto origine dati, è possibile leggere i dati dall'output della query.

```python
dflow = dprep.read_sql(ds, "SELECT top 100 * FROM [SalesLT].[Product]")
dflow.head(5)
```

| |ProductID|NOME|ProductNumber|Colore|StandardCost|ListPrice|Dimensione|Peso|ProductCategoryID|ProductModelID|SellStartDate|SellEndDate|DiscontinuedDate|ThumbNailPhoto|ThumbnailPhotoFileName|rowguid|ModifiedDate| |
|-|---------|----|-------------|-----|------------|---------|----|------|-----------------|--------------|-------------|-----------|----------------|--------------|----------------------|-------|------------|-|
|0|680|HL Road Frame - Nero, 58|FR-R92B-58|Nero|1059,3100|1431,50|58|1016,04|18|6|01-06-2002 00:00:00+00:00|Nessuna|Nessuna|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|43dd68d6-14a4-461F-9069-55309d90ea7e|11-03-2008 |0:01:36.827000 + 00:00|
|1|706|HL Road Frame - Rosso, 58|FR-R92R-58|Rosso|1059,3100|1431,50|58|1016,04|18|6|01-06-2002 00:00:00+00:00|Nessuna|Nessuna|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|9540ff17-2712-4c90-a3d1-8ce5568b2462|11-03-2008 |10:01:36.827000 + 00:00|
|2|707|Sport-100 Casco, Rosso|HL-U509-R|Rosso|13,0863|34,99|Nessuna|Nessuna|35|33|01-07-2005 00:00:00+00:00|Nessuna|Nessuna|b'GIF89aP\x001\x00\xf7\x00\x00\x00\x00\x00\x80...|no_image_available_small.gif|2e1ef41a-c08a-4ff6-8ada-bde58b64a712|11-03-2008 |10:01:36.827000 + 00:00|


## <a name="use-azure-data-lake-storage"></a>Usare Azure Data Lake Storage

Esistono due modi in cui l'SDK può acquisire il token OAuth necessario per accedere ad Azure Data Lake Storage:

* Recuperare il token di accesso da una sessione recente dell'account di accesso dell'utente all'interfaccia della riga di comando di Azure
* Usare un'entità servizio (SP) e un certificato come segreto

### <a name="use-an-access-token-from-a-recent-azure-cli-session"></a>Usare un token di accesso da una sessione recente dell'interfaccia della riga di comando di Azure

Nel computer locale eseguire il comando seguente.

```azurecli
az login
az account show --query tenantId
dflow = read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', tenant='microsoft.onmicrosoft.com')) head = dflow.head(5) head
```

> [!NOTE]
> Se l'account utente è membro di più di un tenant di Azure, è necessario specificare il tenant nel formato di nome host AAD URL.

### <a name="create-a-service-principal-with-the-azure-cli"></a>Creare un'entità servizio con l'interfaccia della riga di comando di Azure

Usare l'interfaccia della riga di comando di Azure per creare un'entità servizio e il certificato corrispondente. Questa entità servizio specifica è configurata con il ruolo `reader`, con il relativo ambito ridotto al solo account di Azure Data Lake Storage "dpreptestfiles".

```azurecli
az account set --subscription "Data Wrangling development"
az ad sp create-for-rbac -n "SP-ADLS-dpreptestfiles" --create-cert --role reader --scopes /subscriptions/35f16a99-532a-4a47-9e93-00305f6c40f2/resourceGroups/dpreptestfiles/providers/Microsoft.DataLakeStore/accounts/dpreptestfiles
```

Questo comando genera l'`appId` e il percorso che porta al file di certificato (in genere nella cartella home). Il file con estensione .crt contiene sia il certificato pubblico sia la chiave privata nel formato PEM.

```
openssl x509 -in adls-dpreptestfiles.crt -noout -fingerprint
```

Per configurare l'ACL per il file system di Azure Data Lake Storage, usare il valore objectId dell'utente. In questo esempio viene invece usata l'entità servizio.

```azurecli
az ad sp show --id "8dd38f34-1fcb-4ff9-accd-7cd60b757174" --query objectId
```

Per configurare gli accessi `Read` e `Execute` per il file system di Azure Data Lake Storage, configurare singolarmente l'ACL per file e cartelle. Questo perché il modello di ACL HDFS sottostante non supporta l'ereditarietà.

```azurecli
az dls fs access set-entry --account dpreptestfiles --acl-spec "user:e37b9b1f-6a5e-4bee-9def-402b956f4e6f:r-x" --path /
az dls fs access set-entry --account dpreptestfiles --acl-spec "user:e37b9b1f-6a5e-4bee-9def-402b956f4e6f:r--" --path /farmers-markets.csv
```

```
certThumbprint = 'C2:08:9D:9E:D1:74:FC:EB:E9:7E:63:96:37:1C:13:88:5E:B9:2C:84'
certificate = ''
with open('./data/adls-dpreptestfiles.crt', 'rt', encoding='utf-8') as crtFile:
    certificate = crtFile.read()

servicePrincipalAppId = "8dd38f34-1fcb-4ff9-accd-7cd60b757174"
```

### <a name="acquire-an-oauth-access-token"></a>Acquisire un token di accesso OAuth

Usare il pacchetto `adal` (`pip install adal`) per creare un contesto di autenticazione sul tenant MSFT e acquisire un token di accesso OAuth. Per Azure Data Lake Store, la risorsa nella richiesta del token deve essere per ' https:\//datalake.azure.net', che è diverso dalla maggior parte delle altre risorse di Azure.

```python
import adal
from azureml.dataprep.api.datasources import DataLakeDataSource

ctx = adal.AuthenticationContext('https://login.microsoftonline.com/microsoft.onmicrosoft.com')
token = ctx.acquire_token_with_client_certificate('https://datalake.azure.net/', servicePrincipalAppId, certificate, certThumbprint)
dflow = dprep.read_csv(path = DataLakeDataSource(path='adl://dpreptestfiles.azuredatalakestore.net/farmers-markets.csv', accessToken=token['accessToken']))
dflow.to_pandas_dataframe().head()
```

||FMID|MarketName|Website|street|city|Contea|
|----|------|-----|----|----|----|----|
|0|1012063|Caledonia Farmers Market Association - Danville|https://sites.google.com/site/caledoniafarmers... ||Danville|Caledonia|
|1|1011871|Stearns Homestead Farmers' Market|http://Stearnshomestead.com |6975 Ridge Road|Parma|Cuyahoga|
|2|1011878|100 Mile Market|https://www.pfcmarkets.com |507 Harrison St|Kalamazoo|Kalamazoo|
|3|1009364|106 S. Main Street Farmers Market|http://thetownofsixmile.wordpress.com/ |106 S. Main Street|Six Mile|||
|4|1010691|10th Street Community Farmers Market|https://agrimissouri.com/... |10th Street e Poplar|Lamar|Barton|

## <a name="next-steps"></a>Passaggi successivi

* Vedere il SDK [Panoramica](https://aka.ms/data-prep-sdk) per esempi di utilizzo e i modelli di progettazione
* Vedere il SDK di Azure Machine Learning Data Prep [esercitazione](tutorial-data-prep.md) per un esempio di risoluzione di uno scenario specifico

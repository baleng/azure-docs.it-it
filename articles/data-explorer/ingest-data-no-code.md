---
title: 'Esercitazione: Inserire dati dei log di diagnostica e attività in Esplora dati di Azure senza una riga di codice'
description: In questa esercitazione verrà descritto come inserire dati in Esplora dati di Azure senza una riga di codice e come eseguire query sui dati.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: tutorial
ms.date: 3/14/2019
ms.openlocfilehash: 5d6b595b442b645f57454e317e6535645f643598
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756834"
---
# <a name="tutorial-ingest-data-in-azure-data-explorer-without-one-line-of-code"></a>Esercitazione: Inserire dati in Esplora dati di Azure senza una riga di codice

Questa esercitazione illustra come inserire i dati dei log di diagnostica e attività in un cluster di Esplora dati di Azure senza scrivere codice. Questo semplice metodo di inserimento consente di iniziare rapidamente a eseguire query in Esplora dati di Azure per l'analisi dei dati.

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Creare tabelle e mapping di inserimento in un database di Esplora dati di Azure.
> * Formattare i dati inseriti usando un criterio di aggiornamento.
> * Creare un [hub eventi](/azure/event-hubs/event-hubs-about) e connetterlo a Esplora dati di Azure.
> * Trasmettere i dati a un hub eventi dai [log di diagnostica di Monitoraggio di Azure](/azure/azure-monitor/platform/diagnostic-logs-overview) e dai [log attività di Monitoraggio di Azure](/azure/azure-monitor/platform/activity-logs-overview).
> * Eseguire query sui dati inseriti usando Esplora dati di Azure.

> [!NOTE]
> Creare tutte le risorse nella stessa località o area di Azure. Questo è un requisito per i log di diagnostica di Monitoraggio di Azure.

## <a name="prerequisites"></a>Prerequisiti

* Se non si ha una sottoscrizione di Azure, creare un [account Azure gratuito](https://azure.microsoft.com/free/) prima di iniziare.
* [Un cluster e un database di Esplora dati di Azure](create-cluster-database-portal.md). In questa esercitazione il nome del database è *TestDatabase*.

## <a name="azure-monitor-data-provider-diagnostic-and-activity-logs"></a>Provider di dati di Monitoraggio di Azure: log di diagnostica e attività

Visualizzare e interpretare i dati forniti dai log di diagnostica e attività seguenti di Monitoraggio di Azure. Verrà creata una pipeline di inserimento in base a questi schemi di dati. Si noti che ogni evento in un log ha una matrice di record. Questa matrice di record verrà suddivisa più avanti nell'esercitazione.

### <a name="diagnostic-logs-example"></a>Esempio di log di diagnostica

I log di diagnostica di Azure sono metriche generate da un servizio di Azure che forniscono dati sul funzionamento del servizio stesso. I dati vengono aggregati con un intervallo di tempo di 1 minuto. Di seguito è riportato un esempio di schema di evento di metrica di Esplora dati di Azure sulla durata di una query:

```json
{
    "records": [
    {
        "count": 14,
        "total": 0,
        "minimum": 0,
        "maximum": 0,
        "average": 0,
        "resourceId": "/SUBSCRIPTIONS/F3101802-8C4F-4E6E-819C-A3B5794D33DD/RESOURCEGROUPS/KEDAMARI/PROVIDERS/MICROSOFT.KUSTO/CLUSTERS/KEREN",
        "time": "2018-12-20T17:00:00.0000000Z",
        "metricName": "QueryDuration",
        "timeGrain": "PT1M"
    },
    {
        "count": 12,
        "total": 0,
        "minimum": 0,
        "maximum": 0,
        "average": 0,
        "resourceId": "/SUBSCRIPTIONS/F3101802-8C4F-4E6E-819C-A3B5794D33DD/RESOURCEGROUPS/KEDAMARI/PROVIDERS/MICROSOFT.KUSTO/CLUSTERS/KEREN",
        "time": "2018-12-21T17:00:00.0000000Z",
        "metricName": "QueryDuration",
        "timeGrain": "PT1M"
    }
    ]
}
```

### <a name="activity-logs-example"></a>Esempio di log attività

I log attività di Azure sono log a livello di sottoscrizione che forniscono informazioni sulle operazioni eseguite sulle risorse nella sottoscrizione. Di seguito è riportato un esempio di evento di log attività per il controllo dell'accesso:

```json
{
    "records": [
    {
        "time": "2018-12-26T16:23:06.1090193Z",
        "resourceId": "/SUBSCRIPTIONS/F80EB51C-C534-4F0B-80AB-AEBC290C1C19/RESOURCEGROUPS/CLEANUPSERVICE/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Start",
        "resultSignature": "Started.",
        "durationMs": 0,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            ...
        }
    },
    {
        "time": "2018-12-26T16:23:06.3040244Z",
        "resourceId": "/SUBSCRIPTIONS/F80EB51C-C534-4F0B-80AB-AEBC290C1C19/RESOURCEGROUPS/CLEANUPSERVICE/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Success",
        "resultSignature": "Succeeded.OK",
        "durationMs": 194,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            "statusCode": "OK",
            "serviceRequestId": "87acdebc-945f-4c0c-b931-03050e085626"
        }
    }]
}
```

## <a name="set-up-an-ingestion-pipeline-in-azure-data-explorer"></a>Configurare una pipeline di inserimento in Esplora dati di Azure

La configurazione di una pipeline di Esplora dati di Azure prevede diversi passaggi come [la creazione di una tabella e l'inserimento dei dati](/azure/data-explorer/ingest-sample-data#ingest-data). È anche possibile manipolare, mappare e aggiornare i dati.

### <a name="connect-to-the-azure-data-explorer-web-ui"></a>Connettersi all'interfaccia utente Web di Esplora dati di Azure

Nel database *TestDatabase* di Esplora dati di Azure selezionare **Query** per aprire l'interfaccia utente Web di Esplora dati di Azure.

![Pagina Query](media/ingest-data-no-code/query-database.png)

### <a name="create-the-target-tables"></a>Creare le tabelle di destinazione

La struttura dei log di Monitoraggio di Azure non è tabellare. Si manipoleranno i dati e si espanderà ogni evento per uno o più record. I dati non elaborati verranno inseriti in una tabella intermedia denominata *ActivityLogsRawRecords* per i log attività e *DiagnosticLogsRawRecords* per i log di diagnostica. A quel punto, i dati verranno manipolati ed espansi. Usando un criterio di aggiornamento, i dati espansi verranno inseriti nella tabella *ActivityLogsRecords* per i log attività e *DiagnosticLogsRecords* per i log di diagnostica. Ciò significa che sarà necessario creare due tabelle separate per l'inserimento dei log attività e due tabelle separate per l'importazione dei log di diagnostica.

Usare l'interfaccia utente Web di Esplora dati di Azure per creare le tabelle di destinazione nel database di Esplora dati di Azure.

#### <a name="the-diagnostic-logs-table"></a>Tabella dei log di diagnostica

1. Nel database *TestDatabase* creare una tabella denominata *DiagnosticLogsRecords* per archiviare i record di log di diagnostica. Usare il comando di controllo seguente `.create table`:

    ```kusto
    .create table DiagnosticLogsRecords (Timestamp:datetime, ResourceId:string, MetricName:string, Count:int, Total:double, Minimum:double, Maximum:double, Average:double, TimeGrain:string)
    ```

1. Selezionare **Run** (Esegui) per creare la tabella.

    ![Esegui query](media/ingest-data-no-code/run-query.png)

1. Creare la tabella dati intermedia denominata *DiagnosticLogsRawRecords* nel database *TestDatabase* per la manipolazione dei dati usando la query seguente. Selezionare **Run** (Esegui) per creare la tabella.

    ```kusto
    .create table DiagnosticLogsRawRecords (Records:dynamic)
    ```

#### <a name="the-activity-logs-tables"></a>Tabelle dei log attività

1. Creare la tabella denominata *ActivityLogsRecords* nel database *TestDatabase* per ricevere i record dei log attività. Per creare la tabella, eseguire la query di Esplora dati di Azure seguente:

    ```kusto
    .create table ActivityLogsRecords (Timestamp:datetime, ResourceId:string, OperationName:string, Category:string, ResultType:string, ResultSignature:string, DurationMs:int, IdentityAuthorization:dynamic, IdentityClaims:dynamic, Location:string, Level:string)
    ```

1. Creare la tabella dati intermedia denominata *ActivityLogsRawRecords* nel database *TestDatabase* per la manipolazione dei dati:

    ```kusto
    .create table ActivityLogsRawRecords (Records:dynamic)
    ```

<!--
     ```kusto
     .alter-merge table ActivityLogsRawRecords policy retention softdelete = 0d
    <[Retention](/azure/kusto/management/retention-policy) for an intermediate data table is set at zero retention policy.
-->

### <a name="create-table-mappings"></a>Creare i mapping della tabella

 Poiché il formato dei dati è `json`, è necessario eseguirne il mapping. Il mapping `json` mappa ogni percorso JSON al nome di una colonna della tabella.

#### <a name="table-mapping-for-diagnostic-logs"></a>Mapping della tabella per i log di diagnostica

Per eseguire il mapping dei dati dei log di diagnostica con la tabella, usare la query seguente:

```kusto
.create table DiagnosticLogsRawRecords ingestion json mapping 'DiagnosticLogsRawRecordsMapping' '[{"column":"Records","path":"$.records"}]'
```

#### <a name="table-mapping-for-activity-logs"></a>Mapping della tabella per i log attività

Per eseguire il mapping dei dati dei log attività con la tabella, usare la query seguente:

```kusto
.create table ActivityLogsRawRecords ingestion json mapping 'ActivityLogsRawRecordsMapping' '[{"column":"Records","path":"$.records"}]'
```

### <a name="create-the-update-policy-for-log-data"></a>Creare il criterio di aggiornamento per i dati dei log

#### <a name="activity-log-data-update-policy"></a>Criterio di aggiornamento dei dati del log attività

1. Creare una [funzione](/azure/kusto/management/functions) che espande la raccolta di record del log attività in modo che ogni valore della raccolta riceva una riga distinta. Usare l'operatore [`mvexpand`](/azure/kusto/query/mvexpandoperator):

    ```kusto
    .create function ActivityLogRecordsExpand() {
        ActivityLogsRawRecords
        | mvexpand events = Records
        | project
            Timestamp = todatetime(events["time"]),
            ResourceId = tostring(events["resourceId"]),
            OperationName = tostring(events["operationName"]),
            Category = tostring(events["category"]),
            ResultType = tostring(events["resultType"]),
            ResultSignature = tostring(events["resultSignature"]),
            DurationMs = toint(events["durationMs"]),
            IdentityAuthorization = events.identity.authorization,
            IdentityClaims = events.identity.claims,
            Location = tostring(events["location"]),
            Level = tostring(events["level"])
    }
    ```

2. Aggiungere il [criterio di aggiornamento](/azure/kusto/concepts/updatepolicy) nella tabella di destinazione. Questo criterio eseguirà automaticamente la query sui nuovi dati inseriti nella tabella dati intermedia *ActivityLogsRawRecords* e inserirà i relativi risultati nella tabella *ActivityLogsRecords*:

    ```kusto
    .alter table ActivityLogsRecords policy update @'[{"Source": "ActivityLogsRawRecords", "Query": "ActivityLogRecordsExpand()", "IsEnabled": "True"}]'
    ```

#### <a name="diagnostic-log-data-update-policy"></a>Criterio di aggiornamento dei dati del log di diagnostica

1. Creare una [funzione](/azure/kusto/management/functions) che espande la raccolta di record del log di diagnostica in modo che ogni valore della raccolta riceva una riga distinta. Usare l'operatore [`mvexpand`](/azure/kusto/query/mvexpandoperator):
     ```kusto
    .create function DiagnosticLogRecordsExpand() {
        DiagnosticLogsRawRecords
        | mvexpand events = Records
        | project
            Timestamp = todatetime(events["time"]),
            ResourceId = tostring(events["resourceId"]),
            MetricName = tostring(events["metricName"]),
            Count = toint(events["count"]),
            Total = todouble(events["total"]),
            Minimum = todouble(events["minimum"]),
            Maximum = todouble(events["maximum"]),
            Average = todouble(events["average"]),
            TimeGrain = tostring(events["timeGrain"])
    }
    ```

2. Aggiungere il [criterio di aggiornamento](/azure/kusto/concepts/updatepolicy) nella tabella di destinazione. Questo criterio eseguirà automaticamente la query su tutti i nuovi dati inseriti nella tabella dati intermedia *DiagnosticLogsRawRecords* e inserirà i relativi risultati nella tabella *DiagnosticLogsRecords*:

    ```kusto
    .alter table DiagnosticLogsRecords policy update @'[{"Source": "DiagnosticLogsRawRecords", "Query": "DiagnosticLogRecordsExpand()", "IsEnabled": "True"}]'
    ```

## <a name="create-an-azure-event-hubs-namespace"></a>Creare uno spazio dei nomi di Hub eventi di Azure

I log di diagnostica di Azure consentono l'esportazione delle metriche in un account di archiviazione o in un hub eventi. In questa esercitazione le metriche verranno instradate tramite un hub eventi. Con i passaggi seguenti verrà creato uno spazio dei nomi di hub eventi e un hub eventi per i log di diagnostica. Monitoraggio di Azure creerà il componente *insights-operational-logs* dell'hub eventi per i log attività.

1. Creare un hub eventi usando un modello di Azure Resource Manager nel portale di Azure. Per seguire il resto dei passaggi di questo articolo, fare clic con il pulsante destro del mouse sul pulsante **Distribuisci in Azure** e quindi selezionare **Apri in una nuova finestra**. Il pulsante **Distribuzione in Azure** consente di passare al portale di Azure.

    [![Pulsante Distribuisci in Azure](media/ingest-data-no-code/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

1. Creare uno spazio dei nomi di hub eventi e un hub eventi per i log di diagnostica.

    ![Creazione dell'hub eventi](media/ingest-data-no-code/event-hub.png)

1. Compilare il modulo con le informazioni seguenti. Per tutte le impostazioni non elencate nella tabella seguente usare i valori predefiniti.

    **Impostazione** | **Valore consigliato** | **Descrizione**
    |---|---|---|
    | **Sottoscrizione** | *Sottoscrizione in uso* | Selezionare la sottoscrizione di Azure da usare per l'hub eventi.|
    | **Gruppo di risorse** | *test-resource-group* | Creare un nuovo gruppo di risorse. |
    | **Posizione** | Selezionare l'area più appropriata in base alle esigenze. | Creare lo spazio dei nomi di hub eventi nella stessa posizione delle altre risorse.
    | **Nome spazio dei nomi** | *AzureMonitoringData* | Scegliere un nome univoco per identificare lo spazio dei nomi.
    | **Nome hub eventi** | *DiagnosticLogsData* | L'hub eventi si trova nello spazio dei nomi, che fornisce un contenitore di ambito univoco. |
    | **Nome gruppo di consumer** | *adxpipeline* | Creare un nome del gruppo di consumer. I gruppi di consumer consentono a più applicazioni di avere ognuna una visualizzazione distinta del flusso di eventi. |
    | | |

## <a name="connect-azure-monitor-logs-to-your-event-hub"></a>Connettere i log di Monitoraggio di Azure all'hub eventi

A questo punto è necessario collegare i log di diagnostica e i log attività all'hub eventi.

### <a name="connect-diagnostic-logs-to-your-event-hub"></a>Collegare i log di diagnostica all'hub eventi

Selezionare una risorsa da cui esportare le metriche. Esistono diversi tipi di risorse che consentono l'esportazione dei log di diagnostica, tra cui lo spazio dei nomi di hub eventi, Azure Key Vault, hub IoT di Azure e i cluster di Esplora dati di Azure. In questa esercitazione verrà usato un cluster di Esplora dati di Azure come risorsa.

1. Selezionare il cluster Kusto nel portale di Azure.
1. Selezionare **Impostazioni di diagnostica** e quindi selezionare il collegamento **Abilita diagnostica**. 

    ![Impostazioni di diagnostica](media/ingest-data-no-code/diagnostic-settings.png)

1. Viene visualizzato il riquadro **Impostazioni di diagnostica**. Eseguire questa procedura:
   1. Assegnare ai dati del log di diagnostica il nome *ADXExportedData*.
   1. In **METRICA** selezionare la casella di controllo **AllMetrics** (facoltativo).
   1. Selezionare la casella di controllo **Streaming in un hub eventi**.
   1. Selezionare **Configura**.

      ![Riquadro Impostazioni di diagnostica](media/ingest-data-no-code/diagnostic-settings-window.png)

1. Nel riquadro **Selezionare l'hub eventi** configurare il modo in cui esportare i dati dai log di diagnostica nell'hub eventi creato:
    1. Nell'elenco **Selezionare lo spazio dei nomi dell'hub eventi** selezionare *AzureMonitoringData*.
    1. Nell'elenco **Selezionare il nome dell'hub eventi**  selezionare *diagnosticlogsdata*.
    1. Nell'elenco **Selezionare il nome dei criteri per l'hub eventi** selezionare **RootManagerSharedAccessKey**.
    1. Selezionare **OK**.

1. Selezionare **Salva**.

### <a name="connect-activity-logs-to-your-event-hub"></a>Collegare i log attività all'hub eventi

1. Scegliere **Log attività** dal menu a sinistra del portale di Azure.
1. Viene visualizzata la finestra **Log attività**. Selezionare **Esporta in Hub eventi**.

    ![Finestra Log attività](media/ingest-data-no-code/activity-log.png)

1. Viene visualizzata la finestra **Esporta log attività**:
 
    ![Finestra Esporta log attività](media/ingest-data-no-code/export-activity-log.png)

1. Nella finestra **Esporta log attività** seguire questa procedura:
      1. Selezionare la propria sottoscrizione.
      1. Nell'elenco **Aree** scegliere **Seleziona tutto**.
      1. Selezionare la casella di controllo **Esporta in un hub eventi**.
      1. Scegliere **Selezionare uno spazio dei nomi del bus di servizio** per aprire il riquadro **Selezionare l'hub eventi**.
      1. Nel riquadro **Selezionare l'hub eventi** selezionare la propria sottoscrizione.
      1. Nell'elenco **Selezionare lo spazio dei nomi dell'hub eventi** selezionare *AzureMonitoringData*.
      1. Nell'elenco **Selezionare il nome dei criteri per l'hub eventi** selezionare il nome dei criteri per l'hub eventi predefinito.
      1. Selezionare **OK**.
      1. Nell'angolo superiore sinistro della finestra selezionare **Salva**.
   Verrà creato un hub eventi denominato *insights-operational-logs*.

### <a name="see-data-flowing-to-your-event-hubs"></a>Esaminare il flusso di dati verso gli hub eventi

1. Attendere alcuni minuti finché non viene definita la connessione e non viene completata l'esportazione dei log attività nell'hub eventi. Passare allo spazio dei nomi di hub eventi per vedere gli hub eventi creati.

    ![Hub eventi creati](media/ingest-data-no-code/event-hubs-created.png)

1. Esaminare il flusso di dati verso l'hub eventi:

    ![Dati dell'hub eventi](media/ingest-data-no-code/event-hubs-data.png)

## <a name="connect-an-event-hub-to-azure-data-explorer"></a>Connettere un hub eventi a Esplora dati di Azure

A questo punto è necessario creare le connessioni dati per i log di diagnostica e i log attività.

### <a name="create-the-data-connection-for-diagnostic-logs"></a>Creare la connessione dati per i log di diagnostica

1. Nel cluster di Esplora dati di Azure denominato *kustodocs* scegliere **Database** dal menu a sinistra.
1. Nella finestra **Database** selezionare il database *TestDatabase*.
1. Scegliere **Inserimento dati** dal menu a sinistra.
1. Nella finestra **Inserimento dati** fare clic su **+ Aggiungi connessione dati**.
1. Nella finestra **Connessione dati** immettere le informazioni seguenti:

    ![Connessione dati dell'hub eventi](media/ingest-data-no-code/event-hub-data-connection.png)

    Origine dati:

    **Impostazione** | **Valore consigliato** | **Descrizione campo**
    |---|---|---|
    | **Nome connessione dati** | *DiagnosticsLogsConnection* | Nome della connessione da creare in Esplora dati di Azure.|
    | **Spazio dei nomi dell'hub eventi** | *AzureMonitoringData* | Nome scelto in precedenza che identifica lo spazio dei nomi. |
    | **Hub eventi** | *diagnosticlogsdata* | Hub eventi creato. |
    | **Gruppo di consumer** | *adxpipeline* | Gruppo di consumer definito nell'hub eventi creato. |
    | | |

    Tabella di destinazione:

    Sono disponibili due opzioni per il routing: *statico* e *dinamico*. Per questa esercitazione viene usato il routing statico (impostazione predefinita), in cui vengono specificati il nome della tabella, il formato dati e il mapping. Lasciare deselezionato **My data includes routing info** (I miei dati includono le informazioni di routing).

     **Impostazione** | **Valore consigliato** | **Descrizione campo**
    |---|---|---|
    | **Tabella** | *DiagnosticLogsRawRecords* | Tabella creata nel database *TestDatabase*. |
    | **Formato dati** | *JSON* | Formato usato nella tabella. |
    | **Mapping di colonne** | *DiagnosticLogsRecordsMapping* | Il mapping creato nel database *TestDatabase*, che mappa i dati JSON in ingresso con i nomi di colonna e i tipi di dati della tabella *DiagnosticLogsRecords*.|
    | | |

1. Selezionare **Create**.  

### <a name="create-the-data-connection-for-activity-logs"></a>Creare la connessione dati per i log attività

Ripetere i passaggi descritti nella sezione Creare la connessione dati per i log di diagnostica per creare una connessione dati per i log attività.

1. Nella finestra **Connessione dati** usare le impostazioni seguenti:

    Origine dati:

    **Impostazione** | **Valore consigliato** | **Descrizione campo**
    |---|---|---|
    | **Nome connessione dati** | *ActivityLogsConnection* | Nome della connessione da creare in Esplora dati di Azure.|
    | **Spazio dei nomi dell'hub eventi** | *AzureMonitoringData* | Nome scelto in precedenza che identifica lo spazio dei nomi. |
    | **Hub eventi** | *insights-operational-logs* | Hub eventi creato. |
    | **Gruppo di consumer** | *$Default* | Il gruppo di consumer predefinito. Se necessario, è possibile crearne uno diverso. |
    | | |

    Tabella di destinazione:

    Sono disponibili due opzioni per il routing: *statico* e *dinamico*. Per questa esercitazione viene usato il routing statico (impostazione predefinita), in cui vengono specificati il nome della tabella, il formato dati e il mapping. Lasciare deselezionato **My data includes routing info** (I miei dati includono le informazioni di routing).

     **Impostazione** | **Valore consigliato** | **Descrizione campo**
    |---|---|---|
    | **Tabella** | *ActivityLogsRawRecords* | Tabella creata nel database *TestDatabase*. |
    | **Formato dati** | *JSON* | Formato usato nella tabella. |
    | **Mapping di colonne** | *ActivityLogsRawRecordsMapping* | Il mapping creato nel database *TestDatabase*, che mappa i dati JSON in ingresso con i nomi di colonna e i tipi di dati della tabella *ActivityLogsRawRecords*.|
    | | |

1. Selezionare **Create**.  

## <a name="query-the-new-tables"></a>Eseguire una query sulle nuove tabelle

È stata configurata una pipeline con un flusso di dati. L'inserimento tramite il cluster richiede 5 minuti per impostazione predefinita, quindi aspettare alcuni minuti durante il flusso di dati prima di avviare la query.

### <a name="an-example-of-querying-the-diagnostic-logs-table"></a>Esempio di query sulla tabella dei log di diagnostica

La query seguente analizza i dati sulla durata della query dai record dei log di diagnostica di Esplora dati di Azure:

```kusto
DiagnosticLogsRecords
| where Timestamp > ago(15m) and MetricName == 'QueryDuration'
| summarize avg(Average)
```

Risultati della query:

|   |   |
| --- | --- |
|   |  avg_Average |
|   | 00:06,156 |
| | |

### <a name="an-example-of-querying-the-activity-logs-table"></a>Esempio di query sulla tabella dei log attività

La query seguente analizza i dati dai record dei log attività di Esplora dati di Azure:

```kusto
ActivityLogsRecords
| where OperationName == 'MICROSOFT.EVENTHUB/NAMESPACES/AUTHORIZATIONRULES/LISTKEYS/ACTION'
| where ResultType == 'Success'
| summarize avg(DurationMs)
```

Risultati della query:

|   |   |
| --- | --- |
|   |  avg(DurationMs) |
|   | 768,333 |
| | |

## <a name="next-steps"></a>Passaggi successivi

Leggere l'articolo seguente per informazioni su come scrivere molte più query sui dati estratti da Esplora dati di Azure:

> [!div class="nextstepaction"]
> [Scrivere query per Esplora dati di Azure](write-queries.md)

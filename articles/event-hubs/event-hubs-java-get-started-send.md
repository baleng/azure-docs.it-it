---
title: Inviare eventi tramite Java - Hub eventi di Azure | Microsoft Docs
description: Questo articolo fornisce una procedura dettagliata per la creazione di un'applicazione Java in grado di inviare eventi ad Hub eventi di Azure.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 4da89e0f99832c429091e0a028fafd8943df811a
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55300116"
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Inviare eventi a Hub eventi di Azure usando Java

Hub eventi di Azure è una piattaforma di Big Data streaming e un servizio di inserimento di eventi che consente di ricevere ed elaborare milioni di eventi al secondo. Hub eventi consente di elaborare e archiviare eventi, dati o dati di telemetria generati dal software distribuito e dai dispositivi. I dati inviati a un hub eventi possono essere trasformati e archiviati usando qualsiasi provider di analisi in tempo reale o adattatori di invio in batch/archiviazione. Per una panoramica dettagliata di Hub eventi, vedere [Panoramica di Hub eventi](event-hubs-about.md) e [Funzionalità di Hub eventi](event-hubs-features.md).

Questa esercitazione illustra come inviare eventi a un hub eventi usando un'applicazione console scritta in Java. 

> [!NOTE]
> È possibile scaricare questa guida introduttiva come esempio da [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend), sostituire le stringhe `EventHubConnectionString` e `EventHubName` con i valori dell'hub eventi in uso ed eseguirla. In alternativa, è possibile seguire la procedura illustrata in questa esercitazione per creare una soluzione propria.

## <a name="prerequisites"></a>Prerequisiti

Per completare questa esercitazione è necessario soddisfare i prerequisiti seguenti:

* Ambiente di sviluppo in Java. Questa esercitazione usa [Eclipse](https://www.eclipse.org/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Creare uno spazio dei nomi di Hub eventi e un hub eventi
Il primo passaggio consiste nell'usare il [portale di Azure](https://portal.azure.com) per creare uno spazio dei nomi di tipo Hub eventi e ottenere le credenziali di gestione necessarie all'applicazione per comunicare con l'hub eventi. Per creare uno spazio dei nomi e un hub eventi, seguire la procedura descritta in [questo articolo](event-hubs-create.md).

Ottenere il valore della chiave di accesso dell'hub eventi seguendo le istruzioni disponibili nell'articolo: [Ottenere una stringa di connessione](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Si userà la chiave di accesso nel codice scritto più avanti in questa esercitazione. Il nome della chiave predefinita è: **RootManageSharedAccessKey**.

Continuare ora con i passaggi seguenti dell'esercitazione.

## <a name="add-reference-to-azure-event-hubs-library"></a>Aggiungere il riferimento alla libreria di Hub eventi di Azure

La libreria client Java per Hub eventi è disponibile per l'uso in progetti Maven dal [repository centrale di Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Per fare riferimento a questa libreria è possibile usare questa dichiarazione di dipendenza all'interno del file di progetto di Maven:

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>2.2.0</version>
</dependency>
```

Per diversi tipi di ambienti di compilazione è possibile ottenere in modo esplicito i file JAR rilasciati più recenti dal [repository centrale di Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Per un autore di eventi semplice, importare il pacchetto *com.microsoft.azure.eventhubs* per le classi di client di hub eventi e il pacchetto *com.microsoft.azure.servicebus* per le classi di utilità, ad esempio eccezioni comuni condivise con il client di messaggistica del bus di servizio di Azure. 

## <a name="write-code-to-send-messages-to-the-event-hub"></a>Scrivere codice per inviare messaggi all'hub eventi

Per l'esempio seguente, creare prima un nuovo progetto Maven per un'applicazione console/shell nell'ambiente di sviluppo Java preferito. Aggiungere una classe denominata `SimpleSend` e includere il codice seguente nella classe:

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
import com.microsoft.azure.eventhubs.EventData;
import com.microsoft.azure.eventhubs.EventHubClient;
import com.microsoft.azure.eventhubs.EventHubException;

import java.io.IOException;
import java.nio.charset.Charset;
import java.time.Instant;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;

public class SimpleSend {

    public static void main(String[] args)
            throws EventHubException, ExecutionException, InterruptedException, IOException {
            
            
    }
 }
```

### <a name="construct-connection-string"></a>Costruire la stringa di connessione

Usare la classe ConnectionStringBuilder per costruire un valore di stringa di connessione da passare all'istanza del client di Hub eventi. Sostituire i segnaposto con i valori ottenuti durante la creazione dello spazio dei nomi e dell'hub eventi:

```java
        final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
                .setNamespaceName("speventhubns") 
                .setEventHubName("speventhub")
                .setSasKeyName("RootManageSharedAccessKey")
                .setSasKey("2+WMsyyy1XmUtEnRsfOmTTyGasfJgsVjGAOIN20J1Y8=");
```

### <a name="send-events"></a>Inviare eventi

Creare un evento singolare trasformando una stringa nella relativa codifica UTF-8. Creare quindi una nuova istanza del client di Hub eventi dalla stringa di connessione e inviare il messaggio:   

```java 
        final Gson gson = new GsonBuilder().create();

        // The Executor handles all asynchronous tasks and this is passed to the EventHubClient instance.
        // This enables the user to segregate their thread pool based on the work load.
        // This pool can then be shared across multiple EventHubClient instances.
        // The following sample uses a single thread executor, as there is only one EventHubClient instance,
        // handling different flavors of ingestion to Event Hubs here.
        final ScheduledExecutorService executorService = Executors.newScheduledThreadPool(4);

        // Each EventHubClient instance spins up a new TCP/SSL connection, which is expensive.
        // It is always a best practice to reuse these instances. The following sample shows this.
        final EventHubClient ehClient = EventHubClient.createSync(connStr.toString(), executorService);


        try {
            for (int i = 0; i < 10; i++) {

                String payload = "Message " + Integer.toString(i);
                byte[] payloadBytes = gson.toJson(payload).getBytes(Charset.defaultCharset());
                EventData sendEvent = EventData.create(payloadBytes);

                // Send - not tied to any partition
                // Event Hubs service will round-robin the events across all Event Hubs partitions.
                // This is the recommended & most reliable way to send to Event Hubs.
                ehClient.sendSync(sendEvent);
            }

            System.out.println(Instant.now() + ": Send Complete...");
            System.out.println("Press Enter to stop.");
            System.in.read();
        } finally {
            ehClient.closeSync();
            executorService.shutdown();
        }

``` 

Compilare ed eseguire il programma e verificare che non siano presenti errori.

Congratulazioni! Sono stati inviati messaggi a un hub eventi.

### <a name="appendix-how-messages-are-routed-to-eventhub-partitions"></a>Appendice: Modalità di indirizzamento dei messaggi alle partizioni di Hub eventi

Prima che i messaggi vengano recuperati da parte dei consumatori, dovranno essere pubblicati nelle partizioni dagli editori. Quando vengono pubblicati i messaggi nell'Hub eventi in modo sincrono tramite il metodo sendSync() nell'oggetto com.microsoft.azure.eventhubs.EventHubClient, i messaggi possono essere inviati a una partizione specifica o distribuiti a tutte le partizioni disponibili in modalità round robin a seconda che la chiave di partizione sia specificata o meno.

Quando viene specificata una stringa che rappresenta la chiave di partizione, la chiave viene sottoposta a hashing per determinare a quale partizione inviare l'evento.

Quando la chiave di partizione non è impostata, i messaggi verranno distribuiti a round robin a tutte le partizioni disponibili

```java
// Serialize the event into bytes
byte[] payloadBytes = gson.toJson(messagePayload).getBytes(Charset.defaultCharset());

// Use the bytes to construct an {@link EventData} object
EventData sendEvent = EventData.create(payloadBytes);

// Transmits the event to event hub without a partition key
// If a partition key is not set, then we will round-robin to all topic partitions
eventHubClient.sendSync(sendEvent);

//  the partitionKey will be hash'ed to determine the partitionId to send the eventData to.
eventHubClient.sendSync(sendEvent, partitionKey);

// close the client at the end of your program
eventHubClient.closeSync();

```

## <a name="next-steps"></a>Passaggi successivi

In questa guida introduttiva si sono inviati messaggi a un hub eventi usando Java. Per informazioni su come ricevere eventi da un hub eventi usando Java, vedere [Ricevere eventi da Hub eventi usando Java](event-hubs-java-get-started-receive-eph.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio


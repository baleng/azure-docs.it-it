---
title: Inviare eventi tramite Python - Hub eventi di Azure | Microsoft Docs
description: Questo articolo fornisce una procedura dettagliata per la creazione di un'applicazione Python che invia eventi all'hub eventi di Azure.
services: event-hubs
author: ShubhaVijayasarathy
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 11/16/2018
ms.author: shvija
ms.openlocfilehash: 2168fc89134615ffb4e0e718cc0cc27b8c1a7839
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59262772"
---
# <a name="send-events-to-event-hubs-using-python"></a>Inviare eventi ad Hub eventi tramite Python

Hub eventi di Azure è una piattaforma di Big Data streaming e un servizio di inserimento di eventi che consente di ricevere ed elaborare milioni di eventi al secondo. Hub eventi consente di elaborare e archiviare eventi, dati o dati di telemetria generati dal software distribuito e dai dispositivi. I dati inviati a un hub eventi possono essere trasformati e archiviati usando qualsiasi provider di analisi in tempo reale o adattatori di invio in batch/archiviazione. Per una panoramica dettagliata di Hub eventi, vedere [Panoramica di Hub eventi](event-hubs-about.md) e [Funzionalità di Hub eventi](event-hubs-features.md).

Questa esercitazione descrive come inviare eventi a un hub eventi da un'applicazione scritta in Python. 

> [!NOTE]
> È possibile scaricare questa guida introduttiva come esempio da [GitHub](https://github.com/Azure/azure-event-hubs-python/tree/master/examples), sostituire le stringhe `EventHubConnectionString` e `EventHubName` con i valori dell'hub eventi in uso ed eseguirla. In alternativa, è possibile seguire la procedura illustrata in questa esercitazione per creare una soluzione propria.

## <a name="prerequisites"></a>Prerequisiti

Per completare questa esercitazione è necessario soddisfare i prerequisiti seguenti:

- Una sottoscrizione di Azure. Se non se ne ha una, [creare un account gratuito](https://azure.microsoft.com/free/) prima di iniziare.
- Python 3.4 o versione successiva.


## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Creare uno spazio dei nomi di Hub eventi e un hub eventi
Il primo passaggio consiste nell'usare il [portale di Azure](https://portal.azure.com) per creare uno spazio dei nomi di tipo Hub eventi e ottenere le credenziali di gestione necessarie all'applicazione per comunicare con l'hub eventi. Per creare uno spazio dei nomi e un hub eventi, seguire la procedura descritta in [questo articolo](event-hubs-create.md).

Ottenere il valore della chiave di accesso dell'hub eventi seguendo le istruzioni disponibili nell'articolo: [Ottenere una stringa di connessione](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Si userà la chiave di accesso nel codice scritto più avanti in questa esercitazione. Il nome della chiave predefinita è: **RootManageSharedAccessKey**.

Continuare ora con i passaggi seguenti dell'esercitazione.

## <a name="install-python-package"></a>Installa pacchetto Python

Per installare i pacchetti di Python per Hub eventi aprire un prompt dei comandi con un percorso contenente Python, quindi eseguire questo comando: 

```bash
pip install azure-eventhub
```

## <a name="create-a-python-script-to-send-events"></a>Creazione di uno script Python per inviare gli eventi

Successivamente, creare un'applicazione Python che invia eventi a un hub eventi:

1. Aprire l'editor preferito di Python, ad esempio[Visual Studio Code][Visual Studio Code].
2. Creare uno script denominato **send.py**. Questo script invia 100 eventi all'hub eventi.
3. Incollare il codice seguente in send.py, sostituendo i valori INDIRIZZO, UTENTE e CHIAVE con i valori ottenuti dal portale di Azure nella sezione precedente: 

```python
import sys
import logging
import datetime
import time
import os

from azure.eventhub import EventHubClient, Sender, EventData

logger = logging.getLogger("azure")

# Address can be in either of these formats:
# "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
# "amqps://<mynamespace>.servicebus.windows.net/myeventhub"
# For example:
ADDRESS = "amqps://mynamespace.servicebus.windows.net/myeventhub"

# SAS policy and key are not required if they are encoded in the URL
USER = "RootManageSharedAccessKey"
KEY = "namespaceSASKey"

try:
    if not ADDRESS:
        raise ValueError("No EventHubs URL supplied.")

    # Create Event Hubs client
    client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
    sender = client.add_sender(partition="0")
    client.run()
    try:
        start_time = time.time()
        for i in range(100):
            print("Sending message: {}".format(i))
            sender.send(EventData(str(i)))
    except:
        raise
    finally:
        end_time = time.time()
        client.stop()
        run_time = end_time - start_time
        logger.info("Runtime: {} seconds".format(run_time))

except KeyboardInterrupt:
    pass
```

## <a name="run-application-to-send-events"></a>Eseguire l'applicazione per inviare gli eventi

Per eseguire lo script aprire un prompt dei comandi con un percorso contenente Python, quindi eseguire questo comando:

```bash
start python send.py
```

Congratulazioni! Sono stati inviati messaggi a un hub eventi.
 
## <a name="next-steps"></a>Passaggi successivi
In questa guida introduttiva si sono inviati messaggi a un hub eventi usando Python. Per informazioni su come ricevere eventi da un hub eventi usando Python, vedere [Ricevere eventi da Hub eventi tramite Python](event-hubs-python-get-started-receive.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[Visual Studio Code]: https://code.visualstudio.com/
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

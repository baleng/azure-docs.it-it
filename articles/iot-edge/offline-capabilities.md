---
title: Usare i dispositivi offline - Azure IoT Edge | Microsoft Docs
description: Comprendere come i moduli e dispositivi IoT Edge possano funzionare senza connessione Internet per periodi prolungati di tempo e come IoT Edge possa consentire anche a dispositivi IoT regolari di funzionare offline.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/30/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: e82c842ec8fce703c48c98eaf09ea5c8d91be9be
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2019
ms.locfileid: "57307990"
---
# <a name="understand-extended-offline-capabilities-for-iot-edge-devices-modules-and-child-devices-preview"></a>Informazioni sulle funzionalità per periodi offline prolungati per i dispositivi IoT Edge, i moduli e i dispositivi figlio (anteprima)

Azure IoT Edge supporta operazioni offline per periodi prolungati nei dispositivi IoT Edge e consente operazioni offline anche nei dispositivi figlio non Edge. Se un dispositivo IoT Edge ha avuto la possibilità di connettersi una volta all'hub IoT, tale dispositivo e tutti i dispositivi figlio possono continuare a funzionare con una connessione Internet intermittente o senza connessione. 

>[!NOTE]
>Il supporto della modalità offline per IoT Edge è disponibile in [versione di anteprima pubblica](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="how-it-works"></a>Funzionamento

Quando un dispositivo IoT Edge passa alla modalità offline, l'hub di IoT Edge svolge tre ruoli. Prima di tutto, archivia i messaggi diretti upstream e li salva fino a quando il dispositivo si riconnette. In secondo luogo, agisce per conto dell'hub IoT per l'autenticazione dei moduli e dei dispositivi figlio in modo che possano continuare a funzionare. In terzo luogo, consente le comunicazioni tra i dispositivi figlio che normalmente passerebbero attraverso l'hub IoT. 

L'esempio seguente mostra il funzionamento di uno scenario IoT Edge in modalità offline:

1. **Configurare un dispositivo IoT Edge.**

   Per i dispositivi IoT Edge le funzionalità offline vengono abilitate automaticamente. Per estendere tale funzionalità ad altri dispositivi IoT, è necessario dichiarare una relazione padre-figlio tra i dispositivi nell'hub IoT. 

2. **Eseguire la sincronizzazione con l'hub IoT.**

   Almeno una volta dopo l'installazione del runtime di IoT Edge, il dispositivo IoT Edge deve essere online per la sincronizzazione con l'hub IoT. Con questa sincronizzazione, il dispositivo IoT Edge ottiene informazioni dettagliate sugli eventuali dispositivi figlio assegnati. Il dispositivo IoT Edge aggiorna inoltre in modo sicuro la cache locale per abilitare le operazioni offline e recupera le impostazioni per l'archiviazione locale dei messaggi di telemetria. 

3. **Disconnettersi.**

   Mentre si è disconnessi dall'hub IoT, il dispositivo IoT Edge, i moduli distribuiti e tutti i dispositivi IoT figlio possono operare a tempo indeterminato. I moduli e i dispositivi figlio possono essere avviati e riavviati eseguendo l'autenticazione con l'hub di IoT Edge mentre è attiva la modalità offline. I dati di telemetria previsti per l'invio upstream all'hub IoT vengono archiviati in locale. La comunicazione tra i moduli o tra i dispositivi IoT figlio viene gestita tramite metodi diretti o messaggi. 

4. **Ristabilire la connessione e ripetere la sincronizzazione con l'hub IoT.**

   Dopo il ripristino della connessione con l'hub IoT, il dispositivo IoT Edge viene sincronizzato di nuovo. I messaggi archiviati in locale vengono recapitati nello stesso ordine in cui sono stati archiviati. Eventuali differenze tra le proprietà desiderate e quelle segnalate per i moduli e i dispositivi vengono riconciliate. Il dispositivo IoT Edge aggiorna tutte le modifiche nel set di dispositivi IoT figlio assegnati.

## <a name="restrictions-and-limits"></a>Restrizioni e limiti

Le funzionalità offline per periodi prolungati descritte in questo articolo sono disponibili in [IoT Edge versione 1.0.4 o versioni successive](https://github.com/Azure/azure-iotedge/releases). Nelle versioni precedenti è disponibile un subset di funzionalità offline. I dispositivi IoT Edge esistenti che non hanno le funzionalità offline per periodi prolungati non possono essere aggiornati modificando la versione del runtime, ma devono essere riconfigurati con una nuova identità del dispositivo IoT Edge per ottenere queste funzionalità. 

Il supporto delle funzionalità offline per periodi prolungati è disponibile in tutte le aree in cui è disponibile l'hub IoT, **ad eccezione** di Stati Uniti orientali.

Solo i dispositivi non IoT Edge possono essere aggiunti come dispositivi figlio. 

I dispositivi IoT Edge e i dispositivi figlio assegnati possono funzionare offline per un periodo illimitato dopo la sincronizzazione iniziale, una tantum. L'archiviazione dei messaggi dipende tuttavia dall'impostazione della durata (TTL) e dallo spazio disponibile su disco per archiviare i messaggi. 

## <a name="set-up-an-iot-edge-device"></a>Configurare un dispositivo IoT Edge

Per estendere le funzionalità offline per periodi prolungati ai dispositivi IoT figlio di un dispositivo IoT Edge, è necessario dichiarare le relazioni padre-figlio nel portale di Azure.

### <a name="assign-child-devices"></a>Assegnare i dispositivi figlio

I dispositivi figlio possono essere qualsiasi dispositivo non Edge registrato nello stesso hub IoT. È possibile gestire la relazione padre-figlio al momento della creazione di un nuovo dispositivo o dalla pagina dei dettagli del dispositivo del dispositivo padre IoT Edge o del dispositivo IoT figlio. 

   ![Gestire i dispositivi figlio dalla pagina dei dettagli del dispositivo IoT Edge](./media/offline-capabilities/manage-child-devices.png)

I dispositivi padre possono avere più dispositivi figlio, ma un dispositivo figlio può avere un solo padre.

### <a name="specifying-dns-servers"></a>Specificare i server DNS 

Per migliorare l'affidabilità, si consiglia di specificare gli indirizzi server DNS usati nell'ambiente. In Linux, ad esempio, aggiornare **/etc/docker/daemon.json** (è possibile che sia necessario creare il file) in modo da includere:

```json
{
    "dns": ["1.1.1.1"]
}
```

Se si usa un server DNS locale, sostituire 1.1.1.1 con l'indirizzo IP del server DNS locale. Riavviare il servizio Docker per rendere effettive le modifiche.


## <a name="optional-offline-settings"></a>Impostazioni facoltative per la modalità offline

Se si prevede di raccogliere tutti i messaggi generati dai dispositivi durante lunghi periodi di tempo offline, configurare l'hub di IoT Edge in modo che possa archiviare tutti i messaggi. Sono due le modifiche che è possibile apportare all'hub di IoT Edge per abilitare l'archiviazione a lungo termine dei messaggi. Prima di tutto aumentare l'impostazione della durata (TTL) e quindi aggiungere altro spazio su disco per l'archiviazione dei messaggi. 

### <a name="time-to-live"></a>Durata (TTL)

L'impostazione della durata (TTL) è la quantità di tempo (in secondi) di attesa per il recapito di un messaggio prima che scada. Il valore predefinito è 7200 secondi (due ore). 

Questa impostazione è una proprietà desiderata dell'hub di IoT Edge, che viene archiviata nel modulo gemello. È possibile configurarla nel portale di Azure, nella sezione **Configura impostazioni avanzate per il runtime di IoT Edge** oppure direttamente nel manifesto della distribuzione. 

```json
"$edgeHub": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {},
        "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
        }
    }
}
```

### <a name="additional-offline-storage"></a>Spazio di archiviazione aggiuntivo per la modalità offline

Per impostazione predefinita, i messaggi vengono archiviati nel file system del contenitore dell'hub di IoT Edge. Se tale quantità di spazio di archiviazione non è sufficiente per le esigenze per la modalità offline, è possibile dedicare spazio di archiviazione locale nel dispositivo IoT Edge. Per l'hub di IoT Edge creare una variabile di ambiente che punti a una cartella di archiviazione nel contenitore. Usare quindi le opzioni di creazione per associare tale cartella di archiviazione a una cartella nel computer host. 

È possibile configurare le variabili di ambiente e le opzioni di creazione per il modulo dell'hub di IoT Edge nel portale di Azure, nella sezione **Configura impostazioni avanzate per il runtime di IoT Edge**, oppure direttamente nel manifesto della distribuzione. 

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": {
            "HostConfig": {
                "Binds": ["<HostStoragePath>:<ModuleStoragePath>"],
                "PortBindings": {
                    "8883/tcp": [{"HostPort":"8883"}],
                    "443/tcp": [{"HostPort":"443"}],
                    "5671/tcp": [{"HostPort":"5671"}]
                }
            }
        }
    },
    "env": {
        "storageFolder": {
            "value": "<ModuleStoragePath>"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

Sostituire `<HostStoragePath>` e `<ModuleStoragePath>` con i percorsi di archiviazione dell'host e del modulo. Entrambi devono essere percorsi assoluti. Nelle opzioni di creazione associare tra loro i percorsi di archiviazione dell'host e del modulo e quindi creare una variabile di ambiente che punti al percorso di archiviazione del modulo.  

Ad esempio, `"Binds":["/etc/iotedge/storage/:/iotedge/storage/"]` indica che la directory **/etc/iotedge/storage** nel sistema host è mappata alla directory **/iotedge/storage/** nel contenitore. Un altro esempio per i sistemi Windows è `"Binds":["C:\\temp:C:\\contemp"]` e indica che la directory **C:\\temp** nel sistema host è mappata alla directory **C:\\contemp** nel contenitore. 

Per informazioni più dettagliate sulle opzioni di creazione, vedere i [documenti su Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate).

## <a name="next-steps"></a>Passaggi successivi

Abilitare le operazioni offline per periodi prolungati negli scenari di [gateway trasparente](how-to-create-transparent-gateway.md).

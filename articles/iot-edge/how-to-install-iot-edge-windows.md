---
title: Installare Azure IoT Edge in Windows | Microsoft Docs
description: Istruzioni per l'installazione di Azure IoT Edge in Windows 10, Windows Server e Windows IoT Core
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 95e984f6f08af01a2ffd7b9b4e0ec598d73f4d05
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/29/2019
ms.locfileid: "58621074"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows"></a>Installare il runtime di Azure IoT Edge in Windows

Il runtime di Azure IoT Edge è ciò che trasforma un dispositivo in un dispositivo IoT Edge. Il runtime può essere distribuito nei dispositivi di dimensioni pari a un server industriale o a un dispositivo Raspberry Pi. Dopo aver configurato un dispositivo con il runtime di IoT Edge, è possibile avviare la distribuzione della logica di business dal cloud. 

Per altre informazioni sul funzionamento del runtime di IoT Edge, vedere [Informazioni sul runtime di Azure IoT Edge e sulla relativa architettura](iot-edge-runtime.md).

Questo articolo descrive i passaggi da eseguire per installare il runtime di Azure IoT Edge in un sistema Windows x64 (Intel o AMD). Il supporto per Windows è attualmente disponibile in versione di anteprima.

> [!NOTE]
> Un problema noto relativo sistema operativo Windows impedisce la transizione per sospensione e ibernazione gli stati di alimentazione, quando si eseguono i moduli di IoT Edge (contenitori di Windows Nano Server con isolamento di processo). Questo problema influisce sulla durata della batteria del dispositivo.
>
> In alternativa, usare il comando `Stop-Service iotedge` arrestare tutti i moduli IoT Edge in esecuzione prima di usare questi stati di alimentazione. 

<!--
> [!NOTE]
> Using Linux containers on Windows systems is not a recommended or supported production configuration for Azure IoT Edge. However, it can be used for development and testing purposes.
-->

Usando Linux contenitore nei sistemi Windows non è una configurazione di produzione consigliato o supportato per Azure IoT Edge. È tuttavia possibile usarla a scopo di sviluppo e test. 

## <a name="prerequisites"></a>Prerequisiti

Usare questa sezione per verificare se il dispositivo Windows in uso può supportare IoT Edge e per prepararlo per un motore del contenitore prima dell'installazione. 

### <a name="supported-windows-versions"></a>Versioni supportate di Windows

Azure IoT Edge supporta diverse versioni di Windows, a seconda che si eseguano contenitori Windows o contenitori Linux. 

La versione più recente di Azure IoT Edge con contenitori Windows può essere eseguita con le versioni di Windows seguenti:
* Windows 10 o IoT Core con aggiornamento dell'ottobre 2018 (build 17763)
* Windows Server 2019 (build 17763)

La versione più recente di Azure IoT Edge con contenitori Linux può essere eseguita con le versioni di Windows seguenti: 
* Aggiornamento dell'anniversario di Windows 10 (build 14393) o versione successiva
* Windows Server 2016 o versioni successive

Per altre informazioni sui sistemi operativi attualmente supportati, vedere [Sistemi supportati da Azure IoT Edge](support.md#operating-systems). 

Per altre informazioni sui componenti inclusi nella versione più recente di IoT Edge, fare riferimento alle [versioni di Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

### <a name="prepare-for-a-container-engine"></a>Preparare il dispositivo per un motore del contenitore 

Azure IoT Edge si basa su un motore del contenitore [compatibile con OCI](https://www.opencontainers.org/). Per gli scenari di produzione, usare il motore Moby incluso nello script di installazione per eseguire contenitori Windows nel dispositivo Windows in uso. Per lo sviluppo e il test, è possibile eseguire contenitori Linux nel dispositivo Windows, ma è necessario installare e configurare un motore del contenitore prima di installare IoT Edge. Per entrambi gli scenari, vedere le sezioni seguenti per i prerequisiti per la preparazione del dispositivo. 

Se si desidera installare IoT Edge in una macchina virtuale, abilitare la virtualizzazione annidata e allocare almeno 2 GB di memoria. Come si abilita la virtualizzazione nidificata è diversa a seconda dell'hypervisor all'utilizzo. Per Hyper-V, macchine virtuali di generazione 2 sono annidati virtualizzazione abilitata per impostazione predefinita. Per VMWare, è disponibile un elemento toggle per abilitare la funzionalità nella macchina virtuale. 

#### <a name="moby-engine-for-windows-containers"></a>Motore Moby per contenitori Windows

Per i dispositivi Windows che eseguono IoT Edge negli scenari di produzione, Moby è l'unico motore del contenitore supportato ufficialmente. Lo script di installazione installa automaticamente il motore Moby nel dispositivo prima di installare IoT Edge. Preparare il dispositivo attivando la funzionalità Contenitori. 

1. Nella barra di avvio cercare **Attivazione e disattivazione delle funzionalità Windows** e aprire il programma Pannello di controllo.
2. Trovare e selezionare **Contenitori**.
3. Selezionare **OK**. 

#### <a name="docker-for-linux-containers"></a>Docker per contenitori Linux

Se si usa Windows per sviluppare e testare i contenitori per i dispositivi Linux, è possibile usare [Docker per Windows](https://www.docker.com/docker-windows) come motore del contenitore. Docker può essere configurato per l'[uso di contenitori Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers). È necessario installare Docker e configurarlo prima di installare IoT Edge. I contenitori Linux non sono supportati nei dispositivi Windows nell'ambiente di produzione. 

Se il dispositivo IoT Edge è un computer Windows, assicurarsi che soddisfi i [requisiti di sistema](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements) per Hyper-V.

## <a name="install-iot-edge-on-a-new-device"></a>Installare IoT Edge in un nuovo dispositivo

>[!NOTE]
>I pacchetti software di Azure IoT Edge sono soggetti alle condizioni di licenza riportate nei pacchetti, all'interno della directory LICENSE. Prima di usare il pacchetto, leggere le condizioni di licenza. L'installazione e l'uso del pacchetto costituiscono accettazione di tali condizioni. Se non si accettano le condizioni di licenza, non usare il pacchetto.

Uno script di PowerShell scarica e installa il daemon di sicurezza di Azure IoT Edge. Il daemon di sicurezza avvia quindi il primo di due moduli di runtime, l'agente IoT Edge, che consente distribuzioni remote di altri moduli. 

Quando si installa il runtime di IoT Edge per la prima volta in un dispositivo, è necessario effettuare il provisioning del dispositivo con un'identità da un hub IoT. È possibile effettuare il provisioning di un singolo dispositivo IoT Edge manualmente usando una stringa di connessione dispositivo fornita dall'hub IoT. In alternativa, è possibile usare il servizio Device Provisioning per effettuare automaticamente il provisioning dei dispositivi. Ciò è utile quando si dispone di molti dispositivi da configurare. A seconda del provisioning, scegliere lo script di installazione appropriato. 

Le sezioni seguenti descrivono i casi d'uso comuni e i parametri per lo script di installazione di IoT Edge in un nuovo dispositivo. 

### <a name="option-1-install-and-manually-provision"></a>Opzione 1: Effettuare il provisioning e l'installazione manualmente

In questa prima opzione viene fornita una stringa di connessione del dispositivo generata dall'hub IoT per il provisioning del dispositivo. 

Seguire i passaggi descritti in [Registrare un nuovo dispositivo Azure IoT Edge](how-to-register-device-portal.md) per registrare il dispositivo e recuperare la stringa di connessione. 

Questo esempio mostra un'installazione manuale con contenitori Windows:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -ContainerOs Windows -DeviceConnectionString '<connection-string>'
```

Quando si installa e si effettua il provisioning di un dispositivo manualmente, è possibile usare ulteriori parametri per modificare l'installazione tra cui:
* Indirizzare il traffico attraverso un server proxy
* Fare in modo che il programma di installazione punti a una directory offline
* Dichiarare un'immagine del contenitore di un agente specifico e fornire le credenziali se si trova in un registro privato
* Ignorare l'installazione dell'interfaccia della riga di comando di Moby

Per altre informazioni su queste opzioni di installazione, continuare a leggere questo articolo oppure passare direttamente a [Tutti i parametri di installazione](#all-installation-parameters).

### <a name="option-2-install-and-automatically-provision"></a>Opzione 2: Installare ed effettuare automaticamente il provisioning

In questa seconda opzione viene effettuato il provisioning del dispositivo con il servizio Device Provisioning in hub IoT. Fornire l'**ID ambito** da un'istanza del servizio Device Provisioning e l'**ID registrazione** dal dispositivo.

Seguire i passaggi descritti in [Creare ed effettuare il provisioning di un dispositivo simulato TPM Edge in Windows](how-to-auto-provision-simulated-device-windows.md) per configurare il servizio di provisioning di dispositivi e recuperare il relativo **ID ambito**, simulare un dispositivo TPM e recuperare il relativo **ID registrazione**, quindi creare una registrazione singola. Dopo aver registrato il dispositivo nell'hub IoT, continuare con l'installazione.  

   >[!TIP]
   >Mantenere aperta la finestra di esecuzione del simulatore TPM durante l'installazione e il test. 

L'esempio seguente mostra un'installazione automatica con i contenitori Windows:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Dps -ContainerOs Windows -ScopeId <DPS scope ID> -RegistrationId <device registration ID>
```

Quando si installa e si effettua il provisioning di un dispositivo manualmente, è possibile usare ulteriori parametri per modificare l'installazione tra cui:
* Indirizzare il traffico attraverso un server proxy
* Fare in modo che il programma di installazione punti a una directory offline
* Dichiarare un'immagine del contenitore di un agente specifico e fornire le credenziali se si trova in un registro privato
* Ignorare l'installazione dell'interfaccia della riga di comando di Moby

Per altre informazioni su queste opzioni di installazione, continuare a leggere questo articolo oppure passare direttamente a [Tutti i parametri di installazione](#all-installation-parameters).

## <a name="update-an-existing-installation"></a>Aggiornare un'installazione esistente

Se il runtime di IoT Edge è già stato installato in precedenza in un dispositivo ed è stato effettuato il provisioning con un'identità dall'hub IoT, è possibile usare uno script di installazione semplificato. Il flag `-ExistingConfig` dichiara che nel dispositivo esiste già un file di configurazione di IoT Edge. Il file di configurazione contiene informazioni sull'identità del dispositivo, nonché i certificati e le impostazioni di rete. È possibile usare questa opzione di installazione indipendentemente dal fatto che il provisioning del dispositivo sia stato effettuato manualmente o automaticamente. 

Per altre informazioni, vedere [Aggiornare il daemon di sicurezza e il runtime di IoT Edge](how-to-update-iot-edge.md).

Questo esempio mostra un'installazione che punta a un file di configurazione esistente e usa i contenitori Windows: 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -ExistingConfig -ContainerOs Windows
```

Quando si installa IoT Edge da un file di configurazione esistente, è possibile usare ulteriori parametri per modificare l'installazione tra cui:
* Indirizzare il traffico attraverso un server proxy
* Fare in modo che il programma di installazione punti a una directory offline o 
* Ignorare l'installazione dell'interfaccia della riga di comando di Moby. 

Non è possibile dichiarare un'immagine del contenitore dell'agente IoT Edge con i parametri di script, poiché tali informazioni sono già impostate nel file di configurazione dall'installazione precedente. Se si vuole modificare l'immagine del contenitore dell'agente, eseguire questa operazione nel file config.yaml. 

Per altre informazioni su queste opzioni di installazione, continuare a leggere questo articolo oppure passare direttamente a [Tutti i parametri di installazione](#all-installation-parameters).

## <a name="offline-installation"></a>Installazione offline

Durante l'installazione vengono scaricati quattro file: 
* ZIP del daemon di sicurezza di IoT Edge (iotedgd) 
* ZIP del motore Moby
* ZIP dell'interfaccia della riga di comando di Moby
* MSI di Visual C++ Redistributable Package (runtime di VC)

È possibile scaricare uno o tutti questi file in precedenza nel dispositivo e quindi fare in modo che lo script di installazione punti alla directory contenente i file. Il programma di installazione verifica innanzitutto il contenuto della directory e quindi scarica solo i componenti che non sono già presenti. Se tutti i quattro file sono disponibili offline, è possibile procedere all'installazione senza connessione Internet. È anche possibile usare questa funzionalità per sostituire le versioni online di uno o più componenti.  

Per i file di installazione più recenti insieme alle versioni precedenti, fare riferimento alle [versioni di Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases)

Per procedere all'installazione con i componenti offline, usare il parametro `-OfflineInstallationPath` e specificare il percorso assoluto della directory del file. Ad esempio,

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -DeviceConnectionString '<connection-string>' -OfflineInstallationPath C:\Downloads\iotedgeoffline
```

## <a name="all-installation-parameters"></a>Tutti i parametri di installazione

Nelle sezioni precedenti sono stati introdotti alcuni scenari di installazione comuni con esempi di come usare i parametri per modificare lo script di installazione. Questa sezione fornisce una tabella di riferimento dei parametri validi che è possibile usare per installare IoT Edge. Per altre informazioni, eseguire `get-help Install-SecurityDaemon -full` in una finestra di PowerShell. 

Per installare IoT Edge con una configurazione esistente, il comando di installazione può usare questi parametri comuni: 

| Parametro | Valori accettati | Commenti |
| --------- | --------------- | -------- |
| **Manuale** | None | **Parametro opzionale**. Ogni installazione deve essere dichiarata manuale, DPS o existingconfig.<br><br>Dichiara che si fornirà una stringa di connessione del dispositivo per effettuare il provisioning del dispositivo manualmente. |
| **Dps** | None | **Parametro opzionale**. Ogni installazione deve essere dichiarata manuale, DPS o existingconfig.<br><br>Dichiara che si forniranno un ID ambito del servizio Device Provisioning (DPS) e l'ID registrazione del dispositivo per effettuare il provisioning tramite DPS.  |
| **ExistingConfig** | None | **Parametro opzionale**. Ogni installazione deve essere dichiarata manuale, DPS o existingconfig.<br><br>Dichiara che nel dispositivo esiste già un file config.yaml con le informazioni di provisioning. |
| **DeviceConnectionString** | Una stringa di connessione da un dispositivo IoT Edge registrato in un hub IoT, tra virgolette singole | **Obbligatorio** per l'installazione manuale. Se non si specifica una stringa di connessione nei parametri di script, ne verrà richiesta una durante l'installazione. |
| **ScopeId** | Un ID ambito da un'istanza del servizio Device Provisioning associata all'hub IoT. | **Obbligatorio** per l'installazione di DPS. Se non si specifica un ID ambito nei parametri di script, ne verrà richiesto uno durante l'installazione. |
| **RegistrationId** | Un ID registrazione generato dal dispositivo | **Obbligatorio** per l'installazione di DPS. Se non si specifica un ID registrazione nei parametri di script, ne verrà richiesto uno durante l'installazione. |
| **ContainerOs** | **Windows** o **Linux** | Se non è specificato alcun sistema operativo del contenitore, il valore predefinito è Linux. Per i contenitori Windows, nell'installazione verrà incluso un motore del contenitore. Per i contenitori Linux, è necessario installare un motore del contenitore prima di avviare l'installazione. L'esecuzione di contenitori Linux in Windows costituisce uno scenario di sviluppo utile, ma non è supportata nell'ambiente di produzione. |
| **Proxy** | URL proxy | Includere questo parametro se il dispositivo deve passare attraverso un server proxy per accedere a Internet. Per altre informazioni, vedere [Configurare un dispositivo IoT Edge per comunicare tramite un server proxy](how-to-configure-proxy-support.md). |
| **InvokeWebRequestParameters** | Tabella hash dei parametri e dei valori | Durante l'installazione vengono effettuate più richieste Web. Usare questo campo per impostare i parametri per queste richieste Web. Questo parametro è utile per configurare le credenziali per i server proxy. Per altre informazioni, vedere [Configurare un dispositivo IoT Edge per comunicare tramite un server proxy](how-to-configure-proxy-support.md). |
| **OfflineInstallationPath** | Percorso directory | Se questo parametro è incluso, il programma di installazione verificherà che la directory contenga i file ZIP iotedged, del motore Moby e dell'interfaccia della riga di comando di Moby e il file MSI del runtime di VC necessari per l'installazione. Se nella directory sono presenti tutti i quattro file, è possibile installare IoT Edge in modalità offline. È anche possibile usare questo parametro per sostituire la versione online di un componente specifico. |
| **AgentImage** | URI dell'immagine dell'agente IoT Edge | Per impostazione predefinita, una nuova installazione di IoT Edge usa il tag di versioni più recente per l'immagine dell'agente IoT Edge. Usare questo parametro per impostare un tag specifico per la versione dell'immagine o per fornire la propria immagine dell'agente. Per altre informazioni, vedere [Informazioni sui tag di IoT Edge](how-to-update-iot-edge.md#understand-iot-edge-tags). |
| **Nome utente** | Nome utente del registro contenitori | Usare questo parametro solo se si imposta il parametro -AgentImage su un contenitore in un registro privato. Specificare un nome utente con accesso al registro. |
| **Password** | Stringa della password di protezione | Usare questo parametro solo se si imposta il parametro -AgentImage su un contenitore in un registro privato. Specificare la password per accedere al registro. | 
| **SkipMobyCli** | None | Applicabile solo se -ContainerOS è impostato su Windows. Non installare l'interfaccia della riga di comando di Moby (docker.exe) su $MobyInstallDirectory. |

## <a name="verify-successful-installation"></a>Verificare l'esito positivo dell'installazione

È possibile verificare lo stato del servizio IoT Edge nel modo seguente: 

```powershell
Get-Service iotedge
```

Esaminare i log del servizio generati negli ultimi cinque minuti nel modo seguente:

```powershell

# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false} |
  format-table -autosize -wrap
```

Elencare infine i moduli in esecuzione con il comando seguente:

```powershell
iotedge list
```

Dopo una nuova installazione, il modulo solo dovrebbe essere in esecuzione **edgeAgent**. Dopo aver [distribuire moduli IoT Edge](how-to-deploy-modules-portal.md), si noterà ad altri utenti. 

## <a name="manage-module-containers"></a>Gestire i contenitori di modulo

Il servizio di IoT Edge richiede un motore contenitore in esecuzione nel dispositivo. Quando si distribuisce un modulo in un dispositivo, il runtime di IoT Edge Usa il motore di contenitori per eseguire il pull dell'immagine del contenitore da un registro nel cloud. Il servizio di IoT Edge consente di interagire con i moduli e recuperare i log, ma a volte è consigliabile usare il motore del contenitore per interagire con il contenitore stesso. 

Per altre informazioni sui concetti di modulo, vedere [moduli comprendere Azure IoT Edge](iot-edge-modules.md). 

Se si eseguono i contenitori di Windows nel dispositivo Windows IoT Edge, l'installazione di IoT Edge incluso il motore contenitore Moby. Se si sta sviluppando i contenitori Linux nel computer di sviluppo Windows, probabilmente si usa Docker Desktop. Il motore Moby si basava sugli stessi standard di Docker ed è stato progettato per l'esecuzione in parallelo nello stesso computer Desktop di Docker. Per questo motivo, se si desidera contenitori destinazione gestiti dal motore Moby, è necessario scegliere come destinazione in modo specifico tale motore invece di Docker. 

Ad esempio, per elencare tutte le immagini Docker, usare il comando seguente:

```powershell
docker images
```

Per elencare tutte le immagini Moby, modificare lo stesso comando con un puntatore al motore di Moby: 

```powershell
docker -H npipe:////./pipe/iotedge_moby_engine images
```

Il motore di URI è elencato nell'output dello script di installazione oppure è possibile trovarlo nella sezione Impostazioni di runtime contenitore per il file config. yaml. 

![uri moby_runtime in config. yaml](./media/how-to-install-iot-edge-windows/moby-runtime-uri.png)

Per altre informazioni sui comandi è possibile usare per interagire con i contenitori e immagini in esecuzione nel dispositivo, vedere [interfacce della riga di comando di Docker](https://docs.docker.com/engine/reference/commandline/docker/).

## <a name="uninstall-iot-edge"></a>Disinstallare IoT Edge

Per rimuovere l'installazione di IoT Edge da un dispositivo Windows, usare il comando seguente da una finestra amministrativa di PowerShell. Questo comando rimuove il runtime di IoT Edge, insieme alla configurazione esistente e ai dati del motore Moby. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-SecurityDaemon -DeleteConfig -DeleteMobyDataRoot
```

Se si vuole reinstallare IoT Edge nel dispositivo, omettere i parametri `-DeleteConfig` e `-DeleteMobyDataRoot` in modo che sia possibile reinstallare il daemon di sicurezza in un secondo momento con le informazioni di configurazione esistenti. 

## <a name="next-steps"></a>Passaggi successivi

Dopo aver eseguito il provisioning del dispositivo IoT Edge con il runtime installato, è possibile [distribuire moduli IoT Edge](how-to-deploy-modules-portal.md).

In caso di problemi che impediscono di installare correttamente IoT Edge, vedere la pagina relativa alla [risoluzione dei problemi](troubleshoot.md).

Per aggiornare un'installazione esistente alla versione più recente di IoT Edge, vedere [Aggiornare il daemon di sicurezza e il runtime di IoT Edge](how-to-update-iot-edge.md).

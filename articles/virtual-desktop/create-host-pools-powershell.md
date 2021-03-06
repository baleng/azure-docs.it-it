---
title: Creare un pool di host di anteprima di Desktop virtuale Windows con PowerShell - Azure
description: Come creare un pool di host in anteprima di Desktop virtuale Windows con i cmdlet di PowerShell.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/05/2019
ms.author: helohr
ms.openlocfilehash: 2af9df4771d58f2288820dad8ef8d7ac84deb8ae
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59258471"
---
# <a name="create-a-host-pool-with-powershell"></a>Creare un pool di host con PowerShell

I pool di host sono una raccolta di una o più macchine virtuali identiche all'interno di ambienti tenant dell'anteprima di Desktop virtuale Windows. Ogni pool di host può contenere un gruppo di app con cui gli utenti possono interagire come farebbero in un desktop fisico.

## <a name="use-your-powershell-client-to-create-a-host-pool"></a>Usare il client di PowerShell per creare un pool di host

Prima di tutto, [scaricare e importare il modulo Desktop virtuale Windows di PowerShell](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) da usare nella sessione di PowerShell, se non è già stato fatto.

Eseguire il cmdlet seguente per accedere all'ambiente di Desktop virtuale Windows

```powershell
Add-RdsAccount -DeploymentUrl https://rdbroker.wvd.microsoft.com
```

Successivamente, eseguire il cmdlet seguente per impostare il contesto per il gruppo di tenant. Se non si ha il nome del gruppo di tenant, il tenant è più probabile "Default Tenant gruppo" in modo che è possibile ignorare questo cmdlet.

```powershell
Set-RdsContext -TenantGroupName <tenantgroupname>
```

Successivamente, eseguire questo cmdlet per creare un nuovo pool di host nel proprio tenant di Desktop virtuale Windows:

```powershell
New-RdsHostPool -TenantName <tenantname> -Name <hostpoolname>
```

Eseguire il successivo cmdlet per creare un token di registrazione per autorizzare un host di sessione a vengono aggiunti al pool di host e salvarlo in un nuovo file nel computer locale. È possibile specificare quanto tempo il token di registrazione è valido utilizzando il parametro - ExpirationHours.

```powershell
New-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname> -ExpirationHours <number of hours> | Select-Object -ExpandProperty Token > <PathToRegFile>
```

Successivamente, eseguire questo cmdlet per aggiungere gli utenti di Azure Active Directory al gruppo di app desktop predefinito per il pool di host.

```powershell
Add-RdsAppGroupUser -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName "Desktop Application Group" -UserPrincipalName <userupn>
```

Il **Add-RdsAppGroupUser** cmdlet non supporta l'aggiunta di gruppi di sicurezza e aggiunge solo un utente alla volta per il gruppo di app. Se si desidera aggiungere più utenti al gruppo di app, eseguire nuovamente il cmdlet con i nomi dell'entità utente appropriato.

Eseguire il cmdlet seguente per esportare il token di registrazione in una variabile, che verrà usato successivamente nel [registrare le macchine virtuali nel pool di Desktop virtuale di Windows host](#register-the-virtual-machines-to-the-windows-virtual-desktop-preview-host-pool).

```powershell
$token = (Export-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname>).Token
```

## <a name="create-virtual-machines-for-the-host-pool"></a>Creare macchine virtuali per il pool di host

A questo punto è possibile creare una macchina virtuale di Azure che possono essere aggiunti al pool di host di Desktop virtuale Windows.

È possibile creare una macchina virtuale in più modi:

- [Creare una macchina virtuale da un'immagine della raccolta di Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#create-virtual-machine)
- [Creare una macchina virtuale da un'immagine gestita](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed)
- [Creare una macchina virtuale da un'immagine non gestita](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

## <a name="prepare-the-virtual-machines-for-windows-virtual-desktop-preview-agent-installations"></a>Preparare le macchine virtuali per le installazioni dell'agente di anteprima di Desktop virtuale Windows

È necessario eseguire le operazioni seguenti per preparare le macchine virtuali prima di installare gli agenti Windows Desktop virtuale e registrare le macchine virtuali al pool di host di Desktop virtuale Windows:

- È necessario aggiunta al dominio del computer. Ciò consente agli utenti di Desktop virtuale Windows in arrivo eseguire il mapping dal proprio account Azure Active Directory al proprio account Active Directory ed è stato consentito l'accesso alla macchina virtuale.
- Se la macchina virtuale è in esecuzione un sistema operativo Windows Server, è necessario installare il ruolo Host sessione Desktop remoto (RDSH). Il ruolo Host sessione Desktop remoto consente gli agenti di Desktop virtuale Windows da installare in modo corretto.

A è stata aggiunta al dominio, eseguire le operazioni seguenti in ogni macchina virtuale:

1. [Connettersi alla macchina virtuale](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) con le credenziali specificate durante la creazione della macchina virtuale.
2. Nella macchina virtuale, avviare **Pannello di controllo** e selezionare **sistema**.
3. Selezionare **nome Computer**, selezionare **modificare le impostazioni**, quindi selezionare **modifica...**
4. Selezionare **dominio** e quindi immettere il dominio di Active Directory nella rete virtuale.
5. Eseguire l'autenticazione con un account di dominio che disponga dei privilegi per le macchine di aggiunta al dominio.

## <a name="register-the-virtual-machines-to-the-windows-virtual-desktop-preview-host-pool"></a>Registrare le macchine virtuali al pool di host di anteprima di Desktop virtuale Windows

Le macchine virtuali a un pool di Desktop virtuale di Windows host di registrazione è semplice come installare gli agenti Windows Desktop virtuale.

Per registrare gli agenti Windows Desktop virtuale, eseguire le operazioni seguenti in ogni macchina virtuale:

1. [Connettersi alla macchina virtuale](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) con le credenziali specificate durante la creazione della macchina virtuale.
2. Scaricare e installare l'agente di Desktop virtuali di Windows.
   - Scaricare il [agente di Desktop virtuale Windows](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrmXv).
   - Fare clic sul programma di installazione scaricato, selezionare **delle proprietà**, selezionare **Unblock**, quindi selezionare **OK**. In questo modo il sistema di considerare attendibile il programma di installazione.
   - Eseguire il programma di installazione. Quando il programma di installazione viene richiesto per il token di registrazione, immettere il valore ottenuto tramite il **Export-RdsRegistrationInfo** cmdlet.
3. Scaricare e installare il caricatore di avvio Windows virtuale dell'agente di Desktop.
   - Scaricare il [caricatore di avvio di Windows dell'agente di Desktop virtuale](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrxrH).
   - Fare clic sul programma di installazione scaricato, selezionare **delle proprietà**, selezionare **Unblock**, quindi selezionare **OK**. In questo modo il sistema di considerare attendibile il programma di installazione.
   - Eseguire il programma di installazione.
4. Installare oppure attivare lo stack side-by-side di Desktop virtuale Windows. I passaggi saranno diversi a seconda di quale versione del sistema operativo Usa la macchina virtuale.
   - Se il sistema operativo della macchina virtuale è Windows Server 2016:
     - Scaricare il [stack side-by-side di Desktop virtuale Windows](https://go.microsoft.com/fwlink/?linkid=2084270).
     - Fare clic sul programma di installazione scaricato, selezionare **delle proprietà**, selezionare **Unblock**, quindi selezionare **OK**. In questo modo il sistema di considerare attendibile il programma di installazione.
     - Eseguire il programma di installazione.
   - Se il sistema operativo della macchina virtuale è Windows 10 1809 o versioni successive o Windows Server 2019 o versione successiva:
     - Scaricare il [script](https://go.microsoft.com/fwlink/?linkid=2084268) per attivare lo stack side-by-side.
     - Fare clic sullo script scaricato, selezionare **delle proprietà**, selezionare **Unblock**, quindi selezionare **OK**. In questo modo il sistema di considerare attendibile lo script.
     - Dal **avviare** menu, cercare Windows PowerShell ISE, pulsante destro del mouse, quindi selezionare **Esegui come amministratore**.
     - Selezionare **File**, quindi **aprire...** e quindi trovare lo script di PowerShell dai file scaricati e aprirlo.
     - Selezionare il pulsante di riproduzione di colore verde per eseguire lo script.

>[!IMPORTANT]
>Per proteggere l'ambiente di Desktop virtuale Windows in Azure, è consigliabile che non aprire la porta in ingresso 3389 nelle macchine virtuali. Desktop virtuale di Windows non richiede una porta in ingresso aperta 3389 per gli utenti per accedere alle macchine virtuali del pool di host. Se è necessario aprire la porta 3389 per la risoluzione dei problemi, è consigliabile usare [-in-time alla VM](https://docs.microsoft.com/en-us/azure/security-center/security-center-just-in-time).

## <a name="next-steps"></a>Passaggi successivi

Ora che sono state apportate a un pool di host, è possibile popolarlo con RemoteApp. Per altre informazioni su come gestire le app in Desktop virtuale Windows, vedere l'esercitazione Gestire i gruppi di app.

> [!div class="nextstepaction"]
> [Gestire l'esercitazione di gruppi di app](./manage-app-groups.md)

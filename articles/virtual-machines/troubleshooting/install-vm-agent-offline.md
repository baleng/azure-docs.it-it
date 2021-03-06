---
title: Installare l'agente di macchine virtuali di Azure in modalità offline | Microsoft Docs
description: Informazioni su come installare l'agente di macchine virtuali di Azure in modalità offline.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: e9fc8351b5e9a4f2274f0906d4071f86dcbcff26
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2019
ms.locfileid: "54259683"
---
# <a name="install-the-azure-virtual-machine-agent-in-offline-mode"></a>Installare l'agente di macchine virtuali di Azure in modalità offline 

L'agente di macchine virtuali di Azure offre funzionalità utili, ad esempio la reimpostazione della password dell'amministratore locale e il push di script. Questo articolo illustra come installare l'agente di macchine virtuali per una macchina virtuale di Windows offline. 

## <a name="when-to-use-the-vm-agent-in-offline-mode"></a>Quando usare l'agente di macchine virtuali in modalità offline

Installare l'agente di macchine virtuali in modalità offline negli scenari seguenti:

- Nella macchina virtuale di Azure distribuita non è installato l'agente oppure l'agente non funziona.
- Si è dimenticata la password dell'amministratore per la macchina virtuale o non è possibile accedere alla macchina virtuale.

## <a name="how-to-install-the-vm-agent-in-offline-mode"></a>Come installare l'agente di macchine virtuali in modalità offline

Usare la procedura seguente per installare l'agente di macchine virtuali in modalità offline.

> [!NOTE]
> È possibile automatizzare il processo di installazione dell'agente di macchine virtuali in modalità offline.
> A questo scopo, usare gli [Azure VM Recovery Scripts](https://github.com/Azure/azure-support-scripts/blob/master/VMRecovery/ResourceManager/README.md) (script di ripristino della macchina virtuale di Azure). Se si sceglie di usare gli script di ripristino della macchina virtuale di Azure, è possibile seguire la procedura seguente:
> 1. Ignorare il passaggio 1 usando gli script per collegare il disco del sistema operativo della VM interessata a una macchina virtuale di ripristino.
> 2. Seguire i passaggi da 2 a 10 per applicare le mitigazioni.
> 3. Ignorare il passaggio 11 usando gli script per ricreare la macchina virtuale.
> 4. Seguire il passaggio 12.

### <a name="step-1-attach-the-os-disk-of-the-vm-to-another-vm-as-a-data-disk"></a>Passaggio 1: Collegare il disco del sistema operativo della macchina virtuale a un'altra macchina virtuale come disco dati

1.  Eliminare la macchina virtuale. Assicurarsi di aver selezionato l'opzione **Keep the disks** (Mantieni i dischi) quando si esegue questa operazione.

2.  Collegare il disco del sistema operativo come disco dati a un'altra macchina virtuale (nota come _macchina virtuale per la risoluzione dei problemi_). Per altre informazioni, vedere [Collegare un disco dati a una macchina virtuale Windows nel portale di Azure](../windows/attach-managed-disk-portal.md).

3.  Connettersi alla macchina virtuale per la risoluzione dei problemi. Aprire **Gestione computer** > **Gestione disco**. Verificare che il disco del sistema operativo sia online e che siano assegnate lettere di unità alle partizioni del disco.

### <a name="step-2-modify-the-os-disk-to-install-the-azure-vm-agent"></a>Passaggio 2: Modificare il disco del sistema operativo per installare l'agente di macchine virtuali di Azure

1.  Attivare una connessione Desktop remoto alla macchina virtuale per la risoluzione dei problemi.

2.  Nel disco del sistema operativo collegato passare alla cartella \windows\system32\config. Copiare tutti i file in questa cartella come backup, nel caso risulti necessario un ripristino dello stato precedente.

3.  Avviare l'**Editor del Registro di sistema** (regedit.exe).

4.  Selezionare la chiave **HKEY_LOCAL_MACHINE**. Scegliere **File** > **Carica hive** dal menu:

    ![Caricare l'hive](./media/install-vm-agent-offline/load-hive.png)

5.  Passare alla cartella \windows\system32\config\SYSTEM nel disco del sistema operativo collegato. Immettere il nome di hive **BROKENSYSTEM**. Il nuovo hive del Registro di sistema viene visualizzato per la chiave **HKEY_LOCAL_MACHINE**.

6.  Passare alla cartella \windows\system32\config\SOFTWARE nel disco del sistema operativo collegato. Immettere il nome di hive **BROKENSOFTWARE**.

7. Se l'agente di macchine virtuali è installato sul disco del sistema operativo collegato, eseguire un backup della configurazione corrente. Se l'agente di macchine virtuali non è stato installato, andare al passaggio successivo.
      
    1. Rinominare la cartella \windowsazure in \windowsazure.old.

    2. Esportare le voci del Registro di sistema seguenti:
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\RdAgent

8.  Usare i file esistenti nella macchina virtuale per la risoluzione dei problemi come repository per l'installazione dell'agente di macchine virtuali. Completare questi passaggi:

    1. Dalla macchina virtuale per la risoluzione dei problemi esportare le sottochiavi seguenti nel formato del Registro di sistema (con estensione reg): 
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\RdAgent

          ![Esportare le sottochiavi del Registro di sistema](./media/install-vm-agent-offline/backup-reg.png)

    2. Modificare i file del Registro di sistema. In ogni file, modificare il valore della voce **SYSTEM** in **BROKENSYSTEM** (come illustrato nelle immagini seguenti) e salvare il file. Tenere presente l'**ImagePath** dell'agente di macchine virtuali corrente. È necessario copiare la cartella corrispondente nel disco del sistema operativo collegato. 

        ![Modificare i valori della sottochiave del Registro di sistema](./media/install-vm-agent-offline/change-reg.png)

    3. Importare i file del Registro di sistema nel repository facendo doppio clic su ogni file del Registro di sistema.

    4. Verificare che le tre sottochiavi seguenti vengano importate correttamente nell'hive **BROKENSYSTEM**:
        - WindowsAzureGuestAgent
        - WindowsAzureTelemetryService
        - RdAgent

    5. Copia la cartella di installazione dell'agente di macchine virtuali corrente nel disco del sistema operativo collegato: 

        1.  Nel disco del sistema operativo collegato, creare una cartella denominata WindowsAzure nel percorso radice.

        2.  Passare a C:\WindowsAzure nella risoluzione dei problemi della macchina virtuale, cercare qualsiasi cartella con nome C:\WindowsAzure\GuestAgent_X.X.XXXX.XXX. Copiare la cartella GuestAgent con numero di versione più recente da C:\WindowsAzure nella cartella WindowsAzure nel disco del sistema operativo collegato. Se non si conosce quale cartella deve essere copiata, copiare tutte le cartelle GuestAgent. L'immagine seguente mostra un esempio della cartella GuestAgent che viene copiata nel disco del sistema operativo collegato.

             ![Copiare la cartella GuestAgent](./media/install-vm-agent-offline/copy-files.png)

9.  Selezionare **BROKENSYSTEM**. Scegliere **File** > **Scarica hive** dal menu.

10.  Selezionare **BROKENSOFTWARE**. Scegliere **File** > **Scarica hive** dal menu.

11.  Scollegare il disco del sistema operativo e quindi ricreare la macchina virtuale usando il disco del sistema operativo.

12.  Accedere alla macchina virtuale. Si noti che RdAgent è in esecuzione e i log vengono generati.

Se la macchina virtuale è stata creata tramite il modello di distribuzione di Resource Manager la procedura è completa.

### <a name="use-the-provisionguestagent-property-for-classic-vms"></a>Usare la proprietà ProvisionGuestAgent per le macchine virtuali classiche

Se la macchina virtuale è stata creata con il modello classico, usare il modulo di Azure PowerShell per aggiornare la proprietà **ProvisionGuestAgent**. La proprietà informa Azure che nella macchina virtuale è installato l'agente di macchine virtuali.

Per impostare la proprietà **ProvisionGuestAgent** eseguire i comandi seguenti in Azure PowerShell:

   ```powershell
   $vm = Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
   $vm.VM.ProvisionGuestAgent = $true
   Update-AzureVM –Name <VM name> –VM $vm.VM –ServiceName <cloud service name>
   ```

Eseguire quindi il comando `Get-AzureVM` . Si noti che la proprietà **GuestAgentStatus** è ora compilata:

   ```powershell
   Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
   GuestAgentStatus:Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVMModel.GuestAgentStatus
   ```

## <a name="next-steps"></a>Passaggi successivi

- [Panoramica dell'agente di macchine virtuali di Azure](../extensions/agent-windows.md)
- [Estensioni e funzionalità della macchina virtuale per Windows](../extensions/features-windows.md)

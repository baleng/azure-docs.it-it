---
title: Integrazione di Automazione di Azure con Griglia di eventi | Microsoft Docs
description: Informazioni su come aggiungere automaticamente un tag quando viene creata una nuova macchina virtuale e inviare una notifica a Microsoft Teams.
keywords: automazione, runbook, team, griglia di eventi, macchina virtuale, VM
services: automation
author: eamonoreilly
manager: ''
ms.service: automation
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 01/14/2019
ms.author: eamono
ms.openlocfilehash: d0764131f0e7e321a87ed383636606b2124ef7d9
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58173771"
---
# <a name="tutorial-integrate-azure-automation-with-event-grid-and-microsoft-teams"></a>Esercitazione: Integrazione di Automazione di Azure con Griglia di eventi e Microsoft Teams

In questa esercitazione si apprenderà come:

> [!div class="checklist"]
> * Importare un runbook di esempio della Griglia di eventi.
> * Creare un webhook Microsoft Teams facoltativo.
> * Creare un webhook per il runbook.
> * Creare una sottoscrizione di Griglia di eventi.
> * Creare una macchina virtuale che attivi il runbook.

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

Per completare questa esercitazione, è necessario un [account di Automazione di Azure](../automation/automation-offering-get-started.md) per contenere il runbook che viene attivato dalla sottoscrizione Griglia di eventi di Azure.

* Il modulo `AzureRM.Tags` deve essere caricato nell'Account di automazione. Vedere [Come aggiornare i moduli di Azure PowerShell in Automazione di Azure](../automation/automation-update-azure-modules.md) per informazioni su come importare i moduli in Automazione di Azure.

## <a name="import-an-event-grid-sample-runbook"></a>Importare un runbook di esempio della griglia di eventi

1. Aprire l'account di Automazione e fare clic sulla pagina **Runbook**.

   ![Selezionare i runbook](./media/ensure-tags-exists-on-new-virtual-machines/select-runbooks.png)

2. Selezionare il pulsante **Sfoglia raccolta**.

3. Cercare **Griglia di eventi**e selezionare **Integrating Azure Automation with Event grid** (Integrazione di Automazione di Azure con griglia di eventi).

    ![Importare il runbook della raccolta](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)

4. Selezionare **Importazione** e denominarlo **Watch-VMWrite**.

5. Una volta importato, selezionare **Modifica** per visualizzare l'origine runbook. Fare clic sul pulsante **Pubblica**.

> [!NOTE]
> La riga 74 nello script deve essere modificata in `Update-AzureRmVM -ResourceGroupName $VMResourceGroup -VM $VM -Tag $Tag | Write-Verbose`. Il parametro `-Tags` è ora `-Tag`.

## <a name="create-an-optional-microsoft-teams-webhook"></a>Creare un webhook Microsoft Teams facoltativo

1. In Microsoft Teams, selezionare **Opzioni** accanto al nome del canale, quindi fare clic su **Connettori**.

    ![Connessioni Microsoft Teams](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)

2. Scorrere l'elenco dei connettori fino a **Incoming Webhook** (Webhook in ingresso) e selezionare **Aggiungi**.

3. Immettere **AzureAutomationIntegration** come nome e selezionare **Crea**.

4. Copiare il webhook negli Appunti e salvarlo. L'URL del webhook viene usato per inviare informazioni a Microsoft Teams.

5. Selezionare **Operazione completata** per salvare il webhook.

## <a name="create-a-webhook-for-the-runbook"></a>Creare un webhook per il runbook

1. Aprire il runbook Watch-VMWrite.

2. Selezionare **Webhook**e selezionare il pulsante **Aggiungi webhook**.

3. Immettere **WatchVMEventGrid** come nome. Copiare l'URL negli Appunti e salvarlo.

    ![Configurare il nome del webhook](media/ensure-tags-exists-on-new-virtual-machines/copy-url.png)

4. Selezionare **Configura i parametri e le impostazioni di esecuzione**, quindi immettere l'URL del webhook Microsoft Teams per **CHANNELURL**. Lasciare **WEBHOOKDATA** vuoto.

    ![Configurare i parametri del webhook](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)

5. Selezionare **Crea** per creare il webhook del runbook di Automazione.

## <a name="create-an-event-grid-subscription"></a>Creare una sottoscrizione di Griglia di eventi

1. Nella pagina della panoramica di **Account di automazione**, selezionare **Griglia di eventi**.

    ![Selezionare Griglia di eventi](media/ensure-tags-exists-on-new-virtual-machines/select-event-grid.png)

2. Fare clic su **+ Sottoscrizione di eventi**.

3. Configurare la sottoscrizione con le informazioni seguenti:

   * Per **Tipo di argomento** selezionare **Sottoscrizioni di Azure**.
   * Deselezionare la casella di controllo **Esegui la sottoscrizione di tutti i tipi di eventi**.
   * Immettere **AzureAutomation** come nome.
   * Nell'elenco a discesa **Tipi di evento definiti**deselezionare tutte le opzioni ad eccezione di **Resource Write Success** (Scrittura risorse corretta).
   * Per **Tipo di endpoint**: selezionare **Webhook**.
   * Fare clic su **Seleziona endpoint**. Nella pagina **Selezionare il webhook** visualizzata incollare l'URL del webhook creato per il runbook Watch-VMWrite.
   * In **FILTRI** immettere la sottoscrizione e il gruppo di risorse in cui si vuole cercare le nuove macchine virtuali create. L'aspetto dovrebbe essere simile al seguente: `/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines`

4. Selezionare **Crea** per salvare la sottoscrizione della Griglia di eventi.

## <a name="create-a-vm-that-triggers-the-runbook"></a>Creare una macchina virtuale che attivi il runbook

1. Creare una nuova macchina virtuale nel gruppo di risorse specificato nel filtro per prefisso della sottoscrizione della Griglia di eventi.

2. Il runbook Watch-VMWrite dovrebbe essere richiamato e alla macchina virtuale dovrebbe essere aggiunto un nuovo tag.

    ![Tag della macchina virtuale](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)

3. Al canale Microsoft Teams viene inviato un nuovo messaggio.

    ![Notifica Microsoft Teams](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione, è stata impostata l'integrazione tra l'Automazione e la Griglia di eventi. Si è appreso come:

> [!div class="checklist"]
> * Importare un runbook di esempio della Griglia di eventi.
> * Creare un webhook Microsoft Teams facoltativo.
> * Creare un webhook per il runbook.
> * Creare una sottoscrizione di Griglia di eventi.
> * Creare una macchina virtuale che attivi il runbook.

> [!div class="nextstepaction"]
> [Creare e instradare eventi personalizzati con Griglia di eventi](../event-grid/custom-event-quickstart.md)

---
title: Attiva il mio ruolo di Azure AD in PIM - Azure Active Directory | Microsoft Docs
description: Informazioni su come attivare i ruoli di Azure AD in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 03/05/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca069da1239a505b3e3686998cd29844ed80ba46
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2019
ms.locfileid: "58576813"
---
# <a name="activate-my-azure-ad-roles-in-pim"></a>Attiva il mio ruolo di Azure AD in PIM

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) semplifica la gestione aziendale dell'accesso con privilegi alle risorse in Azure AD e in altri servizi online Microsoft, ad esempio Office 365 o Microsoft Intune.  

Se un ruolo amministrativo è stato impostato come idoneo, è possibile attivare il ruolo quando è necessario eseguire delle azioni con privilegi. Se ad esempio si gestiscono occasionalmente le funzionalità di Office 365, è possibile che gli amministratori dei ruoli con privilegi dell'organizzazione non configurino l'utente come amministratore globale permanente perché tale ruolo interessa anche altri servizi. L'utente è invece considerato idoneo per ruoli di Azure AD, ad esempio amministratore di Exchange Online. È possibile richiedere l'attivazione del ruolo quando tali privilegi risulteranno necessari; si avrà il controllo amministrativo per un periodo di tempo predeterminato.

Questo articolo è destinato agli amministratori che devono attivare il proprio ruolo di Azure AD in PIM.

## <a name="activate-a-role"></a>Attivare un ruolo

Quando è necessario usare un ruolo di Azure AD, è possibile richiedere l'attivazione tramite il **ruoli personali** opzione di navigazione in PIM.

1. Accedere al [portale di Azure](https://portal.azure.com/).

1. Aprire **Azure AD Privileged Identity Management**. Per informazioni su come aggiungere il riquadro PIM alla dashboard, vedere [Iniziare a usare PIM](pim-getting-started.md).

1. Fare clic su **Ruoli di Azure AD**.

1. Fare clic su **ruoli personali** per visualizzare un elenco di idonei per i ruoli di Azure AD.

    ![Ruoli di Azure AD - ruoli personali](./media/pim-how-to-activate-role/directory-roles-my-roles.png)

1. Individuare un ruolo che si desidera attivare.

    ![Ruoli di Azure AD - mio elenco di ruoli](./media/pim-how-to-activate-role/directory-roles-my-roles-activate.png)

1. Fare clic su **Attiva** per aprire il riquadro dei dettagli di attivazione del ruolo.

1. Se il ruolo richiede l'autenticazione a più fattori (MFA), fare clic su **Verificare la propria identità prima di procedere**. È sufficiente eseguire l'autenticazione una volta per sessione.

    ![Verifica con MFA prima dell'attivazione del ruolo](./media/pim-how-to-activate-role/directory-roles-my-roles-mfa.png)

1. Fare clic su **Verifica la mia identità** e seguire le istruzioni per fornire la verifica aggiuntiva di sicurezza.

    ![Verifica di sicurezza aggiuntiva](./media/pim-how-to-activate-role/additional-security-verification.png)

1. Fare clic su **Attiva** per aprire il riquadro di attivazione.

    ![Riquadro di attivazione](./media/pim-how-to-activate-role/directory-roles-activate.png)

1. Se necessario, specificare un'ora di inizio di attivazione personalizzata.

1. Specificare la durata dell'attivazione.

1. Nella casella **Motivo dell'attivazione**, immettere il motivo della richiesta di attivazione. Alcuni ruoli richiedono di specificare un numero di ticket.

    ![Riquadro di attivazione completato](./media/pim-how-to-activate-role/directory-roles-activation-pane.png)

1. Fare clic su **Attiva**.

    Se il ruolo non richiede l'approvazione, un' **lo stato di attivazione** viene visualizzato il riquadro che visualizza lo stato di attivazione.

    ![Stato di attivazione](./media/pim-how-to-activate-role/activation-status.png)

    Dopo aver complete tutte le fasi, scegliere il **disconnettersi** collegamento disconnettersi dal portale di Azure. Quando si accede nuovamente al portale, è ora possibile usare il ruolo.

    Se il [ruolo richiede l'approvazione](./azure-ad-pim-approval-workflow.md) per l'attivazione, nell'angolo superiore destro del browser verrà visualizzata una notifica che informa che la richiesta è in attesa di approvazione.

    ![Notifica di richiesta in attesa](./media/pim-how-to-activate-role/directory-roles-activate-notification.png)

## <a name="view-the-status-of-your-requests"></a>Visualizzare lo stato della richiesta da attivare

È possibile visualizzare lo stato delle richieste in attesa da attivare.

1. Aprire Azure AD Privileged Identity Management.

1. Fare clic su **Ruoli di Azure AD**.

1. Fare clic su **Richieste personali** per visualizzare un elenco delle richieste.

    ![Ruoli di Azure AD - richieste personali](./media/pim-how-to-activate-role/directory-roles-my-requests.png)

## <a name="deactivate-a-role"></a>Disattivare un ruolo

Un ruolo attivato si disattiva automaticamente quando viene raggiunto il limite di tempo (durata idonea).

Se si completano le attività dell'amministratore prima del raggiungimento del limite di tempo, è possibile disattivare un ruolo manualmente in Azure AD Privileged Identity Management.

1. Aprire Azure AD Privileged Identity Management.

1. Fare clic su **Ruoli di Azure AD**.

1. Fare clic su **Ruoli personali**.

1. Fare clic su **Ruoli attivi** per visualizzare l'elenco di ruoli attivi.

1. Individuare il ruolo da non usare ulteriormente e quindi fare clic su **Disattiva**.

## <a name="cancel-a-pending-request"></a>Annullare una richiesta in sospeso

Nel caso in cui non è richiesta l'attivazione di un ruolo che richiede l'approvazione, è possibile annullare una richiesta in sospeso in qualsiasi momento.

1. Aprire Azure AD Privileged Identity Management.

1. Fare clic su **Ruoli di Azure AD**.

1. Fare clic su **Richieste personali**.

1. Per il ruolo che si vuole annullare, scegliere il pulsante **Annulla**.

    Quando si sceglie Annulla, la richiesta verrà annullata. Per attivare nuovamente il ruolo, è necessario inviare una nuova richiesta per l'attivazione.

   ![Annullare una richiesta in sospeso](./media/pim-how-to-activate-role/directory-role-cancel.png)

## <a name="troubleshoot"></a>Risoluzione dei problemi

### <a name="permissions-not-granted-after-activating-a-role"></a>Autorizzazioni non concesse dopo l'attivazione di un ruolo

Quando si attiva un ruolo in PIM, sono necessari almeno 10 minuti prima di poter accedere al portale di amministrazione desiderato o eseguire funzioni all'interno di un carico di lavoro amministrativo specifico. Una volta completata l'attivazione, disconnettersi dal portale Azure e accedere nuovamente per iniziare a usare il ruolo appena attivato.

Per altri passaggi per la risoluzione dei problemi, vedere [Troubleshooting Elevated Permissions](https://social.technet.microsoft.com/wiki/contents/articles/37568.troubleshooting-elevated-permissions-with-azure-ad-privileged-identity-management.aspx) (Risoluzione dei problemi relativi alle autorizzazioni elevate).

## <a name="next-steps"></a>Passaggi successivi

- [Attivare i ruoli di risorse di Azure in PIM](pim-resource-roles-activate-your-roles.md)

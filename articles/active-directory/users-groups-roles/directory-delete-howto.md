---
title: Eliminare una directory di tenant - Azure Active Directory | Microsoft Docs
description: Spiega come preparare una directory di tenant di Azure AD per l'eliminazione
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 833c2e460ae306a7673e580aaa304be93c3cd044
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58199733"
---
# <a name="delete-an-azure-active-directory-tenant"></a>Eliminare un tenant di Azure Active Directory

Quando viene eliminato un tenant, vengono eliminate anche tutte le risorse in esso contenute. È necessario preparare il tenant, riducendo al minimo le risorse ad esso associate prima di eliminarlo. Solo un amministratore globale di Azure Active Directory (Azure AD) può eliminare un tenant di Azure AD dal portale.

## <a name="prepare-the-tenant-for-deletion"></a>Preparare il tenant per l'eliminazione

Non è possibile eliminare un tenant di Azure AD fino a quando non supera diversi controlli. In questo modo si riduce il rischio che l'eliminazione di un tenant possa incidere negativamente sull'accesso degli utenti, ad esempio sulla possibilità di accedere a Office 365 o alle risorse in Azure. Se il tenant associato a una sottoscrizione viene eliminato accidentalmente, ad esempio, gli utenti non potranno accedere alle risorse di Azure per tale sottoscrizione. Di seguito sono illustrate le condizioni che vengono controllate:

* Non può esistere alcun utente nel tenant, ad eccezione di un amministratore globale che lo eliminerà. Tutti gli altri utenti devono essere eliminati prima che possa essere eliminato il tenant. Se gli utenti vengono sincronizzati dall'ambiente locale, è necessario disattivare la sincronizzazione ed eliminare gli utenti nel tenant cloud con il portale di Azure o i cmdlet di Azure PowerShell. 
* Il tenant non può contenere applicazioni. Tutte le applicazioni devono essere rimosse prima che possa essere eliminato il tenant.
* Al tenant possono essere collegati provider di Multi-Factor Authentication.
* Al tenant non possono essere associate sottoscrizioni per i Microsoft Online Services, ad esempio Microsoft Azure, Office 365 o Azure AD Premium. Se, ad esempio, in Azure è stata creata un tenant predefinito, non è possibile eliminare il tenant se la propria sottoscrizione di Azure si basa ancora su di esso per l'autenticazione. Analogamente, non è possibile eliminare un tenant se la sottoscrizione di un altro utente è associata ad esso. 

## <a name="delete-an-azure-ad-tenant"></a>Come eliminare un tenant di Azure Active Directory

1. Accedi per il [interfaccia di amministrazione di Azure AD](https://aad.portal.azure.com) con un account amministratore globale per il tenant.

2. Selezionare **Azure Active Directory**.

3. Passare all'organizzazione che si desidera eliminare.
  
   ![Conferma dell'organizzazione prima dell'eliminazione](./media/directory-delete-howto/delete-directory-command.png)

4. Selezionare **Elimina directory**.
  
   ![Selezionare il comando per eliminare l'organizzazione](./media/directory-delete-howto/delete-directory-list.png)

5. Se il tenant non supera uno o più controlli, viene fornito un collegamento per ottenere altre informazioni su come passare le verifiche. Dopo aver superato tutti i controlli, selezionare **Elimina** per completare il processo.

## <a name="i-have-an-expired-subscription-but-i-cant-delete-the-tenant"></a>Ho una sottoscrizione scaduta, ma non riesco a eliminare il tenant

Quando è stato configurato il tenant di Azure AD, è per l'organizzazione, ad esempio Azure AD Premium P2, Office 365 Business Premium o Enterprise Mobility + Security E5 possono avere attivato anche le sottoscrizioni basate su licenza. Per evitare la perdita accidentale di dati, queste sottoscrizioni impediscono l'eliminazione della directory finché non sono completamente rimosse. Le sottoscrizioni devono avere **Deprovisioning eseguito** come stato per consentire l'eliminazione del tenant. Una sottoscrizione **Scaduta** o **Annullata** passa allo stato **Disattivata** e il passaggio finale è lo stato **Deprovisioning eseguito**. 

Per sapere come comportarsi quando una sottoscrizione della versione di prova gratuita di Office 365 scade (esclusi Partner/CSP a pagamento, Enterprise Agreement o Contratti multilicenza), vedere la tabella seguente. Per altre informazioni sulla conservazione dei dati di Office 365 dati e il ciclo di vita della sottoscrizione, vedere [Cosa accade ai dati e all'accesso quando la sottoscrizione aziendale a Office 365 termina?](https://support.office.com/article/what-happens-to-my-data-and-access-when-my-office-365-for-business-subscription-ends-4436582f-211a-45ec-b72e-33647f97d8a3). 

Stato sottoscrizione | Dati | Accesso ai dati
----- | ----- | -----
Attivo (30 giorni per la versione di prova gratuita)  | Dati accessibili a tutti    | <li>Gli utenti hanno accesso normale ai file di Office 365 o alle app<li>Gli amministratori hanno accesso normale interfaccia di amministrazione di Microsoft 365 e alle risorse 
Scaduto (30 giorni)   | Dati accessibili a tutti    | <li>Gli utenti hanno accesso normale ai file di Office 365 o alle app<li>Gli amministratori hanno accesso normale interfaccia di amministrazione di Microsoft 365 e alle risorse
Disattivato (30 giorni) | Dati accessibili solo all'amministratore  | <li>Gli utenti non possono accedere ai file di Office 365 o alle app<li>Gli amministratori possono accedere al centro di amministrazione Microsoft 365, ma non è possibile assegnare le licenze o aggiornare gli utenti
Deprovisioning eseguito (30 giorni dopo la disattivazione) | Dati eliminati (eliminati automaticamente se nessun altro servizio è in funzione) | <li>Gli utenti non possono accedere ai file di Office 365 o alle app<li>Gli amministratori possono accedere l'interfaccia di amministrazione di Microsoft 365 per acquistare e gestire altre sottoscrizioni 

## <a name="delete-a-subscription-in-the-microsoft-365-admin-center"></a>Eliminare una sottoscrizione nel centro di amministrazione di Microsoft 365

È possibile inserire una sottoscrizione in un **Deprovisoned** lo stato deve essere eliminato dopo 3 giorni tramite l'interfaccia di amministrazione di Microsoft 365.

1. Accedi per il [interfaccia di amministrazione di Microsoft 365](https://admin.microsoft.com) con un account che sia un amministratore globale nel tenant. Se si sta tentando di eliminare il tenant "Contoso" che presenta contoso.onmicrosoft.com come dominio predefinito iniziale, accedere con un nome UPN, ad esempio admin@contoso.onmicrosoft.com.

2. Andare alla **fatturazione** scheda e selezionare **prodotti e servizi**, quindi scegliere la sottoscrizione da annullare. Dopo aver fatto clic su **Annulla**, aggiornare la pagina.
  
   ![Eliminare il collegamento per l'eliminazione della sottoscrizione](./media/directory-delete-howto/delete-command.png)
  
3. Selezionare **Elimina** per eliminare la sottoscrizione e accettare i termini e le condizioni. Tutti i dati verranno definitivamente eliminati entro tre giorni. Se si cambia idea, è possibile riattivare la sottoscrizione nel corso dei tre giorni.
  
   ![leggere attentamente le condizioni](./media/directory-delete-howto/delete-terms.png)

4. Una volta modificato lo stato, la sottoscrizione è contrassegnata per l'eliminazione. La sottoscrizione passa allo stato **Deprovisioning eseguito** 72 ore dopo.

5. Trascorse 72 ore dall'eliminazione della sottoscrizione nel tenant, è possibile effettuare nuovamente l'accesso nell'interfaccia di amministrazione di Azure AD e non sarà più necessaria alcuna azione né vi saranno sottoscrizioni a impedire la procedura di eliminazione del tenant. Dovrebbe essere possibile eliminare il tenant di Azure AD.
  
   ![superare il controllo della sottoscrizione nella schermata di eliminazione](./media/directory-delete-howto/delete-checks-passed.png)

## <a name="next-steps"></a>Passaggi successivi

[Documentazione di Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)

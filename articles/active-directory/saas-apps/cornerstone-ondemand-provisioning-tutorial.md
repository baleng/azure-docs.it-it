---
title: 'Esercitazione: Configurare Cornerstone OnDemand per il provisioning utenti automatico con Azure Active Directory | Microsoft Docs'
description: Informazioni su come configurare Azure Active Directory per effettuare automaticamente il provisioning e il deprovisioning degli account utente in Cornerstone OnDemand.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d17a3c81784d56c6fcad7c7608559abf732882a
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59274094"
---
# <a name="tutorial-configure-cornerstone-ondemand-for-automatic-user-provisioning"></a>Esercitazione: Configurare Cornerstone OnDemand per il provisioning utenti automatico

Questa esercitazione descrive la procedura da eseguire in Cornerstone OnDemand e Azure Active Directory (Azure AD) per configurare Azure AD in modo da eseguire automaticamente il provisioning e il deprovisioning di utenti e/o gruppi in Cornerstone OnDemand.

> [!NOTE]
> L'esercitazione descrive un connettore basato sul servizio di provisioning utenti di Azure AD. Per informazioni dettagliate sul funzionamento di questo servizio e domande frequenti, vedere [Automatizzare il provisioning e il deprovisioning utenti in applicazioni SaaS con Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Prerequisiti

Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga dei prerequisiti seguenti:

* Un tenant di Azure AD
* Tenant di Cornerstone OnDemand
* Un account utente in Cornerstone OnDemand con autorizzazioni di amministratore

> [!NOTE]
> L'integrazione del provisioning di Azure AD si basa sul [servizio Web Cornerstone OnDemand](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_-_Summary_of_Web_Services_v20151106.pdf), disponibile per i team Cornerstone OnDemand.

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a>Aggiunta di Cornerstone OnDemand dalla raccolta

Prima di configurare Cornerstone OnDemand per il provisioning utenti automatico con Azure AD, è necessario aggiungere Cornerstone OnDemand all'elenco delle applicazioni SaaS gestite dalla raccolta di applicazioni di Azure AD.

**Per aggiungere Cornerstone OnDemand dalla raccolta di applicazioni di Azure AD, seguire i passaggi seguenti:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Pulsante Azure Active Directory](common/select-azuread.png)

2. Passare ad **Applicazioni aziendali** e quindi selezionare l'opzione **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali](common/enterprise-applications.png)

3. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione](common/add-new-app.png)

4. Nella casella di ricerca digitare **Cornerstone OnDemand**, selezionare **Cornerstone OnDemand** nel pannello dei risultati e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Cornerstone OnDemand nell'elenco risultati](common/search-new-app.png)

## <a name="assigning-users-to-cornerstone-ondemand"></a>Assegnazione di utenti a Cornerstone OnDemand

Per determinare gli utenti che dovranno ricevere l'accesso alle app selezionate, Azure Active Directory usa il concetto delle "assegnazioni". Nel contesto del provisioning automatico degli utenti, vengono sincronizzati solo gli utenti e/o i gruppi che sono stati "assegnati" a un'applicazione in Azure AD.

Prima di configurazione e abilitare il provisioning utenti automatico, è necessario stabilire quali utenti e/o gruppi in Azure AD devono accedere a Cornerstone OnDemand. Dopo aver stabilito questo, è possibile assegnare tali utenti e/o gruppi a Cornerstone OnDemand seguendo le istruzioni riportate nell'articolo seguente:

* [Assegnare un utente o gruppo a un'app aziendale](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cornerstone-ondemand"></a>Suggerimenti importanti per l'assegnazione di utenti a Cornerstone OnDemand

* È consigliabile assegnare un singolo utente di Azure AD a Cornerstone OnDemand per testare la configurazione del provisioning utenti automatico. È possibile assegnare utenti e/o gruppi aggiuntivi in un secondo momento.

* Quando si assegna un utente a Cornerstone OnDemand, è necessario un ruolo specifico dell'applicazione valido, se disponibile, nella finestra di dialogo di assegnazione. Gli utenti con il ruolo **Accesso predefinito** vengono esclusi dal provisioning.

## <a name="configuring-automatic-user-provisioning-to-cornerstone-ondemand"></a>Configurazione del provisioning utenti automatico per Cornerstone OnDemand

Questa sezione descrive la procedura per configurare il servizio di provisioning di Azure AD in modo da creare, aggiornare e disabilitare utenti e/o gruppi in Cornerstone OnDemand in base alle assegnazioni di utenti e/o gruppi in Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-cornerstone-ondemand-in-azure-ad"></a>Per configurare in provisioning utenti automatico per Cornerstone OnDemand in Azure AD:

1. Accedi per il [portale di Azure](https://portal.azure.com) e selezionare **applicazioni aziendali**, selezionare **tutte le applicazioni**, quindi selezionare **Cornerstone OnDemand**.

    ![Pannello delle applicazioni aziendali](common/enterprise-applications.png)

2. Nell'elenco delle applicazioni selezionare **Cornerstone OnDemand**.

    ![Collegamento a Cornerstone OnDemand nell'elenco delle applicazioni](common/all-applications.png)

3. Selezionare la scheda **Provisioning**.

    ![Provisioning di Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. Impostare **Modalità di provisioning** su **Automatico**.

    ![Provisioning di Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. Nella sezione **Credenziali amministratore** immettere **Nome utente amministratore**, **Password amministratore** e **Dominio** dell'account di Cornerstone OnDemand.

    * Nel campo **Nome utente amministratore** inserire il dominio\nome utente dell'account amministratore nel tenant di Cornerstone OnDemand. Esempio: contoso\admin.

    * Nel campo **Password amministratore** inserire la password corrispondente al nome utente amministratore.

    * Nel campo **Dominio** inserire l'URL del servizio Web del tenant di Cornerstone OnDemand. Esempio: il servizio si trova in `https://ws-[corpname].csod.com/feed30/clientdataservice.asmx`, per Contoso il dominio è `https://ws-contoso.csod.com/feed30/clientdataservice.asmx`. Per altre informazioni su come recuperare l'URL del servizio Web, vedere [qui](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_Web_Services_-_User-OU_Technical_Specification_v20160222.pdf).

6. Dopo aver compilato i campi indicati nel passaggio 5, fare clic su **Test connessione** per verificare che Azure AD possa connettersi a Cornerstone OnDemand. Se la connessione non riesce, verificare che l'account di Cornerstone OnDemand abbia autorizzazioni di amministratore e riprovare.

    ![Provisioning di Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/TestConnection.png)

7. Nel campo **Messaggio di posta elettronica di notifica** immettere l'indirizzo di posta elettronica di una persona o un gruppo che riceverà le notifiche di errore relative al provisioning e selezionare la casella di controllo **Invia una notifica di posta elettronica in caso di errore**.

    ![Provisioning di Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/EmailNotification.png)

8. Fare clic su **Save**.

9. Nella sezione **Mapping** selezionare **Synchronize Azure Active Directory Users to Cornerstone OnDemand** (Sincronizza utenti di Azure Active Directory in Cornerstone OnDemand).

    ![Provisioning di Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/UserMapping.png)

10. Nella sezione **Mapping degli attributi** esaminare gli attributi utente che vengono sincronizzati da Azure AD a Cornerstone OnDemand. Gli attributi selezionati come proprietà **corrispondenti** vengono usati per trovare le corrispondenze con gli account utente in Cornerstone OnDemand per le operazioni di aggiornamento. Selezionare il pulsante **Salva** per eseguire il commit delle modifiche.

    ![Provisioning di Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/UserMappingAttributes.png)

11. Per configurare i filtri di ambito, fare riferimento alle istruzioni fornite nell'[esercitazione sui filtri per la definizione dell'ambito](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

12. Per abilitare il servizio di provisioning di Azure AD per Cornerstone OnDemand, impostare lo **Stato del provisioning** su **Sì** nella sezione **Impostazioni**.

    ![Provisioning di Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningStatus.png)

13. Definire gli utenti e/o gruppi di cui si vuole eseguire il provisioning in Cornerstone OnDemand selezionando i valori desiderati in **Ambito** nella sezione **Impostazioni**.

    ![Provisioning di Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/SyncScope.png)

14. Quando si è pronti per eseguire il provisioning, fare clic su **Salva**.

    ![Provisioning di Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/Save.png)

L'operazione avvia la sincronizzazione iniziale di tutti gli utenti e/o i gruppi definiti in **Ambito** nella sezione **Impostazioni**. La sincronizzazione iniziale richiede più tempo delle sincronizzazioni successive, che saranno eseguite circa ogni 40 minuti quando il servizio di provisioning di Azure AD è in esecuzione. È possibile usare la sezione **Dettagli sincronizzazione** per monitorare lo stato di avanzamento e selezionare i collegamenti ai report delle attività di provisioning, che descrivono tutte le azioni eseguite dal servizio di provisioning di Azure AD in Cornerstone OnDemand.

Per altre informazioni sulla lettura dei log di provisioning di Azure AD, vedere l'esercitazione relativa alla [creazione di report sul provisioning automatico degli account utente](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitazioni dei connettori

* L'attributo **Position** di Cornerstone OnDemand prevede un valore che corrisponde ai ruoli nel portale di Cornerstone OnDemand. L'elenco dei valori validi per **Position** possono essere ottenuti passando a **Edit User Record > Organization Structure > Position** (Modifica record utente > Struttura organizzativa > Posizione) nel portale di Cornerstone OnDemand.

    ![Utente modifica provisioning di Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/UserEdit.png) ![Posizione provisioning di Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/UserPosition.png) ![Elenco posizioni provisioning di Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/PostionId.png)

## <a name="additional-resources"></a>Risorse aggiuntive

* [Gestione di provisioning degli account utente per le app aziendali](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Passaggi successivi

* [Informazioni su come esaminare i log e ottenere report sulle attività di provisioning](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_03.png

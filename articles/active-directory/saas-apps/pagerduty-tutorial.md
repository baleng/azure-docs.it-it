---
title: 'Esercitazione: Integrazione di Azure Active Directory con PagerDuty | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e PagerDuty.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 5470c13f75d010634f97e87dc1a870a100187973
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "57835068"
---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Esercitazione: Integrazione di Azure Active Directory con PagerDuty

Questa esercitazione descrive come integrare PagerDuty con Azure Active Directory (Azure AD).
L'integrazione di PagerDuty con Azure AD offre i vantaggi seguenti:

* È possibile controllare in Azure AD chi può accedere a PagerDuty.
* È possibile abilitare gli utenti di essere automaticamente eseguito l'accesso a PagerDuty (Single Sign-On) con i propri account Azure AD.
* È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se non si ha una sottoscrizione di Azure, [creare un account gratuito](https://azure.microsoft.com/free/) prima di iniziare.

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con PagerDuty, sono necessari gli elementi seguenti:

* Una sottoscrizione di Azure AD. Se non si dispone di un ambiente Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/)
* Sottoscrizione abilitata per PagerDuty single sign-on

## <a name="scenario-description"></a>Descrizione dello scenario

In questa esercitazione vengono eseguiti la configurazione e il test dell'accesso Single Sign-On di Azure AD in un ambiente di test.

* PagerDuty supporta le **SP** SSO avviato dal

## <a name="adding-pagerduty-from-the-gallery"></a>Aggiunta di PagerDuty dalla raccolta

Per configurare l'integrazione di PagerDuty in Azure AD, è necessario aggiungere PagerDuty dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere PagerDuty dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Pulsante Azure Active Directory](common/select-azuread.png)

2. Passare ad **Applicazioni aziendali** e quindi selezionare l'opzione **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali](common/enterprise-applications.png)

3. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione](common/add-new-app.png)

4. Nella casella di ricerca, digitare **PagerDuty**, selezionare **PagerDuty** nel pannello dei risultati quindi fare clic su **Add** pulsante per aggiungere l'applicazione.

     ![PagerDuty nell'elenco risultati](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato single sign-on di Azure con PagerDuty in base a un utente test di nome **Britta Simon**.
Per single sign-on funzioni, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in PagerDuty.

Per configurare e testare l'accesso Single Sign-On di Azure AD con PagerDuty, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)**: per consentire agli utenti di usare questa funzionalità.
2. **[Configura PagerDuty Single Sign-On](#configure-pagerduty-single-sign-on)**  : per configurare le impostazioni di Single Sign-On sul lato applicazione.
3. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
4. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Creare l'utente di test di PagerDuty](#create-pagerduty-test-user)**  : per avere una controparte di Britta Simon in PagerDuty collegata alla rappresentazione in Azure AD dell'utente.
6. **[Testare l'accesso Single Sign-On](#test-single-sign-on)** per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure.

Per configurare single sign-on di Azure con PagerDuty, seguire i passaggi seguenti:

1. Nel [portale di Azure](https://portal.azure.com/)via le **PagerDuty** pagina di integrazione dell'applicazione, seleziona **l'accesso Single sign-on**.

    ![Collegamento Configura accesso Single Sign-On](common/select-sso.png)

2. Nella finestra di dialogo **Selezionare un metodo di accesso Single Sign-On** selezionare la modalità **SAML/WS-Fed** per abilitare il Single Sign-On.

    ![Selezione della modalità Single Sign-On](common/select-saml-option.png)

3. Nella pagina **Configura l'accesso Single Sign-On con SAML** fare clic sull'icona **Modifica** per aprire la finestra di dialogo **Configurazione SAML di base**.

    ![Modificare la configurazione SAML di base](common/edit-urls.png)

4. Nella sezione **Configurazione SAML di base** seguire questa procedura:

    ![Informazioni su URL e dominio per Single Sign-On di PagerDuty](common/sp-identifier.png)

    a. Nella casella di testo **URL di accesso** digitare un URL usando il modello seguente: `https://<tenant-name>.pagerduty.com`

    b. Nella casella di testo **Identificatore (ID entità)** digitare un URL usando il modello seguente: `https://<tenant-name>.pagerduty.com`

    > [!NOTE]
    > Poiché questi non sono i valori reali, è necessario aggiornarli con l'ID e l'URL di accesso effettivi. Per ottenere questi valori, contattare il [team di supporto clienti di PagerDuty](https://www.pagerduty.com/support/). È anche possibile fare riferimento ai modelli mostrati nella sezione **Configurazione SAML di base** del portale di Azure.

5. Nella pagina **Configura l'accesso Single Sign-On con SAML**, nella sezione **Certificato di firma SAML**, fare clic su **Scarica** per scaricare il **Certificato (Base64)** dalle opzioni specificate in base ai propri requisiti e salvarlo nel computer in uso.

    ![Collegamento di download del certificato](common/certificatebase64.png)

6. Nel **configurazione di PagerDuty** sezione, copiare l'URL appropriato in base alle proprie esigenze.

    ![Copiare gli URL di configurazione](common/copy-configuration-urls.png)

    a. URL di accesso

    b. Identificatore di Azure AD

    c. URL di chiusura sessione

### <a name="configure-pagerduty-single-sign-on"></a>Configura PagerDuty Single Sign-On

1. In un'altra finestra del Web browser accedere al sito aziendale di Pagerduty come amministratore.

2. Nel menu in alto fare clic su **Impostazioni account**.

    ![Impostazioni account](./media/pagerduty-tutorial/ic778535.png "Impostazioni account")

3. Fare clic su **Single Sign-On**.

    ![Single Sign-On](./media/pagerduty-tutorial/ic778536.png "Single Sign-On")

4. Nella pagina **Attiva Single Sign-on (SSO)** eseguire la procedura seguente:

    ![Abilitare l'accesso Single Sign-On](./media/pagerduty-tutorial/ic778537.png "Abilitare l'accesso Single Sign-On")

    a. Aprire nel Blocco note il certificato con codifica Base 64 scaricato dal portale di Azure, copiarne il contenuto negli Appunti e incollarlo nella casella di testo **X.509 Certificate** (Certificato X.509).
  
    b. Nella casella di testo **Login URL** (URL di accesso) incollare l'**URL di accesso** copiato dal portale di Azure.
  
    c. Nella casella di testo **Logout URL** (URL di disconnessione) incollare l'**URL di disconnessione** copiato dal portale di Azure.

    d. Selezionare **Allow username/password login** (Consenti accesso tramite password/nome utente).

    e. Selezionare la casella di testo **Require EXACT authentication context comparison** (Richiedi il confronto ESATTO del contesto di autenticazione).

    f. Fare clic su **Salva modifiche**.

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD 

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

1. Nel riquadro sinistro del portale di Azure, selezionare **Azure Active Directory**, **Utenti** e quindi **Tutti gli utenti**.

    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](common/users.png)

2. Selezionare **Nuovo utente** in alto nella schermata.

    ![Pulsante Nuovo utente](common/new-user.png)

3. In Proprietà utente seguire questa procedura.

    ![Finestra di dialogo Utente](common/user-properties.png)

    a. Nel campo **Nome** immettere **BrittaSimon**.
  
    b. Nel campo **Nome utente** digitare **brittasimon@yourcompanydomain.extension**  
    Ad esempio: BrittaSimon@contoso.com

    c. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella Password.

    d. Fare clic su **Create**(Crea).

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a PagerDuty.

1. Nel portale di Azure, selezionare **applicazioni aziendali**, selezionare **tutte le applicazioni**, quindi selezionare **PagerDuty**.

    ![Pannello delle applicazioni aziendali](common/enterprise-applications.png)

2. Nell'elenco di applicazioni selezionare **PagerDuty**.

    ![Collegamento PagerDuty nell'elenco Applicazioni](common/all-applications.png)

3. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"](common/users-groups-blade.png)

4. Fare clic sul pulsante **Aggiungi utente** e quindi selezionare **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione](common/add-assign-user.png)

5. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti e quindi fare clic sul pulsante **Seleziona** in basso nella schermata.

6. Se si prevede un valore di ruolo nell'asserzione SAML, nella finestra di dialogo **Selezionare un ruolo** selezionare il ruolo appropriato per l'utente dall'elenco, quindi fare clic sul pulsante **Seleziona** nella parte inferiore della schermata.

7. Nella finestra di dialogo **Aggiungi assegnazione** fare clic sul pulsante **Assegna**.

### <a name="create-pagerduty-test-user"></a>Creare l'utente di test di PagerDuty

Per consentire agli utenti di Azure AD di accedere a PagerDuty, è necessario eseguirne il provisioning in PagerDuty.  
Nel caso di PagerDuty, il provisioning è un'attività manuale.

>[!NOTE]
>È possibile usare qualsiasi altro strumento di creazione di account utente di Pagerduty o le API fornite da Pagerduty per eseguire il provisioning degli account utente di Azure Active Directory.

**Per eseguire il provisioning di un account utente, seguire questa procedura:**

1. Accedere al tenant **Pagerduty** .

2. Scegliere **Utenti**dal menu in alto.

3. Fare clic su **Aggiungi utenti**.
   
    ![Aggiungere utenti](./media/pagerduty-tutorial/ic778539.png "Aggiungere utenti")

4.  Nella finestra di dialogo **Invite Users** (Invita utenti) seguire questa procedura:
   
    ![Invitare il team](./media/pagerduty-tutorial/ic778540.png "Invitare il team")

    a. Digitare il **nome e il cognome** dell'utente, ad esempio **Britta Simon**. 
   
    b. Immettere **messaggio di posta elettronica** indirizzo dell'utente, ad esempio **brittasimon\@contoso.com**.
   
    c. Fare clic su **Add** (Aggiungi) e quindi su **Send Invites** (Invia inviti).
   
    >[!NOTE]
    >Tutti gli utenti aggiunti riceveranno un invito per creare un account PagerDuty.

### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On 

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro PagerDuty nel Pannello di accesso, dovrebbe automaticamente accedere a di PagerDuty per il quale configurare SSO. Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Risorse aggiuntive

- [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Che cos'è l'accesso condizionale in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)


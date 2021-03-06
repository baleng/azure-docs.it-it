---
title: 'Esercitazione: integrazione di Azure Active Directory con ServiceNow | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e ServiceNow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0efc69aa7fb081303df6fc6327253fd5b8336999
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "57898602"
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Esercitazione: integrazione di Azure Active Directory con ServiceNow

Questa esercitazione descrive come integrare ServiceNow con Azure Active Directory (Azure AD).
L'integrazione di ServiceNow con Azure AD offre i vantaggi seguenti:

* È possibile controllare in Azure AD chi può accedere a ServiceNow.
* È possibile abilitare gli utenti per l'accesso automatico (Single Sign-On) a ServiceNow con gli account Azure AD personali.
* È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se non si ha una sottoscrizione di Azure, [creare un account gratuito](https://azure.microsoft.com/free/) prima di iniziare.

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con ServiceNow, sono necessari gli elementi seguenti:

* Una sottoscrizione di Azure AD. Se non si dispone di un ambiente Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/)
* Sottoscrizione di ServiceNow abilitata per l'accesso Single Sign-On
* Per ServiceNow, un'istanza o un tenant di ServiceNow, versione Calgary o successiva
* Per ServiceNow Express, un'istanza di ServiceNow Express, versione Helsinki o successiva
* Nel tenant di ServiceNow deve essere abilitato il [plug-in Multiple Provider Single Sign On](https://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0). Questa operazione può essere eseguita [inviando una richiesta di servizio](https://hi.service-now.com).
* Per la configurazione automatica, abilitare il plug-in per più provider per ServiceNow.

## <a name="scenario-description"></a>Descrizione dello scenario

In questa esercitazione vengono eseguiti la configurazione e il test dell'accesso Single Sign-On di Azure AD in un ambiente di test.

* ServiceNow supporta l'accesso SSO avviato da **SP**

* ServiceNow supporta il provisioning utenti [**automatico**](servicenow-provisioning-tutorial.md)

## <a name="adding-servicenow-from-the-gallery"></a>Aggiunta di ServiceNow dalla raccolta

Per configurare l'integrazione di ServiceNow in Azure AD, è necessario aggiungere ServiceNow dalla raccolta all'elenco di app SaaS gestite.

**Per aggiungere ServiceNow dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Pulsante Azure Active Directory](common/select-azuread.png)

2. Passare ad **Applicazioni aziendali** e quindi selezionare l'opzione **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali](common/enterprise-applications.png)

3. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione](common/add-new-app.png)

4. Nella casella di ricerca digitare **ServiceNow**, selezionare **ServiceNow** dal pannello dei risultati e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

     ![ServiceNow nell'elenco risultati](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con ServiceNow usando un utente di test di nome **Britta Simon**.
Per il corretto funzionamento dell'accesso Single Sign-On, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in ServiceNow.

Per configurare e testare l'accesso Single Sign-On di Azure AD con ServiceNow, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD per ServiceNow](#configure-azure-ad-single-sign-on-for-servicenow)**: per abilitare gli utenti all'uso di questa funzionalità.
2. **[Configurare l'accesso Single Sign-On di ServiceNow](#configure-servicenow-single-sign-on)**: per configurare le impostazioni di Single Sign-On sul lato applicazione.
3. **[Configurare l'accesso Single Sign-On di Azure AD per ServiceNow Express](#configure-azure-ad-single-sign-on-for-servicenow-express)**: per abilitare gli utenti all'uso di questa funzionalità.
4. **[Configurare l'accesso Single Sign-On di ServiceNow Express](#configure-servicenow-express-single-sign-on)**: per configurare le impostazioni di Single Sign-On sul lato applicazione.
5. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
6. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
7. **[Creare un utente di test di ServiceNow](#create-servicenow-test-user)**: per avere una controparte di Britta Simon in ServiceNow collegata alla rappresentazione dell'utente in Azure AD.
8. **[Testare l'accesso Single Sign-On](#test-single-sign-on)** per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on-for-servicenow"></a>Configurare l'accesso Single Sign-On di Azure AD per ServiceNow

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure.

Per configurare l'accesso Single Sign-On di Azure AD con ServiceNow, seguire questa procedura:

1. Nella pagina di integrazione dell'applicazione **ServiceNow** del [portale di Azure](https://portal.azure.com/) selezionare **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On](common/select-sso.png)

2. Nella finestra di dialogo **Selezionare un metodo di accesso Single Sign-On** selezionare la modalità **SAML/WS-Fed** per abilitare il Single Sign-On.

    ![Selezione della modalità Single Sign-On](common/select-saml-option.png)

3. Nella pagina **Configura l'accesso Single Sign-On con SAML** fare clic sull'icona **Modifica** per aprire la finestra di dialogo **Configurazione SAML di base**.

    ![Modificare la configurazione SAML di base](common/edit-urls.png)

4. Nella sezione **Configurazione SAML di base** seguire questa procedura:

    ![Informazioni su URL e dominio per l'accesso Single Sign-On di ServiceNow](common/sp-identifier.png)

    a. Nella casella di testo **URL di accesso** digitare un URL usando il modello seguente: `https://<instance-name>.service-now.com/navpage.do`

    b. Nella casella di testo **Identificatore (ID entità)** digitare un URL usando il modello seguente: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Poiché questi non sono i valori reali, è necessario aggiornarli in seguito con l'URL di accesso e l'identificatore effettivi. La procedura è descritta più avanti nell'esercitazione. È anche possibile fare riferimento ai modelli mostrati nella sezione **Configurazione SAML di base** del portale di Azure.

5. Nella pagina **Configura l'accesso Single Sign-On con SAML**, nella sezione **Certificato di firma SAML**, fare clic su **Scarica** per scaricare il **Certificato (Base64)** dalle opzioni specificate in base ai propri requisiti e salvarlo nel computer in uso.

    ![Collegamento di download del certificato](common/certificatebase64.png)

    a. Fare clic sul pulsante Copia per copiare l'**URL dei metadati di federazione dell'app** e incollarlo nel Blocco note, in quanto verrà usato più avanti nell'esercitazione.

    b. Fare clic su **Scarica** per scaricare il **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

6. Nella sezione **Configura ServiceNow** copiare gli URL appropriati in base alle proprie esigenze.

    ![Copiare gli URL di configurazione](common/copy-configuration-urls.png)

    a. URL di accesso

    b. Identificatore Azure AD

    c. URL di chiusura sessione

### <a name="configure-servicenow-single-sign-on"></a>Configurare l'accesso Single Sign-On per ServiceNow

1. Accedere all'applicazione ServiceNow come amministratore.

2. Attivare il plug-in **Integration - Multiple Provider Single Sign-On Installer** seguendo questa procedura:

    a. Nel riquadro di spostamento a sinistra cercare la sezione **System Definition** (Definizione sistema) dalla barra di ricerca e quindi fare clic su **Plugins** (Plug-in).

    ![Attivare il plug-in](./media/servicenow-tutorial/tutorial_servicenow_03.png "Attivare il plug-in")

    b. Cercare **Integration - Multiple Provider Single Sign-On Installer**.

     ![Attivare il plug-in](./media/servicenow-tutorial/tutorial_servicenow_04.png "Attivare il plug-in")

    c. Selezionare il plug-in. Fare clic con il pulsante destro del mouse e scegliere **Activate/Upgrade** (Attiva/Aggiorna).

     ![Attivare il plug-in](./media/servicenow-tutorial/tutorial_activate.png "Attivare il plug-in")

    d. Fare clic sul pulsante **Activate** (Attiva).

     ![Attivare il plug-in](./media/servicenow-tutorial/tutorial_activate1.png "Attivare il plug-in")

3. Nel riquadro di spostamento a sinistra cercare la sezione **Multi-Provider SSO** dalla barra di ricerca e quindi fare clic su **Properties** (Proprietà).

    ![Configurare l'URL dell'app](./media/servicenow-tutorial/tutorial_servicenow_06.png "Configurare l'URL dell'app")

4. Nella finestra di dialogo **Multiple Provider SSO Properties** seguire questa procedura:

    ![Configurare l'URL dell'app](./media/servicenow-tutorial/ic7694981.png "Configurare l'URL dell'app")

    * Per **Enable multiple provider SSO** selezionare **Yes**.
  
    * Per **Enable Auto Importing of users from all identity providers into the user table** (Abilita l'importazione automatica di utenti da tutti i provider di identità nella tabella degli utenti), selezionare **Yes** (Sì).

    * Per **Enable debug logging for the multiple provider SSO integration** (Abilita la registrazione debug per l'integrazione SSO per più provider) selezionare **Yes** (Sì).

    * Nella casella di testo **The field on the user table that...** digitare **user_name**.
  
    * Fare clic su **Save**.

5. Esistono due modi per configurare **ServiceNow**: automaticamente e manualmente.

6. Per configurare **ServiceNow** automaticamente, eseguire questa procedura:

    * Tornare alla pagina Single Sign-On **ServiceNow** nel portale di Azure.

    * Per ServiceNow è disponibile un servizio di configurazione con un clic, che consente la configurazione automatica di ServiceNow in Azure AD per l'autenticazione basata su SAML. Per abilitare questo servizio, passare alla sezione **Configurazione di ServiceNow** e fare clic su **Configura ServiceNow** per aprire la finestra Configura accesso.

        ![Configure Single Sign-On](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

    * Immettere il nome dell'istanza di ServiceNow, il nome utente con diritti amministrativi e la password di amministratore nel modulo **Configura accesso** e quindi fare clic su **Configura ora**. Affinché la procedura funzioni, il nome utente con diritti amministrativi fornito deve avere il ruolo **security_admin** assegnato in ServiceNow. In caso contrario, per configurare manualmente ServiceNow in modo da usare Azure AD come provider di identità SAML, fare clic su **Configura manualmente l'accesso Single Sign-On** e copiare **l'URL di disconnessione, l'ID di entità SAML e l'URL del servizio Single Sign-On SAML** dalla sezione di riferimento rapido.

        ![Configurare l'URL dell'app](./media/servicenow-tutorial/configure.png "Configurare l'URL dell'app")

    * Accedere all'applicazione ServiceNow come amministratore.

    * Nella configurazione automatica tutte le impostazioni necessarie sono configurate sul lato **ServiceNow**, ma l'opzione **X.509 Certificate** (Certificato X.509) non è abilitata per impostazione predefinita. È necessario eseguirne manualmente il mapping al provider di identità in ServiceNow. Eseguire queste operazioni:

    * Nel riquadro di spostamento a sinistra cercare la sezione **Multi-Provider SSO** dalla barra di ricerca e quindi fare clic su **Identity Providers** (Provider di identità).

        ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/tutorial_servicenow_07.png "Configurare l'accesso Single Sign-On")

    * Fare clic sul provider di identità generato automaticamente

        ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/tutorial_servicenow_08.png "Configurare l'accesso Single Sign-On")

    *  Nella sezione **Identity Provider** (Provider di identità) seguire questa procedura:

        ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/automatic_config.png "Configurare l'accesso Single Sign-On")

        * Nella casella di testo **Name** (Nome) digitare un nome per la configurazione, ad esempio **Microsoft Azure Federated Single Sign-on**.

        * Rimuovere il valore **SingleLogoutRequest del provider di identità** popolato nella casella di testo.

        * Copiare il valore di **ServiceNow Homepage** (Home page di ServiceNow) e incollarlo nella casella di testo **URL di accesso** nella sezione **URL e dominio ServiceNow** del portale di Azure.

            > [!NOTE]
            > L'URL della home page dell'istanza di ServiceNow è una concatenazione dell'**URL del tenant di ServiceNow** e **/navpage.do**, ad esempio `https://fabrikam.service-now.com/navpage.do`.

        * Copiare il valore di **Entity ID / Issuer** (ID entità/Emittente) e incollarlo nella casella di testo **Identificatore** nella sezione **URL e dominio ServiceNow** del portale di Azure.

        * Assicurarsi che in **NameID Policy** il valore sia impostato su `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`. 

    * Scorrere verso il basso fino alla sezione **X.509 Certificate** (Certificato X.509) e selezionare **Edit** (Modifica).

        ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/tutorial_servicenow_09.png "Configurare l'accesso Single Sign-On")

    * Selezionare il certificato e fare clic sulla freccia a destra per aggiungerlo

        ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/tutorial_servicenow_11.png "Configurare l'accesso Single Sign-On")

    * Fare clic su **Save**.

    * Fare clic su **Connessione di test** nell'angolo in alto a destra della pagina.

        ![Attivare il plug-in](./media/servicenow-tutorial/tutorial_activate2.png "Attivare il plug-in")

    * Dopo aver fatto clic su **Connessione di test**, compare una finestra popup in cui è necessario immettere le credenziali. Verrà quindi visualizzata la pagina con i risultati mostrata di seguito. Ignorare l'eventuale errore **SSO Logout Test Results** e fare clic sul pulsante **Activate**.

        ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/servicenowactivate.png "Configurare l'accesso Single Sign-On")
  
7. Per configurare **ServiceNow** manualmente, eseguire queste operazioni:

    * Accedere all'applicazione ServiceNow come amministratore.

    * Nel riquadro di spostamento a sinistra fare clic su **Identity Providers**.

        ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/tutorial_servicenow_07.png "Configurare l'accesso Single Sign-On")

    * Nella finestra di dialogo **Identity Providers** (Provider di identità) fare clic su **New** (Nuovo).

        ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/ic7694977.png "Configurare l'accesso Single Sign-On")

    * Nella finestra di dialogo **Identity Providers** (Provider di identità) fare clic su **SAML**.

        ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/ic7694978.png "Configurare l'accesso Single Sign-On")

    * Nella sezione **Import Identity Provider Metadata** (Importa i metadati del provider di identità) seguire questa procedura:

        ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/idp.png "Configurare l'accesso Single Sign-On")

        * Immettere l'**URL dei metadati di federazione dell'app** copiato dal portale di Azure.

        * Fare clic su **Importa**.

    * Viene letto l'URL dei metadati IdP e vengono compilate tutte le informazioni dei campi.

        ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/ic7694982.png "Configurare l'accesso Single Sign-On")

        * Nella casella di testo **Name** (Nome) digitare un nome per la configurazione, ad esempio **Microsoft Azure Federated Single Sign-on**.

        * Rimuovere il valore **SingleLogoutRequest del provider di identità** popolato nella casella di testo.

        * Copiare il valore di **ServiceNow Homepage** (Home page di ServiceNow) e incollarlo nella casella di testo **URL di accesso** nella sezione **URL e dominio ServiceNow** del portale di Azure.

            > [!NOTE]
            > L'URL della home page dell'istanza di ServiceNow è una concatenazione dell'**URL del tenant di ServiceNow** e **/navpage.do**, ad esempio `https://fabrikam.service-now.com/navpage.do`.

        * Copiare il valore di **Entity ID / Issuer** (ID entità/Emittente) e incollarlo nella casella di testo **Identificatore** nella sezione **URL e dominio ServiceNow** del portale di Azure.

        * Assicurarsi che in **NameID Policy** il valore sia impostato su `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`.

        * Fare clic su **Advanced**. Nella casella di testo **User Field** (Campo utente) digitare **email** o **user_name**, a seconda di quale campo viene usato per identificare in modo univoco gli utenti nella distribuzione ServiceNow.

            > [!NOTE]
            > Per configurare Azure AD in modo da generare l'ID utente di Azure AD, ovvero il nome dell'entità utente, o l'indirizzo di posta elettronica come identificatore univoco nel token SAML, passare alla sezione **ServiceNow > Attributi > Single Sign-On** del portale di Azure ed eseguire il mapping del campo scelto all'attributo **nameidentifier**. Il valore archiviato per l'attributo selezionato in Azure AD, ad esempio il nome dell'entità utente, deve corrispondere al valore archiviato in ServiceNow per il campo immesso, ad esempio user_name.

        * Fare clic su **Connessione di test** nell'angolo in alto a destra della pagina.

        * Dopo aver fatto clic su **Connessione di test**, compare una finestra popup in cui è necessario immettere le credenziali. Verrà quindi visualizzata la pagina con i risultati mostrata di seguito. Ignorare l'eventuale errore **SSO Logout Test Results** e fare clic sul pulsante **Activate**.

          ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/servicenowactivate.png "Configurare l'accesso Single Sign-On")

### <a name="configure-azure-ad-single-sign-on-for-servicenow-express"></a>Configurare l'accesso Single Sign-On di Azure AD per ServiceNow Express

1. Nella pagina di integrazione dell'applicazione **ServiceNow** del [portale di Azure](https://portal.azure.com/) selezionare **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On](common/select-sso.png)

2. Nella finestra di dialogo **Selezionare un metodo di accesso Single Sign-On** selezionare la modalità **SAML/WS-Fed** per abilitare il Single Sign-On.

    ![Selezione della modalità Single Sign-On](common/select-saml-option.png)

3. Nella pagina **Configura l'accesso Single Sign-On con SAML** fare clic sull'icona **Modifica** per aprire la finestra di dialogo **Configurazione SAML di base**.

    ![Modificare la configurazione SAML di base](common/edit-urls.png)

4. Nella sezione **Configurazione SAML di base** seguire questa procedura:

    ![Informazioni su URL e dominio per l'accesso Single Sign-On di ServiceNow](common/sp-identifier.png)

    a. Nella casella di testo **URL di accesso** digitare un URL usando il modello seguente: `https://<instance-name>.service-now.com/navpage.do`

    b. Nella casella di testo **Identificatore (ID entità)** digitare un URL usando il modello seguente: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Poiché questi non sono i valori reali, è necessario aggiornarli in seguito con l'URL di accesso e l'identificatore effettivi. La procedura è descritta più avanti nell'esercitazione. È anche possibile fare riferimento ai modelli mostrati nella sezione **Configurazione SAML di base** del portale di Azure.

5. Nella pagina **Configura l'accesso Single Sign-On con SAML**, nella sezione **Certificato di firma SAML**, fare clic su **Scarica** per scaricare il **Certificato (Base64)** dalle opzioni specificate in base ai propri requisiti e salvarlo nel computer in uso.

    ![Collegamento di download del certificato](common/certificatebase64.png)

6. Per ServiceNow è disponibile un servizio di configurazione con un clic, che consente la configurazione automatica di ServiceNow in Azure AD per l'autenticazione basata su SAML. Per abilitare questo servizio, passare alla sezione **Set up ServiceNow** (Configura ServiceNow) e fare clic su **Visualizzare le istruzioni dettagliate** per aprire la finestra Configura accesso.

    ![Configure Single Sign-On](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

7. Immettere il nome dell'istanza di ServiceNow, il nome utente con diritti amministrativi e la password di amministratore nel modulo **Configura accesso** e quindi fare clic su **Configura ora**. Affinché la procedura funzioni, il nome utente con diritti amministrativi fornito deve avere il ruolo **security_admin** assegnato in ServiceNow. In caso contrario, per configurare manualmente ServiceNow in modo da usare Azure AD come provider di identità SAML, fare clic su **Configura manualmente l'accesso Single Sign-On** e copiare **l'URL di disconnessione, l'ID di entità SAML e l'URL del servizio Single Sign-On SAML** dalla sezione di riferimento rapido.

    ![Configurare l'URL dell'app](./media/servicenow-tutorial/configure.png "Configurare l'URL dell'app")

### <a name="configure-servicenow-express-single-sign-on"></a>Configurare l'accesso Single Sign-On per ServiceNow Express

1. Accedere all'applicazione ServiceNow Express come amministratore.

2. Nel riquadro di spostamento a sinistra fare clic su **Single Sign-On**.

    ![Configurare l'URL dell'app](./media/servicenow-tutorial/ic7694980ex.png "Configurare l'URL dell'app")

3. Nella finestra di dialogo **Single Sign-On** fare clic sull'icona di configurazione nell'angolo superiore destro e impostare le proprietà seguenti:

    ![Configurare l'URL dell'app](./media/servicenow-tutorial/ic7694981ex.png "Configurare l'URL dell'app")

    a. Spostare **Enable multiple provider SSO** (Abilita SSO per più provider) verso destra.

    b. Spostare **Enable debug logging for the multiple provider SSO integration** (Abilita la registrazione debug per l'integrazione SSO per più provider) verso destra.

    c. Nella casella di testo **The field on the user table that...** digitare **user_name**.

4. Nella finestra di dialogo **Single Sign-On** fare clic su **Add New Certificate** (Aggiungi nuovo certificato).

    ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/ic7694973ex.png "Configurare l'accesso Single Sign-On")

5. Nella finestra di dialogo **X.509 Certificates** seguire questa procedura:

    ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/ic7694975.png "Configurare l'accesso Single Sign-On")

    a. Nella casella di testo **Name**, digitare un nome per la configurazione, ad esempio: **TestSAML2.0**.

    b. Selezionare **Active**.

    c. Per **Format** selezionare **PEM**.

    d. Per **Type** selezionare **Trust Store Cert**.

    e. Aprire nel Blocco note il certificato con codifica Base 64 scaricato dal portale di Azure, copiarne il contenuto negli Appunti e quindi incollarlo nella casella di testo **PEM Certificate** (Certificato PEM).

    f. Fare clic su **Update** (Aggiorna).

6. Nella finestra di dialogo **Single Sign-On** fare clic su **Add New IdP** (Aggiungi nuovo IdP).

    ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/ic7694976ex.png "Configurare l'accesso Single Sign-On")

7. Nella finestra di dialogo **Add New Identity Provider** (Aggiungi nuovo provider di identità) in **Configure Identity Provider** (Configura provider di identità) seguire questa procedura:

    ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/ic7694982ex.png "Configurare l'accesso Single Sign-On")

    a. Nella casella di testo **Name**, digitare un nome per la configurazione, ad esempio: **SAML 2.0**.

    b. Nel campo **Identity Provider URL** (URL provider di identità) incollare il valore di **Identity Provider ID** (ID provider di identità) copiato dal portale di Azure.

    c. Nel campo **Identity Provider's AuthnRequest** (Richiesta di autenticazione provider di identità) incollare il valore di **Authentication Request URL** (URL richiesta di autenticazione) copiato dal portale di Azure.

    d. Nel campo **Identity Provider's SingleLogoutRequest** (Richiesta di disconnessione singola provider di identità) incollare il valore di **URL disconnessione** copiato dal portale di Azure.

    e. Per **Identity Provider Certificate** (Certificato del provider di identità) selezionare il certificato creato nel passaggio precedente.

8. Fare clic su **Advanced Settings** (Impostazioni avanzate). In **Additional Identity Provider Properties** (Proprietà del provider di identità aggiuntivo) seguire questa procedura:

    ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/ic7694983ex.png "Configurare l'accesso Single Sign-On")

    a. Nella casella di testo **Protocol Binding for the IDP's SingleLogoutRequest** digitare **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect**.

    b. Nella casella di testo **NameID Policy** (Criteri NameID) digitare **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified**.

    c. In **AuthnContextClassRef Method** (Metodo AuthnContextClassRef) digitare `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.

    d. Fare clic su **Create an AuthnContextClass**.

9. In **Additional Service Provider Properties** (Proprietà del provider di identità aggiuntivo) seguire questa procedura:

    ![Configurare l'accesso Single Sign-On](./media/servicenow-tutorial/ic7694984ex.png "Configurare l'accesso Single Sign-On")

    a. Nella casella di testo **ServiceNow Homepage** digitare l'URL della home page dell'istanza di ServiceNow.

    > [!NOTE]
    > L'URL della home page dell'istanza di ServiceNow è una concatenazione dell'**URL del tenant di ServiceNow** e **/navpage.do**, ad esempio `https://fabrikam.service-now.com/navpage.do`.

    b. Nella casella di testo **Entity ID / Issuer** digitare l'URL del tenant ServiceNow.

    c. Nella casella di testo **Audience URI** (URI destinatario) digitare l'URL del tenant ServiceNow.

    d. Nella casella di testo **Clock Skew** digitare **60**.

    e. Nella casella di testo **User Field** (Campo utente) digitare **email** o **user_name**, a seconda di quale campo viene usato per identificare in modo univoco gli utenti nella distribuzione ServiceNow.

    > [!NOTE]
    > Per configurare Azure AD in modo da generare l'ID utente di Azure AD, ovvero il nome dell'entità utente, o l'indirizzo di posta elettronica come identificatore univoco nel token SAML, passare alla sezione **ServiceNow > Attributi > Single Sign-On** del portale di Azure ed eseguire il mapping del campo scelto all'attributo **nameidentifier**. Il valore archiviato per l'attributo selezionato in Azure AD, ad esempio il nome dell'entità utente, deve corrispondere al valore archiviato in ServiceNow per il campo immesso, ad esempio user_name.

    f. Fare clic su **Save**.

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

1. Nel riquadro sinistro del portale di Azure, selezionare **Azure Active Directory**, **Utenti** e quindi **Tutti gli utenti**.

    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](common/users.png)

2. Selezionare **Nuovo utente** in alto nella schermata.

    ![Pulsante Nuovo utente](common/new-user.png)

3. In Proprietà utente seguire questa procedura.

    ![Finestra di dialogo Utente](common/user-properties.png)

    a. Nel campo **Nome** immettere **BrittaSimon**.
  
    b. Nel campo **Nome utente**, digitare **brittasimon\@dominioaziendale.estensione**  
    Ad esempio: BrittaSimon@contoso.com

    c. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella Password.

    d. Fare clic su **Create**(Crea).

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a ServiceNow.

1. Nel portale di Azure selezionare **Applicazioni aziendali**, quindi **Tutte le applicazioni** e infine **ServiceNow**.

    ![Pannello delle applicazioni aziendali](common/enterprise-applications.png)

2. Nell'elenco di applicazioni, selezionare **ServiceNow**.

    ![Collegamento di ServiceNow nell'elenco delle applicazioni](common/all-applications.png)

3. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"](common/users-groups-blade.png)

4. Fare clic sul pulsante **Aggiungi utente** e quindi selezionare **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione](common/add-assign-user.png)

5. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti e quindi fare clic sul pulsante **Seleziona** in basso nella schermata.

6. Se si prevede un valore di ruolo nell'asserzione SAML, nella finestra di dialogo **Selezionare un ruolo** selezionare il ruolo appropriato per l'utente dall'elenco, quindi fare clic sul pulsante **Seleziona** nella parte inferiore della schermata.

7. Nella finestra di dialogo **Aggiungi assegnazione** fare clic sul pulsante **Assegna**.

### <a name="create-servicenow-test-user"></a>Creare l'utente di test di ServiceNow

L'obiettivo di questa sezione consiste nel creare un utente chiamato Britta Simon in ServiceNow. ServiceNow supporta il provisioning utenti automatico, che è abilitato per impostazione predefinita. È possibile scoprire più dettagli [qui](servicenow-provisioning-tutorial.md) su come configurare il provisioning utenti automatico.

> [!NOTE]
> Per creare un utente manualmente, è necessario contattare il [Team di supporto di clienti ServiceNow](https://www.servicenow.com/support/contact-support.html).

### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On 

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro di ServiceNow nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione ServiceNow per cui si è configurato l'accesso SSO. Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Risorse aggiuntive

- [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Che cos'è l'accesso condizionale in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Configura provisioning utenti](servicenow-provisioning-tutorial.md)

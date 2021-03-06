---
title: 'Esercitazione: Integrazione di Azure Active Directory con Kantega SSO for Confluence | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Kantega SSO for Confluence.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: d0d99c14-a6ca-45f2-bb84-633126095e7a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 210d69256f2e7f4727ee866af71dd72e765fb0b6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56205767"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-confluence"></a>Esercitazione: Integrazione di Azure Active Directory con Kantega SSO for Confluence

Questa esercitazione descrive come integrare Kantega SSO for Confluence con Azure Active Directory (Azure AD).

L'integrazione di Kantega SSO for Confluence con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Kantega SSO for Confluence
- È possibile abilitare gli utenti per l'accesso automatico a Kantega SSO for Confluence (Single Sign-On) con gli account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Kantega SSO for Confluence, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di Kantega SSO for Confluence abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Kantega SSO for Confluence dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-kantega-sso-for-confluence-from-the-gallery"></a>Aggiunta di Kantega SSO for Confluence dalla raccolta
Per configurare l'integrazione di Kantega SSO for Confluence in Azure AD, è necessario aggiungere Kantega SSO for Confluence dalla raccolta all'elenco di app SaaS gestite.

**Per aggiungere Kantega SSO for Confluence dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![APPLICAZIONI][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![APPLICAZIONI][3]

1. Nella casella di ricerca digitare **Kantega SSO for Confluence**.

    ![Creazione di un utente test di Azure AD](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_search.png)

1. Nel pannello dei risultati selezionare **Kantega SSO for Confluence** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Kantega SSO for Confluence mediante un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve individuare l'utente di Kantega SSO for Confluence corrispondente a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Kantega SSO for Confluence.

Per stabilire la relazione di collegamento, in Kantega SSO for Confluence assegnare il valore di **nome utente** in Azure AD come valore di **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Kantega SSO for Confluence, è necessario completare i blocchi predefiniti seguenti:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'utilizzo di questa funzionalità.
1. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creazione di un utente test di Kantega SSO for Confluence](#creating-a-kantega-sso-for-confluence-test-user)**: per avere una controparte di Britta Simon in Kantega SSO for Confluence collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Kantega SSO for Confluence.

**Per configurare l'accesso Single Sign-On di Azure AD con Kantega SSO for Confluence, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Kantega SSO for Confluence** del portale di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_samlbase.png)

1. In modalità avviata **IDP** nella sezione **URL e dominio Kantega SSO for Confluence** eseguire l'operazione seguente:

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url1.png)

    a. Nella casella di testo **Identificatore** digitare l'URL adottando il modello seguente: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. Nella casella di testo **URL di risposta** digitare l'URL usando il modello seguente: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

1. In modalità avviata **SP** selezionare **Mostra impostazioni URL avanzate** ed eseguire l'operazione seguente:

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url2.png)

    Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE] 
    > Poiché questi non sono i valori reali, è necessario aggiornarli con l'identificatore, l'URL di risposta e l'URL di accesso effettivi. Questi valori vengono ricevuti durante la configurazione del plug-in Confluence descritto più avanti nell'esercitazione.

1. Nella sezione **Certificato di firma SAML** fare clic su **XML di metadati** e quindi salvare il file dei metadati nel computer.

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/tutorial_general_400.png)
    
1. In un'altra finestra del Web browser accedere al **portale di amministrazione di Confluence** come amministratore.

1. Passare il puntatore del mouse sulla rotellina e scegliere **Add-ons** (Componenti aggiuntivi).
    
    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon1.png)

1. Nella scheda **ATLASSIAN MARKETPLACE** (MARKETPLACE DI ATLASSIAN) fare clic su **Find new add-ons** (Trova nuovi componenti aggiuntivi). 

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon.png)

1. Cercare **Kantega SSO for Confluence SAML Kerberos** e fare clic su **Install** (Installa) per installare il nuovo plug-in di SAML.

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon2.png)

1. Viene avviata l'installazione del plug-in.

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon3.png)

1. Al termine dell'installazione, Fare clic su **Close**.

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon33.png)

1.  Fare clic su **Manage**.

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon34.png)
    
1. Fare clic su **Configure** (Configura) per configurare il nuovo plug-in.

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon35.png)

1. Questo nuovo plug-in è disponibile anche nella scheda**USERS & SECURITY** (UTENTI E SICUREZZA).

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon36.png)
    
1. Nella sezione **SAML** Selezionare **Azure Active Directory (Azure AD)** dall'elenco a discesa **Add identity provider** (Aggiungi provider di identità).

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon4.png)

1. Selezionare il livello di sottoscrizione **Basic** (Di base).

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon5.png)     

1. Nella sezione **App properties** (Proprietà app) seguire questa procedura: 

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon6.png)

    a. Copiare il valore **URI ID app** e usarlo come **Identificatore, URL di risposta e URL di accesso** nella sezione **URL e dominio Kantega SSO for Confluence** nel portale di Azure.

    b. Fare clic su **Avanti**.

1. Nella sezione **Metadata import** (Importazione metadati) seguire questa procedura: 

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon7.png)

    a. Selezionare **Metadata file on my computer** (File metadati in questo computer) e caricare il file di metadati scaricato dal portale di Azure.

    b. Fare clic su **Avanti**.

1. Nella sezione **Name and SSO location** (Nome e percorso SSO) seguire questa procedura:

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon8.png)
    
    a. Aggiungere il nome del provider di identità nella casella di testo **Identity provider name** (Nome provider di identità), ad esempio Azure AD.

    b. Fare clic su **Avanti**.

1. Verificare il certificato di firma e fare clic su **Next** (Avanti).

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon9.png)

1. Nella sezione **Confluence user accounts** (Account utente Confluence) seguire questa procedura:

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon10.png)

    a. Selezionare **Create users in Confluence's internal Directory if needed** (Crea utenti nella directory interna di Confluence se necessario) e immettere il nome appropriato del gruppo per gli utenti (è possibile specificare più gruppi separati da virgola).

    b. Fare clic su **Avanti**.

1. Fare clic su **Fine**.    

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon11.png)

1. Nella sezione **Known domains for Azure AD** (Domini noti per Azure AD) seguire questa procedura: 

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/addon12.png)

    a. Selezionare **Known domains** (Domini noti) dal pannello sinistro della pagina.

    b. Immettere il nome di dominio nella casella di testo **Known domains** (Domini noti).

    c. Fare clic su **Save**. 

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili qui: [Documentazione incorporata di Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/kantegassoforconfluence-tutorial/create_aaduser_01.png) 

1. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/kantegassoforconfluence-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/kantegassoforconfluence-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/kantegassoforconfluence-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="creating-a-kantega-sso-for-confluence-test-user"></a>Creazione di un utente test di Kantega SSO for Confluence

Per consentire agli utenti di Azure AD di accedere a Confluence, è necessario effettuarne il provisioning in Confluence. In Kantega SSO for Confluence il provisioning è un'attività manuale.

**Per eseguire il provisioning di un account utente, seguire questa procedura:**

1. Accedere al sito aziendale Kantega SSO for Confluence come amministratore.

1. Passare il puntatore del mouse e fare clic su **User management** (Gestione utenti).

    ![Aggiungere un dipendente](./media/kantegassoforconfluence-tutorial/user1.png) 

1. Nella sezione Users (Utenti) fare clic sula scheda **Add users** (Aggiungi utenti). Nella pagina della finestra di dialogo **"Add a User"** (Aggiungi un utente) eseguire la procedura seguente:

    ![Aggiungere un dipendente](./media/kantegassoforconfluence-tutorial/user2.png) 

    a. Nella casella di testo **Username** (Nome utente) digitare l'indirizzo di posta elettronica di un utente, ad esempio Brittasimon@contoso.com.

    b. Nella casella di testo **Full Name** (Nome completo) digitare il nome completo dell'utente, ad esempio Britta Simon.

    c. Nella casella di testo **Email** digitare l'indirizzo di posta elettronica dell'utente, ad esempio Brittasimon@contoso.com.

    d. Nella casella di testo **Password** digitare la password dell'utente.

    e. Fare clic sul pulsante **Confirm** (Conferma) e immettere di nuovo la password.
    
    f. Fare clic sul pulsante **Aggiungi**.

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Kantega SSO for Confluence.

![Assegna utente][200] 

**Per assegnare Britta Simon a Kantega SSO for Confluence, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco di applicazioni selezionare **Kantega SSO for Confluence**.

    ![Configure Single Sign-On](./media/kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_app.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Kantega SSO for Confluence nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Kantega SSO for Confluence.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/kantegassoforconfluence-tutorial/tutorial_general_01.png
[2]: ./media/kantegassoforconfluence-tutorial/tutorial_general_02.png
[3]: ./media/kantegassoforconfluence-tutorial/tutorial_general_03.png
[4]: ./media/kantegassoforconfluence-tutorial/tutorial_general_04.png

[100]: ./media/kantegassoforconfluence-tutorial/tutorial_general_100.png

[200]: ./media/kantegassoforconfluence-tutorial/tutorial_general_200.png
[201]: ./media/kantegassoforconfluence-tutorial/tutorial_general_201.png
[202]: ./media/kantegassoforconfluence-tutorial/tutorial_general_202.png
[203]: ./media/kantegassoforconfluence-tutorial/tutorial_general_203.png


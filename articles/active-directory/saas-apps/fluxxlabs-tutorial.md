---
title: 'Esercitazione: integrazione di Azure Active Directory con Fluxx Labs | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Fluxx Labs.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d8fac770-bb57-4e1f-b50b-9ffeae239d07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c48e41318ff5ba189e4cc8b8529bb3b81911052
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56204390"
---
# <a name="tutorial-azure-active-directory-integration-with-fluxx-labs"></a>Esercitazione: integrazione di Azure Active Directory con Fluxx Labs

Questa esercitazione descrive come integrare Fluxx Labs con Azure Active Directory (Azure AD).

L'integrazione di Fluxx Labs con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Fluxx Labs.
- È possibile abilitare gli utenti per l'accesso automatico a Fluxx Labs (Single Sign-On) con i propri account Azure AD.
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Fluxx Labs sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di Fluxx Labs abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non è disponibile un ambiente di valutazione di Azure AD, è possibile [ottenere una versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Fluxx Labs dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-fluxx-labs-from-the-gallery"></a>Aggiunta di Fluxx Labs dalla raccolta
Per configurare l'integrazione di Fluxx Labs in Azure AD, è necessario aggiungere Fluxx Labs dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Fluxx Labs dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Pulsante Azure Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali][2]

1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione][3]

1. Nella casella di ricerca digitare **Fluxx Labs**, selezionare **Fluxx Labs** dal pannello dei risultati e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Fluxx Labs nell'elenco dei risultati](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Fluxx Labs usando un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve sapere qual è l'utente controparte di Fluxx Labs che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Fluxx Labs.

Per stabilire la relazione di collegamento, in Fluxx Labs assegnare il valore del **nome utente** di Azure AD al valore dell'attributo **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Fluxx Labs, è necessario completare le operazioni di base seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)**: per consentire agli utenti di usare questa funzionalità.
1. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creare un utente di test di Fluxx Labs](#create-a-fluxx-labs-test-user)**: per avere una controparte di Britta Simon in Fluxx Labs collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testare l'accesso Single Sign-On](#test-single-sign-on)** per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Fluxx Labs.

**Per configurare il Single Sign-On di Azure AD con Fluxx Labs, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Fluxx Labs** nel portale di Azure fare clic su **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.

    ![Finestra di dialogo Single Sign-On](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_samlbase.png)

1. Nella sezione **URL e dominio Fluxx Labs** seguire questa procedura:

    ![Informazioni sull'accesso Single Sign-On per URL e dominio di Fluxx Labs](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_url.png)

    a. Nella casella di testo **Identificatore** digitare un URL usando il criterio seguente:

    | Environment | Modello URL|
    |-------------|------------|
    | Produzione | `https://<subdomain>.fluxx.io` |
    | Preproduzione | `https://<subdomain>.preprod.fluxxlabs.com`|
        
    b. Nella casella di testo **URL di risposta** digitare l'URL usando il modello seguente:

    | Environment | Modello URL|
    |-------------|------------|
    | Produzione | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | Preproduzione | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|

    > [!NOTE]
    > Poiché questi non sono i valori reali, è necessario aggiornarli con l'identificatore e l'URL di risposta effettivi. Per ottenere questi valori, contattare il [team di supporto di Fluxx Labs](mailto:travis@fluxxlabs.com).

1. Nella sezione **Certificato di firma SAML** fare clic su **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

    ![Collegamento di download del certificato](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Pulsante Salva per la configurazione dell'accesso Single Sign-On](./media/fluxxlabs-tutorial/tutorial_general_400.png)

1. Nella sezione **Configurazione di Fluxx Labs** fare clic su **Configura Fluxx Labs** per aprire la finestra **Configura accesso**. Copiare l'**URL servizio Single Sign-On SAML** dalla **sezione Riferimento rapido.**

    ![Configurazione di Fluxx Labs](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_configure.png)

1. In un'altra finestra del Web browser accedere al sito aziendale di Fluxx Labs come amministratore.

1. Selezionare **Amministrazione** sotto la sezione **Impostazioni**.

    ![Configurazione di Fluxx Labs](./media/fluxxlabs-tutorial/config1.png)

1. Nel pannello Amministrazione selezionare **Plug-in** > **Integrazioni** e quindi selezionare **SAML SSO-(Disabilitato)**

    ![Configurazione di Fluxx Labs](./media/fluxxlabs-tutorial/config2.png)

1. Nella sezione attribute seguire questa procedura:

    ![Configurazione di Fluxx Labs](./media/fluxxlabs-tutorial/config3.png)

    a. Selezionare la casella di controllo **SAML SSO** (SSO SAML).

    b. Nella casella di testo **Request Path** (Percorso richiesta) digitare **/auth/saml**.

    c. Nella casella di testo **Callback Path** (Percorso callback) digitare **/auth/saml/callback**.

    d. Nella casella di testo **Assertion Consumer Service URL (Single Sign-On URL)** (URL del servizio consumer di asserzione - URL Single Sign-On) immettere il valore di **URL di risposta** immesso nel portale di Azure.

    e. Nella casella di testo **Audience (SP Entity ID)** (Destinatari - ID entità provider di servizi) immettere il valore di **Identificatore** immesso nel portale di Azure.

    f. Nella casella di testo **Identity Provider SSO Target URL** (URL di destinazione Single Sign-On del provider di identità) incollare il valore di **SAML Single Sign-On Service URL** (URL servizio Single Sign-On SAML) copiato dal portale di Azure.

    g. Aprire il certificato con codifica Base 64 nel Blocco note, copiarne il contenuto negli Appunti e quindi incollarlo nella casella di testo **Certificato provider di identità**.

    h. Nella casella di testo **Formato identificatore nome** immettere il valore `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    i. Fare clic su **Save**.

    > [!NOTE]
    > Una volta salvato il contenuto, il campo appare vuoto per motivi di sicurezza, ma il valore è stato salvato nella configurazione.

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

   ![Creare un utente test di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel portale di Azure fare clic sul pulsante **Azure Active Directory** nel riquadro sinistro.

    ![Pulsante Azure Active Directory](./media/fluxxlabs-tutorial/create_aaduser_01.png)

1. Per visualizzare l'elenco di utenti, passare a **Utenti e gruppi** e quindi fare clic su **Tutti gli utenti**.

    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](./media/fluxxlabs-tutorial/create_aaduser_02.png)

1. Per aprire la finestra di dialogo **Utente** fare clic su **Aggiungi** nella parte superiore della finestra di dialogo **Tutti gli utenti**.

    ![Pulsante Aggiungi](./media/fluxxlabs-tutorial/create_aaduser_03.png)

1. Nella finestra di dialogo **Utente** seguire questa procedura:

    ![Finestra di dialogo Utente](./media/fluxxlabs-tutorial/create_aaduser_04.png)

    a. Nella casella **Nome** digitare **BrittaSimon**.

    b. Nella casella **Nome utente** digitare l'indirizzo di posta elettronica dell'utente Britta Simon.

    c. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella **Password**.

    d. Fare clic su **Create**(Crea).
  
### <a name="create-a-fluxx-labs-test-user"></a>Creare un utente test di Fluxx Labs

Per consentire agli utenti di Azure AD di accedere a Fluxx Labs, è necessario eseguirne il provisioning in Fluxx Labs. Nel caso di Fluxx Labs il provisioning è un'attività manuale.

**Per eseguire il provisioning di un account utente, seguire questa procedura:**

1. Accedere al sito aziendale di Fluxx Labs come amministratore.

1. Fare clic sull'**icona** visualizzata di seguito.

    ![Configurazione di Fluxx Labs](./media/fluxxlabs-tutorial/config6.png)

1. Nel dashboard fare clic sull'icona visualizzata di seguito per aprire la scheda **New PEOPLE** (Nuove PERSONE).

    ![Configurazione di Fluxx Labs](./media/fluxxlabs-tutorial/config4.png)

1. Nella sezione **NEW PEOPLE** (Nuove persone) seguire questa procedura:

    ![Configurazione di Fluxx Labs](./media/fluxxlabs-tutorial/config5.png)

    a. Fluxx Labs usa l'account di posta elettronica come identificatore univoco per l'accesso Single Sign-On. Compilare il campo **SSO UID** (UID SSO) con l'indirizzo di posta elettronica dell'utente, corrispondente all'indirizzo di posta elettronica usato come account di accesso con SSO.

    b. Fare clic su **Save**.

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Fluxx Labs.

![Assegnare il ruolo utente][200]

**Per assegnare Britta Simon a Fluxx Labs, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco delle applicazioni selezionare **Fluxx Labs**.

    ![Collegamento a Fluxx Labs nell'elenco delle applicazioni](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_app.png)  

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"][202]

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.

### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Fluxx Labs nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Fluxx Labs.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/fluxxlabs-tutorial/tutorial_general_01.png
[2]: ./media/fluxxlabs-tutorial/tutorial_general_02.png
[3]: ./media/fluxxlabs-tutorial/tutorial_general_03.png
[4]: ./media/fluxxlabs-tutorial/tutorial_general_04.png

[100]: ./media/fluxxlabs-tutorial/tutorial_general_100.png

[200]: ./media/fluxxlabs-tutorial/tutorial_general_200.png
[201]: ./media/fluxxlabs-tutorial/tutorial_general_201.png
[202]: ./media/fluxxlabs-tutorial/tutorial_general_202.png
[203]: ./media/fluxxlabs-tutorial/tutorial_general_203.png

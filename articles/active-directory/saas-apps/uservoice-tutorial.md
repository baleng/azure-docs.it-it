---
title: 'Esercitazione: Integrazione di Azure Active Directory con UserVoice | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e UserVoice.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 94e077e07796fc111c35b6571459a5e316096ebc
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56206634"
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Esercitazione: Integrazione di Azure Active Directory con UserVoice

Questa esercitazione descrive come integrare UserVoice con Azure Active Directory (Azure AD).

L'integrazione di UserVoice con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a UserVoice.
- È possibile abilitare gli utenti per l'accesso automatico a UserVoice (Single Sign-On) con i propri account Azure AD.
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con UserVoice, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di UserVoice abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non è disponibile un ambiente di valutazione di Azure AD, è possibile [ottenere una versione di valutazione di un mese](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di UserVoice dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-uservoice-from-the-gallery"></a>Aggiunta di UserVoice dalla raccolta
Per configurare l'integrazione di UserVoice in Azure AD, è necessario aggiungere UserVoice dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere UserVoice dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Pulsante Azure Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![Pannello Applicazioni aziendali][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![Pulsante Nuova applicazione][3]

1. Nella casella di ricerca digitare **UserVoice**, selezionare **UserVoice** nel pannello dei risultati e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![UserVoice nell'elenco dei risultati](./media/uservoice-tutorial/tutorial_uservoice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD

In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con UserVoice in base a un utente di test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di UserVoice che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in UserVoice.

Per stabilire la relazione di collegamento, in UserVoice assegnare il valore di **nome utente** in Azure AD come valore di **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con UserVoice, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurare l'accesso Single Sign-On di Azure AD](#configure-azure-ad-single-sign-on)**: per consentire agli utenti di usare questa funzionalità.
1. **[Creare un utente di test di Azure AD](#create-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creare un utente di test di UserVoice](#create-a-uservoice-test-user)**: per avere una controparte di Britta Simon in UserVoice collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnare l'utente test di Azure AD](#assign-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testare l'accesso Single Sign-On](#test-single-sign-on)** per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione UserVoice.

**Per configurare Single Sign-On di Azure AD con UserVoice, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **UserVoice** del portale di Azure fare clic su **Single Sign-On**.

    ![Collegamento Configura accesso Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Finestra di dialogo Single Sign-On](./media/uservoice-tutorial/tutorial_uservoice_samlbase.png)

1. Nella sezione **URL e dominio UserVoice** seguire questa procedura:

    ![Informazioni sull'accesso Single Sign-On per URL e dominio di UserVoice](./media/uservoice-tutorial/tutorial_uservoice_url.png)

    a. Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://<tenantname>.UserVoice.com`

    b. Nella casella di testo **Identificatore** digitare l'URL adottando il modello seguente: `https://<tenantname>.UserVoice.com`

    > [!NOTE] 
    > Poiché questi non sono i valori reali, Aggiornare questi valori con l'identificatore e l'URL di accesso effettivi. Per ottenere questi valori, contattare il [team di supporto clienti di UserVoice](https://www.uservoice.com/).

1. Nella sezione **Certificato di firma SAML** copiare il valore **IDENTIFICAZIONE PERSONALE** del certificato.

    ![Collegamento di download del certificato](./media/uservoice-tutorial/tutorial_uservoice_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Pulsante Salva per la configurazione dell'accesso Single Sign-On](./media/uservoice-tutorial/tutorial_general_400.png)

1. Nella sezione **Configurazione di UserVoice** fare clic su **Configura UserVoice** per aprire la finestra **Configura accesso**. Copiare i valori **Sign-Out URL (URL di disconnessione) e SAML Single Sign-On Service URL (URL servizio Single Sign-On SAML)** dalla sezione **Quick Reference** (Riferimento rapido).

    ![Configurazione di UserVoice](./media/uservoice-tutorial/tutorial_uservoice_configure.png) 

1. In un'altra finestra del browser Web accedere al sito aziendale di UserVoice come amministratore.

1. Sulla barra degli strumenti in alto fare clic su **Impostazioni**, quindi selezionare **Portale Web** nel menu.
   
    ![Sezione Impostazioni sul lato dell'app](./media/uservoice-tutorial/ic777519.png "Impostazioni")

1. Nelle sezione **User authentication** (Autenticazione utente) della scheda **Web portal** (Portale Web) fare clic su **Edit** (Modifica) per aprire la finestra di dialogo **Edit User Authentication** (Modifica autenticazione utente).
   
    ![Scheda Portale Web](./media/uservoice-tutorial/ic777520.png "Portale Web")

1. Nella finestra di dialogo **Edit User Authentication** seguire questa procedura:
   
    ![Modificare l'autenticazione utente](./media/uservoice-tutorial/ic777521.png "Modificare l'autenticazione utente")
   
    a. Fare clic su **Single Sign-On (SSO)**.
 
    b. Incollare il valore dell'**URL del servizio Single Sign-On SAML** copiato dal portale di Azure nella casella di testo **SSO Remote Sign-In** (Accesso remoto SSO).

    c. Incollare il valore dell'**URL di disconnessione** copiato dal portale di Azure nella casella di testo **SSO Remote Sign-Out** (Disconnessione remota SSO).
 
    d. Incollare il valore di **Identificazione personale** , copiato dal portale di Azure, nella casella di testo **Current certificate SHA1 fingerprint**  (Impronta digitale SHA1 del certificato corrente).
    
    e. Fare clic su **Salva le impostazioni di autenticazione**.

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili qui: [Documentazione incorporata di Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD

Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

   ![Creare un utente test di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel portale di Azure fare clic sul pulsante **Azure Active Directory** nel riquadro sinistro.

    ![Pulsante Azure Active Directory](./media/uservoice-tutorial/create_aaduser_01.png)

1. Per visualizzare l'elenco di utenti, passare a **Utenti e gruppi** e quindi fare clic su **Tutti gli utenti**.

    ![Collegamenti "Utenti e gruppi" e "Tutti gli utenti"](./media/uservoice-tutorial/create_aaduser_02.png)

1. Per aprire la finestra di dialogo **Utente** fare clic su **Aggiungi** nella parte superiore della finestra di dialogo **Tutti gli utenti**.

    ![Pulsante Aggiungi](./media/uservoice-tutorial/create_aaduser_03.png)

1. Nella finestra di dialogo **Utente** seguire questa procedura:

    ![Finestra di dialogo Utente](./media/uservoice-tutorial/create_aaduser_04.png)

    a. Nella casella **Nome** digitare **BrittaSimon**.

    b. Nella casella **Nome utente** digitare l'indirizzo di posta elettronica dell'utente Britta Simon.

    c. Selezionare la casella di controllo **Mostra password** e quindi prendere nota del valore visualizzato nella casella **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="create-a-uservoice-test-user"></a>Creare un utente di test per UserVoice

Per consentire agli utenti di Azure AD di accedere a UserVoice, è necessario eseguirne il provisioning in UserVoice. Nel caso di UserVoice, il provisioning è un'attività manuale.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Per eseguire il provisioning di un account utente, seguire questa procedura:
1. Accedere al tenant di **UserVoice** .

1. Passare a **Impostazioni**.
   
    ![Impostazioni](./media/uservoice-tutorial/ic777811.png "Impostazioni")

1. Fare clic su **Generale**.

1. Fare clic su **Agents and permissions**.
   
    ![Agenti e autorizzazioni](./media/uservoice-tutorial/ic777812.png "Agenti e autorizzazioni")

1. Fare clic su **Aggiungi amministratori**.
   
    ![Aggiungere amministratori](./media/uservoice-tutorial/ic777813.png "Aggiungere amministratori")

1. Nella finestra di dialogo **Invita amministratori** seguire questa procedura:
   
    ![Invitare gli amministratori](./media/uservoice-tutorial/ic777814.png "Invitare gli amministratori")
   
    a. Nella casella di testo Emails digitare l'indirizzo di posta elettronica dell'account di cui si vuole eseguire il provisioning e quindi fare clic su **Add**.
   
    b. Fare clic su **Invita**.

> [!NOTE]
> È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da UserVoice per eseguire il provisioning degli account utente Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendo l'accesso a UserVoice.

![Assegnare il ruolo utente][200] 

**Per assegnare Britta Simon a UserVoice, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco delle applicazioni selezionare **UserVoice**.

    ![Collegamento UserVoice nell'elenco delle applicazioni](./media/uservoice-tutorial/tutorial_uservoice_app.png)  

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Collegamento "Utenti e gruppi"][202]

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Riquadro Aggiungi assegnazione][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro UserVoice nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione UserVoice.
Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/uservoice-tutorial/tutorial_general_01.png
[2]: ./media/uservoice-tutorial/tutorial_general_02.png
[3]: ./media/uservoice-tutorial/tutorial_general_03.png
[4]: ./media/uservoice-tutorial/tutorial_general_04.png

[100]: ./media/uservoice-tutorial/tutorial_general_100.png

[200]: ./media/uservoice-tutorial/tutorial_general_200.png
[201]: ./media/uservoice-tutorial/tutorial_general_201.png
[202]: ./media/uservoice-tutorial/tutorial_general_202.png
[203]: ./media/uservoice-tutorial/tutorial_general_203.png


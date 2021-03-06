---
title: 'Esercitazione: Integrazione di Azure Active Directory con StatusPage | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e StatusPage.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: f6ee8bb3-df43-4c0d-bf84-89f18deac4b9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4494996ed54b25be71367dd3e3043023d0958074
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2019
ms.locfileid: "58224040"
---
# <a name="tutorial-azure-active-directory-integration-with-statuspage"></a>Esercitazione: Integrazione di Azure Active Directory con StatusPage

Questa esercitazione descrive come integrare StatusPage con Azure Active Directory (Azure AD).

L'integrazione di StatusPage con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a StatusPage
- È possibile abilitare gli utenti per l'accesso automatico a StatusPage (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con StatusPage, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di StatusPage abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di StatusPage dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-statuspage-from-the-gallery"></a>Aggiunta di StatusPage dalla raccolta
Per configurare l'integrazione di StatusPage in Azure AD, è necessario aggiungere StatusPage dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere StatusPage dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![APPLICAZIONI][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![APPLICAZIONI][3]

1. Nella casella di ricerca digitare **StatusPage**.

    ![Creazione di un utente test di Azure AD](./media/statuspage-tutorial/tutorial_statuspage_search.png)

1. Nel pannello dei risultati selezionare **StatusPage** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/statuspage-tutorial/tutorial_statuspage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con StatusPage in base a un utente di test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di StatusPage che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in StatusPage.

Per stabilire la relazione di collegamento, in StatusPage assegnare il valore di **nome utente** in Azure AD come valore di **Username**.

Per configurare e testare l'accesso Single Sign-On di Azure AD con StatusPage, è necessario completare i blocchi predefiniti seguenti:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'utilizzo di questa funzionalità.
1. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creazione di un utente di test per StatusPage](#creating-a-statuspage-test-user)**: per avere una controparte di Britta Simon in StatusPage collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione StatusPage.

**Per configurare l'accesso Single Sign-On di Azure AD con StatusPage, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **StatusPage** del portale di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configure Single Sign-On](./media/statuspage-tutorial/tutorial_statuspage_samlbase.png)

1. Nella sezione **URL e dominio StatusPage** seguire questa procedura:

    ![Configure Single Sign-On](./media/statuspage-tutorial/tutorial_statuspage_url.png)

    a. Nella casella di testo **Identificatore** digitare l'URL adottando il criterio seguente:

    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/` |
    | `https://<subdomain>.statuspage.io/` |

    b. Nella casella di testo **URL di risposta** digitare l'URL usando il modello seguente: 
    
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/sso/saml/consume` |
    | `https://<subdomain>.statuspage.io/sso/saml/consume` |

   > [!NOTE]
   > Per richiedere i metadati necessari per configurare l'accesso Single Sign-On, contattare il team di supporto di StatusPage all'indirizzo [SupportTeam@statuspage.io](mailto:SupportTeam@statuspage.io). 
   > 
   > a. Dal file di metadati, copiare il valore relativo all'autorità di certificazione e quindi incollarlo nella casella di testo **Identificatore** .
   > 
   > b. Dal file di metadati copiare il valore relativo all'URL di risposta e quindi incollarlo nella casella di testo **Reply URL** .

1. Nella sezione **Certificato di firma SAML** fare clic su **Certificato (Base64)** e quindi salvare il file del certificato nel computer.

    ![Configure Single Sign-On](./media/statuspage-tutorial/tutorial_statuspage_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Configure Single Sign-On](./media/statuspage-tutorial/tutorial_general_400.png)

1. Nella sezione **Configurazione di StatusPage** fare clic su **Configura StatusPage** per aprire la finestra **Configura accesso**. Copiare l'**URL servizio Single Sign-On SAML** dalla **sezione Riferimento rapido.**

    ![Configure Single Sign-On](./media/statuspage-tutorial/tutorial_statuspage_configure.png) 

1. In un'altra finestra del browser accedere al sito aziendale di StatusPage come amministratore.

1. Nella barra degli strumenti principale scegliere **Manage Account**.
   
    ![Configure Single Sign-On](./media/statuspage-tutorial/tutorial_statuspage_06.png) 

1. Fare clic sulla scheda **Single Sign-on** . 
   
    ![Configure Single Sign-On](./media/statuspage-tutorial/tutorial_statuspage_07.png) 

1. Nella pagina di configurazione dell’accesso Single Sign-On, seguire questa procedura:
   
      ![Configure Single Sign-On](./media/statuspage-tutorial/tutorial_statuspage_08.png) 

      ![Configure Single Sign-On](./media/statuspage-tutorial/tutorial_statuspage_09.png) 
 
      a. Nella casella di testo **SSO Target URL** (URL di destinazione SSO) incollare il valore **SAML Single Sign-On Service URL** (URL servizio Single Sign-On SAML) copiato dal portale di Azure.

      b. Aprire il certificato scaricato nel Blocco note, copiarne il contenuto e incollarlo nella casella di testo **Certificato** . 

      c. Fare clic su **SALVA CONFIGURAZIONE**.

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili qui: [Documentazione incorporata di Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/statuspage-tutorial/create_aaduser_01.png) 

1. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/statuspage-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/statuspage-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/statuspage-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="creating-a-statuspage-test-user"></a>Creazione di un utente test per StatusPage

Questa sezione descrive come creare un utente chiamato Britta Simon in StatusPage.

StatusPage supporta il provisioning JIT (just-in-time), È già stato abilitato in [Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-sign-on).

**Per creare un utente denominato Britta Simon in StatusPage, seguire questa procedura:**

1. Accedere al sito aziendale di StatusPage come amministratore.

1. Nel menu in alto fare clic su **Gestisci Account**.

    ![Configure Single Sign-On](./media/statuspage-tutorial/tutorial_statuspage_06.png)

1. Fare clic sulla scheda **Membri del team**. 
   
    ![Creazione di un utente test di Azure AD](./media/statuspage-tutorial/tutorial_statuspage_10.png) 

1. Fare clic su **AGGIUNGI MEMBRO DEL TEAM**. 
   
    ![Creazione di un utente test di Azure AD](./media/statuspage-tutorial/tutorial_statuspage_11.png) 

1. Digitare i valori **Indirizzo e-mail**, **Nome** e **Cognome** di un utente valido di cui si vuole effettuare il provisioning nelle caselle di testo corrispondenti. 
   
    ![Creazione di un utente test di Azure AD](./media/statuspage-tutorial/tutorial_statuspage_12.png) 

1. Per **Ruolo** scegliere **Client Administrator** (Amministratore client).

1. Fare clic su **CREA ACCOUNT**.

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendo l'accesso a StatusPage.

![Assegna utente][200] 

**Per assegnare Britta Simon a StatusPage, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco di applicazioni selezionare **StatusPage**.

    ![Configure Single Sign-On](./media/statuspage-tutorial/tutorial_statuspage_app.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

Questa sezione descrive come testare la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro StatusPage nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione StatusPage.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/statuspage-tutorial/tutorial_general_01.png
[2]: ./media/statuspage-tutorial/tutorial_general_02.png
[3]: ./media/statuspage-tutorial/tutorial_general_03.png
[4]: ./media/statuspage-tutorial/tutorial_general_04.png

[100]: ./media/statuspage-tutorial/tutorial_general_100.png

[200]: ./media/statuspage-tutorial/tutorial_general_200.png
[201]: ./media/statuspage-tutorial/tutorial_general_201.png
[202]: ./media/statuspage-tutorial/tutorial_general_202.png
[203]: ./media/statuspage-tutorial/tutorial_general_203.png


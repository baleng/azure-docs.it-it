---
title: 'Esercitazione: Integrazione di Azure Active Directory con Yardi eLearning | Microsoft Docs'
description: Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Yardi eLearning.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 7ea58b54-ec5b-4576-8586-814b11d0f4fb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ed9a9a7118a79def9d61f17fa5ba22ebe6d4720a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56176527"
---
# <a name="tutorial-azure-active-directory-integration-with-yardi-elearning"></a>Esercitazione: Integrazione di Azure Active Directory con Yardi eLearning

Questa esercitazione descrive come integrare Yardi eLearning con Azure Active Directory (Azure AD).

L'integrazione di Yardi eLearning con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Yardi eLearning
- È possibile abilitare gli utenti per l'accesso automatico a Yardi eLearning (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account in un'unica posizione centrale: il portale di Azure.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Yardi eLearning, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD
- Sottoscrizione di Yardi eLearning abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.

A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede le due fasi fondamentali seguenti:

1. Aggiunta di Yardi eLearning dalla raccolta
1. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-yardi-elearning-from-the-gallery"></a>Aggiunta di Yardi eLearning dalla raccolta
Per configurare l'integrazione di Yardi eLearning in Azure AD, è necessario aggiungere Yardi eLearning dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Yardi eLearning dalla raccolta, seguire questa procedura:**

1. Nel **[portale di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

1. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![APPLICAZIONI][2]
    
1. Fare clic sul pulsante **Nuova applicazione** nella parte superiore della finestra di dialogo per aggiungere una nuova applicazione.

    ![APPLICAZIONI][3]

1. Nella casella di ricerca digitare **Yardi eLearning**.

    ![Creazione di un utente test di Azure AD](./media/yardielearning-tutorial/tutorial_yardielearning_search.png)

1. Nel pannello dei risultati selezionare **Yardi eLearning** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/yardielearning-tutorial/tutorial_yardielearning_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Yardi eLearning usando un utente di test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere l'utente controparte di Yardi eLearning corrispondente a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Yardi eLearning.

Per stabilire la relazione di collegamento, in Yardi eLearning assegnare il valore del **nome utente** in Azure AD come valore di **Username** (Nome utente).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Yardi eLearning, è necessario completare i blocchi predefiniti seguenti:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'utilizzo di questa funzionalità.
1. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
1. **[Creazione di un utente di test di Yardi eLearning](#creating-a-yardi-elearning-test-user)**: per avere una controparte di Britta Simon in Yardi eLearning collegata alla rappresentazione dell'utente in Azure AD.
1. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Yardi eLearning.

**Per configurare l'accesso Single Sign-On di Azure AD con Yardi eLearning, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Yardi eLearning** del portale di Azure fare clic su **Single Sign-On**.

    ![Configure Single Sign-On][4]

1. Nella finestra di dialogo **Single Sign-On** selezionare **Accesso basato su SAML** per **Modalità** per abilitare l'accesso Single Sign-On.
 
    ![Configure Single Sign-On](./media/yardielearning-tutorial/tutorial_yardielearning_samlbase.png)

1. Nella sezione **URL e dominio Yardi eLearning** seguire questa procedura:

    ![Configure Single Sign-On](./media/yardielearning-tutorial/tutorial_yardielearning_url.png)

    a. Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://<companyname>.yardielearning.com/login`

    b. Nella casella di testo **Identificatore** digitare l'URL adottando il modello seguente: `https://<companyname>.yardielearning.com/trust`

    > [!NOTE] 
    > Poiché questi non sono i valori reali, Aggiornare questi valori con l'identificatore e l'URL di accesso effettivi. Per ottenere questi valori, contattare il [team di supporto clienti di Yardi eLearning](mailto:elearning@yardi.com). 
 
1. Nella sezione **Certificato di firma SAML** fare clic su **XML di metadati** e quindi salvare il file dei metadati nel computer.

    ![Configure Single Sign-On](./media/yardielearning-tutorial/tutorial_yardielearning_certificate.png) 

1. Fare clic sul pulsante **Salva** .

    ![Configure Single Sign-On](./media/yardielearning-tutorial/tutorial_general_400.png)

1. Per configurare l'accesso Single Sign-On sul lato **Yardi eLearning**, è necessario inviare il file **XML metadati** scaricato al [team di supporto di Yardi eLearning](mailto:elearning@yardi.com). 

> [!TIP]
> Un riepilogo delle istruzioni è disponibile all'interno del [portale di Azure](https://portal.azure.com) durante la configurazione dell'app.  Dopo aver aggiunto l'app dalla sezione **Active Directory > Applicazioni aziendali** è sufficiente fare clic sulla scheda **Single Sign-On** e accedere alla documentazione incorporata tramite la sezione **Configurazione** nella parte inferiore. Altre informazioni sulla funzione di documentazione incorporata sono disponibili qui: [Documentazione incorporata di Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test denominato Britta Simon nel portale di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/yardielearning-tutorial/create_aaduser_01.png) 

1. Passare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/yardielearning-tutorial/create_aaduser_02.png) 

1. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/yardielearning-tutorial/create_aaduser_03.png) 

1. Nella pagina della finestra di dialogo **Utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/yardielearning-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea).
 
### <a name="creating-a-yardi-elearning-test-user"></a>Creazione di un utente test di Yardi eLearning

Questa sezione descrive come creare un utente chiamato Britta Simon in Yardi eLearning. Yardi eLearning supporta il provisioning just-in-time, che è abilitato per impostazione predefinita.

Non è necessario alcun intervento dell'utente in questa sezione. Durante un tentativo di accesso a Yardi eLearning viene creato un nuovo utente, se questo non esiste già. 

>[!NOTE]
>Per creare un utente manualmente, è necessario contattare il [team di supporto di Yardi eLearning](mailto:elearning@yardi.com).

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Yardi eLearning.

![Assegna utente][200] 

**Per assegnare Britta Simon a Yardi eLearning, seguire questa procedura:**

1. Nel portale di Azure aprire la visualizzazione delle applicazioni e quindi la visualizzazione delle directory e passare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

1. Nell'elenco di applicazioni selezionare **Yardi eLearning**.

    ![Configure Single Sign-On](./media/yardielearning-tutorial/tutorial_yardielearning_app.png) 

1. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

1. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

1. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

1. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

1. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    
### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

Questa sezione descrive come testare la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Yardi eLearning nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Yardi eLearning. 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/yardielearning-tutorial/tutorial_general_01.png
[2]: ./media/yardielearning-tutorial/tutorial_general_02.png
[3]: ./media/yardielearning-tutorial/tutorial_general_03.png
[4]: ./media/yardielearning-tutorial/tutorial_general_04.png

[100]: ./media/yardielearning-tutorial/tutorial_general_100.png

[200]: ./media/yardielearning-tutorial/tutorial_general_200.png
[201]: ./media/yardielearning-tutorial/tutorial_general_201.png
[202]: ./media/yardielearning-tutorial/tutorial_general_202.png
[203]: ./media/yardielearning-tutorial/tutorial_general_203.png


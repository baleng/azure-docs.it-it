---
title: Configurare l'iscrizione e l'accesso con un account GitHub - Azure Active Directory B2C | Microsoft Docs
description: Consentire l'iscrizione e l'accesso ai clienti con account GitHub alle applicazioni da Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: f1c1ac91c08fe27445f4b9631500543d1d0287bd
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56427276"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Configurare l'iscrizione e l'accesso con un account GitHub tramite Azure Active Directory B2C

> [!NOTE]
> Questa funzionalità è in anteprima.
> 

Per usare un account GitHub come [provider di identità](active-directory-b2c-reference-oauth-code.md) in Azure Active Directory (Azure AD) B2C, è necessario creare nel tenant un'applicazione che lo rappresenti. Se non si possiede già un account GitHub, è possibile crearlo all'indirizzo [https://www.github.com/](https://www.github.com/).

## <a name="create-a-github-oauth-application"></a>Creare un'applicazione OAuth per GitHub

1. Accedere al sito Web [GitHub Developer](https://github.com/settings/developers) con le credenziali di GitHub.
2. Selezionare **OAuth Apps** (App OAuth) e **New OAuth App** (Nuova app OAuth).
3. Immettere **Application name** (Nome applicazione) e **Homepage URL** (URL della home page) personale.
4. Immettere il valore `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` in **Authorization callback URL** (URL callback di autorizzazione). Sostituire `your-tenant-name` con il nome del tenant di Azure AD B2C. Usare lettere minuscole quando si immette il nome del tenant, anche se questo viene definito con lettere maiuscole in Azure AD B2C.
5. Fare clic su **Register application**.
6. Copiare i valori **ID client** e **Segreto client**. Entrambi sono necessari per aggiungere il provider di identità nel tenant.

## <a name="configure-a-github-account-as-an-identity-provider"></a>Configurare un account GitHub come provider di identità

1. Accedere al [portale di Azure](https://portal.azure.com/) come amministratore globale del tenant di Azure AD B2C.
2. Assicurarsi di usare la directory che contiene il tenant di Azure AD B2C. A tale scopo, fare clic sul **filtro delle directory e delle sottoscrizioni** nel menu in alto e scegliere la directory che contiene il tenant.
3. Scegliere **Tutti i servizi** nell'angolo in alto a sinistra del portale di Azure, cercare **Azure AD B2C** e selezionarlo.
4. Selezionare **Provider di identità** e quindi selezionare **Aggiungi**.
5. Specificare un **Nome**. Ad esempio, immettere *GitHub*.
6. Selezionare **Tipo di provider di identità**, selezionare **GitHub (anteprima)** e quindi fare clic su **OK**.
7. Selezionare **Configura questo provider di identità**, immettere l'ID client registrato in precedenza in **ID client** e immettere il segreto client registrato come **Segreto client** dell'applicazione dell'account GitHub creata in precedenza.
8. Fare clic su **OK** e quindi su **Crea** per salvare la configurazione dell'account GitHub.

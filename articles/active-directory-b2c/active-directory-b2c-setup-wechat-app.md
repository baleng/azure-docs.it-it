---
title: Configurare l'iscrizione e l'accesso con un account WeChat tramite Azure Active Directory B2C | Microsoft Docs
description: Consentire l'iscrizione e l'accesso ai clienti con account WeChat alle applicazioni da Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 9c9f6b930c5173e97e29148270dedc7eb086ef5a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55159676"
---
# <a name="set-up-sign-up-and-sign-in-with-a-wechat-account-using-azure-active-directory-b2c"></a>Configurare l'iscrizione e l'accesso con un account WeChat tramite Azure Active Directory B2C

> [!NOTE]
> Questa funzionalità è in anteprima.
> 

## <a name="create-a-wechat-application"></a>Creare un'applicazione WeChat

Per usare un account WeChat come provider di identità in Azure Active Directory (Azure AD) B2C, è necessario creare nel tenant un'applicazione che lo rappresenti. Se non si possiede già un account WeChat, è possibile ottenere informazioni sul sito [https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html](https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>Registrare un'applicazione WeChat

1. Accedere a [https://open.weixin.qq.com/](https://open.weixin.qq.com/) con le credenziali di WeChat.
2. Selezionare **管理中心** (centro di gestione).
3. Seguire i passaggi per registrare una nuova applicazione.
4. Immettere `https://your-tenant_name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` per **授权回调域** (URL callback). Ad esempio, se il nome del tenant è contoso, impostare l'URL su `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
5. Copiare l'**ID APP** e la **CHIAVE APP**. necessari per aggiungere il provider di identità nel tenant.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>Configurare WeChat come provider di identità nel tenant,

1. Accedere al [portale di Azure](https://portal.azure.com/) come amministratore globale del tenant di Azure AD B2C.
2. Assicurarsi di usare la directory che contiene il tenant di Azure AD B2C. A tale scopo, fare clic sul **filtro delle directory e delle sottoscrizioni** nel menu in alto e scegliere la directory che contiene il tenant.
3. Scegliere **Tutti i servizi** nell'angolo in alto a sinistra del portale di Azure, cercare **Azure AD B2C** e selezionarlo.
4. Selezionare **Provider di identità** e quindi selezionare **Aggiungi**.
5. Specificare un **Nome**. Ad esempio, immettere *WeChat*.
6. Fare clic su **Tipo di provider di identità**, selezionare **WeChat (anteprima)** e fare clic su **OK**.
7. Selezionare **Set up this identity provider** (Configura provider di identità), immettere l'ID APP registrato in precedenza in **ID Client** e immettere la CHIAVE APP registrata come **Segreto client** dell'applicazione WeChat creata in precedenza.
8. Fare clic su **OK** e su **Crea** per salvare la configurazione di WeChat.


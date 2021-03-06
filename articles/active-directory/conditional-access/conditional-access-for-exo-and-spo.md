---
title: Configurare SharePoint Online ed Exchange Online per l'accesso condizionale di Azure Active Directory | Microsoft Docs
description: Informazioni su configurare SharePoint Online ed Exchange Online per l'accesso condizionale di Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 62349fba-3cc0-4ab5-babe-372b3389eff6
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2019
ms.author: joflore
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: a90cd381dbe3feaad110c7f10ae328915c051d0a
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2019
ms.locfileid: "58517726"
---
# <a name="how-to-set-up-sharepoint-online-and-exchange-online-for-azure-active-directory-conditional-access"></a>Procedura: Configurare SharePoint Online ed Exchange Online per l'accesso condizionale di Azure Active Directory 

Con l'[accesso condizionale di Azure Active Directory (Azure AD)](overview.md) è possibile controllare il modo in cui gli utenti accedono alle app cloud. Se si vuole usare l'accesso condizionale per controllare l'accesso a SharePoint ed Exchange online, è necessario:

- Verificare se lo scenario di accesso condizionale è supportato
- Impedire alle app client di ignorare l'applicazione dei criteri di accesso condizionale   

Questo articolo illustra come affrontare entrambi i casi.


## <a name="what-you-need-to-know"></a>Informazioni importanti

È possibile usare l'accesso condizionale di Azure AD per proteggere le app cloud quando un tentativo di autenticazione proviene da:

- Un Web browser

- Un'app client che usa l'[autenticazione moderna](https://support.office.com/article/Using-Office-365-modern-authentication-with-Office-clients-776c0036-66fd-41cb-8928-5495c0f9168a)

- Exchange ActiveSync 

Alcune app cloud supportano anche i protocolli di autenticazione legacy. Questo vale, ad esempio, per SharePoint Online ed Exchange Online. Quando un'app client può usare un protocollo di autenticazione legacy per accedere a un'app cloud, Azure AD non è in grado di applicare criteri di accesso condizionale a questo tentativo di accesso. Per impedire a un'app client di ignorare l'applicazione di criteri, è necessario controllare se è possibile abilitare solo l'autenticazione moderna nelle app cloud interessate. 

Tra le app client a cui non è applicabile l'accesso condizionale sono incluse le seguenti:

- Office 2010 e versioni precedenti

- Office 2013 quando l'autenticazione moderna non è abilitata



 
## <a name="control-access-to-sharepoint-online"></a>Controllare l'accesso a SharePoint Online

Oltre all'autenticazione moderna, SharePoint Online supporta i protocolli di autenticazione legacy. Se questi protocolli sono abilitati, i criteri di accesso condizionale per SharePoint non vengono applicati per i client che non usano l'autenticazione moderna.

È possibile disabilitare i protocolli di autenticazione legacy per l'accesso a SharePoint usando il cmdlet **[Set-SPOTenant](https://technet.microsoft.com/library/fp161390.aspx)**: 

    Set-SPOTenant -LegacyAuthProtocolsEnabled $false

## <a name="control-access-to-exchange-online"></a>Controllare l'accesso a Exchange Online

Quando si configurano i criteri di accesso condizionale per Exchange Online, è necessario esaminare gli elementi seguenti:

- Exchange ActiveSync

- Protocolli di autenticazione legacy



### <a name="exchange-activesync"></a>Exchange ActiveSync

Exchange Active Sync supporta l'autenticazione moderna, ma esistono alcune limitazioni relative al supporto per gli scenari di accesso condizionale:

- Quando si selezionano **i client di Exchange Active Sync** nel criterio, non è possibile configurare le altre condizioni.  

    ![Piattaforme del dispositivo](./media/conditional-access-for-exo-and-spo/05.png)

- L'impostazione del requisito di autenticazione a più fattori non è supportata.  

    ![Accesso condizionale](./media/conditional-access-for-exo-and-spo/01.png)

Per proteggere in modo efficace l'accesso a Exchange Online da Exchange ActiveSync, è possibile:

- Configurare criteri di accesso condizionale supportato seguendo questa procedura:

    a. Selezionare solo **Office 365 Exchange Online** come app cloud.  

    ![Accesso condizionale](./media/conditional-access-for-exo-and-spo/04.png)

    b. Selezionare **Exchange Active Sync** come **app client**.  

    ![Piattaforme del dispositivo](./media/conditional-access-for-exo-and-spo/03.png)

- Bloccare Exchange ActiveSync usando regole di Active Directory Federation Services (AD FS).

        @RuleName = "Block Exchange ActiveSync"
        c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
        => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "false");




### <a name="legacy-authentication-protocols"></a>Protocolli di autenticazione legacy

Oltre all'autenticazione moderna, Exchange Online supporta i protocolli di autenticazione legacy. Se questi protocolli sono abilitati, i criteri di accesso condizionale per Exchange Online non vengono applicati per i client che non usano l'autenticazione moderna.

È possibile disabilitare i protocolli di autenticazione legacy per Exchange Online impostando regole di AD FS. Ciò consente di bloccare l'accesso da:

- Client di Office meno recenti, ad esempio Office 2013 senza l'autenticazione moderna abilitata 

- Versioni precedenti di Office


## <a name="set-up-ad-fs-rules"></a>Configurare regole di AD FS

È possibile usare le regole di autorizzazione di rilascio seguenti per abilitare o bloccare il traffico a livello di AD FS. 

### <a name="block-legacy-traffic-from-the-extranet"></a>Bloccare il traffico legacy dalla rete Extranet

Applicando le tre regole seguenti: 

- Si abilita l'accesso per:
    - Il traffico di Exchange ActiveSync
    - Il traffico del browser
    - Il traffico dell'autenticazione moderna
- Si blocca l'accesso per: 
    - App client legacy dalla rete Extranet

**Regola 1:**

    @RuleName = "Allow all intranet traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

**Regola 2:**

    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

**Regola 3:**

    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

### <a name="block-legacy-traffic-from-anywhere"></a>Bloccare il traffico legacy da qualsiasi posizione

Applicando le tre regole seguenti:

- Si abilita l'accesso per: 
    - Il traffico di Exchange ActiveSync
    - Il traffico del browser
    - Il traffico dell'autenticazione moderna
- Si blocca l'accesso per: 
    - App legacy da qualsiasi posizione

##### <a name="rule-1"></a>Regola 1
    @RuleName = "Allow all intranet traffic only for browser and modern authentication clients"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Regola 2
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Regola 3
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere [Accesso condizionale in Azure Active Directory](overview.md).

Per istruzioni sulla configurazione di regole di attestazione, vedere [Configurare le regole di attestazione](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-claim-rules). 







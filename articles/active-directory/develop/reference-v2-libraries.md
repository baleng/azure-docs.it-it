---
title: Librerie di autenticazione di Azure Active Directory 2.0 | Microsoft Docs
description: Librerie client e middleware server compatibili e link a librerie, codice sorgente ed esempi correlati per l'endpoint di Azure Active Directory 2.0.
services: active-directory
documentationcenter: ''
author: negoe
manager: mtillman
editor: ''
ms.assetid: 19cec615-e51f-4141-9f8c-aaf38ff9f746
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2018
ms.author: negoe
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: cf1e7c4acca147020612e6d33d0e72afb4be0865
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/26/2019
ms.locfileid: "56869973"
---
# <a name="azure-active-directory-v20-authentication-libraries"></a>Librerie di autenticazione di Azure Active Directory 2.0

L'[endpoint di Azure Active Directory (Azure AD) 2.0](active-directory-v2-compare.md) supporta i protocolli standard del settore OAuth 2.0 e OpenID Connect 1.0. Microsoft Authentication Library (MSAL) è progettata per funzionare con l'endpoint Azure AD versione 2.0. È anche possibile usare le librerie open source che supportano OAuth 2.0 e OpenID Connect 1.0.

È consigliabile usare librerie scritte da esperti del dominio del protocollo che seguono una metodologia Security Development Lifecycle (SDL), come [quella seguita da Microsoft][Microsoft-SDL]. Se si decide di creare manualmente il codice per i protocolli, seguire una metodologia come Microsoft Security Development Lifecycle (SDL) e prestare attenzione alle considerazioni sulla sicurezza contenute nelle specifiche degli standard per ogni protocollo.

> [!NOTE]
> Si è interessati alla libreria v1.0 di Azure AD (ADAL)? Controllare la [Guida alle librerie ADAL](active-directory-authentication-libraries.md).

## <a name="types-of-libraries"></a>Tipi di librerie

L'endpoint di Azure AD v2.0 usa due tipi di librerie:

* **Librerie client**: I server e i client nativi usano le librerie client per ottenere i token di accesso per chiamare una risorsa, ad esempio Microsoft Graph.
* **Librerie middleware server**: Le librerie middleware server vengono usate dalle app Web per l'accesso degli utenti e dalle API Web per convalidare i token inviati da client nativi o altri server.

## <a name="library-support"></a>Supporto per le librerie

Le librerie dispongono di due categorie di supporto:

* **Librerie supportate da Microsoft**: Microsoft offre correzioni per queste librerie e ha applicato la due diligence SDL alle librerie.
* **Librerie compatibili**: Microsoft ha testato queste librerie in scenari di base e ne ha confermato il funzionamento con l'endpoint 2.0. Microsoft non fornisce correzioni per queste librerie e non ha eseguito una verifica su di esse. Le richieste relative a problemi e funzionalità devono essere indirizzate al progetto open source della libreria.

Per un elenco delle librerie che funzionano con l'endpoint 2.0, vedere le sezioni successive di questo articolo.

## <a name="microsoft-supported-client-libraries"></a>Librerie client supportate da Microsoft

Le librerie di autenticazione client vengono usate per acquisire un token per chiamare un'API Web protetta

| Piattaforma | Libreria | Download | Codice sorgente | Esempio | Riferimenti | Documenti di carattere concettuale | Guida di orientamento |
| --- | --- | --- | --- | --- | --- | --- | ---|
| ![JavaScript](media/sample-v2-code/logo_js.png) | MSAL.js (anteprima) | [NPM](https://www.npmjs.com/package/msal) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs/README.md) |  [App a singola pagina](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |  | [wiki](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki)|
|![Angular JS](media/sample-v2-code/logo_angular.png) | MSAL Angular JS | [NPM](https://www.npmjs.com/package/@azure/msal-angularjs) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs/README.md) |  |  | |
![Angular](media/sample-v2-code/logo_angular.png) | MSAL Angular(Preview) | [NPM](https://www.npmjs.com/package/@azure/msal-angular) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | | | |
| ![.NET Framework](media/sample-v2-code/logo_NET.png) ![UWP](media/sample-v2-code/logo_windows.png) ![Xamarin](media/sample-v2-code/logo_xamarin.png) | MSAL .NET (anteprima) |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [App desktop](guidedsetups/active-directory-mobileanddesktopapp-windowsdesktop-intro.md) | [MSAL.NET](https://docs.microsoft.com/dotnet/api/microsoft.identity.client?view=azure-dotnet-preview) |[wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki#conceptual-documentation) | [Roadmap](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki#roadmap)
| ![iOS/Objective C o swift](media/sample-v2-code/logo_iOS.png) | MSAL obj_c (Preview) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [App iOS](https://github.com/Azure-Samples/active-directory-msal-ios-swift) |  |
|![Android/Java](media/sample-v2-code/logo_Android.png) | MSAL (anteprima) | [Repository centrale](https://repo1.maven.org/maven2/com/microsoft/identity/client/msal/) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [App Android](quickstart-v2-android.md) | [JavaDocs](https://javadoc.io/doc/com.microsoft.identity.client/msal) | | |

## <a name="microsoft-supported-server-middleware-libraries"></a>Librerie middleware server supportate da Microsoft

Le librerie middleware vengono usate per proteggere le applicazioni Web e le API Web. Per le app Web o le API Web scritte con ASP.NET o ASP.NET Core, le librerie middleware vengono usate da ASP.NET/ASP.NET Core

| Piattaforma | Libreria | Download | Codice sorgente | Esempio | Riferimenti
| --- | --- | --- | --- | --- | --- |
| ![.NET](media/sample-v2-code/logo_NET.png) ![.NET Core](media/sample-v2-code/logo_NETcore.png) | ASP.NET Security |[NuGet](https://www.nuget.org/packages/Microsoft.AspNet.Mvc/) |[ASP.NET Security (GitHub)](https://github.com/aspnet/Security) |[App MVC](quickstart-v2-aspnet-webapp.md) |[Informazioni di riferimento sulle API REST](https://docs.microsoft.com/dotnet/api/?view=aspnetcore-2.0) |
| ![.NET](media/sample-v2-code/logo_NET.png)| Estensioni IdentityModel per .NET| |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | [App MVC](quickstart-v2-aspnet-webapp.md) |[Riferimento](https://docs.microsoft.com/dotnet/api/overview/azure/activedirectory/client?view=azure-dotnet) |
| ![Node.js](media/sample-v2-code/logo_nodejs.png) | Passport Azure AD |[NPM](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [App Web](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs) | |

## <a name="compatible-client-libraries"></a>Librerie client compatibili

| Piattaforma | Nome libreria | Versione testata | Codice sorgente | Esempio |
|:---:|:---:|:---:|:---:|:---:|
|![JavaScript](media/sample-v2-code/logo_js.png)|[Hello.js](https://adodson.com/hello.js/) |1.13.5 |[Hello.js](https://github.com/MrSwitch/hello.js) |[SPA](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |
| ![Java](media/sample-v2-code/logo_java.png) | [Scribe Java](https://github.com/scribejava/scribejava) | [Versione 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | [ScribeJava](https://github.com/scribejava/scribejava/) | |
| ![PHP](media/sample-v2-code/logo_php.png) | [PHP League oauth2-client](https://github.com/thephpleague/oauth2-client) | [Versione 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | [oauth2-client](https://github.com/thephpleague/oauth2-client/) | |
| ![Ruby](media/sample-v2-code/logo_ruby.png) |[OmniAuth](https://github.com/omniauth/omniauth/wiki) |omniauth:1.3.1<br />omniauth-oauth2:1.4.0 |[OmniAuth](https://github.com/omniauth/omniauth)<br />[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) |  |
![iOS](media/sample-v2-code/logo_iOS.png) |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |1.2.8 |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |[Esempio di app nativa](active-directory-v2-devquickstarts-ios.md) |

Poiché per qualsiasi libreria conforme standard è possibile usare l'endpoint v2.0, è importante sapere a chi rivolgersi per ottenere supporto.

* Per problemi e richieste di nuove funzionalità nel codice della libreria, contattare il proprietario della libreria.
* Per problemi e richieste di nuove funzionalità nell'implementazione del protocollo sul lato servizio, contattare Microsoft.
* [Presentare una richiesta di funzionalità](https://feedback.azure.com/forums/169401-azure-active-directory) per funzionalità aggiuntive che si desidera vedere nel protocollo.
* [Creare una richiesta di supporto](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) se si rileva un problema in cui l'endpoint di Azure AD versione 2.0 non è conforme a OAuth 2.0 o OpenID Connect 1.0.

## <a name="related-content"></a>Contenuti correlati

Per altre informazioni sull'endpoint di Azure AD 2.0, vedere la [panoramica del modello app di Azure AD 2.0][AAD-App-Model-V2-Overview].

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: v2-overview.md
[ClientLib-NET-Lib]: https://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:/
[ClientLib-Iosmac-Lib]:/
[ClientLib-Iosmac-Repo]:/
[ClientLib-Iosmac-Sample]:/
[ClientLib-Android-Lib]:/
[ClientLib-Android-Repo]:/
[ClientLib-Android-Sample]:/
[ClientLib-Js-Lib]:/
[ClientLib-Js-Repo]:/
[ClientLib-Js-Sample]:/

[Microsoft-SDL]: https://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: https://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: https://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/documentation/articles/active-directory-v2-devquickstarts-node-web/

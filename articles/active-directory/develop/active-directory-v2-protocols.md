---
title: Informazioni sui protocolli di autorizzazione supportati da Azure AD 2.0 | Documentazione Microsoft
description: Guida ai protocolli supportati dall'endpoint v 2.0 di Azure AD.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5fb4fa1b-8fc4-438e-b3b0-258d8c145f22
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c56970091da74cfc389d60ad91f430fcb64d4bba
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59266971"
---
# <a name="v20-protocols---oauth-20-and-openid-connect"></a>Protocolli v2.0 - OAuth 2.0 e OpenID Connect

L'endpoint v2.0 può usare Azure Active Directory (Azure AD) per il servizio IaaS (identity-as-a-service) con protocolli standard del settore, OpenID Connect e OAuth 2.0. Anche se il servizio è conforme agli standard, possono esistere sottili differenze tra le implementazioni di questi protocolli. Le informazioni in questo argomento sono utili se si sceglie di scrivere il codice inviando e gestendo direttamente le richieste HTTP o di usare una libreria open source di terze parti anziché usare una [libreria open source](reference-v2-libraries.md) di Microsoft.

> [!NOTE]
> Non tutti gli scenari e le funzionalità di Azure AD sono supportati dall'endpoint 2.0. Per determinare se è necessario usare l'endpoint v2.0, leggere le informazioni sulle [limitazioni v2.0](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Nozioni di base

In quasi tutti i flussi di OAuth 2.0 e OpenID Connect, sono coinvolte nello scambio quattro parti:

![Ruoli di OAuth 2.0](../../media/active-directory-v2-flows/protocols_roles.png)

* Il **server di autorizzazione** è l’endpoint v2.0 e garantisce l'identità dell'utente, concede e revoca l'accesso alle risorse e rilascia i token. Il server di autorizzazione agisce anche come provider di identità: gestisce in modo sicuro le informazioni dell'utente, l'accesso e le relazioni di trust tra le parti di un flusso.
* Il **proprietario della risorsa** è in genere l'utente finale. È l'entità che possiede i dati e consente a terze parti di accedere a tali dati o risorse.
* Il **client OAuth** è l'app, identificata dall'ID applicazione. In genere il client OAuth è l'entità con cui interagisce l'utente finale e che richiede i token dal server di autorizzazione. Per accedere alla risorsa, il client deve ottenere l'autorizzazione da parte del proprietario della risorsa.
* Il **server della risorsa** è la posizione in cui si trova la risorsa o i dati. Considera attendibile il Server di autorizzazione per autenticare e autorizzare il OAuth Client in modo sicuro e Usa i token di accesso Bearer per assicurarsi che l'accesso a una risorsa può essere concesso.

## <a name="app-registration"></a>Registrazione delle app

Ogni app che deve accettare sia personali e account aziendali o dell'istituto di istruzione deve essere registrato tramite il nuovo **registrazioni per l'App (anteprima)** un'esperienza nello sviluppo di [portale di Azure](https://portal.azure.com/?Microsoft_AAD_RegisteredApps=true#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) prima che può accedere gli utenti utilizzo di OAuth 2.0 oppure OpenID Connect. Il processo di registrazione app raccoglie e assegna all'app alcuni valori:

* Un **ID applicazione** che identifica l'app in modo univoco
* Un **URI di reindirizzamento** o un **identificatore di pacchetto** che può essere usato per indirizzare le risposte all'app
* Altri valori specifici dello scenario

Per altri dettagli, vedere [Azure Active Directory B2C: registrare l'applicazione](quickstart-v2-register-an-app.md).

## <a name="endpoints"></a>Endpoint

Dopo la registrazione, l'app comunica con Azure AD inviando richieste all'endpoint v2.0:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Dove `{tenant}` può assumere uno dei quattro diversi valori:

| Valore | DESCRIZIONE |
| --- | --- |
| `common` | Consente agli utenti con account Microsoft e account aziendali o dell'istituto d'istruzione di Azure AD di accedere all'applicazione. |
| `organizations` | Consente solo agli utenti con account aziendali o dell'istituto d'istruzione di Azure AD di accedere all'applicazione. |
| `consumers` | Consente solo agli utenti con account Microsoft personali (MSA) di accedere all'applicazione. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` oppure `contoso.onmicrosoft.com` | Consente solo agli utenti con account aziendali o dell'istituto d'istruzione di un determinato tenant di Azure AD di accedere all'applicazione. È possibile usare il nome di dominio descrittivo del tenant di Azure AD o l'identificatore GUID. |

Per informazioni su come interagire con questi endpoint, scegliere un tipo di app specifico nella sezione [Protocolli](#protocols) e seguire i collegamenti per altre informazioni.

> [!TIP]
> Qualsiasi app registrata in Azure AD possono usare l'endpoint v2.0, anche se non accedono gli account personali.  In questo modo, è possibile migrare le applicazioni esistenti alla versione 2.0 e [MSAL](reference-v2-libraries.md) senza creare di nuovo l'applicazione.  

## <a name="tokens"></a>Tokens

L'implementazione v2.0 di OAuth 0 2.0 e OpenID Connect fa un uso intensivo dei token di connessione, inclusi quelli rappresentati come JWT (Token Web JSON). Un token di connessione è un token di sicurezza leggero che consente al "portatore" di accedere a una risorsa protetta. In questo senso, per "portatore" si intende qualsiasi parte che sia in grado di presentare il token. Anche se il rilascio del token di connessione è condizionato dal completamento del processo di autenticazione in Azure AD, se non vengono adottate le misure necessarie per proteggere il token durante la trasmissione e l'archiviazione, è possibile che venga intercettato e usato da parti non autorizzate. Molti token di sicurezza hanno meccanismi integrati per prevenire l'uso non autorizzato, ma i token di connessione ne sono sprovvisti e devono essere trasportati su un canale protetto, ad esempio Transport Layer Security (HTTPS). Se un token di connessione viene trasmesso senza protezione, un utente malintenzionato può usare un attacco man-in-the-middle per acquisire il token e usarlo per l'accesso non autorizzato a una risorsa protetta. Gli stessi principi di sicurezza si applicano quando un token di connessione viene archiviato o memorizzato nella cache per un uso futuro. Assicurarsi sempre che l'app trasmetta e archivi i token di connessione in modo sicuro. Per altre considerazioni sulla sicurezza dei token di connessione, vedere la [sezione 5 della specifica RFC 6750](https://tools.ietf.org/html/rfc6750).

Altri dettagli sui diversi tipi di token usati nell'endpoint v2.0 sono disponibili nel [riferimento al token dell'endpoint v2.0](v2-id-and-access-tokens.md).

## <a name="protocols"></a>Protocolli

Se si è pronti vedere alcuni esempi di richieste, iniziare con una delle esercitazioni di seguito. Ognuna corrisponde a uno scenario di autenticazione specifico. Se è necessario determinare quale sia il flusso giusto per l’utente, consultare [i tipi di app che è possibile compilare con v2.0](v2-app-types.md).

* [Compilare applicazioni native e per dispositivi mobili con OAuth 2.0](v2-oauth2-auth-code-flow.md)
* [Creare App web con OpenID Connect](v2-protocols-oidc.md)
* [Compilare App a singola pagina con il flusso implicito OAuth 2.0](v2-oauth2-implicit-grant-flow.md)
* [Compilare daemon o processi sul lato server con il flusso di credenziali client OAuth 2.0](v2-oauth2-client-creds-grant-flow.md)
* [Ottenere i token in un'API web con il flusso on-behalf-of di OAuth 2.0](v2-oauth2-on-behalf-of-flow.md)

---
title: Eseguire la migrazione di utenti con identità di social network in Azure Active Directory B2C | Microsoft Docs
description: Presentazione dei concetti di base sulla migrazione di utenti con identità di social network in Azure AD B2C usando l'API Graph.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/03/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 12b464d9b6bd09acb9c93ab1de0ba178f28a778a
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/03/2019
ms.locfileid: "58894902"
---
# <a name="azure-active-directory-b2c-migrate-users-with-social-identities"></a>Azure Active Directory B2C: Eseguire la migrazione di utenti con identità dei social network
Per eseguire la migrazione del provider di identità ad Azure AD B2C, potrebbe essere necessario eseguire anche la migrazione degli utenti con identità di social networking. L'articolo spiega come eseguire la migrazione di account con identità di social networking esistenti, ad esempio: account di Facebook, LinkedIn, Microsoft e Google per Azure AD B2C. Questo articolo è valido anche per le identità federate, tuttavia queste migrazioni sono meno comuni.

## <a name="prerequisites"></a>Prerequisiti
Questo articolo è una continuazione dell'articolo sulla migrazione degli utenti ed è incentrato sulla migrazione di identità di social networking. Prima di iniziare, leggere [Migrazione degli utenti](active-directory-b2c-user-migration.md).

## <a name="social-identities-migration-introduction"></a>Introduzione alla migrazione di identità di social networking

* In Azure AD B2C i nomi di accesso degli **account locali** (nome utente o indirizzo di posta elettronica) sono archiviati nella raccolta `signInNames` nel record utente. `signInNames` contiene uno o più record `signInName` che specificano i nomi di accesso per l'utente. Ogni nome di accesso deve essere univoco a livello di tenant.

* Le identità degli **account di social networking** sono archiviate nella raccolta `userIdentities`. La voce specifica l'`issuer` (nome del provider di identità), ad esempio facebook.com, e l'`issuerUserId`, ovvero un identificatore utente univoco per l'emittente. L'attributo `userIdentities` contiene uno o più record UserIdentity che specificano il tipo di account di social networking e l'identificatore utente univoco dal provider di identità basato su social network.

* **Combinare account locali con identità di social networking**. Come accennato, i nomi di accesso degli account locali e le identità di account di social networking vengono archiviati in attributi diversi. `signInNames` viene utilizzato per l'account locale, mentre `userIdentities` per account di social networking. Un singolo account Azure AD B2C può essere solo un account locale, solo un account di social networking oppure combinare un account locale con l'identità di social networking in un singolo record utente. Questo comportamento consente di gestire un singolo account, mentre un utente può accedere con le credenziali dell'account locale o con le identità di social networking.

* `UserIdentity` Type - contiene le informazioni relative all'identità dell'utente account di social networking in un tenant di Azure AD B2C:
  * `issuer` La rappresentazione di stringa del provider di identità che ha emesso l'identificatore utente, ad esempio facebook.com.
  * `issuerUserId` L'identificatore utente univoco usato dal provider di identità basati su social network in formato base64.

    ```JSON
    "userIdentities": [{
          "issuer": "Facebook.com",
          "issuerUserId": "MTIzNDU2Nzg5MA=="
      }
    ]
    ```

* A seconda del provider di identità, l'**ID utente di social networking** è un valore univoco per ogni applicazione o account di sviluppo per un utente specificato. Configurare i criteri di Azure AD B2C con lo stesso ID di applicazione assegnato in precedenza dal provider di social networking. In alternativa configurare un'altra applicazione nello stesso account di sviluppo.

## <a name="use-graph-api-to-migrate-users"></a>Usare l'API Graph per eseguire la migrazione degli utenti
L'account utente di Azure AD B2C viene creato tramite [API Graph](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet). Per comunicare con l'API Graph è innanzitutto necessario avere un account di servizio con privilegi amministrativi. In Azure AD registrare un'applicazione e l'autenticazione ad Azure AD. Le credenziali dell'applicazione sono l'ID applicazione e il segreto dell'applicazione. L'applicazione agisce autonomamente, non come utente, per chiamare l'API Graph. Seguire le istruzioni nel passaggio 1 nell'articolo sulla [migrazione degli utenti](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-user-migration).

## <a name="required-properties"></a>Proprietà obbligatorie
L'elenco seguente include le proprietà obbligatorie per la creazione di un utente.
* **accountEnabled** - true
* **displayName** - Nome da visualizzare nella Rubrica per l'utente.
* **passwordProfile** - Profilo di password per l'utente. 

> [!NOTE]
> Solo per gli account di social networking (senza credenziali per un account locale), è comunque necessario specificare la password. Azure AD B2C ignora la password specificata per gli account di social networking.

* **userPrincipalName** - Nome dell'entità utente (someuser@contoso.com). Il nome dell'entità utente deve contenere uno dei domini verificati per il tenant. Per specificare il nome UPN, generare un nuovo valore GUID e concatenarlo con `@` e il nome del tenant.
* **mailNickname** - Alias di posta per l'utente. Questo valore può essere lo stesso ID usato per userPrincipalName. 
* **signInNames** - Uno o più record SignInName che specificano i nomi di accesso per l'utente. Ogni nome di accesso deve essere univoco a livello di società/tenant. Solo per gli account di social networking, questa proprietà può essere lasciata vuota.
* **userIdentities** - Uno o più record UserIdentity che specificano il tipo di account di social networking e l'identificatore utente univoco dal provider di identità basato su social network.
* [facoltativo] **otherMails** - Solo per gli account di social networking, indirizzi di posta elettronica dell'utente 

Per altre informazioni, vedere: [Informazioni di riferimento per l'API Graph](/previous-versions/azure/ad/graph/api/users-operations#CreateLocalAccountUser)

## <a name="migrate-social-account-only"></a>Eseguire la migrazione (solo) di account di social networking
Per creare solo account di social networking, senza credenziali per account locali. Inviare una richiesta HTTPS POST all'API Graph. Il corpo della richiesta contiene le proprietà dell'utente con account di social networking da creare. Come minimo, è necessario specificare le proprietà obbligatorie. 


**POST**  https://graph.windows.net/tenant-name.onmicrosoft.com/users

Inviare i dati di form seguenti: 

```JSON
{
    "objectId": null,
    "accountEnabled": true,
    "mailNickname": "c8c3d3b8-60cf-4c76-9aa7-eb3235b190c8",
    "signInNames": [],
    "creationType": null,
    "displayName": "Sara Bell",
    "givenName": "Sara",
    "surname": "Bell",
    "passwordProfile": {
        "password": "Test1234",
        "forceChangePasswordNextLogin": false
    },
    "passwordPolicies": null,
    "userIdentities": [{
            "issuer": "Facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ],
    "otherMails": ["sara@live.com"],
    "userPrincipalName": "c8c3d3b8-60cf-4c76-9aa7-eb3235b190c8@tenant-name.onmicrosoft.com"
}
```
## <a name="migrate-social-account-with-local-account"></a>Eseguire la migrazione di account di social networking con account locali
Per creare un account locale combinato con identità di social networking. Inviare una richiesta HTTPS POST all'API Graph. Il corpo della richiesta contiene le proprietà dell'utente con account di social networking da creare. Come minimo, è necessario specificare le proprietà obbligatorie. 

**POST**  https://graph.windows.net/tenant-name.onmicrosoft.com/users

Inviare i dati di form seguenti: 

```JSON
{
    "objectId": null,
    "accountEnabled": true,
    "mailNickname": "5164db16-3eee-4629-bfda-dcc3326790e9",
    "signInNames": [{
            "type": "emailAddress",
            "value": "david@contoso.com"
        }
    ],
    "creationType": "LocalAccount",
    "displayName": "David Hor",
    "givenName": "David",
    "surname": "Hor",
    "passwordProfile": {
        "password": "1234567",
        "forceChangePasswordNextLogin": false
    },
    "passwordPolicies": "DisablePasswordExpiration,DisableStrongPassword",
    "userIdentities": [{
            "issuer": "contoso.com",
            "issuerUserId": "ZGF2aWRAY29udG9zby5jb20="
        }
    ],
    "otherMails": [],
    "userPrincipalName": "5164db16-3eee-4629-bfda-dcc3326790e9@tenant-name.onmicrosoft.com"
}
```

## <a name="frequently-asked-questions"></a>Domande frequenti
### <a name="how-can-i-know-the-issuer-name"></a>Come è possibile risalire al nome dell'emittente?
Il nome dell'emittente, o il nome del provider di identità, è configurato nei criteri. Se non si conosce il valore da specificare in `issuer`, seguire questa procedura:
1. Accedere con uno degli account di social networking
2. Dal token JWT copiare il valore `sub`. `sub` contiene in genere l'ID oggetto dell'utente in Azure AD B2C. In alternativa, dal portale di Azure aprire le proprietà dell'utente e copiare l'ID dell'oggetto.
3. Aprire [Graph Explorer di Azure AD](https://graphexplorer.azurewebsites.net).
4. Accedere con l'account amministratore.
5. Eseguire la richiesta GET seguente. Sostituire userObjectId con l'ID utente copiato. **GET** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId
6. Individuare l'elemento `userIdentities` all'interno del risultato JSON da Azure AD B2C.
7. [Facoltativo] È anche possibile che si voglia decodificare il valore `issuerUserId`.

> [!NOTE]
> Usare un account di amministratore tenant B2C che sia locale in relazione al tenant B2C. La sintassi del nome dell'account è admin@tenant-name.onmicrosoft.com.

### <a name="is-it-possible-to-add-social-identity-to-an-existing-local-account"></a>È possibile aggiungere identità di social networking a un account locale esistente?
Sì. Dopo aver creato l'account locale, è possibile aggiungere l'identità di social networking. Eseguire una richiesta PATCH HTTPS. Sostituire userObjectId con l'ID utente da aggiornare. 

**PATCH** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId

Inviare i dati di form seguenti: 

```JSON
{
    "userIdentities": [
        {
            "issuer": "Facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ]
}
```

### <a name="is-it-possible-to-add-multiple-social-identities"></a>È possibile aggiungere più identità di social networking?
Sì. È possibile aggiungere più identità di social networking per un singolo account Azure AD B2C. Eseguire una richiesta PATCH HTTPS. Sostituire userObjectId con l'ID utente. 

**PATCH** https://graph.windows.net/tenant-name.onmicrosoft.com/users/userObjectId

Inviare i dati di form seguenti: 

```JSON
{
    "userIdentities": [
        {
            "issuer": "google.com",
            "issuerUserId": "MjQzMjE2NTc4NTQ="
        },
        {
            "issuer": "facebook.com",
            "issuerUserId": "MTIzNDU2Nzg5MA=="
        }
    ]
}
```

## <a name="optional-user-migration-application-sample"></a>[Facoltativo] Esempio di applicazione per la migrazione degli utenti
[Scaricare ed eseguire la versione 2 dell'app di esempio](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration). La versione 2 dell'app di esempio usa un file JSON che contiene dati utente fittizi, inclusi account locale, account di social networking e identità locale e di social networking in un singolo account.  Per modificare il file JSON, aprire la soluzione Visual Studio `AADB2C.UserMigration.sln`. Nel progetto `AADB2C.UserMigration` aprire il file `UsersData.json`. Il file contiene un elenco di entità utente. Ogni entità utente ha le proprietà seguenti:
* **signInName** - Per l'account locale, indirizzo di posta elettronica per l'accesso
* **displayName** - Nome visualizzato dell'utente
* **firstName** - Nome dell'utente
* **lastName** - Cognome dell'utente
* **password** - Per l'account locale, password dell'utente (può essere vuota)
* **issuer** - Per l'account di social networking, nome del provider di identità
* **issuerUserId** - Per l'account di social networking, identificatore utente univoco usato dal provider di identità basato su social network. Il valore deve essere in testo non crittografato. L'app di esempio applica la codifica base64 a questo valore.
* **email** - Solo per l'account di social networking (non combinato), indirizzo di posta elettronica dell'utente

```JSON
{
  "userType": "emailAddress",
  "Users": [
    {
      // Local account only
      "signInName": "James@contoso.com",
      "displayName": "James Martin",
      "firstName": "James",
      "lastName": "Martin",
      "password": "Pass!w0rd"
    },
    {
      // Social account only
      "issuer": "Facebook.com",
      "issuerUserId": "1234567890",
      "email": "sara@contoso.com",
      "displayName": "Sara Bell",
      "firstName": "Sara",
      "lastName": "Bell"
    },
    {
      // Combine local account with social identity
      "signInName": "david@contoso.com",
      "issuer": "Facebook.com",
      "issuerUserId": "0987654321",
      "displayName": "David Hor",
      "firstName": "David",
      "lastName": "Hor",
      "password": "Pass!w0rd"
    }
  ]
}
```

> [!NOTE]
> Se non si aggiorna il file UsersData.json nell'esempio con i dati reali, è possibile accedere con le credenziali dell'account locale di esempio ma non con gli esempi di account di social networking. Per eseguire la migrazione degli account di social networking reali, specificare i dati corrispondenti.

Per altre informazioni su come usare l'app di esempio, vedere [Azure Active Directory B2C: Migrazione degli utenti](active-directory-b2c-user-migration.md)

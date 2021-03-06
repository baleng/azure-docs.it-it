---
title: Configurare l'autenticazione di Facebook - Servizio app di Azure
description: Informazioni su come configurare l'autenticazione Facebook per un'applicazione dei servizi app.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: cc10c9be5bab3b84c8773d8a930473267db353ab
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53411308"
---
# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Come configurare un'applicazione del servizio App per usare l'account di accesso di Facebook
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Questo argomento descrive come configurare il servizio app di Azure per usare Facebook come provider di autenticazione.

Per completare la procedura descritta in questo argomento, è necessario disporre di un account di Facebook con un indirizzo di posta elettronica verificato e un numero di cellulare. Per creare un nuovo account di Facebook, visitare il sito [facebook.com].

## <a name="register"></a>Registrare l'applicazione con Facebook
1. Accedere al [portale di Azure], e passare all'applicazione. Copiare l' **URL**. Verrà usato per configurare l'app Facebook.
2. In un'altra finestra del browser passare al sito Web [Facebook Developers] e accedere con le credenziali dell'account Facebook.
3. (Facoltativo) Se non è ancora stata effettuata la registrazione, fare clic su **Apps** (App)  > **Register as a Developer** (Registrarsi come sviluppatore), quindi accettare le condizioni e seguire la procedura di registrazione.
4. Fare clic su **App personali** > **Aggiungi una nuova app**.
5. In **Nome visualizzato**, digitare un nome univoco per l'app. Inserire anche l'**Indirizzo di posta elettronica di contatto**, quindi fare clic su **Crea ID app** e completare il controllo di sicurezza. Si passerà al dashboard dello sviluppatore per la nuova app di Facebook.
6. Sotto **Facebook Login** (Account di accesso a Facebook), fare clic su **Setup**, quindi scegliere **Impostazioni** nel riquadro di spostamento a sinistra sotto **Facebook Login** (Account di accesso a Facebook).
7. Aggiungere l'URI di **Redirect URI** (URI di reindirizzamento) dell'applicazione in **Valid OAuth redirect URIs** (URI di reindirizzamento OAuth validi) e quindi fare clic su **Save Changes** (Salva modifiche).
   
   > [!NOTE]
   > L'URI di reindirizzamento corrisponde all'URL dell'applicazione con l'aggiunta del percorso */.auth/login/facebook/callback*. Ad esempio: `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Assicurarsi che sia in uso lo schema HTTPS.
   > 
   > 
8. Nel riquadro di spostamento a sinistra fare clic su **Impostazioni** > **Basic**. Nel campo **App Secret** (Segreto app) fare clic su **Show** (Mostra), fornire la password se richiesto, quindi prendere nota dei valori di **App ID** (ID app) e **App Secret** (Segreto app). Verranno usati più avanti per configurare l'applicazione in Azure.
   
   > [!IMPORTANT]
   > Il segreto dell'app è una credenziale di sicurezza importante. Non condividere questo valore con altri e non distribuirlo all'interno di un'applicazione client.
   > 
   > 
9. L'account di Facebook usato per registrare l'applicazione sarà un account di amministratore dell'app. A questo punto, solo gli amministratori potranno effettuare l'accesso a questa applicazione. Per eseguire l'autenticazione di altri account di Facebook, fare clic su **App Review** (Controllo app) e abilitare l'opzione **Make <nome-app> public** (Rendi pubblica) e consentire così l'accesso pubblico generale con l'autenticazione di Facebook.

## <a name="secrets"></a>Aggiungere le informazioni di Facebook all'applicazione
1. Nel [portale di Azure], passare all'applicazione. Fare clic su **Impostazioni** > **Autenticazione/Autorizzazione**, quindi assicurarsi che l'opzione **Autenticazione servizio app** sia impostata su **Sì**.
2. Fare clic su **Facebook**, incollare i valori di ID app e segreto app i valori ottenuti in precedenza e abilitare facoltativamente tutti gli ambiti richiesti dall'applicazione, quindi fare clic su **OK**.
   
    ![][0]
   
    Per impostazione predefinita, il servizio app fornisce l'autenticazione ma non limita l'accesso alle API e al contenuto del sito solo agli utenti autorizzati. È necessario autorizzare gli utenti nel codice dell'app.
3. (Facoltativo) Per consentire l'accesso al sito solo agli utenti autenticati da Facebook, impostare il parametro **Azione da eseguire quando la richiesta non è autenticata** su **Facebook**. Per poter utilizzare questa funzione, tuttavia, è necessario che tutte le richieste vengano autenticate e che le richieste non autenticate vengano reindirizzate a Facebook per l'autenticazione.
4. Al termine della configurazione dell'autenticazione, fare clic su **Salva**.

È ora possibile usare un account di Facebook per l'autenticazione nell'app.

## <a name="related-content"></a>Contenuti correlati
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook Developers]: https://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: https://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Portale di Azure]: https://portal.azure.com/

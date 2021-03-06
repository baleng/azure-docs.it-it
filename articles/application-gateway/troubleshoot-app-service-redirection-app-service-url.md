---
title: "Risoluzione dei problemi di Gateway applicazione di Azure con il servizio App: consente il reindirizzamento all'URL del servizio App"
description: Questo articolo fornisce informazioni su come risolvere il problema di reindirizzamento quando viene usato il Gateway applicazione di Azure con servizio App di Azure
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/22/2019
ms.author: absha
ms.openlocfilehash: f456cfec82a315a2be877a52e4f3f1850b992736
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59274537"
---
# <a name="troubleshoot-application-gateway-with-app-service"></a>Risolvere i problemi di Gateway applicazione con il servizio App

Informazioni su come diagnosticare e risolvere i problemi riscontrati con il Gateway applicazione e servizio App come il server back-end.

## <a name="overview"></a>Panoramica

In questo articolo si apprenderà come risolvere i problemi seguenti:

> [!div class="checklist"]
> * URL del servizio App introduzione esposti nel browser quando è presente un reindirizzamento
> * Dominio del ARRAffinity Cookie del servizio App è impostato su nome host del servizio App (example.azurewebsites.net) anziché host originale

Quando si configura un pubblico con connessione del servizio App nel pool di back-end del Gateway applicazione e se si dispone di un reindirizzamento configurato nel codice dell'applicazione, si noterà che quando si accede ai Gateway applicazione, si verrà reindirizzati direttamente all'App nel browser URL del servizio.

Questo problema può verificarsi a causa dei motivi principali seguenti:

- È necessario il reindirizzamento configurato nel servizio App. Il reindirizzamento può essere semplice quanto l'aggiunta di una barra finale alla richiesta.
- È necessario l'autenticazione di Azure AD che causa il reindirizzamento.
- È stata abilitata l'opzione "Selezionare indirizzo nome Host dal back-end" nelle impostazioni HTTP del Gateway applicazione.
- Non è necessario il dominio personalizzato registrato con il servizio App.

Quando si usa servizi di applicazione dietro il Gateway applicazione e si usa un dominio personalizzato per il Gateway applicazione di accesso, si può vedere anche il valore di dominio per il cookie ARRAffinity impostato dal servizio App includeranno il nome di dominio "example.azurewebsites.net". Se si desidera che il nome host originale sia anche il dominio del cookie, seguire la soluzione in questo articolo.

## <a name="sample-configuration"></a>Configurazione di esempio

- Listener HTTP: Basic o a più siti
- Pool di indirizzi back-end: Servizio app
- Impostazioni HTTP: Abilitato "pick nome host di indirizzi back-end"
- Probe: Abilitato "pick Hostname dalle impostazioni HTTP"

## <a name="cause"></a>Causa

Un servizio App è accessibile solo con i nomi host configurato nelle impostazioni del dominio personalizzato, per impostazione predefinita, è "example.azurewebsites.net" e se si desidera accedere al servizio App usando il Gateway applicazione con il nome host non è registrato nel servizio App o con Del Gateway applicazione FQDN, è necessario eseguire l'override del nome host nella richiesta originale al nome host del servizio App.

Per ottenere questo risultato con il Gateway applicazione, si usa l'opzione "Selezionare il nome host da indirizzi di back-end" nelle impostazioni HTTP e per il probe di funzionamento, usiamo ""selezionare il nome host dal back-end HTTP impostazioni nella configurazione del probe.

![appservice-1](./media/troubleshoot-app-service-redirection-app-service-url/appservice-1.png)

Per questo motivo, quando il servizio App esegue un reindirizzamento, Usa il nome host "example.azurewebsites.net" nell'intestazione Location, anziché il nome host originale a meno che non configurato in caso contrario. È possibile verificare le seguenti intestazioni esempio di richiesta e risposta.
```
## Request headers to Application Gateway:

Request URL: http://www.contoso.com/path

Request Method: GET

Host: www.contoso.com

## Response headers:

Status Code: 301 Moved Permanently

Location: http://example.azurewebsites.net/path/

Server: Microsoft-IIS/10.0

Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=example.azurewebsites.net

X-Powered-By: ASP.NET
```
Nell'esempio precedente, è possibile notare che l'intestazione della risposta con codice di stato di 301 per il reindirizzamento e l'intestazione del percorso è il nome host del servizio App anziché il nome host originale "www.contoso.com".

## <a name="solution"></a>Soluzione

Questo problema può essere risolto senza che sia un reindirizzamento sul lato dell'applicazione, tuttavia, se non è possibile, che è necessario passare la stessa intestazione host che riceve i Gateway applicazione per il servizio App anche invece di eseguire una sostituzione dell'host.

Una volta che eseguiamo, servizio App eseguirà il reindirizzamento (se presenti) sulla stessa intestazione host originale che fa riferimento al Gateway applicazione e non il proprio.

A tale scopo, è necessario possedere un dominio personalizzato e seguire la procedura riportata di seguito.

- Registrare il dominio per l'elenco di domini personalizzati del servizio App. A tale scopo, è necessario disporre di un record CNAME nel dominio personalizzato che punta al nome di dominio completo del servizio App. Per altre informazioni, vedere [eseguire il mapping di un nome DNS personalizzato esistente al servizio App di Azure](https://docs.microsoft.com//azure/app-service/app-service-web-tutorial-custom-domain).

![appservice-2](./media/troubleshoot-app-service-redirection-app-service-url/appservice-2.png)

- Al termine dell'operazione, il servizio App è pronto ad accettare il nome host "www.contoso.com". A questo punto modificare la voce CNAME in DNS in modo che punti al nome di dominio completo del Gateway applicazione. Ad esempio, "appgw.eastus.cloudapp.azure.com".

- Assicurarsi che il dominio "www.contoso.com" viene risolto al FQDN del Gateway applicazione, quando si esegue una query DNS.

- Impostare il probe personalizzato per disabilitare "Selezionare il nome host dal back-end HTTP Settings". Ciò può essere eseguita dal portale, deselezionare la casella di controllo nelle impostazioni del probe e in PowerShell, non usare la proprietà PickHostNameFromBackendHttpSettings - passare il comando Set-AzApplicationGatewayProbeConfig. Nel campo nome host del probe, immettere il nome di dominio completo del servizio App "example.azurewebsites.net" come le richieste di probe inviate dal Gateway applicazione includeranno questo nell'intestazione dell'host.

  > [!NOTE]
  > Durante il passaggio successivo, assicurarsi che il probe personalizzato non è associato alle impostazioni di back-end HTTP perché gli ancora le impostazioni HTTP sono l'opzione "Selezionare il nome host da indirizzi di back-end" abilitata a questo punto.

- Configurare le impostazioni HTTP del Gateway applicazione per disabilitare "Selezionare il nome host da indirizzi di back-end". Questa operazione può essere eseguita dal portale, deselezionare la casella di controllo e in PowerShell non si usa - PickHostNameFromBackendAddress passare nel comando Set-AzApplicationGatewayBackendHttpSettings.

- Associare il probe personalizzato le impostazioni HTTP di back-end e verificare l'integrità back-end se è integro.

- Al termine, il Gateway applicazione ora deve inoltrare lo stesso nome host "www.contoso.com" al servizio App e lo stesso nome host verrà eseguito il reindirizzamento. È possibile verificare le seguenti intestazioni esempio di richiesta e risposta.

Per implementare i passaggi indicati in precedenza tramite PowerShell per un'installazione esistente, eseguire lo script di PowerShell di esempio riportato di seguito. Si noti come non è stato usato gli interruttori - PickHostname nella configurazione del Probe e le impostazioni HTTP.

```azurepowershell-interactive
$gw=Get-AzApplicationGateway -Name AppGw1 -ResourceGroupName AppGwRG
Set-AzApplicationGatewayProbeConfig -ApplicationGateway $gw -Name AppServiceProbe -Protocol Http -HostName "example.azurewebsites.net" -Path "/" -Interval 30 -Timeout 30 -UnhealthyThreshold 3
$probe=Get-AzApplicationGatewayProbeConfig -Name AppServiceProbe -ApplicationGateway $gw
Set-AzApplicationGatewayBackendHttpSettings -Name appgwhttpsettings -ApplicationGateway $gw -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 30
Set-AzApplicationGateway -ApplicationGateway $gw
```
  ```
  ## Request headers to Application Gateway:

  Request URL: http://www.contoso.com/path

  Request Method: GET

  Host: www.contoso.com

  ## Response headers:

  Status Code: 301 Moved Permanently

  Location: http://www.contoso.com/path/

  Server: Microsoft-IIS/10.0

  Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=www.contoso.com

  X-Powered-By: ASP.NET
  ```
  ## <a name="next-steps"></a>Passaggi successivi

Se i passaggi precedenti non risolvono il problema, aprire un [ticket di supporto](https://azure.microsoft.com/support/options/).

---
title: Come usare Gestione API di Azure con le reti virtuali
description: Informazioni su come configurare una connessione a una rete virtuale in Gestione API di Azure e usarla per accedere ai servizi Web.
services: api-management
documentationcenter: ''
author: vlvinogr
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2019
ms.author: apimpm
ms.openlocfilehash: 78efcefa7df99dfa3386dcdf19aafa47d7b9fab1
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/03/2019
ms.locfileid: "58884509"
---
# <a name="how-to-use-azure-api-management-with-virtual-networks"></a>Come usare Gestione API di Azure con le reti virtuali
Le reti virtuali di Azure (VNET) consentono di posizionare le risorse di Azure in una rete instradabile non Internet a cui si controlla l'accesso. Queste reti possono quindi essere connesse alle reti locali usando diverse tecnologie VPN. Per altre informazioni sulle reti virtuali di Azure, è possibile iniziare dalla [Panoramica sulla rete virtuale di Azure](../virtual-network/virtual-networks-overview.md).

Gestione API di Azure può essere distribuito all'interno della rete virtuale (VNET) in modo che possa accedere ai servizi di back-end all'interno della rete. Il portale per sviluppatori e il gateway dell'API possono essere configurati in modo che siano accessibili da Internet o solo all'interno della rete virtuale.

> [!NOTE]
> Gestione API di Azure supporta le reti virtuali classiche e Azure Resource Manager.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [premium-dev.md](../../includes/api-management-availability-premium-dev.md)]

## <a name="prerequisites"></a>Prerequisiti

Per eseguire i passaggi descritti in questo articolo, è necessario disporre di:

+ Una sottoscrizione di Azure attiva.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Un'istanza di Gestione API. Per altre informazioni, vedere [Create an Azure API Management instance](get-started-create-service-instance.md) (Creare un'istanza di Gestione API di Azure).

## <a name="enable-vpn"></a>Attivare la connessione VNET

### <a name="enable-vnet-connectivity-using-the-azure-portal"></a>Abilitare la connettività di rete virtuale usando il portale di Azure

1. Nel [portale di Azure](https://portal.azure.com/) passare all'istanza di Gestione API.
2. Selezionare **Rete virtuale**.
3. Configurare l'istanza di Gestione API da distribuire all'interno di una rete virtuale.

    ![Menu della rete virtuale di Gestione API][api-management-using-vnet-menu]
4. Selezionare il tipo di accesso da usare:

   * **Esterno**: il gateway di Gestione API e il portale per gli sviluppatori sono accessibili dalla rete internet pubblica tramite un servizio di bilanciamento del carico esterno. Il gateway può accedere alle risorse all'interno della rete virtuale.

     ![Peering pubblico][api-management-vnet-public]

   * **Interno**: il gateway di Gestione API e il portale per gli sviluppatori sono accessibili soltanto dalla rete virtuale tramite un servizio di bilanciamento del carico interno. Il gateway può accedere alle risorse all'interno della rete virtuale.

     ![Peering privato][api-management-vnet-private]`

     Verrà ora visualizzato un elenco di tutte le aree in cui viene eseguito il provisioning del servizio Gestione API. Selezionare una VNET e una subnet per ogni area. L'elenco viene popolato con le reti virtuali classiche e Resource Manager disponibili nelle sottoscrizioni di Azure, impostate nell'area che si sta configurando.

     > [!NOTE]
     > **Endpoint di servizio** nel diagramma precedente include Gateway/Proxy, il portale di Azure, il portale per sviluppatori, GIT e l'endpoint di gestione diretta.
     > **Endpoint di gestione** nel diagramma precedente è l'endpoint ospitato nel servizio per la gestione della configurazione tramite il portale di Azure e Powershell.
     > Inoltre si noti che, sebbene il diagramma mostra gli indirizzi IP per i vari endpoint, il servizio Gestione API risponde **solo** ai relativi nomi host configurati.

     > [!IMPORTANT]
     > Quando si distribuisce un'istanza di gestione API di Azure a una rete virtuale Resource Manager, il servizio deve essere in una subnet dedicata che non contiene altre risorse, a eccezione di istanze di gestione API di Azure. Se si tenta di distribuire un'istanza di gestione API di Azure a una subnet della rete virtuale Resource Manager contenente altre risorse, la distribuzione avrà esito negativo.
     >

     ![Selezionare una VPN][api-management-setup-vpn-select]

5. Fare clic su **Salva** nella parte superiore della schermata.

> [!NOTE]
> L'indirizzo VIP dell'istanza di Gestione API può cambiare ogni volta che la rete virtuale viene abilitata o disabilitata.
> L'indirizzo VIP viene modificato quando Gestione API passa da **Esterna** a **Interna** o viceversa
>

> [!IMPORTANT]
> Se si rimuove Gestione API da una rete virtuale o si modifica quella in cui è distribuito, la rete virtuale usata in precedenza può rimanere bloccata fino a due ore. Durante questo periodo non sarà possibile eliminare la rete virtuale o distribuirvi una nuova risorsa.

## <a name="enable-vnet-powershell"></a>Abilitare la connessione della rete virtuale usando i cmdlet di PowerShell
È inoltre possibile abilitare la connettività della rete virtuale utilizzando i cmdlet di PowerShell

* **Creare un servizio Gestione API all'interno di una rete virtuale**: Usare il cmdlet [New-AzApiManagement](/powershell/module/az.apimanagement/new-azapimanagement) per creare un servizio Gestione API di Azure all'interno di una rete virtuale.

* **Distribuire un servizio Gestione API esistente all'interno di una rete virtuale**: Usare il cmdlet [Update-AzApiManagementRegion](/powershell/module/az.apimanagement/update-azapimanagementregion) per spostare un servizio Gestione API di Azure esistente all'interno di una rete virtuale.

## <a name="connect-vnet"></a>Connettersi a un servizio Web ospitato all'interno di una rete virtuale
Dopo che il servizio Gestione API è stato connesso alla VNET, l'accesso ai servizi di back-end all'interno della rete virtuale non è diverso dall'accesso ai servizi pubblici. È sufficiente digitare l'indirizzo locale o il nome host (se è stato configurato un server DNS per la VNET) del servizio Web nel campo **URL del servizio Web** quando si crea una nuova API o se ne modifica una esistente.

![Aggiungere un'API dalla VPN][api-management-setup-vpn-add-api]

## <a name="network-configuration-issues"> </a>Problemi comuni di configurazione di rete
Di seguito è riportato un elenco di problemi di configurazione comuni che possono verificarsi durante la distribuzione del servizio Gestione API in una rete virtuale.

* **Installazione di server DNS personalizzata**: il servizio Gestione API dipende da vari servizi di Azure. Quando Gestione API è ospitata in una rete virtuale con un server DNS personalizzato, deve risolvere i nomi host dei servizi di Azure. Vedere [queste](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) informazioni aggiuntive sulla configurazione del DNS personalizzato. Vedere la tabella delle porte e altri requisiti di rete per riferimento.

> [!IMPORTANT]
> Se si intende usare un server DNS personalizzato per la rete virtuale, è consigliabile impostarlo **prima** di distribuirvi un servizio Gestione API. In caso contrario è necessario aggiornare il servizio Gestione API ogni volta che si modifica il server DNS eseguendo [l'operazione di applicazione della configurazione di rete](https://docs.microsoft.com/rest/api/apimanagement/ApiManagementService/ApplyNetworkConfigurationUpdates)

* **Porte necessarie per il servizio Gestione API**: il traffico in ingresso e in uscita nella subnet in cui viene distribuita la gestione delle API può essere controllato usando il [gruppo di sicurezza di rete][Network Security Group]. Se una qualsiasi di queste porte non è disponibile, Gestione API potrebbe non funzionare correttamente e potrebbe diventare inaccessibile. Il blocco di una o più di tali porte è un problema di configurazione comune nell'uso di Gestione API in una rete virtuale.

Quando un'istanza del servizio Gestione API è ospitata in una rete virtuale, vengono usate le porte indicate nella tabella seguente.

| Porte di origine/destinazione | Direzione          | Protocollo di trasporto |   [Tag di servizio](../virtual-network/security-overview.md#service-tags) <br> Origine/Destinazione   | Scopo (*)                                                 | Tipo di rete virtuale |
|------------------------------|--------------------|--------------------|---------------------------------------|-------------------------------------------------------------|----------------------|
| * / 80, 443                  | In ingresso            | TCP                | INTERNET / VIRTUAL_NETWORK            | Comunicazione tra client e Gestione API                      | Esterno             |
| */3443                     | In ingresso            | TCP                | ApiManagement / VIRTUAL_NETWORK       | Endpoint di gestione per il portale di Azure e PowerShell         | Esterno e interno  |
| * / 80, 443                  | In uscita           | TCP                | VIRTUAL_NETWORK / Storage             | **Dipendenza su archiviazione di Azure**                             | Esterno e interno  |
| * / 80, 443                  | In uscita           | TCP                | VIRTUAL_NETWORK / AzureActiveDirectory | Azure Active Directory (dove applicabile)                   | Esterno e interno  |
| * / 1433                     | In uscita           | TCP                | VIRTUAL_NETWORK/SQL                 | **Accesso agli endpoint SQL di Azure**                           | Esterno e interno  |
| * / 5672                     | In uscita           | TCP                | VIRTUAL_NETWORK / EventHub            | Dipendenza per il criterio Registra a Hub eventi | Esterno e interno  |
| * / 445                      | In uscita           | TCP                | VIRTUAL_NETWORK / Storage             | Dipendenza dalla condivisione file di Azure per GIT                      | Esterno e interno  |
| * / 1886                     | In uscita           | TCP                | VIRTUAL_NETWORK / INTERNET            | Necessaria per la pubblicazione dello stato di integrità in Integrità risorse          | Esterno e interno  |
| * / 443                     | In uscita           | TCP                | VIRTUAL_NETWORK / AzureMonitor         | Pubblicare log e metriche di diagnostica                        | Esterno e interno  |
| * / 25                       | In uscita           | TCP                | VIRTUAL_NETWORK / INTERNET            | Connessione al server di inoltro SMTP per l'invio di messaggi di posta elettronica                    | Esterno e interno  |
| * / 587                      | In uscita           | TCP                | VIRTUAL_NETWORK / INTERNET            | Connessione al server di inoltro SMTP per l'invio di messaggi di posta elettronica                    | Esterno e interno  |
| * / 25028                    | In uscita           | TCP                | VIRTUAL_NETWORK / INTERNET            | Connessione al server di inoltro SMTP per l'invio di messaggi di posta elettronica                    | Esterno e interno  |
| * / 6381 - 6383              | In ingresso e in uscita | TCP                | VIRTUAL_NETWORK / VIRTUAL_NETWORK     | Istanze di accesso Cache Azure per Redis tra RoleInstances          | Esterno e interno  |
| * / *                        | In ingresso            | TCP                | VIRTUAL_NETWORK/AZURE_LOADBALANCER | Bilanciamento del carico di infrastruttura di Azure                          | Esterno e interno  |

>[!IMPORTANT]
> Le porte per cui *Scopo* è **grassetto** sono necessarie per la corretta distribuzione del servizio Gestione API. Se si bloccano le altre porte, si verifica una riduzione delle prestazioni nella capacità di usare e monitorare il servizio in esecuzione.

+ **Funzionalità SSL**: per abilitare la creazione e la convalida della catena di certificati SSL, il servizio Gestione API richiede la connettività di rete in uscita a ocsp.msocsp.com, mscrl.microsoft.com e crl.microsoft.com. Questa dipendenza non è necessaria se un certificato caricato in Gestione API contiene l'intera catena per la radice dell'autorità di certificazione.

+ **Accesso a DNS**: L'accesso in uscita sulla porta 53 è necessario per la comunicazione con i server DNS. Se è presente un server DNS personalizzato all'altra estremità di un gateway VPN, il server DNS deve essere raggiungibile dalla subnet che ospita Gestione API.

+ **Metriche e monitoraggio dell'integrità**: la connettività di rete in uscita agli endpoint di Monitoraggio di Azure, che si risolve nei domini seguenti:

    | Ambiente Azure | Endpoint                                                                                                                                                                                                                                                                                                                                                              |
    |-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Azure Public      | <ul><li>prod.warmpath.msftcloudes.com</li><li>shoebox2.metrics.nsatc.net</li><li>prod3.metrics.nsatc.net</li><li>prod3-black.prod3.metrics.nsatc.net</li><li>prod3-red.prod3.metrics.nsatc.net</li><li>prod.warm.ingestion.msftcloudes.com</li><li>`azure region`. warm.ingestion.msftcloudes.com dove `East US 2` è eastus2.warm.ingestion.msftcloudes.com</li></ul> |
    | Azure Government  | <ul><li>fairfax.warmpath.usgovcloudapi.net</li><li>shoebox2.metrics.nsatc.net</li><li>prod3.metrics.nsatc.net</li></ul>                                                                                                                                                                                                                                                |
    | Azure Cina       | <ul><li>mooncake.warmpath.chinacloudapi.cn</li><li>shoebox2.metrics.nsatc.net</li><li>prod3.metrics.nsatc.net</li></ul>                                                                                                                                                                                                                                                |

+ **Inoltro SMTP**: Connettività di rete in uscita per l'inoltro SMTP, che si risolve nell'host `smtpi-co1.msn.com`, `smtpi-ch1.msn.com`, `smtpi-db3.msn.com`, `smtpi-sin.msn.com` e `ies.global.microsoft.com`

+ **CAPTCHA del portale per sviluppatori**: connettività di rete in uscita per il CAPTCHA del portale degli sviluppatori, che si risolve nell'host `client.hip.live.com`.

+ **Diagnostica del portale di Azure**: per abilitare il flusso dei log di diagnostica dal portale di Azure quando si usa l'estensione di Gestione API dall'interno di una rete virtuale, è richiesto l'accesso in uscita a `dc.services.visualstudio.com` sulla porta 443. Ciò consente di risolvere eventuali problemi che potrebbero verificarsi quando si usa l'estensione.

+ **Forzano il Tunneling del traffico al Firewall dall'ambiente locale tramite Express Route o Network Appliance virtuale**: Una configurazione comune dei clienti è delegata subnet passano attraverso un firewall locale o un'appliance virtuale di rete per definire la propria route predefinita (0.0.0.0/0) che forza tutto il traffico da Gestione API. Questo flusso di traffico interrompe sempre la connettività con Gestione API di Azure perché il traffico in uscita è bloccato in locale o convertito tramite NAT in un set non riconoscibile di indirizzi che non usano più i diversi endpoint di Azure. La soluzione è necessario eseguire un paio di aspetti:

  * Abilitare gli endpoint di servizio nella subnet in cui viene distribuito il servizio Gestione API. [Gli endpoint di servizio] [ ServiceEndpoints] devono essere abilitati per Azure Sql, archiviazione di Azure, hub eventi di Azure e bus di servizio di Azure. Abilitazione degli endpoint direttamente da Gestione API di subnet delegate a questi servizi consente loro di usare la rete backbone di Microsoft Azure che fornisce il routing ottimale per il traffico del servizio. Se si usano gli endpoint di servizio con una gestione Api sottoposti a tunneling forzato, i servizi di Azure precedenti non verrà forzato il traffico sottoposto a tunneling. L'altre API di gestione viene forzato il traffico delle dipendenze del servizio sottoposto a tunneling e non possono essere perse o il servizio Gestione API non funzionerà correttamente.
    
  * Tutto il controllo piano il traffico da Internet all'endpoint di gestione del servizio Gestione API vengono indirizzate attraverso un set specifico di indirizzi IP in ingresso ospitato da Gestione API. Quando il traffico viene sottoposto a tunneling forzato le risposte non simmetricamente assocerà questi indirizzi IP di origine in ingresso. Per superare il limite, è necessario aggiungere le route definite dall'utente seguenti ([route definite dall'utente][UDRs]) per guidare il traffico ad Azure, impostare la destinazione di queste route host per "Internet". Il set di indirizzi IP in ingresso per il traffico del piano di controllo è come segue:
    
    > 13.84.189.17/32, 13.85.22.63/32, 23.96.224.175/32, 23.101.166.38/32, 52.162.110.80/32, 104.214.19.224/32, 13.64.39.16/32, 40.81.47.216/32, 51.145.179.78/32, 52.142.95.35/32, 40.90.185.46/32, 20.40.125.155/32

  * Per altri di gestione API di dipendenze che vengono sottoposti a tunneling, forzato del servizio loro dovrebbe essere modo per risolvere il nome host e a raggiungere l'endpoint. Questi includono
      - Le metriche e monitoraggio dell'integrità
      - Portale di Azure Diagnostics
      - Inoltro SMTP
      - Portale per sviluppatori CAPTCHA

## <a name="troubleshooting"> </a>Risoluzione dei problemi
* **Installazione iniziale**: quando la distribuzione iniziale del servizio Gestione API in una subnet non ha esito positivo, è consigliabile distribuire prima una macchina virtuale nella stessa subnet. Accedere successivamente al desktop remoto nella macchina virtuale e convalidare l'esistenza di connettività a una delle risorse indicate di seguito nella sottoscrizione di Azure in uso:
    * BLOB di Archiviazione di Azure
    * Database SQL di Azure
    * Tabella di archiviazione di Azure

  > [!IMPORTANT]
  > Dopo aver convalidato la connettività, assicurarsi di rimuovere tutte le risorse distribuite nella subnet, prima di distribuire Gestione API nella subnet.

* **Aggiornamenti incrementali**: Quando si apportano modifiche alla rete, fare riferimento a [API NetworkStatus](https://docs.microsoft.com/rest/api/apimanagement/networkstatus)per verificare che il servizio Gestione API non abbia perso l'accesso a una delle risorse critiche, da cui dipende. Lo stato della connettività dovrebbe essere aggiornato ogni 15 minuti.

* **Collegamenti di navigazione delle risorse**: quando si esegue la distribuzione in una subnet di macchina virtuale in stile Resource Manager, Gestione API riserva la subnet, creando un collegamento di navigazione delle risorse. Se la subnet contiene già una risorsa da un provider diverso, la distribuzione ha **esito negativo**. Quando, analogamente, si sposta un servizio Gestione API in una subnet diversa o lo si elimina, viene rimosso il collegamento di navigazione delle risorse.

## <a name="subnet-size"> </a> Requisito per le dimensioni della subnet
Azure riserva alcuni indirizzi IP all'interno di ogni subnet e questi indirizzi non possono essere usati. Il primo e l'ultimo indirizzo IP delle subnet sono riservati per motivi di conformità al protocollo, insieme ad altri tre indirizzi usati per i servizi di Azure. Per altre informazioni, vedere [Esistono restrizioni sull'uso di indirizzi IP all'interno di tali subnet?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Oltre agli indirizzi IP usati dall'infrastruttura di rete virtuale di Azure, ogni istanza di Gestione API nella subnet usa due indirizzi IP per ogni unità dello SKU Premium o un indirizzo IP per lo SKU Developer. Ogni istanza riserva un indirizzo IP aggiuntivo per il bilanciamento del carico esterno. Quando si esegue la distribuzione in una rete virtuale interna, è necessario un indirizzo IP aggiuntivo per il bilanciamento del carico interno.

Dato il calcolo precedente, le dimensioni minime della subnet, in cui è possibile distribuire gestione API è/29 che offre tre indirizzi IP.

## <a name="routing"> </a> Routing
+ Un indirizzo IP pubblico con bilanciamento del carico (VIP) viene riservato per consentire l'accesso a tutti gli endpoint di servizio.
+ Un indirizzo IP di un intervallo IP di subnet (DIP) viene usato per accedere alle risorse all'interno della rete rituale e un indirizzo IP pubblico (VIP) viene usato per accedere alle risorse esterne alla rete virtuale.
+ L'indirizzo IP pubblico con bilanciamento del carico è disponibile nel pannello Panoramica/Informazioni di base del portale di Azure.

## <a name="limitations"></a>Limitazioni
* Una subnet contenente le istanze di Gestione API non può contenere altri tipi di risorse di Azure.
* La subnet e il servizio di Gestione API devono essere nella stessa sottoscrizione.
* Una subnet contenente le istanze di gestione API non può essere spostata da una sottoscrizione all'altra.
* Per le distribuzioni di Gestione API su più aree in modalità rete virtuale interna configurata, gli utenti sono responsabili della gestione del bilanciamento del carico poiché sono titolari del routing.
* La connettività da una risorsa in una rete virtuale con peering a livello globale in un'altra area al servizio Gestione API in modalità interna non funzionerà a causa delle limitazioni della piattaforma. Per altre informazioni, vedere [Le risorse in una rete virtuale non possono comunicare con il servizio di bilanciamento del carico interno di Azure nella rete virtuale con peering](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints)


## <a name="related-content"></a>Contenuti correlati
* [La connessione di una rete virtuale back-end tramite Vpn Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti)
* [La connessione di una rete virtuale da diversi modelli di distribuzione](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [Come usare Controllo API per tenere traccia delle chiamate in Gestione API di Azure](api-management-howto-api-inspector.md)
* [Domande frequenti sulla rete virtuale](../virtual-network/virtual-networks-faq.md)
* [Tag di servizio](../virtual-network/security-overview.md#service-tags)

[api-management-using-vnet-menu]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-private]: ./media/api-management-using-with-vnet/api-management-vnet-internal.png
[api-management-vnet-public]: ./media/api-management-using-with-vnet/api-management-vnet-external.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[UDRs]: ../virtual-network/virtual-networks-udr-overview.md
[Network Security Group]: ../virtual-network/security-overview.md
[ServiceEndpoints]: ../virtual-network/virtual-network-service-endpoints-overview.md
[ServiceTags]: ../virtual-network/security-overview.md#service-tags

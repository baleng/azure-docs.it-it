---
title: Informazioni sui gateway di rete virtuale per ExpressRoute - Azure | Microsoft Docs
description: Informazioni sui gateway di rete virtuale per ExpressRoute. Questo articolo fornisce informazioni su SKU e tipi di gateway.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: d9c607114d6c6c56c25303a88dcc11f4ab804eb4
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2019
ms.locfileid: "57404339"
---
# <a name="about-virtual-network-gateways-for-expressroute"></a>Informazioni sui gateway di rete virtuale per ExpressRoute
Il gateway di rete virtuale viene usato per inviare il traffico di rete tra le reti virtuali di Azure e i percorsi locali. È possibile usare un gateway di rete virtuale per il traffico di ExpressRoute o il traffico VPN. Questo articolo è incentrato sui gateway di rete virtuale ExpressRoute e contiene informazioni su SKU, prestazioni stimate per SKU e tipi di gateway.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="gateway-types"></a>Tipi di gateway

Quando si crea un gateway di rete virtuale, è necessario specificare alcune impostazioni. Una delle impostazioni necessarie, "-GatewayType", indica se il gateway verrà usato per ExpressRoute o il traffico VPN. Ci sono due tipi di gateway:

* **Vpn**: per inviare il traffico crittografato attraverso una rete Internet pubblica si usa il gateway di tipo "VPN", detto anche gateway VPN. Le connessioni da sito a sito, da punto a sito e da rete virtuale a rete virtuale usano tutte un gateway VPN.

* **ExpressRoute**: per inviare il traffico di rete con una connessione privata si usa il tipo di gateway ExpressRoute. Questo è detto anche gateway ExpressRoute ed è il tipo di gateway usato durante la configurazione di ExpressRoute.

Ogni rete virtuale può avere un solo gateway di rete virtuale per tipo di gateway. Ad esempio, è possibile configurare un gateway di rete virtuale che usa -GatewayType Vpn e una che usa -GatewayType ExpressRoute.

## <a name="gwsku"></a>SKU del gateway
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Se si desidera aggiornare il gateway a uno SKU del gateway più potente, nella maggior parte dei casi è possibile usare il cmdlet di PowerShell 'Resize-AzVirtualNetworkGateway'. Questa tecnica funziona per gli aggiornamenti agli SKU Standard e HighPerformance. Tuttavia, per eseguire l'aggiornamento per allo SKU UltraPerformance sarà necessario ricreare il gateway. La ricreazione di un gateway comporta tempi di inattività.

### <a name="aggthroughput"></a>Prestazioni stimate in base allo SKU del gateway
La tabella seguente illustra i tipi di gateway e le prestazioni stimate. La tabella è valida per entrambi i modelli di distribuzione classica e di Gestione risorse.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> Le prestazioni dell'applicazione dipendono da vari fattori, ad esempio la latenza end-to-end e il numero di flussi di traffico avviati dall'applicazione. I numeri nella tabella rappresentano il limite massimo che l'applicazione può raggiungere in teoria in un ambiente ideale.
>
>

### <a name="zrgw"></a>SKU gateway con ridondanza della zona

È possibile distribuire gateway ExpressRoute anche in zone di disponibilità di Azure. Questo le separa in modo fisico e logico in diverse zone di disponibilità, proteggendo la connettività di rete locale ad Azure da errori a livello di zona.

![Gateway ExpressRoute con ridondanza della zona](./media/expressroute-about-virtual-network-gateways/zone-redundant.png)

I gateway con ridondanza della zona usano nuovi SKU gateway specifici per il gateway ExpressRoute.

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

I nuovi SKU gateway supportano anche altre opzioni di distribuzione per soddisfare meglio le esigenze. Quando si crea un gateway di rete virtuale usando i nuovi SKU gateway, è anche possibile distribuire il gateway in una zona specifica. In questo caso si parla di gateway a livello di zona. Quando si distribuisce un gateway a livello di zona, tutte le istanze del gateway vengono distribuite nella stessa zona di disponibilità.

## <a name="resources"></a>API REST e cmdlet PowerShell
Per altre risorse tecniche e requisiti di sintassi specifici quando si usano le API REST e i cmdlet PowerShell per le configurazioni di gateway di rete virtuale, vedere le pagine seguenti:

| **Classico** | **Gestione risorse** |
| --- | --- |
| [PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#azure) |[PowerShell](https://docs.microsoft.com/powershell/module/az.network#networking) |
| [API REST](https://msdn.microsoft.com/library/jj154113.aspx) |[API REST](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni sulle configurazioni delle connessioni disponibili, vedere [Panoramica tecnica relativa a ExpressRoute](expressroute-introduction.md) .

Per altre informazioni sulla creazione di gateway ExpressRoute, vedere [Create a virtual network gateway for ExpressRoute](expressroute-howto-add-gateway-resource-manager.md) (Creare un gateway di rete virtuale per ExpressRoute).

Per altre informazioni sulla configurazione di gateway con ridondanza della zona, vedere [Creare un gateway di rete virtuale con ridondanza della zona](../../articles/vpn-gateway/create-zone-redundant-vnet-gateway.md).

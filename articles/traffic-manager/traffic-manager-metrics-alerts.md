---
title: Metriche e avvisi in Gestione traffico di Azure
description: Questo articolo descrive le metriche disponibili per Gestione traffico in Azure.
services: traffic-manager
author: KumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/11/2018
ms.author: kumud
ms.openlocfilehash: 1c0c48efc7abf4b810e92b5cd04d1538577c71d3
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59268286"
---
# <a name="traffic-manager-metrics-and-alerts"></a>Metriche e avvisi di Gestione traffico

Gestione traffico offre un bilanciamento del carico basato su DNS, con diversi metodi di routing e opzioni di monitoraggio degli endpoint. Questo articolo descrive le metriche e gli avvisi associati disponibili per i clienti. 

## <a name="metrics-available-in-traffic-manager"></a>Metriche disponibili in Gestione traffico 

Gestione traffico offre le metriche seguenti per ogni profilo, che possono essere usate dai clienti per comprendere l'uso di Gestione traffico e lo stato dei propri endpoint in tale profilo.  

### <a name="queries-by-endpoint-returned"></a>Query da endpoint restituite
Usare [questa metrica](../azure-monitor/platform/metrics-supported.md) per visualizzare il numero di query elaborate da un profilo di Gestione traffico in un periodo di tempo specifico. È anche possibile visualizzare le stesse informazioni con una granularità a livello di endpoint, per comprendere quante volte un endpoint è stato restituito nelle risposte alle query da Gestione traffico.

Nell'esempio seguente, la Figura 1 visualizza tutte le risposte alle query restituite dal profilo di Gestione traffico. 

  
![Visualizzazione aggregata con tutte le query](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-queries-aggregate-view.png)

*Figura 1: Visualizzazione aggregazione con tutte le query*
  
La Figura 2 mostra le stesse informazioni, suddivise tuttavia per endpoint. È quindi visibile il volume di risposte alle query nelle quali è stato restituito un endpoint specifico.

![Metriche di Gestione traffico: visualizzazione del volume di query suddiviso per endpoint](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-query-volume-per-endpoint.png)

*Figura 2: Doppia visualizzazione con volumi di query visualizzati per ogni endpoint restituite*

## <a name="endpoint-status-by-endpoint"></a>Stato endpoint per endpoint
Usare [questa metrica](../azure-monitor/platform/metrics-supported.md#microsoftnetworktrafficmanagerprofiles) per comprendere lo stato di integrità degli endpoint nel profilo. Sono necessari due valori:
 - usare **1**  se l'endpoint è attivo.
 - usare **0**  se l'endpoint è inattivo.

Questa metrica può essere visualizzata sia come valore aggregato che rappresenta lo stato di tutte le metriche (Figura 3), sia come suddivisione (vedi Figura 4) per mostrare lo stato di endpoint specifici. Nel primo caso, se il livello di aggregazione selezionato è **Avg**, il valore di questa metrica sarà la media aritmetica dello stato di tutti gli endpoint. Ad esempio, se un profilo ha due endpoint e solo uno è integro, questa metrica avrà un valore di **0,50** come mostrato nella Figura 3. 


![Metriche di Gestione traffico: visualizzazione composita dello stato degli endpoint](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-composite-view.png)

*Figura 3: Visualizzazione composta di metrica relativa allo stato di endpoint: aggregazione "Avg" selezionata*


![Metriche di Gestione traffico: visualizzazione suddivisa dello stato degli endpoint](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-split-view.png)

*Figura 4: Doppia visualizzazione delle metriche di stato di endpoint*

È possibile utilizzare queste metriche tramite il portale del [servizio Monitoraggio di Azure](../azure-monitor/platform/metrics-supported.md), l'[API REST](https://docs.microsoft.com/rest/api/monitor/), l'[interfaccia della riga di comando di Azure](https://docs.microsoft.com/cli/azure/monitor) e [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.applicationinsights) oppure tramite la sezione delle metriche dell'esperienza del portale di Gestione traffico.

## <a name="alerts-on-traffic-manager-metrics"></a>Avvisi sulle metriche di Gestione traffico
Oltre all'elaborazione e alla visualizzazione delle metriche di Gestione traffico, Monitoraggio di Azure consente ai clienti di configurare e ricevere avvisi associati a tali metriche. È possibile scegliere quali condizioni debbano essere soddisfatte in queste metriche per generare un avviso, la frequenza con cui tali condizioni debbano essere monitorate e le modalità di invio degli avvisi. Per altre informazioni, vedere la [documentazione degli avvisi di Monitoraggio di Azure](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

## <a name="next-steps"></a>Passaggi successivi
- Altre informazioni sul [servizio Monitoraggio di Azure](../azure-monitor/platform/metrics-supported.md)
- Informazioni su come [creare un nuovo grafico usando Monitoraggio di Azure](../azure-monitor/platform/metrics-getting-started.md#create-your-first-metric-chart)

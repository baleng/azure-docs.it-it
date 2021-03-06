---
title: Raccogliere i dati di Windows firewall nell'anteprima di Azure Sentinel | Microsoft Docs
description: Informazioni su come raccogliere i dati di Windows firewall in Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 0e41f896-8521-49b8-a244-71c78d469bc3
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/28/2019
ms.author: rkarlin
ms.openlocfilehash: 3839d81f70b8bc6dcb1da3c4dd77f52443294707
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2019
ms.locfileid: "58574841"
---
# <a name="connect-windows-firewall"></a>Connettere Windows Firewall

> [!IMPORTANT]
> Azure Sentinel è attualmente in anteprima pubblica.
> Questa versione di anteprima viene messa a disposizione senza contratto di servizio e non è consigliata per i carichi di lavoro di produzione. Alcune funzionalità potrebbero non essere supportate o potrebbero presentare funzionalità limitate. Per altre informazioni, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Il connettore di Windows firewall consente di connettere facilmente i log di firewall di Windows, se sono connessi all'area di lavoro Azure Sentinel. Questa connessione consente di visualizzare i dashboard, creare avvisi personalizzati e migliorare l'analisi. Questo ti offre informazioni più dettagliate sulla rete dell'organizzazione e migliorare le funzionalità di sicurezza.  


> [!NOTE]
> 
> - I dati verranno archiviati nella posizione geografica dell'area di lavoro in cui in esecuzione Azure Sentinel.

## <a name="enable-the-connector"></a>Abilitare il connettore 

1. Nel portale di Azure Sentinel, selezionare **raccolta di dati** e quindi fare clic sui **firewall Windows** riquadro. 
1. Selezionare i tipi di dati da trasmettere in streaming.
1. Fare clic su **Installa**.
6. Per usare lo schema appropriato nel Log Analitica per il firewall di Windows, cercare **SecurityEvent**.

## <a name="validate-connectivity"></a>Convalidare la connettività

Potrebbero occorrere fino a 20 minuti fino a quando i log di avvio venga visualizzato nel Log Analitica. 



## <a name="next-steps"></a>Passaggi successivi
In questo documento è stato descritto come connettere Azure Sentinel di firewall di Windows. Per altre informazioni su Azure Sentinel, vedere gli articoli seguenti:
- Informazioni su come [ottenere la visibilità di dati e le potenziali minacce](quickstart-get-visibility.md).
- Iniziare a usare [rilevando minacce con Azure Sentinel](tutorial-detect-threats.md).


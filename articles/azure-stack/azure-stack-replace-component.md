---
title: Sostituire un componente hardware in un nodo di unità di scala di Azure Stack | Microsoft Docs
description: Informazioni su come sostituire un componente hardware in un sistema integrato Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: mabrigg
ms.lastreviewed: 12/06/2018
ms.openlocfilehash: 1671b755b6dc5277141810d177a55bc7d6a51211
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/12/2019
ms.locfileid: "57771668"
---
# <a name="replace-a-hardware-component-on-an-azure-stack-scale-unit-node"></a>Sostituire un componente hardware in un nodo di unità di scala di Azure Stack

*Si applica a: Sistemi integrati di Azure Stack*

Questo articolo descrive il processo generale per sostituire i componenti hardware non a caldo. La sostituzione effettiva procedura varia in base al fornitore dell'hardware OEM (OEM). Per conoscere i passaggi dettagliati specifici per il sistema integrato Azure Stack, vedere la documentazione dell'unità sostituibile sul campo del fornitore.

I componenti non a caldo includono quanto segue:

- CPU*
- Memoria *
- (BMC) della scheda madre/baseboard management controller / video carta
- Scheda di bus del disco controller/host (HBA) / backplane
- Scheda di rete (NIC)
- Disco del sistema operativo *
- Le unità dati (unità che non supportano lo scambio attivo, ad esempio schede PCI-e componente aggiuntivo) *

* Questi componenti potrebbero supportare lo scambio attivo, ma possono variare in base all'implementazione di fornitore. Vedere la documentazione FRU del proprio fornitore OEM per la procedura dettagliata.

Il diagramma di flusso seguente illustra il processo generale di FRU per sostituire un componente hardware non a caldo.

![Diagramma di flusso che mostra un componente flusso di sostituzione](media/azure-stack-replace-component/replacecomponentflow.PNG)

* Questa azione potrebbe non essere necessaria in base alla condizione dell'hardware fisica.

* * Se il fornitore dell'hardware OEM esegue la sostituzione dei componenti e gli aggiornamenti che firmware può variare in base il contratto di supporto.

## <a name="review-alert-information"></a>Esaminare le informazioni sugli avvisi

L'integrità di Azure Stack e un sistema di monitoraggio monitora lo stato delle schede di rete e le unità dati controllate da spazi di archiviazione diretta. Non rileva altri componenti hardware. Per tutti gli altri componenti hardware, gli avvisi vengono generati nell'hardware specifici del fornitore di soluzione che viene eseguito nell'host del ciclo di vita dell'hardware per il monitoraggio.  

## <a name="component-replacement-process"></a>Processo di sostituzione del componente

I passaggi seguenti offrono una panoramica generale del processo di sostituzione di componenti. Seguire questi passaggi senza fare riferimento alla documentazione del FRU forniti dall'OEM.

1. Usare l'azione di arresto per la corretta chiusura del nodo di unità di scala. Questa azione potrebbe non essere necessaria in base alla condizione dell'hardware fisica.

2. In un caso improbabile che l'azione di arresto esito negativo, usare il [svuotare](azure-stack-node-actions.md#drain) azione per inserire il nodo di unità di scala in modalità di manutenzione. Questa azione potrebbe non essere necessaria in base alla condizione dell'hardware fisica.

   > [!NOTE]  
   > In ogni caso, solo un nodo può essere disabilitato e spenta allo stesso tempo senza causare l'interruzione di S2D (spazi di archiviazione diretta).

3. Dopo il nodo di unità di scala è in modalità di manutenzione, usare il [spegnere](azure-stack-node-actions.md#scale-unit-node-actions) azione. Questa azione potrebbe non essere necessaria in base alla condizione dell'hardware fisica.

   > [!NOTE]  
   > Nel caso improbabile che spegnimento azione non funziona, usare invece l'interfaccia web di baseboard management controller (BMC).

4. Sostituire il componente hardware danneggiato. Se il fornitore dell'hardware OEM esegue la sostituzione dei componenti è stato possibile variano a seconda del contratto di supporto.  
5. Aggiornare il firmware. Seguire il processo di aggiornamento del firmware specifico del fornitore tramite l'host del ciclo di vita dell'hardware per assicurarsi che il componente hardware sostituito ha il firmware approvato applicato. Se il fornitore dell'hardware OEM esegue questo passaggio può variare in base al contratto di supporto.  
6. Usare la [Repair](azure-stack-node-actions.md#scale-unit-node-actions) azione per riportare il nodo di unità di scala in unità di scala.
7. Usare l'endpoint con privilegi per [controllare lo stato di ripristino disco virtuale](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair). Con le nuove unità dati, un processo di riparazione archiviazione completa può richiedere più ore a seconda del carico del sistema e spazio usato.
8. Al termine l'azione di ripristino, verificare che tutti gli avvisi attivi sono stati chiusi automaticamente.

## <a name="next-steps"></a>Passaggi successivi

- Per informazioni sulla sostituzione a caldo disco fisico, vedere [sostituisce un disco](azure-stack-replace-disk.md).
- Per informazioni sulla sostituzione di un nodo fisico, vedere [sostituisce un nodo di unità di scala](azure-stack-replace-node.md).

---
title: Gestire la capacità di memoria fisica per Azure Stack | Microsoft Docs
description: Monitorare e gestire lo spazio di archiviazione disponibili per Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 84518E90-75E1-4037-8D4E-497EAC72AAA1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: cb5b64869a26133e1c08095a816c2d2854f85b94
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/18/2019
ms.locfileid: "58103503"
---
# <a name="manage-physical-memory-capacity-for-azure-stack"></a>Gestire la capacità di memoria fisica per Azure Stack

*Si applica a: Sistemi integrati di Azure Stack*

Per aumentare la capacità di memoria totale disponibile per Azure Stack, è possibile aggiungere ulteriore memoria. In Azure Stack i server fisici è detta anche un *nodo di unità di scala*. Tutti i nodi di unità di scala che sono membri di una singola unità di scala deve essere la stessa quantità di memoria.

> [!note]  
> Prima di continuare, consultare la documentazione del produttore dell'hardware per verificare se un produttore supporta un aggiornamento di memoria fisica. Il contratto di supporto fornitore hardware OEM potrebbe richiedere che il fornitore di eseguire l'aggiornamento del firmware del dispositivo e il posizionamento del rack server fisici.

Il diagramma di flusso seguente illustra il processo generale di aggiungere memoria a ogni nodo di unità di scala.

![Aggiunta di memoria in ogni nodo di unità di scala](media/azure-stack-manage-storage-physical-capacity/process-to-add-memory-to-scale-unit.png)

## <a name="add-memory-to-an-existing-node"></a>Aggiungere memoria a un nodo esistente
I passaggi seguenti offrono una panoramica generale del processo di memoria di aggiunta. 

> [!Warning]
> Seguire questi passaggi senza fare riferimento alla documentazione fornita dall'OEM.
> 
> [!Warning]
> L'intera unità di scala deve essere arrestato perché non è supportato un aggiornamento in sequenza di memoria.

1. Arrestare Azure Stack tramite la procedura documentata nel [Start e stop Azure Stack](azure-stack-start-and-stop.md) articolo.
2. Aggiornare la memoria in ogni computer fisico con la documentazione del produttore dell'hardware.
3. Avviare Azure Stack tramite la procedura descritta nel [Start e stop Azure Stack](azure-stack-start-and-stop.md) articolo.

## <a name="next-steps"></a>Passaggi successivi

 - Per informazioni su come gestire gli account di archiviazione in Azure Stack per trovare, ripristinare e recuperare la capacità di archiviazione in base alle esigenze aziendali, vedere [gestire gli account di archiviazione in Azure Stack](azure-stack-manage-storage-accounts.md).
 - Per informazioni su monitora e gestisce la capacità di archiviazione della loro distribuzione di Azure Stack di operatore cloud di Azure Stack, vedere [gestire la capacità di archiviazione per Azure Stack](azure-stack-manage-storage-shares.md). 

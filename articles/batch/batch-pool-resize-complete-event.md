---
title: Evento di completamento ridimensionamento pool di Azure Batch | Microsoft Docs
description: Riferimento per l’evento di completamento ridimensionamento del pool di batch.
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: lahugh
ms.openlocfilehash: 87c98b89a49adbad88841dccbd4ba47d370b2be7
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55474310"
---
# <a name="pool-resize-complete-event"></a>Evento di completamento ridimensionamento pool

 Questo evento viene generato quando il ridimensionamento di un pool è stato completato o non è riuscito.

 L'esempio seguente illustra il corpo di un evento di completamento ridimensionamento pool relativo a un pool le cui dimensioni sono aumentate e che è stato completato correttamente.

```
{
    "id": "p_1_0_01503750-252d-4e57-bd96-d6aa05601ad8",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 4,
    "targetDedicated": 4,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "result": "Success",
    "resizeError": "The operation succeeded"
}
```

|Elemento|Tipo|Note|
|-------------|----------|-----------|
|id|string|ID del pool.|
|nodeDeallocationOption|string|Specifica quando è possibile rimuovere nodi dal pool, in caso di riduzione delle dimensioni del pool.<br /><br /> I valori possibili sono:<br /><br /> **requeue**: termina le attività in esecuzione e le reinserisce nella coda. Le attività verranno eseguite di nuovo quando il processo viene abilitato. I nodi vengono rimossi non appena le attività sono state terminate.<br /><br /> **terminate**: termina le attività in esecuzione. Le attività non verranno più eseguite. I nodi vengono rimossi non appena le attività sono state terminate.<br /><br /> **taskcompletion**: consente il completamento delle attività attualmente in esecuzione. Non viene pianificata alcuna nuova attività durante l'attesa. I nodi vengono rimossi al completamento di tutte le attività.<br /><br /> **Retaineddata**: consente il completamento delle attività attualmente in esecuzione e quindi attende che scadano tutti i periodi di conservazione dati delle attività. Non viene pianificata alcuna nuova attività durante l'attesa. I nodi vengono rimossi alla scadenza di tutti i periodi di conservazione dati delle attività.<br /><br /> Il valore predefinito è requeue.<br /><br /> In caso di aumento delle dimensioni del pool, il valore è impostato su **invalid**.|
|currentDedicated|Int32|Numero di nodi di calcolo attualmente assegnati al pool.|
|targetDedicated|Int32|Numero di nodi di calcolo richiesti per il pool.|
|enableAutoScale|Bool|Specifica se le dimensioni del pool vengono regolate automaticamente nel tempo.|
|isAutoPool|Bool|Specifica se il pool è stato creato tramite il meccanismo di pool automatico di un processo.|
|startTime|DateTime|Data e ora in cui è stato avviato il ridimensionamento del pool.|
|endTime|DateTime|Date e ora in cui è stato completato il ridimensionamento del pool.|
|resultCode|string|Risultato del ridimensionamento.|
|resultMessage|string|L'errore di ridimensionamento include i dettagli del risultato.<br /><br /> Se il ridimensionamento è stato completato correttamente, indica che l'operazione è riuscita.|
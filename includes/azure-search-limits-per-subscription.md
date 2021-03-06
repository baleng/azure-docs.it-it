---
title: File di inclusione
description: File di inclusione
services: search
author: HeidiSteen
ms.service: search
ms.topic: include
ms.date: 04/04/2018
ms.author: heidist
ms.custom: include file
ms.openlocfilehash: 0da7ad35f6efc031a52ef43caa514559c08c94fe
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/29/2019
ms.locfileid: "58633012"
---
È possibile creare più servizi all'interno di una sottoscrizione. Ognuno di essi può eseguire il provisioning a un livello specifico. Si è limitati solo dal numero di servizi consentiti a ogni livello. Ad esempio, è possibile creare fino a 12 servizi a livello Basic e altri 12 servizi a livello S1 nella stessa sottoscrizione. Per altre informazioni sui livelli, vedere [scegliere una SKU o un livello per ricerca di Azure](../articles/search/search-sku-tier.md).

È possibile aumentare i limiti massimi del servizio su richiesta. Se sono necessari più servizi nella stessa sottoscrizione, contattare il supporto di Azure.

| Risorsa            | Gratuito<sup>1</sup> | Basic | S1  | S2 | S3 | S3&nbsp;HD | L1 | L2 |
| ------------------- | ---- | ----- | --- | -- | -- | ----- | -- | -- |
| Numero massimo di servizi    |1     | 12    | 12  | 6  | 6  | 6     | 6  | 6  |
| Scalabilità massima in unità di ricerca (SU)<sup>2</sup> |N/D |3 unità di ricerca |36 unità di ricerca |36 unità di ricerca |36 unità di ricerca |36 unità di ricerca |36 unità di ricerca |36 unità di ricerca |

<sup>1</sup> Il servizio gratuito è basato su risorse condivise, non dedicate. L'aumento delle prestazioni del servizio non è supportato sulle risorse condivise.

<sup>2</sup> unità di ricerca sono unità di fatturazione allocate come un *replica* o una *partizione*. Per l'archiviazione, l'indicizzazione e le operazioni di query sono necessarie entrambe le risorse. Per altre informazioni sui calcoli delle unità di archiviazione, vedere [Ridimensionare i livelli di risorse per i carichi di lavoro di indicizzazione e query](../articles/search/search-capacity-planning.md). 
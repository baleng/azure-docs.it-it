---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2019
ms.author: rogarana
ms.openlocfilehash: 2936fd318f08c74675f7e8b382c861f4a28319fc
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2019
ms.locfileid: "58261364"
---
È possibile collegare un numero di dischi dati a una macchina virtuale di Azure. Basato sugli obiettivi di scalabilità e prestazioni per i dischi dati della macchina virtuale, è possibile determinare il numero e tipo di disco che è necessario soddisfare i requisiti di capacità e prestazioni.

> [!IMPORTANT]
> Per prestazioni ottimali, limitare il numero di dischi a elevato uso collegati alla macchina virtuale per evitare una possibile limitazione. Se tutti i dischi collegati non sono un utilizzo elevato nello stesso momento, la macchina virtuale può supportare un numero maggiore di dischi.

**Per Azure managed disks:**

Nella tabella seguente viene illustrata l'impostazione predefinita e i limiti massimi del numero di risorse per ogni area per sottoscrizione

> | Risorsa | Limite predefinito  | Limite massimo |
> | --- | --- | --- |
> | Dischi gestiti Standard | 25.000 | 50.000 |
> | Managed Disks SSD Standard | 25.000 | 50.000 |
> | Managed Disks Premium | 25.000 | 50.000 |
> | Standard_LRS snapshot | 25.000 | 50.000 |
> | Standard_ZRS snapshot | 25.000 | 50.000 |
> | Immagine gestita | 25.000 | 50.000 |

* **Per gli account di archiviazione Standard:** Un account di archiviazione Standard con una frequenza totale massima di richieste di 20.000 IOPS. Il numero totale di IOPS in tutti i dischi delle macchine virtuali in un account di archiviazione Standard non deve superare questo limite.
  
    È possibile calcolare approssimativamente il numero di dischi a elevato supportato da un account di archiviazione singolo Standard sulla base del limite di frequenza di richiesta. Ad esempio, per una VM di livello Basic, il numero massimo di dischi a elevato è circa 66, ovvero 20.000/300 IOPS per disco. Il numero massimo di dischi a elevato per una VM di livello Standard è circa 40, ovvero 20.000/500 IOPS per disco. 

* **Per gli account di archiviazione Premium:** Un account di archiviazione Premium ha una velocità effettiva totale massima di 50 Gbps. La velocità effettiva totale in tutti i dischi della macchina virtuale non deve superare questo limite.


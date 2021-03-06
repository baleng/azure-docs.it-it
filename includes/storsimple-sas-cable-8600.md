---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 51e1fd18b52d7e215ba43be540156199fb41778e
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55889186"
---
#### <a name="to-attach-the-sas-cables"></a>Per collegare i cavi SAS
1. Identificare l'enclosure principale e l'enclosure EBOD. Le due enclosure possono essere identificate osservandone il retro. Per indicazioni, vedere la seguente figura. 
   
    ![Vista posteriore dell’enclosure principale e dell’enclosure EBOD](./media/storsimple-sas-cable-8600/HCSBackplaneofprimaryandEBODenclosure.png)
   
    **Vista posteriore dell’enclosure principale e dell’enclosure EBOD**
   
   | Etichetta | DESCRIZIONE |
   |:--- |:--- |
   | 1 |Enclosure principale |
   | 2 |Chassis EBOD |
2. Individuare i numeri di serie sulle enclosure principale ed EBOD. L'adesivo con il numero di serie è attaccato alla sporgenza posteriore di ogni enclosure. I due numeri di serie delle enclosure devono essere identici. [Supporto Tecnico Microsoft](../articles/storsimple/storsimple-contact-microsoft-support.md) . Per individuare i numeri di serie più facilmente, vedere la seguente figura.
   
    ![Vista posteriore dell’enclosure con la posizione del numero di serie](./media/storsimple-sas-cable-8600/HCSRearviewofenclosureindicatinglocationofserialnumbersticker.png)
   
    **Posizione dell'etichetta del numero di serie**
   
   | Etichetta | DESCRIZIONE |
   |:--- |:--- |
   | 1 |Sporgenza dell'enclosure |
3. Usare i cavi SAS forniti per collegare l'enclosure EBOD all'enclosure principale come segue:
   
   1. Identificare le quattro porte SAS sull'enclosure principale e sull'enclosure EBOD. Le porte SAS sono contrassegnate con la dicitura EBOD sull'enclosure principale e corrispondono all’enclosure EBOD, come illustrato nella seguente figura relativa al cablaggio SAS.
   2. Usare i cavi SAS forniti per collegare la porta EBOD alla porta A.
   3. La porta EBOD sul controller 0 deve essere collegata alla porta A sul controller 0 EBOD. La porta EBOD sul controller 1 deve essere collegata alla porta A sul controller 1 EBOD. Vedere la figura seguente. 
      
      ![Cablaggio SAS per il dispositivo](./media/storsimple-sas-cable-8600/HCSSAScablingforyourdevice.png)
      
      **Cablaggio SAS**
      
      | Etichetta | DESCRIZIONE |
      |:--- |:--- |
      | A |Enclosure principale |
      | b |Chassis EBOD |
      | 1 |Controller 0 |
      | 2 |Controller 1 |
      | 3 |Controller 0 EBOD |
      | 4 |Controller 1 EBOD |
      | 5, 6 |Porte SAS sull'enclosure principale (contrassegnate con la dicitura EBOD) |
      | 7, 8 |Porte SAS sull'enclosure EBOD (Porta A) |


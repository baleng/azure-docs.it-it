---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 2d0fd904dd704c704662192e1e92fe403f0971c5
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55888510"
---
#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>Per installare gli aggiornamenti rapidi in modalità di manutenzione tramite Windows PowerShell per StorSimple
> [!IMPORTANT]
> In modalità di manutenzione, è necessario applicare l’aggiornamento rapido prima in un controller e quindi nell’altro controller.
> 
> 

1. Attivare la modalità di manutenzione per il dispositivo. Vedere [Passaggio 2: Attivare la modalità di manutenzione](../articles/storsimple/storsimple-update-device.md#step2) per istruzioni su come attivare la modalità di manutenzione.
2. Per applicare l’aggiornamento rapido, digitare:
   
     `Start-HcsHotfix` 
3. Quando richiesto, specificare il percorso della cartella condivisa di rete che contiene i file dell'aggiornamento rapido.
4. Verrà richiesto di confermare. Digitare **Y** per procedere con l'installazione dell'aggiornamento rapido.
5. Dopo aver applicato l'aggiornamento rapido in un controller, accedere all'altro controller. Applicare l'aggiornamento rapido come è stato fatto per il precedente controller.
6. Dopo aver applicato gli aggiornamenti rapidi, uscire dalla modalità di manutenzione. Per istruzioni, vedere [Passaggio 4: Uscire dalla modalità di manutenzione](../articles/storsimple/storsimple-update-device.md#step4).


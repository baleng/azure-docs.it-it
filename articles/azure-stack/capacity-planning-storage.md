---
title: Archiviazione pianificazione della capacità per Azure Stack | Microsoft Docs
description: Informazioni su archiviazione pianificazione della capacità per le distribuzioni di Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2019
ms.author: jeffgilb
ms.reviewer: prchint
ms.lastreviewed: 02/20/2019
ms.openlocfilehash: 32e6e8ff4c37554a0c3fa50e243b241eed2953cf
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56446002"
---
# <a name="azure-stack-storage-capacity-planning"></a>Pianificazione della capacità archiviazione di Azure Stack
Le sezioni seguenti riportano Azure Stack capacità di archiviazione informazioni sulla pianificazione per facilitare la pianificazione per esigenze di archiviazione di soluzioni.

## <a name="uses-and-organization-of-storage-capacity"></a>Viene utilizzato e l'organizzazione delle capacità di archiviazione
La configurazione iperconvergente di Azure Stack consente la condivisione dei dispositivi di archiviazione fisica. Le tre divisioni principali dello spazio di archiviazione disponibile sono compresi tra l'infrastruttura di archiviazione temporanea delle macchine virtuali tenant e lo spazio di archiviazione BLOB, tabelle e code dei servizi di archiviazione coerenti con Azure (ACS) di backup.

## <a name="spaces-direct-cache-and-capacity-tiers"></a>Gli spazi diretti della Cache e livelli di capacità
È presente capacità di archiviazione usato per il sistema operativo, la registrazione locale, i dump e altre risorse di archiviazione temporaneo dell'infrastruttura alle esigenze. Questa capacità di archiviazione locale è separare (dispositivi e capacità) dai dispositivi di archiviazione assegnati alla gestione della configurazione di spazi di archiviazione diretta. Il resto dei dispositivi di archiviazione viene inserito in un singolo pool di capacità di archiviazione indipendentemente dal numero di server nell'unità di scala. Questi dispositivi sono disponibili due tipi: Capacità e della cache.  I dispositivi di Cache sono semplicemente tale-Cache. Spazi diretti utilizzeranno questi dispositivi per write-back e la memorizzazione nella cache di lettura. Le capacità di questi dispositivi di Cache, anche se utilizzato, non si impegna a formattato, "visibile" capacità dei dischi virtuali formattati. I dispositivi di capacità vengono usati per questo scopo e forniscono il percorso"home" dei dati gestiti da spazi di archiviazione.

Tutte le capacità di archiviazione viene allocata e gestite direttamente dall'infrastruttura di Azure Stack. L'operatore necessario prendere decisioni relative alla configurazione, assegnazione, o gestire le scelte in fatto di espansione della capacità. Queste decisioni di progettazione sono state apportate in modo da allinearsi con i requisiti della soluzione e vengono automatizzate durante l'installazione/distribuzione iniziale o durante l'espansione della capacità. Informazioni dettagliate su resilienza, la capacità riservata per le ricompilazioni e altri dettagli sono state considerate come parte della progettazione. 

Gli operatori possono scegliere tra un flash tutte o una configurazione di archiviazione ibrida:

![Pianificazione della capacità di archiviazione di Azure](media/azure-stack-capacity-planning/storage.png)

Nella configurazione tutti flash, la configurazione può essere una configurazione a due livelli o a un solo livello.  Se la configurazione è a livello singolo, tutti i dispositivi di capacità sarà dello stesso tipo (ad esempio, NVMe o SSD SATA o SAS SSD) e i dispositivi di cache non vengono usati. In un tutti a due livelli flash configurazione, la configurazione tipica è NVMe come i dispositivi di cache e quindi entrambi SATA o unità SSD di firma di accesso condiviso come i dispositivi di capacità.

Nell'ambiente ibrido, la configurazione a due livelli, la cache è che una scelta tra NVMe, SATA o SAS SSD e la capacità è l'unità disco rigido. 

Un breve riepilogo della configurazione di archiviazione di Azure Stack e spazi di archiviazione diretta è il seguente:
- Un Pool di spazi di archiviazione per ogni unità di scala (tutti i dispositivi di archiviazione vengono configurati all'interno di un singolo pool)
- I dischi virtuali vengono creati come tre mirror copia per ottimizzare le prestazioni e resilienza
- Ogni disco virtuale viene formattato come un file system (Refs)
- Capacità del disco virtuale è calcolata e allocata in modo da lasciare quantità del dispositivo della capacità di capacità dei dati non allocata nel pool. Questo è l'equivalente di un'unità di capacità per ogni server.
- Ogni file system ReFS avrà BitLocker è abilitato per la crittografia dei dati inattivi. 

I-i dischi virtuali creati automaticamente e le relative capacità sono i seguenti:

|NOME|Calcolo della capacità|DESCRIZIONE|
|-----|-----|-----|
|Dispositivo locale o di avvio|Almeno GB 340<sup>1</sup>|Archiviazione di singoli server per le immagini del sistema operativo e le macchine virtuali dell'infrastruttura "locale"|
|Infrastruttura|3,5 TB|Tutti gli utilizzi di infrastruttura di Azure Stack|
|VmTemp|Vedere di seguito<sup>2</sup>|Le macchine virtuali tenant dispone di un disco temporaneo collegato e che i dati vengono archiviati in questi dischi virtuali|
|ACS|Vedere di seguito <sup>3</sup>|Capacità di archiviazione coerenti con Azure per il servizio BLOB, tabelle e code|

<sup>1</sup> capacità di archiviazione minima richiesta del partner di soluzioni di Azure Stack.

<sup>2</sup> le dimensioni del disco virtuale usata per la macchina virtuale Tenant i dischi temporanei viene calcolata come una percentuale della memoria fisica del server. Come indicato nelle tabelle seguenti per le dimensioni di VM IaaS di Azure, il disco temporaneo è una percentuale della memoria fisica assegnata alla macchina virtuale. L'allocazione eseguita per l'archiviazione di "disco temporaneo" in Azure Stack verrà eseguito in modo da acquisire la maggior parte dei casi d'uso, ma potrebbe non essere in grado di soddisfare tutte le esigenze di archiviazione disco temporaneo. Il rapporto scelto è un compromesso tra disponibilità di archiviazione temporanea mentre non si utilizzano la maggior parte della capacità di archiviazione della soluzione per la capacità del disco temporaneo solo. Viene creato un disco di archiviazione temporanea per ogni server nell'unità di scala. La capacità di archiviazione temporanea non aumenteranno oltre il 10% della capacità di archiviazione complessiva disponibile nel pool di archiviazione di unità di scala. Il calcolo è simile al seguente:

```
  DesiredTempStoragePerServer = PhysicalMemory * 0.65 * 8
  TempStoragePerSolution = DesiredTempStoragePerServer * NumberOfServers
  PercentOfTotalCapacity = TempStoragePerSolution / TotalAvailableCapacity
  If (PercentOfTotalCapacity <= 0.1)
      TempVirtualDiskSize = DesiredTempStoragePerServer
  Else
      TempVirtualDiskSize = (TotalAvailableCapacity * 0.1) / NumberOfServers
```

<sup>3</sup> i-i dischi virtuali creati per l'utilizzo dal servizio contenitore di AZURE sono una divisione semplice della capacità rimanenti. Come già indicato, tutti i-i dischi virtuali sono un mirror a tre vie e relativi a di un'unità capacità di capacità per ogni server sono non allocato. I vari-dischi virtuali enumerati sopra vengono allocati prima e la capacità residua viene quindi usata per i servizio contenitore di AZURE-i dischi virtuali.

## <a name="next-steps"></a>Passaggi successivi
[Scopri il foglio di calcolo di pianificazione della capacità di Azure Stack](capacity-planning-spreadsheet.md)

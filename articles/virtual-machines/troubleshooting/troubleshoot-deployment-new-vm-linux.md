---
title: Risolvere i problemi di distribuzione della macchina virtuale Linux | Microsoft Docs
description: Risolvere i problemi della distribuzione Resource Manager quando si crea una nuova macchina virtuale Linux in Azure
services: virtual-machines-linux, azure-resource-manager
documentationcenter: ''
author: JiangChen79
manager: jeconnoc
editor: ''
tags: top-support-issue, azure-resource-manager
ms.assetid: 906a9c89-6866-496b-b4a4-f07fb39f990c
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/09/2016
ms.author: cjiang
ms.openlocfilehash: 9fea914fdf9b025fd5d38219a6bfc81b4a9cc584
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2019
ms.locfileid: "57450285"
---
# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Risolvere i problemi della distribuzione Resource Manager con la creazione di una nuova macchina virtuale Linux in Azure
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>Problemi principali
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

Per altri problemi e domande sulla distribuzione delle VM, vedere [Risolvere i problemi di distribuzione della macchina virtuale Linux in Azure](troubleshoot-deploy-vm-linux.md).

## <a name="collect-activity-logs"></a>Raccogliere i log di attività
Per avviare la risoluzione dei problemi, raccogliere i log delle attività per identificare l'errore associato al problema. I collegamenti seguenti contengono informazioni dettagliate sul processo da seguire.

[Visualizzare le operazioni di distribuzione](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Visualizzare i log attività per gestire le risorse di Azure](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**S:** Se il sistema operativo è un sistema Linux generalizzato e viene caricato e/o acquisito con l'impostazione generalizzata, quindi non saranno gli eventuali errori. Analogamente, se il sistema operativo Linux è specializzato e viene caricato e/o acquisito con l'impostazione specializzata, non si verificheranno errori.

**Errori di caricamento:**

**N<sup>1</sup>:** Se il sistema operativo è un sistema Linux generalizzato e viene caricato come specializzato, si verificherà un errore di timeout del provisioning perché la macchina virtuale è bloccata nella fase di provisioning.

**N<sup>2</sup>:** Se il sistema operativo Linux specializzato e viene caricato come generalizzato, si verificherà un errore di provisioning perché la nuova macchina virtuale è in esecuzione con il nome del computer, nome utente e la password originale.

**Risoluzione:**

Per risolvere entrambi questi errori, caricare il file VHD originale, disponibile in locale, con la stessa impostazione usata per il sistema operativo (generalizzato/specializzato). Per caricare il disco come generalizzato, ricordarsi di eseguire prima -deprovision.

**Errori di acquisizione:**

**N<sup>3</sup>:** Se il sistema operativo è un sistema Linux generalizzato e viene acquisito come specializzato, si verificherà un errore di timeout del provisioning perché la macchina virtuale originale non può essere usata essendo contrassegnata come generalizzata.

**N<sup>4</sup>:** Se il sistema operativo Linux specializzato e viene acquisito come generalizzato, si verificherà un errore di provisioning perché la nuova macchina virtuale è in esecuzione con il nome del computer, nome utente e la password originale. La VM originale, inoltre, non può essere usata perché è contrassegnata come specializzata.

**Risoluzione:**

Per risolvere entrambi questi errori, eliminare l'immagine corrente dal portale e [acquisirla di nuovo dai dischi rigidi virtuali correnti](../linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) con la stessa impostazione usata per il sistema operativo (generalizzato/specializzato).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Problema: Custom / raccolta / immagine del marketplace; Errore di allocazione
Questo errore si verifica nelle situazioni in cui la nuova richiesta di VM viene aggiunta a un cluster che non può supportare le dimensioni della VM richieste oppure non ha spazio disponibile sufficiente per soddisfare la richiesta.

**Causa 1:** Il cluster non supporta le dimensioni della macchina virtuale richieste.

**Risoluzione 1:**

* Ripetere la richiesta usando una VM di dimensioni inferiori.
* Se le dimensioni della VM richieste non possono essere modificate:
  * Arrestare tutte le VM nel set di disponibilità.
    Fare clic su **Gruppi di risorse** > *gruppo di risorse personale* > **Risorse** > *set di disponibilità personale* > **Macchine virtuali** > *macchina virtuale personale* > **Arresta**.
  * Dopo l'arresto di tutte le VM, creare la nuova VM con le dimensioni desiderate.
  * Avviare prima di tutto la nuova VM e quindi selezionare le VM arrestate e fare clic su **Avvia**.

**Causa 2:** Il cluster non ha risorse disponibili.

**Risoluzione 2:**

* Ripetere la richiesta in un secondo momento.
* Se la nuova VM può far parte di un set di disponibilità diverso
  * Creare una nuova VM in un altro set di disponibilità nella stessa area.
  * Aggiungere la nuova VM alla stessa rete virtuale.

## <a name="next-steps"></a>Passaggi successivi
Se si incontrano problemi quando si avvia una VM Linux arrestata o si ridimensiona una VM Linux esistente in Azure, vedere [Risolvere i problemi della distribuzione di Resource Manager con il riavvio e il ridimensionamento di una macchina virtuale Linux esistente in Azure](../linux/restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


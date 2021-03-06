---
title: Raccogliere i dati di Azure ATP nell'anteprima di Azure Sentinel | Microsoft Docs
description: Informazioni su come raccogliere i dati di Azure ATP in Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 5bf3cc44-ecda-4c78-8a63-31ab42f43605
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/26/2019
ms.author: rkarlin
ms.openlocfilehash: 5254e60b9b7c38e5f4534e90f8aabe938aef99b2
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2019
ms.locfileid: "58574944"
---
# <a name="collect-data-from-azure-advanced-threat-protection-atp"></a>Raccogliere i dati da Azure Advanced Threat Protection (ATP)

> [!IMPORTANT]
> Azure Sentinel è attualmente in anteprima pubblica.
> Questa versione di anteprima viene messa a disposizione senza contratto di servizio e non è consigliata per i carichi di lavoro di produzione. Alcune funzionalità potrebbero non essere supportate o potrebbero presentare funzionalità limitate. Per altre informazioni, vedere [Condizioni supplementari per l'utilizzo delle anteprime di Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


È possibile trasmettere i log dal [Azure Advanced Threat Protection](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp) in Azure Sentinel con un solo clic.

## <a name="prerequisites"></a>Prerequisiti

- Utenti con autorizzazioni di sicurezza amministratore o amministratore globale
- È necessario essere un cliente di anteprima privata di Azure ATP

## <a name="connect-to-azure-atp"></a>Connettersi ad Azure ATP

Assicurarsi che sia la versione di anteprima privata di Azure ATP [abilitata nella rete](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1).
Se Azure ATP viene distribuita e l'inserimento dei dati, gli avvisi sospetti possono facilmente essere trasmessi in Sentinel di Azure. Potrebbero occorrere fino a 24 ore per gli avvisi avviare lo streaming in Azure Sentinel.



1. In Azure Sentinel, selezionare **raccolta di dati** e quindi fare clic sui **Azure ATP** riquadro.

2. Fare clic su **Connetti**.

6. Per usare lo schema appropriato nel Log Analitica per gli avvisi di Azure ATP, cercare **SecurityAlert**.

## <a name="next-steps"></a>Passaggi successivi
In questo documento è stato descritto come connettere Azure Advanced Threat Protection per Azure Sentinel. Per altre informazioni su Azure Sentinel, vedere gli articoli seguenti:
- Informazioni su come [ottenere la visibilità di dati e le potenziali minacce](quickstart-get-visibility.md).
- Iniziare a usare [rilevando minacce con Azure Sentinel](tutorial-detect-threats.md).


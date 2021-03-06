---
title: Limiti e quote - Servizio visione artificiale personalizzato
titlesuffix: Azure Cognitive Services
description: Informazioni sui limiti e le quote per il Servizio visione artificiale personalizzato.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: anroth
ms.openlocfilehash: 85abc4a50710629d3485d05115698e59a93fc96e
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2019
ms.locfileid: "58472441"
---
# <a name="limits-and-quotas"></a>Limiti e quote

Sono disponibili due livelli di chiavi per il servizio visione artificiale personalizzato. È possibile registrarsi per una sottoscrizione F0 (gratuita) o S0 (standard) tramite il portale di Azure. Per informazioni dettagliate su prezzi e transazioni, vedere la pagina corrispondente di [Prezzi di Servizi cognitivi](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/).

Si prevede che nel tempo il numero di immagini di training per progetto di tag per progetto aumenterà per i progetti S0.

||**F0**|**S0**|
|-----|-----|-----|
|Progetti|2|100|
|Immagini di training per progetto |5.000|100,000|
|Previsioni/mese|10,000 |Illimitato|
|Tag/progetto|50|500|
|Iterazioni |10|10|
|Min con l'etichetta di immagini per ogni Tag, classificazione (50 + consigliato) |5|5|
|Min con l'etichetta di immagini per ogni Tag, rilevamento di oggetti (50 + consigliato)|15|15|
|Durata dell'archiviazione immagini stimate|30 giorni|30 giorni|
|Operazioni di [stima](https://go.microsoft.com/fwlink/?linkid=865445) con archiviazione (transazioni al secondo)|2|10|
|Operazioni di [stima](https://go.microsoft.com/fwlink/?linkid=865445) senza archiviazione (transazioni al secondo)|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (chiamate API al secondo)|2|10|
|[Altre chiamate API](https://go.microsoft.com/fwlink/?linkid=865446) (transazioni al secondo)|10|10|
|Dimensioni max dell'immagine (caricamento dell'immagine di training) |6 MB|6 MB|
|Dimensioni max dell'immagine (stima)|4 MB|4 MB|

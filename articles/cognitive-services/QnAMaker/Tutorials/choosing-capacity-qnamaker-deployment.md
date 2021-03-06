---
title: 'Capacità risorsa per la distribuzione: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Prima di creare il servizio QnA Maker, è necessario stabilire quale livello dei servizi precedenti soddisfa le proprie esigenze.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/24/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: a332d263526bb6507e7394c205caa1c4d1f9e3e6
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873581"
---
# <a name="choosing-capacity-for-your-qna-maker-deployment"></a>Scelta della capacità per la distribuzione di QnA Maker

Il servizio QnA Maker stabilisce una dipendenza da tre risorse di Azure:
1.  Servizio app (per il runtime)
2.  Ricerca di Azure (per l'archiviazione di domande e risposte)
3.  App Insights (facoltativo, per archiviare log di chat e dati di telemetria)

Prima di creare il servizio QnA Maker, è necessario stabilire quale livello dei servizi precedenti soddisfa le proprie esigenze. 

Ci sono generalmente tre parametri da considerare:

1. **La velocità effettiva che il servizio deve fornire**: scegliere il [piano app](https://azure.microsoft.com/pricing/details/app-service/plans/) appropriato per il servizio app in base alle proprie esigenze. È possibile [aumentare](https://docs.microsoft.com/azure/app-service/web-sites-scale) o ridurre le prestazioni dell'app. Ciò influenzerà anche la scelta dello SKU di Ricerca di Azure. Per altri dettagli, vedere [qui](https://docs.microsoft.com/azure/search/search-sku-tier).

1. **Dimensioni e numero di Knowledge Base:**: scegliere lo [SKU di Ricerca di Azure](https://azure.microsoft.com/pricing/details/search/) appropriato per lo scenario. È possibile pubblicare N-1 Knowledge Base in un particolare livello, dove N è il numero massimo di indici consentiti nel livello. Verificare anche le dimensioni massime e il numero di documenti consentiti per ogni livello.

    Ad esempio, se il livello include 15 indici consentiti, è possibile pubblicare 14 articoli della knowledge base (1 indice per ogni articolo della knowledge base pubblicato). Il quindicesimo indice viene usato per tutti gli articoli della knowledge base per la creazione e il testing. 

1. **Numero di documenti come origini**: lo SKU gratuito del servizio di gestione di QnA Maker limita a 3 (1 MB ciascuno) il numero di documenti gestibili tramite il portale e le API. Lo SKU Standard non pone limiti al numero di documenti gestibili. Per informazioni dettagliate, vedere [qui](https://aka.ms/qnamaker-pricing).

La tabella seguente indica alcune linee guida generali.

|                        | Gestione di QnA Maker | Servizio app | Ricerca di Azure | Limitazioni                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Sperimentazione        | SKU gratuito             | Livello gratuito   | Livello gratuito    | Pubblicazione di massimo 2 Knowledge Base, dimensioni 50 MB  |
| Ambiente di sviluppo/test   | SKU Standard         | Condiviso      | Basic        | Pubblicazione di massimo 14 Knowledge Base, dimensioni 2 GB    |
| Ambiente di produzione | SKU Standard         | Basic       | Standard     | Pubblicazione di massimo 49 Knowledge Base, dimensioni 25 GB |

Per l'aggiornamento dello stack di QnA Maker, vedere [Aggiornare il servizio QnA Maker](../How-To/upgrade-qnamaker-service.md).

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Aggiornare il servizio QnA Maker](../How-To/upgrade-qnamaker-service.md)

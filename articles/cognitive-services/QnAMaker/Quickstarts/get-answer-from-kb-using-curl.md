---
title: 'Avvio rapido: Usare cURL per ottenere una risposta dalla knowledge base - QnA Maker'
titleSuffix: Azure Cognitive Services
description: Questo avvio rapido assiste nell'ottenimento di una risposta da una knowledge base usando cURL.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/05/2019
ms.author: diberry
ms.openlocfilehash: ce2d39f3941030059cef329aa60e804a44d73a98
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59263095"
---
# <a name="quickstart-get-answer-from-knowledge-base-using-curl"></a>Avvio rapido: Ottenere una risposta dalla knowledge base usando cURL

Questa guida introduttiva basata su cURL assiste nell'ottenimento di una risposta dalla knowledge base.

## <a name="prerequisites"></a>Prerequisiti

* [**cURL**](https://curl.haxx.se/) più recente.
* È necessario disporre di un [servizio QnA Maker](../How-To/set-up-qnamaker-service-azure.md) e disporre di una [knowledge base con domande e risposte](../Tutorials/create-publish-query-in-portal.md).

## <a name="publish-to-get-endpoint"></a>Pubblicare per ottenere gli endpoint

Una volta pronti a generare una risposta per una domanda dalla knowledge base, [pubblicare](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) la knowledge base.

## <a name="use-production-endpoint-with-curl"></a>Usare l'endpoint di produzione con cURL

Una volta pubblicata la knowledge base, nella pagina **Pubblica** vengono visualizzate le impostazioni della richiesta HTTP per generare una risposta. La scheda **CURL** mostra le impostazioni necessarie per generare una risposta dallo strumento da riga di comando, [CURL](https://www.getpostman.com).

[![Prisultati ubblica](../media/qnamaker-use-to-generate-answer/curl-command-on-publish-page.png)](../media/qnamaker-use-to-generate-answer/curl-command-on-publish-page.png#lightbox)

Per generare una risposta con CURL, completare i passaggi seguenti:

1. Copiare il testo nella scheda CURL. 
1. Aprire una riga di comando o un terminale e incollare il testo.
1. Modificare la domanda in modo che sia rilevante per la knowledge base. Prestare attenzione a non rimuovere il file JSON che contiene la domanda.
1. Immettere il comando. 
1. La risposta include le informazioni pertinenti alla risposta. 

    ```bash
    > curl -X POST https://qnamaker-f0.azurewebsites.net/qnamaker/knowledgebases/1111f8c-d01b-4698-a2de-85b0dbf3358c/generateAnswer -H "Authorization: EndpointKey 111841fb-c208-4a72-9412-03b6f3e55ca1" -H "Content-type: application/json" -d "{'question':'How do I programmatically update my Knowledge Base?'}"
    {
      "answers": [
        {
          "questions": [
            "How do I programmatically update my Knowledge Base?"
          ],
          "answer": "You can use our REST APIs to manage your Knowledge Base. See here for details: https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600",
          "score": 100.0,
          "id": 18,
          "source": "Custom Editorial",
          "metadata": [
            {
              "name": "category",
              "value": "api"
            }
          ]
        }
      ]
    }
    ```

## <a name="use-staging-endpoint-with-curl"></a>Usare l'endpoint di gestione temporanea con cURL

Se si desidera ottenere una risposta dall'endpoint di gestione temporanea, usare il parametro booleano querystring `isTest` con il valore di `true`.

`isTest=true`

## <a name="next-steps"></a>Passaggi successivi

La pagina di pubblicazione fornisce anche informazioni a [generare una risposta](get-answer-from-kb-using-postman.md) con Postman. 

> [!div class="nextstepaction"]
> [Usare i metadati durante la generazione di una risposta](../How-to/metadata-generateanswer-usage.md)

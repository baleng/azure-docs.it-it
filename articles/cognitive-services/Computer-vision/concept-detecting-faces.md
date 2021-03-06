---
title: Rilevamento dei visi - Visione artificiale
titleSuffix: Azure Cognitive Services
description: Concetti relativi alla funzione di rilevamento volto dell'API Visione artificiale.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 1056b8be113d56342aea8f83d5325737f7ecb93b
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56308452"
---
# <a name="face-detection-with-computer-vision"></a>Rilevamento volto con Visione artificiale

Visione artificiale può rilevare i visi umani all'interno di un'immagine e generare l'età, il sesso e il rettangolo per ogni viso rilevato. 

> [!NOTE]
> Questa funzionalità è disponibile anche nel servizio [Viso](/azure/cognitive-services/face/). Vedere questa alternativa per analisi più dettagliate dei visi, tra cui l'identificazione dei visi e il rilevamento delle pose. 

## <a name="face-detection-examples"></a>Esempi di rilevamento di visi

L'esempio seguente mostra la risposta JSON restituita da Visione artificiale per un'immagine che contiene un solo viso umano.

![Viso di donna su tetto - Analisi visione](./Images/woman_roof_face.png)

```json
{
    "faces": [
        {
            "age": 23,
            "gender": "Female",
            "faceRectangle": {
                "top": 45,
                "left": 194,
                "width": 44,
                "height": 44
            }
        }
    ],
    "requestId": "8439ba87-de65-441b-a0f1-c85913157ecd",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Png"
    }
}
```

L'esempio successivo mostra la risposta JSON restituita per un'immagine che contiene più visi umani.

![Viso foto di famiglia - Analisi visione](./Images/family_photo_face.png)

```json
{
    "faces": [
        {
            "age": 11,
            "gender": "Male",
            "faceRectangle": {
                "top": 62,
                "left": 22,
                "width": 45,
                "height": 45
            }
        },
        {
            "age": 11,
            "gender": "Female",
            "faceRectangle": {
                "top": 127,
                "left": 240,
                "width": 42,
                "height": 42
            }
        },
        {
            "age": 37,
            "gender": "Female",
            "faceRectangle": {
                "top": 55,
                "left": 200,
                "width": 41,
                "height": 41
            }
        },
        {
            "age": 41,
            "gender": "Male",
            "faceRectangle": {
                "top": 45,
                "left": 103,
                "width": 39,
                "height": 39
            }
        }
    ],
    "requestId": "3a383cbe-1a05-4104-9ce7-1b5cf352b239",
    "metadata": {
        "height": 230,
        "width": 300,
        "format": "Png"
    }
}
```

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su come usare la funzionalità di rilevamento volto, vedere la documentazione di riferimento di [Analyze Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa).

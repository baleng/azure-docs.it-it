---
title: Competenze cognitive deprecate - Ricerca di Azure
description: Questa pagina contiene un elenco delle competenze di ricerca cognitiva considerate deprecate e che non saranno supportate nel prossimo futuro.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: e4849487c2dd9330dbf9e6b6cf0ed4d1b6b96020
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2019
ms.locfileid: "56958100"
---
# <a name="deprecated-cognitive-search-skills"></a>Competenze di ricerca cognitiva deprecate

Questo documento descrive le competenze cognitive considerate deprecate. Usare la guida seguente per il contenuto:

* Nome della competenza: nome della competenza che sarà deprecata. Esegue il mapping all'attributo @odata.type.
* Ultima versione disponibile dell'API: versione più recente dell'API pubblica di Ricerca di Azure tramite la quale i set di competenze contenenti la competenza deprecata corrispondente possono essere creati/aggiornati.
* Fine del supporto: ultimo giorno dopo il quale la competenza corrispondente è considerata non supportata. I set di competenze creati in precedenza dovrebbero continuare a funzionare, ma si consiglia agli utenti di eseguire la migrazione da una competenza deprecata.
* Consigli: percorso di migrazione per usare una competenza supportata. Gli utenti sono invitati a seguire le raccomandazioni per continuare a ricevere supporto.

## <a name="microsoftskillstextnamedentityrecognitionskill"></a>Microsoft.Skills.Text.NamedEntityRecognitionSkill

### <a name="last-available-api-version"></a>Ultima versione disponibile dell'API

2017-11-11-Preview

### <a name="end-of-support"></a>Fine del supporto

15 febbraio 2019

### <a name="recommendations"></a>Consigli 

Usare invece [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md). Fornisce la maggior parte delle funzionalità di NamedEntityRecognitionSkill, ma di qualità più elevata. Include anche informazioni più dettagliate nei campi dell'output complesso.

Per eseguire la migrazione alla [competenza di riconoscimento entità](cognitive-search-skill-entity-recognition.md), sarà necessario apportare una o più delle modifiche seguenti alla definizione della competenza. È possibile aggiornare la definizione della competenza usando l'[API di aggiornamento del set di competenze](https://docs.microsoft.com/rest/api/searchservice/update-skillset).

_Nota_: il punteggio di attendibilità come concetto non è attualmente supportato. Sarà supportato a breve in futuro. Il parametro `minimumPrecision` esiste in `EntityRecognitionSkill` per un utilizzo futuro e per la compatibilità con le versioni precedenti.

1. *(Obbligatorio)* Modificare `@odata.type` da `"#Microsoft.Skills.Text.NamedEntityRecognitionSkill"` a `"#Microsoft.Skills.Text.EntityRecognitionSkill"`.

2. *(Facoltativo)* Se si usa l'output `entities`, usare invece l'output della raccolta complesso `namedEntities` di `EntityRecognitionSkill`. È possibile usare `targetName` nella definizione della competenza per eseguirne il mapping a un'annotazione denominata `entities`.

3. *(Facoltativo)* Se non si specifica in modo esplicito `categories`, `EntityRecognitionSkill` può restituire un tipo diverso di categorie oltre a quelle supportate da `NamedEntityRecognitionSkill`. Per non consentire questo comportamento, assicurarsi di impostare in modo esplicito il parametro `categories` su `["Person", "Location", "Organization"]`.

    _Definizioni di migrazione di esempio_

    * Migrazione di esempio

        _(Prima) Definizione della competenza NamedEntityRecognition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
        _(Dopo) Definizione della competenza EntityRecognition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person"],
            "defaultLanguageCode": "en",
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            }
            ]
        }
        ```
    
    * Migrazione leggermente complessa

        _(Prima) Definizione della competenza NamedEntityRecognition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "entities"
            }
            ]
        }
        ```
        _(Dopo) Definizione della competenza EntityRecognition_
        ```json
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "categories": [ "Person", "Location", "Organization" ],
            "defaultLanguageCode": "en",
            "minimumPrecision": 0.1,
            "inputs": [
            {
                "name": "text",
                "source": "/document/content"
            }
            ],
            "outputs": [
            {
                "name": "persons",
                "targetName": "people"
            },
            {
                "name": "namedEntities",
                "targetName": "entities"
            }
            ]
        }
        ```

## <a name="see-also"></a>Vedere anche 

+ [Competenze predefinite](cognitive-search-predefined-skills.md)
+ [Come definire un set di competenze](cognitive-search-defining-skillset.md)
+ [Competenza di riconoscimento entità](cognitive-search-skill-entity-recognition.md)

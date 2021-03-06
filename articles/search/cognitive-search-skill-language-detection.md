---
title: Competenza rilevamento lingua della ricerca cognitiva - Ricerca di Azure
description: Valuta testo non strutturato e per ogni record restituisce un identificatore di lingua con un punteggio che indica il livello di attendibilità dell'analisi in una pipeline di arricchimento di Ricerca di Azure.
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
ms.openlocfilehash: 605f4c639cfc8c0f9732f7347532e1bd7edc055f
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2019
ms.locfileid: "57404655"
---
#   <a name="language-detection-cognitive-skill"></a>Competenza cognitiva di rilevamento lingua

Il **rilevamento lingua** competenza rileva la lingua del testo di input e segnala il codice una sola lingua per ogni documento inviato nella richiesta. Il codice lingua è associato a un punteggio che indica il livello di attendibilità dell'analisi. Questa competenza usa i modelli di Machine Learning forniti da [Analisi del testo](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) in Servizi cognitivi.

Questa funzionalità è particolarmente utile quando è necessario specificare la lingua del testo come input per altre competenze (ad esempio, la competenza [Analisi del sentiment](cognitive-search-skill-sentiment.md) o la competenza [Divisione del testo](cognitive-search-skill-textsplit.md)).

Rilevamento della lingua si basa su librerie di elaborazione in linguaggio naturale di Bing, che supera il numero del [lingue e aree geografiche supportate](https://docs.microsoft.com/azure/cognitive-services/text-analytics/language-support) elencati per testo Analitica. L'elenco esatto delle lingue non è pubblicato, ma include tutte le lingue, diffuse in più varianti, dialetti e alcune lingue internazionali e relative alla lingua. Se si dispone di contenuto espresso in un linguaggio usato meno frequentemente, è possibile [provare l'API di rilevamento lingua](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) per vedere se viene restituito un codice. La risposta per le lingue che non può essere rilevato è `unknown`.

> [!NOTE]
> Dal 21 dicembre 2018 è possibile [collegare una risorsa di Servizi cognitivi](cognitive-search-attach-cognitive-services.md) a un set di competenze di Ricerca di Azure. Ciò consente anche di addebitare l'esecuzione del set di competenze. In questa data è iniziato anche l'addebito dell'estrazione delle immagini come parte della fase di individuazione dei documenti. L'estrazione di testo dai documenti continua a essere offerta gratuitamente.
>
> L'esecuzione delle [competenze cognitive predefinite](cognitive-search-predefined-skills.md) viene addebitata in base ai [prezzi con pagamento a consumo di Servizi cognitivi](https://azure.microsoft.com/pricing/details/cognitive-services), alla stessa tariffa che verrebbe usata se fosse stata eseguita l'attività direttamente. L'estrazione di immagini è un addebito previsto in Ricerca di Azure, attualmente offerto al prezzo di anteprima. Per informazioni dettagliate, vedere la [pagina dei prezzi di Ricerca di Azure](https://go.microsoft.com/fwlink/?linkid=2042400) oppure [Come funziona la fatturazione](search-sku-tier.md#how-billing-works).

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.LanguageDetectionSkill

## <a name="data-limits"></a>Limiti dei dati
Le dimensioni massime di un record devono essere di 50.000 caratteri in base alla misurazione di `String.Length`. Se è necessario suddividere i dati prima di inviarli all'analizzatore di valutazione, è possibile usare la [competenza di divisione del testo](cognitive-search-skill-textsplit.md).

## <a name="skill-inputs"></a>Input competenze

I parametri fanno distinzione tra maiuscole e minuscole.

| Input     | DESCRIZIONE |
|--------------------|-------------|
| text | Testo da analizzare.|

## <a name="skill-outputs"></a>Output competenze

| Nome output    | DESCRIZIONE |
|--------------------|-------------|
| languageCode | Il codice di lingua ISO 6391 per la lingua identificata. Ad esempio, "en". |
| languageName | Il nome della lingua. Ad esempio, "Inglese". |
| score | Immettere un valore compreso tra 0 e 1. La probabilità che lingua sia identificata correttamente. Il punteggio può essere inferiore a 1 se la frase contiene lingue miste.  |

##  <a name="sample-definition"></a>Definizione di esempio

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      }
    ],
    "outputs": [
      {
        "name": "languageCode",
        "targetName": "myLanguageCode"
      },
      {
        "name": "languageName",
        "targetName": "myLanguageName"
      },
      {
        "name": "score",
        "targetName": "myLanguageScore"
      }

    ]
  }
```

##  <a name="sample-input"></a>Input di esempio

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Glaciers are huge rivers of ice that ooze their way over land, powered by gravity and their own sheer weight. "
           }
      },
      {
        "recordId": "2",
        "data":
           {
             "text": "Estamos muy felices de estar con ustedes."
           }
      }
    ]
```


##  <a name="sample-output"></a>Output di esempio

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
            {
              "languageCode": "en",
              "languageName": "English",
              "score": 1,
            }
      },
      {
        "recordId": "2",
        "data":
            {
              "languageCode": "es",
              "languageName": "Spanish",
              "score": 1,
            }
      }
    ]
}
```


## <a name="error-cases"></a>Casi di errore
Se il testo è espresso in una lingua non supportata, viene generato un errore e non viene restituito alcun identificatore di lingua.

## <a name="see-also"></a>Vedere anche 

+ [Competenze predefinite](cognitive-search-predefined-skills.md)
+ [Come definire un set di competenze](cognitive-search-defining-skillset.md)

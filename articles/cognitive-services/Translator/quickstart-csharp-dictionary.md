---
title: 'Avvio rapido: Cercare le parole con il dizionario bilingue, C# - API Traduzione testuale'
titleSuffix: Azure Cognitive Services
description: In questo argomento di avvio rapido verrà illustrato come ottenere traduzioni alternative per un termine e verranno forniti esempi di utilizzo di tali traduzioni usando .NET Core e l'API Traduzione testuale.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/21/2019
ms.author: erhopf
ms.openlocfilehash: b5a14791b30ef825a136840a81900940c6def16d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58181249"
---
# <a name="quickstart-look-up-words-with-bilingual-dictionary-using-c"></a>Avvio rapido: Cercare le parole con il dizionario bilingue usando C#

In questo argomento di avvio rapido verrà illustrato come ottenere traduzioni alternative per un termine e verranno forniti esempi di utilizzo di tali traduzioni usando .NET Core e l'API Traduzione testuale.

Per questo argomento di avvio rapido è necessario avere un [account di Servizi cognitivi di Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con una risorsa Traduzione testuale. Se non si dispone di un account, è possibile usare la [versione di valutazione gratuita](https://azure.microsoft.com/try/cognitive-services/) per ottenere una chiave di sottoscrizione.

## <a name="prerequisites"></a>Prerequisiti

* [.NET SDK](https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)
* [Pacchetto NuGet di Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) o l'editor di testo preferito
* Una chiave di sottoscrizione di Azure per Traduzione testuale

## <a name="create-a-net-core-project"></a>Creare un progetto .NET Core

Aprire un nuovo prompt dei comandi (o una nuova sessione del terminale) ed eseguire i comandi seguenti:

```console
dotnet new console -o alternate-sample
cd alternate-sample
```

Il primo comando esegue due operazioni. Consente di creare una nuova applicazione console .NET e di creare una directory denominata `alternate-sample`. Il secondo comando consente di passare alla directory del progetto.

In seguito sarà necessario installare Json.NET. Dalla directory del progetto eseguire:

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="add-required-namespaces-to-your-project"></a>Aggiungere gli spazi dei nomi obbligatori al progetto

Il comando `dotnet new console`, eseguito in precedenza, ha consentito di creare un progetto, incluso il file `Program.cs`. Si tratta del file in cui verrà inserito il codice dell'applicazione. Aprire il file `Program.cs` e sostituire le istruzioni using seguenti. Queste istruzioni assicurano l'accesso a tutti i tipi necessari per compilare ed eseguire l'app di esempio.

```csharp
using System;
using System.Net.Http;
using System.Text;
using Newtonsoft.Json;
```

## <a name="create-a-function-to-get-alternate-translations"></a>Creare una funzione per ottenere traduzioni alternative

Nella classe `Program` creare una funzione denominata `AltTranslation`. Questa classe incapsula il codice usato per chiamare la risorsa Dictionary e stampa i risultati nella console.

```csharp
static void AltTranslation()
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="set-the-subscription-key-host-name-and-path"></a>Impostare la chiave di sottoscrizione, il nome host e il percorso

Aggiungere le righe seguenti alla funzione `AltTranslation`. Si noterà che oltre a `api-version`, sono stati aggiunti altri due parametri a `route`. Questi parametri vengono usati per impostare la lingua di input e output della traduzione. In questo esempio vengono usate le lingue inglese (`en`) e spagnolo (`es`).

```csharp
string host = "https://api.cognitive.microsofttranslator.com";
string route = "/dictionary/lookup?api-version=3.0&from=en&to=es";
string subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
```

In seguito, è necessario creare e serializzare l'oggetto JSON che include il testo da tradurre. Tenere presente che è possibile passare più di un oggetto nella matrice `body`.

```csharp
System.Object[] body = new System.Object[] { new { Text = @"Elephants" } };
var requestBody = JsonConvert.SerializeObject(body);
```

## <a name="instantiate-the-client-and-make-a-request"></a>Creare un'istanza del client ed effettuare una richiesta

Queste righe consentono di creare un'istanza di `HttpClient` e `HttpRequestMessage`:

```csharp
using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
  // In the next few sections you'll add code to construct the request.
}
```

## <a name="construct-the-request-and-print-the-response"></a>Costruire una richiesta e stampare la risposta

All'interno di `HttpRequestMessage` vengono eseguite le operazioni seguenti:

* Dichiarare il metodo HTTP
* Costruire l'URI della richiesta
* Inserire il corpo della richiesta (oggetto JSON serializzato)
* Aggiungere le intestazioni obbligatorie
* Effettuare una richiesta asincrona
* Stampare la risposta

Aggiungere questo codice a `HttpRequestMessage`:

```csharp
// Set the method to POST
request.Method = HttpMethod.Post;

// Construct the full URI
request.RequestUri = new Uri(host + route);

// Add the serialized JSON object to your request
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");

// Add the authorization header
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send request, get response
var response = client.SendAsync(request).Result;
var jsonResponse = response.Content.ReadAsStringAsync().Result;

// Print the response
Console.WriteLine(PrettyPrint(jsonResponse));
Console.WriteLine("Press any key to continue.");
```

Aggiungere `PrettyPrint` per aggiungere la formattazione alla risposta JSON:
```csharp
static string PrettyPrint(string s)
{
    return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
}
```

## <a name="put-it-all-together"></a>Combinare tutti gli elementi

L'ultimo passaggio consiste nel chiamare `AltTranslation()` nella funzione `Main`. Individuare `static void Main(string[] args)` e aggiungere le righe seguenti:

```csharp
AltTranslation();
Console.ReadLine();
```

## <a name="run-the-sample-app"></a>Eseguire l'app di esempio

A questo punto è possibile eseguire l'app di esempio. Dalla riga di comando, o dalla sessione di terminale, passare alla directory del progetto ed eseguire:

```console
dotnet run
```

## <a name="sample-response"></a>Risposta di esempio

```json
[
    {
        "displaySource": "elephants",
        "normalizedSource": "elephants",
        "translations": [
            {
                "backTranslations": [
                    {
                        "displayText": "elephants",
                        "frequencyCount": 1207,
                        "normalizedText": "elephants",
                        "numExamples": 5
                    }
                ],
                "confidence": 1.0,
                "displayTarget": "elefantes",
                "normalizedTarget": "elefantes",
                "posTag": "NOUN",
                "prefixWord": ""
            }
        ]
    }
]
```

## <a name="clean-up-resources"></a>Pulire le risorse

Assicurarsi di rimuovere eventuali informazioni riservate dal codice sorgente dell'app di esempio, ad esempio le chiavi di sottoscrizione.

## <a name="next-steps"></a>Passaggi successivi

Esaminare il codice di esempio per questo argomento di avvio rapido e per altre, incluse quelle relative alla traslitterazione e all'identificazione della lingua, e anche altri progetti di esempio di Traduzione testuale su GitHub.

> [!div class="nextstepaction"]
> [Esaminare gli esempi di codice C# su GitHub](https://aka.ms/TranslatorGitHub?type=&language=c%23)

## <a name="see-also"></a>Vedere anche 

* [Tradurre un testo](quickstart-csharp-translate.md)
* [Traslitterare testo](quickstart-csharp-transliterate.md)
* [Identificare la lingua da un input](quickstart-csharp-detect.md)
* [Ottenere un elenco di lingue supportate](quickstart-csharp-languages.md)
* [Determinare la lunghezza delle frasi da un input](quickstart-csharp-sentences.md)

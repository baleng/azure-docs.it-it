---
title: 'Guida introduttiva: Ottenere la lunghezza delle frasi, Go - API Traduzione testuale'
titleSuffix: Azure Cognitive Services
description: In questa guida introduttiva si identifica la lunghezza delle frasi nel testo usando l'API Traduzione testuale con Go.
services: cognitive-services
author: erhopf
manager: erhopf
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/21/2019
ms.author: erhopf
ms.openlocfilehash: 441aa1cd0c43b50c8c1212e7ba8840ed97a890d1
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58175829"
---
# <a name="quickstart-use-the-translator-text-api-to-determine-sentence-length-using-go"></a>Guida introduttiva: Usare l'API Traduzione testuale per determinare la lunghezza delle frasi con Go

In questa Guida introduttiva si apprenderà come determinare la lunghezza delle frasi (in caratteri) usando Go e l'API REST Traduzione testuale.

Per questa guida introduttiva è necessario avere un [account di Servizi cognitivi di Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con una risorsa Traduzione testuale. Se non si dispone di un account, è possibile usare la [versione di valutazione gratuita](https://azure.microsoft.com/try/cognitive-services/) per ottenere una chiave di sottoscrizione.

## <a name="prerequisites"></a>Prerequisiti

Questa guida introduttiva richiede:

* [Go](https://golang.org/doc/install)
* Una chiave di sottoscrizione di Azure per Traduzione testuale

## <a name="create-a-project-and-import-required-modules"></a>Creare un progetto e importare i moduli necessari

Creare un nuovo progetto Go usando l'editor o l'IDE preferito. Copiare quindi questo frammento di codice nel progetto all'interno di un file denominato `sentence-length.go`.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
    "os"
)
```

## <a name="create-the-main-function"></a>Creare la funzione main

Questo esempio proverà a leggere la chiave di sottoscrizione di Traduzione testuale dalla variabile di ambiente `TRANSLATOR_TEXT_KEY`. Se non si ha familiarità con le variabili di ambiente, è possibile impostare `subscriptionKey` come stringa e l'istruzione condizionale come commento.

Copiare questo codice nel progetto:

```go
func main() {
    /*
     * Read your subscription key from an env variable.
     * Please note: You can replace this code block with
     * var subscriptionKey = "YOUR_SUBSCRIPTION_KEY" if you don't
     * want to use env variables. If so, be sure to delete the "os" import.
     */
    subscriptionKey := os.Getenv("TRANSLATOR_TEXT_KEY")
    if subscriptionKey == "" {
       log.Fatal("Environment variable TRANSLATOR_TEXT_KEY is not set.")
    }
    /*
     * This calls our breakSentence function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    breakSentence(subscriptionKey)
}
```

## <a name="create-a-function-to-determine-sentence-length"></a>Creare una funzione per determinare la lunghezza delle frasi

È possibile creare una funzione per determinare la lunghezza delle frasi. Questa funzione copre un unico argomento, la chiave di sottoscrizione di Traduzione testuale.

```go
func breakSentence(subscriptionKey string) {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

Successivamente, è possibile creare l'URL. Per creare l'URL vengono usati i metodi `Parse()` e `Query()`. È possibile notare che vengono aggiunti parametri con il metodo `Add()`.

Copiare questo codice nella funzione `breakSentence`.

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse("https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0")
q := u.Query()
q.Add("languages", "en")
u.RawQuery = q.Encode()
```

>[!NOTE]
> Per altre informazioni su endpoint, route e parametri della richiesta, consultare [API Traduzione testuale 3.0: BreakSentence](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence).

## <a name="create-a-struct-for-your-request-body"></a>Creare uno struct per il corpo della richiesta

Creare quindi una struttura anonima per il corpo della richiesta e codificarla in formato JSON con `json.Marshal()`. Aggiungere questo codice alla funzione `breakSentence`.

```go
// Create an anonymous struct for your request body and encode it to JSON
body := []struct {
    Text string
}{
    {Text: "How are you? I am fine. What did you do today?"},
}
b, _ := json.Marshal(body)
```

## <a name="build-the-request"></a>Compilare la richiesta

Ora che il corpo della richiesta è stato codificato in formato JSON, è possibile compilare la richiesta POST e chiamare l'API Traduzione testuale.

```go
// Build the HTTP POST request
req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
if err != nil {
    log.Fatal(err)
}
// Add required headers to the request
req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
req.Header.Add("Content-Type", "application/json")

// Call the Translator Text API
res, err := http.DefaultClient.Do(req)
if err != nil {
    log.Fatal(err)
}
```

## <a name="handle-and-print-the-response"></a>Gestire e stampare la risposta

Aggiungere questo codice alla funzione `breakSentence` per decodificare la risposta JSON, quindi formattare e stampare il risultato.

```go
// Decode the JSON response
var result interface{}
if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
    log.Fatal(err)
}
// Format and print the response to terminal
prettyJSON, _ := json.MarshalIndent(result, "", "  ")
fmt.Printf("%s\n", prettyJSON)
```

## <a name="put-it-all-together"></a>Combinare tutti gli elementi

La procedura è completata. È stato realizzato un semplice programma che chiamerà l'API Traduzione testuale e restituirà una risposta JSON. A questo punto, è possibile eseguire il programma:

```console
go run sentence-length.go
```

Se si vuole confrontare il proprio codice con quello già disponibile, l'esempio completo è pubblicato su [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go).

## <a name="sample-response"></a>Risposta di esempio

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "sentLen": [
      13,
      11,
      22
    ]
  }
]
```

## <a name="next-steps"></a>Passaggi successivi

Esaminare i pacchetti Go per le API Servizi cognitivi da [Azure SDK per Go](https://github.com/Azure/azure-sdk-for-go) su GitHub.

> [!div class="nextstepaction"]
> [Esaminare i pacchetti Go su GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)

## <a name="see-also"></a>Vedere anche 

Informazioni su come usare l'API Traduzione testuale per:

* [Tradurre un testo](quickstart-go-translate.md)
* [Traslitterare testo](quickstart-go-transliterate.md)
* [Identificare la lingua da un input](quickstart-go-detect.md)
* [Ottenere traduzioni alternative](quickstart-go-dictionary.md)
* [Ottenere un elenco di lingue supportate](quickstart-go-languages.md)

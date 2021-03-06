---
title: Utilizzare il riconoscimento vocale | Microsoft Docs
description: Informazioni su come usare il Riconoscimento vocale nel servizio Voce
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: 26cecedfc3ad2d472b9686e25054fe08253cee77
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39068523"
---
# <a name="use-speech-to-text-in-the-speech-service"></a>Usare il "Riconoscimento vocale" nel servizio Voce

È possibile utilizzare il **Riconoscimento vocale** nelle applicazioni in due modi diversi.

| Metodo | DESCRIZIONE |
|-|-|
| [SDK](speech-sdk.md) | Metodo più semplice per gli sviluppatori C/C++, C# e Java |
| [REST](rest-apis.md) | Riconoscere brevi espressioni utilizzando una richiesta HTTP POST | 

## <a name="using-the-sdk"></a>Uso dell'SDK

[Speech SDK](speech-sdk.md) fornisce il modo più semplice di utilizzare il **Riconoscimento vocale** nell'applicazione con tutte le funzionalità.

1. Creare una factory di riconoscimento vocale, fornendo una chiave di sottoscrizione al servizio Voce e una [regione](regions.md) o un token di autorizzazione. È anche possibile configurare opzioni, come la lingua di riconoscimento o un endpoint personalizzato per i propri modelli di riconoscimento vocale.

2. Ottenere un sistema di riconoscimento dalla factory. Sono disponibili tre diversi tipi di sistemi di riconoscimento. Ogni tipo di sistema di riconoscimento può utilizzare il microfono predefinito del dispositivo, un flusso audio o l’audio da un file.

    Sistema di riconoscimento | Funzione
    -|-
    Riconoscimento vocale|Fornisce la trascrizione in testo dei comandi vocali
    Sistema di riconoscimento delle finalità|Deriva la finalità di chi parla tramite [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/) dopo il riconoscimento\*
    Sistema di riconoscimento di traduzione|Traduce il testo trascritto in un'altra lingua (vedere [Traduzione vocale](how-to-translate-speech.md))

    \* *Per il riconoscimento delle finalità, è necessario utilizzare un codice di sottoscrizione LUIS separato per la creazione di un factory di riconoscimento vocale per il riconoscimento delle finalità.*
    
4. Collegare gli eventi per il funzionamento asincrono, se desiderato. Il sistema di riconoscimento chiama quindi i gestori dell'evento quando hanno risultati intermedi e finali. In caso contrario, l'applicazione riceverà un risultato finale di trascrizione.

5. Avviare il riconoscimento.
   Per il riconoscimento singolo, come il riconoscimento di un comando o di una query, usare `RecognizeAsync()`, che restituisce il primo utterance riconoscimento.
   Per il riconoscimento a esecuzione prolungata, come la trascrizione, usare `StartContinuousRecognitionAsync()` e correlare gli eventi per risultati del riconoscimento asincrono.

### <a name="sdk-samples"></a>Esempi di SDK

Per il set di esempi più recente, vedere il [repository GitHub degli esempi di Speech SDK di Servizi cognitivi](https://aka.ms/csspeech/samples).

## <a name="using-the-rest-api"></a>Utilizzo dell'API REST

L'API REST è il metodo più semplice di riconoscimento vocale se non si utilizza una lingua supportata dall’SDK. Si effettua una richiesta HTTP POST all'endpoint del servizio, passando l'intera espressione nel corpo della richiesta. Si riceve una risposta che contiene il testo riconosciuto.

> [!NOTE]
> Le espressioni sono limitate a 15 secondi o meno quando si utilizza l'API REST.

Per altre informazioni sull'API REST di **riconoscimento vocale**, vedere [API REST](rest-apis.md#speech-to-text). Per visualizzarlo in azione, scaricare gli [esempi di API REST](https://github.com/Azure-Samples/SpeechToText-REST) da GitHub.

## <a name="next-steps"></a>Passaggi successivi

- [Ottenere una sottoscrizione di valutazione gratuita del Servizio di riconoscimento vocale](https://azure.microsoft.com/try/cognitive-services/)
- [Riconoscimento vocale in C++](quickstart-cpp-windows.md)
- [Riconoscimento vocale in C#](quickstart-csharp-dotnet-windows.md)
- [Riconoscimento vocale in Java](quickstart-java-android.md)

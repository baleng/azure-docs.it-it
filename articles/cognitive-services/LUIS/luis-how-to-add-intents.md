---
title: Aggiungere finalità
titleSuffix: Language Understanding - Azure Cognitive Services
description: Aggiungi le finalità all'app LUIS per identificare gruppi di domande o comandi con gli stessi scopi.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.subservice: language-understanding
ms.topic: article
ms.date: 02/19/2019
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: 0d6813c11b5b9ed4aef666f5eb3dc75d8c9de770
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/21/2019
ms.locfileid: "56593423"
---
# <a name="add-intents-to-determine-user-intention-of-utterances"></a>Aggiungere finalità per determinare l'intenzione delle espressioni dell'utente

Aggiungi le [finalità](luis-concept-intent.md) all'app LUIS per identificare gruppi di domande o comandi con lo stesso scopo. 

Le finalità sono gestite nella sezione **Compilazione** sulla barra di navigazione superiore nel pannello **Finalità** a sinistra. 

## <a name="add-intent"></a>Aggiungere le finalità

1. Nella pagina **Intents** (Finalità) selezionare **Create new intent** (Crea nuova finalità).

1. Nella finestra di dialogo **Crea nuova finalità**, inserire il nome della finalità, `GetEmployeeInformation`, quindi fare clic su **Fine**.

    ![Aggiungere le finalità](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

## <a name="add-an-example-utterance"></a>Aggiungere un'espressione di esempio

Le espressioni di esempio sono campioni di testo con domande o commenti degli utenti. Per l'apprendimento di LUIS (Language Understanding), è necessario aggiungere espressioni di esempio a una finalità.

1. Nella pagina dei dettagli delle finalità **GetEmployeeInformation**, inserire un'espressione pertinente che gli utenti potrebbero usare, ad esempio `Does John Smith work in Seattle?` nella casella di testo sotto al nome della finalità, quindi premere Invio.
 
    ![Schermata della pagina dei dettagli delle finalità, con un'espressione evidenziata](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

    LUIS converte tutte le espressioni in lettere minuscole e aggiunge spazi prima e dopo i token come i trattini.

## <a name="intent-prediction-discrepancy-errors"></a>Errori di discrepanza nella stima delle finalità 

Un'espressione in una finalità potrebbe presentare una discrepanza nella stima delle finalità tra la finalità selezionata e il punteggio della stima. LUIS indica questa discrepanza con una casella rossa intorno alla **finalità etichettata** sulla riga dell'espressione di esempio. 

![Screenshot della pagina dei dettagli delle finalità, con errori di discrepanza nella stima delle espressioni](./media/luis-how-to-add-intents/prediction-discrepancy-intent.png) 

Selezionare **Eseguire il training** nella barra di spostamento superiore. La discrepanza di stima è adesso assente.

> [!Note]
> Se una riga rossa sotto una parola o frase in utterance l'esempio, un' [errore di previsione entità](luis-how-to-add-example-utterances.md#entity-status-predictions) si è verificato. È necessario correggere l'errore. 

## <a name="add-a-custom-entity"></a>Aggiungere un'entità personalizzata

Dopo l'aggiunta di un'espressione a una finalità, è possibile selezionare un testo all'interno dell'espressione per creare un'entità personalizzata. Un'entità personalizzata è un modo per assegnare un tag al testo da estrarre, insieme alla finalità corretta. 

Visualizzare [aggiungere entità a utterance](luis-how-to-add-example-utterances.md) per altre informazioni.

## <a name="entity-prediction-discrepancy-errors"></a>Errori di discrepanza nella stima delle entità 

L'entità è sottolineata in rosso per indicare una [discrepanza nella stima dell'entità](luis-how-to-add-example-utterances.md#entity-status-predictions). Poiché questa è la prima occorrenza di un'entità, non ci sono esempi a sufficienza perché LUIS abbia la certezza che questo testo sia contrassegnato con il tag dell'entità corretta. Questa discrepanza viene rimossa quando l'app è sottoposta al training. 

![Schermata della pagina dei dettagli delle finalità, nome dell'entità personalizzata evidenziato in blu](./media/luis-how-to-add-intents/create-custom-entity-name-blue-highlight.png) 

Il testo evidenziato in blu indica un'entità.  

## <a name="add-a-prebuilt-entity"></a>Aggiungere un'entità predefinita

Per altre informazioni, vedere [Entità predefinita](luis-how-to-add-entities.md#add-prebuilt-entity).

## <a name="using-the-contextual-toolbar"></a>Usare la barra degli strumenti contestuale

Quando uno o più espressioni di esempio sono selezionati nell'elenco, selezionando la casella a sinistra di un utterance, la barra degli strumenti sopra l'elenco utterance consente di eseguire le azioni seguenti:

* Riassegnare la finalità: spostare le espressioni per assegnare finalità diverse
* Eliminare espressioni
* Filtri di entità: mostra solo espressioni contenenti le entità filtrate
* Mostra tutto/Solo errori: mostra espressioni con errori di stima o tutte le espressioni
* Visualizzazione di entità/token: mostra le entità con i relativi nomi o il testo non elaborato dell'espressione
* Lente di ingrandimento: ricerca di espressioni contenenti testo specifico

## <a name="working-with-an-individual-utterance"></a>Lavorare su una singola espressione

È possibile eseguire le azioni seguenti su una singola espressione dall'icona dei tre punti a destra dell'espressione:

* Modifica: cambia il testo dell'espressione
* Elimina: rimuove l'espressione dalla finalità. Se si desidera ancora l'espressione, una soluzione migliore consiste nel spostarla nella finalità **Nessuna**. 
* Aggiungere un criterio: un criterio consente di prendere un'espressione comune e contrassegnare il testo sostituibile e ignorabile, riducendo così la necessità di altre espressioni nella finalità. 

La colonna **Finalità etichettata** consente di modificare la finalità dell'espressione.

## <a name="train-your-app-after-changing-model-with-intents"></a>Eseguire il training dell'app dopo la modifica del modello con le finalità

Dopo aver aggiunto, modificato o rimosso le finalità, [eseguire il training](luis-how-to-train.md) e [pubblicare](luis-how-to-publish-app.md) l'app per applicare le modifiche alle query di endpoint. 

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sull'aggiunta di [espressioni di esempio](luis-how-to-add-example-utterances.md) con entità. 

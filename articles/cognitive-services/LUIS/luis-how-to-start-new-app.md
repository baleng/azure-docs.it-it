---
title: Creare una nuova app
titleSuffix: Language Understanding - Azure Cognitive Services
description: Creare e gestire le applicazioni nella pagina Web di Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/28/2019
ms.author: diberry
ms.openlocfilehash: 72c4f23f47e0a2c6d9a96dbbe36716bc3ab665f1
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/03/2019
ms.locfileid: "58891430"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>Creare una nuova app LUIS nel portale LUIS
Un'app LUIS può essere creata in due modi. È possibile creare un'app LUIS nel portale [LUIS](https://www.luis.ai) o tramite le [API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) di creazione di LUIS.

## <a name="using-the-luis-portal"></a>Usare il portale LUIS
È possibile creare una nuova app nel portale di LUIS in diversi modi:

* Iniziare con un'applicazione vuota e creare finalità, espressioni ed entità.
* Iniziare con un'applicazione vuota e aggiungere un [dominio predefinito](luis-how-to-use-prebuilt-domains.md).
* Importare un'applicazione LUIS da un file JSON che contiene già finalità, espressioni ed entità.

## <a name="using-the-authoring-apis"></a>Usare le API di creazione
È possibile creare una nuova app con le API di creazione in due modi:

* [Inizia](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) con un'applicazione vuota e crea finalità, espressioni ed entità.
* [Inizia](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/59104e515aca2f0b48c76be5) con un dominio predefinito.  


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>
 

## <a name="create-new-app-in-luis"></a>Creare la nuova appa in LUIS

1. Nella pagina **App personali**, seleziona **Crea una nuova applicazione**.

    ![Elenco di app di Language Understanding](./media/luis-create-new-app/apps-list.png)


2. Nella finestra di dialogo, assegna all'applicazione il nome di "TravelAgent".

    ![Creare una nuova finestra di dialogo dell'applicazione](./media/luis-create-new-app/create-app.png)

3. Scegli le impostazioni cultura dell'applicazione (per l'applicazione TravelAgent, scegli la lingua inglese), quindi seleziona **Fine**. 

    > [!NOTE]
    > Le impostazioni cultura non possono essere modificate dopo la creazione dell'applicazione. 

## <a name="import-an-app-from-file"></a>Importare un'app da file

1. Nella pagina **App personali**, seleziona **Importa una nuova applicazione**.
1. Nella finestra di dialogo popup, selezionare un file JSON dell'app valido, quindi ****.

### <a name="import-errors"></a>Errori di importazione

Gli errori possibili sono: 

* Un'app con lo stesso nome esiste già. Reimportare le app e impostare il **nome facoltativo** a un nuovo nome. 

## <a name="export-app"></a>Esportare l'applicazione

1. Nella pagina **App personali**, seleziona **Importa una nuova applicazione**.
1. Nella finestra di dialogo **Importa una nuova applicazione**, seleziona il file JSON che definisce l'applicazione LUIS.

## <a name="delete-app"></a>Eliminare l'applicazione

1. Nella pagina **App personali** seleziona i tre punti di sospensione (...) alla fine della riga dell'applicazione.
1. Seleziona **Elimina** dal menu.
1. Nella finestra di conferma fai clic su **Ok**.

## <a name="next-steps"></a>Passaggi successivi

La prima attività dell'applicazione consiste nell'[aggiungere finalità](luis-how-to-add-intents.md).

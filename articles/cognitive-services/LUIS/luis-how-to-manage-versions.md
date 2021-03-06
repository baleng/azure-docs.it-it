---
title: Gestire le versioni
titleSuffix: Language Understanding - Azure Cognitive Services
description: Le versioni consentono di compilare e pubblicare modelli diversi. Una procedura consigliata consiste nel clonare il modello attualmente attivo in una versione diversa dell'app prima di apportare modifiche al modello.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/29/2019
ms.author: diberry
ms.openlocfilehash: dfe23baa67c87b04a65630611ef71758beda268d
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2019
ms.locfileid: "58754895"
---
# <a name="use-versions-to-edit-and-test-without-impacting-staging-or-production-apps"></a>Usare le versioni per eseguire modifiche e test senza conseguenze per le app di staging o produzione

Le versioni consentono di compilare e pubblicare modelli diversi. Una procedura consigliata consiste nel clonare il modello attualmente attivo in una [versione](luis-concept-version.md) diversa dell'app prima di apportare modifiche al modello. 

Per usare le versioni, aprire l'app selezionando il relativo nome nella pagina **Mie app**, quindi selezionare **Gestisci** nella barra superiore e successivamente **Versioni** nella sezione di navigazione a sinistra. 

L'elenco delle versioni mostra le versioni pubblicate, dove vengono pubblicate e la versione attualmente attiva. 

[![Sezione relativa alla gestione della pagina delle versioni](./media/luis-how-to-manage-versions/versions-import.png "Sezione relativa alla gestione della pagina delle versioni")](./media/luis-how-to-manage-versions/versions-import.png#lightbox)

## <a name="clone-a-version"></a>Clonare una versione

1. Selezionare la versione che si vuole clonare, quindi selezionare **Clone** nella barra degli strumenti. 

2. Nella finestra di dialogo **Clone version** (Clona versione) digitare un nome per la nuova versione, ad esempio "0.2".

   ![Finestra di dialogo Clone Version (Clona versione)](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
     > [!NOTE]
     > L'ID della versione può essere composto solo da caratteri, cifre o '.' e non può superare i 10 caratteri.
 
   Verrà creata una nuova versione con il nome specificato, che verrà impostata come versione attiva.

## <a name="set-active-version"></a>Impostare la versione attiva

Selezionare una versione dall'elenco, quindi selezionare **Attiva** nella barra degli strumenti. 

[![Sezione relativa alla gestione della pagina delle versioni, esecuzione di un'azione relativa alla versione](./media/luis-how-to-manage-versions/versions-other.png "Sezione relativa alla gestione della pagina delle versioni, esecuzione di un'azione relativa alla versione")](./media/luis-how-to-manage-versions/versions-other.png#lightbox)

## <a name="import-version"></a>Importare la versione

1. Selezionare **Importa versione** nella barra degli strumenti. 

2. Nel comando **Importa nuova versione** della finestra popup, immettere il nuovo nome della versione di dieci caratteri. È necessario impostare un ID della versione solo se la versione nel file JSON è già presente nell'app.

    ![Sezione relativa alla gestione della pagina delle versioni, importazione di una nuova versione](./media/luis-how-to-manage-versions/versions-import-pop-up.png)

    Una volta importata una versione, la nuova versione diventa quella attiva.

### <a name="import-errors"></a>Errori di importazione

* Errori di tokenizer: Se si verifica una **errore tokenizer** durante l'importazione, si sta cercando di importare una versione che utilizza una diversa [tokenizer](luis-language-support.md#custom-tokenizer-versions) rispetto a quella dell'app è attualmente utilizzata. Per risolvere questo problema, vedere [eseguire la migrazione tra versioni diverse di tokenizer](luis-language-support.md#migrating-between-tokenizer-versions).

<a name = "export-version"></a>

## <a name="other-actions"></a>Altre azioni

* Per **eliminare** una versione, selezionare una versione dall'elenco, quindi selezionare **Elimina** nella barra degli strumenti. Selezionare **OK**. 
* Per **eliminare** una versione, selezionare una versione dall'elenco, quindi selezionare **Rinomina** nella barra degli strumenti. Immettere un nuovo nome e selezionare **Fatto**. 
* Per **esportare** una versione, selezionare una versione dall'elenco, quindi selezionare **Esporta app** nella barra degli strumenti. Il file sarà scaricato sul computer locale. 


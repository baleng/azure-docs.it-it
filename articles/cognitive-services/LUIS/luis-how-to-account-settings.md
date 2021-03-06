---
title: Gestire l'account e le chiavi
titleSuffix: Language Understanding - Azure Cognitive Services
description: Le informazioni principali di un account LUIS sono l'account utente e la chiave di creazione. Le informazioni di accesso viene gestite in account.microsoft.com. La creazione e modifica chiave è gestita dalla pagina Impostazioni del portale LUIS.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: d7d63ad642ab2d3b336e15dcca606077762ceb9d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58116623"
---
# <a name="manage-account-and-authoring-key"></a>Gestire l'account e la chiave di creazione

Le informazioni principali di un account LUIS sono l'account utente e la chiave di creazione. Le informazioni di accesso sono gestite in [account.microsoft.com](https://account.microsoft.com). La creazione e modifica chiave è gestita dal [LUIS](luis-reference-regions.md) portal **impostazioni** pagina.

## <a name="authoring-key"></a>Chiave di creazione

Questo singolo, specifico dell'area di creazione chiave, nel **impostazioni** pagina, è possibile creare tutte le app dal [LUIS](luis-reference-regions.md) del portale, nonché il [API di creazione](https://aka.ms/luis-authoring-api). Per praticità, la chiave di creazione è autorizzata a creare un numero [limitato](luis-boundaries.md) di query di endpoint al mese.

[![Pagina delle impostazioni di LUIS](./media/luis-how-to-account-settings/account-settings.png)](./media/luis-how-to-account-settings/account-settings.png#lightbox)

La chiave di creazione viene usata per le app di cui si è titolari e per le app di cui si è collaboratori.

## <a name="authoring-key-regions"></a>Aree delle chiavi di creazione

La chiave di creazione è specifica dell'[area di creazione](luis-reference-regions.md#publishing-regions). La chiave non può essere usata in un'area diversa.

## <a name="reset-authoring-key"></a>Reimpostare la chiave di creazione

Se la chiave di creazione è danneggiata, reimpostare la chiave. La chiave viene reimpostata su tutte le app nel [LUIS](luis-reference-regions.md) portale. Se le app vengono create mediante le API di creazione, è necessario modificare il valore di `Ocp-Apim-Subscription-Key` con la nuova chiave.

## <a name="delete-account"></a>Eliminare l'account

Per informazioni sui dati che vengono eliminati quando viene eliminato l'account, vedere [Data storage and removal](luis-concept-data-storage.md#accounts) (Archiviazione e rimozione dei dati).

## <a name="next-steps"></a>Passaggi successivi

Altre informazioni sulla [chiave di creazione](luis-concept-keys.md#authoring-key).


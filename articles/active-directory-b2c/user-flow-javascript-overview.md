---
title: JavaScript e versioni del contratto di pagina per i flussi utente in Azure Active Directory B2C | Microsoft Docs
description: Informazioni su come abilitare JavaScript e usare le versioni del contratto di pagina per personalizzare un flusso utente in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 5102755c9e830f43fa92e8546e5125960e0a2f9a
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401548"
---
# <a name="about-using-javascript-and-page-contract-versions-in-a-user-flow"></a>Informazioni sull'uso di JavaScript e sulle versioni del contratto di pagina in un flusso utente

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure AD B2C offre un set di contenuto a pacchetti che include codice HTML, CSS e JavaScript per gli elementi dell'interfaccia utente nei flussi utente. Se si intende abilitare il codice [JavaScript](javascript-samples.md) lato client nei flussi utente, è opportuno assicurarsi che gli elementi su cui si basa JavaScript non siano modificabili. In caso contrario, tutte le modifiche potrebbero causare un comportamento imprevisto nelle pagine del flusso utente. Per evitare questi problemi, è possibile imporre l'uso di un contratto di pagina per un flusso utente e specificare una versione del contratto di pagina. Questa operazione assicura che tutte le definizioni del contenuto su cui si basa JavaScript non siano modificabili. Anche se non si prevede di abilitare JavaScript per un flusso utente, è possibile specificare una versione del contratto di pagina per le pagine del flusso utente.

> [!NOTE]
> Questo articolo descrive JavaScript per i flussi utente, ma è anche possibile usare JavaScript e selezionare le versioni del contratto di pagina quando si usano [criteri personalizzati](page-contract.md).

## <a name="enable-javascript"></a>Abilitare JavaScript

Nelle proprietà del flusso utente, è possibile abilitare JavaScript, operazione che impone anche l'uso di un contratto di pagina. È quindi possibile impostare la versione del contratto di pagina come descritto nella sezione seguente.

![Abilitare le impostazioni di JavaScript](media/user-flow-javascript-overview/javascript-settings.PNG)

## <a name="specify-a-page-contract-version"></a>Specificare una versione del contratto di pagina

Sia che si preveda o meno di abilitare JavaScript nelle proprietà del flusso utente, è possibile specificare una versione del contratto di pagina per le pagine del flusso utente. Aprire il flusso utente e selezionare **Layout di pagina**. In **Nome layout**, selezionare una pagina di flusso utente e scegliere la **Versione del contratto di pagina**.

![Abilitare le impostazioni di JavaScript](media/user-flow-javascript-overview/page-contract-version.PNG)

## <a name="next-steps"></a>Passaggi successivi
Vedere gli [Esempi JavaScript da usare in Azure Active Directory B2C](javascript-samples.md).

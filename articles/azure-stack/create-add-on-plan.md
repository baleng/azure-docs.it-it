---
title: In questo articolo descrive come aggiornare i piani e offerte di Azure Stack | Microsoft Docs
description: Questo articolo descrive come visualizzare e modificare le offerte di Azure Stack e i piani esistenti.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.date: 03/07/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 03/07/2019
ms.openlocfilehash: 00bb17eadfee32e9b0d006ac76bb8e1cd614f13e
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/12/2019
ms.locfileid: "57780389"
---
# <a name="azure-stack-add-on-plans"></a>Piani aggiuntivi di Azure Stack

Un operatore di Azure Stack, si creare piani aggiuntivi per modificare un [piano di base](azure-stack-create-plan.md) quando si desidera offrire servizi aggiuntivi o estendere *computer*, *archiviazione*, oppure *rete* quote oltre l'offerta iniziale piano base. Piani aggiuntivi modificare il piano di base e sono le estensioni facoltative che gli utenti possono scegliere di sottoscrivere.

Vi sono casi quando la combinazione di tutti gli elementi in un singolo piano è ottima. In altri casi è consigliabile avere una base piano e quindi offrire i servizi aggiuntivi utilizzando piani aggiuntivi. Ad esempio, è possibile decidere di offrire servizi IaaS come parte di un piano di base, con tutti i servizi PaaS considerati come i piani aggiuntivi.

Un altro motivo per usare i piani aggiuntivi è consentono di monitorare l'utilizzo delle risorse. A tale scopo, è possibile iniziare con un piano di base che include le quote di dimensioni relativamente ridotte (a seconda dei servizi necessarie). Quindi, come gli utenti raggiungono la capacità, si sarebbe un avviso che hanno stato utilizzato l'allocazione delle risorse in base al piano assegnato. Da qui, gli utenti possono selezionare un piano aggiuntivo che fornisce le risorse aggiuntive.

> [!NOTE]
> Quando non si desidera usare un piano del componente aggiuntivo per estendere una quota, è possibile anche effettuare [modificare la configurazione originale della quota](azure-stack-quota-types.md#edit-a-quota).

Quando si aggiunge un piano del componente aggiuntivo a una sottoscrizione di offerta esistente, le risorse aggiuntive possono richiedere fino a un'ora da visualizzare.

Piani aggiuntivi vengono creati tramite la modifica di un'offerta esistente.

## <a name="create-an-add-on-plan-1902-and-later"></a>Creare un piano aggiuntivo (1902 e versioni successive)

1. Accedere al portale di amministrazione di Azure Stack come amministratore del cloud.
2. Seguire la stessa procedura usata per [creare un nuovo piano base](azure-stack-create-plan.md) per creare un nuovo piano di offerta di servizi che non sono stati offerti in precedenza.
3. Nel portale di amministrazione, fare clic su **offre** e quindi selezionare l'offerta venga aggiornato con un piano del componente aggiuntivo.

   ![Crea piano aggiuntivo](media/create-add-on-plan/add-on1.png)

4. Scorrere verso il basso le proprietà di offerta e selezionare **piani aggiuntivi**. Fare clic su **Aggiungi**.

    ![Crea piano aggiuntivo](media/create-add-on-plan/add-on2.png)

5. Selezionare il piano da aggiungere. In questo esempio, il piano viene chiamato **20 storageaccounts**. Dopo aver selezionato il piano, fare clic su **seleziona** per aggiungere il piano all'offerta. Si dovrebbe ricevere una notifica che il piano è stato aggiunto all'offerta.

    ![Crea piano aggiuntivo](media/create-add-on-plan/add-on3.png)

6. Esaminare l'elenco di piani aggiuntivi inclusi con l'opzione per verificare che sia elencato il nuovo piano di componente aggiuntivo.

    [![Crea piano di componente aggiuntivo](media/create-add-on-plan/add-on4.png "crea piano di componente aggiuntivo")](media/create-add-on-plan/add-on4lg.png#lightbox)

## <a name="create-an-add-on-plan-1901-and-earlier"></a>Creare un piano aggiuntivo (1901 e precedenti)

1. Accedere al portale di amministrazione di Azure Stack come amministratore del cloud.
2. Seguire la stessa procedura usata per [creare un nuovo piano base](azure-stack-create-plan.md) per creare un nuovo piano di offerta di servizi che non sono stati offerti in precedenza. In questo esempio, Key Vault (**Microsoft. keyvault**) i servizi verranno inclusi nel nuovo piano.
3. Nel portale di amministrazione, fare clic su **offre** e quindi selezionare l'offerta venga aggiornato con un piano del componente aggiuntivo.

   ![Crea piano aggiuntivo](media/create-add-on-plan/1.PNG)

4. Scorrere verso il basso le proprietà di offerta e selezionare **piani aggiuntivi**. Fare clic su **Aggiungi**.

    ![Crea piano aggiuntivo](media/create-add-on-plan/2.PNG)

5. Selezionare il piano da aggiungere. In questo esempio, il piano viene chiamato **piano di insieme di credenziali delle chiavi**. Dopo aver selezionato il piano, fare clic su **seleziona** per aggiungere il piano all'offerta. Si dovrebbe ricevere una notifica che il piano è stato aggiunto all'offerta.

    ![Crea piano aggiuntivo](media/create-add-on-plan/3.PNG)

6. Esaminare l'elenco di piani aggiuntivi inclusi con l'opzione per verificare che sia elencato il nuovo piano di componente aggiuntivo.

    ![Crea piano aggiuntivo](media/create-add-on-plan/4.PNG)

## <a name="next-steps"></a>Passaggi successivi

* [Creare un'offerta](azure-stack-create-offer.md)
---
title: Usare la convalida come servizio per il portale di Azure Stack per pianificare il primo test | Microsoft Docs
description: Utilizzare la convalida come servizio per il portale di Azure Stack per pianificare il primo test.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ms.openlocfilehash: 3fb5c3deeddb6f3ee381ca45df76feebf3405b21
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/12/2019
ms.locfileid: "57766570"
---
# <a name="scheduling-a-test"></a>Pianificazione di un test

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Pianificare un test di convalida come portale del servizio (VaaS) per la soluzione di Azure Stack. Una soluzione VaaS rappresenta una soluzione di Azure Stack con hardware specifiche di una distinta base (BoM). È possibile pianificare un test per verificare che l'hardware è possibile eseguire Azure Stack.

Per verificare la soluzione, creare il flusso di lavoro per un test. Un flusso di lavoro VaaS opera all'interno del contesto di una soluzione VaaS. Rappresenta un set di gruppi di test che verificano la funzionalità di una distribuzione di Azure Stack nell'hardware. Aggiungere i parametri dell'ambiente della soluzione e selezionare uno o più test da eseguire sulla tua soluzione.

Mentre il flusso di lavoro di superamento Test può essere utilizzato per eseguire qualsiasi test forniti da VaaS, inclusi i test da flussi di lavoro di convalida, i risultati dal flusso di lavoro superamento Test non sono considerati *ufficiale*. Per informazioni sui flussi di lavoro di convalida ufficiale, vedere [flussi di lavoro](azure-stack-vaas-key-concepts.md#workflows).

## <a name="prerequisites"></a>Prerequisiti

Prima di seguire questa Guida introduttiva, è necessario terminare gli elementi seguenti:

- [Configurare la convalida come delle risorse del servizio](azure-stack-vaas-set-up-resources.md)
- [Distribuire l'agente locale](azure-stack-vaas-local-agent.md) (obbligatorio)
- [Convalida di un concetti chiave di servizio](azure-stack-vaas-key-concepts.md) (obbligatorio)

## <a name="start-a-workflow"></a>Avviare un flusso di lavoro

![Accedere al portale di VaaS](media/vaas_portalsignin.png)

Accedere al portale, selezionare o creare una soluzione e quindi selezionare la soluzione.

1. Accedi per il [VaaS portale](https://azurestackvalidation.com).
2. Digitare il nome di una soluzione esistente o selezionare **nuova soluzione** per creare una nuova soluzione. Per istruzioni, vedere [creare una soluzione nel portale di VaaS](azure-stack-vaas-key-concepts.md#create-a-solution-in-the-vaas-portal).
3. Selezionare **avviare** nel **Test superati** riquadro.

## <a name="specify-parameters"></a>Specificare i parametri

![Testo alternativo](media/vaas_test_pass_parameters.png)

Specificare i parametri che si applicano a tutti i test all'interno del flusso di lavoro.

1. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]
2. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]
3. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]
4. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]
5. Selezionare **successivo** per selezionare i test da pianificare.

## <a name="select-tests-to-run"></a>Selezionare i test da eseguire

I test selezionati verranno pianificati una volta creato il flusso di lavoro.

1. Selezionare i test da eseguire nel flusso di lavoro.

    Se si desidera sostituire i parametri comuni (vale a dire, i parametri forniti nella sezione precedente) per qualsiasi test, scegliere il **modifica** collegamento accanto a specificare nuovi valori.

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-agent](includes/azure-stack-vaas-workflow-step_select-agent.md)]

1. Selezionare **successivo** per esaminare il flusso di lavoro.

## <a name="review-and-submit"></a>Rivedi e invia

Completare la creazione del flusso di lavoro.

1. Esaminare le informazioni visualizzate.

    Il servizio crea il flusso di lavoro con le informazioni fornite e i test selezionati verranno pianificati.

    Se qualsiasi elemento non viene visualizzata correttamente, usare il **Previous** i pulsanti per passare a una sezione precedente.

1. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]

## <a name="next-steps"></a>Passaggi successivi

- [Monitorare e gestire i test nel portale di VaaS](azure-stack-vaas-monitor-test.md)

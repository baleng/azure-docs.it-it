---
title: Accedere ad Azure Notebooks
description: Accedere rapidamente ad Azure Notebooks e impostare un ID utente per poter accedere ai progetti salvati e condividere notebook con altri utenti.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: fb8c94b1-6d0a-4b77-8d14-ae6efcdd99f4
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2019
ms.author: kraigb
ms.openlocfilehash: f3effc900b79ddb7beac6a3aaf2eee0a264f7b4d
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280860"
---
# <a name="quickstart-sign-in-and-set-a-user-id"></a>Avvio rapido: Accedere e impostare un ID utente

Sebbene sia sempre possibile visualizzare Azure Notebooks senza eseguire l'accesso, è necessario accedere per eseguire i notebook, accedere ai notebook e ai progetti salvati e condividere i notebook con altri utenti.

## <a name="sign-in"></a>Accesso

1. Selezionare **Accedi** in alto a destra nella pagina [notebooks.azure.com](https://notebooks.azure.com/).

    ![Posizione del comando Accedi su Azure Notebooks](media/accounts/sign-in-command.png)

1. Quando richiesto, immettere l'indirizzo di posta elettronica di un account Microsoft o di un account aziendale o dell'istituto di istruzione e selezionare **Avanti**. I tipi di account sono descritti in [Account utente per Azure Notebooks](azure-notebooks-user-account.md). Se non si possiede un account Microsoft, o per crearne uno da usare in modo specifico con Azure Notebooks, selezionare **Creane uno**:

    ![Comando Crea un nuovo account Microsoft nel prompt di accesso](media/accounts/create-new-microsoft-account.png)

1. Immettere la password quando viene richiesto.

1. Se si esegue l'accesso per la prima volta, Azure Notebooks chiede l'autorizzazione per accedere all'account. Selezionare **Sì** per continuare:

    ![Prompt delle autorizzazioni dell'account](media/accounts/account-permission-prompt.png)

## <a name="set-a-user-id"></a>Impostare un ID utente

1. Al primo accesso, all'utente viene assegnato un ID utente temporaneo, ad esempio "anon-idrca3". Se si dispone di un ID utente che inizia con "anon-", Azure Notebooks richiede di creare un ID personale. L'ID utente viene usato in qualsiasi URL ottenuto per condividere progetti e notebook. È quindi consigliabile scegliere un nome univoco e significativo.

    ![Prompt per l'inserimento di un ID utente per Azure Notebooks](media/accounts/create-user-id.png)

    Se si seleziona **No grazie**, Azure Notebooks continua a richiedere un ID utente ogni volta che si accede. L'ID utente può essere anche impostato in qualsiasi momento nel [profilo utente](azure-notebooks-user-profile.md).

1. Dopo aver eseguito l'accesso, Azure Notebooks consente di passare alla pagina del profilo pubblico, in cui è possibile selezionare **Modifica informazioni del profilo** per compilare il resto delle informazioni dell'utente (per altre informazioni, vedere [Profilo e ID utente](azure-notebooks-user-profile.md)):

    ![Vista iniziale di una pagina del profilo Azure Notebooks](media/accounts/profile-page-new.png)

> [!NOTE]
> Se viene visualizzato il messaggio, "ID utente è già in uso," provare un altro ID. Gli ID utente sono univoci per tutti gli account Azure Notebooks e Azure Notebooks riserva anche determinati gli ID utente, ad esempio nomi di marchi di Microsoft.

## <a name="sign-out"></a>Disconnessione

Per disconnettersi, selezionare il nome utente nell'angolo in alto a destra della pagina e quindi selezionare **Esci**:

![Posizione del comando Esci su Azure Notebooks](media/accounts/sign-out-command.png)

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Guida introduttiva: Creare e condividere un notebook](quickstart-create-share-jupyter-notebook.md)

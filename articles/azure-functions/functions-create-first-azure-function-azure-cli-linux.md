---
title: Creare la prima funzione ospitata in Linux in Azure
description: Informazioni su come creare in Azure la prima funzione ospitata in Linux usando Azure Functions Core Tools e l'interfaccia della riga di comando di Azure.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 03/12/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc, fasttrack-edit
ms.devlang: javascript
manager: jeconnoc
ms.openlocfilehash: 78c2f599ba7d22e6de070f5867398e111a396d45
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "57849738"
---
# <a name="create-your-first-function-hosted-on-linux-using-core-tools-and-the-azure-cli-preview"></a>Creare la prima funzione ospitata in Linux usando Core Tools e l'interfaccia della riga di comando di Azure (anteprima)

Funzioni di Azure consente di eseguire il codice in un ambiente Linux [senza server](https://azure.com/serverless), senza dover prima creare una macchina virtuale o pubblicare un'applicazione Web. L'hosting in Linux richiede il [runtime di Functions 2.0](functions-versions.md). Il supporto per l'esecuzione di un'app per le funzioni in Linux nel [piano a consumo](functions-scale.md#consumption-plan) serverless è attualmente disponibile in anteprima. Per altre informazioni, vedere [questo articolo con considerazioni sull'anteprima](https://aka.ms/funclinux).

Questa guida introduttiva illustra come usare l'interfaccia della riga di comando di Azure per creare la prima app per le funzioni in esecuzione in Linux. Il codice della funzione viene creato in locale e quindi distribuito in Azure usando [Azure Functions Core Tools](functions-run-local.md).

I passaggi seguenti sono supportati in computer Mac, Windows o Linux. Questo articolo descrive come creare funzioni in JavaScript o C#. Per informazioni su come creare funzioni Python, vedere [Creare la prima funzione Python con Azure Functions Core Tools e l'interfaccia della riga di comando di Azure (anteprima)](functions-create-first-function-python.md).

## <a name="prerequisites"></a>Prerequisiti

Prima di eseguire questo esempio, è necessario disporre di quanto segue:

+ Installare la [versione 2.x di Core Tools di Azure](functions-run-local.md#v2).

+ Installare l'[interfaccia della riga di comando di Azure]( /cli/azure/install-azure-cli). Questo articolo richiede l'interfaccia della riga di comando di Azure 2.0 o versioni successive. Eseguire `az --version` per trovare la versione in uso. È possibile usare anche [Azure Cloud Shell](https://shell.azure.com/bash).

+ Una sottoscrizione di Azure attiva.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>Creare un progetto di app per la funzione locale

Eseguire il comando seguente dalla riga di comando per creare un progetto di app della funzione nella cartella `MyFunctionProj` della directory locale corrente. Viene creato anche un repository di GitHub in `MyFunctionProj`.

```bash
func init MyFunctionProj
```

Quando richiesto, usare i tasti di direzione per selezionare un runtime del ruolo di lavoro dalle seguenti opzioni linguaggio:

+ `dotnet`: crea un progetto libreria di classi .NET (.csproj).
+ `node`: crea un progetto JavaScript o TypeScript. Quando richiesto, scegliere `JavaScript`.
+ `python`: crea un progetto Python. Per le funzioni Python, vedere l'[Avvio rapido per Python](functions-create-first-function-python.md).

Quando viene eseguito il comando, viene visualizzato qualcosa di simile a quanto segue:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

Usare il comando seguente per passare alla nuova cartella del progetto `MyFunctionProj`.

```bash
cd MyFunctionProj
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-update-function-code](../../includes/functions-update-function-code.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Creare un'app per le funzioni Linux in Azure

Per ospitare l'esecuzione delle funzioni in Linux, è necessaria un'app per le funzioni. L'app per le funzioni offre un ambiente senza server per l'esecuzione del codice della funzione. Consente di raggruppare le funzioni come un'unità logica per semplificare la gestione, la distribuzione e la condivisione delle risorse. Creare un'app per le funzioni in esecuzione in Linux usando il comando [az functionapp create](/cli/azure/functionapp#az-functionapp-create).

Nel comando seguente sostituire il segnaposto `<app_name>` con il nome univoco dell'app per le funzioni e il segnaposto `<storage_name>` con il nome dell'account di archiviazione. `<app_name>` è anche il dominio DNS predefinito per l'app per le funzioni. Questo nome deve essere univoco in tutte le app di Azure. È necessario impostare anche il runtime `<language>` per l'app per le funzioni, da `dotnet` (C#), `node` (JavaScript/TypeScript) o `python`.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westus --os-type Linux \
--name <app_name> --storage-account  <storage_name> --runtime <language>
```

Dopo aver creato l'app per le funzioni, viene visualizzato il messaggio seguente:

```output
Your serverless Linux function app 'myfunctionapp' has been successfully created.
To active this function app, publish your app content using Azure Functions Core Tools or the Azure portal.
```

Ora è possibile pubblicare il progetto nella nuova app per le funzioni in Azure.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]
---
title: Esempio di script di Azure PowerShell - Creare un'app e distribuirla dal repository GIT locale | Microsoft Docs
description: Esempio di script di Azure PowerShell - Creare un'app Web e distribuire il codice da un repository Git locale
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 5a927f23-8e70-45fd-9aae-980d4e7a007d
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 8103777b85d8e11416811c694103c58755f1a23a
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56114434"
---
# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a>Creare un'App Web e distribuire il codice da un archivio Git locale

Questo script di esempio crea un'app Web nel servizio app con le relative risorse correlate e quindi distribuisce il codice dell'app Web da un repository Git locale.

Se necessario, effettuare l'aggiornamento all'ultima versione di Azure PowerShell usando le istruzioni riportate nella [guida ad Azure PowerShell](/powershell/azure/overview) e quindi eseguire `Connect-AzAccount` per creare una connessione con Azure. È inoltre necessario eseguire il commit del codice dell'applicazione in un repository Git locale.

## <a name="sample-script"></a>Script di esempio

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/deploy-local-git/deploy-local-git.ps1?highlight=1 "Create a web app and deploy code from a local Git repository")]

## <a name="clean-up-deployment"></a>Pulire la distribuzione 

Dopo aver eseguito lo script di esempio, usare il comando seguente per rimuovere il gruppo di risorse, l'App Web e tutte le risorse correlate.

```powershell
Remove-AzResourceGroup -Name $webappname -Force
```

## <a name="script-explanation"></a>Spiegazione dello script

Questo script usa i comandi seguenti. Ogni comando della tabella include collegamenti alla documentazione specifica del comando.

| Comando | Note |
|---|---|
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Crea un'app Web con il gruppo di risorse e il gruppo del servizio app necessari. Quando la directory corrente contiene un repository Git, aggiungere anche un'istanza remota di `azure`. |

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sul modulo Azure PowerShell, vedere la [documentazione di Azure PowerShell](/powershell/azure/overview).

Altri esempi di Azure PowerShell per app Web del servizio app di Azure sono disponibili in [Azure PowerShell samples](../samples-powershell.md) (Esempi di Azure PowerShell).

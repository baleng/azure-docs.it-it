---
title: Configurare Key Vault per macchine virtuali Windows in Azure Resource Manager | Documentazione Microsoft
description: Come configurare un insieme di credenziali delle chiavi da usare con una macchina virtuale di Azure Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 33a483e2-cfbc-4c62-a588-5d9fd52491e2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: kasing
ms.openlocfilehash: a8c29f015b6b3652361a886585cb4ccc3f3b7293
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2019
ms.locfileid: "58519953"
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Configurare l'insieme di credenziali delle chiavi per le macchine virtuali in Azure Resource Manager

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

In Azure Resource Manager gli stack, i segreti e i certificati vengono modellati come risorse fornite dal provider di risorse dell'insieme di credenziali delle chiavi. Per altre informazioni sugli insiemi di credenziali delle chiavi, vedere [Informazioni sull'insieme di credenziali delle chiavi di Azure](../../key-vault/key-vault-whatis.md)

> [!NOTE]
> 1. Per consentire l'uso dell'insieme di credenziali delle chiavi con le macchine virtuali di Azure Resource Manager, è necessario impostare su true la proprietà **EnabledForDeployment** nell'insieme di credenziali delle chiavi. È possibile farlo in vari tipi di client.
> 2. È necessario creare l'insieme di credenziali delle chiavi nella stessa sottoscrizione e nello stesso percorso della macchina virtuale.
>
>

## <a name="use-powershell-to-set-up-key-vault"></a>Utilizzare PowerShell per configurare l'insieme di credenziali delle chiavi
Per creare un insieme di credenziali delle chiavi con PowerShell, vedere [Impostare e recuperare un segreto da Azure Key Vault tramite PowerShell](../../key-vault/quick-create-powershell.md).

Per i nuovi insiemi di credenziali delle chiavi, è possibile usare questo cmdlet di PowerShell:

    New-AzKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Per gli insiemi di credenziali delle chiavi esistenti, è possibile usare questo cmdlet di PowerShell:

    Set-AzKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="use-cli-to-set-up-key-vault"></a>Usare l'interfaccia della riga di comando per impostare l'insieme di credenziali delle chiavi
Per creare un insieme di credenziali delle chiavi usando l'interfaccia della riga di comando (CLI), vedere l'articolo su come [gestire l'insieme di credenziali delle chiavi tramite l'interfaccia della riga di comando](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

Per l'interfaccia della riga di comando, prima di assegnare i criteri di distribuzione è necessario creare l'insieme di credenziali delle chiavi. A questo scopo, è possibile eseguire questo comando:

    az keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Utilizzare modelli per configurare l'insieme di credenziali delle chiavi
Se si usa un modello, è necessario impostare la proprietà `enabledForDeployment` su `true` per la risorsa dell'insieme di credenziali delle chiavi.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Per altre opzioni che è possibile configurare quando si crea un insieme di credenziali delle chiavi utilizzando i modelli, vedere l'articolo su come [creare un insieme di credenziali delle chiavi](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).

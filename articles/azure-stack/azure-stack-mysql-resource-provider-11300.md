---
title: Note sulla versione di Azure Stack MySQL resource provider 1.1.30.0 | Microsoft Docs
description: Informazioni sulle novità nell'aggiornamento più recente MySQL di Azure Stack resource provider, inclusi i problemi noti e posizione di download.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2018
ms.author: jeffgilb
ms.reviewer: jiahan
ms.lastreviewed: 12/10/2018
ms.openlocfilehash: 5934d075378df9f04130b79eb43131d71eaa25af
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2019
ms.locfileid: "57449462"
---
# <a name="mysql-resource-provider-11300--release-notes"></a>Note sulla versione di MySQL resource provider 1.1.30.0

*Si applica a: Azure Stack Development Kit e i sistemi integrati di Azure Stack*

Queste note sulla versione vengono descritti i miglioramenti e problemi noti nella versione del provider di risorse 1.1.30.0 MySQL.

## <a name="build-reference"></a>Riferimento alla compilazione
Scaricare il provider di risorse MySQL binario e quindi eseguire il programma di autoestrazione per estrarre il contenuto in una directory temporanea. Il provider di risorse dispone di uno Stack di Azure corrispondente minimo di compilazione. La versione minima dello Stack di Azure necessaria per installare questa versione del provider di risorse MySQL è elencata di seguito:

> |Versione minima di Azure Stack|Versione del provider di risorse MySQL|
> |-----|-----|
> |Azure Stack 1808 update (1.1808.0.97)|[1.1.30.0](https://aka.ms/azurestackmysqlrp11300)|
> |     |     |

> [!IMPORTANT]
> Applicare l'aggiornamento di Azure Stack supportato minimo per il sistema integrato Azure Stack o distribuire più recente Azure Stack Development Kit (ASDK) prima di distribuire la versione più recente del provider di risorse MySQL.

## <a name="new-features-and-fixes"></a>Nuove funzionalità e correzioni
Questa versione del provider di risorse MySQL di Azure Stack include le correzioni e i miglioramenti seguenti:

- **I dati di telemetria abilitata per le distribuzioni di provider di risorse MySQL**. Raccolta dati di telemetria è stata abilitata per le distribuzioni del provider di risorse MySQL. Dati di telemetria raccolti includono distribuzione del provider di risorse, avviare e arrestare i tempi, uscire da stato, messaggi di chiusura e i dettagli dell'errore (se applicabile).

- **Aggiornamento di crittografia TLS 1.2**. TLS 1.2 di solo supporto abilitato per la comunicazione di provider di risorse con i componenti interni di Azure Stack. 

### <a name="fixes"></a>Correzioni

- **Provider di risorse MySQL compatibilità di PowerShell per Azure Stack**. Il provider di risorse MySQL è stato aggiornato a con il profilo di Stack 2018-03-01-ibrida di Azure PowerShell e per garantire la compatibilità con AzureRM 1.3.0 aziendali e versioni successive.

- **Pannello di MySQL login change password**. Risolto un problema in cui la password non può essere modificata nel pannello Modifica password. Le notifiche di modifica collegamenti rimossi dalla password.

## <a name="known-issues"></a>Problemi noti 

- **SKU MySQL può richiedere fino a un'ora siano visibili nel portale di**. Operazione può richiedere un'ora per i nuovi SKU creata sia visibile per l'uso durante la creazione di nuovi database MySQL. 

    **Soluzione alternativa**: No.

- **Riutilizzare gli account di accesso MySQL**. Tentativo di creare un nuovo MySQL account di accesso con lo stesso nome utente come un account di accesso esistente nella stessa sottoscrizione comporterà riutilizzo all'account di accesso e la password esistente. 

    **Soluzione alternativa**: Usare nomi utente diversi durante la creazione di nuovi account di accesso nella stessa sottoscrizione o creare gli account di accesso con lo stesso nome utente in diverse sottoscrizioni.

- **Requisito di supporto di TLS 1.2**. Se si prova a distribuire o aggiornare il provider di risorse MySQL da un computer in cui non è abilitato TLS 1.2, l'operazione potrebbe non riuscire. Eseguire il comando PowerShell seguente nel computer usato per distribuire o aggiornare il provider di risorse per verificare che TLS 1.2 venga restituito come supportate:

  ```powershell
  [System.Net.ServicePointManager]::SecurityProtocol
  ```

  Se **Tls12** non è incluso nell'output del comando, TLS 1.2 non è abilitato nel computer.

    **Soluzione alternativa**: Eseguire il comando PowerShell seguente per abilitare TLS 1.2 e quindi avviare la distribuzione di provider di risorse o aggiornare lo script dalla stessa sessione di PowerShell:

    ```powershell
    [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12
    ```
 
### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Problemi noti per gli amministratori Cloud Azure Stack operativo
Vedere la documentazione nel [note sulla versione di Azure Stack](azure-stack-servicing-policy.md).

## <a name="next-steps"></a>Passaggi successivi
[Altre informazioni sui provider di risorse MySQL](azure-stack-mysql-resource-provider.md).

[Preparare la distribuzione del provider di risorse MySQL](azure-stack-mysql-resource-provider-deploy.md#prerequisites).

[Eseguire l'aggiornamento da una versione precedente del provider di risorse MySQL](azure-stack-mysql-resource-provider-update.md). 
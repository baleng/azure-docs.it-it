---
title: Usare un dashboard delle risorse per eseguire una verifica di accesso in PIM - Azure Active Directory | Microsoft Docs
description: Questo articolo descrive come usare un dashboard delle risorse per eseguire una verifica di accesso in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 37057e531ea8387fbed84c9b03bbfb9c14d160ea
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2019
ms.locfileid: "58574961"
---
# <a name="use-a-resource-dashboard-to-perform-an-access-review-in-pim"></a>Usare un dashboard delle risorse per eseguire una verifica di accesso in PIM

È possibile usare un dashboard delle risorse per eseguire una verifica di accesso in Azure Active Directory (Azure AD) Privileged Identity Management (PIM). Il dashboard Visualizzazione amministratore include tre componenti principali:

- Una rappresentazione grafica delle attivazioni del ruolo delle risorse.
- Due grafici che visualizzano la distribuzione delle assegnazioni di ruolo in base al tipo di assegnazione.
- Un'area dati che si riferisce a nuove assegnazioni di ruolo.

![Screenshot del dashboard Visualizzazione amministratore che mostra grafici e diagrammi](media/azure-pim-resource-rbac/rbac-overview-top.png)

![Screenshot del dashboard Visualizzazione amministratore che mostra elenchi di dati](media/azure-pim-resource-rbac/role-settings.png)

La rappresentazione grafica delle attivazioni del ruolo delle risorse copre gli ultimi sette giorni. Come ambito per questi dati viene specificata la risorsa selezionata e vengono visualizzate attivazioni per i ruoli più comuni (proprietario, collaboratore, amministratore accesso utenti) e per tutti i ruoli combinati.

Alla destra del grafico relativo alle attivazioni due grafici mostrano la distribuzione delle assegnazioni di ruolo in base al tipo di assegnazione, per utenti e gruppi. È possibile cambiare il valore in una percentuale o viceversa selezionando una sezione del grafico.

Sotto i grafici viene visualizzato il numero di utenti e gruppi con nuove assegnazioni di ruolo negli ultimi 30 giorni e viene mostrato un elenco di ruoli ordinati in base a numero totale di assegnazioni (in ordine decrescente).

## <a name="next-steps"></a>Passaggi successivi

- [Avviare una verifica di accesso per i ruoli delle risorse di Azure in PIM](pim-resource-roles-start-access-review.md) 

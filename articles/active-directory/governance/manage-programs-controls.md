---
title: Gestire i programmi e controlli per le verifiche di accesso - Azure Active Directory | Microsoft Docs
description: Informazioni su come creare programmi aggiuntivi per ogni governance, gestione dei rischi e iniziativa di conformità dell'organizzazione per raccogliere e organizzare le verifiche di accesso di Azure Active Directory come controlli.
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
ms.subservice: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 43c0f1c041bfed1b041a9926efd869d167c6f1e9
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2019
ms.locfileid: "58577264"
---
# <a name="manage-programs-and-controls-for-azure-ad-access-reviews"></a>Gestire i programmi e controlli per Azure AD le verifiche di accesso

Azure Active Directory (Azure AD) include le verifiche di accesso dell'accesso delle applicazioni e di membri del gruppo. Questi esempi di controlli assicurano la supervisione per chi ha accesso alle applicazioni e alle appartenenze ai gruppi dell'organizzazione. Le organizzazioni possono usare questi controlli per gestire in modo efficiente la governance, i requisiti di conformità e la gestione dei rischi.

## <a name="create-and-manage-programs-and-their-controls"></a>Creare e gestire i programmi e i relativi controlli
È possibile semplificare il rilevamento e la raccolta delle verifiche di accesso per scopi diversi organizzandole nei programmi. Ogni verifica di accesso può essere collegata a un programma. Quando si preparano i report per un revisore, è possibile concentrarsi sulle verifiche di accesso nell'ambito per un'iniziativa specifica.  Programmi e i risultati della verifica di accesso sono visibili agli utenti nell'amministratore globale, l'utente amministratore, amministratore della sicurezza o ruolo di lettore di sicurezza.

Per visualizzare un elenco di programmi, accedere alla [pagina delle verifiche di accesso](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) e selezionare **Programmi**.

**Programma predefinito** è sempre presente. Se si utilizza un amministratore globale o un ruolo utente amministratore, è possibile creare programmi aggiuntivi. Ad esempio, è possibile scegliere che sia disponibile un programma per ogni obiettivo di business o iniziativa di conformità.

Se un programma non è più necessario e non dispone di controlli collegati, è possibile eliminarlo.

## <a name="next-steps"></a>Passaggi successivi

- [Creare una verifica di accesso di gruppi o applicazioni](create-access-review.md)
- [Recuperare i risultati della verifica di accesso per gruppi o applicazioni](retrieve-access-review.md)

---
title: Completare una verifica di accesso di gruppi o applicazioni - Azure Active Directory | Microsoft Docs
description: Informazioni su come completare una verifica di accesso dei membri del gruppo o l'accesso alle applicazioni in Azure Active Directory le verifiche di accesso.
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
ms.date: 05/02/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4265a7e08eab079e55ce91b27142ec3e55b3f3e9
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2019
ms.locfileid: "58579598"
---
# <a name="complete-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Completare una verifica di accesso dei gruppi o applicazioni in Azure AD le verifiche di accesso

Gli amministratori possono usare Azure Active Directory (Azure AD) per [creare una verifica di accesso](create-access-review.md) per gli utenti o i membri del gruppo assegnati a un'applicazione. Azure AD invia automaticamente ai revisori un messaggio di posta elettronica in cui viene chiesto di verificare l'accesso. Se un utente non ha ricevuto un messaggio di posta elettronica, è possibile inviargli le istruzioni [verificare l'accesso a gruppi o applicazioni](perform-access-review.md). Tenere presente che i guest a cui è stato assegnato il ruolo di revisore ma che non hanno accettato l'invito non riceveranno un messaggio di posta elettronica dalle verifiche di accesso, poiché dovranno accettare l'invito prima di poter eseguire la verifica. Al termine del periodo della verifica di accesso o se l'amministratore interrompe la verifica, seguire la procedura descritta in questo articolo per esaminare e applicare i risultati.

## <a name="view-an-access-review-in-the-azure-portal"></a>Visualizzare una verifica di accesso nel portale di Azure

1. Accedere alla [pagina delle verifiche di accesso](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), selezionare **Programmi** e selezionare il programma che contiene il controllo della verifica di accesso.

2. Selezionare **Gestisci** e selezionare il controllo della verifica di accesso. Se sono presenti molti controlli nel programma, è possibile filtrare i controlli di un tipo specifico e ordinarli in base allo stato. È possibile anche cercare il nome del controllo della verifica di accesso o il nome visualizzato del proprietario che lo ha creato. 

## <a name="stop-a-review-that-hasnt-finished"></a>Interrompere una verifica non completata

Se la verifica non ha raggiunto la data di fine pianificata, un amministratore può selezionare **Arresta** per terminare in anticipo la verifica. Dopo avere interrotto la verifica, gli utenti non possono più essere verificati. Non è possibile riavviare una verifica dopo che è stata interrotta.

## <a name="apply-the-changes"></a>Applicare le modifiche. 

Al termine di una verifica di accesso, sia che abbia raggiunto la data di fine o che sia stata interrotta manualmente da un amministratore, e se per la verifica non è stata configurata l'applicazione automatica, selezionare **Applica** per applicare manualmente le modifiche. Il risultato della verifica viene implementato aggiornando il gruppo o l'applicazione. Se l'accesso di un utente è stato negato durante la verifica, Azure AD rimuoverà l'assegnazione dell'applicazione o l'appartenenza quando l'amministratore seleziona questa opzione. 

Al termine di una verifica di accesso, se è stata configurata l'applicazione automatica, lo stato della verifica passerà da Completato ad Applicato attraverso alcuni stati intermedi. È possibile che, dopo alcuni minuti, eventuali utenti negati vengano rimossi dall'appartenenza al gruppo di risorse o dall'assegnazione all'app.

Se per una verifica si configura l'applicazione automatica o si seleziona **Applica**, questa operazione non avrà alcun effetto su un gruppo che ha origine in una directory locale o in un gruppo dinamico. Per modificare un gruppo che ha origine in locale, scaricare i risultati e applicare queste modifiche alla rappresentazione del gruppo in questa directory.

## <a name="download-the-results-of-the-review"></a>Scaricare i risultati della verifica

Per recuperare i risultati della verifica, selezionare **Approvazioni** e quindi selezionare **Scarica**. Il file CSV risultante può essere visualizzato in Excel o in altri programmi aperti in file CSV con codifica UTF-8.

## <a name="optional-delete-a-review"></a>Facoltativo: Eliminare una verifica
È possibile eliminare una verifica che non è più utile. Selezionare **Eliminare** per rimuovere la verifica da Azure AD.

> [!IMPORTANT]
> Non verrà visualizzato un avviso prima dell'eliminazione ed è quindi necessario assicurarsi di voler eseguire questa operazione.
> 
> 

## <a name="next-steps"></a>Passaggi successivi

- [Gestire l'accesso utente con le verifiche di accesso di Azure AD](manage-user-access-with-access-reviews.md)
- [Gestire l'accesso guest con le verifiche di accesso di Azure AD](manage-guest-access-with-access-reviews.md)
- [Gestire i programmi e i controlli per le verifiche di accesso di Azure AD](manage-programs-controls.md)
- [Creare una verifica di accesso di gruppi o applicazioni](create-access-review.md)
- [Creare una verifica di accesso degli utenti in un ruolo amministrativo Azure AD](../privileged-identity-management/pim-how-to-start-security-review.md)

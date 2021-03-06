---
title: Avviare una verifica di accesso per i ruoli di Azure AD in PIM - Azure Active Directory | Microsoft Docs
description: Informazioni su come avviare una verifica di accesso per i ruoli di Azure AD in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/21/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5cbf96c165d79c26985663ef5a9d64bbf8f9892
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2019
ms.locfileid: "58574995"
---
# <a name="start-an-access-review-for-azure-ad-roles-in-pim"></a>Avviare una verifica di accesso per i ruoli di Azure AD in PIM
Le assegnazioni dei ruoli diventano "obsolete" quando gli utenti hanno accessi con privilegi di cui non necessitano più. Per ridurre il rischio associato alle assegnazioni dei ruoli obsolete, gli amministratori dei ruoli con privilegi o gli amministratori globali devono creare periodicamente delle verifiche degli accessi, in modo da chiedere agli amministratori di verificare i ruoli assegnati agli utenti. Questo documento illustra i passaggi per avviare una verifica di accesso in Azure Active Directory (Azure AD) Privileged Identity Management (PIM).

## <a name="start-an-access-review"></a>Avviare una verifica dell'accesso
> [!NOTE]
> Se l'applicazione PIM non è stata aggiunta al dashboard nel portale di Azure, vedere i passaggi descritti in [Introduzione ad Azure AD Privileged Identity Management](pim-getting-started.md)
> 
> 

Nella pagina principale dell'applicazione PIM sono disponibili tre opzioni per avviare una verifica dell'accesso:

* **Verifiche di accesso** > **Aggiungi**
* **Ruoli** > **Verifica**
* Selezionare il ruolo specifico da verificare dall'elenco dei ruoli > **Verifica**

Quando si fa clic sul pulsante **Verifica**, viene visualizzato il pannello **Inizia una verifica di accesso**. In questo pannello configurare la verifica assegnando un nome e un limite temporale, scegliere un ruolo da verificare e decidere chi eseguirà la verifica.

![Schermata Inizia una verifica di accesso](./media/pim-how-to-start-security-review/PIM_start_review.png)

### <a name="configure-the-review"></a>Configurare la verifica
Per creare una verifica di accesso, è necessario assegnare un nome e impostare le date di inizio e di fine.

![Schermata di configurazione della verifica](./media/pim-how-to-start-security-review/PIM_review_configure.png)

Definire una durata della verifica che consenta agli utenti di completare l'operazione. Se la verifica termina prima della data di fine, è sempre possibile arrestare la verifica in anticipo.

### <a name="choose-a-role-to-review"></a>Scegliere un ruolo da verificare
Ogni verifica è incentrata su un solo ruolo. A meno che non si abbia avviato la verifica di accesso dal pannello di un ruolo specifico, è ora necessario scegliere un ruolo.

1. Passare a **Verifica l'appartenenza ai ruoli**
   
    ![Schermata Verifica l'appartenenza ai ruoli](./media/pim-how-to-start-security-review/PIM_review_role.png)
2. Scegliere un ruolo dall'elenco.

### <a name="decide-who-will-perform-the-review"></a>Decidere chi eseguirà la verifica
Sono disponibili tre opzioni per l'esecuzione di una verifica. La revisione può essere assegnata a un altro utente, essere eseguita personalmente oppure ogni utente può verificare il proprio accesso.

1. Passare a **Selezionare i revisori**
   
    ![Schermata Selezionare i revisori](./media/pim-how-to-start-security-review/PIM_review_reviewers.png)
2. Scegliere una delle opzioni disponibili:
   
   * **Seleziona il revisore**: usare questa opzione quando non è noto chi abbia bisogno dell'accesso. Con questa opzione è possibile assegnare l'esecuzione della revisione a un proprietario delle risorse o a un gestore del gruppo.
   * **Io**: utile se si vuole visualizzare in anteprima come funzionano le verifiche dell'accesso o se si vuole eseguire una verifica per conto di utenti che non possono eseguire questa operazione.
   * **I membri verificano se stessi**: usare questa opzione per fare in modo che gli utenti verifichino le proprie assegnazioni di ruolo.

### <a name="start-the-review"></a>Avviare la verifica
Infine è disponibile l'opzione per richiedere agli utenti di fornire un motivo se approvano l'accesso. Se si vuole, aggiungere una descrizione della verifica e selezionare **Start**(Avvia).

È necessario informare gli utenti che è presente una verifica dell'accesso in attesa e illustrare [come eseguire una verifica dell'accesso](pim-how-to-perform-security-review.md).

## <a name="manage-the-access-review"></a>Gestire la verifica di accesso
È possibile tenere traccia dello stato di avanzamento delle verifiche eseguite dai revisori nella sezione Verifiche di accesso del dashboard di Azure AD PIM. Nessun diritto di accesso verrà modificato nella directory fino al [completamento della verifica](pim-how-to-complete-review.md).

Fino al termine del periodo di verifica, è possibile ricordare agli utenti di completare la verifica o arrestare la verifica in anticipo nella sezione Verifiche di accesso.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Passaggi successivi

- [Completare una verifica di accesso per i ruoli di Azure AD in PIM](pim-how-to-complete-review.md)
- [Eseguire una verifica di accesso dei ruoli personali AD Azure in PIM](pim-how-to-perform-security-review.md)
- [Avviare una verifica di accesso per i ruoli delle risorse di Azure in PIM](pim-resource-roles-start-access-review.md)

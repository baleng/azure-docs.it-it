---
title: Gli amministratori di gestire utenti e dispositivi - Azure MFA - Azure Active Directory
description: Viene descritto come modificare le impostazioni utente (ad esempio, come imporre agli utenti di ripetere il processo di registrazione).
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: c78d6d901c050f6d1df8b53b34f0088d3ad8b0f8
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2019
ms.locfileid: "58368477"
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Gestire le impostazioni utente nel cloud con Azure Multi-Factor Authentication

Come amministratore, è possibile gestire le impostazioni relative all'utente e al dispositivo riportate di seguito:

* Richiedere agli utenti di fornire di nuovo i metodi di contatto
* Eliminare password per le app
* Richiedere l'autenticazione a più fattori su tutti i dispositivi attendibili

## <a name="require-users-to-provide-contact-methods-again"></a>Richiedere agli utenti di fornire di nuovo i metodi di contatto

Questa impostazione impone all'utente di completare di nuovo il processo di registrazione. Le app non basate su browser continuano a funzionare se l'utente dispone delle relative password.  È possibile eliminare le password dell'app degli utenti anche selezionando **Eliminare tutte le password dell'app esistenti generate dagli utenti selezionati**.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Come richiedere agli utenti di fornire di nuovo i metodi di contatto

1. Accedere al [portale di Azure](https://portal.azure.com).
2. A sinistra selezionare **Azure Active Directory** > **Utenti** > **Tutti gli utenti**.
3. A destra selezionare **Multi-Factor Authentication** sulla barra degli strumenti. Viene aperta la pagina dell'autenticazione a più fattori.
4. Selezionare la casella accanto a uno o più utenti che si desidera gestire. Sulla destra viene visualizzato un elenco di opzioni di azione rapida.
5. Selezionare **Gestisci le impostazioni dell'utente**.
6. Selezionare la casella accanto a **Richiedere agli utenti selezionati di fornire di nuovo i metodi di contatto**.
   ![Richiedere agli utenti di fornire di nuovo i metodi di contatto](./media/howto-mfa-userdevicesettings/reproofup.png)
7. Fare clic su **save**.
8. Fare clic su **chiudi**.

## <a name="delete-users-existing-app-passwords"></a>Eliminare le password per le app esistenti degli utenti

Questa impostazione elimina tutte le password dell'app create da un utente. Le app non basate su browser associate a tali password dell'app non funzioneranno più fino a quando non verrà creata una nuova password dell'app.

### <a name="how-to-delete-users-existing-app-passwords"></a>Come eliminare le password per le app esistenti degli utenti

1. Accedere al [portale di Azure](https://portal.azure.com).
2. A sinistra selezionare **Azure Active Directory** > **Utenti** > **Tutti gli utenti**.
3. A destra selezionare **Multi-Factor Authentication** sulla barra degli strumenti. Viene aperta la pagina dell'autenticazione a più fattori.
4. Selezionare la casella accanto a uno o più utenti che si desidera gestire. Sulla destra viene visualizzato un elenco di opzioni di azione rapida.
5. Selezionare **Gestisci le impostazioni dell'utente**.
6. Selezionare la casella accanto a **Eliminare tutte le password dell'app esistenti generate dagli utenti selezionati**.
   ![Eliminare tutte le password dell'app](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
7. Fare clic su **save**.
8. Fare clic su **chiudi**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Ripristinare MFA in tutti i dispositivi memorizzati per un utente

Una delle funzionalità configurabili di Azure Multi-Factor Authentication è dare agli utenti la possibilità di contrassegnare i dispositivi come attendibili. Per altre informazioni, vedere [Configurare le impostazioni di Azure Multi-Factor Authentication](howto-mfa-mfasettings.md#remember-multi-factor-authentication).

Gli utenti possono rifiutare esplicitamente la verifica in due passaggi per un numero configurabile di giorni per i propri dispositivi regolari. Se un account viene compromesso o un dispositivo attendibile viene smarrito, è necessario poter rimuovere lo stato attendibile e richiedere di nuovo la verifica in due passaggi.

L'impostazione **Ripristina l'autenticazione a più fattori in tutti i dispositivi memorizzati** indica che all'utente verrà chiesto di eseguire la verifica in due passaggi al successivo accesso, indipendentemente dal fatto che abbia scelto di contrassegnare il dispositivo come attendibile.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>Come ripristinare Multi-Factor Authentication in tutti i dispositivi sospesi per un utente

1. Accedere al [portale di Azure](https://portal.azure.com).
2. A sinistra selezionare **Azure Active Directory** > **Utenti** > **Tutti gli utenti**.
3. A destra selezionare **Multi-Factor Authentication** sulla barra degli strumenti. Viene aperta la pagina dell'autenticazione a più fattori.
4. Selezionare la casella accanto a uno o più utenti che si desidera gestire. Sulla destra viene visualizzato un elenco di opzioni di azione rapida.
5. Selezionare **Gestisci le impostazioni dell'utente**.
6. Selezionare la casella **Ripristina l'autenticazione a più fattori in tutti i dispositivi memorizzati**
   ![Ripristina l'autenticazione a più fattori in tutti i dispositivi memorizzati](./media/howto-mfa-userdevicesettings/rememberdevices.png)
7. Fare clic su **save**.
8. Fare clic su **chiudi**.

## <a name="next-steps"></a>Passaggi successivi

- Per altre informazioni, vedere [Configurare le impostazioni di Azure Multi-Factor Authentication](howto-mfa-mfasettings.md)
- Se gli utenti hanno bisogno di supporto, suggerire loro di vedere il [Manuale dell'utente per la verifica in due passaggi](../user-help/multi-factor-authentication-end-user.md)

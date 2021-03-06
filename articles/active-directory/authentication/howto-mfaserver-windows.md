---
title: L'autenticazione di Windows e Server Azure MFA - Azure Active Directory
description: Distribuzione dell'autenticazione di Windows e del server Azure Multi-Factor Authentication.
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
ms.openlocfilehash: 36172850c345fc190c3326413f2883dc2b070e98
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2019
ms.locfileid: "58367891"
---
# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>L'autenticazione di Windows e Azure il Server Multi-Factor Authentication

Usare la sezione Autenticazione di Windows del server Azure Multi-Factor Authentication per abilitare e configurare l'autenticazione di Windows per le applicazioni. Prima di configurare l'autenticazione di Windows, tenere presente quanto segue:

* Dopo aver eseguito la configurazione, riavviare Azure Multi-Factor Authentication per rendere effettivo Servizi terminal.
* Se "Richiedere corrispondenza utente Azure Multi-Factor Authentication" è selezionata e non si è nell'elenco degli utenti, non sarà possibile accedere alla macchina dopo il riavvio.
* La funzione IP attendibili dipende da se l'applicazione può fornire l'IP del client con l'autenticazione. Attualmente solo la funzione Servizi Terminal è supportata.  

> [!NOTE]
> Questa funzionalità non è supportata per proteggere Servizi Terminal in Windows Server 2012 R2.

## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>Per proteggere un'applicazione con l'autenticazione di Windows, utilizzare la procedura seguente

1. Nel server Microsoft Azure Multi-Factor Authentication fare clic sull'icona Autenticazione di Windows.
   ![Autenticazione di Windows nel Server MFA](./media/howto-mfaserver-windows/windowsauth.png)
2. Selezionare la casella di controllo **Abilita autenticazione di Windows**. Per impostazione predefinita, questa casella è deselezionata.
3. La scheda applicazioni consente all'amministratore di configurare uno o più applicazioni per l'autenticazione di Windows.
4. Selezionare un server o un'applicazione, specificare se il server/applicazione è abilitato. Fare clic su **OK**.
5. Fare clic su **Aggiungi...**
6. La scheda di ID attendibili consente di ignorare Azure Multi-Factor Authentication per le sessioni Windows provenienti da IP specifici. Ad esempio, se i dipendenti usano l'applicazione dall'ufficio e da casa, è possibile decidere di non volere che i loro telefoni squillino per Azure Multi-Factor Authentication in ufficio. A tale scopo, specificare la subnet dell'ufficio come voce di ID attendibili.
7. Fare clic su **Aggiungi...**
8. Selezionare **IP singolo** se si vuole ignorare un singolo indirizzo IP.
9. Selezionare **Intervallo IP** se si vuole ignorare un intero intervallo di indirizzi IP. Esempio: 10.63.193.1-10.63.193.100.
10. Selezionare **Subnet** per specificare un intervallo di indirizzi IP usando la notazione subnet. Immettere l’IP iniziale della subnet e scegliere la mask appropriata dall'elenco a discesa.
11. Fare clic su **OK**.

## <a name="next-steps"></a>Passaggi successivi

- [Configurare dispositivi VPN di terze parti per il server Azure MFA](howto-mfaserver-nps-vpn.md)

- [Ampliare l'infrastruttura di autenticazione esistente con l'estensione NPS per Azure MFA](howto-mfa-nps-extension.md)

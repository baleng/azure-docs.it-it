---
title: Creare un'offerta per un'applicazione SaaS di Azure | Microsoft Docs
description: Come creare un'offerta per un'applicazione SaaS in Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/04/2018
ms.author: pbutlerm
ms.openlocfilehash: bcaf470fae34056fe932cf8b5fd3076899f895ae
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/18/2019
ms.locfileid: "57880280"
---
# <a name="create-a-new-saas-application-offer"></a>Creare una nuova offerta per un'applicazione SaaS

Questo articolo descrive come creare e pubblicare la voce dell'offerta di un'applicazione (app) SaaS per Azure Marketplace.

## <a name="offer-process"></a>Processo dell'offerta

Il diagramma seguente illustra il processo per la creazione di un'offerta per un'app SaaS.

![Processo per la creazione di un'offerta SaaS](./media/saas-offer-process-overview.png)

## <a name="offer-components"></a>Componenti dell'offerta

L'offerta di app SaaS è costituita da cinque sezioni, descritte nella tabella seguente:

|  **Gruppo di risorse**   |  **Descrizione**  |
|  ---------------   |  ---------------  |
|    Impostazioni dell'offerta  |  Da usare per configurare un'identità univoca per l'app SaaS.                 |
|  Informazioni tecniche    |  Da usare per configurare il tipo di soluzione SaaS e specificare i dettagli della connessione per l'applicazione.                |
|  Informazioni canale      |   Fornire informazioni sul canale come contatti e materiali GTM.                |
|  Test drive        |   Sezione facoltativa per la definizione di un servizio che consentirà ai clienti di testare l'offerta prima di acquistarla.                |
|  Dati da visualizzare nella vetrina       | Contiene specifiche e risorse di marketing, legali e di gestione dei clienti potenziali.  <ul><li> Le risorse di marketing includono il nome dell'offerta, la descrizione e i loghi</li> <li> Gli asset legali includono l'informativa sulla privacy, le condizioni per l'utilizzo e altra documentazione legale</li>  <li> I criteri di gestione dei clienti potenziali consentono di specificare come gestire i clienti potenziali dal portale dell'utente finale Azure Marketplace.</li> </ul> |
| Contatti            | Contiene informazioni sui criteri e sui contatti di supporto |

## <a name="new-offer-form"></a>Modulo Nuova offerta

Accedere al [portale Cloud Partner](https://cloudpartner.azure.com/) e quindi selezionare **+ Nuova offerta** nella barra dei menu a sinistra. Dal menu Nuova offerta scegliere **App SaaS** per visualizzare il modulo Nuova offerta e avviare il processo di definizione delle risorse per la nuova offerta di un'applicazione SaaS.

![Menu Nuova offerta per le app SaaS](./media/azure-new-saas-offer.png)

## <a name="next-steps"></a>Passaggi successivi

La pagina Nuova offerta per il tipo di offerta SaaS include un set di schede e di campi modulo che è possibile usare per creare una nuova offerta. Ognuno degli articoli seguenti illustra come usare la scheda per definire i gruppi di risorse e i servizi di supporto per la nuova offerta.

- [Scheda Impostazioni dell'offerta](./cpp-offer-settings-tab.md)
- [Scheda Informazioni tecniche](./cpp-technical-info-tab.md)
- [Scheda Informazioni canale](./cpp-channel-info-tab.md)
- [Scheda Test Drive](./cpp-testdrive-tab.md)
- [Scheda Dettagli vetrina](./cpp-storefront-tab.md)
- [Scheda Contatti](./cpp-contacts-tab.md)

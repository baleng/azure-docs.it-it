---
title: Trasmettere i log di Azure Active Directory a SumoLogic tramite Monitoraggio di Azure (anteprima) | Microsoft Docs
description: Informazioni su come integrare i log di Azure Active Directory con SumoLogic tramite Monitoraggio di Azure (anteprima)
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3bed1b96052117534f319bbf9570618e6481032f
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2019
ms.locfileid: "58437209"
---
# <a name="integrate-azure-active-directory-logs-with-sumologic-using-azure-monitor-preview"></a>Integrare i log di Azure Active Directory con SumoLogic tramite Monitoraggio di Azure (anteprima)

Questo articolo illustra come integrare i log di Azure Active Directory (Azure AD) con SumoLogic tramite Monitoraggio di Azure. Innanzitutto indirizzare i log a un hub eventi di Azure e quindi integrare l'hub eventi con SumoLogic.

## <a name="prerequisites"></a>Prerequisiti

Per usare questa funzionalità, sono necessari:
* Un hub eventi di Azure contenente i log attività di Azure AD. Informazioni su come [trasmettere i log attività a un hub eventi](quickstart-azure-monitor-stream-logs-to-event-hub.md). 
* Sottoscrizione di SumoLogic abilitata per l'accesso Single Sign-On.

## <a name="steps-to-integrate-azure-ad-logs-with-sumologic"></a>Procedura per integrare i log di Azure AD con SumoLogic 

1. In primo luogo, [trasmettere i log di Azure AD a un hub eventi di Azure](quickstart-azure-monitor-stream-logs-to-event-hub.md).
2. Configurare l'istanza di SumoLogic per [raccogliere i log per Azure Active Directory](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Collect_Logs_for_Azure_Active_Directory).
3. [Installare l'app SumoLogic di Azure AD](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards) per usare i dashboard preconfigurati che forniscono un'analisi in tempo reale dell'ambiente.

   ![dashboard](./media/howto-integrate-activity-logs-with-sumologic/overview-dashboard.png)

## <a name="next-steps"></a>Passaggi successivi

* [Interpretare lo schema dei log di controllo in Monitoraggio di Azure](reference-azure-monitor-audit-log-schema.md)
* [Interpretare lo schema dei log di accesso in Monitoraggio di Azure](reference-azure-monitor-sign-ins-log-schema.md)
* [Domande frequenti e problemi noti](concept-activity-logs-azure-monitor.md#frequently-asked-questions)

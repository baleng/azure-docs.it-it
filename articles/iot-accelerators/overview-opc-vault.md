---
title: Informazioni sulla gestione certificati OPC UA di Azure IoT | Microsoft Docs
description: Panoramica di OPC Vault
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: ba3313d927ec4317d6c051f6058d75a415b92288
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759771"
---
# <a name="what-is-azure-iot-open-platform-communications-opc-ua-certificate-management"></a>Informazioni sulla gestione certificati OPC (Open Platform Communications) UA di Azure IoT

Il servizio di gestione certificati OPC UA di Azure IoT, anche detto OPC Vault, è un microservizio che consente di configurare, registrare e gestire il ciclo di vita dei certificati per le applicazioni server e client OPC UA nel cloud. Questo articolo descrive i semplici casi d'uso di OPC Vault.

## <a name="certificate-management"></a>Gestione dei certificati

Ad esempio, un'azienda manifatturiera deve connettere il proprio server OPC UA a una nuova applicazione client sviluppata. Quando viene eseguito l'accesso iniziale del server, nell'applicazione server OPC UA viene immediatamente visualizzato un messaggio di errore, a indicare che l'applicazione client non è sicura. Questo meccanismo è integrato nel server OPC UA per impedire l'accesso non autorizzato all'applicazione, evitando attacchi di hacker nella fabbrica.

## <a name="application-security-management"></a>Gestione della sicurezza delle applicazioni
Un professionista della sicurezza usa il microservizio OPC Vault per consentire al server OPC UA di comunicare facilmente con qualsiasi applicazione client, perché OPC Vault include tutte le funzioni per la registrazione, l'archiviazione e la gestione del ciclo di vita dei certificati. Una volta connesso in modo sicuro, il server OPC UA può comunicare con la nuova applicazione client sviluppata.

## <a name="the-complete-opc-vault-architecture"></a>Architettura OPC Vault completa
Il diagramma seguente illustra l'architettura OPC Vault completa.

![Architettura OPC Vault](media/overview-opc-vault-architecture/opc-vault.png)
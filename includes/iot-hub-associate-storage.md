---
title: File di inclusione
description: File di inclusione
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 01/22/2019
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 8693c48905155ed757bb727e42f4180f36c015f1
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513941"
---
## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Associare un account di archiviazione di Azure all'hub IoT

Poiché l'app per dispositivo simulato carica un file in un BLOB, è necessario avere un account di [Archiviazione di Azure](../articles/storage/common/storage-quickstart-create-account.md) associato al proprio hub IoT. Quando si associa un account di Archiviazione di Azure a un hub IoT, l'hub IoT genera un URI di firma di accesso condiviso. Un dispositivo può usare questo URI di firma di accesso condiviso per eseguire l'upload di un file in modo sicuro in un contenitore BLOB. Il servizio hub IoT e gli SDK di dispositivo coordinano il processo che genera l'URI di firma di accesso condiviso e lo rendono disponibile per un dispositivo che lo userà per caricare un file.

Seguire le istruzioni in [Configure file uploads using the Azure portal](../articles/iot-hub/iot-hub-configure-file-upload.md) (Configurare i caricamenti dei file tramite il portale di Azure). Assicurarsi che un contenitore BLOB venga associato all'hub IoT e che siano abilitate le notifiche file.

![Abilitazione di notifiche file nel portale](./media/iot-hub-associate-storage/enable-file-notifications.png)
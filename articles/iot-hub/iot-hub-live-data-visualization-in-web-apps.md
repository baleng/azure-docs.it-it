---
title: Visualizzazione in tempo reale dei dati del sensore dall'hub IoT di Azure – App Web | Microsoft Docs
description: Usare la funzionalità App Web di Azure del Servizio app di Microsoft Azure per visualizzare i dati su temperatura e umidità raccolti tramite il sensore e inviati all'hub IoT di Azure.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: robinsh
ms.openlocfilehash: b870c432ea8021d9eeb14eec85ff9912b96ddc96
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59270881"
---
# <a name="visualize-real-time-sensor-data-from-your-azure-iot-hub-by-using-the-web-apps-feature-of-azure-app-service"></a>Visualizzare i dati del sensore in tempo reale dall'hub IoT di Azure usando la funzionalità App Web del Servizio App di Azure

![Diagramma end-to-end](./media/iot-hub-live-data-visualization-in-web-apps/1_iot-hub-end-to-end-diagram.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Contenuto dell'esercitazione

In questa esercitazione si imparerà a visualizzare i dati del sensore in tempo reale che l'hub IoT riceve eseguendo un'applicazione Web ospitata in un'app Web. Se si desidera provare a visualizzare i dati nell'hub IoT con Power BI, vedere [Usare Power BI per visualizzare i dati del sensore in tempo reale dall'hub IoT di Azure](iot-hub-live-data-visualization-in-power-bi.md).

## <a name="what-you-do"></a>Operazioni da fare

* Creare un'app Web nel portale di Azure.
* Preparare l'hub IoT per l'accesso dei dati mediante l'aggiunta di un gruppo di consumer.
* Configurare l'app Web per leggere i dati del sensore dall'hub IoT.
* Caricare un'applicazione Web che deve essere ospitata dall'app Web.
* Aprire l'app Web per visualizzare i dati di temperatura e umidità in tempo reale dall'hub IoT.

## <a name="what-you-need"></a>Elementi necessari

* [Configurare il dispositivo](iot-hub-raspberry-pi-kit-node-get-started.md) che presenta i seguenti requisiti:

  * Una sottoscrizione di Azure attiva
  * Un hub IoT nella sottoscrizione
  * Un'applicazione client che invia messaggi all'hub IoT

* [Download GIT](https://www.git-scm.com/downloads)

## <a name="create-a-web-app"></a>Creare un'app Web

1. Nel [portale di Azure](https://portal.azure.com/) fare clic su **Crea una risorsa** > **Web e dispositivi mobili** > **App Web**.

2. Immettere un nome univoco del processo, verificare la sottoscrizione, specificare un gruppo di risorse e il percorso, selezionare **Aggiungi al dashboard**, quindi fare clic su **Crea**.

   È consigliabile selezionare la stessa posizione del gruppo di risorse. 
   
   ![Creare un'app Web](./media/iot-hub-live-data-visualization-in-web-apps/2_create-web-app-azure.png)

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="configure-the-web-app-to-read-data-from-your-iot-hub"></a>Configurare l'app Web per leggere i dati dall'hub IoT

1. Aprire l'app Web di cui è stato appena effettuato il provisioning.

2. Fare clic su **Impostazioni applicazione**, quindi aggiungere le seguenti coppie chiave/valore in **Impostazioni app**:

   | Chiave                                   | Valore                                                        |
   |---------------------------------------|--------------------------------------------------------------|
   | Azure.IoT.IoTHub.ConnectionString     | Ottenuto dall’interfaccia della riga di comando di Azure                                      |
   | Azure.IoT.IoTHub.ConsumerGroup        | Il nome del gruppo di consumer che si aggiunge all'hub IoT  |
   | WEBSITE_NODE_DEFAULT_VERSION          | 8.9.4                                                        |

   ![Aggiungere impostazioni all'app Web di Azure con coppie chiave/valore](./media/iot-hub-live-data-visualization-in-web-apps/3_web-app-settings-key-value-azure.png)

3. Fare clic su **Impostazioni applicazione**, in **Impostazioni generali**, attivare/disattivare l'opzione **Web Socket**, quindi fare clic su **Salva**.

   ![Attivare/disattivare l'opzione Web Socket](./media/iot-hub-live-data-visualization-in-web-apps/4_toggle_web_sockets.png)

## <a name="upload-a-web-application-to-be-hosted-by-the-web-app"></a>Caricare un'applicazione Web che deve essere ospitata dall'app Web

Su GitHub è disponibile un'applicazione Web che consente di visualizzare i dati del sensore in tempo reale dall'hub IoT. È sufficiente configurare l'app Web che dovrà usare un repository Git, scaricare l'applicazione Web da GitHub e caricarla in Azure affinché l'applicazione Web possa ospitarla.

1. Nell'app Web fare clic su **Opzioni di distribuzione** > **Scegli origine** > **Repository Git locale**, quindi scegliere **OK**.

   ![Configurare la distribuzione dell'app Web per l'uso del repository Git](./media/iot-hub-live-data-visualization-in-web-apps/5_configure-web-app-deployment-local-git-repository-azure.png)

2. Fare clic su **Credenziali per la distribuzione**, creare un nome utente e una password da usare per connettersi al repository Git in Azure, quindi scegliere **Salva**.

3. Fare clic su **Panoramica** e prendere nota del valore di **URL clone GIT**.

   ![Ottenere l'URL clone Git dell'app Web](./media/iot-hub-live-data-visualization-in-web-apps/6_web-app-git-clone-url-azure.png)

4. Aprire una finestra del comando o del terminale nel computer locale.

5. Scaricare l'app Web da GitHub e caricarla in Azure affinché ospiti l'app Web. A tale scopo, eseguire i comandi seguenti:

   ```bash
   git clone https://github.com/Azure-Samples/web-apps-node-iot-hub-data-visualization.git
   cd web-apps-node-iot-hub-data-visualization
   git remote add webapp <Git clone URL>
   git push webapp master:master
   ```

   > [!NOTE]
   > \<URL clone GIT\> è l'URL del repository Git che si trova nella pagina **Panoramica** dell'app Web.

## <a name="open-the-web-app-to-see-real-time-temperature-and-humidity-data-from-your-iot-hub"></a>Aprire l'app Web per visualizzare i dati di temperatura e umidità in tempo reale dall'hub IoT

Nella pagina **Panoramica** dell'app Web fare clic sull'URL per aprire l'app Web.

![Ottenere l'URL dell'app Web](./media/iot-hub-live-data-visualization-in-web-apps/7_web-app-url-azure.png)

Vengono visualizzati i dati di temperatura e umidità in tempo reale dall'hub IoT.

![Pagina dell'app Web che mostra temperatura e umidità in tempo reale](./media/iot-hub-live-data-visualization-in-web-apps/8_web-app-page-show-real-time-temperature-humidity-azure.png)

> [!NOTE]
> Verificare che l'applicazione di esempio sia in esecuzione nel dispositivo. Se non lo fosse, si otterrà un grafico vuoto, ed è possibile fare riferimento alle esercitazioni descritte in [Configurare il dispositivo](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="next-steps"></a>Passaggi successivi

L'app Web è stata usata correttamente per visualizzare i dati del sensore in tempo reale dall'hub IoT.

Per un modo alternativo di visualizzare i dati dall'hub IoT di Azure, vedere [Usare Power BI per visualizzare i dati del sensore in tempo reale dall'hub IoT](iot-hub-live-data-visualization-in-power-bi.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
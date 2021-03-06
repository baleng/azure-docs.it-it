---
title: Introduzione alla gestione dei dispositivi dell'hub IoT di Azure (Node) | Documentazione Microsoft
description: Come usare la gestione dei dispositivi dell'hub IoT per riavviare un dispositivo remoto. Usare Azure IoT SDK per Node.js per implementare un'app per dispositivo simulato che include un metodo diretto e un'app di servizio che richiama il metodo diretto.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/25/2017
ms.openlocfilehash: 15166d3943bc72a2eeff3580cefdd264ecaba61d
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59258086"
---
# <a name="get-started-with-device-management-node"></a>Introduzione alla gestione dei dispositivi (Node)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Questa esercitazione illustra come:

* Usare la [portale di Azure](https://portal.azure.com) per creare un IoT Hub e creare un'identità del dispositivo nell'hub IoT.

* Creare un'app di dispositivo simulato contenente un metodo diretto per il riavvio del dispositivo. I metodi diretti vengono richiamati dal cloud.

* Creare un'app console Node.js che chiama il metodo diretto di riavvio nell'app per dispositivo simulato tramite l'hub IoT.

Al termine di questa esercitazione si avranno due app console Node.js:

* **dmpatterns_getstarted_device.js**, che si connette all'hub IoT con l'identità del dispositivo creata in precedenza, riceve un metodo diretto per il riavvio, simula un riavvio fisico e segnala il tempo impiegato per l'ultimo riavvio.

* **dmpatterns_getstarted_service.js**, che chiama un metodo diretto nel dispositivo simulato, visualizza la risposta e le proprietà segnalate aggiornate.

Per completare l'esercitazione, sono necessari gli elementi seguenti:

* Node.js 4.0.x o versione successiva. [Preparare l'ambiente di sviluppo](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md) viene descritto come installare Node. js per questa esercitazione in Windows o Linux.

* Un account Azure attivo. Se non si dispone di un account, è possibile crearne uno [gratuito](https://azure.microsoft.com/pricing/free-trial/) in pochi minuti.

## <a name="create-an-iot-hub"></a>Creare un hub IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Ottenere la stringa di connessione per l'hub IoT

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Creare un'app di dispositivo simulato

In questa sezione si eseguirà i passaggi seguenti:

* Creare un'app console Node.js che risponde a un metodo diretto chiamato dal cloud

* Attivare un riavvio del dispositivo simulato

* Usare le proprietà segnalate per abilitare le query nei dispositivi gemelli in modo da identificare i dispositivi e l'ora dell'ultimo riavvio

1. Creare una cartella vuota denominata **manageddevice**.  Nella cartella **manageddevice** creare un file package.json eseguendo questo comando al prompt dei comandi.  Accettare tutte le impostazioni predefinite:
      
    ```
    npm init
    ```

2. Eseguire questo comando al prompt dei comandi nella cartella **manageddevice** per installare il pacchetto SDK per dispositivi **azure-iot-device** e il pacchetto **azure-iot-device-mqtt**:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. Con un editor di testo creare un file **dmpatterns_getstarted_device.js** nella cartella **manageddevice**.

4. Aggiungere le istruzioni "require" seguenti all'inizio del file **dmpatterns_getstarted_device.js**:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Aggiungere una variabile **connectionString** e usarla per creare un'istanza **Client**.  Sostituire la stringa di connessione con la stringa di connessione del dispositivo.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Aggiungere la funzione seguente per implementare il metodo diretto nel dispositivo
   
    ```
    var onReboot = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };
   
        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
   
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```

7. Aprire la connessione all'hub IoT e avviare il listener del metodo diretto:

   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```

8. Salvare e chiudere il file **dmpatterns_getstarted_device.js**.

> [!NOTE]
> Per semplicità, in questa esercitazione non si implementa alcun criterio di ripetizione dei tentativi. Nel codice di produzione è consigliabile implementare criteri di ripetizione dei tentativi, ad esempio un backoff esponenziale, come suggerito nell'articolo [Gestione degli errori temporanei](/azure/architecture/best-practices/transient-faults).

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Attivare un riavvio remoto nel dispositivo con un metodo diretto

In questa sezione viene creata un'app console Node.js che attiva un riavvio remoto in un dispositivo usando un metodo diretto. L'app esegue query nel dispositivo gemello per ottenere l'ora dell'ultimo riavvio del dispositivo in questione.

1. Creare una cartella vuota denominata **triggerrebootondevice**. Nella cartella **triggerrebootondevice** creare un file package.json eseguendo questo comando al prompt dei comandi. Accettare tutte le impostazioni predefinite:
   
    ```
    npm init
    ```

2. Eseguire questo comando al prompt dei comandi nella cartella **triggerrebootondevice** per installare il pacchetto SDK per dispositivi **azure-iothub** e il pacchetto **azure-iot-device-mqtt**:
   
    ```
    npm install azure-iothub --save
    ```

3. Con un editor di testo creare un file **dmpatterns_getstarted_service.js** nella cartella **triggerrebootondevice**.

4. Aggiungere le istruzioni "require" seguenti all'inizio del file **dmpatterns_getstarted_service.js**:

  
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Aggiungere le dichiarazioni di variabile seguenti e sostituire i valori segnaposto:

   
    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```

6. Aggiungere la funzione seguente per richiamare il metodo per riavviare il dispositivo di destinazione:
   
    ```
    var startRebootDevice = function(twin) {
   
        var methodName = "reboot";
   
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
   
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) {
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```

7. Aggiungere la funzione seguente per eseguire una query per il dispositivo e ottenere l'ora dell'ultimo riavvio:
   
    ```
    var queryTwinLastReboot = function() {
   
        registry.getTwin(deviceToReboot, function(err, twin){
   
            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```

8. Aggiungere il codice seguente per chiamare le funzioni che attivano il metodo diretto per il riavvio ed eseguire una query per ottenere l'ora dell'ultimo riavvio:

   
    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```

9. Salvare e chiudere il file **dmpatterns_getstarted_service.js**.

## <a name="run-the-apps"></a>Eseguire le app

A questo punto è possibile eseguire le app.

1. Al prompt dei comandi nella cartella **manageddevice** eseguire questo comando per iniziare l'ascolto del metodo diretto di riavvio.

   
    ```
    node dmpatterns_getstarted_device.js
    ```

2. Al prompt dei comandi nella cartella **triggerrebootondevice** eseguire questo comando per attivare il riavvio remoto e cercare il dispositivo gemello per trovare l'ora dell'ultimo riavvio.

   
    ```
    node dmpatterns_getstarted_service.js
    ```

3. Nella console viene visualizzata la risposta del dispositivo al metodo diretto.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]
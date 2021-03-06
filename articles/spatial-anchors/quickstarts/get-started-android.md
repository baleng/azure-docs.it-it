---
title: "Guida introduttiva: Creare un'app Android con Ancoraggi nello spazio di Azure | Microsoft Docs"
description: In questa guida introduttiva si apprenderà come creare un'app Android usando Ancoraggi nello spazio.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 8f2a5fdaf2222de7a802e8ff2a1f6fdb37dae4c3
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "57880040"
---
# <a name="quickstart-create-an-android-app-with-azure-spatial-anchors"></a>Guida introduttiva: Creare un'app Android con Ancoraggi nello spazio di Azure

Questa guida introduttiva illustra come creare un'app Android usando [Ancoraggi nello spazio di Azure](../overview.md) in Java o C++/NDK. Ancoraggi nello spazio di Azure è un servizio per lo sviluppo multipiattaforma che consente di creare esperienze di realtà mista usando oggetti la cui posizione persiste tra dispositivi nel corso del tempo. Al termine, si avrà un'app ARCore per Android in grado di salvare e richiamare un ancoraggio nello spazio.

Si apprenderà come:

> [!div class="checklist"]
> * Creare un account di Ancoraggi nello spazio
> * Configurare l'identificatore e la chiave dell'account di Ancoraggi nello spazio
> * Distribuire ed eseguire l'app in un dispositivo Android

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerequisiti

Per completare questa guida introduttiva, accertarsi di disporre di quanto segue:

- Un computer Windows o macOS con <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3+</a> installato.
  - Se in esecuzione su Windows, sarà necessario anche <a href="https://git-scm.com/download/win" target="_blank">Git per Windows</a>.
  - Se in esecuzione su macOS, installare Git tramite HomeBrew. Immettere il comando seguente in una singola riga del terminale: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. Quindi eseguire `brew install git`.
  - Per creare l'esempio in NDK, sarà anche necessario installare gli strumenti NDK e CMake 3.6 SDK in Android Studio.
- Un dispositivo Android <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">abilitato per lo sviluppo</a> e <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">idoneo per ARCore</a>.
- L'app deve essere destinata ad ARCore 1.5 (il supporto per ARCore 1.6+ verrà aggiunto prossimamente)

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Aprire il progetto di esempio

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Se si crea l'esempio in NDK Android, sarà necessario scaricare `arcore_c_api.h` da [qui](https://raw.githubusercontent.com/google-ar/arcore-android-sdk/v1.5.0/libraries/include/arcore_c_api.h) e inserirlo in `Android\NDK\libraries\include`.

Aprire Android Studio.

# <a name="javatabopenproject-java"></a>[Java](#tab/openproject-java)

Selezionare **Open an existing Android Studio project** (Apri un progetto di Android Studio esistente) e selezionare il progetto disponibile in `Android/Java/`.

# <a name="ndktabopenproject-ndk"></a>[NDK](#tab/openproject-ndk)

Selezionare **Open an existing Android Studio project** (Apri un progetto di Android Studio esistente) e selezionare il progetto disponibile in `Android/NDK/`.

***

## <a name="configure-account-identifier-and-key"></a>Configurare l'identificatore e la chiave dell'account

Il passaggio successivo consiste nel configurare l'app in modo da usare l'identificatore e la chiave dell'account. Questi dati sono stati copiati in un editor di testo durante la [configurazione della risorsa Ancoraggi nello spazio](#create-a-spatial-anchors-resource).

# <a name="javatabopenproject-java"></a>[Java](#tab/openproject-java)

Aprire `Android/Java/app/src/main/java/com/microsoft/sampleandroid/AzureSpatialAnchorsActivity.java`.

Individuare il campo `SpatialAnchorsAccountKey` e sostituire `Set me` con la chiave dell'account.

Individuare il campo `SpatialAnchorsAccountId` e sostituire `Set me` con l'identificatore dell'account.

# <a name="ndktabopenproject-ndk"></a>[NDK](#tab/openproject-ndk)

Aprire `Android/NDK/app/src/main/cpp/spatial_services_application.cc`.

Individuare il campo `SpatialAnchorsAccountKey` e sostituire `Set me` con la chiave dell'account.

Individuare il campo `SpatialAnchorsAccountId` e sostituire `Set me` con l'identificatore dell'account.

***

## <a name="deploy-the-app-to-your-android-device"></a>Distribuire l'app nel dispositivo Android

Accendere il dispositivo Android, eseguire l'accesso e connetterlo al PC con un cavo USB.

Selezionare **Run** (Esegui) sulla barra degli strumenti di Android Studio.

![Distribuzione ed esecuzione in Android Studio](./media/get-started-android/android-studio-deploy-run.png)

Selezionare il dispositivo Android nella finestra di dialogo **Select Deployment Target** (Seleziona destinazione distribuzione) e scegliere **OK** per eseguire l'app nel dispositivo Android.

Seguire le istruzioni nell'app per posizionare e richiamare un ancoraggio.

Arrestare l'app selezionando **Stop** (Arresta) sulla barra degli strumenti di Android Studio.

![Arresto dell'app in Android Studio](./media/get-started-android/android-studio-stop.png)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Esercitazione: Condividere gli ancoraggi nello spazio tra dispositivi](../tutorials/tutorial-share-anchors-across-devices.md)

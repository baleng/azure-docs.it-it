---
title: Come distribuire le dipendenze cloud di gestione dispositivi Azure IoT OPC UA | Microsoft Docs
description: Come distribuire le dipendenze OPC dei dispositivi gemelli di Azure.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: cdd89293328da47c8e338bb6fca8c7b93dd84f97
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759553"
---
# <a name="deploying-dependencies-for-local-development"></a>Distribuzione di dipendenze per lo sviluppo locale

Questo articolo illustra come distribuire solo la necessità di servizi della piattaforma Azure di eseguire operazioni di sviluppo e debug locale.   Al termine, sarà necessario un gruppo di risorse distribuito che contiene tutto ciò che occorre per sviluppo e debug locale.

## <a name="deploy-azure-platform-services"></a>Distribuire servizi della piattaforma Azure

1. Assicurarsi di disporre di PowerShell e [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-1.1.0) estensioni installate.  Aprire un prompt dei comandi o terminale ed eseguire:

   ```bash
   git clone https://github.com/Azure/azure-iiot-components
   cd azure-iiot-components
   ```

   ```bash
   deploy -type local
   ```

2. Seguire le istruzioni per assegnare un nome al gruppo di risorse per la distribuzione.  Lo script distribuisce solo le dipendenze per gruppo di risorse nella sottoscrizione di Azure, ma non i microservizi.  Lo script registra inoltre un'applicazione in Azure Active Directory.  Questa operazione è necessaria per supportare l'autenticazione basata su OAUTH.  Distribuzione può richiedere alcuni minuti.

3. Una volta completato lo script, è possibile selezionare per salvare il file con estensione env.  Il file con estensione env dell'ambiente è il file di configurazione di tutti i servizi e strumenti da eseguire nel computer di sviluppo.  

## <a name="troubleshooting-deployment-failures"></a>Risoluzione degli errori di distribuzione

### <a name="resource-group-name"></a>Nome del gruppo di risorse

Assicurarsi di che usare il nome di un gruppo di risorse a breve e semplice.  Il nome viene usato anche assegnare nomi alle risorse di conseguenza che deve rispondere con i requisiti di denominazione.  

### <a name="azure-active-directory-aad-registration"></a>Registrazione con Azure Active Directory (AAD)

Lo script di distribuzione tenta di registrare le applicazioni di AAD in Azure Active Directory.  A seconda di diritti per il tenant di AAD selezionato, questo potrebbe non riuscire.   Sono disponibili 3 opzioni:

1. Se si sceglie un tenant di AAD da un elenco di tenant, riavviare lo script e scegliere una diversa dall'elenco.
2. In alternativa, distribuire un tenant AAD privato, riavviare lo script e selezionare questa opzione per usarlo.
3. Continuare senza autenticazione.  Poiché si esegue localmente i microservizi, questo è accettabile, ma non possono simulare gli ambienti di produzione.  

## <a name="next-steps"></a>Passaggi successivi

Ora che servizi di gestione dei dispositivi OPC è stata distribuita in un progetto esistente, ecco il passaggio successivo consigliato:

> [!div class="nextstepaction"]
> [Informazioni su come distribuire i moduli di gestione dei dispositivi OPC](howto-opc-twin-deploy-modules.md)

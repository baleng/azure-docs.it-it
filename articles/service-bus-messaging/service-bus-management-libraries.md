---
title: Librerie di gestione del bus di servizio di Azure | Microsoft Docs
description: Gestire entità di messaggistica e spazi dei nomi del bus di servizio da .NET.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/05/2018
ms.author: aschhab
ms.openlocfilehash: d6c2cd813e96b40fc9f95785a1fd28e324a0437b
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54850961"
---
# <a name="service-bus-management-libraries"></a>Librerie di gestione del bus di servizio

Le librerie di gestione del bus di servizio di Azure possono eseguire il provisioning di entità e spazi dei nomi del bus di servizio in modo dinamico, per consentire distribuzioni complesse e scenari di messaggistica e permettere di determinare a livello di codice le entità di cui eseguire il provisioning. Queste librerie sono attualmente disponibili per .NET.

## <a name="supported-functionality"></a>Funzionalità supportate

* Creazione, aggiornamento, eliminazione di spazi dei nomi
* Creazione, aggiornamento, eliminazione di code
* Creazione, aggiornamento, eliminazione di argomenti
* Creazione, aggiornamento, eliminazione di sottoscrizioni

## <a name="prerequisites"></a>Prerequisiti

Per iniziare a usare le librerie di gestione del bus di servizio, è necessario eseguire l'autenticazione con il servizio Azure Active Directory (Azure AD). Azure AD richiede l'autenticazione come entità servizio, che fornisce l'accesso alle risorse di Azure in uso. Per informazioni su come creare un'entità servizio, vedere uno di questi articoli:  

* [Usare il portale di Azure per creare un'applicazione Active Directory e un'entità servizio che accedono alle risorse](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [Usare Azure PowerShell per creare un'entità servizio per accedere alle risorse](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Usare l'interfaccia della riga di comando di Azure per creare un'entità servizio per accedere alle risorse](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

Nel corso di queste esercitazioni vengono forniti un `AppId` (ID client), un `TenantId` e un `ClientSecret` (chiave di autenticazione) che sono usati per l'autenticazione da parte delle librerie di gestione. È necessario disporre delle autorizzazioni di **Proprietario** per il gruppo di risorse in cui verranno eseguite le librerie.

## <a name="programming-pattern"></a>Modello di programmazione

Il modello di modifica delle risorse del bus di servizio segue un protocollo comune:

1. Ottenere un token da Azure AD usando la libreria **Microsoft.IdentityModel.Clients.ActiveDirectory**:
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.core.windows.net/", new ClientCredential(clientId, clientSecret));
   ```
2. Creare l'oggetto `ServiceBusManagementClient`:

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```
3. Impostare i parametri `CreateOrUpdate` sui valori specificati:

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```
4. Eseguire la chiamata:

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="next-steps"></a>Passaggi successivi

* [Esempio di gestione .NET](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [Informazioni di riferimento sull'API Microsoft.Azure.Management.ServiceBus](/dotnet/api/Microsoft.Azure.Management.ServiceBus)

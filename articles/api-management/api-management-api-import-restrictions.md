---
title: Limitazioni e problemi noti dell'importazione dell'API di Gestione API di Azure | Microsoft Docs
description: Dettagli di problemi noti e limitazioni relative all'importazione in Gestione API di Azure con i formati Open API, WSDL o WADL.
services: api-management
documentationcenter: ''
author: vladvino
manager: vlvinogr
editor: ''
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: apipm
ms.openlocfilehash: 251caa840446e75ff13d9b4dcebcbae3a36473c8
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2019
ms.locfileid: "54265448"
---
# <a name="api-import-restrictions-and-known-issues"></a>Problemi noti e limitazioni dell'importazione dell'API
## <a name="about-this-list"></a>Informazioni sull'elenco
Quando si importa un'API è possibile riscontrare delle restrizioni o identificare dei problemi che è necessario correggere per completare l'importazione. L'articolo illustra questi aspetti, organizzati in base al formato di importazione dell'API.

## <a name="open-api"> </a>OpenAPI/Swagger

Se vengono restituiti errori durante l'importazione del documento OpenAPI, verificare che sia stato prima convalidato. A tale scopo, usare la finestra di progettazione nel portale di Azure (Progettazione - Front End - Editor della specifica OpenAPI) o usare uno strumento di terze parti come <a href="https://editor.swagger.io">Swagger Editor</a>.

### <a name="open-api-general"> </a>Generale

* I parametri obbligatori per percorso e query devono avere nomi univoci. In OpenAPI un nome di parametro deve essere univoco solo all'interno di una determinata posizione, ad esempio percorso, query, intestazione. In Gestione API, invece, le operazioni possono essere discriminate in base a parametri sia di percorso che di query, cosa che non è possibile in OpenAPI. Per questo motivo è necessario che i nomi dei parametri siano univoci all'interno dell'intero modello di URL.
* I puntatori **$ref** non possono fare riferimento a file esterni.
* **x-ms-paths** e **x-servers** sono le uniche estensioni supportate.
* Le estensioni personalizzate vengono ignorate in fase di importazione e non vengono salvate o mantenute per l'esportazione.
* **Ricorsione**: Gestione API non supporta le definizioni con carattere ricorsivo, ad esempio gli schemi che fanno riferimento a se stessi.
* L'URL del file di origine, se disponibile, viene applicato agli URL di server relativi.

### <a name="open-api-v2"> </a>OpenAPI versione 2

* È supportato solo il formato JSON.

### <a name="open-api-v3"> </a>OpenAPI versione 3

* Se sono specificati molti **server**, Gestione API proverà a selezionare il primo URL HTTPs. Se non vi sono URL HTTPs, selezionerà il primo URL HTTP. Se non vi sono nemmeno URL HTTP, l'URL del server sarà vuoto.
* **Examples** non è supportato, mentre **example** lo è.
* **Multipart/form-data** non è supportato.

> [!IMPORTANT]
> Vedere questo [documento](https://blogs.msdn.microsoft.com/apimanagement/2018/04/11/important-changes-to-openapi-import-and-export/) per informazioni importanti e suggerimenti correlati all'importazione OpenAPI.

## <a name="wsdl"></a>WSDL
I file WSDL vengono usati per generare le API SOAP pass-through o come back-end dell'API SOAP-REST.
* **Associazioni SOAP**: solo le associazioni SOAP con stile di codifica "document" e "literal" sono supportate. Non è disponibile il supporto per lo stile di codifica SOAP "rpc".
* **WSDL:Import**: questo attributo non è supportato. I clienti devono unire le importazioni in un solo documento.
* **Messaggi con più parti**: questo tipo di messaggi non è supportato.
* **wsHttpBinding di WCF**: i servizi SOAP creati con Windows Communication Foundation devono usare basicHttpBinding poiché wsHttpBinding non è supportato.
* **MTOM**: i servizi che usano MTOM <em>potrebbero</em> funzionare. Al momento il supporto ufficiale non è previsto.
* **Ricorsione**: i tipi definiti in modo ricorsivo, che, ad esempio, fanno riferimento a una matrice di se stessi, non sono supportati da APIM.
* **Più spazi dei nomi**: possono essere usati più spazi dei nomi in uno schema, ma solo lo spazio dei nomi di destinazione può essere usato per definire le parti del messaggio. Gli spazi dei nomi diversi dalla destinazione, che vengono usati per definire altri elementi di input o output non vengono mantenuti. Sebbene un documento WSDL di questo tipo possa essere importato, in caso di esportazione tutte le parti del messaggio avranno lo spazio dei nomi di destinazione del documento WSDL.

## <a name="wadl"> </a>WADL
Attualmente non sono noti problemi di importazione del formato WADL.

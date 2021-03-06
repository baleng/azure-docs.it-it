---
title: Introduzione all'API di Azure Cosmos DB per MongoDB
description: Informazioni su come usare Azure Cosmos DB per archiviare ed eseguire query su grandi quantità di dati tramite l'API di Azure Cosmos DB per MongoDB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: overview
ms.date: 12/26/2018
author: sivethe
ms.author: sivethe
ms.openlocfilehash: d0f61afaba094a1e499a91f9937a31554438759e
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54042173"
---
# <a name="azure-cosmos-dbs-api-for-mongodb"></a>API Azure Cosmos DB per MongoDB

[Azure Cosmos DB](introduction.md) è il servizio di database multimodello distribuito a livello globale di Microsoft per applicazioni cruciali. Azure Cosmos DB offre [distribuzione globale predefinita](distribute-data-globally.md), [scalabilità elastica in termini di archiviazione e velocità effettiva](partition-data.md) ovunque nel mondo, latenze pari a singole unità di millisecondi al 99° percentile e disponibilità elevata garantita, il tutto supportato da [contratti di servizio leader del settore](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB [indicizza automaticamente i dati](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) senza che sia necessario gestire manualmente indici e schemi. Si tratta di un database multimodello che supporta modelli di dati di documenti, coppie chiave/valore, grafi e colonne. Per impostazione predefinita, è possibile interagire con Cosmos DB tramite l'API SQL. Inoltre, il servizio Cosmos DB implementa i protocolli di collegamento per le API NoSQL più comuni, tra cui Cassandra, MongoDB, Gremlin e Archiviazione tabelle di Azure. In questo modo, è possibile usare gli strumenti e i driver dei client NoSQL con cui si ha familiarità per interagire con il database Cosmos.

## <a name="wire-protocol-compatibility"></a>Compatibilità del protocollo di collegamento

Azure Cosmos DB implementa i protocolli di collegamento dei database NoSQL più comuni, tra cui Cassandra, MongoDB, Gremlin e Archiviazione tabelle di Azure. Fornendo un'implementazione nativa dei protocolli di collegamento in modo efficiente e direttamente in Azure Cosmos DB, consente a SDK client, driver e strumenti esistenti dei database NoSQL di interagire con Cosmos DB in modo trasparente. Cosmos DB non usa il codice sorgente dei database per fornire le API con compatibilità di collegamento per i database NoSQL.

Per impostazione predefinita, l'API di Azure Cosmos DB per MongoDB è compatibile con la versione 3.2 del protocollo di collegamento di MongoDB. Le funzionalità o gli operatori di query aggiunti nella versione 3.4 del protocollo di collegamento sono attualmente disponibili come funzionalità in anteprima. I driver client MongoDB che identificano queste versioni dei protocolli dovrebbero essere in grado di collegarsi in modo nativo a Cosmos DB.

![API Azure Cosmos DB per MongoDB](./media/mongodb-introduction/cosmosdb-mongodb.png) 

## <a name="key-benefits"></a>Vantaggi principali

I vantaggi principali di Cosmos DB come database distribuito come servizio a livello globale e completamente gestito sono descritti [qui](introduction.md). Inoltre, implementando in modo nativo i protocolli di collegamento delle API NoSQL più comuni, Cosmos DB offre i vantaggi seguenti:

* Migrare facilmente l'applicazione in Cosmos DB, mantenendo al tempo stesso parti significative della logica dell'applicazione.
* Mantenere l'applicazione portabile e continuare a rimanere indipendenti dal fornitore di cloud.
* Ottenere contratti di servizio leader del settore e supportati finanziariamente per le API NoSQL più comuni basate su Cosmos DB.
* Ridimensionare in modo elastico la velocità effettiva e le risorse di archiviazione per i database Cosmos in base alle proprie esigenze e pagare solo quello che è necessario. Ciò comporta risparmi significativi sui costi.
* Distribuzione globale chiavi in mano con funzionalità di replica multimaster.

## <a name="cosmos-dbs-api-for-mongodb"></a>API di Cosmos DB per MongoDB

Seguire le guide rapide per creare un account Cosmos ed eseguire la migrazione dell'applicazione MongoDB esistente per usare Azure Cosmos DB o compilarne uno nuovo:

* [Eseguire la migrazione di un'app Web Node.js MongoDB esistente](create-mongodb-nodejs.md).
* [Creare un'app Web usando l'API Azure Cosmos DB per MongoDB e .NET SDK](create-mongodb-dotnet.md)
* [Creare un'app console usando l'API Azure Cosmos DB per MongoDB e Java SDK](create-mongodb-java.md)

## <a name="next-steps"></a>Passaggi successivi

Ecco alcuni riferimenti per iniziare:

* Per istruzioni su come ottenere le informazioni della stringa di connessione dell'account, seguire l'esercitazione [Connettere un'applicazione MongoDB ad Azure Cosmos DB](connect-mongodb-account.md).
* Per informazioni su come creare una connessione tra i database Cosmos e l'app MongoDB in Studio 3T, vedere l'esercitazione [Usare Studio 3T con Azure Cosmos DB](mongodb-mongochef.md).
* Per importare i dati in un database Cosmos, vedere l'esercitazione [Importare i dati di MongoDB in Azure Cosmos DB](mongodb-migrate.md).
* Connettersi a un account Cosmos usando [Robo 3T](mongodb-robomongo.md).
* Informazioni su come [configurare le preferenze di lettura per le app distribuite globalmente](../cosmos-db/tutorial-global-distribution-mongodb.md).

<sup>Nota: Questo articolo illustra una funzionalità di Azure Cosmos DB che fornisce la compatibilità del protocollo di collegamento con i database di MongoDB. Microsoft non esegue database MongoDB per fornire questo servizio. Azure Cosmos DB non è affiliato a MongoDB, Inc.</sup>

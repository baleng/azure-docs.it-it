---
title: Introduzione agli strumenti di sviluppo per archiviazione di Azure Stack | Microsoft Docs
description: Indicazioni per iniziare a usare gli strumenti di sviluppo di archiviazione di Azure Stack
services: azure-stack
author: mattbriggs
ms.author: mabrigg
ms.date: 02/27/2019
ms.topic: conceptual
ms.service: azure-stack
manager: femila
ms.reviewer: xiaofmao
ms.lastreviewed: 02/27/2019
ms.openlocfilehash: 1640e06d2d6eec19d516fb3ddf0e98c579e667a7
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/18/2019
ms.locfileid: "58080795"
---
# <a name="get-started-with-azure-stack-storage-development-tools"></a>Introduzione agli strumenti di sviluppo per archiviazione di Azure Stack

*Si applica a: Azure Stack Development Kit e i sistemi integrati di Azure Stack*

Microsoft Azure Stack offre un set di servizi di archiviazione che include i blob, tabelle e archiviazione code.

Usare questo articolo come guida per iniziare a usare gli strumenti di sviluppo di archiviazione di Azure Stack. È possibile trovare ulteriori informazioni e codice di esempio nelle esercitazioni di archiviazione di Azure corrispondenti.

> [!NOTE]  
> Esistono differenze note tra l'archiviazione di Azure Stack e archiviazione di Azure, inclusi i requisiti specifici per ogni piattaforma. Ad esempio, esistono requisiti per il suffisso dell'endpoint specifico per Azure Stack e le librerie client specifiche. Per altre informazioni, vedere [archiviazione di Azure Stack: Differenze e considerazioni](azure-stack-acs-differences.md).

## <a name="azure-client-libraries"></a>Librerie client di Azure

Per le librerie client di archiviazione, tenere presente la versione compatibile con l'API REST. È anche necessario specificare l'endpoint di Azure Stack nel codice.

### <a name="1811-update-or-newer-versions"></a>1811 update o versioni successive

| Libreria client | Versione supportata di Azure Stack | Collegamento | Specifica dell'endpoint |
|----------------|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET | 9.2.0 | Pacchetto NuGet:<br>https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0<br> <br>Versione di GitHub:<br>https://github.com/Azure/azure-storage-net/releases/tag/v9.2.0 | file app.config |
| Java | 7.0.0 | Pacchetto Maven:<br>https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/7.0.0<br> <br>Versione di GitHub:<br>https://github.com/Azure/azure-storage-java/releases/tag/v7.0.0 | Configurazione della stringa di connessione |
| Node.js | 2.8.3 | Collegamento NPM:<br>https://www.npmjs.com/package/azure-storage<br>(Eseguire: `npm install azure-storage@2.8.3`)<br> <br>Versione di Github:<br>https://github.com/Azure/azure-storage-node/releases/tag/v2.8.3 | Dichiarazione istanza del servizio |
| C++ | 5.2.0 | Pacchetto NuGet:<br>https://www.nuget.org/packages/Microsoft.Azure.Storage.CPP.v140/5.2.0<br> <br>Versione di GitHub:<br>https://github.com/Azure/azure-storage-cpp/releases/tag/v5.2.0 | Configurazione della stringa di connessione |
| PHP | 1.2.0 | Versione di GitHub:<br>Comuni: https://github.com/Azure/azure-storage-php/releases/tag/v1.2.0-common<br>BLOB: https://github.com/Azure/azure-storage-php/releases/tag/v1.2.0-blob<br>Coda:<br>https://github.com/Azure/azure-storage-php/releases/tag/v1.1.1-queue<br>tavolo: https://github.com/Azure/azure-storage-php/releases/tag/v1.1.0-table<br> <br>Installazione tramite Composer (per altre informazioni, [vedere i dettagli seguenti](#install-php-client-via-composer---current).) | Configurazione della stringa di connessione |
| Python | 1.1.0 | Versione di GitHub:<br>Comuni:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.1.0-common<br>BLOB:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.1.0-blob<br>Coda:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.1.0-queue | Dichiarazione istanza del servizio |
| Ruby | 1.0.1 | Pacchetto RubyGems:<br>Comuni:<br>https://rubygems.org/gems/azure-storage-common/versions/1.0.1<br>BLOB: https://rubygems.org/gems/azure-storage-blob/versions/1.0.1<br>Coda: https://rubygems.org/gems/azure-storage-queue/versions/1.0.1<br>tavolo: https://rubygems.org/gems/azure-storage-table/versions/1.0.1<br> <br>Versione di GitHub:<br>Comuni: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-common<br>BLOB: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-blob<br>Coda: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-queue<br>tavolo: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-table | Configurazione della stringa di connessione |

#### <a name="install-php-client-via-composer---current"></a>Installare il client PHP tramite Composer - corrente

Per l'installazione tramite Composer: (richiedere il blob come esempio).

1. Creare un file denominato **Composer. JSON** nella radice del progetto con il codice seguente:

    ```json
    {
      "require": {
      "Microsoft/azure-storage-blob":"1.2.0"
      }
    }
    ```

2. Scaricare [Phar](https://getcomposer.org/composer.phar) alla radice del progetto.
3. Eseguire: `php composer.phar install`.

### <a name="previous-versions-1802-to-1809-update"></a>Versioni precedenti (aggiornamento 1802 per 1809)

| Libreria client | Versione supportata di Azure Stack | Collegamento | Specifica dell'endpoint |
|----------------|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET | 8.7.0 | Pacchetto NuGet:<br>https://www.nuget.org/packages/WindowsAzure.Storage/8.7.0<br> <br>Versione di GitHub:<br>https://github.com/Azure/azure-storage-net/releases/tag/v8.7.0 | file app.config |
| Java | 6.1.0 | Pacchetto Maven:<br>http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/6.1.0<br> <br>Versione di GitHub:<br>https://github.com/Azure/azure-storage-java/releases/tag/v6.1.0 | Configurazione della stringa di connessione |
| Node.js | 2.7.0 | Collegamento NPM:<br>https://www.npmjs.com/package/azure-storage<br>(Eseguire: `npm install azure-storage@2.7.0`)<br> <br>Versione di Github:<br>https://github.com/Azure/azure-storage-node/releases/tag/v2.7.0 | Dichiarazione istanza del servizio |
| C++ | 3.1.0 | Pacchetto NuGet:<br>https://www.nuget.org/packages/wastorage.v140/3.1.0<br> <br>Versione di GitHub:<br>https://github.com/Azure/azure-storage-cpp/releases/tag/v3.1.0 | Configurazione della stringa di connessione |
| PHP | 1.0.0 | Versione di GitHub:<br>Comuni: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-common<br>BLOB: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-blob<br>Coda:<br>https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-queue<br>tavolo: https://github.com/Azure/azure-storage-php/releases/tag/v1.0.0-table<br> <br>Installazione tramite Composer (vedere i dettagli sotto).) | Configurazione della stringa di connessione |
| Python | 1.0.0 | Versione di GitHub:<br>Comuni:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-common<br>BLOB:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-blob<br>Coda:<br>https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-queue | Dichiarazione istanza del servizio |
| Ruby | 1.0.1 | Pacchetto RubyGems:<br>Comuni:<br>https://rubygems.org/gems/azure-storage-common/versions/1.0.1<br>BLOB: https://rubygems.org/gems/azure-storage-blob/versions/1.0.1<br>Coda: https://rubygems.org/gems/azure-storage-queue/versions/1.0.1<br>tavolo: https://rubygems.org/gems/azure-storage-table/versions/1.0.1<br> <br>Versione di GitHub:<br>Comuni: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-common<br>BLOB: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-blob<br>Coda: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-queue<br>tavolo: https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-table | Configurazione della stringa di connessione |

#### <a name="install-php-client-via-composer---previous"></a>Installare il client PHP tramite Composer - precedente

Per l'installazione tramite Composer: (prendere BLOB come esempio).

1. Creare un file denominato **Composer. JSON** nella radice del progetto con il codice seguente:

   ```json
    {
      "require": {
      "Microsoft/azure-storage-blob":"1.0.0"
      }
    }
   ```

2. Scaricare [Phar](https://getcomposer.org/composer.phar) alla radice del progetto.
3. Eseguire: `php composer.phar install`.

## <a name="endpoint-declaration"></a>Dichiarazione dell'endpoint

Un endpoint di Azure Stack include due parti: il nome di un'area e il dominio di Azure Stack.
In Azure Stack Development Kit, è l'endpoint predefinito **local.azurestack.external**.
Se non si è certi sull'endpoint della, contattare l'amministratore del cloud.

## <a name="examples"></a>Esempi

### <a name="net"></a>.NET

Per Azure Stack, il suffisso dell'endpoint viene specificato nel file app. config:

```xml
<add key="StorageConnectionString"
value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;
EndpointSuffix=local.azurestack.external;" />
```

### <a name="java"></a>Java

Per Azure Stack, il suffisso dell'endpoint viene specificato nell'impostazione della stringa di connessione:

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key;" +
    "EndpointSuffix=local.azurestack.external";
```

### <a name="nodejs"></a>Node.js

Per Azure Stack, il suffisso dell'endpoint viene specificato nell'istanza di dichiarazione:

```nodejs
var blobSvc = azure.createBlobService('myaccount', 'mykey',
'myaccount.blob.local.azurestack.external');
```

### <a name="c"></a>C++

Per Azure Stack, il suffisso dell'endpoint viene specificato nell'impostazione della stringa di connessione:

```cpp
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;
AccountName=your_storage_account;
AccountKey=your_storage_account_key;
EndpointSuffix=local.azurestack.external"));
```

### <a name="php"></a>PHP

Per Azure Stack, il suffisso dell'endpoint viene specificato nell'impostazione della stringa di connessione:

```php
$connectionString = 'BlobEndpoint=http://<storage account name>.blob.local.azurestack.external/;
QueueEndpoint=http:// <storage account name>.queue.local.azurestack.external/;
TableEndpoint=http:// <storage account name>.table.local.azurestack.external/;
AccountName=<storage account name>;AccountKey=<storage account key>'
```

### <a name="python"></a>Python

Per Azure Stack, il suffisso dell'endpoint viene specificato nell'istanza di dichiarazione:

```python
block_blob_service = BlockBlobService(account_name='myaccount',
account_key='mykey',
endpoint_suffix='local.azurestack.external')
```

### <a name="ruby"></a>Ruby

Per Azure Stack, il suffisso dell'endpoint viene specificato nell'impostazione della stringa di connessione:

```ruby
set
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;
AccountName=myaccount;
AccountKey=mykey;
EndpointSuffix=local.azurestack.external
```

## <a name="blob-storage"></a>Archiviazione BLOB

Le esercitazioni di archiviazione Blob di Azure seguenti sono applicabili ad Azure Stack. Si noti il requisito di suffisso dell'endpoint specifico per Azure Stack descritto nella precedente [esempi](#examples) sezione.

* [Introduzione all'archiviazione BLOB di Azure con .NET](../../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [Come usare l'archiviazione BLOB da Java](../../storage/blobs/storage-java-how-to-use-blob-storage.md)
* [Come usare l'archiviazione BLOB da Node.js](../../storage/blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Come usare l'archiviazione BLOB da C++](../../storage/blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Come usare l'archiviazione BLOB da PHP](../../storage/blobs/storage-php-how-to-use-blobs.md)
* [Come usare l'archiviazione Blob di Azure da Python](../../storage/blobs/storage-python-how-to-use-blob-storage.md)
* [Come usare l'archiviazione BLOB da Ruby](../../storage/blobs/storage-ruby-how-to-use-blob-storage.md)

## <a name="queue-storage"></a>Archiviazione code

Le esercitazioni di archiviazione code di Azure seguenti sono applicabili ad Azure Stack. Si noti il requisito di suffisso dell'endpoint specifico per Azure Stack descritto nella precedente [esempi](#examples) sezione.

* [Introduzione all'archiviazione code di Azure con .NET](../../storage/queues/storage-dotnet-how-to-use-queues.md)
* [Come usare l'archiviazione di accodamento da Java](../../storage/queues/storage-java-how-to-use-queue-storage.md)
* [Come usare l'archiviazione di accodamento da Node.js](../../storage/queues/storage-nodejs-how-to-use-queues.md)
* [Come usare l'archiviazione delle code da C++](../../storage/queues/storage-c-plus-plus-how-to-use-queues.md)
* [Come usare l'archiviazione di accodamento da PHP](../../storage/queues/storage-php-how-to-use-queues.md)
* [Come usare l'archiviazione di accodamento da Python](../../storage/queues/storage-python-how-to-use-queue-storage.md)
* [Come usare l'archiviazione di accodamento da Ruby](../../storage/queues/storage-ruby-how-to-use-queue-storage.md)

## <a name="table-storage"></a>Archiviazione tabelle

Le esercitazioni di archiviazione tabelle di Azure seguenti sono applicabili ad Azure Stack. Si noti il requisito di suffisso dell'endpoint specifico per Azure Stack descritto nella precedente [esempi](#examples) sezione.

* [Introduzione all'archiviazione tabelle di Azure con .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Come usare l'archiviazione tabelle da Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Come usare l'archiviazione tabelle di Azure da Node.js](../../cosmos-db/table-storage-how-to-use-nodejs.md)
* [Come usare l'archiviazione tabelle da C++](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Come usare l'archiviazione tabelle da PHP](../../cosmos-db/table-storage-how-to-use-php.md)
* [Come usare l'archiviazione tabelle in Python](../../cosmos-db/table-storage-how-to-use-python.md)
* [Come usare l'archiviazione tabelle da Ruby](../../cosmos-db/table-storage-how-to-use-ruby.md)

## <a name="next-steps"></a>Passaggi successivi

* [Introduzione ad archiviazione di Microsoft Azure](../../storage/common/storage-introduction.md)

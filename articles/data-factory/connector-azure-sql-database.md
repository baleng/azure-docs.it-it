---
title: Copiare dati da o verso il database SQL di Azure usando Data Factory | Microsoft Docs
description: Informazioni su come copiare dati da archivi dati di origine supportati al database SQL di Azure o dal database SQL ad archivi dati sink supportati usando Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: jingwang
ms.openlocfilehash: d0ecf6a48735ec2ba1623f97d4760d230a6e6fbf
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59266315"
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Copiare dati da o nel database SQL di Azure tramite Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you use:"]
> * [Versione 1](v1/data-factory-azure-sql-connector.md)
> * [Versione corrente](connector-azure-sql-database.md)

Questo articolo illustra come usare l'attività di copia in Azure Data Factory per copiare i dati da o verso il database SQL di Azure. Si basa sull'articolo di [panoramica dell'attività di copia](copy-activity-overview.md), che presenta informazioni generali sull'attività di copia.

## <a name="supported-capabilities"></a>Funzionalità supportate

È possibile copiare dati da o verso un database SQL di Azure a qualsiasi archivio dati sink supportato. È anche possibile copiare dati da qualsiasi archivio dati di origine supportato al database SQL di Azure. Per un elenco degli archivi dati supportati come origini o sink dall'attività di copia, vedere la tabella [Archivi dati e formati supportati](copy-activity-overview.md#supported-data-stores-and-formats).

In particolare, il connettore del database SQL di Azure supporta queste funzioni:

- La copia di dati tramite l'autenticazione SQL e l'autenticazione token dell'applicazione Azure Active Directory (Azure AD) con entità servizio o identità gestite per le risorse di Azure.
- Come origine, il recupero di dati tramite query SQL o stored procedure.
- Come sink, aggiungere dati alla tabella di destinazione o richiamare una stored procedure con logica personalizzata durante la copia.

La funzionalità [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017) di database SQL Server non è attualmente supportata. 

> [!IMPORTANT]
> Se si copiano i dati tramite il runtime di integrazione di Azure Data Factory, configurare un [firewall del server SQL di Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) per consentire ai servizi di Azure di accedere al server.
> Se si copiano dati usando un runtime di integrazione self-hosted, configurare il firewall del server SQL di Azure per consentire l'intervallo IP appropriato. Questo intervallo include l'indirizzo IP del computer usato per connettersi al database SQL di Azure.

## <a name="get-started"></a>Attività iniziali

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Le sezioni seguenti riportano informazioni dettagliate sulle proprietà usate per definire entità di Data Factory specifiche in un connettore del database SQL di Azure.

## <a name="linked-service-properties"></a>Proprietà del servizio collegato

Per un servizio collegato al database SQL di Azure sono supportate queste proprietà:

| Proprietà | Descrizione | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà **type** deve essere impostata su **AzureSqlDatabase**. | Sì |
| connectionString | Specifica le informazioni necessarie per connettersi all'istanza del database SQL di Azure per la proprietà **connectionString**. <br/>Contrassegnare questo campo come SecureString per archiviare la chiave in modo sicuro in Data Factory. È anche possibile inserire la password/chiave entità servizio in Azure Key Vault e, se si tratta dell'autenticazione SQL, estrarre la configurazione `password` dalla stringa di connessione. Vedere gli esempi JSON sotto la tabella e l'articolo [Archiviare le credenziali in Azure Key Vault](store-credentials-in-key-vault.md) per altri dettagli. | Sì |
| servicePrincipalId | Specificare l'ID client dell'applicazione. | Sì, quando si usa l'autenticazione Azure AD con un'entità servizio. |
| servicePrincipalKey | Specificare la chiave dell'applicazione. Contrassegnare questo campo come **SecureString** per archiviarlo in modo sicuro in Data Factory oppure [fare riferimento a un segreto archiviato in Azure Key Vault](store-credentials-in-key-vault.md). | Sì, quando si usa l'autenticazione Azure AD con un'entità servizio. |
| tenant | Specificare le informazioni sul tenant (nome di dominio o ID tenant) in cui si trova l'applicazione. Recuperarle passando il cursore del mouse sull'angolo superiore destro del portale di Azure. | Sì, quando si usa l'autenticazione Azure AD con un'entità servizio. |
| connectVia | [Runtime di integrazione](concepts-integration-runtime.md) da usare per la connessione all'archivio dati. È possibile usare il runtime di integrazione di Azure o il runtime di integrazione self-hosted se l'archivio dati si trova in una rete privata. Se non specificato, viene usato il runtime di integrazione di Azure predefinito. | No  |

Per altri tipi di autenticazione, fare riferimento alle sezioni seguenti relative, rispettivamente, ai prerequisiti e agli esempi JSON:

- [Autenticazione in SQL](#sql-authentication)
- [Autenticazione del token dell'applicazione Azure AD: Entità servizio](#service-principal-authentication)
- [Autenticazione del token dell'applicazione Azure AD: Identità gestite per le risorse di Azure](#managed-identity)

>[!TIP]
>Se viene restituito l'errore con codice "UserErrorFailedToConnectToSqlServer" e un messaggio quale "Il limite di sessioni per il database è XXX ed è stato raggiunto.", aggiungere `Pooling=false` alla stringa di connessione e riprovare.

### <a name="sql-authentication"></a>Autenticazione in SQL

#### <a name="linked-service-example-that-uses-sql-authentication"></a>Esempio di servizio collegato tramite l'autenticazione SQL

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Password in Azure Key Vault:** 

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Autenticazione di un'entità servizio

Per usare l'autenticazione token dell'applicazione di Azure AD basata sull'entità servizio, seguire questa procedura:

1. **[Creare un'applicazione di Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)** nel portale di Azure. Prendere nota del nome dell'applicazione e dei valori seguenti che definiscono il servizio collegato:

    - ID applicazione
    - Chiave applicazione
    - ID tenant

1. **[Effettuare il provisioning di un amministratore di Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)** per il server SQL di Azure nel portale di Azure, se l'operazione non è già stata eseguita. L'amministratore di Azure AD deve essere un utente o un gruppo di Azure AD, ma non può essere un'entità servizio. Questo passaggio viene eseguito in modo che, nel passaggio successivo, sia possibile usare un'identità di Azure AD per creare un utente di database indipendente per l'entità servizio.

1. **[Creare utenti del database indipendente](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)** per l'entità servizio. Connettersi al database dal quale o verso il quale si desidera copiare i dati usando strumenti come SSMS, con un'identità di Azure AD con almeno l'autorizzazione ALTER ANY USER. Eseguire il T-SQL seguente: 
    
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

1. **Concedere all'entità servizio le autorizzazioni necessarie**, come si fa di norma per gli utenti SQL o altri utenti. Eseguire il codice seguente:

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

1. **Configurare un servizio collegato al database SQL di Azure** in Azure Data Factory.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Esempio di servizio collegato tramite l'autenticazione basata su entità servizio

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            },
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a>Autenticazione di identità gestite per le risorse di Azure

Una data factory può essere associata a un'[identità gestita per le risorse di Azure](data-factory-service-identity.md), che rappresenta la data factory specifica. È possibile usare questa identità gestita per l'autenticazione del Database SQL di Azure. La factory designata può accedere ai dati e copiarli da o verso il database tramite questa identità.

Per usare l'autenticazione identità gestita, seguire questa procedura:

1. **Creare un gruppo in Azure AD.** Impostare l'identità gestita come membro del gruppo.
    
   1. Trovare l'identità di data factory gestito dal portale di Azure. Accedere alle **proprietà** della data factory. Copiare l'ID IDENTITÀ DEL SERVIZIO.
    
   1. Installare il [modulo Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2). Accedere usando il comando `Connect-AzureAD`. Eseguire i comandi seguenti per creare un gruppo e aggiungere l'identità gestita come un membro.
      ```powershell
      $Group = New-AzureADGroup -DisplayName "<your group name>" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
      Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId "<your data factory managed identity object ID>"
      ```
    
1. **[Effettuare il provisioning di un amministratore di Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)** per il server SQL di Azure nel portale di Azure, se l'operazione non è già stata eseguita. L'amministratore di Azure AD può essere un utente o un gruppo di Azure AD. Se si concede al gruppo con identità gestita un ruolo di amministratore, ignorare i passaggi 3 e 4. L'amministratore avrà accesso completo al database.

1. **[Creare utenti del database indipendente](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)** per il gruppo di Azure AD. Connettersi al database dal quale o verso il quale si desidera copiare i dati usando strumenti come SSMS, con un'identità di Azure AD con almeno l'autorizzazione ALTER ANY USER. Eseguire il T-SQL seguente: 
    
    ```sql
    CREATE USER [your AAD group name] FROM EXTERNAL PROVIDER;
    ```

1. **Concedere al gruppo di Azure AD le autorizzazioni necessarie**, come si fa di norma per gli utenti SQ e altri utenti. Ad esempio, eseguire il codice seguente:

    ```sql
    EXEC sp_addrolemember [role name], [your AAD group name];
    ```

1. **Configurare un servizio collegato al database SQL di Azure** in Azure Data Factory.

**Esempio:**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Proprietà del set di dati

Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione dei set di dati, vedere l'articolo [Set di dati](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services). Questa sezione presenta un elenco delle proprietà supportate dal set di dati del database SQL di Azure.

Per copiare dati da o verso il database SQL di Azure, impostare la proprietà **type** del set di dati su **AzureSqlTable**. Sono supportate le proprietà seguenti:

| Proprietà | Descrizione | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà **type** del set di dati deve essere impostata su **AzureSqlTable**. | Sì |
| tableName | Il nome della tabella o della vista nell'istanza del database SQL di Azure a cui fa riferimento il servizio collegato. | No per l'origine, Sì per il sink |

#### <a name="dataset-properties-example"></a>Esempio di proprietà dei set di dati

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Proprietà dell'attività di copia

Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione delle attività, vedere l'articolo sulle [pipeline](concepts-pipelines-activities.md). Questa sezione presenta un elenco delle proprietà supportate dall'origine e dal sink del database SQL di Azure.

### <a name="azure-sql-database-as-the-source"></a>Database SQL di Azure come origine

Per copiare dati da un database SQL di Azure, impostare la proprietà **type** nell'origine dell'attività di copia su **SqlSource**. Nella sezione **source** dell'attività di copia sono supportate le proprietà seguenti:

| Proprietà | Descrizione | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà **type** dell'origine dell'attività di copia deve essere impostata su **SqlSource**. | Sì |
| SqlReaderQuery | Usare la query SQL personalizzata per leggere i dati. Esempio: `select * from MyTable`. | No  |
| sqlReaderStoredProcedureName | Nome della stored procedure che legge i dati dalla tabella di origine. L'ultima istruzione SQL deve essere un'istruzione SELECT nella stored procedure. | No  |
| storedProcedureParameters | Parametri per la stored procedure.<br/>I valori consentiti sono coppie nome-valore. I nomi e le maiuscole e minuscole dei parametri devono corrispondere ai nomi e alle maiuscole e minuscole dei parametri della stored procedure. | No  |

### <a name="points-to-note"></a>Punti da notare

- Se la proprietà **sqlReaderQuery** è specificata per **SqlSource**, l'attività di copia esegue questa query nell'origine del database SQL di Azure per ottenere i dati. In alternativa è possibile specificare una stored procedure. Indicare i parametri **sqlReaderStoredProcedureName** e **storedProcedureParameters** (se accettati dalla stored procedure).
- Se non si specifica né **sqlReaderQuery** né **sqlReaderStoredProcedureName**, le colonne definite nella sezione della **struttura** del set di dati JSON vengono usate per creare una query. `select column1, column2 from mytable` viene eseguito sul Database SQL di Azure. Se la definizione del set di dati non include la **struttura**, vengono selezionate tutte le colonne della tabella.

#### <a name="sql-query-example"></a>Esempio di query SQL

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="stored-procedure-example"></a>Esempio di stored procedure

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="stored-procedure-definition"></a>Definizione della stored procedure

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="azure-sql-database-as-the-sink"></a>Database SQL di Azure come sink

Per copiare i dati in un database SQL di Azure, impostare la proprietà **type** dell'attività di copia su **SqlSink**. Nella sezione **sink** dell'attività di copia sono supportate le proprietà seguenti:

| Proprietà | Descrizione | Obbligatoria |
|:--- |:--- |:--- |
| type | La proprietà **type** del sink dell'attività di copia deve essere impostata su **SqlSink**. | Sì |
| writeBatchSize | Numero di righe nella tabella SQL inserimenti **per ogni batch**.<br/> Il valore consentito è **integer** (numero di righe). |  No. Il valore predefinito è 10000. |
| writeBatchTimeout | Tempo di attesa per il completamento dell'operazione di inserimento batch prima del timeout.<br/> Il valore consentito è **timespan**. Esempio: "00:30:00" (30 minuti). | No  |
| preCopyScript | Specificare una query SQL per l'attività di copia da eseguire prima di scrivere i dati nel database SQL di Azure. Viene richiamata solo una volta per esecuzione della copia. Usare questa proprietà per pulire i dati precaricati. | No  |
| sqlWriterStoredProcedureName | Il nome della stored procedure che definisce come applicare i dati di origine in una tabella di destinazione. Un esempio è eseguire operazioni di upsert o trasformare usando la propria logica di business. <br/><br/>Questa stored procedure viene **richiamata per batch**. Per le operazioni che vengono eseguite una sola volta e non hanno nulla a che fare con dati di origine, usare la proprietà `preCopyScript`. Esempi di operazioni sono eliminazione e troncamento. | No  |
| storedProcedureParameters |Parametri per la stored procedure.<br/>I valori consentiti sono coppie nome-valore. I nomi e le maiuscole e minuscole dei parametri devono corrispondere ai nomi e alle maiuscole e minuscole dei parametri della stored procedure. | No  |
| sqlWriterTableType | Specificare il nome di un tipo di tabella da usare nella stored procedure. L'attività di copia fa in modo che i dati spostati siano disponibili in una tabella temporanea con questo tipo di tabella. Il codice della stored procedure può quindi unire i dati copiati con i dati esistenti. | No  |

> [!TIP]
> Quando si copiano dati in un database SQL di Azure, per impostazione predefinita l'attività di copia aggiunge i dati alla tabella di sink. Per eseguire un'operazione di upsert o una logica di business aggiuntiva, usare la stored procedure in **SqlSink**. Altre informazioni sono contenute in [Richiamo della stored procedure per SQL Sink](#invoking-stored-procedure-for-sql-sink).

#### <a name="append-data-example"></a>Esempio di dati Append

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

#### <a name="invoke-a-stored-procedure-during-copy-for-upsert-example"></a>Esempio di richiamo di una stored procedure durante la copia per l'operazione di upsert

Altre informazioni sono contenute in [Richiamo della stored procedure per SQL Sink](#invoking-stored-procedure-for-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "sqlWriterTableType": "CopyTestTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="identity-columns-in-the-target-database"></a>Colonne Identity nel database di destinazione

In questa sezione viene mostrato come copiare dati da una tabella di origine senza una colonna Identity a una tabella di destinazione con una colonna Identity.

#### <a name="source-table"></a>Tabella di origine

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```

#### <a name="destination-table"></a>Tabella di destinazione

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

> [!NOTE]
> La tabella di destinazione contiene una colonna Identity.

#### <a name="source-dataset-json-definition"></a>Definizione JSON del set di dati di origine

```json
{
    "name": "SampleSource",
    "properties": {
        "type": " AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTbl"
        }
    }
}
```

#### <a name="destination-dataset-json-definition"></a>Definizione JSON del set di dati di destinazione

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "TargetTbl"
        }
    }
}
```

> [!NOTE]
> La tabella di origine e quella di destinazione hanno schemi diversi. 

La tabella di destinazione dispone di una colonna aggiuntiva con un'identità. In questo scenario è necessario specificare la proprietà **structure** nella definizione del set di dati di destinazione, che non include la colonna Identity.

## <a name="invoking-stored-procedure-for-sql-sink"></a> Chiamare una stored procedure da un sink SQL

Quando si copiano dati in un database SQL di Azure, è anche possibile configurare e richiamare una stored procedure specificata dall'utente con parametri aggiuntivi.

È possibile usare una stored procedure quando non si possono usare i meccanismi di copia predefiniti. Generalmente vengono usati quando è necessario eseguire un'operazione di upsert, cioè inserimento e aggiornamento, o un'elaborazione extra prima dell'inserimento finale dei dati di origine nella tabella di destinazione. Alcuni esempi di elaborazione extra sono l'unione di colonne, la ricerca di altri valori e l'inserimento in più di una tabella.

L'esempio seguente illustra come usare una stored procedure per eseguire un'operazione di upsert in una tabella del database SQL di Azure. Si presuppone che i dati di input e la tabella **Marketing** del sink abbiano tre colonne: **ProfileID**, **State** e **Category**. Eseguire l'operazione di upsert nella colonna **ProfileID** e applicarla solo a una categoria specifica.

**Set di dati di output:** "tableName" deve essere lo stesso nome parametro di tipo tabella della stored procedure (vedere di seguito script di stored procedure).

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

Definire le **sink SQL** sezione nell'attività di copia come indicato di seguito.

```json
"sink": {
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing",
    "storedProcedureParameters": {
        "category": {
            "value": "ProductA"
        }
    }
}
```

Nel database definire la stored procedure con lo stesso nome di **SqlWriterStoredProcedureName**, che gestisce i dati di input dell'origine specificata e li unisce nella tabella di output. Il nome del parametro del tipo di tabella nella stored procedure deve essere identico al valore **tableName** definito nel set di dati.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
AS
BEGIN
  MERGE [dbo].[Marketing] AS target
  USING @Marketing AS source
  ON (target.ProfileID = source.ProfileID and target.Category = @category)
  WHEN MATCHED THEN
      UPDATE SET State = source.State
  WHEN NOT MATCHED THEN
      INSERT (ProfileID, State, Category)
      VALUES (source.ProfileID, source.State, source.Category);
END
```

Nel database definire il tipo di tabella con lo stesso nome di **sqlWriterTableType**. Lo schema del tipo di tabella deve essere identico allo schema restituito dai dati di input.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL,
    [Category] [varchar](256) NOT NULL
)
```

La funzionalità di stored procedure sfrutta i [parametri valutati a livello di tabella](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database"></a>Mapping dei tipi di dati per il database SQL di Azure

Quando si copiano i dati da o verso il database SQL di Azure, vengono usati i mapping seguenti dai tipi di dati del database SQL di Azure ai tipi di dati provvisori di Azure Data Factory. Vedere [Mapping dello schema e del tipo di dati](copy-activity-schema-and-type-mapping.md) per informazioni su come l'attività di copia esegue il mapping dello schema di origine e del tipo di dati al sink.

| Tipi di dati del database SQL di Azure | Tipo di dati provvisorio di Data Factory |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| bit |Boolean |
| char |String, Char[] |
| date |DateTime |
| DateTime |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| Decimal |Decimal |
| FILESTREAM attribute (varbinary(max)) |Byte[] |
| Float |Double |
| image |Byte[] |
| int |Int32 |
| money |Decimal |
| nchar |String, Char[] |
| ntext |String, Char[] |
| numeric |Decimal |
| nvarchar |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |DateTime |
| smallint |Int16 |
| smallmoney |Decimal |
| sql_variant |Oggetto |
| text |String, Char[] |
| time |TimeSpan |
|  timestamp |Byte[] |
| tinyint |Byte |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| xml |xml |

>[!NOTE]
> Per i tipi di dati associati al tipo provvisorio Decimal, Azure Data Factory supporta attualmente la precisione fino a 28. Se si hanno dati che richiedono una precisione maggiore di 28, è consigliabile convertirli in una stringa in una query SQL.

## <a name="next-steps"></a>Passaggi successivi
Per un elenco degli archivi dati supportati come origini o sink dall'attività di copia in Azure Data Factory, vedere [Archivi dati e formati supportati](copy-activity-overview.md##supported-data-stores-and-formats).

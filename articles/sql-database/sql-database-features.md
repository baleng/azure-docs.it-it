---
title: Confronto tra le funzionalità del database SQL di Azure | Microsoft Docs
description: Questo articolo mette a confronto le funzionalità di SQL Server disponibili nei diversi tipi del database SQL di Azure.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: bonova, sstein
manager: craigg
ms.date: 02/08/2019
ms.openlocfilehash: e1c15b78b93c638c8941356319de2c5e17712795
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358262"
---
# <a name="feature-comparison-azure-sql-database-versus-sql-server"></a>Confronto delle funzionalità: Database SQL di Azure e SQL Server

Il database SQL di Azure ha una base di codice in comune con SQL Server. Le funzionalità di SQL Server supportate dal database SQL di Azure dipendono dal tipo di database creato. Con il database SQL di Azure è possibile creare un database come parte di un'[istanza gestita](sql-database-managed-instance.md), come un database singolo o come parte di un pool elastico.

Microsoft introduce costantemente nuove funzionalità per il database SQL di Azure. Visitare la pagina Web Aggiornamenti di Azure per ottenere informazioni sugli aggiornamenti più recenti usando questi filtri:

- Filtrato per [servizio Database SQL](https://azure.microsoft.com/updates/?service=sql-database).
- Filtrato per Disponibilità generale [annunci](https://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) per le funzionalità del database SQL.

## <a name="sql-server-feature-support-in-azure-sql-database"></a>Supporto delle funzionalità di SQL Server nel database SQL di Azure

La tabella seguente elenca le principali funzionalità di SQL Server. Per ogni funzionalità viene specificato se questa è parzialmente o completamente supportata e viene riportato un collegamento ad altre informazioni.

| **Funzionalità di SQL** | **Supportate per i database singoli e pool elastici** | **È supportata dalle istanze gestite** |
| --- | --- | --- |
| [Replica geografica attiva](sql-database-active-geo-replication.md) | Sì - tutti i livelli diversi da con iperscalabilità di servizio | No, vedere i [Gruppi di failover automatico](sql-database-auto-failover-group.md) |
| [Gruppi di failover automatico](sql-database-auto-failover-group.md) | Sì - tutti i livelli diversi da con iperscalabilità di servizio | Sì, in [anteprima pubblica](sql-database-auto-failover-group.md)|
| [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Sì. Vedere [Archivio certificati](sql-database-always-encrypted.md) e [Insieme di credenziali delle chiavi](sql-database-always-encrypted-azure-key-vault.md) | Sì. Vedere [Archivio certificati](sql-database-always-encrypted.md) e [Insieme di credenziali delle chiavi](sql-database-always-encrypted-azure-key-vault.md) |
| [Gruppi di disponibilità AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | La [disponibilità elevata](sql-database-high-availability.md) è inclusa in ogni database. Il ripristino di emergenza è trattato in [Panoramica della continuità aziendale del database SQL di Azure](sql-database-business-continuity.md) | La [disponibilità elevata](sql-database-high-availability.md) è inclusa in ogni database. Il ripristino di emergenza è trattato in [Panoramica della continuità aziendale del database SQL di Azure](sql-database-business-continuity.md) |
| [Collegamento di un database](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | No  | No  |
| [Ruoli applicazione](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Sì | Sì |
|[Controllo](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Sì](sql-database-auditing.md)| [Sì](sql-database-managed-instance-auditing.md) |
| [Backup automatici](sql-database-automated-backups.md) | Sì | Sì |
| [(Uso forzato del piano) di ottimizzazione automatica](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Sì](sql-database-automatic-tuning.md)| [Sì](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) |
| [Ottimizzazione automatica (indici)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Sì](sql-database-automatic-tuning.md)| No  |
| [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) | Sì | Sì |
| [File BACPAC (esportazione)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Sì. Vedere [Esportazione di un database SQL](sql-database-export.md) | Sì |
| [File BACPAC (importazione)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Sì. Vedere [Importazione di un database SQL](sql-database-import.md) | Sì |
| [Comando di BACKUP](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | No, solo backup automatici avviati dal sistema, vedere [Backup automatici](sql-database-automated-backups.md) | Backup automatici avviati dal sistema e backup di sola copia avviati dall'utente, vedere le [differenze relative al backup](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Funzioni predefinite](https://docs.microsoft.com/sql/t-sql/functions/functions) | Supportate per la maggior parte. Vedere le singole funzioni | Sì, vedere le [differenze relative a stored procedure, funzioni e trigger](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Change Data Capture](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | No  | Sì |
| [Rilevamento modifiche](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Sì |Sì |
| [Regole di confronto - database](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation) | Sì | Sì |
| [Regole di confronto - server / dell'istanza](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation) | No  | Sì, in [anteprima pubblica](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md)|
| [Indici Columnstore](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Sì, [livello Premium, livello Standard; S3 e versioni successive, livello Utilizzo generico e livelli Business Critical](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |Sì |
| [Common Language Runtime (CLR)](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | No  | Sì, vedere le [differenze relative a CLR](sql-database-managed-instance-transact-sql-information.md#clr) |
| [Database indipendenti](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Sì | No [a causa di un difetto di ripristino compreso il ripristino temporizzato in](sql-database-managed-instance-transact-sql-information.md#cannot-restore-contained-database) |
| [Utenti indipendenti](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Sì | Sì |
| [Controllo delle parole chiave del linguaggio di flusso](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Sì | Sì |
| [Query tra database](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | No, vedere [Query elastiche](sql-database-elastic-query-overview.md) | Sì, oltre a [Query elastiche](sql-database-elastic-query-overview.md) |
| [Transazioni tra database](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | No  | Sì, all'interno dell'istanza. Visualizzare [collegata differenze tra server](sql-database-managed-instance-transact-sql-information.md#linked-servers) per le query tra istanze. |
| [Cursori](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Sì |Sì |
| [Compressione dei dati](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Sì |Sì |
| [Posta elettronica database](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | No  | Sì |
| [Servizio migrazione del database (DMS)](https://docs.microsoft.com/sql/dma/dma-overview) | Sì | Sì |
| [Mirroring del database](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | No  | No  |
| [Impostazioni di configurazione del database](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Sì | Sì |
| [Data Quality Services (DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | No  | No  |
| [Snapshot del database](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | No  | No  |
| [Tipi di dati](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Sì |Sì |
| [Istruzioni DBCC](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | Supportate per la maggior parte. Vedere le singole istruzioni | Sì, vedere le [differenze relative a DBCC](sql-database-managed-instance-transact-sql-information.md#dbcc) |
| [Istruzioni DDL](https://docs.microsoft.com/sql/t-sql/statements/statements) | Supportate per la maggior parte. Vedere le singole istruzioni | Sì, vedere le [differenze relative a T-SQL](sql-database-managed-instance-transact-sql-information.md) |
| [Trigger DDL](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Solo database |  Sì |
| [Viste partizionate distribuite](https://docs.microsoft.com/sql/t-sql/statements/create-view-transact-sql#partitioned-views) | No  | Sì |
| [Transazioni distribuite - MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | No. Vedere [Transazioni elastiche](sql-database-elastic-transactions-overview.md) |  No. vedere [collegata differenze tra server](sql-database-managed-instance-transact-sql-information.md#linked-servers) |
| [Istruzioni DML](https://docs.microsoft.com/sql/t-sql/queries/queries) | Sì | Sì |
| [Trigger DML](https://docs.microsoft.com/sql/relational-databases/triggers/create-dml-triggers) | Supportate per la maggior parte. Vedere le singole istruzioni |  Sì |
| [Viste a gestione dinamica](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Supportate nella maggior parte dei casi, vedere singole viste a gestione dinamica |  Sì, vedere le [differenze relative a T-SQL](sql-database-managed-instance-transact-sql-information.md) |
|[Maschera dati dinamica](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)|[Sì](sql-database-dynamic-data-masking-get-started.md)| [Sì](sql-database-dynamic-data-masking-get-started.md) |
| [Pool elastici](sql-database-elastic-pool.md) | Sì | Predefinito. Una singola istanza gestita può avere più database che condividono lo stesso pool di risorse |
| [Notifiche degli eventi](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | No. Vedere [Avvisi](sql-database-insights-alerts-portal.md) | No  |
| [Espressioni](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Sì | Sì |
| [Eventi estesi](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Supportati in alcuni casi. Vedere [Eventi estesi nel database SQL](sql-database-xevent-db-diff-from-svr.md) | Sì - vedere le [differenze relative agli eventi estesi](sql-database-managed-instance-transact-sql-information.md#extended-events) |
| [Stored procedure estese](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | No  | No  |
[File e gruppi di file](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Solo gruppi di file primari | Sì |
| [Filestream](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | No  | No  |
| [Ricerca full-text](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |  I word breaker di terze parti non sono supportati |I word breaker di terze parti non sono supportati |
| [Funzioni](https://docs.microsoft.com/sql/t-sql/functions/functions) | Supportate per la maggior parte. Vedere le singole funzioni | Sì, vedere le [differenze relative a stored procedure, funzioni e trigger](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Ripristino geografico](sql-database-recovery-using-backups.md#geo-restore) | Sì - tutti i livelli diversi da con iperscalabilità di servizio | No, è possibile ripristinare i backup completi COPY_ONLY eseguiti periodicamente, vedere le [differenze relative al backup](sql-database-managed-instance-transact-sql-information.md#backup) e le [differenze relative al ripristino](sql-database-managed-instance-transact-sql-information.md#restore-statement). |
| [Elaborazione di grafici](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview) | Sì | Sì |
| [Ottimizzazione in memoria](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Sì, [solo livelli Premium e Business critical](sql-database-in-memory.md) | Sì - [solo livello Business Critical](sql-database-managed-instance.md) |
| [Supporto di dati JSON](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | [Sì](sql-database-json-features.md) | [Sì](sql-database-json-features.md) |
| [Elementi del linguaggio](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | Supportati per la maggior parte. Vedere i singoli elementi |  Sì, vedere le [differenze relative a T-SQL](sql-database-managed-instance-transact-sql-information.md) |
| [Server collegati](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | No. Vedere [Query elastica](sql-database-elastic-query-horizontal-partitioning.md) | Solo per SQL Server e il database SQL |
| [Log shipping](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | La [disponibilità elevata](sql-database-high-availability.md) è inclusa in ogni database. Il ripristino di emergenza è trattato in [Panoramica della continuità aziendale del database SQL di Azure](sql-database-business-continuity.md) |La [disponibilità elevata](sql-database-high-availability.md) è inclusa in ogni database. Il ripristino di emergenza è trattato in [Panoramica della continuità aziendale del database SQL di Azure](sql-database-business-continuity.md) |
| [Master Data Services (MDS)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | No  | No  |
| [Registrazione minima nell'importazione bulk](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | No  | No  |
| [Modifica dei dati di sistema](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | No  | Sì |
| [Automazione OLE](https://docs.microsoft.com/sql/database-engine/configure-windows/ole-automation-procedures-server-configuration-option) | No  | No  |
| [Operazioni online sugli indici](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Sì | Sì |
| [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql)|No |Sì, vedere le [differenze relative a T-SQL](sql-database-managed-instance-transact-sql-information.md)|
| [OPENJSON](https://docs.microsoft.com/sql/t-sql/functions/openjson-transact-sql)|Sì|Sì|
| [FUNZIONE OPENQUERY](https://docs.microsoft.com/sql/t-sql/functions/openquery-transact-sql)|No |Sì, vedere le [differenze relative a T-SQL](sql-database-managed-instance-transact-sql-information.md)|
| [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql)|No |Sì, vedere le [differenze relative a T-SQL](sql-database-managed-instance-transact-sql-information.md)|
| [OPENXML](https://docs.microsoft.com/sql/t-sql/functions/openxml-transact-sql)|Sì|Sì|
| [Operatori](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | Supportati per la maggior parte. Vedere i singoli operatori |Sì, vedere le [differenze relative a T-SQL](sql-database-managed-instance-transact-sql-information.md) |
| [Partizionamento](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Sì | Sì |
| [Ripristino temporizzato di un database](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Sì, tutti i piani di servizio diversi da quello con iperscalabilità - vedere [ripristino del Database SQL](sql-database-recovery-using-backups.md#point-in-time-restore) | Sì. Vedere [Ripristino di un database SQL](sql-database-recovery-using-backups.md#point-in-time-restore) |
| [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | No  | No  |
| [Gestione basata su criteri](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | No  | No  |
| [Predicati](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Sì | Sì |
| [Notifiche delle query](https://docs.microsoft.com/sql/relational-databases/native-client/features/working-with-query-notifications) | No  | Sì |
| [Analisi delle prestazioni della query](sql-database-query-performance.md) | Sì | No  |
| [Servizi R](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Sì, in [anteprima pubblica](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services)  | No  |
| [Resource governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | No  | Sì |
| [Istruzioni RESTORE](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | No  | Sì, vedere le [differenze relative al ripristino](sql-database-managed-instance-transact-sql-information.md#restore-statement) |
| [Ripristino del database da backup](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | Solo da backup automatici, vedere [Ripristino di un database SQL](sql-database-recovery-using-backups.md) | Da backup automatici, vedere [Ripristino di un database SQL](sql-database-recovery-using-backups.md), e da backup completi, vedere le [differenze relative al backup](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Sicurezza a livello di riga](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Sì | Sì |
| [Ricerca semantica](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | No  | No  |
| [Numeri di sequenza](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Sì | Sì |
| [Broker di servizio](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | No  | Sì, vedere le [differenze relative a Service Broker](sql-database-managed-instance-transact-sql-information.md#service-broker) |
| [Impostazioni di configurazione del server](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | No  | Sì, vedere le [differenze relative a T-SQL](sql-database-managed-instance-transact-sql-information.md) |
| [Istruzioni SET](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | Supportate per la maggior parte. Vedere le singole istruzioni | Sì, vedere le [differenze relative a T-SQL](sql-database-managed-instance-transact-sql-information.md)|
| [SMO](https://docs.microsoft.com/sql/relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide) | Sì | Sì |
| [Spatial](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Sì | Sì |
| [Analitica SQL](https://docs.microsoft.com/azure/azure-monitor/insights/azure-sql) | Sì | Sì |
| [Sincronizzazione dati SQL](sql-database-get-started-sql-data-sync.md) | Sì | No  |
| [Agente SQL Server](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | No. Vedere [Processi elastici](sql-database-elastic-jobs-getting-started.md) | Sì, vedere le [differenze relative a SQL Server Agent](sql-database-managed-instance-transact-sql-information.md#sql-server-agent) |
| [SQL Server Analysis Services (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | No, vedere [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) | No. Vedere [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| [Controllo di SQL Server](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | No. Vedere [Controllo del database SQL](sql-database-auditing.md) | Sì, vedere le [differenze relative al controllo](sql-database-managed-instance-transact-sql-information.md#auditing) |
| [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) | Sì | Sì |
| [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Sì, con SSIS gestito nell'ambiente di Azure Data Factory in cui i pacchetti vengono archiviati nel database SSISDB ospitato dal database SQL di Azure ed eseguiti nel runtime di integrazione SSIS di Azure vedere [Creare il runtime di integrazione SSIS di Azure in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>Per confrontare le funzioni SSIS nel server del database SQL e in Istanza gestita, vedere [Compare Azure SQL Database single databases/elastic pools and Managed Instance](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance) (Confrontare i singoli database SQL di Azure/pool elastici e Istanza gestita). | Sì, con SSIS gestito nell'ambiente di Azure Data Factory in cui i pacchetti vengono archiviati nel database SSISDB ospitato da Istanza gestita ed eseguiti nel runtime di integrazione SSIS di Azure vedere [Creare il runtime di integrazione SSIS di Azure in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). <br/><br/>Per confrontare le funzioni SSIS nel database SQL e in Istanza gestita, vedere [Compare Azure SQL Database single databases/elastic pools and Managed Instance](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance) (Confrontare i singoli database SQL di Azure/pool elastici e Istanza gestita). |
| [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) | Sì | Sì |
| [SQL Server PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Sì | Sì |
| [SQL Server Profiler](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | No. Vedere [Eventi estesi](sql-database-xevent-db-diff-from-svr.md) | Sì |
| [Replica SQL Server](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Solo per iscritti alla replica transazionale e snapshot](sql-database-single-database-migrate.md) | Sì, in [anteprima pubblica](https://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance) |
| [SQL Server Reporting Services (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | No, vedere [Power BI](https://docs.microsoft.com/power-bi/) | No, vedere [Power BI](https://docs.microsoft.com/power-bi/) |
| [Stored procedure](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Sì | Sì |
| [Funzioni archiviate dal sistema](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Supportate per la maggior parte. Vedere le singole funzioni | Sì, vedere le [differenze relative a stored procedure, funzioni e trigger](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Stored procedure di sistema](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Supportate in alcuni casi. Vedere le singole stored procedure | Sì, vedere le [differenze relative a stored procedure, funzioni e trigger](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Tabelle di sistema](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Supportate in alcuni casi. Vedere le singole tabelle | Sì, vedere le [differenze relative a T-SQL](sql-database-managed-instance-transact-sql-information.md) |
| [Viste del catalogo di sistema](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Supportate in alcuni casi. Vedere le singole viste | Sì, vedere le [differenze relative a T-SQL](sql-database-managed-instance-transact-sql-information.md) |
| [Tabelle temporanee](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#database-scoped-global-temporary-tables-azure-sql-database) | Tabelle temporanee locali e globali in ambito database | Tabelle temporanee locali e globali in ambito istanza |
| [Tabelle temporali](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | [Sì](sql-database-temporal-tables.md) | [Sì](sql-database-temporal-tables.md) |
|Introduzione al rilevamento delle minacce|  [Sì](sql-database-threat-detection.md)|[Sì](sql-database-managed-instance-threat-detection.md)|
| [Flag di traccia](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql) | No  | No  |
| [variables](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Sì | Sì |
| [Transparent data encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Sì, solo livelli di servizio Utilizzo generico e Business Critical| [Sì](transparent-data-encryption-azure-sql.md) |
[VNet](../virtual-network/virtual-networks-overview.md) | Parziale, vedere [Endpoint della rete virtuale](sql-database-vnet-service-endpoint-rule-overview.md) | Sì, solo modello Resource Manager |
| [Windows Server Failover Clustering](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | La [disponibilità elevata](sql-database-high-availability.md) è inclusa in ogni database. Il ripristino di emergenza è trattato in [Panoramica della continuità aziendale del database SQL di Azure](sql-database-business-continuity.md) | La [disponibilità elevata](sql-database-high-availability.md) è inclusa in ogni database. Il ripristino di emergenza è trattato in [Panoramica della continuità aziendale del database SQL di Azure](sql-database-business-continuity.md) |
| [Indici XML](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Sì | Sì |

## <a name="next-steps"></a>Passaggi successivi

- Per informazioni sul servizio di Database SQL di Azure, vedere [Informazioni sul database SQL](sql-database-technical-overview.md)
- Per altre informazioni in proposito, vedere l'articolo di [informazioni su Istanza gestita](sql-database-managed-instance.md).

---
title: Ripristinare un database SQL di Azure da un backup | Microsoft Docs
description: Informazioni sul ripristino temporizzato, che consente di ripristinare un database SQL di Azure a un momento precedente (fino a 35 giorni).
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: ca54ae11390b388c3158bd220ee5c7829172a5c3
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/29/2019
ms.locfileid: "58620479"
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Ripristinare un database SQL di Azure mediante i backup automatici del database

Per impostazione predefinita, i backup del database SQL vengono archiviati in Archiviazione BLOB con replica geografica (RA-GRS). Per il ripristino di database tramite [backup automatici](sql-database-automated-backups.md) sono disponibili le opzioni seguenti:

- Creare un nuovo database nello stesso server di database SQL ripristinato a un determinato momento nel tempo entro il periodo di conservazione.
- Creare un database nello stesso server di Database SQL ripristinato in base all'ora di eliminazione per un database eliminato.
- Creare un nuovo database su qualsiasi server di database SQL nella stessa area ripristinato in base ai backup più recenti.
- Creare un nuovo database su qualsiasi server di database SQL in qualsiasi altra area ripristinato in base ai backup replicati più recenti.

Se è stata configurata la [conservazione a lungo termine dei backup](sql-database-long-term-retention.md), è anche possibile creare un nuovo database da qualsiasi backup di conservazione a lungo termine su qualsiasi server di database SQL in qualunque area.

> [!IMPORTANT]
> Non è possibile sovrascrivere un database esistente durante il ripristino.

Quando si usa un livello di servizio Standard o Premium, un database ripristinato comporta un costo di archiviazione aggiuntivo nei casi seguenti:

- Ripristino di P11 – P15 in S4 S12 o P1 – P6 se le dimensioni massime del database sono maggiori di 500 GB.
- Ripristino di P1–P6 in S4-S12 se le dimensioni massime del database sono maggiori di 250 GB.

Viene addebitato un costo aggiuntivo perché la dimensione massima del database ripristinato è superiore alla dimensione di archiviazione inclusa per la dimensione di calcolo; vengono applicati costi aggiuntivi per l'archiviazione aggiuntiva di cui viene eseguito il provisioning che supera la dimensione inclusa. Per i dettagli dei prezzi delle risorse di archiviazione aggiuntive, vedere la [pagina dei prezzi del database SQL](https://azure.microsoft.com/pricing/details/sql-database/). Se la quantità effettiva di spazio usato è inferiore alla quantità inclusa di risorse di archiviazione, questo costo aggiuntivo può essere evitato riducendo le dimensioni massime del database fino alla quantità inclusa.

> [!NOTE]
> [I backup di database automatici](sql-database-automated-backups.md) vengono usati quando si crea un [copia del database](sql-database-copy.md).

## <a name="recovery-time"></a>Tempo di ripristino

Il tempo di ripristino di un database tramite i backup automatici del database dipende da numerosi fattori:

- Le dimensioni del database
- Le dimensioni di calcolo del database
- Il numero dei log delle transazioni interessate
- La quantità di attività che deve essere ripetuta per il ripristino al punto di ripristino
- La larghezza di banda di rete se il ripristino avviene in un'area diversa
- Il numero di richieste simultanee di ripristino in corso di elaborazione nell'area di destinazione

Per un database molto grande e/o attivo il ripristino potrebbe richiedere diverse ore. Nel caso di un'interruzione prolungata in un'area, è possibile che in altre aree vengano elaborate molte richieste di ripristino geografico. In presenza di numerose richieste, i tempi di recupero dei database in tale area possono aumentare. Per la maggior parte dei database, il ripristino richiede meno di 12 ore.

Per una singola sottoscrizione, ci sono alcune limitazioni sul numero di richieste simultanee di ripristino (compresi ripristino temporizzato, ripristino geografico e ripristino da un backup con conservazione a lungo termine) che possono essere inviate ed eseguite:

| | **Numero massimo di richieste simultanee elaborate** | **Numero massimo di richieste simultanee inviate** |
| :--- | --: | --: |
|Database singolo (per sottoscrizione)|10|60|
|Pool elastico (per pool)|4|200|
||||

Non è disponibile una funzionalità incorporata per il ripristino in blocco. Lo script [Database SQL di Azure: Full Server Recovery](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) è un esempio di un modo per eseguire questa operazione.

> [!IMPORTANT]
> Per eseguire il ripristino tramite i backup automatici, è necessario essere un membro del ruolo di collaboratore di SQL Server o proprietario della sottoscrizione - vedere [RBAC: ruoli predefiniti](../role-based-access-control/built-in-roles.md). Per il ripristino, è possibile usare il portale di Azure, PowerShell o l'API REST. Non è possibile usare Transact-SQL.

## <a name="point-in-time-restore"></a>Ripristino temporizzato

È possibile ripristinare un database autonomo, in pool o di istanza a un punto specifico nel tempo come un nuovo database nello stesso server tramite il portale di Azure, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) o l'[API REST](https://docs.microsoft.com/rest/api/sql/databases). Un database può essere ripristinato a qualsiasi livello di servizio o dimensione di calcolo. Assicurarsi di disporre di risorse sufficienti sul server in cui si sta ripristinando il database. Al termine, il database ripristinato è un normale database online completamente accessibile alle tariffe normali in base al livello di prestazioni e alla dimensione di calcolo. Non è previsto alcun addebito fino al completamento del ripristino del database.

In genere si ripristina un database a un punto precedente per scopi di ripristino. Quando si esegue questa operazione, è possibile considerare il database ripristinato come una sostituzione per il database originale o usarlo per recuperare i dati e quindi aggiornare il database originale.

- **Sostituzione del database**

  Se il database ripristinato è inteso come database sostitutivo dell'originale, è necessario verificare che la dimensione di calcolo e/o il livello di servizio sia appropriato e ridimensionare il database, se necessario. È possibile rinominare il database originale e quindi assegnare al database ripristinato il nome originale usando il comando [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) in T-SQL.

- **Ripristino dei dati**

  Se si prevede di recuperare dati dal database ripristinato in caso di errore dell'applicazione o dell'utente, è necessario scrivere ed eseguire gli opportuni script di ripristino per estrarre i dati dal database ripristinato e inserirli nel database originale. Anche se il completamento dell'operazione di ripristino può richiedere molto tempo, il database in fase di ripristino è visibile nell'elenco dei database per tutto il processo. Se si elimina il database durante il ripristino, l'operazione verrà annullata e non verrà addebitata per il database il cui ripristino non è stato completato.

Per eseguire il ripristino di un database singolo, in pool o di istanza a un determinato momento nel tempo tramite il portale di Azure, aprire la pagina per il database e fare clic su **Ripristina** nella barra degli strumenti.

![ripristino a un determinato momento nel tempo](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

> [!IMPORTANT]
> Per ripristinare a livello di codice un database da un backup, vedere [Esecuzione a livello di codice del ripristino tramite backup automatici](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="deleted-database-restore"></a>Ripristino di un database eliminato

Il ripristino di un database eliminato consente di ripristinare un database all'ora di eliminazione per un database eliminato sullo stesso server di database SQL, usando il portale di Azure, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) o [REST (createMode=Restore)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate). È possibile [ripristinare un database eliminato in istanza gestita tramite PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../recreate-dropped-database-on-azure-sql-managed-instance). È possibile ripristinare un database eliminato a un punto precedente durante la conservazione tramite [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase).

> [!TIP]
> Per uno script di PowerShell di esempio che illustra come ripristinare un database eliminato, vedere [Ripristinare un database SQL tramite PowerShell](scripts/sql-database-restore-database-powershell.md).
> [!IMPORTANT]
> Se si elimina un'istanza del server di database SQL di Azure, anche tutti i database relativi vengono eliminati e non possono essere recuperati. Non è attualmente disponibile alcun supporto per il ripristino di un server eliminato.

### <a name="deleted-database-restore-using-the-azure-portal"></a>Ripristino di un database eliminato con il portale di Azure

Per ripristinare un database eliminato con il portale di Azure durante il [periodo di conservazione basato su DTU](sql-database-service-tiers-dtu.md) o il [periodo di conservazione basato su vCore](sql-database-service-tiers-vcore.md) tramite il portale di Azure, aprire la pagina per il server e nell'area Operazioni fare clic su **Database eliminati**.

![deleted-database-restore-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)

![deleted-database-restore-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

> [!IMPORTANT]
> Per ripristinare a livello di codice un database eliminato, vedere [Esecuzione a livello di codice del ripristino tramite backup automatici](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="geo-restore"></a>Ripristino geografico

Il ripristino geografico consente di ripristinare un database SQL presente su un server in qualsiasi area di Azure usando gli ultimi backup con replica geografica. Il ripristino geografico usa un backup con ridondanza geografica come origine e può essere usato per ripristinare un database anche se il database o il data center è inaccessibile a causa di un'interruzione del servizio.

Il ripristino geografico è l'opzione di ripristino predefinita quando il database non è disponibile a causa di un evento imprevisto nell'area in cui è ospitato. Se si verifica un evento imprevisto su larga scala che determina la mancata disponibilità dell'applicazione di database, è possibile ripristinare un database dai backup con replica geografica in un server in un'altra area. Esiste un ritardo tra il momento in cui un backup viene creato e quando ne viene eseguita la replica geografica in un BLOB di Azure in un'area diversa. Questo ritardo può essere al massimo di un'ora, quindi, in caso di emergenza, può verificarsi una perdita massima di un'ora di dati. La figura seguente mostra il ripristino del database dall'ultimo backup disponibile in un'altra area.

![Ripristino geografico](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Per uno script di PowerShell di esempio che illustra come eseguire un ripristino geografico, vedere [Ripristinare un database SQL tramite PowerShell](scripts/sql-database-restore-database-powershell.md).

Il ripristino temporizzato in un database di replica geografica secondaria non è attualmente supportato. Il ripristino temporizzato può essere eseguito solo in un database primario. Per informazioni dettagliate sull'uso del ripristino geografico in caso di un'interruzione del servizio, vedere [Ripristinare un database SQL di Azure o eseguire il failover in un database secondario](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Il ripristino da backup è la più semplice delle soluzioni di ripristino di emergenza disponibili nel database SQL con il massimo valore per l'obiettivo del punto di ripristino e il tempo di recupero stimato. Per le soluzioni che usano database di piccole dimensioni (ad esempio, livello di servizio Basic o database tenant di piccole dimensioni in pool elastici), il ripristino geografico è spesso una soluzione di ripristino di emergenza ragionevole con un tempo di recupero stimato fino a 12 ore (in genere molto di meno). Per le soluzioni che usano database di grandi dimensioni e richiedono tempi di ripristino inferiori, è consigliabile usare la [replica geografica attiva](sql-database-active-geo-replication.md) o i [gruppi di failover automatico](sql-database-auto-failover-group.md). La replica geografica attiva offre un obiettivo del punto di ripristino e un tempo di recupero stimato decisamente inferiori perché richiede solo l'avvio di un failover in un database secondario con replica continua. I gruppi di failover automatico abilitano il failover automatico per un gruppo di database. Per altre informazioni sulle opzioni di continuità aziendale, vedere [Panoramica sulla continuità aziendale](sql-database-business-continuity.md).

### <a name="geo-restore-using-the-azure-portal"></a>Eseguire il ripristino geografico tramite il portale di Azure

Per geo-ripristinare un database eliminato durante il [periodo di conservazione basato su DTU](sql-database-service-tiers-dtu.md) o il [periodo di conservazione basato su vCore](sql-database-service-tiers-vcore.md) tramite il portale di Azure, aprire la pagina dei database SQL e fare clic su **Aggiungi**. Nel casella di testo **Seleziona origine** selezionare **Backup**. Specificare il backup da cui eseguire il ripristino nell'area e nel server di propria scelta.

> [!Note]
> Ripristino geografico nel portale di Azure non è disponibile nell'istanza gestita. Usare PowerShell.

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Esecuzione a livello di codice del ripristino tramite backup automatici

Come indicato in precedenza, oltre al portale di Azure, il ripristino di database può essere eseguito a livello di codice usando Azure PowerShell o l'API REST. Le tabelle seguenti descrivono il set di comandi disponibili.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Il modulo Azure PowerShell per Resource Manager è ancora supportato dal Database SQL di Azure, ma i progetti di sviluppo future è per il modulo Az.Sql. Per questi cmdlet, vedere [azurerm. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Gli argomenti per i comandi nel modulo Az e nei moduli AzureRm sono sostanzialmente identici.

- Per ripristinare un database in pool o autonomo, vedere [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase).

  | Cmdlet | DESCRIZIONE |
  | --- | --- |
  | [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) |Ottiene uno o più database. |
  | [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup) | Ottiene un database eliminato che è possibile ripristinare. |
  | [Get-AzSqlDatabaseGeoBackup](/powershell/module/az.sql/get-azsqldatabasegeobackup) |Ottiene una copia di backup con ridondanza geografica di un database. |
  | [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) |Ripristina un database SQL. |

  > [!TIP]
  > Per uno script di PowerShell di esempio che illustra come eseguire un ripristino temporizzato di un database, vedere [Ripristinare un database SQL tramite PowerShell](scripts/sql-database-restore-database-powershell.md).

- Per ripristinare un database di istanza gestita, vedere [Restore-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase).

  | Cmdlet | DESCRIZIONE |
  | --- | --- |
  | [Get-AzSqlInstance](/powershell/module/az.sql/get-azsqlinstance) |Ottiene uno o più istanze gestite. |
  | [Get-AzSqlInstanceDatabase](/powershell/module/az.sql/get-azsqlinstancedatabase) | Ottiene un'istanza di database. |
  | [Restore-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase) |Ripristina un database di istanza. |

### <a name="rest-api"></a>API REST

Per ripristinare un database singolo o in pool con l'API REST:

| API | DESCRIZIONE |
| --- | --- |
| [REST (createMode=Recovery)](https://docs.microsoft.com/rest/api/sql/databases) |Ripristina un database |
| [Get Create or Update Database Status](https://docs.microsoft.com/rest/api/sql/operations) |Restituisce lo stato durante un'operazione di ripristino |

### <a name="azure-cli"></a>Interfaccia della riga di comando di Azure

- Per ripristinare un database singolo o in pool con l'interfaccia della riga di comando di Azure, vedere [Ripristino az sql db](/cli/azure/sql/db#az-sql-db-restore).
- Per ripristinare un'istanza gestita con CLI di Azure, vedere [ripristino midb di az sql](/cli/azure/sql/midb#az-sql-midb-restore)

## <a name="summary"></a>Riepilogo

I backup automatici proteggono i database da errori dell'utente e delle applicazioni, dall'eliminazione accidentale e da interruzioni prolungate. Questa funzionalità incorporata è disponibile per tutti i livelli di servizio e le dimensioni di calcolo.

## <a name="next-steps"></a>Passaggi successivi

- Per la panoramica e gli scenari della continuità aziendale, vedere [Continuità aziendale del database SQL di Azure](sql-database-business-continuity.md).
- Per informazioni sui backup automatici del database SQL di Azure, vedere [Backup automatici del database SQL](sql-database-automated-backups.md).
- Per altre informazioni sulla conservazione a lungo termine, vedere [Conservazione a lungo termine](sql-database-long-term-retention.md).
- Per altre informazioni su opzioni di ripristino più veloci, vedere [Replica geografica attiva](sql-database-active-geo-replication.md) o [Gruppi di failover automatico](sql-database-auto-failover-group.md).

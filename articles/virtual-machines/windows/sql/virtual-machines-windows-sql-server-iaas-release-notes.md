---
title: Note sulla versione di SQL Server in una macchina virtuale Azure | Microsoft Docs
description: Informazioni sulle nuove funzionalità e i miglioramenti di SQL Server in una macchina virtuale Azure
services: virtual-machines-windows
author: MashaMSFT
ms.author: mathoma
manager: craigg
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 2/13/2019
ms.openlocfilehash: 23e072369aa8ac6ca6ada5ec185df1a8d7e03c5b
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59282543"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>Note sulla versione di SQL Server in una macchina virtuale Azure

Azure consente di distribuire una macchina virtuale con un'immagine di SQL Server predefinita. Questo articolo presenta una sintesi delle nuove funzionalità e dei miglioramenti introdotti nelle versioni recenti di [SQL Server in macchine virtuali di Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/). L'articolo elenca anche gli aggiornamenti del contenuto rilevanti non direttamente correlati alla versione ma pubblicati nello stesso intervallo di tempo. Per i miglioramenti apportati ad altri servizi di Azure, vedere [Aggiornamenti dei servizi](https://azure.microsoft.com/updates)

## <a name="march-2019"></a>Marzo 2019

### <a name="service-improvements"></a>Miglioramenti al servizio

| Miglioramenti al servizio | Dettagli |
| --- | --- |
| **Supporto di immagine personalizzata** | È ora possibile installare il [estensione SQL IaaS](virtual-machines-windows-sql-server-agent-extension.md#installation) alle immagini del sistema operativo e SQL personalizzate, che offre le funzionalità limitate dei [gestione flessibile delle licenze](virtual-machines-windows-sql-ahb.md). Quando la registrazione di un'immagine personalizzata con il provider di risorse SQL, specificare il tipo di licenza come 'AHUB' come in caso contrario, la registrazione avrà esito negativo.  | 
| **Supporto di istanza denominata** | È quindi possibile usare la [estensione SQL IaaS](virtual-machines-windows-sql-server-agent-extension.md#installation) con un'istanza denominata, se l'istanza predefinita è stato disinstallato correttamente. | 
| **Miglioramento del portale** | L'esperienza del portale di Azure per la distribuzione di una macchina virtuale di SQL Server è stato rinnovato per migliorare l'usabilità. Per altre informazioni, vedere i briefing [Quickstart](quickstart-sql-vm-create-portal.md) e più completa [sulle procedure](virtual-machines-windows-portal-sql-server-provision.md) Guida alla distribuzione di una macchina virtuale di SQL Server. |
| &nbsp; | &nbsp; |

### <a name="documentation-improvements"></a>Miglioramenti alla documentazione

Nessuna


## <a name="february-2019"></a>Febbraio 2019

### <a name="service-improvements"></a>Miglioramenti al servizio

| Miglioramenti al servizio | Dettagli |
| --- | --- |
| **Miglioramento del portale** | È ora possibile modificare il modello di licenza per una VM di SQL Server dal pagamento a consumo a bring-your-own-license utilizzando il [portale di Azure](virtual-machines-windows-sql-ahb.md#with-the-azure-portal-1).|
|**Semplificazione di distribuzione del gruppo di disponibilità con macchine Virtuali di SQL Azure CLI** | È ora più facile che mai, per distribuire un gruppo di disponibilità in una VM di SQL Server in Azure. [Comando della macchina virtuale di Azure SQL](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid) consente di creare il listener del gruppo di disponibilità, servizio ILB e il cluster WSFC, tutto da riga di comando e in fase di record. Per altre informazioni, vedere [usare CLI della macchina virtuale SQL di Azure per configurare gruppi di disponibilità AlwaysOn per SQL Server in una VM di Azure](virtual-machines-windows-sql-availability-group-cli.md). | 
| &nbsp; | &nbsp; |


## <a name="december-2018"></a>Dicembre 2018

| Miglioramenti al servizio | Dettagli |
| --- | --- |
| **Nuovo provider di risorse gruppo di cluster SQL** | È disponibile un nuovo provider di risorse (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups) che definisce i metadati del Cluster di failover di Windows. La partecipazione della VM di SQL Server a *SqlVirtualMachineGroups* esegue il bootstrap del servizio Cluster di failover di Windows e unisce le VM di SQL Server al cluster.  |
|**Automatizzare la configurazione di una distribuzione gruppo di disponibilità con modelli di avvio rapido di Azure** |È ora possibile creare il Cluster di failover di Windows, aggiungere VM di SQL Server a esso, creare il listener e configurare il bilanciamento del carico interno con due modelli di avvio rapido di Azure. Per altre informazioni, vedere [modello di avvio rapido di Azure usare per configurare gruppi di disponibilità AlwaysOn per SQL Server in una VM di Azure](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **Registrazione Provider di risorse automatico della macchina virtuale SQL** | Le VM di SQL Server distribuite dopo il mese corrente vengono registrate automaticamente con il nuovo provider di risorse di SQL Server. Le VM di SQL Server distribuite prima del mese corrente devono sempre essere registrate manualmente. Per altre informazioni, vedere [registrare VM di SQL esistente con il provider di risorse di VM di SQL](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider).|
| &nbsp; | &nbsp; |


## <a name="november-2018"></a>Novembre 2018

| Miglioramenti al servizio | Dettagli |
| --- | --- |
| **Nuovo provider di risorse di VM di SQL** |  È disponibile un nuovo provider di risorse per VM di SQL Server (Microsoft.SqlVirtualMachine) che consente una migliore gestione della VM di SQL Server. Per altre informazioni sulla registrazione di una macchina virtuale, vedere [Registrare una macchina virtuale SQL esistente con un nuovo provider di risorse](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider). |
|**Modello di licenza commutatore** |È ora possibile passare dal modello di licenza a pagamento in base all'utilizzo al modello di licenza Bring-Your-Own per la propria macchina virtuale SQL con l'interfaccia della riga di comando di Azure o Powershell. Per altre informazioni, vedere [Come cambiare il modello di licenza per una macchina virtuale SQL](virtual-machines-windows-sql-ahb.md). | 
| &nbsp; | &nbsp; |


## <a name="additional-resources"></a>Risorse aggiuntive

**Macchine virtuali Windows**:

* [Panoramica di SQL Server in una macchina virtuale Windows](virtual-machines-windows-sql-server-iaas-overview.md).
* [Effettuare il provisioning di un Windows SQL Server della macchina virtuale](virtual-machines-windows-portal-sql-server-provision.md)
* [Migrazione di un Database di SQL Server in una macchina virtuale di Azure](virtual-machines-windows-migrate-sql.md)
* [Disponibilità elevata e ripristino di emergenza per SQL Server in macchine virtuali di Azure](virtual-machines-windows-sql-high-availability-dr.md)
* [Procedure consigliate per le prestazioni per SQL Server in Macchine virtuali di Azure](virtual-machines-windows-sql-performance.md)
* [Modelli di applicazione e strategie di sviluppo per SQL Server in Macchine virtuali di Azure](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Macchine virtuali Linux**:

* [Panoramica di SQL Server in una VM Linux](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Effettuare il provisioning di una macchina virtuale Linux SQL Server](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [FAQ (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [SQL Server nella documentazione Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)

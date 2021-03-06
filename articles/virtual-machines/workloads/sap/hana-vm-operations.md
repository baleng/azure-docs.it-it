---
title: Configurazioni e operazioni dell'infrastruttura SAP HANA in Azure | Microsoft Docs
description: Guida operativa per i sistemi SAP HANA distribuiti in macchine virtuali di Azure.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6f60fdced25fdc594c28972f555bb28a9c629f21
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/03/2019
ms.locfileid: "58878656"
---
# <a name="sap-hana-infrastructure-configurations-and-operations-on-azure"></a>Configurazioni e operazioni dell'infrastruttura SAP HANA in Azure
Questa guida contiene le indicazioni necessarie per configurare l'infrastruttura di Azure e gestire i sistemi SAP HANA distribuiti in macchine virtuali native di Azure. Il documento include anche informazioni sulla configurazione per lo scale-out di SAP HANA per lo SKU di VM M128s. Questo documento non deve sostituire la documentazione standard di SAP, che include:

- [Guida all'amministrazione di SAP](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [Guide all'installazione di SAP](https://service.sap.com/instguides)
- [Note su SAP](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Prerequisiti
Per usare questa guida sono necessarie conoscenze di base dei componenti di Azure seguenti:

- [Macchine virtuali di Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [La rete di Azure e le reti virtuali](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Archiviazione di Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Per altre informazioni su SAP NetWeaver e altri componenti SAP in Azure, vedere la sezione [SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) della [documentazione di Azure](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Considerazioni di base sulla configurazione
Le sezioni seguenti offrono considerazioni di base sulla configurazione per la distribuzione di sistemi SAP HANA in macchine virtuali di Azure.

### <a name="connect-into-azure-virtual-machines"></a>Connettersi nelle macchine virtuali di Azure
Come illustrato nella [Guida alla pianificazione di macchine virtuali di Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide), sono disponibili due metodi di base per la connessione nelle macchine virtuali di Azure:

- Connessione tramite Internet ed endpoint pubblici in una macchina virtuale di collegamento o in una macchina virtuale che esegue SAP HANA.
- Connessione tramite una rete [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) o Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

Per gli scenari di produzione è necessaria la connettività da sito a sito tramite VPN o ExpressRoute. Questo tipo di connessione è necessario anche per gli scenari non di produzione che si inseriscono negli scenari di produzione in cui viene usato il software SAP. La figura seguente mostra un esempio di connettività intersito:

![Connettività intersito](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Scegliere i tipi di macchine virtuali di Azure
I tipi di macchine virtuali di Azure che possono essere usati per gli scenari di produzione sono elencati nella [documentazione SAP per IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Per gli scenari non di produzione è disponibile una gamma più ampia di macchine virtuali native di Azure.

>[!NOTE]
> Per gli scenari non di produzione, usare i tipi di macchine virtuali elencati nella [nota di SAP #1928533](https://launchpad.support.sap.com/#/notes/1928533). Per l'utilizzo di macchine virtuali di Azure per gli scenari di produzione, scegliere macchine virtuali con certificazione SAP HANA nell'[elenco di piattaforme IaaS certificate](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) pubblicato da SAP.

Per distribuire le macchine virtuali in Azure, usare:

- Il portale di Azure.
- I cmdlet di Azure PowerShell.
- L'interfaccia della riga di comando di Azure.

È anche possibile distribuire una piattaforma SAP HANA installata completa nei servizi di macchine virtuali di Azure tramite la [piattaforma SAP Cloud](https://cal.sap.com/). Per una descrizione del processo di installazione, vedere [Distribuire SAP S/4HANA o BW/4HANA in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h) oppure usare lo script di automazione rilasciato [qui](https://github.com/AzureCAT-GSI/SAP-HANA-ARM).

### <a name="choose-azure-storage-type"></a>Scegliere un tipo di archiviazione di Azure
Azure offre due tipi di archiviazione adatti per le macchine virtuali di Azure che eseguono SAP HANA: Unità disco rigido (HDD) Standard e unità SSD Premium. Per informazioni su questi tipi di dischi, vedere l'articolo [Selezionare un tipo di disco](../../windows/disks-types.md)

- Unità disco rigido (HDD) Standard
- Unità SSD Premium

Azure offre due metodi di distribuzione per VHD su Archiviazione Standard e Premium di Azure. Se lo scenario lo permette, sfruttare i vantaggi offerti dalle distribuzioni di [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/).

Per un elenco dei tipi di archiviazione e dei relativi contratti di servizio per operazioni di I/O al secondo e velocità effettiva di archiviazione, vedere la [documentazione di Azure per Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="configuring-the-storage-for-azure-virtual-machines"></a>Configurazione dell'archiviazione per macchine virtuali di Azure

Finora non è mai stato necessario preoccuparsi dei sottosistemi di I/O e delle relative funzionalità perché il fornitore dell'appliance deve assicurarsi che siano soddisfatti i requisiti di archiviazione minima per SAP HANA. Se si configura l'infrastruttura di Azure in autonomia, è necessario conoscere alcuni di questi requisiti anche per comprendere i requisiti di configurazione consigliati nelle sezioni seguenti. Lo stesso vale per la configurazione delle macchine virtuali su cui si vuole eseguire SAP HANA. Alcune delle caratteristiche richieste derivano dalla necessità di:

- Abilitare il volume di lettura/scrittura in **/hana/log** per minimo 250 MB/sec con dimensioni di I/O di 1 MB
- Abilitare l'attività di lettura per minimo 400 MB/sec per **/hana/data** con dimensioni di I/O di 16 MB e 64 MB
- Abilitare l'attività di scrittura per minimo 250 MB/sec per **/hana/data** con dimensioni di I/O di 16 MB e 64 MB

Detto questo, una bassa latenza di archiviazione è fondamentale per i sistemi DBMS, anche perché i sistemi DBMS, come SAP HANA, mantengono dati in memoria. Il percorso critico per l'archiviazione riguarda in genere le scritture nel log delle transazioni dei sistemi DBMS. Ma possono essere critiche anche operazioni come la scrittura di punti di salvataggio o il caricamento di dati in memoria dopo il ripristino da un arresto anomalo del sistema. È quindi obbligatorio usare Dischi Premium di Azure per i volumi **/hana/data** e **/hana/log**. Per ottenere la velocità effettiva minima di **/hana/log** e **/hana/data** come richiesto da SAP, è necessario configurare un sistema RAID 0 tramite MDADM o LVM su più dischi di Archiviazione Premium di Azure e usare i volumi RAID come volumi **/hana/data** e **/hana/log**. Per le dimensioni di striping di RAID 0 è raccomandato l'uso di:

- 64 KB o 128 KB per **/hana/data**
- 32 KB per **/hana/log**

> [!NOTE]
> Non è necessario configurare alcun livello di ridondanza con i volumi RAID perché l'Archiviazione Standard e Premium di Azure mantiene tre immagini di un disco rigido virtuale. L'utilizzo di un volume RAID è richiesto esclusivamente per configurare volumi in grado di offrire una velocità effettiva di I/O sufficiente.

Accumulare un certo numero di dischi rigidi virtuali di Azure in una configurazione RAID consente di ottenere valori cumulativi per le operazioni di I/O al secondo e la velocità effettiva di archiviazione. Pertanto, se si configura un sistema RAID 0 su 3 dischi di Archiviazione Premium di Azure P30, si dovrebbero ottenere livelli triplicati di operazioni di I/O al secondo e velocità effettiva di archiviazione rispetto a un singolo disco di Archiviazione Premium di Azure P30.

Gli elementi consigliati sulla memorizzazione nella cache riportati di seguito presuppongono caratteristiche di I/O per SAP HANA come:

- Non esiste quasi nessun carico di lavoro in lettura sui file di dati HANA, ad eccezione degli I/O di grandi dimensioni dopo il riavvio dell'istanza di HANA o quando i dati vengono caricati in HANA. Un altro caso di I/O in lettura di dimensioni maggiori sui file di dati è rappresentato dai backup di database HANA. Di conseguenza spesso la lettura della cache non ha senso in quanto, nella maggior parte dei casi, tutti i volumi dei file di dati devono essere letti completamente.
- La scrittura sui file di dati si verifica in burst in base ai punti di salvataggio HANA e al ripristino da arresto anomalo del sistema HANA. La scrittura di punti di salvataggio è un'operazione asincrona e non impedisce le transazioni dell'utente. La scrittura di dati durante il ripristino da arresto anomalo del sistema è un'operazione critica a livello di prestazioni per ottenere che il sistema risponda di nuovo rapidamente. Tuttavia, il ripristino da arresto anomalo del sistema dovrebbe essere un caso piuttosto eccezionale.
- Esistono pochissime operazioni di lettura dai file di rollforward HANA. Le eccezioni sono gli I/O di grandi dimensioni quando si eseguono backup dei log di transazioni, il ripristino da arresto anomalo del sistema o nella fase di riavvio di un'istanza HANA.  
- Il carico principale sul file di log di rollforward SAP HANA è rappresentato delle operazioni di scrittura. A seconda del tipo di carico di lavoro, è possibile che le operazioni di I/O siano di piccole dimensioni, ad esempio 4 kB o, in altri casi, di almeno 1 MB. La latenza di scrittura rispetto al log di rollforward SAP HANA è critica dal punto di vista delle prestazioni.
- Tutti i dati delle scritture devono essere salvati in modo permanente su disco in modo affidabile.

Tenendo conto di questi criteri di I/O osservati di SAP HANA, la memorizzazione nella cache per i diversi volumi tramite Archiviazione Premium di Azure deve essere impostata nel modo seguente:

- **/hana/data**: nessuna memorizzazione nella cache
- **/hana/log**: nessuna memorizzazione nella cache, ad eccezione della serie M come illustrato più avanti in questo documento
- **/hana/shared**: lettura della cache


Durante il ridimensionamento o quando si sceglie una macchina virtuale, tenere presente anche la velocità effettiva di I/O complessiva delle macchine virtuali. Per informazioni sulla velocità effettiva di archiviazione complessiva delle macchine virtuali, vedere l'articolo [Dimensioni delle macchine virtuali ottimizzate per la memoria](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

#### <a name="linux-io-scheduler-mode"></a>Modalità di pianificazione I/O Linux
Linux offre varie modalità di pianificazione I/O diverse. È una raccomandazione comune tra i fornitori di Linux e SAP di modificare la modalità di pianificazione I/O per i volumi di disco dalla modalità **cfq** alla modalità **noop**. Informazioni dettagliate sono disponibili nella [nota SAP n. 1984798](https://launchpad.support.sap.com/#/notes/1984787). 


#### <a name="storage-solution-with-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Soluzione di archiviazione con l'acceleratore di scrittura di Azure per macchine virtuali di Azure della serie M
L'acceleratore di scrittura di Azure è una funzionalità implementata esclusivamente per le macchine virtuali di Azure della serie M. Come indicato dal nome, lo scopo della funzionalità è migliorare la latenza di I/O delle scritture per l'Archiviazione Premium di Azure. Per SAP HANA, l'uso dell'acceleratore di scrittura è previsto solo sul volume **/hana/log**. Le configurazioni illustrate finora devono quindi essere modificate. La modifica principale è la suddivisione tra **/hana/data** e **/hana/log** per poter usare l'acceleratore di scrittura di Azure solo sul volume **/hana/log**. 

> [!IMPORTANT]
> La certificazione SAP HANA per le macchine virtuali di Azure della serie M è valida esclusivamente con l'acceleratore di scrittura di Azure per il volume **/hana/log**. Di conseguenza, per le distribuzioni di SAP HANA in scenari di produzione nelle macchine virtuali di Azure della serie M è prevista la configurazione con l'acceleratore di scrittura di Azure per il volume **/hana/log**.  

> [!NOTE]
> Per gli scenari di produzione, controllare se un determinato tipo di macchina virtuale è supportato da SAP per SAP HANA nella [documentazione SAP per IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

Le configurazioni consigliate sono simili alle seguenti:

| SKU di VM | RAM | Max. velocità effettiva<br /> Velocità effettiva | /hana/data | /hana/log | /hana/shared | volume /root | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GiB | 1000 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1000 GiB | 1000 MB/s | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |

Controllare se la velocità effettiva di archiviazione per i diversi volumi suggeriti potrà soddisfare i requisiti del carico di lavoro che si vuole eseguire. Se il carico di lavoro richiede volumi più elevati per **/hana/data** e **/hana/log**, è necessario aumentare il numero di dischi rigidi virtuali di Archiviazione Premium di Azure. Il ridimensionamento di un volume con più dischi rigidi virtuali di quelli elencati consentirà di aumentare le operazioni di I/O al secondo e la velocità effettiva di I/O entro i limiti del tipo di macchina virtuale di Azure.

L'acceleratore di scrittura di Azure funziona solo in combinazione con [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/). Almeno i dischi di Archiviazione Premium di Azure che costituiscono il volume **/hana/log** devono quindi essere distribuiti come dischi gestiti.

Esistono limiti per il numero di dischi rigidi virtuali di Archiviazione Premium di Azure per ogni macchina virtuale supportati dall'acceleratore di scrittura di Azure. I limiti correnti sono:

- 16 dischi rigidi virtuali per una macchina virtuale M128xx
- 8 dischi rigidi virtuali per una macchina virtuale M64xx
- 4 dischi rigidi virtuali per una macchina virtuale M32xx

Istruzioni più dettagliate su come abilitare l'acceleratore di scrittura di Azure sono disponibili nell'articolo [Acceleratore di scrittura](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).

La stessa documentazione include anche informazioni dettagliate e sulle restrizioni per l'acceleratore di scrittura di Azure.


#### <a name="cost-conscious-azure-storage-configuration"></a>Configurazione di Archiviazione di Azure tenendo conto dei costi
La tabella seguente mostra una configurazione di tipi di macchine virtuali comunemente usati dai clienti per ospitare SAP HANA in macchine virtuali di Azure. Potrebbero essere presenti alcuni tipi di macchina virtuale che non soddisfano tutti i criteri minimi per SAP HANA o non sono ufficialmente supportati con SAP HANA da SAP, anche se offrono prestazioni adeguate per scenari non di produzione. 

> [!NOTE]
> Per gli scenari di produzione, controllare se un determinato tipo di macchina virtuale è supportato da SAP per SAP HANA nella [documentazione SAP per IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).


| SKU di VM | RAM | Max. velocità effettiva<br /> Velocità effettiva | /hana/data and /hana/log<br /> con striping con LVM o MDADM | /hana/shared | volume /root | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 112 GiB | 768 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E16v3 | 128 GiB | 384 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 GiB | 768 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 432 GiB | 1200 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GiB | 2000 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M32ts | 192 GiB | 500 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M64ls | 512 GiB | 1000 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 |1 x S30 |
| M64s | 1000 GiB | 1000 MB/s | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


I dischi consigliati per i tipi di macchine virtuali più piccoli con 3 x P20 superano in dimensioni i volumi riguardanti le indicazioni di spazio contenute nel [white paper sull'archiviazione di SAP TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). Tuttavia, la scelta indicata nella tabella è stata effettuata per offrire una velocità effettiva del disco sufficiente per SAP HANA. Se è necessario apportare modifiche al volume **/hana/backup**, che è stato ridimensionato per mantenere backup che rappresentano il doppio del volume di memoria, procedere liberamente.   
Controllare se la velocità effettiva di archiviazione per i diversi volumi suggeriti potrà soddisfare i requisiti del carico di lavoro che si vuole eseguire. Se il carico di lavoro richiede volumi più elevati per **/hana/data** e **/hana/log**, è necessario aumentare il numero di dischi rigidi virtuali di Archiviazione Premium di Azure. Il ridimensionamento di un volume con più dischi rigidi virtuali di quelli elencati consentirà di aumentare le operazioni di I/O al secondo e la velocità effettiva di I/O entro i limiti del tipo di macchina virtuale di Azure. 

> [!NOTE]
> Le configurazioni sopra indicate NON traggono vantaggio dal [contratto di servizio per macchine virtuali con istanza singola di macchina virtuale di Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) perché usa in combinazione Archiviazione Premium di Azure e Archiviazione Standard di Azure. Tuttavia, la selezione è stata scelta per ottimizzare i costi. È necessario scegliere Archiviazione Premium per tutti i dischi sopra quello elencato come Archiviazione di Azure Standard (Sxx) per rendere la configurazione della macchina virtuale conforme al contratto di servizio per singole macchine virtuali di Azure.


> [!NOTE]
> I suggerimenti sulla configurazione del disco forniti fanno riferimento ai requisiti minimi definiti da SAP per i provider di infrastruttura. Nelle distribuzioni effettive dei clienti e negli scenari con carichi di lavoro reali, tuttavia, si sono verificate situazioni in cui questi suggerimenti non garantivano funzionalità sufficienti. Queste situazioni si possono verificare quando un cliente richiede un ricaricamento più veloce dei dati dopo un riavvio HANA o se una configurazione di backup richiede una larghezza di banda maggiore per l'archiviazione. Altri casi includono, ad esempio, percorsi **/hana/log** in cui 5000 IOPS non sono sufficienti per un carico di lavoro specifico. Considerare quindi queste indicazioni come punto di inizio e adattarle in base ai requisiti del carico di lavoro.
>  

### <a name="set-up-azure-virtual-networks"></a>Configurare le reti virtuali di Azure
Dopo aver stabilito la connettività da sito a sito in Azure tramite VPN o ExpressRoute, è necessario avere almeno una rete virtuale di Azure che sia connessa tramite un gateway virtuale al circuito VPN o ExpressRoute. Nelle distribuzioni semplici il gateway virtuale può essere distribuito in una subnet della rete virtuale di Azure che ospita anche le istanze di SAP HANA. Per installare SAP HANA, si creano altre due subnet all'interno della rete virtuale di Azure. Una subnet ospita le macchine virtuali necessarie per eseguire le istanze di SAP HANA, mentre l'altra esegue macchine virtuali Jumpbox o di gestione per ospitare SAP HANA Studio, altro software di gestione o il software dell'applicazione.

> [!IMPORTANT]
> Se non per motivi di funzionalità, ma più importante se non per motivi di prestazioni, non è supportato configurare [appliance virtuali di rete di Azure](https://azure.microsoft.com/solutions/network-appliances/) nel percorso di comunicazione tra l'applicazione SAP e il livello DBMS di un sistema SAP basato su SAP NetWeaver, Hybris o S/4 HANA. La comunicazione tra il livello applicazione SAP e il livello DBMS deve essere diretta. La limitazione non include le [regole Azure ASG e NSG](https://docs.microsoft.com/azure/virtual-network/security-overview), a condizione che queste regole ASG e NSG consentano la comunicazione diretta. Altri scenari in cui non sono supportate appliance virtuali di rete sono nei percorsi di comunicazione tra macchine virtuali di Azure che rappresentano i nodi del cluster Pacemaker di Linux e i dispositivi SBD, come descritto in [Disponibilità elevata per SAP NetWeaver su macchine virtuali di Azure in SUSE Linux Enterprise Server for SAP applications](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse). Oppure nei percorsi di comunicazione tra macchine virtuali di Azure e Windows Server SOFS configurati come descritto in [Clustering di un'istanza ASCS/SCS di SAP in un cluster di failover Windows tramite una condivisione file in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share). Le appliance virtuali di rete nei percorsi di comunicazione possono facilmente raddoppiare la latenza di rete tra due partner di comunicazione e possono limitare la velocità effettiva nei percorsi critici tra il livello applicazione SAP e il livello DBMS. In alcuni scenari esaminati con i clienti, le appliance virtuali di rete possono causare errori dei cluster Linux Pacemaker nei casi in cui le comunicazioni tra i nodi del cluster Linux Pacemaker e il relativo dispositivo SBD avvengono tramite un'appliance virtuale di rete.  
> 

> [!IMPORTANT]
> Un'altra struttura **NON** supportata è la separazione del livello dell'applicazione SAP e del livello DBMS in reti virtuali Azure diverse non [peer](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). È consigliabile separare il livello dell'applicazione SAP e il livello DBMS usando subnet all'interno di una rete virtuale di Azure, anziché usare reti virtuali di Azure diverse. Se si decide di non seguire il consiglio e separare i due livelli in reti virtuali diverse, le due reti virtuali devono essere [peer](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). Tenere presente che il traffico di rete tra due reti virtuali [peer](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) di Azure è soggetto a costi di trasferimento. Se il livello dell'applicazione SAP e il livello DBMS vengono separati tra due reti virtuali Azure peer, i grandi volumi di dati (dell'ordine di molti terabyte) scambiati tra il livello dell'applicazione SAP e il livello DBMS possono originare costi importanti. 

Quando si installano le macchine virtuali per l'esecuzione di SAP HANA, queste macchine virtuali devono essere dotate di:

- Due schede di interfaccia di rete virtuali installate: una si connette alla subnet di gestione e l'altra viene usata per la connessione dalla rete locale o da altre reti all'istanza di SAP HANA nella macchina virtuale di Azure.
- Indirizzi IP privati statici distribuiti per entrambe le schede di interfaccia di rete virtuali.

> [!NOTE]
> È consigliabile assegnare indirizzi IP statici tramite Azure alle singole schede di interfaccia di rete virtuali. Non è opportuno assegnare indirizzi IP statici all'interno del sistema operativo guest a una scheda di interfaccia di rete virtuale. Alcuni servizi di Azure, come Backup di Azure, si basano sul fatto che almeno la scheda di interfaccia di rete virtuale primaria sia impostata su DHCP e non su indirizzi IP statici. Vedere anche il documento [Risolvere i problemi relativi al backup delle macchine virtuali di Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Se è necessario assegnare più indirizzi IP statici a una macchina virtuale, occorre assegnare più schede di interfaccia di rete virtuali a una macchina virtuale.
>
>

Per le distribuzioni permanenti, è tuttavia necessario creare un'architettura di rete del data center virtuale in Azure. Questa architettura si consiglia la separazione tra il Gateway di rete virtuale di Azure che si connette a un'istanza locale in una rete virtuale di Azure separato. Questa rete virtuale separata deve ospitare tutto il traffico che lascia l'azienda in locale o a internet. Questo approccio consente di distribuire il software per il controllo e la registrazione del traffico che entra nel data center virtuale in Azure in questa rete virtuale dell'hub separata. Si avrà quindi una rete virtuale che ospita tutto il software e le configurazioni, correlata al traffico in ingresso e in uscita nella distribuzione di Azure.

Gli articoli [Data center virtuale di Microsoft Azure: una prospettiva di rete](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter) e [Data center virtuale di Azure e piano di controllo aziendale](https://docs.microsoft.com/azure/architecture/vdc/) forniscono altre informazioni sull'approccio basato su data center virtuale e sulla struttura della rete virtuale di Azure correlata.


>[!NOTE]
>Il traffico trasmesso tra una rete virtuale dell'hub e la rete virtuale spoke con il [peering reti virtuali di Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) è soggetto a [costi](https://azure.microsoft.com/pricing/details/virtual-network/) aggiuntivi. In base a tali costi, potrebbe essere necessario valutare l'opportunità di un compromesso tra l'esecuzione di una struttura di rete hub-spoke rigorosa e l'esecuzione di più [gateway di Azure ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways) da connettere agli "spoke" per ignorare il peering reti virtuali. Anche i gateway di Azure ExpressRoute comportano tuttavia [costi](https://azure.microsoft.com/pricing/details/vpn-gateway/) aggiuntivi. È anche possibile incorrere in costi aggiuntivi per il software di terze parti usato per la registrazione, il controllo e il monitoraggio del traffico di rete. A seconda dei costi per lo scambio di dati tramite il peering reti virtuali da un lato e dei costi creati dai gateway di Azure ExpressRoute aggiuntivi e dalle licenze software aggiuntive, è possibile decidere per la micro-segmentazione all'interno di una rete virtuale usando le subnet come unità di isolamento invece delle reti virtuali.


Per una panoramica dei diversi metodi per l'assegnazione di indirizzi IP, vedere [Tipi di indirizzi IP e metodi di allocazione in Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

Per le macchine virtuali che eseguono SAP HANA, è consigliabile usare indirizzi IP statici assegnati, perché alcuni attributi di configurazione per HANA fanno riferimento a indirizzi IP.

I [gruppi di sicurezza di rete di Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) vengono usati per indirizzare il traffico instradato all'istanza di SAP HANA o al jumpbox. I gruppi di sicurezza di rete ed eventualmente i [gruppi di sicurezza delle applicazioni](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups) sono associati alla subnet di SAP HANA e alla subnet di gestione.

La figura seguente mostra una panoramica di uno schema di distribuzione approssimativo per SAP HANA che segue un'architettura di rete virtuale hub-spoke:

![Schema di distribuzione approssimativo per SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)

Per distribuire SAP HANA in Azure senza una connessione da sito a sito, è comunque necessario schermare l'istanza di SAP HANA da Internet pubblico e nasconderla dietro un proxy di inoltro. In questo scenario di base la distribuzione si basa su servizi DNS predefiniti di Azure per risolvere i nomi host. Questi servizi sono particolarmente importanti in distribuzioni più complesse in cui vengono usati indirizzi IP pubblici. Usare i gruppi di sicurezza di rete di Azure e le [appliance virtuali di rete di Azure](https://azure.microsoft.com/solutions/network-appliances/) per controllare e monitorare il routing da Internet all'architettura di rete virtuale di Azure in Azure. L'immagine seguente mostra uno schema approssimativo per la distribuzione di SAP HANA senza connessione da sito a sito in un'architettura di rete virtuale hub-spoke:
  
![Schema di distribuzione approssimativo per SAP HANA senza connessione da sito a sito](media/hana-vm-operations/hana-simple-networking2.PNG)
 

Per un'altra descrizione di come usare le appliance virtuali di rete di Azure per controllare e monitorare l'accesso da Internet senza l'architettura di rete virtuale hub-spoke, vedere l'articolo [Distribuire appliance virtuali di rete con disponibilità elevata](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha).


## <a name="configuring-azure-infrastructure-for-sap-hana-scale-out"></a>Configurazione dell'infrastruttura di Azure per lo scale-out di SAP HANA
Microsoft ha uno SKU di VM di serie M certificato per la configurazione con scale-out di SAP HANA. Le VM di tipo M128s sono state certificate per uno scale-out di un massimo di 16 nodi. Per le modifiche alle certificazioni dello scale-out di SAP HANA nelle VM di Azure, vedere l'[elenco di piattaforme IaaS certificate](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

Le versioni minime del sistema operativo per la distribuzione di configurazioni di scale-out nelle macchine virtuali di Azure sono:

- SUSE Linux 12 SP3
- Red Hat Linux 7.4

Nella certificazione di scale-out a 16 nodi

- Un nodo è il nodo master
- Un massimo di 15 nodi sono nodi di lavoro

>[!NOTE]
>Nelle distribuzioni di scale-out nelle macchine virtuali di Azure non è possibile usare un nodo di standby
>

Il motivo per cui non è possibile configurare un nodo di standby è duplice:

- A questo punto, Azure non ha servizi NFS nativi. Di conseguenza, le condivisioni NFS devono essere configurate con l'aiuto di funzionalità di terze parti.
- Nessuna delle configurazioni NFS di terze parti può soddisfare i criteri di latenza di archiviazione per SAP HANA con le soluzioni distribuite in Azure.

Di conseguenza, i volumi **/hana/data** e **/hana/log** non possono essere condivisi. Se non si condividono questi volumi dei nodi singoli, non è possibile utilizzare un nodo di standby di SAP HANA in una configurazione con scale-out.

Di conseguenza, la struttura di base per un singolo nodo in una configurazione con scale-out sarà simile alla seguente:

![Nozioni di base sullo scale-out di un singolo nodo](media/hana-vm-operations/scale-out-basics.PNG)

La configurazione di base di un nodo di macchina virtuale per lo scale-out di SAP HANA è simile alla seguente:

- Per **/hana/shared**, si crea un cluster NFS a disponibilità elevata basato su SUSE Linux 12 SP3. Questo cluster ospiterà le condivisioni NFS **/hana/shared** della configurazione con scale-out e SAP NetWeaver o BW/4HANA Central Services. La documentazione per creare una configurazione di questo tipo è disponibile nell'articolo [Disponibilità elevata per NFS in macchine virtuali di Azure su SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs)
- Tutti gli altri volumi di disco **NON** vengono condivisi tra i diversi nodi e **NON** sono basati su NFS. Le configurazioni e le procedure per le installazioni di HANA con scale-out con **/hana/data** e **/hana/log** non condivisi sono illustrate più avanti in questo documento.

>[!NOTE]
>Il cluster NFS a disponibilità elevata visualizzato finora nelle immagini è supportato solo con SUSE Linux. Una soluzione NFS a disponibilità elevata, basata su Red Hat verrà suggerita in un secondo momento.

Il ridimensionamento dei volumi è lo stesso che nello scale-up, tranne che per **/hana/shared**. Per lo SKU di VM M128s, le dimensioni e i tipi suggeriti sono simili ai seguenti:

| SKU di VM | RAM | Max. velocità effettiva<br /> Velocità effettiva | /hana/data | /hana/log | volume /root | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | --- |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 2 x P20 | 1 x P6 | 1 x P6 | 2 x P40 |


Controllare se la velocità effettiva di archiviazione per i diversi volumi suggeriti potrà soddisfare i requisiti del carico di lavoro che si vuole eseguire. Se il carico di lavoro richiede volumi più elevati per **/hana/data** e **/hana/log**, è necessario aumentare il numero di dischi rigidi virtuali di Archiviazione Premium di Azure. Il ridimensionamento di un volume con più dischi rigidi virtuali di quelli elencati consentirà di aumentare le operazioni di I/O al secondo e la velocità effettiva di I/O entro i limiti del tipo di macchina virtuale di Azure. Applicare anche l'acceleratore di scrittura di Azure ai dischi che costituiscono il volume **/hana/log**.
 
Nel documento [SAP HANA TDI Storage Requirements](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) (Requisiti di archiviazione di SAP HANA TDI) viene indicata una formula per definire le dimensioni del volume **/hana/shared** per lo scale-out come dimensioni memoria di un singolo nodo di lavoro ogni quattro nodi di lavoro.

Supponendo che si usi la VM di Azure M128s certificata per lo scale-out di SAP HANA con circa 2 TB di memoria, le raccomandazioni SAP possono essere riepilogate come segue:

- Un nodo master e fino a quattro nodi di lavoro. La dimensione del volume **/hana/shared** deve essere di 2 TB. 
- Un nodo master e da cinque a otto nodi di lavoro. La dimensione di **/hana/shared** deve essere di 4 TB. 
- Un nodo master e da 9 a 12 nodi di lavoro. Per **/hana/shared** sarà necessaria una dimensione di 6 TB. 
- Un nodo master e da 12 a 15 nodi di lavoro. Per il volume **/hana/shared** è necessaria una dimensione di 8 TB.

L'altra importante struttura visualizzata nelle immagini della configurazione a nodo singolo per una VM SAP HANA con scale-out è la configurazione della rete virtuale, o meglio delle subnet. SAP consiglia di separare il traffico destinato ai client o alle applicazioni dalle comunicazioni tra i nodi HANA. Come illustrato nelle immagini, questo obiettivo viene raggiunto con due diverse schede di interfaccia di rete virtuale collegate alla VM. Le due schede di interfaccia di rete virtuale si trovano in subnet diverse e hanno due indirizzi IP diversi. È quindi possibile controllare il flusso del traffico con regole di routing che usano gruppi di sicurezza di rete o route definite dall'utente.

In particolare in Azure non esistono mezzi e metodi per applicare Qualità del servizio (QoS) e quote in schede di interfaccia di rete virtuale specifiche. Di conseguenza, la separazione delle comunicazioni tra i nodi e verso i client o le applicazioni non consente in alcun modo di stabilire la priorità di un flusso di traffico su un altro. La separazione rimane invece una misura di sicurezza per proteggere le comunicazioni tra i nodi delle configurazioni con scale-out.  

>[!IMPORTANT]
>SAP consiglia di separare il traffico di rete destinato ai client o alle applicazioni e il traffico tra i nodi, come descritto in questo documento. È quindi consigliabile adottare un'architettura come quella illustrata nelle ultime immagini.
>

Per quanto riguarda la rete, l'architettura minima richiesta sarà simile alla seguente:

![Nozioni di base sullo scale-out di un singolo nodo](media/hana-vm-operations/scale-out-networking-overview.PNG)

I limiti supportati finora sono di 15 nodi di lavoro oltre al nodo master.

Per quanto riguarda lo spazio di archiviazione, l'architettura sarà simile alla seguente:


![Nozioni di base sullo scale-out di un singolo nodo](media/hana-vm-operations/scale-out-storage-overview.PNG)

Il volume **/hana/shared** si trova nella configurazione della condivisione NFS a disponibilità elevata. Tutte le altre unità sono invece montate "in locale" nelle singole VM. 

### <a name="highly-available-nfs-share"></a>Condivisione NFS a disponibilità elevata
Il cluster NFS a disponibilità elevata finora funziona solo con SUSE Linux. Il documento [Disponibilità elevata per NFS in macchine virtuali di Azure su SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) descrive come configurarlo. Se si non condivide il cluster NFS con altre configurazioni HANA all'esterno della rete virtuale di Azure che esegue le istanze di SAP HANA, installarlo nella stessa rete virtuale. Installarlo nella subnet e assicurarsi che non tutto il traffico non autorizzato possa accedere alla subnet. L'obiettivo è invece limitare il traffico verso tale subnet agli indirizzi IP della VM che gestiscono il traffico verso il volume **/hana/shared**.

Per quanto riguarda la scheda di interfaccia di rete virtuale di una VM con scale-out HANA che deve instradare il traffico di **/hana/shared**, le raccomandazioni sono:

- Poiché il traffico verso **/hana/shared** è moderato, instradarlo tramite la scheda di interfaccia di rete virtuale assegnata alla rete client nella configurazione minima
- Se necessario, per il traffico verso **/hana/shared**, distribuire una terza subnet nella rete virtuale in cui si distribuisce la configurazione con scale-out SAP HANA e assegnare una terza scheda di interfaccia di rete virtuale ospitata in tale subnet. Usare la terza scheda di interfaccia di rete virtuale e l'indirizzo IP associato per il traffico verso la condivisione NFS. È quindi possibile applicare regole di routing e accesso separate.

>[!IMPORTANT]
>In nessun caso il traffico di rete tra le VM con SAP HANA distribuito con lo scale-out e NFS a disponibilità elevata può essere instradato tramite un'[appliance virtuale di rete](https://azure.microsoft.com/solutions/network-appliances/) o appliance virtuali simili. I gruppi di sicurezza di rete di Azure invece non sono dispositivi di questo tipo. Controllare le regole di routing per verificare che le appliance virtuali di rete o appliance virtuali simili vengano deviate quando accedono alla condivisione NFS a disponibilità elevata dalle macchine virtuali che eseguono SAP HANA.
> 

Per condividere il cluster NFS a disponibilità elevata tra le configurazioni SAP HANA, spostare tutte le configurazioni HANA nella stessa rete virtuale. 
 

### <a name="installing-sap-hana-scale-out-n-azure"></a>Installazione dello scale-out SAP HANA in Azure
Per installare una configurazione SAP con scale-out, è necessario eseguire i passaggi generali seguenti:

- Distribuzione o adattamento di una nuova infrastruttura di rete virtuale di Azure
- Distribuzione delle nuove macchine virtuali tramite volumi di Archiviazione Premium gestiti di Azure
- Distribuzione di un nuovo cluster NFS a disponibilità elevata o adattamento di uno esistente
- Adattare il routing di rete per assicurarsi che, ad esempio, la comunicazione tra nodi tra le macchine virtuali non venga instradata tramite un'[appliance virtuale di rete](https://azure.microsoft.com/solutions/network-appliances/). Lo stesso vale per il traffico tra le VM e il cluster NFS a disponibilità elevata.
- Installare il nodo master SAP HANA.
- Adattare i parametri di configurazione del nodo master SAP HANA
- Continuare l'installazione dei nodi di lavoro SAP HANA

#### <a name="installation-of-sap-hana-in-scale-out-configuration"></a>Installazione di SAP HANA nella configurazione con scale-out
Quando viene distribuita l'infrastruttura di VM di Azure e vengono eseguite tutte le altre operazioni di preparazione, è necessario installare le configurazioni con scale-out di SAP HANA seguendo questi passaggi:

- Installare il nodo master SAP HANA in base alla documentazione di SAP
- **Dopo l'installazione, è necessario modificare il file global.ini e aggiungere il parametro "basepath_shared = no" a global.ini**. Questo parametro consentirà di eseguire SAP HANA in modalità scale-out senza i volumi **/hana/data** and **/hana/log** condivisi tra i nodi. Informazioni dettagliate sono fornite nella [nota SAP n. 2080991](https://launchpad.support.sap.com/#/notes/2080991).
- Dopo aver modificato il parametro global.ini, riavviare l'istanza di SAP HANA
- Aggiungere altri nodi di lavoro. Vedere anche <https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US/0d9fe701e2214e98ad4f8721f6558c34.html>. Specificare la rete interna per la comunicazione tra nodi di SAP HANA durante l'installazione o in seguito usando, ad esempio, hdblcm locale. Per una documentazione più dettagliata, vedere anche la [nota SAP n. 2183363](https://launchpad.support.sap.com/#/notes/2183363). 

Seguendo questa routine di configurazione, la configurazione con scale-out installata userà dischi non condivisi per eseguire **/hana/data** e **/hana/log**. Il volume **/hana/shared** verrà invece inserito nella condivisione NFS a disponibilità elevata.


## <a name="sap-hana-dynamic-tiering-20-for-azure-virtual-machines"></a>SAP HANA Dynamic Tiering 2.0 per macchine virtuali di Azure

Oltre alle certificazioni di SAP HANA nelle macchine virtuali della serie M di Azure, anche SAP HANA Dynamic Tiering 2.0 è supportato in Microsoft Azure. Vedere più avanti i collegamenti alla documentazione di SAP HANA Dynamic Tiering. Anche se non esistono differenze nell'installazione o nel funzionamento del prodotto, ad esempio, tramite il pannello di controllo di SAP HANA in una macchina virtuale, alcuni elementi importanti sono obbligatori per il supporto ufficiale in Azure. Di seguito vengono descritti questi punti chiave. Nel resto di questo articolo verrà usata l'abbreviazione "DT 2.0" invece del nome completo Dynamic Tiering 2.0.

SAP HANA Dynamic Tiering 2.0 non è supportato da SAP BW o S4HANA. I casi d'uso principali sono attualmente le applicazioni HANA native.


### <a name="overview"></a>Panoramica

La figura seguente offre una panoramica del supporto di DT 2.0 in Microsoft Azure. Esiste un set di requisiti obbligatori, che deve essere seguito per garantire la conformità con la certificazione ufficiale:

- DT 2.0 deve essere installato in una VM di Azure dedicata. Non può essere eseguito nella stessa VM in cui viene eseguito SAP HANA
- Le VM SAP HANA e DT 2.0 devono essere distribuite nella stessa rete virtuale di Azure
- Le VM SAP HANA e DT 2.0 devono essere distribuite con la rete accelerata di Azure abilitata
- Il tipo di archiviazione per le VM DT 2.0 deve essere Archiviazione premium di Azure
- Alla VM DT 2.0 devono essere collegati più dischi di Azure
- È necessario creare un volume RAID/con striping software (tramite lvm o mdadm) usando lo striping tra i dischi di Azure

Altri dettagli verranno illustrati nelle sezioni seguenti.

![Panoramica dell'architettura di SAP HANA DT 2.0](media/hana-vm-operations/hana-dt-20.PNG)



### <a name="dedicated-azure-vm-for-sap-hana-dt-20"></a>VM di Azure dedicata per SAP HANA DT 2.0

In IaaS di Azure DT 2.0 è supportato solo in una VM dedicata. Non è consentito eseguire DT 2.0 nella stessa VM di Azure in cui è in esecuzione l'istanza di HANA. Inizialmente si possono usare due tipi di macchina virtuale per eseguire SAP HANA DT 2.0:

- M64-32ms 
- E32sv3 

Per la descrizione dei tipi di VM, vedere [qui](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

Considerata l'idea alla base di DT 2.0, ovvero l'offloading dei dati ad accesso frequente per risparmiare sui costi, la scelta migliore è usare dimensioni di VM corrispondenti. Non esistono tuttavia regole rigide sulle possibili combinazioni. Tutto dipende dal carico di lavoro del cliente specifico.

Le configurazioni consigliate sono:

| Tipo di VM SAP HANA | Tipo di VM DT 2.0 |
| --- | --- | 
| M128ms | M64-32ms |
| M128s | M64-32ms |
| M64ms | E32sv3 |
| M64s | E32sv3 |


Sono possibili tutte le combinazioni di macchine virtuali della serie M certificate per SAP HANA con macchine virtuali DT 2.0 supportate (M64-32ms e E32sv3).


### <a name="azure-networking-and-sap-hana-dt-20"></a>Rete di Azure e SAP HANA DT 2.0

Per installare DT 2.0 in una VM dedicata, è necessaria una velocità effettiva di rete tra la VM DT 2.0 e la VM SAP HANA di almeno 10 GB. È quindi obbligatorio inserire tutte le VM nella stessa rete virtuale di Azure e abilitare la rete accelerata di Azure.

Per altre informazioni sulla rete accelerata di Azure, vedere [qui](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli)

### <a name="vm-storage-for-sap-hana-dt-20"></a>Risorse di archiviazione delle macchine virtuali per SAP HANA DT 2.0

In base alle indicazioni sulle procedure consigliate di DT 2.0, la velocità effettiva di I/O dei dischi deve essere almeno di 50 MB/sec per ogni core fisico. Dalla specifica per i due tipi di macchina virtuale di Azure supportati per DT 2.0 è possibile ricavare il limite massimo di velocità effettiva di I/O dei dischi per la macchina virtuale:

- E32sv3:   768 MB/sec (senza memorizzazione nella cache), ovvero un rapporto di 48 MB/sec per ogni core fisico
- M64-32ms:  1000 MB/sec (senza memorizzazione nella cache), ovvero un rapporto di 62,5 MB/sec per ogni core fisico

È necessario collegare più dischi di Azure alla VM DT 2.0 e creare un volume RAID (striping) di software a livello di sistema operativo per raggiungere il limite massimo della velocità effettiva dei dischi per ogni VM. Un singolo disco di Azure non può fornire la velocità effettiva per raggiungere il limite massimo per le VM in questo ambito. Archiviazione Premium di Azure è obbligatorio per eseguire DT 2.0. 

- Per informazioni dettagliate sui tipi di dischi di Azure, vedere [qui](../../windows/disks-types.md)
- Per informazioni dettagliate sulla creazione del volume RAID del software tramite mdadm, vedere [qui](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid)
- Per informazioni dettagliate sulla configurazione di LVM per creare un volume con striping per la velocità effettiva massima, vedere [qui](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm)

A seconda dei requisiti delle dimensioni, sono disponibili diverse opzioni per raggiungere la velocità effettiva massima di una VM. Di seguito sono elencate le possibili configurazioni dei dischi volume di dati per ogni tipo di VM DT 2.0 per raggiungere il limite massimo di velocità effettiva della VM. La VM E32sv3 deve essere considerata come base per i carichi di lavoro più piccoli. Se non dovesse risultare abbastanza veloce, potrebbe essere necessario ridimensionare la VM a M64-32ms.
Poiché la VM M64-32ms ha una quantità elevata di memoria, il carico di I/O potrebbe non raggiungere il limite soprattutto per i carichi di lavoro che eseguono un'intensa attività di lettura. Un numero inferiore di dischi nel set di striping potrebbe quindi essere sufficiente a seconda del carico di lavoro specifico del cliente, ma, per sicurezza, le configurazioni dei dischi seguenti sono state scelte per garantire la velocità effettiva massima:


| SKU di VM | Configurazione disco 1 | Configurazione disco 2 | Configurazione disco 3 | Configurazione disco 4 | Configurazione disco 5 | 
| ---- | ---- | ---- | ---- | ---- | ---- | 
| M64-32ms | 4 x P50 -> 16 TB | 4 x P40 -> 8 TB | 5 x P30 -> 5 TB | 7 x P20 -> 3,5 TB | 8 x P15 -> 2 TB | 
| E32sv3 | 3 x P50 -> 12 TB | 3 x P40 -> 6 TB | 4 x P30 -> 4 TB | 5 x P20 -> 2,5 TB | 6 x P15 -> 1,5 TB | 


Soprattutto in caso di intensa attività di lettura del carico di lavoro, per incrementare le prestazioni di I/O, è possibile attivare la cache host di Azure "di sola lettura" consigliata per i volumi di dati del software del database. Per il log delle transazioni invece, la cache del disco host di Azure deve essere "nessuna". 

Per quanto riguarda le dimensioni del volume di log, è consigliabile iniziare con un 15% euristico delle dimensioni dei dati. La creazione del volume di log può essere eseguita usando tipi diversi di dischi di Azure a seconda del costo e dei requisiti di velocità effettiva. Per il volume di log è richiesta una velocità effettiva di I/O elevata.  Se si usa il tipo di macchina virtuale M64-32ms, è caldamente consigliata l'abilitazione dell'[acceleratore di scrittura](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator). L'acceleratore di scrittura di Azure offre la latenza di scrittura su disco ottimale per il log delle transazioni (disponibile solo per la serie M). Esistono tuttavia alcuni elementi da considerare, ad esempio, il numero massimo di dischi per tipo di VM. [Qui](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator) sono disponibili informazioni dettagliate sull'acceleratore di scrittura


Ecco alcuni esempi di dimensioni del volume di log:

| dimensioni volume dati e tipo di disco | configurazione volume di log e tipo di disco 1 | configurazione volume di log e tipo di disco 2 |
| --- | --- | --- |
| 4 x P50 -> 16 TB | 5 x P20 -> 2,5 TB | 3 x P30 -> 3 TB |
| 6 x P15 -> 1,5 TB | 4 x P6 -> 256 GB | 1 x P15 -> 256 GB |


Come per lo scale-out di SAP HANA, la directory /hana/shared deve essere condivisa tra la VM SAP HANA e la VM DT 2.0. È consigliabile la stessa architettura dello scale-out di SAP HANA con VM dedicate, che fungono da server NFS a disponibilità elevata. Per fornire un volume di backup condiviso, si può usare la medesima progettazione, ma sarà il cliente a decidere se è necessaria la disponibilità elevata o se è possibile usare solo una VM dedicata con capacità di archiviazione sufficiente per fungere da server di backup.



### <a name="links-to-dt-20-documentation"></a>Collegamenti alla documentazione di DT 2.0 

- [Guida all'installazione e aggiornamento SAP HANA dinamica la suddivisione in livelli](https://help.sap.com/viewer/88f82e0d010e4da1bc8963f18346f46e/2.0.03/en-US)
- [Risorse ed esercitazioni di SAP HANA dinamica la suddivisione in livelli](https://help.sap.com/viewer/fb9c3779f9d1412b8de6dd0788fa167b/2.0.03/en-US)
- [SAP HANA dinamica suddivisione in livelli dell'architettura PoC](https://blogs.sap.com/2017/12/08/sap-hana-dynamic-tiering-delivering-on-low-tco-with-impressive-performance/)
- [Miglioramenti di suddivisione in livelli dinamici di SAP HANA 2.0 SPS 02](https://blogs.sap.com/2017/07/31/sap-hana-2.0-sps-02-dynamic-tiering-enhancements/)




## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>Operazioni per la distribuzione di SAP HANA in macchine virtuali di Azure
Le sezioni seguenti descrivono alcune delle operazioni correlate alla distribuzione di sistemi SAP HANA in macchine virtuali di Azure.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Operazioni di backup e ripristino nelle macchine virtuali di Azure
I documenti seguenti descrivono come eseguire il backup e il ripristino della distribuzione di SAP HANA:

- [Panoramica del backup di SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [Backup di SAP HANA a livello di file](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Backup degli snapshot di archiviazione SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>Avviare e riavviare le macchine virtuali che contengono SAP HANA
Una delle principali caratteristiche del cloud pubblico di Azure è che i costi vengono addebitati solo in base ai minuti di calcolo. Ad esempio, quando si arresta una macchina virtuale che esegue SAP HANA, durante l'intervallo di calcolo vengono fatturati solo i costi di archiviazione. Un'altra caratteristica si presenta quando si specificano gli indirizzi IP statici per le macchine virtuali nella distribuzione iniziale. Quando si riavvia una macchina virtuale con SAP HANA, la macchina viene riavviata con l'indirizzo IP precedente. 


### <a name="use-saprouter-for-sap-remote-support"></a>Usare SAProuter per il supporto remoto SAP
Se esiste una connessione da sito a sito tra le sedi locali e Azure e si eseguono componenti SAP, è probabile che SAProuter sia già in esecuzione. In questo caso, completare le attività seguenti per il supporto remoto:

- Gestire l'indirizzo IP privato e statico della macchina virtuale che ospita SAP HANA nella configurazione di SAProuter.
- Configurare il gruppo di sicurezza di rete della subnet che ospita la macchina virtuale HANA per consentire il traffico attraverso la porta TCP/IP 3299.

Se ci si connette ad Azure tramite Internet senza avere a disposizione un router SAP per la macchina virtuale con SAP HANA, è necessario installare il componente. Installare SAProuter in una macchina virtuale separata nella subnet di gestione. L'immagine seguente mostra uno schema approssimativo per la distribuzione di SAP HANA senza connessione da sito a sito e con SAProuter:

![Schema di distribuzione approssimativo per SAP HANA senza connessione da sito a sito e con SAProuter](media/hana-vm-operations/hana-simple-networking3.PNG)

Installare SAProuter in una macchina virtuale separata e non nella macchina virtuale Jumpbox. La macchina virtuale separata deve avere un indirizzo IP statico. Per connettere la propria istanza di SAProuter all'istanza di SAProuter ospitata da SAP, contattare SAP per chiedere un indirizzo IP. L'istanza di SAProuter ospitata da SAP è la controparte dell'istanza di SAProuter installata nella macchina virtuale. Usare l'indirizzo IP fornito da SAP per configurare l'istanza di SAProuter. Nelle impostazioni di configurazione l'unica porta necessaria è la porta TCP 3299.

Per altre informazioni su come configurare e gestire le connessioni di supporto remoto tramite SAProuter, vedere la [documentazione di SAP](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Disponibilità elevata con SAP HANA in macchine virtuali native di Azure
Se si esegue SUSE Linux Enterprise Server for SAP Applications 12 SP1 o versione successiva, è possibile creare un cluster Pacemaker con dispositivi STONITH. È possibile usare i dispositivi per impostare una configurazione di SAP HANA che usi la replica sincrona con la replica di sistema HANA e il failover automatico. Per altre informazioni sulla procedura di configurazione, vedere [Guida alla disponibilità elevata di SAP HANA per macchine virtuali di Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).

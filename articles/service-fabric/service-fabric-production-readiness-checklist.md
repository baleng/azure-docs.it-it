---
title: Elenco di controllo per l'idoneità per la produzione di Azure Service Fabric | Microsoft Docs
description: Preparare l'applicazione e il cluster di Service Fabric per la produzione seguendo le procedure consigliate.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chakdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/10/2018
ms.author: aljo
ms.openlocfilehash: e94280f9df1d4ac59856a73f6f6c2b7f7a0b9cc0
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/18/2019
ms.locfileid: "58107492"
---
# <a name="production-readiness-checklist"></a>Elenco di controllo per l'idoneità per la produzione

L'applicazione e il cluster sono pronti ad accettare il traffico della produzione? L'esecuzione e il test dell'applicazione e del cluster non implicano necessariamente che questi siano pronti per l'uso in produzione. Per assicurare il corretto funzionamento dell'applicazione e del cluster, esaminare le voci dell'elenco di controllo seguente. È altamente consigliabile verificare che tutti i requisti riportati nell'elenco siano soddisfatti. È ovviamente possibile scegliere di usare soluzioni alternative per una determinata voce, ad esempio framework di diagnostica personalizzati.


## <a name="pre-requisites-for-production"></a>Prerequisiti per la produzione
1. Le [procedure consigliate per la sicurezza di Azure Service Fabric](https://docs.microsoft.com/azure/security/azure-service-fabric-security-best-practices) sono: 
1. Usare certificati X.509
1. Configurare criteri di sicurezza
1. Configurare l'autenticazione SSL per Azure Service Fabric
1. Usare l'isolamento e la sicurezza di rete con Azure Service Fabric
1. Configurare Azure Key Vault per la sicurezza
1. Microsoft.Network/loadBalancersAssegnare utenti ai ruoli
1. Implementare la configurazione di sicurezza di Reliable Actors, se si usa il modello di programmazione di Actors
1. Per i cluster con più di 20 core o 10 nodi, creare un tipo di nodo primario dedicato per i servizi di sistema. Aggiungere [vincoli di posizionamento](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) per riservare il tipo di nodo primario ai servizi di sistema.
1. Per il tipo di nodo primario usare uno SKU D2v2 o superiore. È consigliabile scegliere uno SKU con capacità del disco rigido di almeno 50 GB.
1. I cluster di produzione devono essere [sicuri](service-fabric-cluster-security.md). Per un esempio di configurazione di un cluster sicuro, vedere questo [modello di cluster](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/7-VM-Windows-3-NodeTypes-Secure-NSG). Usare nomi comuni per i certificati ed evitare i certificati autofirmati.
1. Applicare [vincoli di risorse a contenitori e servizi](service-fabric-resource-governance.md) per evitare che utilizzino più del 75% delle risorse dei nodi. 
1. Comprendere la finalità del [livello di durabilità](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) e impostarlo. Un livello di durabilità Silver o superiore è consigliato per i tipi di nodo che eseguono carichi di lavoro con stato. Per il tipo di nodo primario il livello di durabilità deve essere impostato su Silver o superiore.
1. Comprendere la finalità del [livello di affidabilità](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) del tipo di nodo e selezionarlo. È consigliabile impostare un livello di affidabilità Silver o superiore.
1. Eseguire i test di carico e scalabilità dei carichi di lavoro per identificare i [requisiti di capacità](service-fabric-cluster-capacity.md) per il cluster. 
1. I servizi e le applicazioni sono sottoposti a monitoraggio e vengono generati e archiviati log delle applicazioni, con funzionalità di avviso. Ad esempio, vedere [aggiungere la registrazione all'applicazione di Service Fabric](service-fabric-how-to-diagnostics-log.md) e [monitorare i contenitori con log di monitoraggio di Azure](service-fabric-diagnostics-oms-containers.md).
1. Il cluster esegue il monitoraggio con avvisi (ad esempio, con [monitoraggio di Azure registra](service-fabric-diagnostics-event-analysis-oms.md)). 
1. L'infrastruttura di set di scalabilità di macchina virtuale sottostante viene monitorata con avvisi (ad esempio, con [monitoraggio di Azure registra](service-fabric-diagnostics-oms-agent.md).
1. Il cluster ha sempre [un certificato primario e uno secondario](service-fabric-cluster-security-update-certs-azure.md) per evitare che gli utenti restino bloccati.
1. Gestire cluster separati per lo sviluppo, la gestione temporanea e la produzione. 
1. Gli [aggiornamenti delle applicazioni](service-fabric-application-upgrade.md) e [quelli dei cluster](service-fabric-tutorial-upgrade-cluster.md) vengono prima testati nei cluster di sviluppo e di gestione temporanea. 
1. Disattivare gli aggiornamenti automatici nei cluster di produzione e attivarli per i cluster di sviluppo e gestione temporanea, ripristinando lo stato precedente, quando necessario. 
1. Definire un Obiettivo del punto di ripristino (RPO) per il servizio, configurare un [processo di ripristino di emergenza](service-fabric-disaster-recovery.md) e testarlo.
1. Pianificare il [ridimensionamento](service-fabric-cluster-scaling.md) del cluster manualmente o a livello di codice.
1. Pianificare l'[applicazione di patch](service-fabric-patch-orchestration-application.md) ai nodi del cluster. 
1. Configurare una pipeline di integrazione continua/distribuzione continua (CI/CD) in modo che le modifiche più recenti vengano continuamente testate, Ad esempio, usando [Azure DevOps](service-fabric-tutorial-deploy-app-with-cicd-vsts.md) o [Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md)
1. Testare i cluster di sviluppo e gestione temporanea in condizioni di carico con il [servizio di analisi degli errori](service-fabric-testability-overview.md) e indurre una condizione di [chaos](service-fabric-controlled-chaos.md) in un ambiente controllato. 
1. Pianificare il [ridimensionamento](service-fabric-concepts-scalability.md) delle applicazioni. 


Se si usa il modello di programmazione Reliable Actors o Reliable Services di Service Fabric, è necessario verificare che siano soddisfatti i requisiti seguenti:
1. Aggiornare le applicazioni durante la fase di sviluppo locale per verificare che il codice del servizio rispetti il token di annullamento nel metodo `RunAsync` e chiuda i listener di comunicazione personalizzati.
1. Evitare gli [inconvenienti comuni](service-fabric-work-with-reliable-collections.md) che si verificano durante l'uso delle raccolte Reliable Collections.
1. Monitorare i contatori delle prestazioni della memoria CLR .NET durante l'esecuzione dei test di carico e verificare la presenza di frequenze elevate di Garbage Collection o l'aumento delle dimensioni degli heap con eccessivo tempo di esecuzione.
1. Mantenere copie di backup offline di [Reliable Services e Reliable Actors](service-fabric-reliable-services-backup-restore.md) e testare il processo di ripristino.
1. Il numero di istanze di macchina virtuale di tipo nodo primario deve idealmente essere uguale al valore minimo per il livello di affidabilità dei cluster. Le condizioni che rendono appropriato il superamento temporaneo del valore minimo del livello sono: ridimensionamento verticale dello SKU del set di scalabilità di macchine virtuali dei nodi di tipo primario.

## <a name="optional-best-practices"></a>Procedure consigliate facoltative

Anche se gli elenchi precedenti includono i prerequisiti per passare alla fase di produzione, è necessario prendere in considerazione anche le procedure consigliate seguenti:
1. Stabilire un collegamento al [modello di integrità di Service Fabric](service-fabric-health-introduction.md) per estendere le funzionalità predefinite di generazione di report e valutazione dell'integrità.
1. Distribuire un watchdog personalizzato che esegue il monitoraggio dell'applicazione e genera report relativi al [carico](service-fabric-cluster-resource-manager-metrics.md) per il [bilanciamento delle risorse](service-fabric-cluster-resource-manager-balancing.md). 


## <a name="next-steps"></a>Passaggi successivi
* [Distribuire un cluster Windows di Service Fabric](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* [Distribuire un cluster Linux di Service Fabric](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* Altre informazioni sul [ciclo di vita dell'applicazione](service-fabric-application-lifecycle.md) di Service Fabric.

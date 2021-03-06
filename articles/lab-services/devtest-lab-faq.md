---
title: Domande frequenti su Azure DevTest Labs | Documentazione Microsoft
description: Risposte alle domande comuni su Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: afe83109-b89f-4f18-bddd-b8b4a30f11b4
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: spelluru
ms.openlocfilehash: d8fc929b21bedcb3e7e2bd3f5ed1d6c867bca3c8
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/01/2019
ms.locfileid: "58803375"
---
# <a name="azure-devtest-labs-faq"></a>Domande frequenti su Azure DevTest Labs
Ottenere le risposte ad alcune delle domande più comuni relative ad Azure DevTest Labs.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

**Generale**

## <a name="blog-post"></a>Post di blog
Blog del Team di DevTest Labs è stato ritirato a partire da 20 marzo 2019. 

### <a name="where-can-i-track-feature-updates-going-forward"></a>In cui è possibile tenere traccia degli aggiornamenti delle funzionalità in futuro?
In futuro, pubblicheremo gli aggiornamenti delle funzionalità e/o post di blog informativo sul blog di Azure e gli aggiornamenti di Azure. Questi post di blog contiene collegamenti alla documentazione laddove necessario.

Sottoscrivere il [Blog di Azure DevTest Labs](https://azure.microsoft.com/blog/tag/azure-devtest-labs/) e [DevTest Labs Azure Aggiorna](https://azure.microsoft.com/updates/?product=devtest-lab) per rimanere informati sulle nuove funzionalità in DevTest Labs.

### <a name="what-happens-to-the-existing-blog-posts"></a>Cosa accade ai post di blog esistente?
Si sta attualmente lavorando migrazione post di blog esistente (ad eccezione degli aggiornamenti di interruzione) al nostro [documentazione di DevTest Labs](devtest-lab-overview.md). Quando nel blog di MSDN è deprecato, si verrà reindirizzato alla panoramica della documentazione di DevTest Labs. Dopo aver reindirizzato, è possibile cercare l'articolo che si sta cercando il titolo 'Filtro da'. Si noti che è non hanno ancora eseguito la migrazione tutti i post, ma deve essere eseguite dalla fine di questo mese. 


### <a name="where-do-i-see-outage-updates"></a>In cui vengono visualizzati gli aggiornamenti di interruzione di servizio?
Pubblicheremo gli aggiornamenti di interruzione di servizio usando l'handle di Twitter in futuro. Seguici su Twitter per ottenere gli aggiornamenti più recenti su interruzioni ed errori noti.

### <a name="twitter"></a>Twitter 
L'handle di Twitter: [@azlabservices](https://twitter.com/azlabservices)

## <a name="what-if-my-question-isnt-answered-here"></a>Cosa fare se non è disponibile una risposta alla domanda?
Se la domanda non è elencata qui, informare Microsoft e possiamo aiutarti a trovare una risposta.

* Pubblicare una domanda alla fine di questo articolo di domande frequenti. Interagire con il team di Cache di Azure e altri membri della community in merito a questo articolo.
* Per raggiungere un gruppo di destinatari più ampio, pubblicare una domanda nel [forum MSDN di Azure DevTest Labs](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs). Interagire con il team di Azure DevTest Labs e altri membri della community.
* Per richiedere funzionalità, inviare le richieste e le idee al [sito User Voice per Azure DevTest Labs](https://feedback.azure.com/forums/320373-azure-devtest-labs).

## <a name="why-should-i-use-azure-devtest-labs"></a>Perché usare Azure DevTest Labs?
Azure DevTest Labs può far risparmiare tempo e denaro al team. Gli sviluppatori possono creare ambienti personalizzati usando svariate basi diverse. Possono anche usare gli elementi per distribuire e configurare rapidamente le applicazioni. Usando formule e immagini personalizzate, è possibile salvare le macchine virtuali come modelli e riprodurle facilmente nel team. DevTest Labs offre anche diversi criteri configurabili che consentono agli amministratori di lab di ridurre gli sprechi e di gestire gli ambienti di un team. Questi criteri includono l'arresto automatico, la soglia di costo, il numero massimo di macchine virtuali per utente e le dimensioni massime delle macchine virtuali. Per una spiegazione più approfondita di DevTest Labs, vedere la [panoramica](devtest-lab-overview.md) o guardare il [video introduttivo](https://channel9.msdn.com/Blogs/Azure/what-is-azure-devtest-labs).

## <a name="what-does-worry-free-self-service-mean"></a>Cosa significa "self-service senza preoccupazioni"?
Self-service senza preoccupazioni significa che gli sviluppatori e i tester creano i propri ambienti in base alle esigenze. Gli amministratori hanno la sicurezza di sapere che DevTest Labs aiuta a ridurre gli sprechi e controllare i costi. Gli amministratori possono specificare quali sono le dimensioni consentite per le VM e il numero massimo di VM e quando le VM vengono avviate e arrestate. DevTest Labs consente anche di monitorare facilmente i costi e di impostare avvisi per sapere come vengono usate le risorse del lab.

## <a name="how-can-i-use-devtest-labs"></a>Come si usa DevTest Labs?
DevTest Labs è utile quando si richiedono dev o ambienti di test e vuole riprodurli rapidamente o gestirli con criteri di riduzione dei costi.

Ecco alcuni scenari per cui i clienti usano DevTest Labs:

* Gestire ambienti di sviluppo e test da un'unica posizione. Usare criteri per ridurre i costi e creare immagini personalizzate per condividere le build tra i membri del team.
* Sviluppare un'applicazione usando immagini personalizzate per salvare lo stato del disco durante le fasi di sviluppo.
* Tenere traccia dei costi in relazione allo stato.
* Creare ambienti di test di massa per i test di controllo di qualità.
* Usare elementi e formule per configurare e riprodurre facilmente un'applicazione in diversi ambienti.
* Distribuire le macchine virtuali per gli hackathon (attività di sviluppo o test collaborative) e quindi effettuare facilmente il deprovisioning al termine dell'evento.

## <a name="how-am-i-billed-for-devtest-labs"></a>Come avviene la fatturazione di DevTest Labs?
DevTest Labs è un servizio gratuito. La creazione di lab e la configurazione di criteri, modelli ed elementi in DevTest Labs sono gratuite. Si pagano solo le risorse di Azure usate nei lab, ad esempio macchine virtuali, account di archiviazione e reti virtuali. Per altre informazioni sui costi delle risorse dei lab, vedere [Prezzi di Azure DevTest Labs](https://azure.microsoft.com/pricing/details/devtest-lab/).


**Sicurezza**
## <a name="what-are-the-different-security-levels-in-devtest-labs"></a>Quali sono i diversi livelli di sicurezza in DevTest Labs?
L'accesso sicuro è determinato dal [controllo degli accessi in base al ruolo](../role-based-access-control/built-in-roles.md). Per comprendere il funzionamento dell'accesso, è utile conoscere le differenze tra un'autorizzazione, un ruolo e un ambito, come definiti dal controllo degli accessi in base al ruolo.

* **Autorizzazione**: un'autorizzazione è un accesso definito a un'azione specifica. Un'autorizzazione può ad esempio essere l'accesso in lettura a tutte le macchine virtuali.
* **Ruolo**: un ruolo è un set di autorizzazioni che possono essere raggruppate e assegnate a un utente. Un utente con il ruolo di proprietario della sottoscrizione ha ad esempio accesso a tutte le risorse all'interno di una sottoscrizione.
* **Ambito**: un ambito è un livello nella gerarchia di una risorsa di Azure. Un ambito può ad esempio essere un gruppo di risorse, un singolo lab oppure l'intera sottoscrizione.

Nell'ambito di DevTest Labs, ci sono due tipi di ruoli che definiscono le autorizzazioni utente:

* **Proprietario del lab**: un proprietario del lab ha accesso a tutte le risorse all'interno dello stesso. Un proprietario del lab può modificare i criteri, leggere e scrivere in qualsiasi macchina virtuale, modificare la rete virtuale e così via.
* **Utente del lab**: un utente del lab può visualizzare tutte le risorse del lab, ad esempio macchine virtuali, criteri e reti virtuali. Un utente del lab non può tuttavia modificare i criteri di qualsiasi macchina virtuale creata da altri utenti. 

È anche possibile creare ruoli personalizzati in DevTest Labs. Per informazioni su come creare ruoli personalizzati in DevTest Labs, vedere [Concedere le autorizzazioni utente per specifici criteri di lab](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Poiché gli ambiti sono gerarchici, quando un utente ha le autorizzazioni per un determinato ambito, gli vengono automaticamente concesse tali autorizzazioni per ogni ambito di livello inferiore. Se ad esempio a un utente è assegnato il ruolo di proprietario della sottoscrizione, l'utente ha accesso a tutte le risorse in una sottoscrizione. Queste risorse includono tutte le macchine virtuali, tutte le reti virtuali e tutti i lab. Il proprietario di una sottoscrizione eredita automaticamente il ruolo di proprietario del lab, ma non il contrario. Il proprietario di un lab ha accesso a un lab, che è un ambito più basso del livello della sottoscrizione. Il proprietario di un lab non può quindi vedere le macchine virtuali, le reti virtuali o qualsiasi altra risorsa esterna al lab.

## <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Come si crea un ruolo per consentire agli utenti di eseguire un'attività specifica?
Per un articolo completo su come creare ruoli personalizzati e assegnare le autorizzazioni a un ruolo, vedere [Concedere le autorizzazioni utente per specifici criteri di lab](devtest-lab-grant-user-permissions-to-specific-lab-policies.md). Ecco un esempio di script che crea il ruolo di utente avanzato di DevTest Labs (*DevTest Labs Advanced User*) che ha l'autorizzazione per avviare e arrestare tutte le macchine virtuali del lab:

    $policyRoleDef = Get-AzRoleDefinition "DevTest Labs User"
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*')
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "DevTest Labs Advanced User"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action")
    $policyRoleDef = New-AzRoleDefinition -Role $policyRoleDef  


**Integrazione continua/distribuzione continua e automazione**
## <a name="does-devtest-labs-integrate-with-my-cicd-toolchain"></a>Azure DevTest Labs si integra con la toolchain di integrazione continua/distribuzione continua?
Se si usa Azure DevOps, è disponibile un'[estensione DevTest Labs Tasks](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) che consente di automatizzare la pipeline di rilascio in DevTest Labs. Ecco alcune delle attività che è possibile eseguire con questa estensione:

* Creare e distribuire automaticamente una macchina virtuale. È anche possibile configurare la macchina virtuale con la build più recente usando le attività di Azure DevOps Services in PowerShell o Copia dei file di Azure.
* Acquisire automaticamente lo stato di una macchina virtuale dopo il test per riprodurre un bug nella stessa macchina virtuale per ulteriori indagini.
* Eliminare la macchina virtuale alla fine della pipeline di rilascio quando non è più necessaria.

I post di blog seguenti forniscono indicazioni e informazioni sull'uso dell'estensione di Azure DevOps Services:

* [DevTest Labs and the Azure DevOps extension](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/) (DevTest Labs e l'estensione Azure DevOps)
* [Deploy a new VM in an existing DevTest Labs lab from Azure DevOps Services](https://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS) (Distribuire una nuova macchina virtuale in un lab di DevTest Labs esistente da Azure DevOps Services)
* [Using Azure DevOps Services release management for continuous deployments to DevTest Labs](https://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs) (Uso della gestione del rilascio di Azure DevOps Services per distribuzioni continue in DevTest Labs)

Per altre toolchain di integrazione continua/recapito continuo è possibile ottenere gli stessi scenari distribuendo i [modelli di Azure Resource Manager](https://aka.ms/dtlquickstarttemplate) usando i [cmdlet di Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md) e [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). È anche possibile usare le [API REST per DevTest Labs](https://aka.ms/dtlrestapis) per l'integrazione con la toolchain.  


**Macchine virtuali**
## <a name="why-cant-i-see-vms-on-the-virtual-machines-page-that-i-see-in-devtest-labs"></a>Perché non riesco a visualizzare VM nella pagina macchine virtuali che vengono visualizzate in DevTest Labs?
Quando si crea una macchina virtuale in DevTest Labs, si ottiene l'autorizzazione ad accedervi. È possibile visualizzare la macchina virtuale sia nella pagina di laboratori e scegliere il **macchine virtuali** pagina. Gli utenti assegnati al ruolo utente lab di DevTest Labs possono visualizzare tutte le macchine virtuali che sono state create nel lab del lab **tutte le macchine virtuali** pagina. Agli utenti con ruolo utente lab di DevTest Labs non viene tuttavia concesso automaticamente l'accesso in lettura alle risorse macchine virtuali create da altri. Pertanto, tali macchine virtuali non vengono visualizzati nei **macchine virtuali** pagina.

## <a name="what-is-the-difference-between-a-custom-image-and-a-formula"></a>Qual è la differenza tra un'immagine personalizzata e una formula?
Un'immagine personalizzata è un disco rigido virtuale. Una formula è un'immagine che è possibile configurare con impostazioni aggiuntive e quindi salvare e riprodurre. Un'immagine personalizzata può essere più adatta per creare rapidamente più ambienti usando la stessa immagine di base non modificabile. Una formula può essere migliore se si vuole riprodurre la configurazione della macchina virtuale con i bit più recenti, come parte di una rete virtuale o di una subnet o come macchina virtuale con dimensioni specifiche. Per una spiegazione più approfondita, vedere [Confronto tra immagini personalizzate e formule in DevTest Labs](devtest-lab-comparing-vm-base-image-types.md).

## <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Come si creano più VM dallo stesso modello contemporaneamente?
Ci sono due opzioni per creare simultaneamente più macchine virtuali dallo stesso modello:
* È possibile usare l'[estensione Azure DevTest Labs Tasks](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks). 
* È possibile [generare un modello di Resource Manager](devtest-lab-add-vm.md#save-azure-resource-manager-template) durante la creazione di una macchina virtuale e [distribuire il modello di Resource Manager da Windows PowerShell](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="how-do-i-move-my-existing-azure-vms-into-my-devtest-labs-lab"></a>Come si spostano le macchine virtuali di Azure esistenti nel lab di DevTest Labs?
Per copiare le macchine virtuali esistenti in DevTest Labs:

1. Copiare il file VHD della macchina virtuale esistente usando uno script di PowerShell:
   * Gestione risorse: [CopyRmVHDFromVMToLab.ps1](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyRmVHDFromVMToLab.ps1)
   * Modello di distribuzione classica: [CopyClassicVHDFromVMToLab.ps1](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyClassicVHDFromVMToLab.ps1)
2. [Creare l'immagine personalizzata](devtest-lab-create-template.md) nel lab di DevTest Labs.
3. Creare una macchina virtuale nel lab dall'immagine personalizzata.

## <a name="can-i-attach-multiple-disks-to-my-vms"></a>È possibile collegare più dischi alle VM?
Sì, è possibile collegare più dischi alle macchine virtuali.  

## <a name="if-i-want-to-use-a-windows-os-image-for-my-testing-do-i-have-to-purchase-an-msdn-subscription"></a>Per usare un'immagine del sistema operativo per le operazioni di test è necessario acquistare un abbonamento a MSDN?
Per usare immagini del sistema operativo client Windows (Windows 7 o versioni successive) per attività di sviluppo o test in Azure, è necessario eseguire una delle operazioni seguenti:

- [Acquistare un abbonamento a MSDN](https://www.visualstudio.com/products/how-to-buy-vs).
- Se si ha un contratto Enterprise Agreement, creare una sottoscrizione di Azure con l'[offerta Sviluppo/test Enterprise](https://azure.microsoft.com/offers/ms-azr-0148p).

Per altre informazioni sui crediti Azure per ogni offerta MSDN, vedere [Credito Azure mensile per sottoscrittori di Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

## <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Come si automatizza il processo di caricamento dei file VHD per creare immagini personalizzate?
Per automatizzare il processo di caricamento dei file VHD per creare immagini personalizzate, sono disponibili due opzioni:

* Usare [AzCopy](../storage/common/storage-use-azcopy.md#upload-blobs-to-blob-storage) per copiare o caricare i file VHD nell'account di archiviazione associato al lab.
* Usare [Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md). Storage Explorer è un'app autonoma che è possibile eseguire in Windows, OSX e Linux.   

Per trovare l'account di archiviazione di destinazione associato al lab:

1. Accedere al [portale di Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Nel menu di sinistra selezionare **Gruppi di risorse**.
3. Trovare e selezionare il gruppo di risorse associato al lab.
4. In **Panoramica** selezionare uno degli account di archiviazione.
5. Selezionare **BLOB**.
6. Cercare i caricamenti nell'elenco. Se non ce ne sono, tornare al passaggio 4 e provare con un altro account di archiviazione.
7. Usare l'**URL** come destinazione nel comando AzCopy.

## <a name="how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Come è possibile automatizzare il processo di eliminazione di tutte le macchine virtuali nel lab?
È possibile eliminare le macchine virtuali dal lab tramite il portale di Azure. È anche possibile eliminare tutte le macchine virtuali del lab usando uno script di PowerShell. Nell'esempio seguente modificare i valori dei parametri sotto il commento **Values to change** (Valori da modificare). È possibile recuperare i valori `subscriptionId`, `labResourceGroup` e `labName` dal pannello del lab nel portale di Azure.

    # Delete all the VMs in a lab.

    # Values to change:
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Sign in to your Azure account.
    Connect-AzAccount

    # Select the Azure subscription that has the lab. This step is optional
    # if you have only one subscription.
    Select-AzSubscription -SubscriptionId $subscriptionId

    # Get the lab that has the VMs that you want to delete.
    $lab = Get-AzResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)

    # Get the VMs from that lab.
    $labVMs = Get-AzResource | Where-Object {
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.Name -like "$($lab.Name)/*"}

    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzResource -ResourceId $labVM.ResourceId -Force
    }

**Elementi**
## <a name="what-are-artifacts"></a>Cosa sono gli elementi?
Gli elementi sono componenti personalizzabili che è possibile usare per distribuire i bit più recenti o gli strumenti di sviluppo in una macchina virtuale. Collegare gli elementi alla macchina virtuale quando si crea la macchina virtuale. Dopo il provisioning della macchina virtuale, gli elementi vengono usati per distribuire e configurare la macchina virtuale. Svariati elementi preesistenti sono disponibili nel nostro [repository GitHub pubblico](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts). È anche possibile [creare elementi personalizzati](devtest-lab-artifact-author.md).


**Configurazione del lab**
## <a name="how-do-i-create-a-lab-from-a-resource-manager-template"></a>Come si crea un lab da un modello di Resource Manager?
È disponibile un [repository GitHub di modelli di Azure Resource Manager per lab](https://aka.ms/dtlquickstarttemplate) che è possibile distribuire così come sono oppure modificare per creare modelli personalizzati per i propri lab. Ogni modello ha un collegamento per distribuire il lab così com'è nella sottoscrizione di Azure. In alternativa, è possibile personalizzare il modello ed [eseguire la distribuzione con PowerShell o l'interfaccia della riga di comando di Azure](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Perché le macchine virtuali vengono create in gruppi di risorse diversi con nomi arbitrari? È possibile rinominare o modificare questi gruppi di risorse?
I gruppi di risorse vengono creati in questo modo per consentire a DevTest Labs di gestire le autorizzazioni utente e di accedere alle macchine virtuali. Anche se è possibile spostare una macchina virtuale in un altro gruppo di risorse e usare il nome desiderato, è consigliabile non apportare modifiche ai gruppi di risorse. Stiamo lavorando per migliorare questa esperienza e garantire maggiore flessibilità.   

## <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Quanti lab è possibile creare nella stessa sottoscrizione?
Non c'è un limite specifico al numero di lab che è possibile creare per ogni sottoscrizione. È tuttavia previsto un limite per la quantità di risorse usate per ogni sottoscrizione. Sono disponibili informazioni su [limiti e quote per le sottoscrizioni di Azure](../azure-subscription-service-limits.md) e su [come aumentare questi limiti](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-many-vms-can-i-create-per-lab"></a>Quante VM è possibile creare per ogni lab?
Non esiste un limite specifico al numero di VM che possono essere create per ogni lab. È tuttavia previsto un limite per le risorse (core delle macchine virtuali, indirizzi IP pubblici e così via) usate per ogni sottoscrizione. Sono disponibili informazioni su [limiti e quote per le sottoscrizioni di Azure](../azure-subscription-service-limits.md) e su [come aumentare questi limiti](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Come è possibile condividere un collegamento diretto al mio lab?

1. Passare al lab nel portale di Azure.
2. Copiare l'URL del lab dal browser e quindi condividerlo con gli utenti del lab.

> [!NOTE]
> Se un utente del lab è un utente esterno con un [account Microsoft](#what-is-a-microsoft-account), ma che non è membro dell'istanza di Active Directory dell'organizzazione, potrebbe venire visualizzato un messaggio di errore quando l'utente cerca di accedere al collegamento condiviso. In questo caso, chiedere all'utente di selezionare prima di tutto il proprio nome nell'angolo superiore destro del portale di Azure. Quindi, nella sezione **Directory** del menu, l'utente può selezionare la directory in cui si trova il lab.
>
>

## <a name="what-is-a-microsoft-account"></a>Che cos'è un account Microsoft?
Un account Microsoft è l'account che si usa per quasi tutte le attività eseguite con servizi e dispositivi Microsoft. È composto da un indirizzo e-mail e una password usati per accedere a Skype, Outlook.com, OneDrive, Windows Phone e Xbox LIVE. Grazie a questo singolo account, file, foto, contatti e impostazioni possono seguire l'utente su qualsiasi dispositivo.

> [!NOTE]
> In precedenza, l'account Microsoft era detto *Windows Live ID*.
>
>


**Risoluzione dei problemi**
## <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Si è verificato un errore per l'elemento durante la creazione della VM. Come si risolve il problema?
Per informazioni su come ottenere i log relativi all'elemento che presenta l'errore, vedere [Diagnosticare errori di elementi in DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md).

## <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Perché la rete virtuale esistente non viene salvata correttamente?
È possibile che il nome della rete virtuale contenga dei punti. In tal caso, provare a rimuovere i punti o a sostituirli con trattini. Provare quindi a salvare di nuovo la rete virtuale.

## <a name="why-do-i-get-a-parent-resource-not-found-error-when-i-provision-a-vm-from-powershell"></a>Perché quando si effettua il provisioning di una macchina virtuale da PowerShell viene visualizzato un errore che indica che la risorsa padre non è stata trovata?
Quando una risorsa è l'elemento padre di un'altra risorsa, deve essere già presente prima di creare la risorsa figlio. Se la risorsa padre non esiste, viene visualizzato il messaggio **ParentResourceNotFound**. Se non si specifica una dipendenza dalla risorsa padre, la risorsa figlio può essere distribuita prima dell'elemento padre.

Le macchine virtuali sono risorse figlio di un lab in un gruppo di risorse. Quando si usano modelli di Resource Manager per distribuire macchine virtuali tramite PowerShell, il nome del gruppo di risorse specificato nello script di PowerShell deve coincidere con quello del gruppo di risorse del lab. Per altre informazioni, vedere [Risolvere errori comuni durante la distribuzione di risorse in Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-common-deployment-errors).

## <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>Se la distribuzione di una macchina virtuale ha esito negativo, dove è possibile trovare maggiori informazioni sul tipo di errore?
Gli errori di distribuzione delle macchine virtuali vengono acquisiti nei log attività. È possibile trovare i log attività delle VM nel lab **log di controllo** o **diagnostica della macchina virtuale** nel menu delle risorse nella pagina macchine Virtuali del lab (la pagina viene visualizzata dopo aver selezionato la macchina virtuale dal **personali virtuali le macchine** elenco).

In alcuni casi, l'errore di distribuzione si verifica prima dell'inizio della distribuzione della macchina virtuale. Questo succede, ad esempio, quando viene superato il limite della sottoscrizione per una risorsa creata con la macchina virtuale. In questo caso, i dettagli dell'errore vengono acquisiti nei log attività a livello di lab. Tali log attività si trovano nella parte inferiore delle impostazioni **Configurazione e criteri**. Per altre informazioni sull'utilizzo dei log attività in Azure, vedere [Visualizzare i log attività per controllare le azioni sulle risorse](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

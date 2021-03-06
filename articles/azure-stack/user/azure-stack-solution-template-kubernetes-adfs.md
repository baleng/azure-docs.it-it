---
title: Distribuzione di Kubernetes in Azure Stack tramite Active Directory Federated Services (ADFS) | Microsoft Docs
description: Informazioni su come distribuire Kubernetes in Azure Stack tramite Active Directory Federated Services (ADFS).
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/11/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 02/11/2019
ms.openlocfilehash: bdb46a5e11b26f04d008160fa5a782050211b56e
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484374"
---
# <a name="deploy-kubernetes-to-azure-stack-using-active-directory-federated-services"></a>Distribuzione di Kubernetes in Azure Stack tramite Active Directory Federated Services

*Si applica a: Azure Stack Development Kit e i sistemi integrati di Azure Stack*

> [!Note]  
> Kubernetes in Azure Stack è disponibile in anteprima. Scenario disconnesso di Azure Stack non è attualmente supportato per l'anteprima.

È possibile seguire i passaggi descritti in questo articolo per distribuire e configurare le risorse di Kubernetes. Usare questi passaggi quando Active Directory Federated Services (ADFS) è il servizio di gestione di identità.

## <a name="prerequisites"></a>Prerequisiti 

Per iniziare, assicurarsi che si abbia le autorizzazioni appropriate e che Azure Stack è pronto.

1. Generare una pubblica e privata coppia di chiavi SSH per accedere alla VM Linux in Azure Stack. È necessaria la chiave pubblica quando si crea il cluster.

    Per istruzioni su come generare una chiave, vedere [generazione di chiavi SSH](https://github.com/msazurestackworkloads/acs-engine/blob/master/docs/ssh.md#ssh-key-generation).

1. Verificare che si abbia una sottoscrizione valida nel portale tenant di Azure Stack e di avere sufficiente IP pubblico indirizzi disponibili per aggiungere nuove applicazioni.

    Il cluster non può essere distribuito in Azure Stack **amministratore** sottoscrizione. È necessario usare una **utente** sottoscrizione. 

1. È necessario il servizio dell'insieme di credenziali delle chiavi nella sottoscrizione di Azure Stack.

1. È necessario il Kubernetes Cluster nel marketplace. 

Se non sono presenti il servizio dell'insieme di credenziali delle chiavi e l'elemento del marketplace Kubernetes Cluster, rivolgersi all'amministratore di Azure Stack.

## <a name="create-a-service-principal"></a>Creare un'entità servizio

È necessario rivolgersi all'amministratore di Azure Stack per configurare l'entità servizio quando si usa AD FS come soluzione di identità. L'entità servizio consente all'applicazione di accedere alle risorse di Azure Stack.

1. L'amministratore di Azure Stack consente di accedere con un certificato e le informazioni per l'entità servizio.

   - Le informazioni dell'entità servizio dovrebbero essere simile:

     ```Text  
       ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
       ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
       Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
       ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
       PSComputerName        : azs-ercs01
       RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
     ```

   - Il certificato sarà un file con estensione `.pfx`. Si archivierà il certificato in un Key Vault come chiave privata.

2. Assegnare la nuova entità servizio di un ruolo di collaboratore per la sottoscrizione. Per istruzioni, vedere [assegnare un ruolo](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals).

3. Creare un insieme di credenziali delle chiavi per archiviare il certificato per la distribuzione. Usare gli script di PowerShell seguenti anziché il portale.

   - Sono necessarie le seguenti informazioni:

       | Valore | DESCRIZIONE |
       | ---   | ---         |
       | Endpoint di Azure Resource Manager | Microsoft Azure Resource Manager è un framework di gestione che consente agli amministratori di distribuire, gestire e monitorare le risorse di Azure. Azure Resource Manager può gestire queste attività come un gruppo, anziché singolarmente e in un'unica operazione.<br>L'endpoint in Azure Stack Development Kit (ASDK) è: `https://management.local.azurestack.external/`<br>L'endpoint in sistemi integrati è: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/` |
       | L'ID sottoscrizione | Il [ID sottoscrizione](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview#subscriptions) è la modalità di accesso offerte in Azure Stack. |
       | Il nome utente | Usare semplicemente il nome dell'utente anziché il nome di dominio e nome utente, ad esempio `username` invece di `azurestack\username`. |
       | Nome del gruppo di risorse  | Il nome di un nuovo gruppo di risorse o selezionare un gruppo di risorse. Il nome della risorsa deve essere alfanumerico e lettere minuscole. |
       | Nome del key Vault | Nome dell'insieme di credenziali.<br> Modello Regex: `^[a-zA-Z0-9-]{3,24}$` |
       | Località del gruppo di risorse | Il percorso del gruppo di risorse. Questa è l'area che scelta per l'installazione di Azure Stack. |

   - Aprire PowerShell con privilegi elevati, e [connettersi ad Azure Stack](azure-stack-powershell-configure-user.md#connect-with-ad-fs). Eseguire lo script seguente con i parametri aggiornati ai valori di:

   ```powershell  
       $armEndpoint="<Azure Resource Manager Endpoint>"
       $subscriptionId="<Your Subscription ID>"
       $username="<your user name >"
       $resource_group_name = "<the resource group name >"
       $key_vault_name = "<keyvault name>"
       $resource_group_location="<resource group location>"
        
       # Login Azure Stack Environment
       Add-AzureRmEnvironment -ARMEndpoint $armEndpoint -Name t
       $mycreds = Get-Credential
       Login-AzureRmAccount -Credential $mycreds -Environment t -Subscription $subscriptionId
        
       # Create new Resource group and key vault
       New-AzureRmResourceGroup -Name $resource_group_name -Location $resource_group_location -Force
        
       # Note, Do not omit -EnabledForTemplateDeployment flag
       New-AzureRmKeyVault -VaultName $key_vault_name -ResourceGroupName $resource_group_name -Location $resource_group_location -EnabledForTemplateDeployment
        
       # Obtain the security identifier(SID) of the active directory user
       $adUser = Get-ADUser -Filter "Name -eq '$username'" -Credential $mycreds
       $objectSID = $adUser.SID.Value
       # Set the key vault access policy
       Set-AzureRmKeyVaultAccessPolicy -VaultName $key_vault_name -ResourceGroupName $resource_group_name -ObjectId $objectSID -BypassObjectIdValidation -PermissionsToKeys all -PermissionsToSecrets all
     ```

4. Caricare il certificato all'insieme di credenziali chiave.

   - Sono necessarie le seguenti informazioni:

       | Valore | DESCRIZIONE |
       | ---   | ---         |
       | Percorso certificato | Il nome di dominio completo o percorso del file per il certificato. |
       | Password certificato | La password del certificato. |
       | Nome del segreto | Il nome del segreto usato per fare riferimento al certificato archiviato nell'insieme di credenziali. |
       | Nome del key vault | Il nome dell'insieme di credenziali chiave creata nel passaggio precedente. |
       | Endpoint di Azure Resource Manager | L'endpoint in Azure Stack Development Kit (ASDK) è: `https://management.local.azurestack.external/`<br>L'endpoint in sistemi integrati è: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/` |
       | L'ID sottoscrizione | Il [ID sottoscrizione](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview#subscriptions) è la modalità di accesso offerte in Azure Stack. |

   - Aprire PowerShell con privilegi elevati, e [connettersi ad Azure Stack](azure-stack-powershell-configure-user.md#connect-with-ad-fs). Eseguire lo script seguente con i parametri aggiornati ai valori di:

    ```powershell
        
     # upload the pfx to key vault
     $tempPFXFilePath = "<certificate path>"
     $password = "<certificate password>"
     $keyVaultSecretName = "<secret name>"
     $keyVaultName = "<key vault name>"
     $armEndpoint="<Azure Resource Manager Endpoint>"
     $subscriptionId="<Your Subscription ID>"
     # Login Azure Stack Environment
     Add-AzureRmEnvironment -ARMEndpoint $armEndpoint -Name t
     $mycreds = Get-Credential
     Login-AzureRmAccount -Credential $mycreds -Environment t -Subscription $subscriptionId
    
     $certContentInBytes = [io.file]::ReadAllBytes($tempPFXFilePath)
     $pfxAsBase64EncodedString = [System.Convert]::ToBase64String($certContentInBytes)
     $jsonObject = @"
     {
     "data": "$pfxAsBase64EncodedString",
     "dataType" :"pfx",
     "password": "$password"
     }
     "@
     $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
     $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
     $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
     $keyVaultSecret = Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret 
     ```

## <a name="deploy-kubernetes"></a>Distribuzione di Kubernetes

1. Aprire il [portale di Azure Stack](https://portal.local.azurestack.external).

1. Selezionare **+ crea una risorsa** > **calcolo** > **Kubernetes Cluster**. Fare clic su **Create**(Crea).

    ![Distribuisci modello di soluzione](media/azure-stack-solution-template-kubernetes-deploy/01_kub_market_item.png)

### <a name="1-basics"></a>1. Nozioni di base

1. Selezionare **nozioni di base** nel creare un Cluster Kubernetes.

    ![Distribuisci modello di soluzione](media/azure-stack-solution-template-kubernetes-deploy/02_kub_config_basic.png)

1. Selezionare i **sottoscrizione** ID.

1. Immettere il nome del nuovo gruppo di risorse o selezionare un gruppo di risorse esistente. Il nome della risorsa deve essere alfanumerico e lettere minuscole.

1. Selezionare il **posizione** del gruppo di risorse. Questa è l'area che scelta per l'installazione di Azure Stack.

### <a name="2-kubernetes-cluster-settings"></a>2. Impostazioni del Cluster Kubernetes

1. Selezionare **le impostazioni del Cluster Kubernetes** nel creare un Cluster Kubernetes.

    ![Distribuisci modello di soluzione](media/azure-stack-solution-template-kubernetes-deploy/03_kub_config_settings-adfs.png)

1. Immettere il **nome utente dell'amministratore della macchina virtuale Linux**. Nome utente per le macchine virtuali di Linux che fanno parte del cluster Kubernetes e DVM.

1. Immettere il **chiave pubblica SSH** utilizzati per l'autorizzazione a tutti i computer Linux creato come parte del cluster Kubernetes e DVM.

1. Immettere il **Master del prefisso DNS del profilo** che è univoco nell'area. Deve essere un nome univoco con area, ad esempio `k8s-12345`. Provare a scelta lo stesso del gruppo di risorse nome ritiene più pratica.

    > [!Note]  
    > Per ogni cluster, usare un prefisso DNS del profilo master nuovi ed esclusivi.

1. Selezionare il **Kubernetes Master Pool profilo conteggio**. Il conteggio contiene il numero di nodi nel pool master. Può esistere da 1 a 7. Questo valore deve essere un numero dispari.

1. Selezionare **VMSize la VM master Kubernetes**.

1. Selezionare il **numero profilo del Pool di nodi Kubernetes**. Il conteggio contiene il numero di agenti nel cluster. 

1. Selezionare il **profilo di archiviazione**. È possibile scegliere **Blob di disco** oppure **disco gestito**. Specifica le macchine virtuali del nodo dimensioni di macchina virtuale di Kubernetes. 

1. Selezionare **ADFS** per il **sistema di identità di Azure Stack** per l'installazione di Azure Stack.

1. Immettere il **entità servizio ClientId** viene utilizzato dal provider di cloud di Azure in Kubernetes. L'ID Client è stato identificato come l'ID dell'applicazione quando l'amministratore di Azure Stack è creata l'entità servizio.

1. Immettere il **gruppo di risorse di Key Vault** che origina l'insieme di credenziali delle chiavi che contiene il certificato.

1. Immettere il **nome del Key Vault** il nome dell'insieme di credenziali chiave che contiene il certificato come segreto. 

1. Immettere il **segreto di Key Vault**. Il nome del segreto fa riferimento il certificato.

1. Immettere il **versione del Provider Cloud di Azure in Kubernetes**. Si tratta della versione per il provider di Azure in Kubernetes. Azure Stack rilascia una compilazione Kubernetes personalizzata per ogni versione di Azure Stack.

### <a name="3-summary"></a>3. Summary

1. Selezionare riepilogo. Il pannello visualizza un messaggio di convalida per le impostazioni delle configurazioni di Kubernetes Cluster.

    ![Distribuisci modello di soluzione](media/azure-stack-solution-template-kubernetes-deploy/04_preview.png)

2. Verificare le impostazioni.

3. Selezionare **OK** per distribuire il cluster.

> [!TIP]  
>  Se hai domande sulla distribuzione, è possibile pubblicare una domanda o vedere se un utente ha già risposto alla domanda nel [Forum di Azure Stack](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). 

## <a name="next-steps"></a>Passaggi successivi

[Connettersi al cluster](azure-stack-solution-template-kubernetes-deploy.md#connect-to-your-cluster)

[Abilitare il Dashboard di Kubernetes](azure-stack-solution-template-kubernetes-dashboard.md)
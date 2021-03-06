---
title: Installare PowerShell per Azure Stack | Microsoft Docs
description: Informazioni su come installare PowerShell per Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 02/08/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: 7e631281405b173405f28c134432e870c757b3da
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648403"
---
# <a name="install-powershell-for-azure-stack"></a>Installare PowerShell per Azure Stack

*Si applica a: Azure Stack Development Kit e i sistemi integrati di Azure Stack*

Per usare il cloud, è necessario installare i moduli di PowerShell compatibile con Azure Stack. Compatibilità viene abilitata tramite una funzionalità denominata *profili API*.

I profili delle API forniscono un modo per gestire le differenze di versione tra Azure e Azure Stack. Un profilo di versione API è un set di moduli di PowerShell per Azure Resource Manager con le versioni API specifiche. Ogni piattaforma di cloud ha un set di profili di versione API supportati. Ad esempio, Azure Stack supporta una versione del profilo specifico, ad esempio **2018-03-01-hybrid**. Quando si installa un profilo, vengono installati i moduli di PowerShell per Azure Resource Manager che corrispondono al profilo specificato.

È possibile installare Azure Stack i moduli di PowerShell compatibili in Internet connesso, parzialmente connessi o scenari disconnessi. Questo articolo illustra le istruzioni dettagliate per installare PowerShell per Azure Stack per questi scenari.

## <a name="1-verify-your-prerequisites"></a>1. Verificare i prerequisiti

Prima iniziare a usare Azure Stack e PowerShell, sono necessari i prerequisiti seguenti:

- **PowerShell versione 5.0** per controllare la versione in uso, eseguire **$PSVersionTable.PSVersion** e confrontare le **principali** versione. Se PowerShell 5.0 insufficienti, seguire le [installazione di Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).

  > [!Note]
  > PowerShell 5.0 richiede un computer Windows.

- **Eseguire Powershell in un prompt dei comandi con privilegi elevati**.
  È necessario eseguire PowerShell con privilegi amministrativi.

- **L'accesso a PowerShell Gallery** è necessario l'accesso per il [PowerShell Gallery](https://www.powershellgallery.com). La raccolta è il repository centrale per i contenuti PowerShell. Il **PowerShellGet** modulo contiene i cmdlet per l'individuazione, installazione, aggiornamento e la pubblicazione di elementi PowerShell quali moduli, risorse DSC, capacità del ruolo e script di PowerShell Gallery e altri private repository. Se si usa PowerShell in uno scenario disconnesso, è necessario recuperare risorse da un computer con una connessione a Internet e archiviarle in un percorso accessibile nel computer disconnesso.

## <a name="2-validate-the-powershell-gallery-accessibility"></a>2. Convalidare l'accessibilità di PowerShell Gallery

Verificare se PSGallery è registrato come un repository.

> [!Note]  
> Questo passaggio richiede l'accesso a Internet.

Aprire un prompt di PowerShell con privilegi elevato ed eseguire i cmdlet seguenti:

```powershell
Import-Module -Name PowerShellGet -ErrorAction Stop
Import-Module -Name PackageManagement -ErrorAction Stop
Get-PSRepository -Name "PSGallery"
```

Se il repository non è registrato, aprire una sessione di PowerShell con privilegi elevata ed eseguire il comando seguente:

```powershell
Register-PsRepository -Default
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```

## <a name="3-uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>3. Disinstallare le versioni esistenti dei moduli di PowerShell per Azure Stack

Prima di installare la versione richiesta, assicurarsi di disinstallare tutti i moduli AzureRM dello Stack di Azure PowerShell installati in precedenza. È possibile disinstallarle usando uno dei due metodi seguenti:

1. Per disinstallare i moduli AzureRM di PowerShell esistenti, chiudere tutte le sessioni di PowerShell attive ed eseguire i cmdlet seguenti:

    ```powershell
    Get-Module -Name Azs.* -ListAvailable | Uninstall-Module -Force -Verbose
    Get-Module -Name Azure* -ListAvailable | Uninstall-Module -Force -Verbose
    ```

    Se si riscontra un errore, ad esempio 'il modulo è già in uso',. Chiudere le sessioni di PowerShell che usano i moduli e rieseguire lo script precedente.

2. Eliminare tutte le cartelle che iniziano con `Azure` oppure `Azs.` dalle `C:\Program Files\WindowsPowerShell\Modules` e `C:\Users\{yourusername}\Documents\WindowsPowerShell\Modules` cartelle. Eliminazione di queste cartelle rimuove tutti i moduli di PowerShell esistenti.

## <a name="4-connected-install-powershell-for-azure-stack-with-internet-connectivity"></a>4. Connessione: Installare PowerShell per Azure Stack con connettività Internet

Azure Stack richiede i **2018-03-01-hybrid** profilo versione API per Azure Stack 1808 o versione successiva. Il profilo è disponibile tramite l'installazione di **AzureRM.BootStrapper** modulo. Inoltre, per i moduli AzureRM, è anche necessario installare i moduli di PowerShell di Azure Stack-specifici. Il profilo di versione API e i moduli di PowerShell per Azure Stack è necessario verranno dipendono dalla versione di Azure Stack si è in esecuzione.

Installazione prevede tre passaggi:

1. Installare PowerShell per Azure Stack a seconda della versione di Azure Stack
2. Abilitare le funzionalità di archiviazione aggiuntivi
3. Confermare l'installazione di PowerShell

### <a name="install-azure-stack-powershell"></a>Installare PowerShell per Azure Stack

Eseguire lo script di PowerShell seguente per installare i moduli nella workstation di sviluppo:

- Azure Stack 1901 o versione successivo:

    ```powershell
    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.

    Install-Module AzureRM -RequiredVersion 2.4.0
    Install-Module -Name AzureStack -RequiredVersion 1.7.1
    ```

    > [!Note]  
    > La versione 1.7.1 del modulo di Azure Stack è una versione di modifica di rilievo. Eseguire la migrazione da Azure Stack 1.6.0, vedere la [Guida alla migrazione](https://aka.ms/azspshmigration171).
    > Il modulo AzureRm versione 2.4.0 dotata di una modifica di rilievo per il cmdlet Remove-AzureRmStorageAccount. Questo cmdlet si aspetta - parametro Force per essere specificato per la rimozione dell'account di archiviazione senza conferma.

- Azure Stack 1811:

    ```powershell
    # Install the AzureRM.BootStrapper module. Select Yes when prompted to install NuGet
    Install-Module -Name AzureRM.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    Install-Module -Name AzureStack -RequiredVersion 1.6.0
    ```

- Azure Stack 1810 o versioni precedenti:

    ```powershell
    # Install the AzureRM.BootStrapper module. Select Yes when prompted to install NuGet
    Install-Module -Name AzureRM.BootStrapper

    # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force

    Install-Module -Name AzureStack -RequiredVersion 1.5.0
    ```

> [!Note]  
> Per eseguire l'aggiornamento di Azure PowerShell dal **2017-03-09-profilo** per **2018-03-01-hybrid**, vedere il [Guida alla migrazione](https://github.com/azure/azure-powershell/blob/AzureRM/documentation/migration-guides/Stack/migration-guide.2.3.0.md).

### <a name="enable-additional-storage-features"></a>Abilitare le funzionalità di archiviazione aggiuntivi

Per usare le funzionalità di ulteriore spazio di archiviazione (indicate nella sezione connesso), di download e installare anche i pacchetti seguenti.

```powershell
# Install the Azure.Storage module version 4.5.0
Install-Module -Name Azure.Storage -RequiredVersion 4.5.0 -Force -AllowClobber

# Install the AzureRm.Storage module version 5.0.4
Install-Module -Name AzureRM.Storage -RequiredVersion 5.0.4 -Force -AllowClobber

# Remove incompatible storage module installed by AzureRM.Storage
Uninstall-Module Azure.Storage -RequiredVersion 4.6.1 -Force

# Load the modules explicitly specifying the versions
Import-Module -Name Azure.Storage -RequiredVersion 4.5.0
Import-Module -Name AzureRM.Storage -RequiredVersion 5.0.4
```

### <a name="confirm-the-installation-of-powershell"></a>Confermare l'installazione di PowerShell

Confermare l'installazione eseguendo il comando seguente:

```powershell
Get-Module -Name "Azure*" -ListAvailable
Get-Module -Name "Azs*" -ListAvailable
```

Se l'installazione ha esito positivo, i moduli AzureRM e AzureStack vengono visualizzati nell'output.

## <a name="5-disconnected-install-powershell-without-an-internet-connection"></a>5. Disconnesso: Installare PowerShell senza una connessione Internet

In uno scenario disconnesso, è necessario innanzitutto scaricare i moduli di PowerShell in un computer con connettività Internet e quindi di trasferirle a Azure Stack Development Kit per l'installazione.

Accedere a un computer con connettività Internet e usare gli script seguenti per scaricare i pacchetti di Azure Resource Manager e AzureStack, a seconda della versione di Azure Stack.

Installazione prevede quattro passaggi:

1. Installare PowerShell per Azure Stack per un computer connesso a
2. Abilitare le funzionalità di archiviazione aggiuntivi
3. I pacchetti di PowerShell per la workstation disconnessa del trasporto
4. Confermare l'installazione di PowerShell

### <a name="install-azure-stack-powershell"></a>Installare PowerShell per Azure Stack

- Azure Stack 1901 o versione successivo.

    ```powershell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.4.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.7.1
    ```

    > [!Note]  
    > La versione 1.7.1 del modulo di Azure Stack è una modifica sostanziale. Eseguire la migrazione da AzureStack 1.6.0, vedere la [Guida alla migrazione](https://github.com/Azure/azure-powershell/tree/AzureRM/documentation/migration-guides/Stack).

  - Azure Stack 1811 o versioni precedenti.

    ```powershell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.6.0
    ```

  - Azure Stack 1809 o versioni precedenti.

    ```powershell
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

    $Path = "<Path that is used to save the packages>"
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
    Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
    ```

    > [!NOTE]
    > Nei computer senza una connessione a Internet, è consigliabile eseguire il cmdlet seguente per disabilitare la raccolta dei dati di telemetria. Senza disabilitare la raccolta dei dati di telemetria che si verifichi una riduzione delle prestazioni dei cmdlet. Questa opzione è disponibile solo per i computer senza connessioni internet
    > ```powershell
    > Disable-AzureRmDataCollection
    > ```

### <a name="enable-additional-storage-features"></a>Abilitare le funzionalità di archiviazione aggiuntivi

Per usare le funzionalità di ulteriore spazio di archiviazione (indicate nella sezione connesso), di download e installare anche i pacchetti seguenti.

```powershell
$Path = "<Path that is used to save the packages>"
Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name Azure.Storage -Path $Path -Force -RequiredVersion 4.5.0
Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRm.Storage -Path $Path -Force -RequiredVersion 5.0.4
```

### <a name="add-your-packages-to-your-workstation"></a>Aggiungere i pacchetti nella workstation

1. Copiare i pacchetti scaricati in un dispositivo USB.

2. Accedere alla workstation disconnessa e copiare i pacchetti dal dispositivo USB in un percorso nella workstation.

3. A questo punto registrare questo percorso come il repository predefinito e installare i moduli AzureRM e AzureStack da questo repository:

   ```powershell
   #requires -Version 5
   #requires -RunAsAdministrator
   #requires -Module PowerShellGet
   #requires -Module PackageManagement

   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository -Name $RepoName -SourceLocation $SourceLocation  -InstallationPolicy Trusted

   Install-Module -Name AzureRM -Repository $RepoName

   Install-Module -Name AzureStack -Repository $RepoName
   ```

### <a name="confirm-the-installation-of-powershell"></a>Confermare l'installazione di PowerShell

Confermare l'installazione eseguendo il comando seguente:

```powershell
Get-Module -Name "Azure*" -ListAvailable
Get-Module -Name "Azs*" -ListAvailable
```

## <a name="6-configure-powershell-to-use-a-proxy-server"></a>6. Configurare PowerShell per usare un server proxy

Negli scenari che richiedono un server proxy per accedere a Internet, è innanzitutto necessario configurare PowerShell per l'uso di un server proxy esistente:

1. Aprire un prompt di PowerShell con privilegi elevato.
2. Eseguire i comandi seguenti:

   ```powershell
   #To use Windows credentials for proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

   #Alternatively, to prompt for separate credentials that can be used for #proxy authentication
   [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
   ```

## <a name="next-steps"></a>Passaggi successivi

- [Scaricare strumenti di Azure Stack da GitHub](azure-stack-powershell-download.md)
- [Configurare l'ambiente di PowerShell dell'utente Azure Stack](user/azure-stack-powershell-configure-user.md)
- [Configurare l'ambiente PowerShell dell'operatore Azure Stack](azure-stack-powershell-configure-admin.md)
- [Gestire i profili di versione API in Azure Stack](user/azure-stack-version-profiles.md)

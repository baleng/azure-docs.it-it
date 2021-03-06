---
title: Creare e pubblicare un elemento del Marketplace in Azure Stack | Microsoft Docs
description: Creare e pubblicare un elemento del Marketplace in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2019
ms.author: sethm
ms.reviewer: avishwan
ms.lastreviewed: 01/08/2019
ms.openlocfilehash: 81f06e0f5d5201b902504d8275f356f9a1731065
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/18/2019
ms.locfileid: "58098900"
---
# <a name="create-and-publish-a-marketplace-item"></a>Creare e pubblicare un elemento del Marketplace

*Si applica a: Azure Stack Development Kit e i sistemi integrati di Azure Stack*

## <a name="create-a-marketplace-item"></a>Creare un elemento del Marketplace

1. Scaricare il [dello strumento Azure Gallery Packager](https://www.aka.ms/azurestackmarketplaceitem) e l'elemento del Marketplace Azure Stack di esempio.
2. Aprire l'elemento del Marketplace di esempio e rinominare il **SimpleVMTemplate** cartella. Usare lo stesso nome come l'elemento del Marketplace; ad esempio, **Contoso.TodoList**. Questa cartella contiene:

   ```shell
   /Contoso.TodoList/
   /Contoso.TodoList/Manifest.json
   /Contoso.TodoList/UIDefinition.json
   /Contoso.TodoList/Icons/
   /Contoso.TodoList/Strings/
   /Contoso.TodoList/DeploymentTemplates/
   ```

3. [Creare un modello di Gestione risorse di Azure](../azure-resource-manager/resource-group-authoring-templates.md) o scegliere un modello da GitHub. L'elemento del Marketplace Usa questo modello per creare una risorsa.

    > [!Note]  
    > Mai nel codice eventuali segreti, ad esempio i codici Product Key, password o informazioni personali dei clienti nel modello di Azure Resource Manager. File JSON del modello sono accessibili senza la necessità per l'autenticazione una volta pubblicato in gallery. Tutti i segreti in Store [Key Vault](../azure-resource-manager/resource-manager-keyvault-parameter.md) e chiamarle da all'interno del modello.

4. Per assicurarsi che la risorsa può essere distribuita correttamente, testare il modello con le API di Microsoft Azure Stack.
5. Se il modello si basa su un'immagine di macchina virtuale, seguire le istruzioni per [aggiungere un'immagine di macchina virtuale in Azure Stack](azure-stack-add-vm-image.md).
6. Salvare il modello di Azure Resource Manager nel **/Contoso.TodoList/DeploymentTemplates/** cartella.
7. Scegliere le icone e testo per l'elemento del Marketplace. Aggiungere le icone per il **icone** cartella e aggiungere testo al **risorse** del file nel **stringhe** cartella. Usare la **piccola**, **medium**, **grandi**, e **wide** convenzione di denominazione per le icone. Vedere le [elemento del Marketplace riferimento all'interfaccia utente](#reference-marketplace-item-ui) per una descrizione dettagliata di queste dimensioni.

   > [!NOTE]
   > Tutte le dimensioni delle icone quattro (small, medium, large, wide) sono necessari per compilare correttamente l'elemento del Marketplace.
   >
   >
8. Nel **manifest** file, modificare **nome** per il nome dell'elemento di Marketplace. Modificare anche **server di pubblicazione** al nome o della società.
9. Sotto **artefatti**, modificare **name** e **percorso** per le informazioni corrette per il modello di Azure Resource Manager che è stato incluso:

   ```json
   "artifacts": [
      {
          "name": "Your template name",
          "type": "Template",
          "path": "DeploymentTemplates\\your path",
          "isDefault": true
      }
   ```

10. Sostituire **My Items Marketplace** con un elenco di categorie in cui deve essere visualizzato l'elemento del Marketplace:

    ```json
    "categories":[
    "My Marketplace Items"
    ],
    ```

11. Per eventuali ulteriori modifiche da manifest, fare riferimento a [riferimento: Marketplace elemento manifest](#reference-marketplace-item-manifestjson).

12. Per comprimere le cartelle in un file con estensione azpkg, aprire un prompt dei comandi ed eseguire il comando seguente:

    ```shell
    AzureGalleryPackager.exe package –m <path to manifest.json> -o <output location for the package>
    ```

    > [!NOTE]
    > Il percorso completo al pacchetto di output deve esistere. Ad esempio, se il percorso di output è C:\MarketPlaceItem\yourpackage.azpkg, la cartella C:\MarketPlaceItem deve esistere.
    >
    >

## <a name="publish-a-marketplace-item"></a>Pubblicare un elemento del Marketplace

1. Usare PowerShell o Azure Storage Explorer per caricare l'elemento del Marketplace (con estensione azpkg) in archiviazione Blob di Azure. È possibile caricare in archiviazione di Azure Stack locale o caricare in archiviazione di Azure; ovvero un percorso temporaneo per il pacchetto. Assicurarsi che il blob sia accessibile pubblicamente.
2. Nella macchina virtuale client nell'ambiente di Microsoft Azure Stack, assicurarsi che la sessione di PowerShell sia configurata con le credenziali di amministratore del servizio. È possibile trovare istruzioni su come autenticare PowerShell in Azure Stack [distribuire un modello con PowerShell](user/azure-stack-deploy-template-powershell.md).
3. Quando si usa [PowerShell 1.3.0](azure-stack-powershell-install.md) o versioni successive, è possibile utilizzare il **Add-AzsGalleryItem** cmdlet di PowerShell per pubblicare l'elemento del Marketplace in Azure Stack. Prima di usare PowerShell 1.3.0, usare il cmdlet **Add-AzureRMGalleryitem** al posto di **Add-AzsGalleryItem**. Ad esempio, quando si usa PowerShell 1.3.0 o versioni successive:

   ```powershell
   Add-AzsGalleryItem -GalleryItemUri `
   https://sample.blob.core.windows.net/gallerypackages/Microsoft.SimpleTemplate.1.0.0.azpkg –Verbose
   ```

   | Parametro | DESCRIZIONE |
   | --- | --- |
   | SubscriptionID |ID sottoscrizione amministratore. È possibile recuperarlo tramite PowerShell. Se si preferisce eseguire questa operazione nel portale, passare alla sottoscrizione provider e copiare l'ID sottoscrizione. |
   | GalleryItemUri |URI del BLOB per il pacchetto di raccolta che è già stato caricato in un archivio. |
   | Apiversion |Impostare come **2015-04-01**. |

4. Passare al portale. È ora possibile visualizzare l'elemento del Marketplace nel portale, come un operatore o un utente. Il pacchetto potrebbe richiedere diversi minuti prima della visualizzazione.

5. L'elemento del Marketplace a questo punto è stato salvato per il Marketplace di Azure Stack. È possibile scegliere di eliminarlo dal percorso di archiviazione Blob.
    > [!Caution]  
    > Tutti gli elementi di raccolta predefinito e degli elementi di raccolta personalizzati sono ora accessibili senza autenticazione con gli URL seguenti:  
`https://adminportal.[Region].[external FQDN]:30015/artifact/20161101/[Template Name]/DeploymentTemplates/Template.json`
`https://portal.[Region].[external FQDN]:30015/artifact/20161101/[Template Name]/DeploymentTemplates/Template.json`
`https://systemgallery.blob.[Region].[external FQDN]/dev20161101-microsoft-windowsazure-gallery/[Template Name]/UiDefinition.json`

6. È possibile rimuovere un elemento del Marketplace tramite il **Remove-AzureRMGalleryItem** cmdlet. Ad esempio: 

   ```powershell
   Remove-AzsGalleryItem -Name Microsoft.SimpleTemplate.1.0.0  –Verbose
   ```

   > [!NOTE]
   > L'interfaccia utente di Marketplace può visualizzare un errore dopo la rimozione di un elemento. Per correggere l'errore, fare clic su **impostazioni** nel portale. Quindi, selezionare **Annulla modifiche** sotto **personalizzazione del portale**.
   >
   >

## <a name="reference-marketplace-item-manifestjson"></a>Riferimenti: Manifest di elemento di Marketplace

### <a name="identity-information"></a>Informazioni sull'identità

| NOME | Obbligatorio | Type | Vincoli | DESCRIZIONE |
| --- | --- | --- | --- | --- |
| NOME |X |string |[A-Za-z0-9] + | |
| Editore |X |string |[A-Za-z0-9] + | |
| Versione |X |string |[SemVer v2](https://semver.org/) | |

### <a name="metadata"></a>Metadata

| NOME | Obbligatorio | Type | Vincoli | DESCRIZIONE |
| --- | --- | --- | --- | --- |
| DisplayName |X |string |Consiglio di 80 caratteri |Il portale potrebbe non visualizzare correttamente il nome dell'elemento se è più lungo di 80 caratteri. |
| PublisherDisplayName |X |string |Consiglio di 30 caratteri |Il portale potrebbe non visualizzare correttamente il nome del server di pubblicazione se è più lungo di 30 caratteri. |
| PublisherLegalName |X |string |Numero massimo di 256 caratteri | |
| Summary |X |string |Da 60 a 100 caratteri | |
| LongSummary |X |string |Da 140 a 256 caratteri |Non ancora applicabile in Azure Stack. |
| DESCRIZIONE |X |[HTML](https://auxdocs.azurewebsites.net/en-us/documentation/articles/gallery-metadata#html-sanitization) |500 per 5.000 caratteri | |

### <a name="images"></a>Immagini

Marketplace utilizza le icone seguenti:

| NOME | Larghezza | Altezza: | Note |
| --- | --- | --- | --- |
| Largo |255 px |115 px |È necessario sempre |
| Grande |115 px |115 px |È necessario sempre |
| Media |90 px |90 px |È necessario sempre |
| Piccolo |40 px |40 px |È necessario sempre |
| Schermata |533 px |32 px |Facoltativo |

### <a name="categories"></a>Categorie

Ogni elemento del Marketplace deve essere contrassegnato con una categoria che identifica in cui l'elemento viene visualizzato nel portale dell'interfaccia utente. È possibile scegliere una delle categorie esistenti in Azure Stack (calcolo, dati e archiviazione, e così via) o uno nuovo.

### <a name="links"></a>Collegamenti

Ogni elemento del Marketplace può includere diversi collegamenti a contenuti aggiuntivi. I collegamenti vengono specificati come un elenco di nomi e gli URI:

| NOME | Obbligatorio | Type | Vincoli | DESCRIZIONE |
| --- | --- | --- | --- | --- |
| DisplayName |X |string |Numero massimo di 64 caratteri | |
| Uri |X |URI | | |

### <a name="additional-properties"></a>Proprietà aggiuntive

Oltre ai metadati precedenti, gli autori del Marketplace possono fornire dati di coppia chiave/valore personalizzato nel formato seguente:

| NOME | Obbligatorio | Type | Vincoli | DESCRIZIONE |
| --- | --- | --- | --- | --- |
| DisplayName |X |string |Numero massimo di 25 caratteri | |
| Valore |X |string |Numero massimo di 30 caratteri | |

### <a name="html-sanitization"></a>Purificazione HTML

Per qualsiasi campo che accettano HTML, sono consentiti elementi e gli attributi seguenti:

`h1, h2, h3, h4, h5, p, ol, ul, li, a[target|href], br, strong, em, b, i`

## <a name="reference-marketplace-item-ui"></a>Riferimenti: Elemento del Marketplace dell'interfaccia utente

Di seguito sono riportati le icone e testo per gli elementi del Marketplace come illustrato nel portale di Azure Stack.

### <a name="create-blade"></a>Pannello Crea

![Pannello Crea](media/azure-stack-create-and-publish-marketplace-item/image1.png)

### <a name="marketplace-item-details-blade"></a>Pannello con i dettagli dell'elemento del Marketplace

![Pannello con i dettagli dell'elemento del Marketplace](media/azure-stack-create-and-publish-marketplace-item/image3.png)

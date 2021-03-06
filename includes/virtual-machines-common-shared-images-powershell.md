---
title: File di inclusione
description: File di inclusione
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 12/10/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 91889971e1ab8a9ea8341f6bc57735d973ea0e89
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/18/2019
ms.locfileid: "58115830"
---
## <a name="launch-azure-cloud-shell"></a>Avviare Azure Cloud Shell

Azure Cloud Shell è una shell interattiva gratuita che può essere usata per eseguire la procedura di questo articolo. Include strumenti comuni di Azure preinstallati e configurati per l'uso con l'account. 

Per aprire Cloud Shell, basta selezionare **Prova** nell'angolo superiore destro di un blocco di codice. È anche possibile avviare Cloud Shell in una scheda separata del browser visitando [https://shell.azure.com/powershell](https://shell.azure.com/powershell). Selezionare **Copia** per copiare i blocchi di codice, incollarli in Cloud Shell e premere INVIO per eseguirli.


## <a name="preview-register-the-feature"></a>Anteprima: Registrare la funzionalità

Raccolte di immagini condivise è disponibile in anteprima, ma è necessario registrare la funzionalità prima di poterla usare. Per registrare la funzionalità Raccolte di immagini condivise:

```azurepowershell-interactive
Register-AzProviderFeature `
   -FeatureName GalleryPreview `
   -ProviderNamespace Microsoft.Compute
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="get-the-managed-image"></a>Ottenere l'immagine gestita

È possibile visualizzare un elenco delle immagini disponibili in un gruppo di risorse usando [Get-AzImage](https://docs.microsoft.com/powershell/module/az.compute/get-azimage). Quando si conosce il nome dell'immagine e in quale gruppo di risorse si trova, è possibile usare nuovamente `Get-AzImage` per ottenere l'oggetto immagine e archiviarlo in una variabile da usare in un secondo momento. Questo esempio mostra come ottenere un'immagine denominata *myImage* dal gruppo di risorse "myResourceGroup" che lo assegna alla variabile *$managedImage*. 

```azurepowershell-interactive
$managedImage = Get-AzImage `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
```

## <a name="create-an-image-gallery"></a>Creare un raccolta di immagini 

Una raccolta di immagini è la risorsa principale usata per l'abilitazione della condivisione di immagini. I nomi di raccolta devono essere univoci all'interno della sottoscrizione. Creare una raccolta di immagini usando [New-AzGallery](https://docs.microsoft.com/powershell/module/az.compute/new-azgallery). L'esempio seguente crea una raccolta denominata *myGallery* nel gruppo di risorse *myGalleryRG*.

```azurepowershell-interactive
$resourceGroup = New-AzResourceGroup `
   -Name 'myGalleryRG' `
   -Location 'West Central US'  
$gallery = New-AzGallery `
   -GalleryName 'myGallery' `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -Description 'Shared Image Gallery for my organization'  
```
   
## <a name="create-an-image-definition"></a>Creare una definizione dell'immagine 

Creare la definizione di immagine della raccolta usando [New-AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). In questo esempio, l'immagine della raccolta è denominata *myGalleryImage*.

```azurepowershell-interactive
$galleryImage = New-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $gallery.Location `
   -Name 'myImageDefinition' `
   -OsState generalized `
   -OsType Windows `
   -Publisher 'myPublisher' `
   -Offer 'myOffer' `
   -Sku 'mySKU'
```
### <a name="using-publisher-offer-and-sku"></a>Con server di pubblicazione, offerta e SKU 
Per i clienti che prevedono sull'implementazione di immagini condivise, **in una versione futura**, sarà possibile usare il personale definito **-server di pubblicazione**, **-offrono** e **- Sku** i valori per trovare e specificare una definizione dell'immagine, quindi creare una VM usando l'ultima versione dell'immagine dal corrispondente definizione di immagine. Di seguito, un esempio di tre definizioni di immagini con i relativi valori:

|Definizione delle immagini|Editore|Offerta|Sku|
|---|---|---|---|
|myImage1|myPublisher|myOffer|mySku|
|myImage2|myPublisher|standardOffer|mySku|
|myImage3|Test|standardOffer|testSku|

Questi tre presentano set univoci di valori. È possibile avere versioni delle immagini che condividono uno o due, ma non tutti e tre i valori. **In una versione futura**, sarà possibile combinare questi valori per richiedere la versione più recente di un'immagine specifica. **Il server non funziona nella versione corrente**, ma saranno disponibili in futuro. Al rilascio, usando la sintassi seguente deve essere utilizzato per impostare l'immagine di origine come *myImage1* nella tabella precedente.

```powershell
$vmConfig = Set-AzVMSourceImage `
   -VM $vmConfig `
   -PublisherName myPublisher `
   -Offer myOffer `
   -Skus mySku 
```

Ciò è simile al modo in cui è possibile attualmente specificare utilizzo editore, offerta e lo SKU per [immagini di Azure Marketplace](../articles/virtual-machines/windows/cli-ps-findimage.md) per ottenere la versione più recente di un'immagine del Marketplace. A tal fine,ogni definizione di immagine deve possedere un set univoco di questi valori.  

## <a name="create-an-image-version"></a>Creare una versione di immagine

Creare una versione di immagine da un'immagine gestita usando [New-AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). In questo esempio la versione dell'immagine è *1.0.0* e viene replicata nei datacenter degli *Stati Uniti centrali* e degli *Stati Uniti centro-meridionali*.


```azurepowershell-interactive
$region1 = @{Name='South Central US';ReplicaCount=1}
$region2 = @{Name='West Central US';ReplicaCount=2}
$targetRegions = @($region1,$region2)
$job = $imageVersion = New-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryImageVersionName '1.0.0' `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -TargetRegion $targetRegions  `
   -Source $managedImage.Id.ToString() `
   -PublishingProfileEndOfLifeDate '2020-01-01' `
   -asJob 
```

Può essere necessario un po' di tempo per replicare l'immagine in tutte le aree di destinazione, per questo motivo Microsoft ha creato un processo in modo che sia possibile tenere traccia dello stato di avanzamento. Per visualizzare lo stato di avanzamento del processo, digitare `$job.State`.

```azurepowershell-interactive
$job.State
```


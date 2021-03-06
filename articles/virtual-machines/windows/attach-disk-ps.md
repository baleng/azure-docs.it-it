---
title: Collegare un disco dati a una macchina virtuale Windows in Azure usando PowerShell | Microsoft Docs
description: Come collegare un disco dati nuovo o esistente a una macchina virtuale Windows con PowerShell con il modello di distribuzione Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: a42fec94a23db82192cf05a47080d982a0857056
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729043"
---
# <a name="attach-a-data-disk-to-a-windows-vm-with-powershell"></a>Collegare un disco dati a una macchina virtuale Windows con PowerShell

Questo articolo illustra come collegare dischi nuovi ed esistenti a una macchina virtuale Windows tramite PowerShell. 

Esaminare prima di tutto i suggerimenti seguenti:

* La dimensione della macchina virtuale controlla il numero di dischi dati che è possibile collegare. Per altre informazioni, vedere [Dimensioni delle macchine virtuali in Azure](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Per usare unità SSD Premium, è necessario un [tipo di macchina virtuale abilitato per Archiviazione Premium](sizes-memory.md), ad esempio una macchina virtuale serie DS o GS.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>Aggiungere un disco dati vuoto a una macchina virtuale

Questo esempio illustra come aggiungere un disco dati vuoto a una macchina virtuale esistente.

### <a name="using-managed-disks"></a>Uso di Managed Disks

```azurepowershell-interactive
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk1 = New-AzDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-managed-disks-in-an-availability-zone"></a>Uso di Managed Disks in una zona di disponibilità

Per creare un disco in una zona di disponibilità, usare [New AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azdiskconfig) con il parametro `-Zone`. L'esempio seguente crea un disco nella zona *1*.

```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US 2'
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128 -Zone 1
$dataDisk1 = New-AzDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

### <a name="initialize-the-disk"></a>Inizializzare il disco

Dopo aver aggiunto un disco vuoto, è necessario inizializzarlo. Per inizializzare il disco, è possibile accedere a una macchina virtuale e usare la funzionalità di gestione del disco. Se sono stati abilitati il componente [WinRM](https://docs.microsoft.com/windows/desktop/WinRM/portal) e un certificato nella macchina virtuale al momento della creazione, è possibile usare PowerShell remoto per inizializzare il disco. È anche possibile usare un'estensione di script personalizzata:

```azurepowershell-interactive
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```

Il file di script può contenere il codice per inizializzare i dischi, ad esempio:

```azurepowershell-interactive
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```

## <a name="attach-an-existing-data-disk-to-a-vm"></a>Collegare un disco dati esistente a una macchina virtuale

È possibile collegare un disco gestito esistente a una macchina virtuale come disco dati.

```azurepowershell-interactive
$rgName = "myResourceGroup"
$vmName = "myVM"
$location = "East US" 
$dataDiskName = "myDisk"
$disk = Get-AzDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a>Passaggi successivi

Creare uno [snapshot](snapshot-copy-managed-disk.md).

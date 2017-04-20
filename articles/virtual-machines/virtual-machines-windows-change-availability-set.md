<properties
    pageTitle="Een set van VMs beschikbaarheid wijzigen | Microsoft Azure"
    description="Informatie over het wijzigen van de beschikbaarheid instellen voor de virtuele machines met Azure PowerShell en het implementatiemodel Resource Manager."
    keywords=""
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="drewm"/>



# <a name="change-the-availability-set-for-a-windows-vm"></a>De beschikbaarheid voor een Windows VM instellen wijzigen

De volgende stappen wordt beschreven hoe u de beschikbaarheid van een VM met Azure PowerShell set wijzigen. Een VM kan alleen worden toegevoegd aan een beschikbaarheid instellen wanneer deze wordt gemaakt. Instellen om de beschikbaarheid te wijzigen, moet u de te verwijderen en opnieuw de virtuele machine. 

## <a name="change-the-availability-set-using-powershell"></a>De beschikbaarheid met PowerShell set wijzigen

1. Vastleggen van de volgende belangrijke details van de VM worden gewijzigd.

    Naam van de VM
    
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <Name-of-resource-group> -Name <name-of-VM>
    $vm.Name
    ```
 
    VM-grootte
    
    ```powershell
    $vm.HardwareProfile.VmSize
    ```

    Primaire netwerk netwerkinterface en optionele netwerkinterfaces indien aanwezig op de VM
    
    ```powershell
    $vm.NetworkProfile.NetworkInterfaces[0].Id
    ```

    Profiel van OS schijf

    ```powershell
    $vm.StorageProfile.OsDisk.OsType
    $vm.StorageProfile.OsDisk.Name
    $vm.StorageProfile.OsDisk.Vhd.Uri
    ```

    Profielen voor elke gegevensschijf schijf 
    
    ```powershell
    $vm.StorageProfile.DataDisks[<index>].Lun
    $vm.StorageProfile.DataDisks[<index>].Vhd.Uri
    ```

    VM-extensies geïnstalleerd 
    
    ```powershell
    $vm.Extensions
    ```

2. Verwijder de VM zonder te verwijderen op een van de schijven of de netwerkinterfaces.

    ```powershell
    Remove-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName> 
    ```

3. Maak de beschikbaarheid als deze nog niet bestaat

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName <resourceGroupName> -Name <availabilitySetName> -Location "<location>" 
    ```

4. Maak de VM met behulp van de nieuwe set van beschikbaarheid

    ```powershell
    $vm2 = New-AzureRmVMConfig -VMName <VM-name> -VMSize <vm-size> -AvailabilitySetId <availability-set-id>

    Set-AzureRmVMOSDisk -CreateOption "Attach" -VM <vmConfig> -VhdUri <osDiskURI> -Name <osDiskName> [-Windows | -Linux]

    Add-AzureRmVMNetworkInterface -VM <vmConfig> -Id  <nicId> 

    New-AzureRmVM -ResourceGroupName <resourceGroupName> -Location <location> -VM <vmConfig>
    ``` 

5. Data schijven en extensies toevoegen. Zie [Gegevensschijf koppelen aan VM](virtual-machines-windows-attach-disk-portal.md) en [Uitbreiding configuratie voorbeelden](virtual-machines-windows-extensions-configuration-samples.md)voor meer informatie. Gegevensschijven en extensies kunnen worden toegevoegd aan de VM met PowerShell of Azure CLI.

## <a name="example-script"></a>Voorbeeldscript

Het volgende script wordt een voorbeeld gegeven van het verzamelen van de vereiste gegevens, de oorspronkelijke VM verwijderen en vervolgens opnieuw te maken in een nieuwe set van beschikbaarheid.

```powershell
    #set variables
    $rg = "demo-resource-group"
    $vmName = "demo-vm"
    $newAvailSetName = "demo-as"
    $outFile = "C:\temp\outfile.txt"

    #Get VM Details
    $OriginalVM = get-azurermvm -ResourceGroupName $rg -Name $vmName

    #Output VM details to file
    "VM Name: " | Out-File -FilePath $outFile 
    $OriginalVM.Name | Out-File -FilePath $outFile -Append

    "Extensions: " | Out-File -FilePath $outFile -Append
    $OriginalVM.Extensions | Out-File -FilePath $outFile -Append

    "VMSize: " | Out-File -FilePath $outFile -Append
    $OriginalVM.HardwareProfile.VmSize | Out-File -FilePath $outFile -Append

    "NIC: " | Out-File -FilePath $outFile -Append
    $OriginalVM.NetworkProfile.NetworkInterfaces[0].Id | Out-File -FilePath $outFile -Append

    "OSType: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.OsType | Out-File -FilePath $outFile -Append

    "OS Disk: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.Vhd.Uri | Out-File -FilePath $outFile -Append

    if ($OriginalVM.StorageProfile.DataDisks) {
    "Data Disk(s): " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.DataDisks | Out-File -FilePath $outFile -Append
    }

    #Remove the original VM
    Remove-AzureRmVM -ResourceGroupName $rg -Name $vmName

    #Create new availability set if it does not exist
    $availSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -ErrorAction Ignore
    if (-Not $availSet) {
    $availset = New-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -Location $OriginalVM.Location
    }

    #Create the basic configuration for the replacement VM
    $newVM = New-AzureRmVMConfig -VMName $OriginalVM.Name -VMSize $OriginalVM.HardwareProfile.VmSize -AvailabilitySetId $availSet.Id
    Set-AzureRmVMOSDisk -VM $NewVM -VhdUri $OriginalVM.StorageProfile.OsDisk.Vhd.Uri  -Name $OriginalVM.Name -CreateOption Attach -Windows

    #Add Data Disks
    foreach ($disk in $OriginalVM.StorageProfile.DataDisks ) { 
    Add-AzureRmVMDataDisk -VM $newVM -Name $disk.Name -VhdUri $disk.Vhd.Uri -Caching $disk.Caching -Lun $disk.Lun -CreateOption Attach -DiskSizeInGB $disk.DiskSizeGB
    }

    #Add NIC(s)
    foreach ($nic in $OriginalVM.NetworkInterfaceIDs) {
        Add-AzureRmVMNetworkInterface -VM $NewVM -Id $nic
    }

    #Create the VM
    New-AzureRmVM -ResourceGroupName $rg -Location $OriginalVM.Location -VM $NewVM -DisableBginfoExtension
```

## <a name="next-steps"></a>Volgende stappen

Extra opslagruimte toevoegen aan uw VM door een extra [schijf met gegevens](virtual-machines-windows-attach-disk-portal.md)toe te voegen.


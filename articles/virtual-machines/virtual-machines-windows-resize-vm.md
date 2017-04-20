<properties
    pageTitle="Het formaat van een Windows VM | Microsoft Azure"
    description="Het formaat van een virtuele Windows-computer in het implementatiemodel Resource Manager met Azure Powershell gemaakt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>

    
# <a name="resize-a-windows-vm"></a>Het formaat van een Windows VM

In dit artikel wordt beschreven hoe u het formaat van een Windows VM, gemaakt in de Resource Manager implementatiemodel met Azure Powershell.

Nadat u een virtuele machine (VM) maakt, kunt u de VM schalen omhoog of omlaag door de grootte van de VM. In sommige gevallen moet u eerst de VM opheffen. Dit kan gebeuren als de nieuwe grootte is niet beschikbaar op de hardware-cluster dat momenteel als host voor de VM optreedt.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Het formaat van een Windows VM niet in een set van beschikbaarheid

1. Een overzicht van de VM-formaten die beschikbaar zijn op de hardware-cluster waarin de VM wordt gehost. 

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```

2. Als de gewenste grootte wordt weergegeven, voer de volgende opdrachten om het formaat van de VM. Als het gewenste formaat niet wordt vermeld, gaat u naar stap 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Als het gewenste formaat niet wordt vermeld, voert u de volgende opdrachten de VM, de toewijzing het formaat en de VM start.

    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [AZURE.WARNING] Dynamische IP-adressen toegewezen aan de VM VM vrijgeven worden vrijgegeven. De schijven van OS en de gegevens worden niet beïnvloed. 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Het formaat van een Windows VM in een set van beschikbaarheid

Als de nieuwe grootte voor een VM in een set beschikbaarheid niet beschikbaar op de hardware-cluster de VM momenteel als host optreedt is, moet alle VMs in de beschikbaarheid aan om de grootte van de VM te worden opgeheven. Ook is het mogelijk de grootte van andere VMs bijgewerkt in de beschikbaarheid ingesteld nadat een VM hebt aangepast. Als u een VM in een set van beschikbaarheid, moet u de volgende stappen uitvoeren.

1. Een overzicht van de VM-formaten die beschikbaar zijn op de hardware-cluster waarin de VM wordt gehost.

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```

2. Als de gewenste grootte wordt weergegeven, voer de volgende opdrachten om het formaat van de VM. Als niet wordt vermeld, gaat u naar stap 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Als het gewenste formaat niet wordt vermeld, gaat u verder met de volgende stappen uit om alle VMs in de set beschikbaarheid deallocate, VMs formaat en opnieuw.

4.  Stop alle VMs in de beschikbaarheid.

    ```powershell
    $rg = "<resourceGroupName>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
    } 
    ```
              
5.  Het formaat en de VMs opnieuw in de beschikbaarheid.

    ```powershell
    $rg = "<resourceGroupName>"
    $newSize = "<newVmSize>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
        $vm.HardwareProfile.VmSize = $newSize
        Update-AzureRmVM -ResourceGroupName $rg -VM $vm
        Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
    }
    ```

## <a name="next-steps"></a>Volgende stappen

- Uitvoeren van meerdere exemplaren van de VM voor extra schaalbaarheid, en schaalt. Zie [automatisch schalen Windows machines in een virtuele Machine schaal instellen](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md)voor meer informatie.




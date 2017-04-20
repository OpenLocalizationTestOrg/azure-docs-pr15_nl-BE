<properties
    pageTitle="Beheren met behulp van bronbeheer en PowerShell VMs | Microsoft Azure"
    description="Virtuele machines met behulp van bronbeheer Azure en PowerShell beheren."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-resource-manager-and-powershell"></a>Azure virtuele Machines met behulp van bronbeheer en PowerShell beheren

## <a name="install-azure-powershell"></a>Azure PowerShell installeren
 
[Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor informatie over het installeren van de nieuwste versie van Azure PowerShell, uw abonnement selecteren en aanmelden bij uw account.

## <a name="set-variables"></a>Variabelen instellen

De opdrachten in het artikel moeten de naam van de bronnengroep waarin de virtuele machine bevindt en de naam van de virtuele machine te beheren. De waarde van **$rgName** vervangen door de naam van de bronnengroep met de virtuele machine. De waarde van **$vmName** vervangen door de naam van de VM. De variabelen maken.

    $rgName = "resource-group-name"
    $vmName = "VM-name"

## <a name="display-information-about-a-virtual-machine"></a>Informatie weergeven over een virtuele machine

De virtuele machine informatie opvragen.
  
    Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Het resultaat zoals in het volgende voorbeeld:

    ResourceGroupName        : rg1
    Id                       : /subscriptions/{subscription-id}/resourceGroups/
                               rg1/providers/Microsoft.Compute/virtualMachines/vm1
    Name                     : vm1
    Type                     : Microsoft.Compute/virtualMachines
    Location                 : centralus
    Tags                     : {}
    AvailabilitySetReference : {
                                  "id": "/subscriptions/{subscription-id}/resourceGroups/
                                  rg1/providers/Microsoft.Compute/availabilitySets/av1"
                               }
    Extensions               : []
    HardwareProfile          : {
                                  "vmSize": "Standard_A0"
                               }
    InstanceView             : null
    NetworkProfile           : {
                                  "networkInterfaces": [
                                    {
                                      "properties.primary": null,
                                      "id": "/subscriptions/{subscription-id}/resourceGroups/
                                      rg1/providers/Microsoft.Network/networkInterfaces/nc1"
                                    }
                                  ]
                               }
    OSProfile                : {
                                  "computerName": "vm1",
                                  "adminUsername": "myaccount1",
                                  "adminPassword": null,
                                  "customData": null,
                                  "windowsConfiguration": {
                                    "provisionVMAgent": true,
                                    "enableAutomaticUpdates": true,
                                    "timeZone": null,
                                    "additionalUnattendContents": [],
                                    "winRM": null
                                  },
                                  "linuxConfiguration": null,
                                  "secrets": []
                               }
    Plan                     : null
    ProvisioningState        : Succeeded
    StorageProfile           : {
                                  "imageReference": {
                                    "publisher": "MicrosoftWindowsServer",
                                    "offer": "WindowsServer",
                                    "sku": "2012-R2-Datacenter",
                                    "version": "latest"
                                  },
                                  "osDisk": {
                                    "osType": "Windows",
                                    "encryptionSettings": null,
                                    "name": "osdisk",
                                    "vhd": {
                                      "Uri": "http://sa1.blob.core.windows.net/vhds/osdisk1.vhd"
                                    }
                                    "image": null,
                                    "caching": "ReadWrite",
                                    "createOption": "FromImage"
                                  }
                                  "dataDisks": [],
                               }
    DataDiskNames            : {}
    NetworkInterfaceIDs      : {/subscriptions/{subscription-id}/resourceGroups/
                                rg1/providers/Microsoft.Network/networkInterfaces/nc1}

## <a name="stop-a-virtual-machine"></a>Een virtuele machine stoppen

Stop de actieve virtuele machine.

    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName

U wordt gevraagd om bevestiging:

    Virtual machine stopping operation
    This cmdlet will stop the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
        
Voer **Y** om te stoppen met de virtuele machine.

Na een paar minuten wordt ongeveer uit zoals in het volgende voorbeeld:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:11:57 PM
    EndTime    : 9/13/2016 12:14:40 PM

## <a name="start-a-virtual-machine"></a>Een virtuele machine start

Start de virtuele machine als deze gestopt.

    Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Na een paar minuten wordt ongeveer uit zoals in het volgende voorbeeld:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:32:55 PM
    EndTime    : 9/13/2016 12:35:09 PM

Als u starten van een virtuele machine die al wordt uitgevoerd wilt, beschreven gebruik **Opnieuw AzureRmVM** volgende.

## <a name="restart-a-virtual-machine"></a>Een virtuele machine start

De actieve virtuele machine start.

    Restart-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Het resultaat zoals in het volgende voorbeeld:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:54:40 PM
    EndTime    : 9/13/2016 12:55:54 PM

## <a name="delete-a-virtual-machine"></a>Verwijderen van een virtuele machine

Hiermee verwijdert u de virtuele machine.  

    Remove-AzureRmVM -ResourceGroupName $rgName –Name $vmName

> [AZURE.NOTE] U kunt de **-Force** parameter voor het overslaan van de bevestiging wordt gevraagd.

Als u hebt gebruikt de parameter - Force, wordt u gevraagd om bevestiging:

    Virtual machine removal operation
    This cmdlet will remove the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Het resultaat zoals in het volgende voorbeeld:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## <a name="update-a-virtual-machine"></a>Bijwerken van een virtuele machine

In dit voorbeeld ziet hoe u de grootte van de virtuele machine bijwerkt.
        
    $vmSize = "Standard_A1"
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    $vm.HardwareProfile.vmSize = $vmSize
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
    
Het resultaat zoals in het volgende voorbeeld:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK
                              
Zie [voor virtuele machines in Azure](virtual-machines-windows-sizes.md) voor een lijst met beschikbare formaten voor een virtuele machine.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Een schijf met gegevens toevoegen aan een virtuele machine

In dit voorbeeld ziet u hoe een gegevensschijf toevoegen aan een bestaande virtuele machine.

    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm

De schijf die u toevoegt, is niet geïnitialiseerd. U kunt zich aanmelden bij deze en Schijfbeheer gebruiken om de schijf initialiseren. Als u geïnstalleerd WinRM en een certificaat hebt gemaakt, kunt u externe PowerShell naar de schijf initialiseren. U kunt ook een aangepast script extensie: 

    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"

Het scriptbestand kan bevatten als deze code op de schijf initialiseren:

    $disks = Get-Disk |   Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { ([char]$_) }
    $count = 0
    $labels = @("data1","data2")

    foreach($d in $disks) {
        $driveLetter = $letters[$count].ToString()
        $d | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] `
            -Confirm:$false -Force 
        $count++
    }

## <a name="next-steps"></a>Volgende stappen

Als er problemen met een installatie, misschien interessant [Troubleshooting resource groep implementaties met Azure portal](../resource-manager-troubleshoot-deployments-portal.md)

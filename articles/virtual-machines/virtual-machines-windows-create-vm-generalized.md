<properties
    pageTitle="VM een gegeneraliseerde VHD maken | Microsoft Azure"
    description="Informatie over het maken van een virtuele Windows-computer vanaf een algemene VHD-installatiekopie met Azure PowerShell, in het implementatiemodel Resource Manager."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-generalized-vhd-image"></a>Een VM maken op basis van een algemene VHD-installatiekopie

Een algemene VHD-installatiekopie heeft al uw persoonlijke accountgegevens verwijderd met behulp van [Sysprep](virtual-machines-windows-generalize-vhd.md). U kunt een gegeneraliseerde VHD Sysprep uitvoert op een VM in lokalen, vervolgens [uploaden van de VHD naar Azure](virtual-machines-windows-upload-image.md), of Sysprep uitvoert op een bestaande Azure VM en vervolgens [kopieert de VHD](virtual-machines-windows-vhd-copy.md)maken.

Als u wilt dat een VM maken van een gespecialiseerde VHD, Zie [een VM vanaf een speciale VHD maken](virtual-machines-windows-create-vm-specialized.md).

De snelste manier om een VM maken vanuit een gegeneraliseerde VHD is [slag sjabloon] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image). 


## <a name="prerequisites"></a>Vereisten

Als u gebruiken een VHD geüpload vanaf een VM op ruimten, zoals een gemaakt met behulp van Hyper-V wilt, moet u controleren of u de aanwijzingen in het [voorbereiden van een Windows-VHD te uploaden naar Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md)gevolgd. 

Geüploade VHD's zowel bestaande Azure VM VHD's moet worden generalized voordat u een VM kunt maken met behulp van deze methode. Zie [generaliseren een virtuele Windows-computer met behulp van Sysprep](virtual-machines-windows-generalize-vhd.md)voor meer informatie. 


## <a name="set-the-uri-of-the-vhd"></a>De URI van de VHD instellen

De URI voor de VHD te gebruiken is in de indeling: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**VHD. In dit voorbeeld de VHD met de naam **myVHD** is in de opslag rekening **mystorageaccount** in de container **mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

De vNet en het subnetmasker van het [virtuele netwerk](../virtual-network/virtual-networks-overview.md)maken.


1. Het subnet maken. In het volgende voorbeeld wordt een subnet genaamd **mySubnet** in de resource groep **myResourceGroup** met het adresprefix **10.0.0.0/24**gemaakt.  

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
      
2. Het virtuele netwerk maken. In het volgende voorbeeld wordt een virtueel netwerk met de naam **myVnet** in de **VS West** locatie bij het adresprefix van **10.0.0.0/16**gemaakt.  

    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-network-interface"></a>Maak een openbare IP-adres en netwerk interface

Om te kunnen communiceren met de virtuele machine in het virtuele netwerk, moet u een [openbaar IP-adres](../virtual-network/virtual-network-ip-addresses-overview-arm.md) en een netwerkinterface.

1. Maak een openbaar IP-adres. In dit voorbeeld wordt een openbaar IP-adres met de naam **myPip**. 

    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Maak de NIC. In dit voorbeeld wordt een NIC met de naam **myNic**. 

    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>De beveiligingsgroep netwerk en een RDP-regel maken

Om te kunnen aanmelden bij uw VM met RDP, moet u een regel waarmee RDP toegang via poort 3389. 

In dit voorbeeld wordt een NSG met de naam **myNsg** met een regel voor **myRdpRule** waarmee RDP-verkeer via poort 3389. Zie voor meer informatie over de NSGs [poorten openen voor een VM in Azure PowerShell gebruiken](virtual-machines-windows-nsg-quickstart-powershell.md).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Een variabele maken voor het virtuele netwerk

Een variabele maken voor de voltooide virtueel netwerk. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

## <a name="create-the-vm"></a>De VM maken

De volgende PowerShell script laat zien hoe de virtuele machine configuraties instellen en gebruiken van het geüploade image voor VM als bron voor de nieuwe installatie.

</br>


```powershell
    # Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential
    
    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"
    
    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"
    
    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"
    
    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"
    
    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"
    
    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage, Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"
    
    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName
    
    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    
    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    
    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
    
    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows
    
    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a>Controleer of de VM is gemaakt. 

Als alles klaar is, ziet u de zojuist gemaakte VM in [Azure portal](https://portal.azure.com) onder **Zoeken** > **virtuele machines**, of met behulp van de volgende PowerShell-opdrachten:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Volgende stappen

Zie voor het beheren van uw nieuwe virtuele machine met Azure PowerShell [beheren virtuele machines met behulp van bronbeheer Azure en PowerShell](virtual-machines-windows-ps-manage.md).



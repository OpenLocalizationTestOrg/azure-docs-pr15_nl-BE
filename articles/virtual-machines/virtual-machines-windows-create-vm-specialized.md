<properties
    pageTitle="Maak een kopie van uw Windows-VM | Microsoft Azure"
    description="Informatie over het maken van een kopie van uw gespecialiseerde Azure VM met Windows, in het implementatiemodel Resource Manager."
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
    ms.date="09/21/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-specialized-vhd"></a>Een VM maken van een gespecialiseerde VHD

Maak een nieuwe VM door een gespecialiseerde VHD koppelen als de OS-schijf met Powershell. Een gespecialiseerde VHD onderhoudt de gebruikersaccounts, toepassingen en andere gegevens uit uw oorspronkelijke VM. 

Als u wilt dat een VM maken vanuit een gegeneraliseerde VHD, Zie [een VM uit een algemene VHD-installatiekopie maken](virtual-machines-windows-create-vm-generalized.md).

## <a name="create-the-subnet-and-vnet"></a>Het subNet en een vNet maken

De vNet en het subnetmasker van het [virtuele netwerk](../virtual-network/virtual-networks-overview.md)maken.

1. Het subNet maken. In dit voorbeeld maakt u een subnet genaamd **mySubNet**in de **myResourceGroup**van de resource-groep en wordt het voorvoegsel van subnet adres ingesteld op **10.0.0.0/24**.

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```

2. De vNet maken. In het volgende voorbeeld wordt de virtuele-netwerknaam **myVnetName**, de locatie voor het **Westelijk Verenigde Staten**en het adresprefix voor het virtuele netwerk op **10.0.0.0/16**. 

    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-nic"></a>Maak een openbaar IP-adres en NIC

Om te kunnen communiceren met de virtuele machine in het virtuele netwerk, moet u een [openbaar IP-adres](../virtual-network/virtual-network-ip-addresses-overview-arm.md) en een netwerkinterface.

1. De openbare IP maken. In dit voorbeeld wordt is de naam van de openbare IP-adres ingesteld op **myIP**.

    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Maak de NIC. In dit voorbeeld wordt is de naam van de Netwerkkaart ingesteld op **myNicName**.

    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>De beveiligingsgroep netwerk en een RDP-regel maken

Om te kunnen aanmelden bij uw VM met RDP, moet u een regel waarmee RDP toegang via poort 3389. Omdat de VHD voor de nieuwe VM is gemaakt van een bestaande kunt gespecialiseerde VM, nadat de VM is gemaakt, moet u een bestaande account van de bron virtuele machine waarvoor de machtiging Aanmelden via RDP.

In het volgende voorbeeld wordt de naam van de NSG **myNsg** en de naam RDP naar **myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```

Zie voor meer informatie over de eindpunten en NSG regels [openen van poorten voor een VM in Azure PowerShell gebruiken](virtual-machines-windows-nsg-quickstart-powershell.md).

## <a name="create-the-vm-configuration"></a>De VM-configuratie maken

De VM-configuratie instellen om de gekopieerde VHD als de OS-VHD te koppelen.


```powershell
    # Set the URI for the VHD that you want to use. In this example, the VHD file named "myOsDisk.vhd" is kept in a storage account named "myStorageAccount" in a container named "myContainer".
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    
    #Set the VM name and size. This example sets the VM name to "myVM" and the VM size to "Standard_A2".
    $vmName = "myVM"
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"

    #Add the NIC
    $vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

    #Add the OS disk by using the URL of the copied OS VHD. In this example, when the OS disk is created, the term "osDisk" is appened to the VM name to create the OS disk name. This example also specifies that this Windows-based VHD should be attached to the VM as the OS disk.
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
```


Als u gegevensschijven die moeten worden gekoppeld aan de VM hebt, moet u ook het volgende toevoegen: 

```powershell
    # Optional: Add data disks by using the URLs of the copied data VHDs at the appropriate Logical Unit Number (Lun).
    $dataDiskName = $vmName + "dataDisk"
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 0 -CreateOption attach
```

De gegevens en het besturingssysteem schijf URL's als volgt uitzien: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. U kunt dit vinden op de portal door te bladeren naar de doelcontainer opslag, klikt u op het besturingssysteem of gegevens VHD die is gekopieerd en vervolgens de inhoud van de URL te kopiëren.


## <a name="create-the-vm"></a>De VM maken

Maak de VM met de configuraties die we zojuist hebben gemaakt.

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Als deze opdracht geslaagd is, ziet u uitvoer als volgt:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
 
```
 
## <a name="verify-that-the-vm-was-created"></a>Controleer of de VM is gemaakt. 
 
U moet de nieuwe VM ziet in de [Azure portal](https://portal.azure.com)onder **Zoeken** > **virtuele machines**, of met behulp van de volgende PowerShell-opdrachten:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Volgende stappen

Om aan te melden met uw nieuwe virtuele machine, Ga naar de VM in de [portal](https://portal.azure.com)en klikt u op **verbinding maken met**het externe bureaublad RDP-bestand openen. De referenties van de oorspronkelijke virtuele machine kunt aanmelden bij uw nieuwe virtuele machine gebruikt. Zie [verbinding maken met en aanmelden bij een Azure virtuele machine met Windows](virtual-machines-windows-connect-logon.md)voor meer informatie.








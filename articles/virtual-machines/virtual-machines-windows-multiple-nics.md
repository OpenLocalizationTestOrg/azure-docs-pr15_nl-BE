<properties
   pageTitle="Maak een Windows VM met meerdere NIC's | Microsoft Azure"
   description="Informatie over het maken van een Windows VM met meerdere NIC's die zijn gekoppeld met behulp van sjablonen Azure PowerShell of Resource Manager."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-windows-vm-with-multiple-nics"></a>Een Windows VM maken met meerdere NIC 's
U kunt een virtuele machine (VM) maken in Azure met meerdere virtuele netwerkinterfaces (NIC's) is gekoppeld. Een algemeen scenario zou zijn om met verschillende subnetten voor front-end en back-end-verbinding of een netwerk dat is toegewezen aan een controle of een back-up oplossing. In dit artikel vindt snel opdrachten voor het maken van een VM met meerdere NIC's is gekoppeld. Voor gedetailleerde informatie, met inbegrip van het maken van meerdere NIC's binnen uw eigen PowerShell scripts, meer informatie over het [implementeren van meerdere NIC's VMs](../virtual-network/virtual-network-deploy-multinic-arm-ps.md). Verschillende [formaten VM](virtual-machines-windows-sizes.md) ondersteunen een wisselend aantal NIC's, zodat het formaat van uw VM dienovereenkomstig.

>[AZURE.WARNING] Wanneer u een VM maken - u de NIC's aan een bestaande VM toevoegen kunt, moet u meerdere netwerkkaarten koppelen. U kunt [een VM op basis van de oorspronkelijke virtuele schijven maken](virtual-machines-windows-vhd-copy.md) en meerdere NIC's implementatie van de VM te maken.

## <a name="create-core-resources"></a>Belangrijkste resources maken
Zorg dat u de [meest recente Azure PowerShell is geïnstalleerd en geconfigureerd](../powershell-install-configure.md). Log in op uw Azure account:

```powershell
Login-AzureRmAccount
```

In de volgende voorbeelden kunt u voorbeeld parameternamen vervangen door uw eigen waarden. Voorbeeld van de parameternamen opgenomen `myResourceGroup`, `mystorageaccount`, en `myVM`.

Maak eerst een resourcegroep. In het volgende voorbeeld maakt u een groep met de naam `myResourceGroup` in de `WestUs` locatie:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

Maak een account opslag voor uw VMs. Het volgende voorbeeld wordt een opslag rekening met de naam `mystorageaccount`:

```powershell
$storageAcc = New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "mystorageaccount" `
    -Kind "Storage" -SkuName "Premium_LRS" 
```

## <a name="create-virtual-network-and-subnets"></a>Virtueel netwerk en de subnetten maken
Twee subnetten virtueel netwerk - definiëren voor front-verkeer en één voor verkeer van de back-end. In het volgende voorbeeld definieert twee subnetten, met de naam `mySubnetFrontEnd` en `mySubnetBackEnd`:

```powershell
$mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
    -AddressPrefix "192.168.1.0/24"
$mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
    -AddressPrefix "192.168.2.0/24"
```

Het virtuele netwerk en de subnetten maken. Het volgende voorbeeld wordt een virtueel netwerk met de naam `myVnet`:

```powershell
$myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myVnet" -AddressPrefix "192.168.0.0/16" `
    -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
```


## <a name="create-multiple-nics"></a>Maak meerdere NIC 's
Twee NIC's, één Netwerkkaart op de front-end-subnet en één NIC koppelen aan de back-end-subnet maken. In het volgende voorbeeld maakt u twee NIC's met de naam `myNic1` en `myNic2`:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic1" -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic2" -SubnetId $backEnd.Id
```

Maakt u gewoonlijk ook een [beveiligingsgroep netwerk](../virtual-network/virtual-networks-nsg.md) of [de belasting voor documentconversies](../load-balancer/load-balancer-overview.md) te beheren en distribueren van verkeer via de VMs. De [meer gedetailleerde meerdere NIC's VM](../virtual-network/virtual-network-deploy-multinic-arm-ps.md) artikel helpt u bij een netwerk beveiligingsgroep maken en toewijzen van NIC's.


## <a name="create-the-virtual-machine"></a>De virtuele machine maken
Start nu uw VM configuratie maken. De grootte van elke VM heeft een limiet voor het totale aantal NIC's die u aan een VM toevoegen kunt. Meer informatie over [Windows VM formaten](virtual-machines-windows-sizes.md). 

Stel eerst uw VM referenties naar de `$cred` variabele als volgt:

```powershell
$cred = Get-Credential
```

In het volgende voorbeeld definieert een VM met de naam `myVM` en gebruikt een VM-grootte die maximaal twee NIC's ondersteunt (`Standard_DS2_v2`):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2_v2"
```

De rest van de VM-configuratie maken. In het volgende voorbeeld maakt u een Windows Server 2012 R2 VM:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName Te"MyVM" `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
```

Koppel de twee NIC's die u eerder hebt gemaakt:

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
```

De opslag en de virtuele schijf configureren voor uw nieuwe VM:

```powershell
$blobPath = "vhds/WindowsVMosDisk.vhd"
$osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
$diskName = "windowsvmosdisk"
$vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $diskName -VhdUri $osDiskUri `
    -CreateOption "fromImage"
```

Tot slot maakt u een VM:

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "WestUS"
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Meerdere NIC's met behulp van bronbeheer sjablonen maken
Declaratieve JSON bestanden Azure Resource Manager-sjablonen gebruiken voor het definiëren van uw omgeving. Hier vindt u een [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). Resource Manager-sjablonen kunnen u meerdere exemplaren van een resource tijdens de implementatie, zoals het maken van meerdere NIC's maken. U *exemplaar* geeft u het aantal exemplaren te maken:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Meer informatie over het [maken van meerdere exemplaren met *kopiëren*](../resource-group-create-multiple.md). 

U kunt ook een `copyIndex()` om vervolgens een nummer toevoegen aan de naam van een resource, kunt u voor het maken van `myNic1`, `MyNic2`, enz. Hier volgt een voorbeeld van de indexwaarde toe te voegen:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Hier vindt u een volledig voorbeeld van [meerdere NIC's met behulp van bronbeheer sjablonen maken](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Volgende stappen
Zorg ervoor dat [Windows VM formaten](virtual-machines-windows-sizes.md) controleren bij het maken van een VM met meerdere netwerkkaarten. Let op het maximum aantal NIC's elke grootte VM ondersteunt. 

Houd er rekening mee dat u geen extra NIC's toevoegen aan een bestaande VM, moet u de NIC's bij het implementeren van de VM. Wees voorzichtig bij het plannen van de distributie om te controleren of de vereiste verbinding met het netwerk vanaf het begin.
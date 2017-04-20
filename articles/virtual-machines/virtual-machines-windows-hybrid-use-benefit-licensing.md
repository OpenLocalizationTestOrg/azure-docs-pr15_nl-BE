<properties
   pageTitle="Azure hybride gebruik vergoeding voor venster Server | Microsoft Azure"
   description="Meer informatie over het optimaliseren van uw Windows Server Software Assurance-voordelen om licenties op ruimten naar Azure"
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
   ms.workload="infrastructure-services"
   ms.date="07/13/2016"
   ms.author="georgem"/>

# <a name="azure-hybrid-use-benefit-for-windows-server"></a>Azure hybride gebruik vergoeding voor Windows Server

U kunt uw locatie op Windows Server-licenties brengen Azure en Windows Server VMs in Azure uitvoeren tegen een verlaagde prijs voor klanten die Windows Server met Software Assurance. Het voordeel van Azure hybride gebruik kunt u Windows Server VMs in Azure uitgevoerd en alleen krijgen in rekening gebracht voor het tarief berekenen op basis. Zie de [Azure hybride gebruik voordeel pagina licentieverlening](https://azure.microsoft.com/pricing/hybrid-use-benefit/)voor meer informatie. In dit artikel wordt uitgelegd hoe u Windows Server VMs in Azure gebruiken deze licentie vergoeding implementeren.

> [AZURE.NOTE] U afbeeldingen op Marketplace Azure niet gebruiken voor de implementatie van Windows Server VMs gebruik van het voordeel Azure hybride gebruiken. U moet uw VMs PowerShell of bronnenbeheerder sjablonen met uw VMs correct wordt geregistreerd als die in aanmerking komen voor kortingen berekenen op basis implementeren.

## <a name="pre-requisites"></a>Minimumvereisten
Er zijn enkele van de vereisten om te kunnen gebruikmaken van Azure hybride gebruik vergoeding voor Windows Server VMs in Azure:

- De module Azure PowerShell is geïnstalleerd
- Uw Windows Server VHD naar Azure opslag geüpload hebben

### <a name="install-azure-powershell"></a>Azure PowerShell installeren
Zorg ervoor dat u hebt [geïnstalleerd en geconfigureerd, de meest recente Azure PowerShell](../powershell-install-configure.md). Zelfs als u uw VMs met behulp van bronbeheer sjablonen implementeren, moet u nog steeds Azure PowerShell is geïnstalleerd voor het uploaden van uw Windows Server VHD (Zie de volgende stap).

### <a name="upload-a-windows-server-vhd"></a>Een Windows-Server VHD uploaden

Als u wilt implementeren op een Windows Server VM in Azure, moet u eerst een VHD met build base Windows Server maken. Deze VHD moet op de juiste manier worden voorbereid met Sysprep voordat u het uploaden naar Azure. U kunt [meer lezen over de VHD-vereisten en het Sysprep-proces](./virtual-machines-windows-upload-image.md) en [Sysprep-ondersteuning voor serverfuncties](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Back-up van de VM voordat u Sysprep uitvoert. Als u de VHD hebt voorbereid, de VHD uploaden naar uw Azure opslag account met behulp van de `Add-AzureRmVhd` cmdlet als volgt:

```
Add-AzureRmVhd -ResourceGroupName MyResourceGroup -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd" -LocalFilePath 'C:\Path\To\myvhd.vhd'
```

> [AZURE.NOTE] Microsoft SQL Server en SharePoint Server Dynamics kunnen ook gebruikmaken van uw Software Assurance-licentie. U moet nog steeds de installatiekopie van Windows Server voorbereiden door de installatie van de toepassingsonderdelen van uw en licentiecodes dienovereenkomstig biedt, wordt de schijfkopie uploaden naar Azure. Bekijk de juiste documentatie voor het uitvoeren van Sysprep met uw toepassing, zoals [Overwegingen voor het installeren van SQL Server met behulp van Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) of [een referentiekopie van SharePoint Server 2016 (Sysprep) bouwen](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).

U kunt ook meer informatie over het [uploaden van de VHD naar Azure proces](./virtual-machines-windows-upload-image.md#upload-the-vm-image-to-your-storage-account).

> [AZURE.TIP] Dit artikel is gericht op de implementatie van Windows Server VMs. U kunt ook Windows Client VMs op dezelfde manier implementeren. In de volgende voorbeelden, vervangt u `Server` met `Client` op de juiste manier.

## <a name="deploy-a-vm-via-powershell-quick-start"></a>Implementeren van een VM via PowerShell Quick-Start
Wanneer u uw Windows Server VM via PowerShell implementeert, hebt u een extra parameter voor `-LicenseType`. Zodra u uw VHD geüpload naar Azure hebt, maak een nieuwe VM met `New-AzureRmVM` en geeft u het type licentieverlening als volgt:

```
New-AzureRmVM -ResourceGroupName MyResourceGroup -Location "West US" -VM $vm -LicenseType Windows_Server
```

U kunt [een meer gedetailleerde stapsgewijze instructies over het implementeren van een VM in Azure via PowerShell Lees](./virtual-machines-windows-hybrid-use-benefit-licensing.md#deploy-windows-server-vm-via-powershell-detailed-walkthrough) hieronder of een meer beschrijvende gids op de verschillende stappen voor het [maken van een Windows VM met behulp van bronbeheer en PowerShell](./virtual-machines-windows-ps-create.md)lezen.

## <a name="deploy-a-vm-via-resource-manager"></a>Implementeren van een VM via Resource Manager
Binnen de sjablonen Resource Manager, een extra parameter voor `licenseType` kan worden opgegeven. U kunt meer lezen over [Azure Resource Manager sjablonen ontwerpen](../resource-group-authoring-templates.md). Zodra u uw VHD geüpload naar Azure hebt, u Resource Manager sjabloon bewerken om het licentietype opnemen als onderdeel van de compute-provider en de sjabloon Normaal implementeren:

```
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   },
```
 
## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Controleer of uw VM gebruik maakt van de vergoeding voor licentieverlening
Zodra u uw VM hebt geïmplementeerd via de PowerShell of Resource Manager-implementatiemethode, controleert u of het licentietype met `Get-AzureRmVM` als volgt:
 
```
Get-AzureRmVM -ResourceGroup MyResourceGroup -Name MyVM
```

De uitvoer ziet er ongeveer als volgt:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Dit contrasteert met de volgende VM geïmplementeerd zonder het voordeel van Azure hybride gebruik licentieverlening, zoals een VM rechtstreeks vanuit de galerie Azure geïmplementeerd:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : 
```
 
## <a name="detailed-powershell-walkthrough"></a>Gedetailleerde scenario voor PowerShell

De volgende gedetailleerde stappen voor PowerShell weergeven voor een volledige implementatie van een VM. Hier vindt u meer context de werkelijke cmdlets en andere onderdelen worden gemaakt in [een Windows VM met behulp van bronbeheer en PowerShell maken](./virtual-machines-windows-ps-create.md). U stap voor stap de resourcegroep, account opslag en virtuele netwerken maken en vervolgens uw VM definiëren en ten slotte maken uw VM.
 
Eerst veilig referenties ophalen, een locatie en naam van de resource instellen:

```
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "TestLicensing"
```

Maak een openbare IP-adres:

```
$publicIPName = "testlicensingpublicip"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
```

Uw subnet, NIC en VNET te definiëren:

```
$subnetName = "testlicensingsubnet"
$nicName = "testlicensingnic"
$vnetName = "testlicensingvnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Naam van de VM en maak een VM-config:

```
$vmName = "testlicensing"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Definieer uw besturingssysteem:

```
$computerName = "testlicensing"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
```

Uw NIC toevoegen aan de VM:

```
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

U definieert de opslag te gebruiken:

```
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName testlicensing
```

De VHD naar behoren is voorbereid, te uploaden en te koppelen aan uw VM voor gebruik:

```
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://testlicensing.blob.core.windows.net/vhd/licensing.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Ten slotte uw VM maken en definiëren van het type licentie voor het gebruik van Azure hybride gebruik voordeel:

```
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType Windows_Server
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Licentieverlening Azure hybride gebruik vergoeding](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

Meer informatie over het [gebruik van Resource Manager-sjablonen](../azure-resource-manager/resource-group-overview.md).

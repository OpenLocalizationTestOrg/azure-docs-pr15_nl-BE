<properties
    pageTitle="Een SQL Server virtuele Machine maken in Azure PowerShell (Resource Manager) | Microsoft Azure"
    description="Stappen en PowerShell-scripts biedt voor het maken van een Azure VM met SQL Server virtuele machine galerijafbeeldingen."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Inrichten van een SQL Server virtuele machine met Azure PowerShell (Resource Manager)

> [AZURE.SELECTOR]
- [Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShell](virtual-machines-windows-ps-sql-create.md)

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt beschreven hoe u een enkel Azure virtuele machine met behulp van de **Bronnenbeheerder Azure** implementatiemodel met Azure PowerShell-cmdlets maken. In deze zelfstudie maken we een enkele virtuele machine met een enkele schijf uit een afbeelding in de galerie met SQL. We gaan de nieuwe voorzieningen voor de opslag, netwerk en compute resources die worden gebruikt door de virtuele machine maken. Als er bestaande voorzieningen voor elk van deze bronnen, kunt u deze providers gebruiken.

Als u de klassieke versie van dit onderwerp, Zie [een SQL Server virtuele machine met Azure PowerShell klassieke bepaling](virtual-machines-windows-classic-ps-sql-create.md).

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u nodig:

- Een Azure account en een abonnement op voordat u begint. Als u nog geen hebt, meld u aan voor een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).
- [Azure PowerShell)](../powershell-install-configure.md), minimale versie van 1.4.0 of hoger (deze zelfstudie geschreven versie 1.5.0).
    - Typ **Get-Module Azure-ListAvailable**voor het ophalen van uw versie.

## <a name="configure-your-subscription"></a>Uw abonnement configureren

Windows PowerShell te openen en toegang tot uw account Azure tot stand brengen door de volgende cmdlet wordt uitgevoerd. U krijgt een scherm inloggen uw referenties moet invoeren. Gebruik hetzelfde e-mailadres en wachtwoord die u gebruikt voor aanmelding bij de Azure portal.

    Add-AzureRmAccount

Na succesvol inloggen ziet u enkele gegevens op het scherm met de SubscriptionId die u hebt aangemeld. Dit is het abonnement waarin de bronnen voor deze zelfstudie worden gemaakt als u naar een ander abonnement. Als er meerdere SubscriptionIds, voeren de volgende cmdlet als resultaat een lijst van al uw SubscriptionIds:

    Get-AzureRmSubscription

Wilt wijzigen in een andere SubscriptionID, voert u de volgende cmdlet met uw gewenste SubscriptionId.

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a>Image-variabelen definiëren

Ter vereenvoudiging van de bruikbaarheid en het begrip van het script voltooid in deze zelfstudie wordt begin met een aantal variabelen definiëren. De parameterwaarden veranderen wanneer u naar eigen inzicht, maar pas op voor de naamgeving van de beperkingen die zijn gerelateerd aan de lengte van de naam en speciale tekens wanneer de waarden die u wilt wijzigen.

### <a name="location-and-resource-group"></a>Locatie en de resourcegroep
Met twee variabelen kunt u definiëren het gegevensgebied en de bronnengroep waarin u de andere bronnen voor de virtuele machine maken.

Desgewenst wijzigen en vervolgens de volgende cmdlets voor het initialiseren van deze variabelen worden uitgevoerd.

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a>Eigenschappen van opslag

De volgende variabelen gebruiken voor het definiëren van de account van de opslag en het type opslag moet worden gebruikt door de virtuele machine.

Desgewenst wijzigen en vervolgens de volgende cmdlet om deze variabelen initialiseren uitvoeren. Houd er rekening mee dat in dit voorbeeld we [Premium opslag gebruiken](../storage/storage-premium-storage.md), wordt aanbevolen voor de werkbelasting van de productie. Zie voor meer informatie over deze richtsnoeren en andere aanbevelingen, [de beste prestaties voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-performance.md).

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a>Eigenschappen van netwerk

De volgende variabelen gebruiken voor het definiëren van de netwerkinterface, de methode voor TCP/IP-toewijzing, de virtuele-netwerknaam, de subnetnaam van de virtuele, het bereik van IP-adressen voor het virtuele netwerk, het bereik van IP-adressen van het subnet en het publieke domein naamlabel moet worden gebruikt door het netwerk in de virtuele machine.

Desgewenst wijzigen en vervolgens de volgende cmdlet om deze variabelen initialiseren uitvoeren.

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"   

### <a name="virtual-machine-properties"></a>Eigenschappen van de virtuele machine

De volgende variabelen gebruiken voor het definiëren van de naam van de virtuele machine, de computernaam, de grootte van de virtuele machine en de schijfnaam besturingssysteem voor de virtuele machine.

Desgewenst wijzigen en vervolgens de volgende cmdlet om deze variabelen initialiseren uitvoeren.

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a>Eigenschappen van afbeelding

De volgende variabelen gebruiken voor het definiëren van de afbeelding te gebruiken voor de virtuele machine. In dit voorbeeld wordt wordt de afbeelding 2016 Enterprise van SQL Server gebruikt.

Desgewenst wijzigen en vervolgens de volgende cmdlet om deze variabelen initialiseren uitvoeren.

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

Houd er rekening mee dat u een volledige lijst met aanbiedingen van SQL Server-afbeelding met de opdracht Get-AzureRmVMImageOffer kunt krijgen:

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

En u kunt zien de SKU's die beschikbaar zijn voor een aanbieding met de opdracht Get-AzureRmVMImageSku. Met de volgende opdracht geeft dat alle SKU's beschikbaar voor de **SQL2014SP1-WS2012R2** bieden.

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Met het implementatiemodel Resource Manager is het eerste object dat u maakt de resourcegroep. We gebruiken de cmdlet [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt759837.aspx) een Azure resourcegroep en de bijbehorende bronnen te maken met de Resourcegroepnaam en de locatie is gedefinieerd door de variabelen die u eerder is geïnitialiseerd.

Voer de volgende cmdlet om uw nieuwe resourcegroep te maken.

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a>Maak een account voor opslag

De virtuele machine moet opslagbronnen voor de schijf en voor de SQL Server gegevens- en logboekbestanden. Voor eenvoud maken we een enkele schijf voor beide. U kunt extra schijven later met de cmdlet [Add-Azure schijf](https://msdn.microsoft.com/library/azure/dn495252.aspx) plaatst u uw SQL Server-gegevens en -logboekbestanden op speciale schijven koppelen. De cmdlet [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) gebruiken we een standaard opslag-account maken in de nieuwe resourcegroep en met de naam van opslag, opslag Sku-naam en locatie gedefinieerd met behulp van de variabelen die u eerder is geïnitialiseerd.

Voer de volgende cmdlet om uw nieuwe opslag-account maken.  

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a>Netwerk resources maken

De virtuele machine moet een aantal netwerkbronnen voor verbinding met het netwerk.

- Elke virtuele machine moet een virtueel netwerk.
- Een virtueel netwerk moet ten minste één subnet zijn gedefinieerd.
- Een netwerkinterface moet worden gedefinieerd met een openbare of een particuliere IP-adres.

### <a name="create-a-virtual-network-subnet-configuration"></a>De configuratie van een virtueel netwerk subnet maken

We wordt maken een subnetconfiguratie voor ons virtuele netwerk gestart. Voor onze zelfstudie maken we een standaard subnet met de cmdlet [New-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) . We gaan onze subnet virtueel netwerkconfiguratie maken met de naam en het adres subnetprefix gedefinieerd met behulp van de variabelen die u eerder is geïnitialiseerd.

>[AZURE.NOTE] Kunt u extra eigenschappen van het virtuele netwerk subnet is geconfigureerd met deze cmdlet, maar dat valt buiten het bestek van deze zelfstudie.

De volgende cmdlet wilt maken van de subnetconfiguratie van uw virtuele worden uitgevoerd.

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Vervolgens maken we ons virtuele netwerk met de cmdlet [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) . We gaan ons virtuele netwerk in uw nieuwe resourcegroep maken, met de naam, locatie en adres voorvoegsel gedefinieerd met behulp van de variabelen die u eerder is geïnitialiseerd en met behulp van de subnetconfiguratie dat u hebt gedefinieerd in de vorige stap.

Voer de volgende cmdlet om uw virtuele netwerk te maken.

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-the-public-ip-address"></a>Het openbare IP-adres maken

Nu hebben we onze virtueel netwerk gedefinieerd, we moet een IP-adres voor de verbinding met de virtuele machine te configureren. Voor deze zelfstudie maken we een openbaar IP-adres met behulp van dynamische IP-adressen voor de ondersteuning van Internet-verbinding. We gebruiken de cmdlet [New-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) maken van het openbare IP-adres in de resourcegroep prevously gemaakt en met de naam, locatie, toewijzingsmethode en DNS-domein naamlabel gedefinieerd met behulp van de variabelen die u eerder is geïnitialiseerd.

>[AZURE.NOTE] Kunt u extra eigenschappen van het openbare IP-adres met behulp van deze cmdlet, maar dat valt buiten het bestek van deze eerste zelfstudie. U kunt ook een privé-adres of een adres met een statisch adres maken, maar die ook valt buiten het bestek van deze zelfstudie.

Voer de volgende cmdlet wilt maken van uw openbare IP-adres.

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-the-network-interface"></a>De netwerkinterface maken

We zijn nu gereed voor het maken van de netwerkinterface die onze virtuele machine wilt gebruiken. De cmdlet [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) gebruiken we onze netwerkinterface in de resourcegroep gemaakt eerder en met de naam, locatie, subnet en eerder gedefinieerde openbare IP-adres maken.

Voer de volgende cmdlet om uw netwerkinterface te maken.

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a>Een VM-object configureren

Nu dat we opslag- en netwerkbronnen zijn gedefinieerd hebben, gaan we compute bronnen definiëren voor de virtuele machine. Voor training, zullen wij geven de grootte van de virtuele machine en verschillende eigenschappen van het besturingssysteem, blob-opslag definiëren de netwerkinterface die we eerder hebben gemaakt, en geef de schijf opgeven.

### <a name="create-the-vm-object"></a>De VM-object maken

We wordt door te geven van de grootte van de virtuele machine gestart. Wij zijn een DS13 opgeven voor deze zelfstudie. We gebruiken de cmdlet [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) voor het maken van een configureerbare virtuele machine-object met de naam en grootte gedefinieerd met behulp van de variabelen die u eerder is geïnitialiseerd.

De volgende cmdlet maken virtuele machine-object worden uitgevoerd.

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Een referentie-object waarin de naam en het wachtwoord voor de lokale administrator-referenties

Voordat we de besturingssysteem-eigenschappen voor de virtuele machine instellen kunt, moeten we de referenties voor de lokale administrator-account als een veilige tekenreeks. Om dit te bereiken, gebruiken we de cmdlet [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) .

De volgende cmdlet uitvoeren en typ de naam en het wachtwoord voor de lokale administrator-account in de virtuele Windows-computer in het venster Windows PowerShell referentie.

    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Het besturingssysteem eigenschappen instellen voor de virtuele machine

We kunnen nu de virtuele machine besturingssysteem eigenschappen in te stellen. We gebruiken de cmdlet [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) het type besturingssysteem als Windows instellen, moet de [agent van de virtuele machine](virtual-machines-windows-classic-agents-and-extensions.md) moet worden geïnstalleerd, dat voor automatische updates mogelijk maakt door de cmdlet opgeven en de naam van de virtuele machine, de computernaam en de referentie met de variabelen die u eerder is geïnitialiseerd.

De volgende cmdlet om het besturingssysteem eigenschappen instellen voor de virtuele machine wordt uitgevoerd.

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-the-network-interface-to-the-virtual-machine"></a>De netwerkinterface toevoegen aan de virtuele machine

Vervolgens zullen we de netwerkinterface die is gemaakt eerder toevoegen aan de virtuele machine. We gebruiken de cmdlet [Add-AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) toe te voegen van de netwerkinterface die met behulp van de network interface-variabele die u eerder hebt gedefinieerd.

De volgende cmdlet in te stellen van de netwerk-interface voor de virtuele machine wordt uitgevoerd.

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>De blob opslaglocatie instellen voor de schijf moet worden gebruikt door de virtuele machine

Wij Stel vervolgens de locatie van het blob voor de schijf moet worden gebruikt door de virtuele machine met behulp van de variabelen die u eerder hebt gedefinieerd.

De volgende cmdlet om de blob-opslaglocatie uitvoeren.

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Het besturingssysteem schijfeigenschappen instellen voor de virtuele machine

We zullen Stel vervolgens het besturingssysteem schijfeigenschappen voor de virtuele machine. We gebruiken de cmdlet [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) opgeven dat het besturingssysteem op de virtuele machine zal afkomstig van een afbeelding in te stellen in de cache opslaan als u wilt lezen alleen zijn (omdat SQL Server wordt geïnstalleerd op dezelfde schijf) en definieert de naam van de virtuele machine en het besturingssysteem schijf gedefinieerd met behulp van de variabelen die we eerder gedefinieerd.

De volgende cmdlet het besturingssysteem als schijfeigenschappen wilt instellen voor de virtuele machine wordt uitgevoerd.

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Geef de afbeelding platform voor de virtuele machine

Onze laatste configuratiestap is het opgeven van de afbeelding van het platform voor onze virtuele machine. Voor onze zelfstudie, we gebruiken de nieuwste SQL Server 2016 CTP-afbeelding. We gebruiken de cmdlet [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) deze afbeelding gebruiken zoals gedefinieerd door de variabelen die u eerder hebt gedefinieerd.

De volgende cmdlet opgeven van de afbeelding platform voor uw virtuele machine wordt uitgevoerd.

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-the-sql-vm"></a>Maak de SQL-VM

Nu u de configuratieprocedures hebt voltooid, bent u klaar voor het maken van de virtuele machine. We gebruiken de cmdlet [New-AzureRmVM](https://msdn.microsoft.com/library/mt603754.aspx) voor het maken van de virtuele machine met behulp van de variabelen die we hebben gedefinieerd.

De volgende cmdlet te maken van de virtuele machine wordt uitgevoerd.

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

De virtuele machine wordt gemaakt. U ziet dat een standaard opslag-account voor boot diagnostische gegevens gemaakt omdat de opslag opgegeven account voor de virtuele machine-schijf een premium-account voor opslag is.

Nu kunt u deze computer weergeven in de Portal Azure [het openbare IP-adres](virtual-machines-windows-portal-sql-server-provision.md#Connect)en de volledig gekwalificeerde domeinnaam.  

## <a name="example-script"></a>Voorbeeldscript

Het volgende script bevat de volledige PowerShell script voor deze zelfstudie. Hierbij wordt ervan uitgegaan dat u al setup de Azure-abonnement hebt te gebruiken met de opdrachten **Add-AzureRmAccount** en **Selecteer AzureRmSubscription** .


    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create the VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a>Volgende stappen
Nadat u de virtuele machine hebt gemaakt, bent u klaar voor verbinding met de virtuele machine met behulp van setup en RDP-verbinding. Zie [verbinding maken met een virtuele Machine op Azure (Resource Manager) in SQL Server](virtual-machines-windows-sql-connect.md)voor meer informatie.

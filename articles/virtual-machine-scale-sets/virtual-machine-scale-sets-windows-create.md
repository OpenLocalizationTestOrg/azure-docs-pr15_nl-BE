<properties
    pageTitle="Maak een virtuele Machine schaal ingesteld met PowerShell | Microsoft Azure"
    description="Maak een virtuele Machine schaal ingesteld met PowerShell"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Maak een Windows VM schaal met Azure PowerShell instellen

Deze stappen volgen een aanvullen-in-the-lege cellen benadering voor het maken van een set Azure VM schaal. Zie [Virtuele Machine schaal instellen-overzicht](virtual-machine-scale-sets-overview.md) voor meer informatie over de schaal wordt ingesteld.

Het moet ongeveer 30 minuten duren om de stappen in dit artikel.

## <a name="step-1-install-azure-powershell"></a>Stap 1: Installeer Azure PowerShell

[Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor informatie over het installeren van de nieuwste versie van Azure PowerShell, uw abonnement selecteren en aanmelden bij uw account.

## <a name="step-2-create-resources"></a>Stap 2: Resources maken

De middelen die nodig zijn voor de nieuwe schaal set maken.

### <a name="resource-group"></a>Resourcegroep

Een set virtuele machine schaal moet worden opgenomen in een resourcegroep.

1. Krijg een lijst met beschikbare locaties waar u resources kunt plaatsen:

        Get-AzureLocation | Sort Name | Select Name

2. Selecteer een locatie die het meest geschikt voor u en maak vervolgens de variabele de waarde van **$locName** vervangen door de naam van de locatie:

        $locName = "location name from the list, such as Central US"

3. Vervang de waarde van **$rgName** met de naam die u wilt gebruiken voor de nieuwe resourcegroep en maak vervolgens de variabele: 

        $rgName = "resource group name"
        
4. De resourcegroep maken:
    
        New-AzureRmResourceGroup -Name $rgName -Location $locName

    Worden er ongeveer uit zoals in het volgende voorbeeld:

        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Opslag account

Een account voor de opslag wordt gebruikt door een virtuele machine voor het opslaan van de schijf besturingssysteem en diagnostische gegevens die worden gebruikt voor schaling. Indien mogelijk, is het raadzaam een opslag-account voor elke virtuele machine gemaakt in een schaal. Als dat niet mogelijk, plan voor niet meer dan 20 VMs per account opslag. In het voorbeeld in dit artikel worden drie opslag rekeningen voor drie virtuele machines worden gemaakt.

1. De waarde van **$saName** vervangen door een naam voor de account van de opslag. Test de naam voor de uniekheid. 

        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName

    Als het antwoord **True**is, wordt de voorgestelde naam uniek is.

3. Vervang de waarde van **$saType** met het type van de opslag-account en maak vervolgens de variabele:  

        $saType = "storage account type"
        
    Mogelijke waarden zijn: Standard_LRS, Standard_GRS, Standard_RAGRS of Premium_LRS.
        
4. De account maken:
    
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName

    Worden er ongeveer uit zoals in het volgende voorbeeld:

        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext

5. Herhaal stap 1 tot en met 4 om drie opslag rekeningen, bijvoorbeeld myst1, myst2 en myst3 te maken.

### <a name="virtual-network"></a>Virtueel netwerk

Een virtueel netwerk is vereist voor de virtuele machines in de schaal.

1. Vervang de waarde van **$subnetName** met de naam die u wilt gebruiken voor het subnet in het virtuele netwerk en maak vervolgens de variabele: 

        $subnetName = "subnet name"
        
2. De subnetconfiguratie maken:
    
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
        
    Het adresprefix kan afwijken van het virtuele netwerk.

3. Vervang de waarde van **$netName** met de naam die u wilt gebruiken voor het virtuele netwerk en maak vervolgens de variabele: 

        $netName = "virtual network name"
        
4. Het virtuele netwerk maken:
    
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>Configuratie van de schaal instellen

Hebt u de resources die u moet configureren voor de schaal, dus laten we maken.  

1. Vervang de waarde van **$ipName** met de naam die u wilt gebruiken voor de IP-configuratie en maak vervolgens de variabele: 

        $ipName = "IP configuration name"
        
2. De IP-configuratie maken:

        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

2. Vervang de waarde van **$vmssConfig** met de naam die u wilt gebruiken voor de configuratie van de set schaal en maak vervolgens de variabele:   

        $vmssConfig = "Scale set configuration name"
        
3. De configuratie voor de schaal maken:

        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
        
    In dit voorbeeld ziet u dat een schaal instellen wordt gemaakt met de drie virtuele machines. Overzicht [virtuele Machine schaal ingesteld](virtual-machine-scale-sets-overview.md) voor meer informatie over de capaciteit van de schaal wordt ingesteld. Deze stap omvat ook het instellen van de grootte (genoemd SkuName) van de virtuele machines in de set. Voor een grootte die aan uw behoeften voldoet, zoekt met een [grootte voor virtuele machines](../virtual-machines/virtual-machines-windows-sizes.md).
    
4. Configuratie van de netwerkinterface toevoegen aan de configuratie van schaal instellen:
        
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
        
    Worden er ongeveer uit zoals in het volgende voorbeeld:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>Profiel van besturingssysteem

1. Vervang de waarde van **$computerName** door het voorvoegsel voor de computernaam die u wilt gebruiken en maak vervolgens de variabele: 

        $computerName = "computer name prefix"
        
2. Vervang de waarde van **$adminName** de naam van de administrator-account op de virtuele machines en maak vervolgens de variabele:

        $adminName = "administrator account name"
        
3. Vervang de waarde van **$adminPassword** met een wachtwoord voor de account en maak vervolgens de variabele:

        $adminPassword = "password for administrator accounts"
        
4. Het besturingssysteem-profiel te maken:

        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Opslag-profiel

1. Vervang de waarde van **$storageProfile** met de naam die u wilt gebruiken voor de opslag profiel en maak vervolgens de variabele:  

        $storageProfile = "storage profile name"
        
2. Maak de variabelen die de afbeelding definiëren:  
      
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
        
    Voor informatie over andere afbeeldingen die u wilt gebruiken, zoekt op [navigeren en selecteer Azure VM beelden met Windows PowerShell en de CLI Azure](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md).
        
3. Vervang de waarde van **$vhdContainers** met een lijst met paden waar de virtuele harde schijven zijn opgeslagen, zoals 'https://mystorage.blob.core.windows.net/vhds', en maak vervolgens de variabele:
       
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
        
4. Het opslag-profiel te maken:

        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Virtuele machine schaal instellen

Ten slotte kunt u de schaal instellen.

1. Vervang de waarde van **$vmssName** met de naam van de virtuele machine schaal instellen en vervolgens de variabele maken:

        $vmssName = "scale set name"
        
2. Maak de schaal instellen:

        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss

    Worden er ongeveer uit zoals in dit voorbeeld van een geslaagde implementatie van:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>Stap 3: Bronnen verkennen

Deze bronnen gebruiken om te verkennen van de virtuele machine schaal set die u hebt gemaakt:

- Azure portal - een beperkte hoeveelheid informatie is beschikbaar via de portal.
- [Azure Resource Explorer](https://resources.azure.com/) - dit programma is het meest geschikt voor het verkennen van de huidige status van uw set schaal.
- Azure PowerShell - Gebruik deze opdracht om informatie te verkrijgen:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        
        Or 
        
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        

## <a name="next-steps"></a>Volgende stappen

- Beheren van de schaal instellen dat u zojuist hebt gemaakt met de informatie in het [beheren van virtuele machines in een virtuele Machine schaal instellen](virtual-machine-scale-sets-windows-manage.md)
- Stel automatische schaling van de schaal instellen met behulp van informatie in de [Automatische schaling en virtuele machine schaal instellen](virtual-machine-scale-sets-autoscale-overview.md)
- Meer informatie over de verticale schaal aan de hand van [verticale automatisch schalen met virtuele Machine schaal sets](virtual-machine-scale-sets-vertical-scale-reprovision.md)

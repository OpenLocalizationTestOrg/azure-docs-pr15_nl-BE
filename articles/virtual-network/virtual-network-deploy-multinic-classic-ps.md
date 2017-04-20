<properties
   pageTitle="Meerdere NIC VMs met PowerShell in het klassieke implementatiemodel implementeren | Microsoft Azure"
   description="Informatie over het implementeren van meerdere NIC VMs met PowerShell in het implementatiemodel klassiek"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-classic-using-powershell"></a>Meerdere NIC VMs (klassiek) met PowerShell implementeren

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

U kunt het maken van virtuele machines (VMs) in Azure en meerdere netwerkinterfaces (NIC's) koppelen aan elk van uw VMs. Scheiding van de typen verkeer inschakelen meerdere NIC's via de NIC's. Zo kan één NIC communiceren met het Internet, terwijl een ander alleen met interne bronnen niet verbonden met het Internet communiceert. De mogelijkheid voor het scheiden van netwerkverkeer over meerdere netwerkkaarten is vereist voor veel netwerk virtuele apparaten, zoals de levering van de toepassing en optimalisatie van WAN-oplossingen.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Meer informatie over hoe u [deze stappen uitvoert met behulp van het model Resource Manager](virtual-network-deploy-multinic-arm-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Momenteel wordt er geen VMs met een enkele NIC en VMs met meerdere NIC's in dezelfde cloud-service. Daarom moet u de back-endservers implementeren in een andere cloud-service dan en andere onderdelen in het scenario. De volgende stappen gebruiken een cloud-service met de naam *IaaSStory* voor de belangrijkste bronnen en *IaaSStory BackEnd* voor de back-endservers.

## <a name="prerequisites"></a>Vereisten

Voordat u de back-endservers implementeren kunt, moet u de belangrijkste cloud-service met de nodige middelen voor dit scenario te implementeren. U moet ten minste een virtueel netwerk maken met een subnet voor de back-end. Ga naar [een virtueel netwerk via PowerShell maken](virtual-networks-create-vnet-classic-netcfg-ps.md) om informatie over het implementeren van een virtueel netwerk.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>De back-end VMs implementeren

Het VMs back-end is afhankelijk van het maken van de onderstaande bronnen.

- **Back-end-subnet**. De databaseservers uitmaken deel van een afzonderlijk subnet, om het verkeer te scheiden. Het onderstaande script verwacht dit subnet te bestaan in een vnet met de naam *WTestVnet*.
- **Opslag-account voor gegevensschijven**. Voor betere prestaties gebruikt de gegevensschijven in de databaseservers solid state schijf (SSD)-technologie, die een opslag premium account vereist. Zorg ervoor dat de Azure locatie die u voor de ondersteuning van de premie opslag implementeren.
- **Beschikbaarheid instellen**. Alle databaseservers wordt toegevoegd aan een enkele beschikbaarheid instellen om ervoor te zorgen is ten minste één van de VMs en tijdens het onderhoud worden uitgevoerd.

### <a name="step-1---start-your-script"></a>Stap 1: Start het script

U gebruikt het volledige PowerShell-script kunt downloaden [hier](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Volg de onderstaande stappen om te wijzigen in uw omgeving werkt met het script.

1. Wijzig de waarden van de variabelen op basis van uw bestaande brongroep geïmplementeerd boven in de [voorwaarden](#Prerequisites).

        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"

2. Wijzig de waarden van de variabelen op basis van de waarden die u wilt gebruiken voor de implementatie van back-end.

        $backendCSName         = "IaaSStory-Backend"
        $prmStorageAccountName = "iaasstoryprmstorage"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $diskSize              = 127
        $vmNamePrefix          = "DB"
        $dataDiskSuffix        = "datadisk"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Stap 2: het maken van benodigde bronnen voor uw VMs

U moet een nieuwe wolk service en een account opslag voor de gegevensschijven maken voor alle VMs. Ook moet u een afbeelding en een lokale administrator-account opgeven voor de VMs. De volgende stappen uit als u wilt maken van deze bronnen, worden uitgevoerd.

1. Maak een nieuwe wolk service.

        New-AzureService -ServiceName $backendCSName -Location $location

2. Maak een nieuwe account voor premium-opslag.

        New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
            -Location $location `
            -Type Premium_LRS

3. Stel de opslag rekening dat hierboven is gemaakt als de huidige account opslag voor uw abonnement.

        $subscription = Get-AzureSubscription `
            | where {$_.IsCurrent -eq $true}  
        Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
            -CurrentStorageAccountName $prmStorageAccountName

4. Selecteer een afbeelding voor de VM.

        $image = Get-AzureVMImage `
            | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
            | sort PublishedDate -Descending `
            | select -ExpandProperty ImageName -First 1

5. Referenties voor de lokale beheerder ingesteld.

        $cred = Get-Credential -Message "Enter username and password for local admin account"

### <a name="step-3---create-vms"></a>Stap 3: VMs maken

U moet maken zo veel VMs en de benodigde netwerkkaarten en VMs in de FOR-lus maken met een lus. De NIC's en VMs maken, de volgende stappen worden uitgevoerd.

1. Start een `for` herhalen als u wilt de opdrachten voor het maken van een VM en twee NIC's zo vaak als nodig herhalen op basis van de waarde van de `$numberOfVMs` variabele.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Maak een `VMConfig` object opgeven van de afbeelding, de grootte en de beschikbaarheid voor de VM instellen.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureVMConfig -Name $vmName `
                            -ImageName $image `
                            -InstanceSize $vmSize `
                            -AvailabilitySetName $avSetName  

3. De VM inrichten als een Windows VM.

            Add-AzureProvisioningConfig -VM $vmConfig -Windows `
                -AdminUsername $cred.UserName `
                -Password $cred.Password

4. De NIC instellen en toewijzen van een statisch IP-adres.

            Set-AzureSubnet -SubnetNames $backendSubnetName -VM $vmConfig
            Set-AzureStaticVNetIP -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig

5. Voeg een tweede Netwerkkaart voor elke VM.

            Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
                -SubnetName $backendSubnetName `
                -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
                -VM $vmConfig

6. Voor elke VM maken voor gegevensschijven.

            $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk1Name `
                -LUN 0       

            $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk2Name `
                -LUN 1

7. Maak van elke VM en beëindig de lus.

            New-AzureVM -VM $vmConfig `
                -ServiceName $backendCSName `
                -Location $location `
                -VNetName $vnetName
        }

### <a name="step-4---run-the-script"></a>Stap 4 - het script uitvoeren

Nu dat u hebt gedownload en het script op basis van uw behoeften wordt gewijzigd, runt hij een script wilt maken van de back-end database VMs met meerdere netwerkkaarten.

1. Sla het script en uitvoeren vanaf de opdrachtprompt **PowerShell** of **PowerShell ISE**. Ziet u de eerste uitvoer zoals hieronder wordt weergegeven.

        OperationDescription    OperationId                          OperationStatus
        --------------------    -----------                          ---------------
        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      

        WARNING: No deployment found in service: 'IaaSStory-Backend'.

2. Vul de gegevens in het dialoogvenster referenties nodig en klik op **OK**. De volgende uitvoer wordt weergegeven.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded

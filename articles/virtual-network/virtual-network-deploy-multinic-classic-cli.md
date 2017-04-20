<properties
   pageTitle="Implementeren met behulp van de CLI Azure in het implementatiemodel klassiek van meerdere NIC VMs | Microsoft Azure"
   description="Informatie over het implementeren van meerdere NIC VMs met behulp van de CLI Azure in het implementatiemodel klassiek"
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

#<a name="deploy-multi-nic-vms-classic-using-the-azure-cli"></a>Meerdere NIC VMs (klassiek) met behulp van de CLI Azure implementeren

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

U kunt het maken van virtuele machines (VMs) in Azure en meerdere netwerkinterfaces (NIC's) koppelen aan elk van uw VMs. Scheiding van de typen verkeer inschakelen meerdere NIC's via de NIC's. Zo kan één NIC communiceren met het Internet, terwijl een ander alleen met interne bronnen niet verbonden met het Internet communiceert. De mogelijkheid voor het scheiden van netwerkverkeer over meerdere netwerkkaarten is vereist voor veel netwerk virtuele apparaten, zoals de levering van de toepassing en optimalisatie van WAN-oplossingen.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Meer informatie over hoe u [deze stappen uitvoert met behulp van het model Resource Manager](virtual-network-deploy-multinic-arm-cli.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Momenteel wordt er geen VMs met een enkele NIC en VMs met meerdere NIC's in dezelfde cloud-service. Daarom moet u de back-endservers implementeren in een andere cloud-service dan en andere onderdelen in het scenario. De volgende stappen gebruiken een cloud-service met de naam *IaaSStory* voor de belangrijkste bronnen en *IaaSStory BackEnd* voor de back-endservers.

## <a name="prerequisites"></a>Vereisten

Voordat u de back-endservers implementeren kunt, moet u de belangrijkste cloud-service met de nodige middelen voor dit scenario te implementeren. U moet ten minste een virtueel netwerk maken met een subnet voor de back-end. Ga naar [een virtueel netwerk met behulp van de CLI Azure maken](virtual-networks-create-vnet-classic-cli.md) om informatie over het implementeren van een virtueel netwerk.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>De back-end VMs implementeren

Het VMs back-end is afhankelijk van het maken van de onderstaande bronnen.

- **Opslag-account voor gegevensschijven**. Voor betere prestaties gebruikt de gegevensschijven in de databaseservers solid state schijf (SSD)-technologie, die een opslag premium account vereist. Zorg ervoor dat de Azure locatie die u voor de ondersteuning van de premie opslag implementeren.
- **NIC's**. Elke VM heeft twee NIC's voor toegang tot de database, en een voor het management.
- **Beschikbaarheid instellen**. Alle databaseservers wordt toegevoegd aan een enkele beschikbaarheid instellen om ervoor te zorgen is ten minste één van de VMs en tijdens het onderhoud worden uitgevoerd.

### <a name="step-1---start-your-script"></a>Stap 1: Start het script

U kunt het volledige bash script gebruikt downloaden [hier](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh). Volg de onderstaande stappen om te wijzigen in uw omgeving werkt met het script.

1. Wijzig de waarden van de variabelen op basis van uw bestaande brongroep geïmplementeerd boven in de [voorwaarden](#Prerequisites).

        location="useast2"
        vnetName="WTestVNet"
        backendSubnetName="BackEnd"

2. Wijzig de waarden van de variabelen op basis van de waarden die u wilt gebruiken voor de implementatie van back-end.

        backendCSName="IaaSStory-Backend"
        prmStorageAccountName="iaasstoryprmstorage"
        image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
        avSetName="ASDB"
        vmSize="Standard_DS3"
        diskSize=127
        vmNamePrefix="DB"
        osDiskName="osdiskdb"
        dataDiskPrefix="db"
        dataDiskName="datadisk"
        ipAddressPrefix="192.168.2."
        username='adminuser'
        password='adminP@ssw0rd'
        numberOfVMs=2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Stap 2: het maken van benodigde bronnen voor uw VMs

1. Maak een nieuwe wolk service voor alle back-end VMs. Let op het gebruik van de `$backendCSName` variabele voor de naam van de resource, en `$location` voor de regio Azure.

        azure service create --serviceName $backendCSName \
            --location $location

2. Maak een account premie opslag voor de schijven OS en gegevens moet worden gebruikt door uw VMs.

        azure storage account create $prmStorageAccountName \
            --location $location \
            --type PLRS

### <a name="step-3---create-vms-with-multiple-nics"></a>Stap 3: VMs maken met meerdere NIC 's

1. Start een lus maken meerdere VMs, op basis van de `numberOfVMs` variabelen.

        for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
        do

2. Geef de naam en het IP-adres van elk van de twee netwerkadapters voor elke VM.

            nic1Name=$vmNamePrefix$suffixNumber-DA
            x=$((suffixNumber+3))
            ipAddress1=$ipAddressPrefix$x

            nic2Name=$vmNamePrefix$suffixNumber-RA
            x=$((suffixNumber+53))
            ipAddress2=$ipAddressPrefix$x

4. Maak de VM. Let op het gebruik van de `--nic-config` parameter met een lijst van alle NIC's met de naam, het subnetmasker en IP-adres.

            azure vm create $backendCSName $image $username $password \
                --connect $backendCSName \
                --vm-name $vmNamePrefix$suffixNumber \
                --vm-size $vmSize \
                --availability-set $avSetName \
                --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
                --virtual-network-name $vnetName \
                --subnet-names $backendSubnetName \
                --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::

5. Maak voor elke VM, twee gegevensschijven.

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
        done

### <a name="step-4---run-the-script"></a>Stap 4 - het script uitvoeren

Nu dat u hebt gedownload en het script op basis van uw behoeften wordt gewijzigd, wordt het script te maken van de back-end database VMs met meerdere NIC's uitvoeren.

1. Sla het script en uitvoeren vanaf de **Bash** -terminal. Ziet u de eerste uitvoer zoals hieronder wordt weergegeven.

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. De uitvoering wordt beëindigd na een paar minuten en ziet u de rest van de output zoals hieronder wordt weergegeven.

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK

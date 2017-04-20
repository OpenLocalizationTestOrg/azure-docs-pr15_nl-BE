<properties
    pageTitle="Azure Premium opslag gebruiken met SQL Server | Microsoft Azure"
    description="In dit artikel worden gemaakt met het klassieke implementatiemodel bronnen gebruikt en geeft advies over het gebruik van Azure Premium opslag met SQL Server wordt uitgevoerd op Azure virtuele Machines."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="jhubbard"
    editor="monicar"    
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth"/>

# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Azure Premium opslag gebruiken met SQL Server op virtuele Machines


## <a name="overview"></a>Overzicht

[Azure Premium opslag](../storage/storage-premium-storage.md) is de volgende generatie met lage latentie en hoge doorvoersnelheden i/o-opslag. Dit werkt het beste voor de belangrijkste i/o intensieve belasting, zoals SQL Server op IaaS [virtuele Machines](https://azure.microsoft.com/services/virtual-machines/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Dit artikel bevat een planning en richtlijnen voor het migreren van een virtuele Machine met SQL Server Premium opslag gebruiken. Dit omvat Azure infrastructuur (netwerk-, storage) en Gast Windows VM stappen. Het voorbeeld in de [bijlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) bevat een volledige uitgebreide complete migratie van het verplaatsen van grotere VMs profiteren van verbeterde lokale SSD-opslag met PowerShell.

Het is belangrijk te begrijpen met Azure Premium-opslagruimte met SQL Server op IAAS VMs end-to-end. Dit omvat:

- Identificatie van de vereisten voor het gebruik van de premie opslag.
- Voorbeelden van de implementatie van SQL Server op IaaS naar Premium opslag voor nieuwe implementaties.
- Voorbeelden van migratie bestaande installaties, zowel zelfstandige servers als SQL altijd op beschikbaarheidsgroepen-implementaties.
- Migratie van mogelijke benaderingen.
- Voorbeeld van de volledige end-to-end met Azure, Windows en SQL Server-stappen voor de migratie van een bestaande implementatie altijd op.

Zie voor meer achtergrondinformatie over Azure virtuele Machines van SQL Server, [SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md).

**Auteur:** Daniel Sol **technische revisoren:** Luis Carlos Vargas haring, Sanjay Mishra, Pravin Mital, Thomas Schwertl, Gonzalo Ruiz.

## <a name="prerequisites-for-premium-storage"></a>Vereisten voor de opslag van de premie

Er zijn verschillende voorwaarden voor het gebruik van de premie opslag.

### <a name="machine-size"></a>Grootte van de machine

Voor het gebruik van de premie opslag moet u de DS serie virtuele Machines (VM) gebruiken. Als u DS serie machines in de cloud-service voordat u niet hebt gebruikt, moet u de bestaande VM verwijderen, houden de aangesloten schijven en maak vervolgens een nieuwe wolk service voordat de VM als DS * rol opnieuw te maken. Zie voor meer informatie op de grootte van de virtuele Machine [virtuele Machine en Cloud Service formaten voor Azure](virtual-machines-linux-sizes.md).

### <a name="cloud-services"></a>Cloud services

U kunt alleen DS * VMs met Premium-opslagruimte gebruiken wanneer ze worden gemaakt in een nieuwe wolk service. Als u SQL Server altijd op in Azure, moet het altijd op Listener wordt verwezen naar de Azure interne of externe Load Balancer-IP-adres dat is gekoppeld aan een cloud-service. Dit artikel is gericht op het behoud van beschikbaarheid in dit scenario migreren.

> [AZURE.NOTE] Een reeks DS * moet de eerste VM aan de nieuwe Cloud-Service wordt geïmplementeerd.

### <a name="regional-vnets"></a>Regionale VNETS

U moet het virtuele netwerk (VNET) die als host fungeert voor uw VMs worden regionale configureren voor DS * VMs. Dit 'genereren' in de VNET is dat de grotere VMs in andere clusters worden ingericht en communicatie tussen beide toestaan. In de volgende schermafdruk toont de gemarkeerde locatie regionale VNETs, terwijl het eerste resultaat een 'klein' VNET bevat.

![RegionalVNET][1]

U kunt een Microsoft support ticket om te migreren naar een regionale VNET verhogen, Microsoft zal wijzigen en klik vervolgens op om de migratie naar regionale VNETs, de eigenschap AffinityGroup in de netwerkconfiguratie. De netwerkconfiguratie in PowerShell eerst exporteren en vervolgens de eigenschap **AffinityGroup** in het **VirtualNetworkSite** -element vervangen door een eigenschap **Location** . Geef `Location = XXXX` waarbij `XXXX` is een gebied met Azure. De nieuwe configuratie vervolgens importeren.

Bijvoorbeeld, overwegen de volgende VNET-configuratie:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

Als dit naar een regionale VNET in West-Europa verplaatsen, wijzigen van de configuratie het volgende:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Opslag-accounts

U moet een nieuwe opslag-account die is geconfigureerd voor opslag van de premie. Houd er rekening mee dat het gebruik van de premie opslag is ingesteld op de rekening van de opslag, niet op afzonderlijke VHD's, maar bij gebruik van een DS * reeks VM u de VHD Premium en standaardopslag rekeningen koppelen kunt. Kunt u overwegen dit als u niet wilt dat de VHD OS op de account van de premie opslag plaatsen.

De volgende opdracht **Nieuw AzureStorageAccountPowerShell** met de "Premium_LRS" **Type** maakt een opslag Premium Account:

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>Cache-instellingen voor VHD 's

Het belangrijkste verschil tussen het maken van schijven die deel van een rekening van de premie opslag uitmaken is de schijf-cache-instelling. Voor SQL Server Data volume schijven verdient het gebruik van '**Caching lezen**'. Bij Transaction log volumes, worden de instelling van de schijf-cache ingesteld op '**geen**'. Dit is anders dan de aanbevelingen voor de standaardopslag-accounts.

Zodra de VHD's zijn gekoppeld, kan de cache-instelling kan niet worden gewijzigd. U moet loskoppelen en opnieuw de VHD koppelen met een bijgewerkte cache-instelling.

### <a name="windows-storage-spaces"></a>Windows storage spaces

U kunt [Windows Storage Spaces](https://technet.microsoft.com/library/hh831739.aspx) net als met de vorige standaardopslag, zo kunt u voor het migreren van een VM die wordt al gebruikt door opslagruimtes. Het voorbeeld in [bijlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) (stap 9 en forward) bevat de Powershell code te extraheren en een VM met meerdere gekoppelde VHD's importeren.

Opslagpools werden gebruikt in de standaard Azure opslag account verbeteren doorvoer en latentie te verlagen. Wellicht hebt u waarde opslagpools met Premium opslag voor nieuwe implementaties testen, maar zij extra complexiteit met setup van opslag toevoegen.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>Het zoeken naar welke kaart opslagpools Azure virtuele schijven

Er zijn verschillende cache-instelling aanbevelingen voor gekoppelde VHD's, misschien de VHD's kopiëren naar een opslag van Premium-account. Echter, wanneer u opnieuw aan de nieuwe DS-serie VM koppelen, mogelijk moet u de cache-instellingen wijzigen. Het is eenvoudiger om toe te passen van de premie opslag aanbevolen cache-instellingen als u afzonderlijke VHD's voor de gegevensbestanden van SQL en logboekbestanden (in plaats van een enkele VHD met zowel).

> [AZURE.NOTE] De cacheoptie die u kiest als u SQL Server-gegevens en logboekbestanden bestanden op hetzelfde volume, is afhankelijk van de i/o toegang patronen voor de werkbelasting van uw database. Alleen testen kan aantonen welke cacheoptie is het beste voor dit scenario.

Als u werkt met Windows Storage ruimten die zijn opgebouwd uit meerdere VHD's u moet te kijken naar uw oorspronkelijke scripts te identificeren waaraan VHD's gekoppeld zijn echter in bepaalde toepassingen, zodat vervolgens stelt u de cache-instellingen derhalve voor elke schijf.

Als er geen oorspronkelijke script weer te geven die VHD's toewijzen aan de groep beschikbaar is, kunt u de volgende stappen uit om te bepalen van de toewijzing van opslagruimte op een schijf/toepassingen.

Voor elke schijf, gebruikt u de volgende stappen uit:

1. Lijst van schijven die zijn aangesloten op een VM met de opdracht **Get-AzureVM** ophalen:

    Get-AzureVM - servicenaam <servicename> -naam <vmname> | Get-AzureDataDisk

1. Noteer de Diskname en de LUN.

    ![DisknameAndLUN][2]

1. Extern bureaublad in de VM. Ga naar **Computer Management** | **Device Manager** | **schijven**. Bekijk de eigenschappen van elk van de 'Microsoft virtuele schijven"

    ![VirtualDiskProperties][3]

1. Het LUN-nummer is een verwijzing naar het LUN dat u opgeeft wanneer u de VHD koppelen aan de VM.
1. Voor de 'Microsoft Virtual Disk' Ga naar het tabblad **Details** , klik in de lijst **eigenschap** Ga naar **Stuurprogrammasleutel.** In de **waarde**ziet de die wordt **gecompenseerd**, die is 0002 in de volgende schermafdruk. De 0002 geeft het PhysicalDisk2 dat wordt verwezen naar de opslag van toepassingen.

    ![VirtualDiskPropertyDetails][4]

2. Voor elke groep, dump van de gekoppelde schijven:

    Get StoragePool FriendlyName - AMS1pooldata | Get-fysieke schijf

    ![GetStoragePool][5]

Nu kunt u deze gegevens koppelen VHD's gekoppeld aan fysieke schijven in opslagpools.

Als u VHD's hebt toegewezen aan fysieke schijven in opslagpools vervolgens los en via kopiëren naar een rekening van de premie opslag, koppelt u deze met de juiste cache-instelling. Zie het voorbeeld in het [aanhangsel](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)stap 8 tot en met 12. Deze stappen laten zien hoe een schijfconfiguratie VHD VM gekoppeld naar een CSV-bestand uitpakken, de VHD's kopiëren, wijzigen van de schijf-cache-instellingen en ten slotte de VM implementeren als DS serie VM met alle aangesloten schijven.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>VM opslag bandbreedte en doorvoer van VHD-opslag

Het bedrag van de opslagcapaciteit is afhankelijk van de opgegeven DS * VM-grootte en de grootte van de VHD. Het VMs hebben verschillende vergoedingen voor het aantal VHD's die kunnen worden gekoppeld en de maximale bandbreedte (in MB/s) wordt ondersteund. Zie [virtuele Machine en Cloud Service formaten voor Azure](virtual-machines-linux-sizes.md)voor de nummers specifieke bandbreedte.

Verhoogde IOP's bereikt u met een grotere schijf. Dit moet u overwegen wanneer u over de te volgen migratieroute denkt. Voor meer informatie, [Zie de tabel voor IOP's en het type](../storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

Ten slotte kunt u overwegen dat VMs hebben andere schijf voor maximale bandbreedten die worden ondersteund voor alle schijven die zijn aangesloten. U kunt de schijf met maximale bandbreedte beschikbaar voor die rol VM grootte kan verzadigen onder hoge belasting. Bijvoorbeeld ondersteunt een Standard_DS14 maximaal 512 MB/s; Daarom kan u de bandbreedte van de schijf van de VM verzadigen met drie schijven van P30. Maar in dit voorbeeld wordt de maximale doorvoer afhankelijk van de combinatie van lees- en schrijfmachtigingen IOs kan worden overschreden.

## <a name="new-deployments"></a>Nieuwe installaties

De volgende twee secties laten zien hoe u SQL Server VMs kunt implementeren op Premium-opslag. Zoals eerder hoeft niet noodzakelijkerwijs te plaatst u de cd-rom van het besturingssysteem naar Premium-opslag. U kunt dit doen als u een intensieve i/o-belasting op de VHD OS plaatsen zijn voornemen.

Het eerste voorbeeld beschrijft het gebruik van bestaande Azure-afbeeldingen. Het tweede voorbeeld ziet hoe u een aangepaste VM-afbeelding die u in een bestaande account in de standaard opslag hebt.

> [AZURE.NOTE] In deze voorbeelden wordt ervan uitgegaan dat u al een regionale VNET hebt gemaakt.

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Maak een nieuwe VM met Premium-opslagruimte met afbeelding

In het volgende voorbeeld ziet u hoe de VHD OS naar opslag premie en premie opslag VHD's koppelen. U kunt echter ook de OS schijf plaatsen in een standaardopslag-account en voeg vervolgens VHD's die zich op een rekening van de premie opslag bevinden. Beide scenario's worden aangetoond.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>Stap 1: Maak een Account premie opslag


    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>Stap 2: Maak een nieuwe wolk Service

    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Stap 3: Reserveren een Cloud Service VIP (optioneel)
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>Stap 4: Maak een VM-Container
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Stap 5: OS VHD op Standard of Premium opslag plaatsen
    #NOTE: Set up subscription and default storage account which will be used to place the OS VHD in

    #If you want to place the OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>Stap 6: VM maken
    #Get list of available SQL Server Images from the Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember to change to DS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks to VM Config
    #Note the size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising the Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Maak een nieuwe VM om Premium-opslag met een aangepaste afbeelding gebruiken

Dit scenario laat zien waar u een bestaande aangepaste images die zich in een standaardopslag-account bevinden hebt. Zoals vermeld als u wilt plaatsen de VHD OS op Premium opslag moet u Kopieer de afbeelding die bestaat in de standaardopslag-account en deze overbrengen naar een premie opslag voordat deze kan worden gebruikt. Als u een afbeelding op de vooronderstelling, kun je deze methode die rechtstreeks naar de rekening van de premie opslag te kopiëren.

#### <a name="step-1-create-storage-account"></a>Stap 1: Maak een Account voor opslag
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>Stap 2 Cloud-Service maken
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>Stap 3: Gebruik een bestaande afbeelding
U kunt een bestaande afbeelding. Of u kunt [een afbeelding maken van een bestaande machine](virtual-machines-windows-classic-capture-image.md). Let op de machine u afbeelding niet hoeft te worden* DS machine. Als u de afbeelding hebt, de volgende stappen laten zien hoe om het te kopiëren naar de rekening van de premie opslag met de * *Start AzureStorageBlobCopy** PowerShell-commandlet.

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for the storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Stap 4: Kopie Blob tussen rekeningen opslag
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>Stap 5: Controleer regelmatig de status van de kopie:
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>Stap 6: Afbeelding schijf naar Azure schijf opslagplaats in abonnement toevoegen
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [AZURE.NOTE] Merkt u dat, hoewel de statusrapporten als succes, u kan nog steeds een fout van de lease-overeenkomst. In dat geval wacht ongeveer 10 minuten.

#### <a name="step-7--build-the-vm"></a>Stap 7: De VM Build
U maakt hier de VM van uw image en twee Premium opslag VHD's koppelen:

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need to be a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use to DS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Bestaande installaties die niet altijd op beschikbaarheidsgroepen

> [AZURE.NOTE] Voor bestaande installaties, ziet u eerst de sectie [vereisten](#prerequisites-for-premium-storage) van dit onderwerp.

Er zijn verschillende overwegingen die voor de SQL Server-implementaties die niet altijd op beschikbaarheidsgroepen en degene die dit doen. Als u niet altijd op en hebt een bestaande SQL Server-zelfstandige, kunt u upgraden naar Premium opslag met behulp van een nieuwe wolk service- en account. Houd rekening met de volgende opties:

- **Een nieuwe SQL Server-VM maken**. Zoals beschreven in de nieuwe toepassingen, kunt u een nieuwe SQL Server VM die gebruikmaakt van een account premie opslag maken. Vervolgens een back-up en herstel van uw SQL Server-databases van configuratie- en gebruikersbestanden. De toepassing moet worden bijgewerkt als u verwijst naar de nieuwe SQL Server als intern of extern wordt geopend. U moet alle 'van db' objecten kopiëren als u een migratie naast elkaar (SxS) SQL Server aan het doen was. Dit omvat objecten zoals aanmeldingen, certificaten en gekoppelde servers.
- **Migreren van een bestaande SQL Server-VM**. Dit houdt in dat de VM SQL Server off line nemen en vervolgens overdragen aan een nieuwe wolk service, inclusief alle bijbehorende gekoppelde VHD's kopiëren naar de rekening van de premie opslag. Wanneer de VM online komt, wordt de hostnaam als voordat u verwijzen naar de toepassing. Let erop dat de grootte van de bestaande schijf is van invloed op de prestatie-eigenschappen. Bijvoorbeeld een schijf van 400 GB wordt naar boven afgerond op een P20. Als u dat schijfprestaties niet nodig te hebben weet, kan u opnieuw de VM als een VM DS serie, en koppelen van de premie opslag VHD's van de grootte-prestatie-specificatie vereist. U kunt vervolgens loskoppelen en weer aan de SQL DB-bestanden.

> [AZURE.NOTE] Bij het kopiëren van de VHD schijven u moet rekening houden met de grootte, afhankelijk van de grootte betekent dat type opslagschijf Premium worden ingedeeld in, dit schijf prestaties specificatie bepaalt. Azure worden afgerond tot het dichtstbijzijnde schijf grootte, zodat als u een schijf van 400 GB, dit zal worden naar boven afgerond op een P20. Afhankelijk van uw bestaande i/o-vereisten van de VHD OS moet u mogelijk niet dit migreren naar een opslag van Premium-account.

Als uw SQL Server extern wordt geopend, wordt de VIP cloud-service wijzigen. U ook moet update eindpunten, ACL's en DNS-instellingen.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Bestaande installaties die altijd op beschikbaarheidsgroepen gebruiken

> [AZURE.NOTE] Voor bestaande installaties, ziet u eerst de sectie [vereisten](#prerequisites-for-premium-storage) van dit onderwerp.

In eerste instantie in deze sectie gaan we altijd op de interactie met Azure Networking. Wij zullen vervolgens migraties in twee scenario's opsplitsen: waar sommige uitvaltijd kan worden verdragen migraties en migraties waarbij minimale uitvaltijd moeten worden gerealiseerd.

Over lokalen SQL Server altijd op beschikbaarheidsgroepen gebruiken een Listener op-ruimten die registreert een virtuele DNS-naam en IP-adres wordt gedeeld door een of meer SQL-Servers. Wanneer clients verbinding worden verzonden in het onderzoektijdvak listener voor de primaire SQL-Server. Dit is de server die eigenaar is van de altijd op IP-bron op dat moment.

![DeploymentsUseAlways op][6]

In Microsoft Azure is er slechts één IP-adres is toegewezen aan een Netwerkadapter op de VM, dus met het oog op dezelfde laag van abstractie als in de lokalen, Azure maakt gebruik van het IP-adres dat is toegewezen aan de interne of externe netwerktaakverdeling (ILB/ELB). De IP-bron die wordt gedeeld tussen de servers is ingesteld op de hetzelfde IP-adres als het ILB/ELB. Dit is gepubliceerd in de DNS-server en clientverkeer via het ILB/ELB wordt doorgegeven aan de primaire SQL Server-replica. Het ILB/ELB weet welke SQL Server primair is sinds sondes wordt gebruikt om de bron altijd op IP-probe. In het vorige voorbeeld, dat elk knooppunt een eindpunt waarnaar wordt verwezen door de ELB/ILB sondes, indien deze reageert, is de primaire SQL-Server.

> [AZURE.NOTE] Het ILB en ELB zijn beide toegewezen aan een bepaalde Azure cloud-service, dus alle cloud migratie in Azure betekent waarschijnlijk dat de Load Balancer IP verandert.

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Migreren altijd op implementaties waardoor sommige uitvaltijd

Er zijn twee strategieën altijd op implementaties waardoor sommige uitvaltijd te migreren:

1. **Meer secundaire replica's toevoegen aan een bestaand altijd op Cluster**
1. **Migreren naar een nieuwe altijd op Cluster**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1. meer secundaire replica's aan een bestaand altijd op Cluster toevoegen

Een strategie is meer secundaire servers toevoegen aan de altijd op beschikbaarheid-groep. Moet u deze in een nieuwe wolk service toevoegen en de listener bijwerken met de nieuwe load balancer IP.

##### <a name="points-of-downtime"></a>Punten van uitvaltijd:

- Validatie van het cluster.
- Altijd op failover testen voor nieuwe secundaire servers.

Als u Windows opslagpools binnen de VM voor hogere i/o-doorvoer, en deze zullen worden off line tijdens een volledige validatie van de Cluster genomen. Het testen van de validatie is vereist als u knooppunten aan het cluster toevoegen. De tijd die nodig is voor het uitvoeren van de test kan variëren, zodat u deze in uw testomgeving representatieve om bij benadering de tijd van hoe lang dit duurt moet testen.

U moet inrichten tijd waar u handmatige failover en chaos tests op de zojuist toegevoegde knooppunten uitvoeren kunt zodat u altijd over hoge beschikbaarheid-functies zoals verwacht.

![DeploymentUseAlways On2][7]

> [AZURE.NOTE] Alle exemplaren van SQL Server waar de opslag van toepassingen worden gebruikt moet worden gestopt voordat de validatie wordt uitgevoerd.
##### <a name="high-level-steps"></a>Stappen op hoog niveau

1. Maak twee nieuwe SQL-Servers in nieuwe wolk service met aangesloten Premium-opslagruimte.
1. Kopiëren via een volledige back-ups en herstel met **NORECOVERY**.
1. Kopiëren via 'van gebruiker DB' afhankelijke objecten, zoals aanmeldingen enz.
1. Een nieuwe interne Load Balancer (ILB) nieuw of gebruikt een externe Load Balancer (ELB) en Load Balanced eindpunten vervolgens instellen op beide nieuwe knooppunten.
> [AZURE.NOTE] Controleer dat alle knooppunten hebben de juiste Endpoint-configuratie voordat u doorgaat

1. Gebruiker/toepassing toegang tot de SQL-Server stoppen (als opslagpools).
1. Stop SQL Server Engine Services op alle knooppunten (als opslagpools).
1. Voeg nieuwe knooppunten in het cluster en volledige validatie uitgevoerd.
1. Als de validatie succesvol is, start alle Services van SQL Server.
1. Back-up van transactielogboeken, en de gebruiker-database terugzetten.
1. Nieuwe knooppunten in de altijd op beschikbaarheid-groep toevoegen en replicatie in een **synchrone**plaats.
1. De IP-adresbron van het nieuwe wolk Service ILB/ELB via PowerShell voor altijd aan de hand van het voorbeeld met meerdere locaties in het [aanhangsel](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)toevoegen. In Windows clustering, de **mogelijke eigenaars** van de bron **-IP-adres** ingesteld op de oude nieuwe knooppunten. Zie de sectie ' Toe te voegen IP-adresbron op hetzelfde Subnet' van het [aanhangsel](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).
1. Failover naar een nieuw knooppunt.
1. Controleer de nieuwe knooppunten automatische failover-Partners en test de failover.
1. Knooppunten uit beschikbaarheid groep verwijderen.

##### <a name="advantages"></a>Voordelen

- Nieuwe SQL-Servers kan worden getest (SQL Server en Application) voordat ze worden toegevoegd aan altijd aan.
- U kunt de VM-grootte wijzigen en de opslag aanpassen aan uw exacte behoeften. Het zou evenwel nuttig om behouden alle bestandspaden van SQL.
- U kunt bepalen wanneer de overdracht van de DB back-ups op de secundaire replica's worden gestart. Dit verschilt van kopiëren met behulp van Azure **Start AzureStorageBlobCopy** commandlet VHD's, want dat is een asynchrone kopiëren.

##### <a name="disadvantages"></a>Nadelen
- Wanneer Windows opslagpools gebruikt, is de Cluster uitvaltijd tijdens de validatie van het volledige Cluster voor de nieuwe extra knooppunten.
- Afhankelijk van de versie van SQL Server en het bestaande aantal secundaire replica's, u mogelijk niet meer secundaire replica's toevoegen zonder bestaande secundaire servers.
- Het is mogelijk SQL data transfer lang tijdens het instellen van de secundaire servers.
- Er is extra kosten tijdens de migratie als u nieuwe computers waarop een parallel.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. migreren naar een nieuwe altijd op Cluster

Een andere strategie is een gloednieuwe altijd op Cluster met nieuwe knooppunten maken in de nieuwe wolk service en leid de clients om het te gebruiken.

##### <a name="points-of-downtime"></a>Punten van uitvaltijd

Er is uitvaltijd wanneer u toepassingen en gebruikers overbrengen naar de nieuwe listener altijd op. Afhankelijk van de uitvaltijd:

- Tijd laatste transactielogboekback-ups herstellen van databases op de nieuwe servers.
- De tijd voor het bijwerken van clienttoepassingen nieuwe listener altijd in gebruik genomen.

##### <a name="advantages"></a>Voordelen

- U kunt de werkelijke productie-omgeving, SQL Server, maken en testen van OS wijzigingen.
- U hebt de optie voor het aanpassen van de opslag en verkleint de grootte van de VM. Dit kan leiden tot vermindering van de kosten.
- Tijdens dit proces kunt u uw versie van SQL Server of een versie bijwerken. U kunt ook het besturingssysteem upgraden.
- Het vorige altijd op Cluster kan fungeren als een doel effen terugdraaien.

##### <a name="disadvantages"></a>Nadelen

- U moet de DNS-naam van de listener wijzigen als u wilt dat beide altijd op-clusters die gelijktijdig wordt uitgevoerd. Hiermee voegt u beheer tijdens de migratie overhead als tekenreeksen voor client-toepassingen met de nieuwe naam van de Listener overeenkomen moeten.
- Een mechanisme voor synchronisatie tussen de twee omgevingen te houden ze zo dicht mogelijk tot een minimum beperken van de uiteindelijke synchronisatie-vereisten voor de migratie moet worden geïmplementeerd.
- Er wordt toegevoegd kosten tijdens de migratie als u de nieuwe omgeving uitgevoerd.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Migreren altijd op implementaties voor minimale uitvaltijd

Er zijn twee strategieën voor migratie implementaties altijd voor een minimale uitvaltijd:

1. **Gebruik een bestaande secundaire: één Site**
1. **Gebruik van bestaande secundaire Replica(s): meerdere locaties**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. gebruik van een bestaande secundaire: één Site

Een strategie voor een minimale uitvaltijd is een wolk bestaande secundaire en verwijderen uit de huidige cloud-service. Vervolgens de VHD's kopiëren naar de nieuwe account voor de opslag van de premie en de VM maken in de nieuwe wolk service. Werk vervolgens de listener in clusters en overname bij storingen.

##### <a name="points-of-downtime"></a>Punten van uitvaltijd

- Uitvaltijd is wanneer u het laatste knooppunt met het eindpunt taakverdeling bijwerkt.
- De client opnieuw verbinden kan afhankelijk van de configuratie van de client en DNS worden uitgesteld.
- Er is extra uitvaltijd als u ervoor kiest om de groep altijd op de Cluster off line te verwisselen van de IP-adressen. U kunt dit voorkomen door een afhankelijkheid of en mogelijke eigenaars voor de extra IP-adresbron. Zie de sectie ' Toe te voegen IP-adresbron op hetzelfde Subnet' van het [aanhangsel](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).

> [AZURE.NOTE] Als u wilt dat het toegevoegde knooppunt voor partake in als altijd op failover-Partner, moet u een eindpunt Azure met een verwijzing naar de belasting in evenwicht instellen toevoegen. Bij het uitvoeren van de opdracht **Add-AzureEndpoint** om dit te doen kunnen actieve verbindingen blijft open, maar nieuwe verbindingen met de listener worden niet tot stand worden gebracht totdat de taakverdeling is bijgewerkt. In dit testen is gezien naar de laatste 90-120seconds, dit moet worden getest.

##### <a name="advantages"></a>Voordelen

- Zonder extra kosten tijdens de migratie.
- Een een-op-migratie.
- Minder complexiteit.
- Verhoogde IOP's staat van de premie opslag SKU's. Wanneer de schijven losgekoppeld van de VM worden en gekopieerd naar de nieuwe cloud-service een 3de partij kan verhogen het VHD-formaat waarmee hogere doorvoercapaciteit hulpprogramma worden gebruikt. Zie deze [discussie forum](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows)voor het verhogen van VHD-formaat.

##### <a name="disadvantages"></a>Nadelen

- Er is een tijdelijk verlies van HA en DR tijdens de migratie.
- Aangezien dit een 1:1-migratie, moet u een minimale grootte van VM die uw aantal VHD's, ondersteunt zodat u mogelijk niet uw VMs afslanken.
- In dit scenario gebruikt de commandlet Azure **Start AzureStorageBlobCopy** die asynchroon is. Er is geen SLA op kopiëren is voltooid. De tijd van de kopieën varieert, terwijl dit wachten in een wachtrij hangt, die deze afhankelijk van de hoeveelheid gegevens is wordt overbrengen. De tijd wordt verhoogd als de overdracht naar een andere Azure gegevenscentrum Premium opslag in een andere regio gaat. Hebt u slechts 2 knooppunten, kunt u een mogelijke oplossing voor het geval de kopie langer dan in het testen duurt. Het kan hierbij de volgende ideeën.
    - Een tijdelijk 3e SQL Server-knooppunt voor HA toevoegen vóór de migratie met een overeengekomen uitval.
    - De migratie buiten Azure gepland onderhoud worden uitgevoerd.
    - Controleer dat het clusterquorum correct is geconfigureerd.  

##### <a name="high-level-steps"></a>Stappen op hoog niveau

Dit document is niet aantonen dat een volledig voorbeeld van begin tot eind, maar het [aanhangsel](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) details die kunnen worden gebruikt bevat voor het uitvoeren van dit.

![MinimalDowntime][8]

- Verzamelen van de schijfconfiguratie en het knooppunt verwijderen (gekoppelde VHD's niet verwijderd).
- Maak een account premie opslag en VHD's kopiëren van de standaardopslag-account
- Nieuwe wolk service maken en implementeren van de VM SQL2 in die cloud-service. Maak de VM met behulp van de gekopieerde oorspronkelijke OS VHD en de gekopieerde VHD's koppelen.
- ILB configureren / ELB en eindpunten toevoegen.
- Listener een update:
    - De altijd op groep off line nemen en de altijd op Listener wordt bijgewerkt met nieuwe ILB / ELB IP-adres.
    - Of het toevoegen van de IP-adresbron van nieuwe wolk Service ILB/ELB via PowerShell in Windows clustering. Vervolgens stelt u de mogelijke eigenaars van de bron-IP-adres op de gemigreerde knooppunt, SQL2, en dit als of afhankelijkheid in de naam van het netwerk ingesteld. Zie de sectie ' Toe te voegen IP-adresbron op hetzelfde Subnet' van het [aanhangsel](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage).
- Controleer de DNS-configuratie/doorgiftetaak aan de clients.
- SQL1 VM migreren en doorloop de stappen 2 tot en met 4.
- Als u stappen 5ii, voegt u SQL1 als mogelijke eigenaar voor de extra IP-adresbron
- Test de failover.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. gebruikmaken van bestaande secundaire replica(s): meerdere locaties

Als u knooppunten in meer dan één Azure datacenter (DC hebt) of als u een hybride omgeving hebt, kunt vervolgens u een configuratie altijd op in deze omgeving om uitvaltijd te minimaliseren.

De aanpak is het altijd op synchronisatie op synchrone voor de in de lokalen of secundaire Azure DC en failover via SQL Server. Vervolgens de VHD's kopiëren naar een rekening van de premie opslag en de machine opnieuw in een nieuwe wolk service. De listener bijwerken en vervolgens niet terug.

##### <a name="points-of-downtime"></a>Punten van uitvaltijd

De uitvaltijd bestaat uit de tijd voor failover naar de alternatieve DC en terug. Ook afhankelijk van de client en DNS-configuratie en de client opnieuw verbinden kan worden vertraagd.
Houd rekening met het volgende voorbeeld van een hybride altijd op configuratie:

![MultiSite1][9]

##### <a name="advantages"></a>Voordelen

- U kunt gebruikmaken van bestaande infrastructuur.
- U hebt de optie voor het vooraf de Azure opslag op de DC DR Azure eerst upgraden.
- De DR Azure DC-opslag kan opnieuw worden geconfigureerd.
- Er is een minimum van twee failover tijdens de migratie, met uitzondering van failover van de test.
- U hoeft niet verplaatsen van de SQL Server-gegevens met back-up en terugzetten.

##### <a name="disadvantages"></a>Nadelen

- Afhankelijk van de clienttoegang tot SQL Server, kan er wachttijd wanneer SQL Server wordt uitgevoerd in een alternatieve DC naar de toepassing.
- Kopie van VHD's Premium opslag tijd kan lang zijn. Dit kan invloed op uw beslissing over de vraag of het knooppunt in de groep beschikbaar te houden. Overweeg dit als logboek intensieve werk wordt geladen tijdens de migratie wordt uitgevoerd is vereist, omdat het primaire knooppunt moet houden van de niet-gerepliceerde transacties in het transactielogboek. Dus kan dit aanzienlijk groeien.
- In dit scenario gebruikt de commandlet Azure **Start AzureStorageBlobCopy** die asynchroon is. Er is geen SLA na voltooiing. De tijd van de kopieën varieert, terwijl deze wachten, in wachtrij, afhankelijk van het ook afhankelijk van de hoeveelheid gegevens overbrengen. Dus u hoeft alleen een van de knooppunten in de 2e Datacenter, u moet treffen risicobeperking in het geval de kopie langer duurt dan in het testen. Het kan hierbij de volgende ideeën.
    - Een tijdelijke 2e SQL-knooppunt voor HA toevoegen vóór de migratie met een overeengekomen uitval.
    - De migratie buiten Azure gepland onderhoud worden uitgevoerd.
    - Controleer dat het clusterquorum correct is geconfigureerd.

In dit scenario wordt ervan uitgegaan dat u de installatie hebt gedocumenteerd en weten hoe de opslag is toegewezen om het te wijzigen voor optimale schijf cache-instellingen.

##### <a name="high-level-steps"></a>Stappen op hoog niveau
![Multisite2][10]

- De lokalen op / alternatieve Azure DC de primaire SQL-Server, en kunnen de andere automatische failover-Partner (AFP).
- Verzamelen van gegevens over de schijfconfiguratie van SQL2 en het knooppunt verwijderen (gekoppelde VHD's niet verwijderd).
- Maak een account premie opslag en VHD's kopiëren van de standaardopslag-account.
- Een nieuwe wolk service maken en de VM SQL2 maken met de premies schijven gekoppeld.
- ILB configureren / ELB en eindpunten toevoegen.
- De altijd op Listener bijwerken met nieuwe ILB / ELB IP-adres- en failover.
- Controleer de DNS-configuratie.
- De AFP wijzigen in SQL2, en SQL1 migreren en doorloop de stappen 2 tot en met 5.
- Test de failover.
- De AFP overschakelen naar SQL1 en SQL2

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>Aanhangsel: Een meerdere locaties altijd op een Cluster migreren naar Premium-opslag

De rest van dit onderwerp biedt een gedetailleerd voorbeeld van een cluster met meerdere locaties altijd converteren naar opslag Premium. Ook geconverteerd naar interne taakverdeling (ILB) de Listener uit met behulp van een externe-taakverdeling (ELB).

### <a name="environment"></a>Omgeving

- Windows 2k 12 / SQL 2k 12
- Bestanden op SP 1 DB
- 2 x opslagpools per knooppunt

![Appendix1][11]

### <a name="vm"></a>VM:

In dit voorbeeld gaan we verplaatsen van een ELB naar ILB aantonen. ELB is beschikbaar voor het ILB, zodat dit het overschakelen naar deze tijdens de migratie worden weergegeven.

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a>Pre-stappen: Verbinding maken met abonnement

    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Stap 1: Maak nieuwe Account voor opslag en Cloud Service
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where the vm to migrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>Stap 2: De toegestane storingen in bronnen verhogen<Optional>
Op bepaalde resources die bij uw altijd op beschikbaarheid-groep horen zijn beperkingen op het aantal storingen die in een periode waarin de cluster-service probeert optreden kunnen te starten, de resourcegroep. Het wordt aanbevolen dat u deze terwijl u via deze procedure worden lopen sinds als u dit niet handmatig failover en trigger failovers door het afsluiten van computers kun je dicht bij deze limiet verhogen.

Zou zij behoedzaam te verdubbelen de toelage storing hiervoor in Failover-clusterbeheer, Ga naar de eigenschappen van de bronnengroep altijd:

![Appendix3][13]

Wijzig de maximale storingen 6.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Stap 3: Toevoeging IP-adresbron van het Cluster Group<Optional>

Als er slechts één IP-adres voor de clustergroep en dit wordt uitgelijnd op het subnet van de wolk, pas op, als u per ongeluk offline alle clusterknooppunten in de cloud op dat netwerk vervolgens door de Cluster-IP-bron halen en de clusternetwerknaam worden niet on line worden gebracht. In dit geval kan updates voor andere clusterbronnen.

#### <a name="step-4-dns-configuration"></a>Stap 4: DNS-configuratie

Voor het implementeren van een vloeiende overgang afhankelijk is van de manier waarop DNS wordt gebruikt en bijgewerkt.
Wanneer altijd op is geïnstalleerd, wordt een groep Windows Cluster Resource als u Failoverclusterbeheer openen, ziet u dat ten minste drie middelen beschikken, zijn de twee waarnaar het document verwijst:

- Virtuele netwerknaam (VNN) – Dit is de DNS-naam die client verbinding maakt wanneer de verbinding met SQL-Servers via altijd op willen.
- IP-adresbron – dit is het IP-adres dat is gekoppeld aan de VNN, kunt u meer dan één hebben en in een configuratie met meerdere locaties hebt u een IP-adres per site-en subnet.

Bij het maken van verbinding met SQL Server, SQL Server Client stuurprogramma wordt de DNS-records die zijn gekoppeld aan de listener halen en maak verbinding met elke altijd op gekoppelde IP-adres, bespreken hieronder we enkele factoren die dit kunnen beïnvloeden.

Het aantal gelijktijdige DNS-records die gekoppeld aan de naam van de listener zijn hangt niet alleen het aantal IP-adressen die zijn gekoppeld, maar de ' RegisterAllIpProviders'setting in failover-clusters voor de resource altijd op VNN.

Als u altijd op in Azure er zijn verschillende stappen voor het maken van Listener en IP-adressen u moet handmatig configureren 'RegisterAllIpProviders' 1, dit is niet een on premise altijd op implementatie waar al ingesteld op 1.

Als 'RegisterAllIpProviders' is ingesteld op 0, dan ziet u alleen DNS-record in DNS die is gekoppeld aan de Listener:

![Appendix4][14]

Als 'RegisterAllIpProviders' 1 is:

![Appendix5][15]

De volgende code wordt dump van de VNN-instellingen en voor u ingesteld, opmerking om de wijziging te activeren, u moet de VNN off line nemen en weer on line te schakelen deze nemen de Listener offline veroorzaakt door verstoring van de client connectivity.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

In een volgende migratie stap moet u de listener altijd bijwerken met een bijgewerkte IP-adres dat verwijst naar een taakverdeling, dient een IP-adres resource verwijderen en toevoegen. Nadat het IP-update moet u ervoor zorgen het nieuwe IP-adres is bijgewerkt in DNS-Zone en dat de clients hun lokale DNS-cache worden bijgewerkt.

Als uw clients bevinden zich in een ander netwerksegment en verwijst naar een andere DNS-server, moet u nagaan wat er gebeurt over een DNS-Zone Transfer tijdens de migratie, zoals de toepassing opnieuw tijd zal worden beperkt door ten minste de Zone Transfer tijd van het nieuwe IP-adressen voor de listener. Als u hier tijd beperking, te bespreken en testen van een incrementele zoneoverdracht met uw teams Windows forceren en ook plaatsen als u de DNS-hostrecord aan een lagere Time To Live (TTL), zodat de clients bijwerken. Zie voor meer informatie, [Incrementele zoneoverdrachten](https://technet.microsoft.com/library/cc958973.aspx) en [Begin DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).

De standaard TTL-waarde voor DNS-Record die is gekoppeld aan de Listener in altijd op in Azure is 1200 seconden. U kunt dit verlagen als u onder tijd beperking tijdens de migratie om ervoor te zorgen de clients hun DNS bijwerken met de bijgewerkte IP-adres voor de listener. U kunt zien en de configuratie wijzigen door de configuratie van de VNN storten:

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

Houd er rekening mee, hoe lager het 'HostRecordTTL', een hoger bedrag van DNS-verkeer wordt uitgevoerd.

##### <a name="client-application-settings"></a>Instellingen voor client-toepassing

Als de clienttoepassing SQL de .net 4.5 ondersteunt SQLClient en vervolgens kunt gebruiken "MULTISUBNETFAILOVER = TRUE' trefwoord, dit wordt aanbevolen om sneller verbinding met SQL altijd op beschikbaarheid groep tijdens failover kan worden toegepast. Het inventariseren door alle IP-adressen die zijn gekoppeld aan de listener altijd op parallel en een meer agressieve TCP-verbinding opnieuw snelheid tijdens een failover uitvoert.

Voor meer informatie over de bovenstaande instellingen Zie [MultiSubnetFailover trefwoord en de bijbehorende functies](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Zie ook [Ondersteuning voor hoge beschikbaarheid, noodherstel SqlClient](https://msdn.microsoft.com/library/hh205662(v=vs.110).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>Stap 5: Cluster quorum instellingen

Als u gaat te hebben van ten minste één SQL Server omlaag tegelijk, moet u het cluster quorum-instelling wijzigen als bestand delen Witness (FSW) met 2 knooppunten, het quorum Knooppuntmeerderheid en gebruikmaken van dynamische stemmingen in te stellen, en dit voor één knooppunt te blijven staan.


    Set-ClusterQuorum -NodeMajority  

Zie [het Quorum in een failover-Cluster van Windows Server 2012 beheren en configureren](https://technet.microsoft.com/library/jj612870.aspx)voor meer informatie over het beheren en configureren van het clusterquorum.

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Stap 6: Bestaande eindpunten en ACL's te halen
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for the Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Sla deze naar een tekstbestand.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>Stap 7: Partners Failover en replicatie-modus wijzigen

Als u meer dan 2 SQL-Servers hebt, u moet wijzigen in de overname van een andere secundaire in een andere domeincontroller of op de lokale 'Synchroon' en een automatische failover-Partner (AFP), is dit zodat u HA onderhouden terwijl u wijzigingen aanbrengt. U kunt dit doen via TSQL van Hoewel SSMS wijzigen:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>Stap 8: Secundaire VM uit cloud-service verwijderen

U moet eerst een wolk secundaire knooppunt migreren worden planning als dit momenteel primaire, kunt u een handmatige failover moet starten.

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns the disks associated with the VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Stap 9: Instellingen in het CSV-bestand in de schijfcache wijzigen en opslaan

Voor gegevensvolumes moeten deze worden ingesteld op alleen-lezen.

Voor TLOG volumes moeten deze ingesteld op geen.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>Stap 10: Kopie VHD 's
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



U kunt de status van het exemplaar van de VHD naar de rekening van de premie opslag controleren:

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

Wacht totdat deze zijn vastgelegd als een succes.

Voor informatie over afzonderlijke BLOB's:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>Stap 11: Register OS schijf

    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Stap 12: Secundaire importeren in nieuwe wolk service

De onderstaande code gebruikt ook de toegevoegde optie hier kunt u de machine importeren en gebruiken de VIP retainable.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember to change to XIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Stap 13: ILB op maken nieuwe Cloud Svc toevoegen Load Balanced eindpunten en ACL's
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

####<a name="step-14-update-always-on"></a>Stap 14: Werken altijd op
    #Code to be executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Nu verwijderen de oude IP-adres van de cloud-service.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>Stap 15: DNS-update controle

Nu moet u controleren van DNS-Servers op uw SQL Server client-netwerken en controleer of clustering heeft toegevoegd de extra hostrecord voor de extra IP-adres. Als deze DNS-servers niet hebt bijgewerkt, kunt u automatisch een DNS-zoneoverdracht en ervoor te zorgen dat de clients in er subnet kunnen worden omgezet in beide altijd op IP-adressen, dit is dus u hoeft niet te wachten op automatische DNS-replicatie.

#### <a name="step-16-reconfigure-always-on"></a>Stap 16: Altijd opnieuw op

Op dit moment wachten op de secundaire knooppunt om volledig opnieuw te synchroniseren met het lokale knooppunt- en Ga naar het REPLICATIEKNOOPPUNT synchrone en kunnen de AFP is gemigreerd.  

#### <a name="step-17-migrate-second-node"></a>Stap 17: Tweede knooppunt migreren
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Stap 18: De instellingen in het CSV-bestand in de schijfcache wijzigen en opslaan

Voor gegevensvolumes moeten deze worden ingesteld op alleen-lezen.

Voor TLOG volumes moeten deze ingesteld op geen.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>Stap 19: Nieuwe onafhankelijke opslag Account voor secundaire knooppunt maken
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset the storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a>Stap 20: Kopie VHD 's
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


U kunt de status van de kopie VHD controleren voor alle VHD's: ForEach ($disk in $diskobjects) {$lun = $disk. LUN-$vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. Schijflabel $diskName = $disk. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Wacht totdat deze zijn vastgelegd als een succes.

Informatie voor individuele BLOB's: #Check induvidual blob status Get-AzureStorageBlobCopyState-Blob-'danRegSvcAms-dansqlams1-2014-07-03.vhd'-Container $containerName-Context $xioContextnode2

#### <a name="step-21-register-os-disk"></a>Stap 21: Register OS schijf
    #change storage account to the new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join to existing Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>Stap 22: Voeg Load Balanced eindpunten en ACL's
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in the Azure classic portal or Machine Endpoints through powershell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>Stap 23: Test failover

Laat nu de gemigreerde knooppunt worden gesynchroniseerd met de lokale altijd op knooppunt, plaatst u deze in op de modus voor synchrone replicatie en wacht totdat deze wordt gesynchroniseerd. Vervolgens failover uit op prem naar het eerste knooppunt gemigreerd, wordt de AFP. Zodra die heeft gewerkt, wijzigen in het laatste knooppunt van de gemigreerde de AFP.

Test de failover tussen alle knooppunten en u al chaos proeven failovers werk verwacht, en in een tijdige manor uitvoeren.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>Stap 24: Terugplaatsen cluster quorum instellingen / DNS TTL / Failover Pntrs / synchronisatie-instellingen
##### <a name="adding-ip-address-resource-on-same-subnet"></a>IP-adresbron toevoegen op hetzelfde Subnet

Als u slechts 2 SQL-Servers hebt en migreert naar een nieuwe wolk service wilt maar wilt bewaren op hetzelfde subnet, kunt u voorkomen de listener off line nemen de oorspronkelijke altijd op IP-adres verwijderen en het nieuwe IP-adres toevoegen. Als u de VMs naar een ander subnet migreert moet u niet om dit te doen omdat er een extra clusternetwerk dat verwijst naar dat subnet.

Zodra u hebt opgeroepen de gemigreerde secundaire en toegevoegd in de nieuwe bron-IP-adres voor de nieuwe cloud-service voordat u de bestaande primaire failover, moet u deze stappen binnen de failover-Clusterbeheer uitvoeren:

Zie het [aanhangsel](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), stap 14 in IP-adres toevoegen.

1. Wijzig de mogelijke eigenaar 'Bestaande primaire SQL Server' in het volgende voorbeeld zijn 'dansqlams4' voor de huidige IP-adresbron:

    ![Appendix13][23]

1. Voor de nieuwe bron-IP-adres, de mogelijke eigenaar 'Migrated secundaire SQL Server' in het volgende voorbeeld zijn 'dansqlams5' te wijzigen:

    ![Appendix14][24]

1. Als deze optie is ingesteld, kunt u failover en wanneer het laatste knooppunt wordt gemigreerd mogelijke eigenaars worden bewerkt zodat het knooppunt dat wordt toegevoegd als mogelijke eigenaar:

    ![Appendix15][25]

## <a name="additional-resources"></a>Aanvullende bronnen
- [Azure Premium opslag](../storage/storage-premium-storage.md)
- [Virtuele Machines](https://azure.microsoft.com/services/virtual-machines/)
- [SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png

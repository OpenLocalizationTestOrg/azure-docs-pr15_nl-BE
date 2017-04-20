<properties
    pageTitle="Altijd een Listener ILB voor op beschikbaarheidsgroepen configureren | Microsoft Azure"
    description="In deze zelfstudie worden gemaakt met het klassieke implementatiemodel bronnen gebruikt en maakt een altijd op beschikbaarheid groep Listener in Azure met behulp van een interne Load Balancer (ILB)."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Een listener ILB voor altijd op beschikbaarheidsgroepen configureren in Azure

> [AZURE.SELECTOR]
- [Interne Listener](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Externe Listener](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="overview"></a>Overzicht

In dit onderwerp wordt beschreven hoe u een listener voor een altijd op beschikbaarheid-groep configureren met behulp van een **Interne Load Balancer (ILB)**.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zie voor meer informatie over het configureren van een listener ILB voor een groep altijd op beschikbaarheid in model Resource Manager [configureren een interne taakverdeling voor een groep van beschikbaarheid altijd op in Azure](virtual-machines-windows-portal-sql-alwayson-int-listener.md).


Uw groep beschikbaarheid bevatten replica's op-ruimten, Azure, of zowel op locatie als Azure voor hybride configuraties omvatten. Azure replica's kunnen zich bevinden in dezelfde regio of tussen de verschillende regio's met meerdere virtuele netwerken (VNets). De volgende stappen wordt ervan uitgegaan dat u al hebt [geconfigureerd, een groep van beschikbaarheid](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , maar een listener niet hebt geconfigureerd.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Richtlijnen en beperkingen voor interne listeners
Houd rekening met de volgende richtlijnen op de beschikbaarheid van groep listener in Azure ILB gebruiken:

- De beschikbaarheid van groep listener wordt ondersteund op Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2.

- Listener van de groep slechts één interne beschikbaarheid wordt per cloud-service ondersteund, omdat de listener aan het ILB is geconfigureerd en er slechts één ILB per cloud-service is. Het is echter mogelijk te maken van meerdere externe listeners. Zie [een externe listener voor altijd op beschikbaarheidsgroepen in Azure configureren](virtual-machines-windows-classic-ps-sql-ext-listener.md)voor meer informatie.

- Het wordt niet ondersteund voor het maken van een interne listener in dezelfde cloud service waar hebt u ook een externe listener met behulp van de cloud-service openbare VIP.

## <a name="determine-the-accessibility-of-the-listener"></a>De toegankelijkheid van de listener bepalen

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Dit artikel is gericht op het maken van een listener die gebruikmaakt van een **Interne Load Balancer (ILB)**. Als u een openbare of externe listener, ziet u de versie van dit artikel stappen bevat voor het instellen van een [externe listener](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>VM eindpunten evenwichtige taakverdeling maken met directe server retour

ILB, moet u eerst de interne taakverdeling maken. Dit is in het onderstaande script.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Voor het **ILB**, moet u een statisch IP-adres toewijzen. Bekijk eerst de huidige configuratie van de VNet door de volgende opdracht uit te voeren:

        (Get-AzureVNetConfig).XMLConfiguration

1. Noteer de naam van het **subnetmasker** voor het subnet waarop het VMs die host zijn van de replica's. Deze wordt gebruikt in de parameter **$SubnetName** in het script.

1. Noteer de naam van de **VirtualNetworkSite** en de eerste **AddressPrefix** voor het subnet waarop het VMs die host zijn van de replica's. Zoek een beschikbaar IP-adres door beide waarden worden doorgegeven aan de opdracht **Test-AzureStaticVNetIP** en de **AvailableAddresses**onderzoeken. Bijvoorbeeld als de VNet is met de naam *MyVNet* en had een subnetadres bereik dat begint bij de *172.16.0.128*, zou de volgende opdracht lijst met beschikbare adressen:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses

1. Kies een van de beschikbare adressen en in de **$ILBStaticIP** -parameter van het volgende script gebruiken.

3. Onder de PowerShell-script naar een teksteditor kopiëren en stel de waarden van variabelen aanpassen aan uw omgeving (Let erop dat de standaardinstellingen zijn opgegeven voor sommige parameters). Houd er rekening mee dat bestaande installaties die gebruikmaken van affiniteit groepen ILB niet toevoegen. Zie voor meer informatie over de vereisten van de ILB, [Interne taakverdeling](../load-balancer/load-balancer-internal-overview.md). Ook als uw beschikbaarheidsgroep Azure gebieden omvat, moet u het script uitvoeren eenmaal in elk datacenter voor de knooppunten die zich in dat datacenter bevinden en de cloud-service.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the VNet
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load balanced endpoint for each node in $AGNodes using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

1. Als u de variabelen zijn ingesteld, het script kopiëren vanuit de editor in uw Azure PowerShell-sessie uit te voeren. Als het bericht nog steeds >>, typ ENTER opnieuw om te controleren of het script wordt gestart. Opmerking

>[AZURE.NOTE] De klassieke Azure portal ondersteunt niet de interne verdeling van de belasting op dit moment ziet u het ILB of de eindpunten in de klassieke Azure portal. **Get-AzureEndpoint** retourneert echter een intern IP-adres als de Load Balancer wordt uitgevoerd. Anders is het resultaat leeg.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Controleer of KB2854082 wordt indien nodig geïnstalleerd

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>De firewallpoorten openen in beschikbaarheid groepsknooppunten

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>De listener beschikbaarheid groep maken

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Voor het ILB, moet u het IP-adres van de interne Load Balancer (ILB) eerder hebt gemaakt. Gebruik het volgende script om dit IP-adres in PowerShell.

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

1. Op één van de VMs de PowerShell-script voor uw besturingssysteem kopieert in een teksteditor en de variabelen ingesteld op de waarden die u eerder hebt opgeschreven.

    Of hoger voor Windows Server 2012 kunt u het volgende script gebruiken:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
        
    Voor Windows Server 2008 R2 kunt u het volgende script gebruiken:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255
    

1. Eenmaal u de variabelen hebt ingesteld, een verhoogde Windows PowerShell-venster te openen en vervolgens het script kopiëren vanuit de teksteditor en plak in uw Azure PowerShell-sessie uit te voeren. Als het bericht nog steeds >>, typ ENTER opnieuw om te controleren of het script wordt gestart.

2. Herhaal deze stap voor elke VM. Dit script wordt het bron-IP-adres geconfigureerd met het IP-adres van de cloud-service en andere parameters ingesteld als de poort van de sonde. Wanneer de bron-IP-adres on line wordt gebracht, kan het vervolgens reageren op de polling op de poort van de sonde vanuit het eindpunt taakverdeling eerder in deze handleiding hebt gemaakt.

## <a name="bring-the-listener-online"></a>De listener on line brengen

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Follow-up artikelen

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Test de beschikbaarheid groep listener (binnen de dezelfde VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]

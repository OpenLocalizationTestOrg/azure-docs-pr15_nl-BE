<properties
    pageTitle="Configureren van een externe Listener voor altijd op beschikbaarheidsgroepen | Microsoft Azure"
    description="Deze zelfstudie doorloopt u de stappen voor het maken van een altijd op beschikbaarheid groep Listener in Azure die extern toegankelijk is via het openbare virtuele IP-adres van de bijbehorende cloud-service."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Een externe listener voor altijd op beschikbaarheidsgroepen configureren in Azure

> [AZURE.SELECTOR]
- [Interne Listener](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Externe Listener](virtual-machines-windows-classic-ps-sql-ext-listener.md)

In dit onderwerp wordt beschreven hoe u een listener voor een altijd op beschikbaarheid-groep die extern toegankelijk is op het internet te configureren. Dit wordt mogelijk gemaakt door het adres van de cloud-service **Openbare virtuele IP-(VIP)** koppelen aan de listener.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Uw groep beschikbaarheid bevatten replica's op-ruimten, Azure, of zowel op locatie als Azure voor hybride configuraties omvatten. Azure replica's kunnen zich bevinden in dezelfde regio of tussen de verschillende regio's met meerdere virtuele netwerken (VNets). De volgende stappen wordt ervan uitgegaan dat u al hebt [geconfigureerd, een groep van beschikbaarheid](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , maar een listener niet hebt geconfigureerd.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Richtlijnen en beperkingen voor externe listeners

Houd rekening met de volgende richtlijnen over de beschikbaarheid van groep listener in Azure wanneer u implementeert met behulp van de wolk symphysis VIP adres:

- De beschikbaarheid van groep listener wordt ondersteund op Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2.

- De clienttoepassing moet zich bevinden op een andere cloud-service dan met de beschikbaarheidsgroep van VMs. Azure biedt geen ondersteuning voor directe server terug met de client en server in dezelfde cloud-service.

- Standaard de stappen in dit artikel wordt aangegeven hoe een listener voor het gebruik van het adres van de virtuele IP-(VIP) wolk configureren. Het is echter mogelijk om te reserveren en meerdere VIP adressen voor uw cloud-service maken. Hiermee kunt u de stappen in dit artikel gebruiken voor het maken van meerdere listeners die gekoppeld aan een andere VIP zijn. Zie voor meer informatie over het maken van meerdere VIP-adressen, [Meerdere VIP's per cloud-service](../load-balancer/load-balancer-multivip.md).

- Als u een listener voor een hybride omgeving maakt, moet het netwerk op ruimten verbinding met het openbare Internet naast de site naar site VPN-verbinding met het virtuele netwerk van Azure hebben. Wanneer in het subnet Azure, is de listener beschikbaarheid groep door het openbare IP-adres van de respectieve cloud-service alleen bereikbaar.

- Het wordt niet ondersteund voor het maken van een externe listener in dezelfde cloud service waar u hebt ook een interne listener met behulp van de interne Load Balancer (ILB).

## <a name="determine-the-accessibility-of-the-listener"></a>De toegankelijkheid van de listener bepalen

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

In dit artikel is gericht op het maken van een listener die **externe taakverdeling**wordt gebruikt. Als u wilt dat een listener die is gebonden aan het virtuele netwerk ziet u de versie van dit artikel stappen bevat voor het instellen van een [listener met ILB](virtual-machines-windows-classic-ps-sql-int-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>VM eindpunten evenwichtige taakverdeling maken met directe server retour

Externe taakverdeling maakt gebruik van de virtuele de openbare virtuele IP-adres van de cloud-service die fungeert als host voor uw VMs. U hoeft dus niet te maken of in dit geval de taakverdeling configureren.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Onder de PowerShell-script naar een teksteditor kopiëren en stel de waarden van variabelen aanpassen aan uw omgeving (standaardwaarden zijn opgegeven voor enkele parameters). Houd er rekening mee dat als uw beschikbaarheidsgroep Azure gebieden omvat, u moet het script uitvoeren eenmaal in elk datacenter voor de knooppunten die zich in dat datacenter bevinden en de cloud-service.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas

        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

1. Als u de variabelen zijn ingesteld, het script kopiëren vanuit de editor in uw Azure PowerShell-sessie uit te voeren. Als het bericht nog steeds >>, typ ENTER opnieuw om te controleren of het script wordt gestart.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Controleer of KB2854082 wordt indien nodig geïnstalleerd

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>De firewallpoorten openen in beschikbaarheid groepsknooppunten

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>De listener beschikbaarheid groep maken

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Bij externe load balancing, moet u de openbare virtuele IP-adres van de cloud-service met de replica's. Log in op de klassieke Azure portal. Ga naar de cloud-service die uw beschikbaarheidsgroep VM bevat. Open de **Dashboard** .

3. Noteer het adres weergegeven onder **openbare virtuele IP-(VIP) adres**. Als uw oplossing VNets omvat, Herhaal deze stap voor elke service cloud met een VM die fungeert als host voor een replica.

4. Op één van de VMs onder de PowerShell-script naar een teksteditor kopiëren en de variabelen ingesteld op de waarden die u eerder hebt opgeschreven.

        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service

        Import-Module FailoverClusters

        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.

        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255


1. Eenmaal u de variabelen hebt ingesteld, een verhoogde Windows PowerShell-venster te openen en vervolgens het script kopiëren vanuit de teksteditor en plak in uw Azure PowerShell-sessie uit te voeren. Als het bericht nog steeds >>, typ ENTER opnieuw om te controleren of het script wordt gestart.

1. Herhaal deze stap voor elke VM. Dit script wordt het bron-IP-adres geconfigureerd met het IP-adres van de cloud-service en andere parameters ingesteld als de poort van de sonde. Wanneer de bron-IP-adres on line wordt gebracht, kan het vervolgens reageren op de polling op de poort van de sonde vanuit het eindpunt taakverdeling eerder in deze handleiding hebt gemaakt.

## <a name="bring-the-listener-online"></a>De listener on line brengen

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Follow-up artikelen

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Test de beschikbaarheid groep listener (binnen de dezelfde VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>De listener beschikbaarheid groep testen (via het internet)

Toegang tot de listener van buiten het virtuele netwerk, moet u extern publiek/taakverdeling (beschreven in dit onderwerp) in plaats van het ILB, die alleen toegankelijk binnen de VNet hetzelfde is. In de verbindingsreeks, kunt u de naam van de cloud-service opgeven. Bijvoorbeeld, als u had een cloud-service met de naam *mycloudservice*, zou de instructie sqlcmd zijn als volgt:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

In tegenstelling tot het vorige voorbeeld moet SQL-verificatie worden gebruikt, omdat de beller kan geen windows-verificatie via het internet gebruiken. Zie voor meer informatie [altijd op groep beschikbaarheid in Azure VM: Client Connectivity scenario's](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). Als u SQL-verificatie gebruikt, zorg ervoor dat u op beide replica's dezelfde aanmeldingsnaam maakt. Voor meer informatie over het oplossen van aanmeldingen met beschikbaarheidsgroepen zien [hoe aanmeldingen toewijzen of gebruiken bevat SQL databasegebruiker verbinding maken met andere replica's en toewijzen aan de beschikbaarheid van databases](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

Als de replica's altijd op verschillende subnetten worden, clients moeten opgeven **MultisubnetFailover = True** in de verbindingsreeks. Dit resulteert in parallelle verbindingspogingen met replica's in de verschillende subnetten. Houd er rekening mee dat dit scenario een implementatie van cross-regio altijd op groep beschikbaarheid omvat.

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]

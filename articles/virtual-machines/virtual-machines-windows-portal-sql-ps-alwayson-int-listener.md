<properties
   pageTitle="Altijd op beschikbaarheid groep luisteraars – Microsoft Azure configureren"
   description="Beschikbaarheid groep luisteraars op het model Azure Resource Manager, met behulp van een interne taakverdeling met een of meer IP-adressen configureren."
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="10/20/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Een of meer altijd op beschikbaarheid groep luisteraars - Resource Manager configureren 

Dit onderwerp wordt beschreven hoe u twee dingen doen:

- Maak een interne taakverdeling voor SQL Server beschikbaarheidsgroepen met PowerShell-cmdlets.

- Extra IP-adressen toevoegen aan een load balancer ter ondersteuning van meer dan één groep voor SQL Server-beschikbaarheid. 

In SQL Server is een groep beschikbaarheid listener een virtuele-netwerknaam waarmee clients verbinding maken met toegang tot een database in de primaire of secundaire replica. Op Azure virtuele machines bevat een load balancer het IP-adres voor de listener. De taakverdeling verkeer routeert naar het exemplaar van SQL Server die op de poort van de sonde luistert. In de meeste gevallen wordt een beschikbaarheidsgroep voor de van een interne taakverdeling. Een Azure interne taakverdeling kan host een of meer IP-adressen. Elk IP-adres gebruikt een specifieke sonde poort. Dit document laat zien hoe PowerShell gebruiken voor het maken van een nieuwe taakverdeling of IP-adressen toevoegen aan een bestaande taakverdeling voor SQL Server beschikbaarheidsgroepen. 

De mogelijkheid om meerdere IP-adressen toewijzen aan een interne taakverdeling is nieuw in Azure en is alleen beschikbaar in het model van Resource Manager. Om deze taak moet u een beschikbaarheidsgroep van SQL Server wordt geïmplementeerd op Azure virtuele machines in Bronbeheer model. Zowel SQL Server virtuele machines moet behoren tot dezelfde set beschikbaarheid. De [Microsoft-sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) kunt u automatisch de beschikbaarheidsgroep maken in Azure Resource Manager. Met deze sjabloon maakt automatisch de beschikbaarheidsgroep, met inbegrip van de interne taakverdeling voor u. Als u wilt, kunt u [een groep AlwaysOn beschikbaarheid handmatig configureren](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Dit onderwerp vereist dat al uw beschikbaarheidsgroepen zijn geconfigureerd.  

Verwante onderwerpen omvatten:

- [AlwaysOn beschikbaarheidsgroepen configureren in Azure VM (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   

- [VNet-VNet-verbinding configureren met behulp van bronbeheer Azure en PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>Windows Firewall configureren

De Windows Firewall zodat SQL Server-toegang configureren. U moet de firewall configureren voor verbindingen via TCP-poorten gebruik te maken van het exemplaar van SQL Server, evenals de poort die wordt gebruikt door de sonde listener. Zie voor gedetailleerde instructies [voor toegang tot de Database-Engine Windows Firewall configureren](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Maak een binnenkomende regel voor de SQL Server-poort en de poort van de sonde.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Voorbeeldscript: Maak een interne taakverdeling met PowerShell

> [AZURE.NOTE] Als u de beschikbaarheidsgroep hebt gemaakt met de [Microsoft-sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) worden de belasting hoeft u niet om deze stap te voltooien. 

De volgende PowerShell script maakt een interne taakverdeling, configureert u de regels voor taakverdeling en een IP-adres ingesteld voor de taakverdeling. Het script wordt uitgevoerd, opent u Windows PowerShell ISE en het script in het Script-veld te plakken. Gebruik `Login-AzureRMAccount` om in te loggen met PowerShell. Als er meerdere abonnementen voor Azure, `Select-AzureRmSubscription ` voor het instellen van het abonnement. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the Front End configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the Back End configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="example-script-add-an-ip-address-to-an-existing-load-balancer-with-powershell"></a>Van voorbeeldscript: een IP-adres toevoegen aan een bestaande taakverdeling met PowerShell

Als u meer dan één groep met beschikbaarheid, gebruiken PowerShell een extra IP-adres toevoegen aan een bestaande taakverdeling. Elk IP-adres moet een eigen taakverdeling regel sonde poort en front-poort.

De front-end-poort is de poort die door toepassingen worden gebruikt voor verbinding met de SQL Server-exemplaar. IP-adressen voor de van de verschillende beschikbaarheidsgroepen kunnen front-end dezelfde poort gebruiken.

>[AZURE.NOTE] Elk IP-adres vereist voor groepen van de beschikbaarheid van SQL Server, een specifieke sonde poort. Als één IP-adres op een load balancer poort sonde 59999 gebruikt, kunnen geen andere IP-adressen op die taakverdeling sonde poort 59999 gebruiken. 

- Voor meer informatie over taakverdeling Zie grenzen **particuliere front end IP per taakverdeling** onder [Networking grenzen - Azure Resource Manager](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).

- Voor informatie over beschikbaarheid Zie groep grenzen [beperkingen (beschikbaarheidsgroepen)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Het volgende script wordt een nieuw IP-adres toegevoegd aan een bestaande taakverdeling. De variabelen voor uw omgeving werken. Het ILB gebruikt de luisterpoort voor load balancing-front-end-poort. Deze poort is de poort die door SQL Server luistert. Voor de standaardexemplaren van SQL Server is deze poort 1433. Regel voor een groep van de beschikbaarheid van de taakverdeling vereist een zwevende IP-adres (directe server return) waardoor de back-end-poort hetzelfde als de front-end-poort is.

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```



## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Configureer het cluster de load balancer IP-adres gebruiken 

De volgende stap is het configureren van de listener op het cluster en de listener on line brengen. U kunt dit doen door het volgende te doen: 

1. De beschikbaarheid van groep listener op het failover-cluster maken  

2. De listener on line brengen

## <a name="1-create-the-availability-group-listener-on-the-failover-cluster"></a>1. de groep beschikbaarheid listener op het failover-cluster maken

In deze stap is een client access point toevoegen aan het failover-cluster Failover Cluster Management en vervolgens PowerShell gebruiken voor het configureren van de clusterbron luistert op de poort van de sonde. 

1. Met RDP verbinding maken met de Azure virtuele machine die fungeert als host voor de primaire replica. 

2. Failoverclusterbeheer openen.

3. Selecteer het knooppunt voor **netwerken** en noteer de naam van het cluster. Deze naam gebruiken in de `$ClusterNetworkName` variabele in het PowerShell-script.

4. Vouw de naam van het cluster en klik vervolgens op **rollen**.

5. Klik met de rechtermuisknop op de naam van de beschikbaarheid in het venster **rollen** en selecteer **Add Resource** > **Client Access Point**.

6. In het vak **naam** een naam voor deze nieuwe listener en klik tweemaal op **volgende** en klik op **Voltooien**. Worden niet weergegeven in de bron on line of listener op dit punt.
 
    De naam voor de nieuwe listener is de naam van het netwerk waarmee toepassingen verbinding maken met databases in de groep van de beschikbaarheid van SQL Server.

7. Klik op het tabblad **bronnen** en vouw de Client Access Point dat u zojuist hebt gemaakt. Klik met de rechtermuisknop op de IP-bron en klik op Eigenschappen. Noteer de naam van het IP-adres. Gebruikt u deze naam in de `$IPResourceName` variabele in het PowerShell-script.

8. Op **Statische IP-adres** onder **IP-adres** en het statische IP-adres ingesteld op hetzelfde adres dat u gebruikt bij het instellen van het IP-adres van de load balancer op de Azure portal. 

9. NetBIOS uitschakelen voor dit adres en klik op **OK**. Herhaal deze stap voor elke resource IP-als uw oplossing uit meerdere Azure VNets. 

10. De groep beschikbaarheid van SQL Server-bron afhankelijk maken van het IP-adres. Klik rechts op de bron in het cluster manager, dit is op het tabblad **bronnen** , onder **Andere bronnen**. 

11. Op het knooppunt dat momenteel fungeert als host voor de primaire replica, een verhoogde PowerShell ISE open en plak de volgende opdrachten in een nieuw script. Klik op het tabblad **afhankelijkheden** op de naam van de listener.
 
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>

    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```
 
6. Bijwerken van de variabelen en het IP-adres en poort configureren voor de nieuwe listener PowerShell script uitvoeren.

 >[AZURE.NOTE] Als uw SQL-Servers in verschillende regio's, moet u tweemaal het PowerShell script uitvoeren. De eerste keer gebruikt de naam van het cluster, cluster-IP-resourcenaam, en IP-adres van de verdeling van de eerste groep laden. Naam van het cluster, cluster-IP-resourcenaam, gebruikt u de tweede keer en IP-adres van de verdeling van de tweede groep van bronnen laden.

Het cluster heeft nu een resource beschikbaarheid groep listener.

## <a name="2-bring-the-listener-online"></a>2. de listener on line brengen

Met de beschikbaarheid van listener groepsbron geconfigureerd, u kunt de listener on line brengen zodat toepassingen verbinding met databases in de beschikbaarheidsgroep met de listener maken kunnen.

1. Ga terug naar Failoverclusterbeheer. Vouw **rollen** uit en selecteer vervolgens de groep beschikbaarheid. Klik met de rechtermuisknop op de naam van de listener en klik op **Eigenschappen**op het tabblad **bronnen** .

1. Klik op het tabblad **afhankelijkheden** . Als er meerdere resources zijn opgenomen, Controleer of de IP-adressen hebt of niet en afhankelijkheden. Klik op **OK**.

1. Klik met de rechtermuisknop op de naam van de listener en klik op **On line brengen**.

1. Zodra de listener online, via het tabblad **bronnen is** , met de rechtermuisknop op de beschikbaarheidsgroep en klik op **Eigenschappen**.

1. Maak een afhankelijkheid op bron (niet het IP-adres resources-naam) van de listener. Klik op **OK**.

1. Start SQL Server Management Studio en verbinding maken met de primaire replica.

1. Ga naar **AlwaysOn hoge beschikbaarheid** | **beschikbaarheidsgroepen** | **groep beschikbaarheid Listeners**. 

1. U ziet nu de listener-naam die u hebt gemaakt in Failover-clusterbeheer. Klik met de rechtermuisknop op de naam van de listener en klik op **Eigenschappen**.

1. Geef in het vak **poort** het poortnummer voor de listener beschikbaarheid groep met behulp van de $EndpointPort u eerder hebt gebruikt (1433 is de standaardinstelling), klikt u op **OK**.

U hebt nu een beschikbaarheid van SQL Server group in Azure virtuele machines die worden uitgevoerd in de modus Resource Manager. 

## <a name="test-the-connection-to-the-listener"></a>Test de verbinding met de listener

De verbinding testen:

1. RDP met een SQL-Server die zich in hetzelfde virtuele netwerk, maar is niet de eigenaar van de replica. Dit is de andere SQL Server in het cluster.

2. Hulpprogramma **sqlcmd** gebruiken om de verbinding te testen. Bijvoorbeeld het volgende script een verbinding tot stand **sqlcmd** naar de primaire replica via de listener met Windows-verificatie:

    ```
    sqlmd -S <listenerName> -E
    ```

    Als de listener met behulp van een andere poort dan de standaard poort (1433), de poort opgeven in de verbindingsreeks. Bijvoorbeeld de opdracht sqlcmd is verbonden met een listener op 1435-poort: 
    
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

De SQLCMD automatisch verbinding met het exemplaar van SQL Server fungeert als host voor de primaire replica. 

>[AZURE.NOTE] Zorg ervoor dat de poort die u opgeeft geopend op de firewall van zowel SQL Server is. Beide servers vereisen een binnenkomende regel voor de TCP-poort die u gebruikt. Zie [toevoegen of de firewallregel bewerken](http://technet.microsoft.com/library/cc753558.aspx) voor meer informatie. 

## <a name="guidelines-and-limitations"></a>Richtlijnen en beperkingen

Houd rekening met de volgende richtlijnen op beschikbaarheid groep listener in Azure met interne balancer laadt:

- Met een interne taakverdeling u alleen toegang tot de listener uit binnen hetzelfde virtuele netwerk.

## <a name="for-more-information"></a>Voor meer informatie

Zie voor meer informatie [configureren altijd op beschikbaarheid handmatig groeperen in Azure VM](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

### <a name="powershell-cmdlets"></a>PowerShell-cmdlets

Gebruik de volgende PowerShell-cmdlets voor het maken van een interne taakverdeling voor Azure virtuele machines.

- `New-AzureRmLoadBalancer`Hiermee maakt u een taakverdeling. [Nieuw AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) Zie voor meer informatie. 

- `New-AzureRMLoadBalancerFrontendIpConfig`Hiermee maakt u een front-end-IP-configuratie voor een taakverdeling. [Nieuw AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) Zie voor meer informatie.

- `New-AzureRmLoadBalancerRuleConfig`Hiermee maakt u een configuratie voor een taakverdeling. [Nieuw AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) Zie voor meer informatie. 

- `New-AzureRMLoadBalancerBackendAddressPoolConfig`Hiermee maakt u een back-end-adresgroepconfiguratie voor de verdeling van de belasting. [Nieuw AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) Zie voor meer informatie. 

- `New-AzureRmLoadBalancerProbeConfig`Hiermee maakt u een sonde configuratie voor een taakverdeling. [Nieuw AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) Zie voor meer informatie.

Als u een load balancer verwijderen uit een resourcegroep Azure wilt, gebruikt u `Remove-AzureRmLoadBalancer`. Zie [Software-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx)voor meer informatie.

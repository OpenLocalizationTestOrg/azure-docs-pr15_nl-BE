<properties
   pageTitle="Maak een interne taakverdeling met PowerShell in Resource Manager | Microsoft Azure"
   description="Informatie over het maken van een interne taakverdeling met PowerShell in Resource Manager"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-using-powershell"></a>Maak een interne taakverdeling met PowerShell

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


Hieronder wordt uitgelegd hoe u een interne taakverdeling met behulp van bronbeheer Azure met PowerShell maakt. Met Azure Resource Manager de artikelen voor het maken van een interne taakverdeling worden afzonderlijk geconfigureerd en vervolgens gecombineerd om een load balancer maken.

U moet maken en configureren van de volgende objecten als u wilt een load balancer implementeren:

- Front end-IP-configuratie - zal de particuliere IP-adres voor binnenkomend netwerkverkeer configureren
- Back-end-adresgroep - zal de netwerkinterfaces die ontvangt de taakverdeling verkeer afkomstig van de front-end-IP-adresgroep configureren
- Taakverdeling regels - bron en de configuratie van de lokale poort voor de taakverdeling.
- Sondes - Hiermee configureert u de sonde gezondheid status voor de exemplaren van de virtuele Machine.
- Binnenkomende regels NAT - Hiermee configureert u de poortregels rechtstreeks toegang tot een van de exemplaren van de virtuele Machine.

U krijgt meer informatie over het laden taakverdeling voor onderdelen met Azure resourcemanager op [Azure Resource Manager ondersteuning voor taakverdeling](load-balancer-arm.md).

De volgende stappen wordt uitgelegd hoe een taakverdeling tussen de twee virtuele machines configureren.


## <a name="setup-powershell-to-use-resource-manager"></a>PowerShell Setup Resource Manager gebruiken

Controleer of u beschikt over de meest recente productieversie van de module Azure voor PowerShell en PowerShell setup correct hebt voor toegang tot uw abonnement Azure.

### <a name="step-1"></a>Stap 1

        Login-AzureRmAccount

### <a name="step-2"></a>Stap 2

De abonnementen voor de account controleren

        Get-AzureRmSubscription

U wordt gevraagd te verifiëren met uw referenties.<BR>

### <a name="step-3"></a>Stap 3

Kies welke van uw Azure-abonnementen te gebruiken. <BR>


        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="create-resource-group-for-load-balancer"></a>Resourcegroep maken voor taakverdeling

### <a name="step-4"></a>Stap 4

Maak een nieuwe resourcegroep (overslaan deze stap als u een bestaande bronnengroep gebruiken)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

Azure Resource Manager vereist dat alle bronnengroepen van een locatie opgeven. Dit wordt gebruikt als de standaardlocatie voor resources in die groep. Zorg ervoor dat alle opdrachten voor het maken van een load balancer zullen dezelfde resourcegroep.

In het bovenstaande voorbeeld wij een resourcegroep "NRP RG" en de locatie 'West ons' genoemd.

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Virtueel netwerk en een persoonlijk IP-adres voor de IP-adresgroep-front-end maken


### <a name="step-1"></a>Stap 1

Een subnet voor het virtuele netwerk wordt gemaakt en toegewezen aan de variabele $backendSubnet

    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24

Een virtueel netwerk maken:

    $vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

Het virtuele netwerk wordt gemaakt en het subnet lb subnet worden toegevoegd aan het virtuele netwerk NRPVNet wordt toegewezen aan de variabele $vnet



## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Front end-IP-adresgroep en back-end-adresgroep maken

Het instellen van een IP-adresgroep front-end voor de binnenkomende load balancer netwerk verkeer en back-end-adresgroep voor het ontvangen van de belasting in evenwicht verkeer.

### <a name="step-1"></a>Stap 1

Maak een front-end-IP-adresgroep met particuliere IP-adres 10.0.2.5 voor het subnet 10.0.2.0/24 die het eindpunt van het binnenkomende verkeer.

    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id

### <a name="step-2"></a>stap 2

Een back-end-adresgroep gebruikt voor het binnenkomende verkeer wordt ontvangen van de front-end-IP-adresgroep ingesteld:

    $beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"


## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>Maak LB, NAT-regels, sonde en de belasting voor documentconversies

Na het maken van de IP-adresgroep front-end en de back-end-adresgroep, moet u de regels die tot de load balancer-bron behoren te maken:

### <a name="step-1"></a>Stap 1

    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

     $lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80


In het bovenstaande voorbeeld maakt de volgende items:

- NAT regel die al het inkomende verkeer op poort 3441 zal gaan op poort 3389.
- een tweede NAT regel die al het inkomende verkeer op poort 3442 zal gaan op poort 3389.
- alle binnenkomende verkeer op openbare poort 80, lokale poort 80 in de back-end-adresgroep aan het saldo van een load balancer-regel die wordt geladen.
- een sonde regel die de status van het pad "HealthProbe.aspx controleert"



### <a name="step-2"></a>Stap 2

Maak de optelling van alle objecten (NAT, Load balancer regels, sonde configuraties) taakverdeling:

    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe


## <a name="create-network-interfaces"></a>Netwerkinterfaces maken

Na de interne taakverdeling maakt, moet u definiëren welke netwerkinterfaces ontvangt de binnenkomende netwerkverkeer taakverdeling, NAT-regels en sonde. De netwerkinterface is in dit geval wordt afzonderlijk geconfigureerd en later met een virtuele machine kan worden toegewezen.


### <a name="step-1"></a>Stap 1


De resource virtueel netwerk en subnet maken netwerkinterfaces downloaden:

    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet


Deze stap maakt u een netwerkinterface die deel gaat uitmaken van de load balancer back-end-toepassingen en koppel de eerste regel voor NAT voor RDP voor deze netwerkinterface:

    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

### <a name="step-2"></a>Stap 2

Maak een tweede netwerkinterface naam LB Nic2 zijn:

Deze stap maakt u een tweede netwerkinterface, toewijzen aan dezelfde load balancer back-end-toepassingen en het koppelen van de tweede regel voor NAT gemaakt voor RDP:

     $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

Het eindresultaat wordt het volgende weergegeven:

    $backendnic1

Verwachte output:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
                           "PublicIpAddress": {
                             "Id": null
                           },
                           "LoadBalancerBackendAddressPools": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                             }
                           ],
                           "LoadBalancerInboundNatRules": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                             }
                           ],
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Stap 3

Gebruik de opdracht Add-AzureRmVMNetworkInterface de NIC toewijzen aan een virtuele Machine.

U vindt de instructies stap voor stap naar een virtuele machine maken en toewijzen aan een NIC na de documentatie: [een Azure VM maken met behulp van PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md).

Als u al een virtuele machine gemaakt hebt, kunt u de network interface met de volgende stappen kunt toevoegen:

#### <a name="step-1"></a>Stap 1

De resource load balancer laadt in een variabele (als u dat nog niet hebt gedaan). De variabele $lb wordt genoemd en gebruikt u dezelfde namen uit de load balancer bron dat hierboven is gemaakt.

    $lb= Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG

#### <a name="step-2"></a>Stap 2

De back-end-configuratie om een variabele te laden.

    $backend= Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

#### <a name="step-3"></a>Stap 3

De reeds gemaakte netwerkinterface laden in een variabele. de naam van de variabele gebruikt is $nic. De naam van de netwerk-interface gebruikt is hetzelfde in het bovenstaande voorbeeld.

    $nic=Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG

#### <a name="step-4"></a>Stap 4

De back-end-configuratie op de netwerkinterface wijzigen.

    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

#### <a name="step-5"></a>Stap 5

Het netwerk interface-object opslaan.

    Set-AzureRmNetworkInterface -NetworkInterface $nic

Nadat u een netwerkinterface wordt toegevoegd aan de load balancer back-end-toepassingen, wordt gestart op basis van de regels voor die resource load balancer voor taakverdeling netwerkverkeer ontvangen.

## <a name="update-an-existing-load-balancer"></a>Bijwerken van een bestaande taakverdeling


### <a name="step-1"></a>Stap 1

Met behulp van de taakverdeling van het bovenstaande voorbeeld, load balancer-object toewijzen aan variabele $slb met Get-AzureRmLoadBalancer

    $slb=get-azureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

### <a name="step-2"></a>Stap 2

In het volgende voorbeeld voegt u een nieuwe regel voor NAT inkomende poort 81 in de front-end en poort 8181 gebruiken voor de back-end-toepassingen naar een bestaande taakverdeling toe

    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp


### <a name="step-3"></a>Stap 3

Sla de nieuwe configuratie met behulp van Set AzureLoadBalancer

    $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Een load balancer verwijderen

Met de opdracht verwijderen AzureRmLoadBalancer verwijdert u een eerder gemaakte taakverdeling met de naam "NRP-LB" in een resourcegroep "NRP RG" genoemd

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] U kunt de optionele schakeloptie - kracht om te voorkomen dat de prompt voor verwijdering.



## <a name="next-steps"></a>Volgende stappen

[Een Load balancer distributie-modus configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)
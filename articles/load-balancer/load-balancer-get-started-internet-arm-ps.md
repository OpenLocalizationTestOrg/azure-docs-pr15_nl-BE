<properties
   pageTitle="Maken van een internetverbinding taakverdeling in Resource Manager met PowerShell | Microsoft Azure"
   description="Informatie over het maken van een internetverbinding taakverdeling in Resource Manager met PowerShell"
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

# <a name="get-started"></a>Maken van een internetverbinding taakverdeling in Resource Manager met PowerShell

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op het implementatiemodel Resource Manager. U kunt ook [informatie over het maken van een internetverbinding taakverdeling met behulp van het implementatiemodel klassiek](load-balancer-get-started-internet-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>Implementatie van de oplossing met Azure PowerShell

De volgende procedures wordt uitgelegd hoe een internetgerichte taakverdeling maken met behulp van bronbeheer Azure met PowerShell. Met Azure Resource Manager elke resource is gemaakt en afzonderlijk geconfigureerd en deze vervolgens tot een taakverdeling.

U moet maken en configureren van de volgende objecten als u wilt een load balancer implementeren:

- Front-IP-configuratie: bevat het adres van openbare IP-(PIP) voor binnenkomend netwerkverkeer.
- Back-end-adresgroep: netwerkinterfaces (NIC's) bevat voor de virtuele machines voor het netwerkverkeer ontvangen van de taakverdeling.
- Load balancing-regels: regels bevat die een openbare poort in de taakverdelingsvoorziening aan een poort in de back-end-adresgroep toewijzen.
- Binnenkomende NAT regels: regels bevat die een openbare poort in de taakverdelingsvoorziening aan een poort voor een specifieke virtuele machine in de back-end-adresgroep toewijzen.
- Sondes: gezondheid sondes gebruikt om te controleren van de beschikbaarheid van exemplaren van de virtuele machine in de back-end-adresgroep bevat.

Zie [Bronbeheer Azure ondersteuning voor taakverdeling](load-balancer-arm.md)voor meer informatie.

## <a name="set-up-powershell-to-use-resource-manager"></a>PowerShell instellen voor gebruik van Resource Manager

Zorg ervoor dat u beschikt over de meest recente productieversie van de module Azure Resource Manager voor PowerShell:

1. Aanmelden bij Azure.

        Login-AzureRmAccount

    Geef uw referenties op wanneer daarom wordt gevraagd.

2. Controleer de abonnementen voor de account.

        Get-AzureRmSubscription

3. Kies welke van uw Azure-abonnementen te gebruiken.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Een resourcegroep maken. (Sla deze stap over als u een bestaande resourcegroep.)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Een virtueel netwerk en een openbaar IP-adres voor de front-IP-adresgroep maken

1. Een subnet en een virtueel netwerk maken.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Maak een Azure openbare IP-adresbron, met de naam **PublicIP**moet worden gebruikt door een front-end-IP-adresgroep met de DNS-naam **loadbalancernrp.westus.cloudapp.azure.com**. De volgende opdracht wordt het type van de statische toewijzing.

        $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' –AllocationMethod Static -DomainNameLabel loadbalancernrp

    >[AZURE.IMPORTANT]Verdeling van de belasting wordt het domeinlabel van de openbare IP-als voorvoegsel voor de FQDN-naam gebruikt. Dit is anders dan de klassieke implementatiemodel dat gebruikmaakt van de cloud-service als de FQDN-naam van de taakverdeling.
    >In dit voorbeeld wordt de FQDN-naam **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Een IP-adresgroep front-end en een back-end-adresgroep maken

1. Een front-end-IP-adresgroep **LB-Frontend** die gebruikmaakt van de **PublicIp** resource met de naam maken.

        $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP

2. Maak een back-end-adresgroep **LB backend**met de naam.

        $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>NAT regels, een regel load balancer een sonde en een load balancer maken

In dit voorbeeld wordt de volgende items:

- Een NAT regel alle binnenkomend verkeer op poort 3441 poort 3389 vertalen
- Een NAT regel alle binnenkomend verkeer op poort 3442 poort 3389 vertalen
- Een regel van de sonde te controleren van de status van een pagina met de naam **HealthProbe.aspx**
- Een regel load balancer voor een evenwicht tussen alle binnenkomend verkeer op poort 80 voor poort 80 op de adressen in de groep back-end
- Een taakverdeling met alle objecten in deze

Voer deze stappen uit:

1. De NAT-regels maken.

        $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

        $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

2. Maak een sonde met gezondheid. Er zijn twee manieren voor het configureren van een sonde:

    HTTP-sonde

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    TCP-sonde

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2

3. Maak een regel load balancer.

        $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80

4. De taakverdeling met behulp van de eerder gemaakte objecten maken.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe

## <a name="create-nics"></a>NIC's maken

Netwerkinterfaces maken (of bestaande wijzigen) en vervolgens te koppelen aan de NAT-regels, regels voor load balancer en sondes:

1. Lees het virtuele netwerk en een virtueel netwerk subnet, waarbij de NIC's moeten worden gemaakt.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Maak een NIC met de naam **lb nic1 worden**, en te koppelen aan de eerste regel van de NAT en de eerste (en enige) back-end-adresgroep.

        $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

3. Maak een NIC met de naam **lb nic2 worden**, en te koppelen aan de tweede regel voor NAT en de eerste (en enige) back-end-adresgroep.

        $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

4. Controleer of de netwerkadapters.

        $backendnic1

    Verwachte output:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
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
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Gebruik de `Add-AzureRmVMNetworkInterface` cmdlet de NIC's toewijzen aan verschillende VMs.

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Zie voor instructies over het maken van een virtuele machine en het toewijzen van een NIC, [maken een VM Azure PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md).

## <a name="add-the-network-interface-to-the-load-balancer"></a>De netwerkinterface toevoegen aan de taakverdeling

1. De taakverdeling van Azure ophalen.

    De resource load balancer laadt in een variabele (als u dat nog niet hebt gedaan). De variabele wordt **$lb**genoemd. Gebruik dezelfde namen uit de load balancer-bron die u eerder hebt gemaakt.

        $lb= get-azurermloadbalancer –name NRP-LB -resourcegroupname NRP-RG

2. De back-end-configuratie om een variabele te laden.

        $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

3. De reeds gemaakte netwerkinterface laden in een variabele. Naam van de variabele is **$nic**. De naam van de netwerk-interface, dezelfde is uit het eerdere voorbeeld.

        $nic =get-azurermnetworkinterface –name lb-nic1-be -resourcegroupname NRP-RG

4. De back-end-configuratie op de netwerkinterface wijzigen.

        $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

5. Het netwerk interface-object opslaan.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Nadat u een netwerkinterface wordt toegevoegd aan de load balancer back-end-toepassingen, wordt gestart op basis van de regeling van de belasting voor de resource load balancer netwerkverkeer ontvangen.

## <a name="update-an-existing-load-balancer"></a>Bijwerken van een bestaande taakverdeling

1. Toewijzen met behulp van de taakverdeling uit het eerdere voorbeeld, een load balancer-object aan de variabele **$slb** met behulp van `Get-AzureLoadBalancer`.

        $slb = get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

2. In het volgende voorbeeld kunt u een binnenkomende regel voor NAT--toevoegen met behulp van poort 81 in de front-end-toepassingen en poort 8181 voor de back-end-toepassingen--aan een bestaande taakverdeling.

        $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP

3. De nieuwe configuratie op te slaan met behulp van `Set-AzureLoadBalancer`.

        $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Een load balancer verwijderen

Gebruik de opdracht `Remove-AzureLoadBalancer` genaamd **NRP RG**een eerder gemaakte taakverdeling met de naam **NRP LB** in een resourcegroep verwijderen.

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] U kunt de optionele schakeloptie **-Force** om te voorkomen dat de prompt voor verwijdering.

## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een load balancer distributie-modus configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)

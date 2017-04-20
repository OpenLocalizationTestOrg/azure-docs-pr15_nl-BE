<properties
    pageTitle="Een Internet facing taakverdeling met IPv6 met PowerShell voor Resource Manager maken | Microsoft Azure"
    description="Informatie over het maken van een internetverbinding taakverdeling met IPv6 met PowerShell voor bronbeheer"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6, azure-taakverdeling, dual-stack, openbaar IP-, systeemeigen ipv6, mobiele, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Maken van een internetverbinding taakverdeling met IPv6 met PowerShell voor Resource Manager aan de slag

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Sjabloon](./load-balancer-ipv6-internet-template.md)

Azure-taakverdeling is een taakverdeling laag-4 (TCP, UDP). De taakverdeling biedt een hoge beschikbaarheid door binnenkomend verkeer over gezonde service-exemplaren in de cloud-services of virtuele machines in een load balancer verdelen. Azure taakverdeling kan ook presenteren die diensten op meerdere poorten of meerdere IP-adressen.

## <a name="example-deployment-scenario"></a>Voorbeeld van de implementatie van scenario

In het volgende diagram ziet u de oplossing in dit artikel wordt ingezet voor de taakverdeling.

![Load balancer scenario](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

In dit scenario maakt u de volgende bronnen Azure:

- de taakverdeling van een internetverbinding met een IPv4- en IPv6-openbare IP-adres
- twee laden balancing regels voor de openbare VIP's worden toegewezen aan de particuliere eindpunten
- een beschikbaarheid ingesteld op de bevat de twee VMs
- twee virtuele machines (VMs)
- een virtuele netwerkinterface voor elke VM met zowel IPv4 als IPv6-adressen die zijn toegewezen

## <a name="deploying-the-solution-using-the-azure-powershell"></a>Implementatie van de oplossing met de Azure PowerShell

De volgende stappen wordt aangegeven hoe voor het maken van een internetverbinding met Azure Resource Manager met PowerShell taakverdeling. Met Azure Resource Manager elke resource wordt gemaakt en vervolgens put samen om een bron van afzonderlijk geconfigureerd.

Voor de implementatie van een load balancer maken en configureren van de volgende objecten:

- IP-configuratie van front-end - bevat openbare IP-adressen voor binnenkomende netwerkverkeer.
- Back-end-adresgroep - bevat netwerkinterfaces (NIC's) voor de virtuele machines voor het netwerkverkeer ontvangen van de taakverdeling.
- Regels voor taakverdeling - regels een openbare poort op de taakverdeling toe te wijzen aan de poort van de back-end-adresgroep bevat.
- Binnenkomende regels NAT - regels een openbare poort op de taakverdeling toe te wijzen aan een poort voor een specifieke virtuele machine in de back-end-adresgroep bevat.
- Sondes - gezondheid sondes gebruikt om te controleren van de beschikbaarheid van exemplaren van de virtuele machines in de back-end-adresgroep bevat.

Zie [Bronbeheer Azure ondersteuning voor taakverdeling](load-balancer-arm.md)voor meer informatie.

## <a name="set-up-powershell-to-use-resource-manager"></a>PowerShell instellen voor gebruik van Resource Manager

Zorg ervoor dat u beschikt over de meest recente productieversie van de module Azure Resource Manager voor PowerShell.

1. Log in Azure

        Login-AzureRmAccount

    Geef uw referenties op wanneer daarom wordt gevraagd.

2. De abonnementen voor de account controleren

        Get-AzureRmSubscription

3. Kies welke van uw Azure-abonnementen te gebruiken.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Een resourcegroep (overslaan deze stap als u met behulp van een bestaande bronnengroep) maken

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Een virtueel netwerk en een openbaar IP-adres voor de front-IP-adresgroep maken

1. Een virtueel netwerk met een subnet maken.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Azure openbaar IP-adres (PIP)-bronnen voor de front-IP-adresgroep maken.

        $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
        $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6

    >[AZURE.IMPORTANT] Verdeling van de belasting wordt het domeinlabel van de openbare IP-als voorvoegsel voor de FQDN-naam gebruikt. In dit voorbeeld zijn de FQDN-namen *lbnrpipv4.westus.cloudapp.azure.com* en *lbnrpipv6.westus.cloudapp.azure.com*.

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Een Front-End-IP-configuraties en een Back-End-adresgroep maken

1. Maken van front-adresconfiguratie die gebruikmaakt van het openbare IP-adressen die u hebt gemaakt.

        $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
        $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6

2. Back-end-adresgroepen maken.

        $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
        $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"


## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>LB regels, NAT regels een sonde en een load balancer maken

In dit voorbeeld wordt de volgende items:

- een NAT regel alle binnenkomend verkeer op poort 443 op poort 4443 vertalen
- een load balancer regel voor evenwicht tussen al het binnenkomende verkeer op poort 80 voor poort 80 op de adressen in de groep back-end.
- een load balancer regel RDP-verbinding met het VMs op poort 3389 toestaan.
- een regel van de sonde bij het controleren van de status van een pagina met de naam *HealthProbe.aspx* of een service op poort 8080
- een taakverdeling met alle objecten in deze

1. De NAT-regels maken.

        $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
        $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443

2. Maak een sonde met gezondheid. Er zijn twee manieren voor het configureren van een sonde:

    HTTP-sonde

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    of TCP-sonde

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
        $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2


    In dit voorbeeld gaan we de sondes TCP gebruiken.

3. Maak een regel load balancer.

        $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389

4. De taakverdeling met behulp van de eerder gemaakte objecten maken.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule

## <a name="create-nics-for-the-back-end-vms"></a>NIC's maken voor de back-end VMs

1. Lees het virtuele netwerk en virtuele netwerk Subnet, waarbij de NIC's moeten worden gemaakt.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. IP-configuraties en NIC's maken voor het VMs.

        $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
        $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
        $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

        $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
        $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
        $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a>Virtuele machines maken en toewijzen van de zojuist gemaakte NIC 's

Zie voor meer informatie over het maken van een VM [maken en vooraf configureren van een virtuele Windows-computer met Resource Manager en Azure PowerShell](..\virtual-machines\virtual-machines-windows-ps-create.md)

1. Maak een account instellen voor beschikbaarheid en opslag

        New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
        $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
        New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName $LRS
        $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'

2. Elke VM maken en toewijzen via de vorige NIC's gemaakt

        $mySecureCredentials= Get-Credential -Message “Type the username and password of the local administrator account.”

        $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
        $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
        $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

        $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
        $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
        $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2

## <a name="next-steps"></a>Volgende stappen

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een load balancer distributie-modus configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP-time-outinstellingen voor de taakverdeling configureren](load-balancer-tcp-idle-timeout.md)

<properties
    pageTitle="Algemene netwerk PowerShell-opdrachten voor VMs | Microsoft Azure"
    description="Algemene PowerShell-opdrachten om u te helpen een virtueel netwerk en de bijbehorende bronnen maken voor VMs is gestart."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="common-network-azure-powershell-commands-for-vms"></a>Algemene netwerk Azure PowerShell-opdrachten voor VMs

Als u een virtuele machine maakt, moet u een [virtueel netwerk](../virtual-network/virtual-networks-overview.md) maken of informatie over een bestaande virtuele netwerk waarin de VM kan worden toegevoegd. Meestal wanneer u een VM maakt, moet u ook rekening te houden met het maken van de bronnen die in dit artikel wordt beschreven.

[Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor informatie over het installeren van de nieuwste versie van Azure PowerShell, uw abonnement selecteren en aanmelden bij uw account.

## <a name="create-network-resources"></a>Netwerk resources maken

Taak | Opdracht 
-------------- | -------------------------
Subnet-configuraties maken | $subnet1 = [New AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) -naam "subnet_name" - AddressPrefix XX. X.X.X/XX<BR>$subnet2 = New AzureRmVirtualNetworkSubnetConfig-naam "subnet_name" - AddressPrefix XX. X.X.X/XX<BR><BR>Een typische netwerk mogelijk een subnet voor een [internet facing taakverdeling](../load-balancer/load-balancer-internet-overview.md) en een afzonderlijk subnet bestemd voor een [interne taakverdeling](../load-balancer/load-balancer-internal-overview.md). |
Een virtueel netwerk maken | $vnet = [New AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) -naam "virtual_network_name" - ResourceGroupName "resource_group_name"-locatie "naam_locatie" - AddressPrefix XX. X.X.X/XX-Subnet $subnet1, $subnet2
Test voor een unieke domeinnaam | "Domeinnaam" [Test-AzureRmDnsAvailability](https://msdn.microsoft.com/library/mt619419.aspx) - DomainQualifiedName-locatie "naam_locatie"<BR><BR>U kunt een DNS-domeinnaam voor een [openbaar IP-bron](../virtual-network/virtual-network-ip-addresses-overview-arm.md)wordt gemaakt van een domainname.location.cloudapp.azure.com aan het openbare IP-adres-toewijzing in de Azure managed DNS-servers opgeven. De naam mag alleen letters, cijfers en streepjes. De eerste en laatste teken moet een letter of nummer en de domeinnaam moet uniek zijn binnen de Azure locatie. Als **de waarde True** wordt geretourneerd, is de voorgestelde naam uniek.
Een openbaar IP-adres maken | $pip = [New AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) -naam "ip_address_name" - ResourceGroupName "resource_group_name" - DomainNameLabel "domeinnaam"-locatie "naam_locatie" - AllocationMethod Dynamic<BR><BR>Het openbare IP-adres wordt gebruikt voor de domeinnaam die u eerder hebt getest en wordt gebruikt door de frontend-configuratie van de taakverdeling.
Maak een frontend IP-configuratie | $frontendIP = [New AzureRmLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) -naam "frontend_ip_name" - PublicIpAddress $pip<BR><BR>De frontend configuratie omvat het openbare IP-adres dat u eerder hebt gemaakt voor binnenkomend netwerkverkeer.
Een back-end-adresgroep maken | $beAddressPool = [New AzureRmLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) -naam "backend_pool_name"<BR><BR>Interne adressen bevat voor de verdeling van de belasting die toegankelijk zijn via een netwerkinterface-end.
Een sonde maken | $healthProbe = [New AzureRmLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) -naam 'probe_name' - RequestPath 'HealthProbe.aspx'-Protocol-http-poort 80 - IntervalInSeconds 15 - ProbeCount 2<BR><BR>Gezondheid sondes gebruikt om te controleren van de beschikbaarheid van exemplaren van de virtuele machines in de backend-adresgroep bevat.
Maak een regel voor taakverdeling | $lbRule = [New AzureRmLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) -naam HTTP - FrontendIpConfiguration $frontendIP - BackendAddressPool $beAddressPool-$healthProbe Probe-80 TCP-Protocol - FrontendPort - BackendPort 80<BR><BR>Regels bevat die geen openbare poort voor de taakverdeling op een poort in de backend-adresgroep toewijzen.
Een binnenkomende regel voor NAT maken | $inboundNATRule = [New AzureRmLoadBalancerInboundNatRuleConfig](https://msdn.microsoft.com/library/mt603606.aspx) -naam "rule_name" - FrontendIpConfiguration $frontendIP-3441 Protocol TCP - FrontendPort - BackendPort 3389<BR><BR>Regels voor toewijzing van een openbare poort op de verdeling van de belasting op een poort voor een specifieke virtuele machine in de backend-adresgroep bevat.
Maak een load balancer | $loadBalancer = "resource_group_name" [Nieuw-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) - ResourceGroupName-de naam 'load_balancer_name'-locatie "naam_locatie" - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-$healthProbe Probe
Maak een netwerkinterface | $nic1 = "resource_group_name" [Nieuw-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) - ResourceGroupName-de naam 'network_interface_name'-locatie "naam_locatie" - PrivateIpAddress XX. X.X.X-Subnet subnet2 - LoadBalancerBackendAddressPool-$loadBalancer.BackendAddressPools[0] - LoadBalancerInboundNatRule-$loadBalancer.InboundNatRules[0]<BR><BR>Maak een netwerk-interface het openbare IP-adres en virtueel netwerk subnet dat u eerder hebt gemaakt.
    
## <a name="get-information-about-network-resources"></a>Informatie over netwerkbronnen

Taak | Opdracht 
-------------- | -------------------------
Lijst met virtuele netwerken | [Get-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603515.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Hier worden de virtuele netwerken in de resourcegroep.
Informatie opvragen over een virtueel netwerk | Get-AzureRmVirtualNetwork-naam "virtual_network_name" - ResourceGroupName "resource_group_name"
Lijst met subnetten in een virtueel netwerk | Get-AzureRmVirtualNetwork-naam "virtual_network_name" - ResourceGroupName "resource_group_name" & #124; Selecteer subnetten
Informatie opvragen over een subnet | [Get-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603817.aspx) -naam "subnet_name" - VirtualNetwork $vnet<BR><BR>Informatie ophalen over het subnet in het opgegeven virtuele netwerk. De waarde $vnet geeft het object dat wordt geretourneerd door de Get-AzureRmVirtualNetwork.
Lijst met IP-adressen | [Get-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619342.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Hier worden de openbare IP-adressen in de resourcegroep.
Lijst met netwerktaakverdeling | [Get-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603668.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Hier worden alle Netwerktaakverdeling in de resourcegroep.
Lijst met netwerkinterfaces | [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Hier worden alle netwerkinterfaces in de resourcegroep.
Informatie over een netwerkinterface | Get-AzureRmNetworkInterface-naam "network_interface_name" - ResourceGroupName "resource_group_name"<BR><BR>Met deze eigenschap wordt informatie over een specifieke netwerkinterface.
De IP-configuratie van een netwerkinterface ophalen | [Get-AzureRmNetworkInterfaceIPConfig](https://msdn.microsoft.com/library/mt732618.aspx) -naam "ipconfiguration_name" - NetworkInterface $nic<BR><BR>Met deze eigenschap wordt informatie over de IP-configuratie van de opgegeven netwerk-interface. De waarde $nic geeft het object dat wordt geretourneerd door de Get-AzureRmNetworkInterface.

## <a name="manage-network-resources"></a>Netwerkbronnen beheren

Taak | Opdracht 
-------------- | -------------------------
Een subnet aan een virtueel netwerk toevoegen | [Toevoegen-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603722.aspx) - AddressPrefix XX. X.X.X/XX-naam "subnet_name" - VirtualNetwork $vnet<BR><BR>Een subnet aan een bestaande virtuele netwerk toegevoegd. De waarde $vnet geeft het object dat wordt geretourneerd door de Get-AzureRmVirtualNetwork.
Een virtueel netwerk verwijderen | [Verwijderen AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt619338.aspx) -naam "virtual_network_name" - ResourceGroupName "resource_group_name"<BR><BR>Hiermee verwijdert u de opgegeven virtuele netwerk uit de resourcegroep.
Een netwerkinterface verwijderen | [Verwijderen AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt603836.aspx) -naam "network_interface_name" - ResourceGroupName "resource_group_name"<BR><BR>Hiermee verwijdert u de opgegeven netwerk-interface van de resourcegroep.
Een load balancer verwijderen | [Verwijderen AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603862.aspx) -naam "load_balancer_name" - ResourceGroupName "resource_group_name"<BR><BR>Hiermee verwijdert u de opgegeven taakverdeling uit de resourcegroep.
Een openbaar IP-adres verwijderen | [Verwijderen AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619352.aspx)-naam "ip_address_name" - ResourceGroupName "resource_group_name"<BR><BR>Hiermee verwijdert u de opgegeven openbare IP-adres uit de resourcegroep.

## <a name="next-steps"></a>Volgende stappen

- Gebruik de netwerkinterface die u zojuist hebt gemaakt wanneer u [een VM maken](virtual-machines-windows-ps-create.md).
- Meer informatie over hoe u [maken van een VM met meerdere netwerkinterfaces kunt](../virtual-network/virtual-networks-multiple-nics.md).

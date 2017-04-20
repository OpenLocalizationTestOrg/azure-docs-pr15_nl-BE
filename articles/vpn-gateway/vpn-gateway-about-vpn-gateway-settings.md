<properties 
   pageTitle="Over VPN-Gateway-instellingen voor virtuele netwerkgateways | Microsoft Azure"
   description="Meer informatie over VPN-Gateway-instellingen voor het virtuele netwerk Azure."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway-settings"></a>Over VPN-Gateway-instellingen

Een VPN-gateway verbinding oplossing is afhankelijk van de configuratie van meerdere resources wilt verzenden van netwerkverkeer tussen virtuele netwerken en locaties ruimten. Elke resource bevat configureerbare instellingen. De combinatie van de bronnen en instellingen bepaalt de uitkomst van de verbinding.

De secties in dit artikel worden de bronnen en de instellingen die betrekking op een VPN-gateway in het implementatiemodel **Resource Manager hebben** . Kan handig zijn om de beschikbare configuraties met behulp van verbinding topologie diagrammen weergeven. Vindt u beschrijvingen en topologie diagrammen voor elke oplossing van de verbinding in het artikel [Over VPN-Gateway](vpn-gateway-about-vpngateways.md) . 

## <a name="gwtype"></a>Gateway typen

Elke virtuele netwerk kan slechts één virtueel netwerkgateway van elke soort hebben. Wanneer u een gateway virtueel netwerk maakt, moet u ervoor zorgen dat het type gateway juist voor uw configuratie is.

De beschikbare waarden voor - GatewayType zijn: 

- VPN
- ExpressRoute

Een VPN-gateway vereist de `-GatewayType` *Vpn*.  

Voorbeeld:

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
    -VpnType RouteBased
 

## <a name="gwsku"></a>Gateway-SKU 's


[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configuring-the-gateway-sku"></a>De SKU-gateway configureren

**De SKU-gateway op te geven in de Azure portal**

Als de Azure portal maakt u een virtuele netwerkgateway Resource Manager, selecteert u de SKU-gateway met behulp van de vervolgkeuzelijst. De opties die wordt weergegeven overeen met de Gateway en de VPN-type dat u selecteert.

Bijvoorbeeld als u het type gateway 'VPN' en de VPN-type '-beleid' selecteert, ziet u alleen de 'Standaard' SKU omdat dat de enige SKU die beschikbaar voor VPN's PolicyBased. Als u 'Route op basis van' selecteert, kunt u uit de Basic-, Standard- en HighPerformance-SKU's. 


**De SKU-gateway opgeven met PowerShell**


In het volgende voorbeeld van PowerShell geeft de `-GatewaySku` als *standaard*.

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard `
    -GatewayType Vpn -VpnType RouteBased

**Een SKU-gateway wijzigen**

Als u wilt dat uw gateway SKU upgraden naar een krachtiger SKU (van Basic-/ Standard naar HighPerformance) kunt u de `Resize-AzureRmVirtualNetworkGateway` PowerShell-cmdlet. Ook kunt u de gateway SKU grootte met deze cmdlet downgraden.

De volgende PowerShell-voorbeeld ziet u een gateway SKU wordt aangepast aan de HighPerformance.

    $gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

### <a name="estimated-aggregate-throughput-by-gateway-sku-and-type"></a>Geschatte totale doorvoer door gateway SKU en type

De volgende tabel ziet u de typen gateway en de geschatte totale doorvoer. Deze tabel geldt voor de resourcemanager en de van klassieke implementatiemodellen.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 


## <a name="connectiontype"></a>Verbindingstypen

Het implementatiemodel Resource Manager moet elke configuratie u een specifiek virtueel netwerk gateway-verbindingstype. De beschikbare Resource Manager PowerShell waarden voor `-ConnectionType` zijn:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient

In de volgende PowerShell voorbeeld maken we een S2S-verbinding waarvoor u het verbindingstype voor *IPsec*.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'


## <a name="vpntype"></a>VPN-typen

Als u de gateway virtueel netwerk voor de configuratie van een VPN-gateway maakt, moet u een VPN-type. De VPN-type dat u kiest, is afhankelijk van de topologie van de verbinding die u wilt maken. Bijvoorbeeld vereist een P2S verbinding een RouteBased VPN-type. Een VPN-type kan ook afhankelijk zijn van de hardware die u wilt gebruiken. S2S configuraties vereisen een VPN-apparaat. Sommige VPN-apparaten ondersteunen alleen een bepaald VPN-type.

De VPN-type dat u selecteert moet voldoen aan alle de verbinding voor de oplossing die dat u wilt maken. Bijvoorbeeld, als u maken van een gateway S2S VPN-verbinding en een P2S VPN-gateway verbinding voor hetzelfde virtuele netwerk wilt, gebruikt u VPN-type *RouteBased* omdat P2S is een type VPN RouteBased vereist. Ook moet u controleren of dat uw VPN-apparaat een RouteBased VPN-verbinding ondersteund. 

Wanneer u een virtueel netwerkgateway hebt gemaakt, kunt u de VPN-type niet wijzigen. U moet de gateway virtueel netwerk verwijderen en een nieuwe maken. Er zijn twee typen voor VPN:

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]


In het volgende voorbeeld van PowerShell geeft de `-VpnType` als *RouteBased*. Wanneer u een gateway maken wilt, moet u ervoor zorgen dat de VpnType - juist voor uw configuratie is. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig `
    -GatewayType Vpn -VpnType RouteBased

##  <a name="requirements"></a>Gateway-vereisten

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 


## <a name="gwsub"></a>Gateway-subnet

Als u wilt een virtueel netwerk-gateway configureert, moet u eerst een gateway-subnet maken voor uw VNet. Het gateway-subnet moet de naam *GatewaySubnet* goed te laten werken. Deze naam kunt Azure weet dat dit subnet moet worden gebruikt voor de gateway.

De minimale grootte van uw subnet gateway is volledig afhankelijk van de configuratie die u wilt maken. Hoewel het maken van een subnet gateway /29 zo klein mogelijk is, raden wij aan dat u een gateway-subnet van /28 of groter maken (/ 28, /27, /26, enz.). 

Maken van een groter gateway voorkomt dat u omhoog tegen groottebeperkingen gateway wordt uitgevoerd. U hebt bijvoorbeeld een gateway virtueel netwerk gemaakt met een gateway subnet grootte /29 voor een verbinding S2S. U nu wilt configureren een S2S/ExpressRoute naast configuratie. Deze configuratie is een minimale grootte voor gateway subnet /28 vereist. De configuratieset maken, zou u moeten het gateway-subnet aan de minimumvereisten voor de verbinding, wordt de /28 wijzigen.

In het volgende voorbeeld van PowerShell Resource Manager bevat een gateway subnet genaamd GatewaySubnet. U ziet dat de CIDR-notatie geeft een /27, waarmee voldoende IP-adressen voor de meeste configuraties die momenteel beschikbaar zijn.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 


## <a name="lng"></a>Lokale netwerkgateways

Bij het maken van de configuratie van een VPN-gateway vertegenwoordigt de gateway voor het lokale netwerk vaak uw locatie op gebouwen. In het implementatiemodel klassiek worden het lokale netwerkgateway is een lokale Site genoemd. 

U geeft de gateway voor het lokale netwerk een naam, het openbare IP-adres van het VPN-apparaat op gebouwen en geef de adresprefixen die op de locatie van gebouwen bevinden zich. Azure wordt gekeken naar de bestemming adresprefixen voor netwerkverkeer, raadpleegt de configuratie die u hebt opgegeven voor uw lokale netwerkgateway en routeert pakketten dienovereenkomstig. U ook opgeven lokaal netwerkgateways voor VNet-VNet-configuraties die gebruikmaken van een VPN-gateway verbinding.

In het volgende voorbeeld van PowerShell maakt u een nieuwe LAN gateway:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Soms moet u het lokale netwerk gateway-instellingen wijzigen. Bijvoorbeeld, wanneer u toevoegen of wijzigen van het bereik, of als het IP-adres van de VPN-apparaat verandert. U kunt deze instellingen in de klassieke portal op de pagina lokale netwerken wijzigen voor een klassieke VNet. Voor bronbeheer Zie [wijzigen LAN gateway-instellingen met PowerShell](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>REST API's en PowerShell cmdlets

Zie de volgende pagina's voor meer technische bronnen en vereisten van de specifieke syntaxis wanneer u REST API's en PowerShell-cmdlets voor VPN-Gateway-configuraties:

|**Klassiek** | **Resource Manager**|
|-----|----|
|[PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST-API](https://msdn.microsoft.com/library/jj154113.aspx)|[REST-API](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Volgende stappen

Zie [Over VPN-Gateway](vpn-gateway-about-vpngateways.md) voor meer informatie over beschikbare verbinding configuraties. 







 

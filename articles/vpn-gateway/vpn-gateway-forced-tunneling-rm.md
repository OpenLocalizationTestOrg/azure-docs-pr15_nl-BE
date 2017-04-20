<properties 
   pageTitle="Geforceerde tunneling voor verbindingen van Site naar Site met behulp van het implementatiemodel Resource Manager configureren | Microsoft Azure"
   description="Het omleiden of 'dwingen' alle Internet-verkeer naar de locatie van het on-premises."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Geforceerde tunneling met behulp van het implementatiemodel Azure Resource Manager configureren

> [AZURE.SELECTOR]
- [PowerShell - klassiek](vpn-gateway-about-forced-tunneling.md)
- [PowerShell - Resource Manager](vpn-gateway-forced-tunneling-rm.md)

Geforceerde tunneling kunt u omleiding of "kracht" alle Internet-verkeer terug naar uw locatie on-premises via een Site naar Site VPN-tunnel voor inspectie en controle. Dit is een essentiële vereiste voor de meeste zakelijke IT beleid.

Zonder geforceerde tunneling zal Internet-verkeer dat uit uw VMs in Azure altijd Bladeren van Azure netwerkinfrastructuur rechtstreeks uit met het Internet, zonder de optie om te controleren of het verkeer wilt controleren. Onbevoegde toegang tot het Internet mogelijk kan leiden tot vrijgeven van informatie of andere soorten inbreuken op de beveiliging

Dit artikel helpt u bij het configureren gedwongen tunneling voor virtuele netwerken die zijn gemaakt met behulp van het implementatiemodel Resource Manager.

**Over de Azure-implementatie**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Van implementatiemodellen en hulpmiddelen voor geforceerde tunneling**

Een geforceerde tunneling verbinding kan worden geconfigureerd voor zowel het model Klassiek deployment en Resource Manager-implementatiemodel. Zie de volgende tabel voor meer informatie. Deze tabel bijgewerkt zodra nieuwe artikelen en nieuwe implementatiemodellen extra hulpprogramma's beschikbaar voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we rechtstreeks op uit de tabel.

[AZURE.INCLUDE [vpn-gateway-table-forced-tunneling](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="about-forced-tunneling"></a>Over gedwongen tunneling


In het volgende diagram ziet u hoe gedwongen tunneling works. 

![Gedwongen Tunneling](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

In het bovenstaande voorbeeld tunnel de Frontend subnet niet verplicht is. De werklast in de Frontend-subnet kunnen blijven accepteren en rechtstreeks reageren op aanvragen van klanten via Internet. De middelste laag en de back-end subnetten worden gedwongen tunnel. Alle uitgaande Internet-verbindingen van deze twee subnetten worden gedwongen of omgeleid naar een site op locatie via een van de S2S VPN-tunnels.

Hierdoor kunt u beperken en toegang tot het Internet vanuit uw virtuele machines te controleren of services in Azure, cloud terwijl u doorgaat met het inschakelen van de architectuur met meerdere tier-service die is vereist. Ook kunt u toepassen gedwongen tunneling met de hele virtuele netwerken als er geen internetverbinding werklasten in uw virtuele netwerken.

## <a name="requirements-and-considerations"></a>Vereisten en overwegingen

Geforceerde tunneling in Azure is geconfigureerd via een virtueel netwerk door gebruiker gedefinieerde routes. Verkeer doorsturen naar een site op het bedrijf wordt uitgedrukt als een standaardroute naar de Azure VPN-gateway. Zie voor meer informatie over routering van door de gebruiker gedefinieerd en virtuele netwerken [door de gebruiker gedefinieerde routes en doorsturen via IP](../virtual-network/virtual-networks-udr-overview.md).

- Elk subnet virtueel netwerk heeft een ingebouwde, systeem-routeringstabel. De routeringstabel systeem heeft de volgende drie groepen van routes:

    - **VNet lokale routes:** Direct naar de bestemming VMs in hetzelfde virtuele netwerk
    
    - **On-premises routes:** Op de Azure VPN-gateway
    
    - **Standaardroute:** Rechtstreeks met het Internet. Pakketten die bestemd zijn voor de particuliere IP-adressen die niet vallen onder de vorige twee routes worden weggehaald.

-  Deze procedure wordt door gebruiker gedefinieerde routes (UDR) een routeringstabel om een standaardroute toevoegen en koppelen de routeringstabel naar de subnetten VNet gedwongen tunneling op deze subnetten inschakelen.

- Geforceerde tunneling moet worden gekoppeld aan een VNet die een route gebaseerde VPN-gateway heeft. U moet een "standaard-site' tussen de lokale sites meerdere ruimten verbonden met het virtuele netwerk.

- ExpressRoute gedwongen tunneling via dit mechanisme niet is geconfigureerd, maar in plaats daarvan door het adverteren van een standaardroute via de peering ExpressRoute BGP-sessies is ingeschakeld. Zie de [Documentatie bij de ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) voor meer informatie.

## <a name="configuration-overview"></a>Configuratie, overzicht

De volgende procedure maakt u een resourcegroep en een VNet. U vervolgens een VPN-gateway maken en configureren van gedwongen tunneling. In deze procedure is het virtuele netwerk "MultiTier VNet" 3 de subnetten: *front-end*, *Midtier*en *back-end*4 cross-premises verbindingen: *DefaultSiteHQ*en 3 *takken*.

De *DefaultSiteHQ* instellen als standaardverbinding voor site tunneling gedwongen de procedurestappen en de Midtier configureren en back-end subnetten gebruiken gedwongen tunneling.

    
## <a name="before-you-begin"></a>Voordat u begint

Controleer of u de volgende items voordat u begint met uw configuratie.

- Een abonnement op Azure. Als u niet al een Azure-abonnement hebt, kunt u uw [MSDN-abonnee vergoedingen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of teken omhoog voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).

- U moet de meest recente versie van de Azure Resource Manager PowerShell-cmdlets (1.0 of hoger) installeren. [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor meer informatie over het installeren van de PowerShell-cmdlets.


## <a name="configure-forced-tunneling"></a>Geforceerde tunnels configureren

1. In de console PowerShell aanmelden bij uw account Azure. Deze cmdlet wordt u gevraagd de aanmeldingsreferenties voor de Account Azure. Na het aanmelden, wordt het gedownload uw accountinstellingen zodat ze beschikbaar voor Azure PowerShell zijn.

        Login-AzureRmAccount 

2. Een lijst van uw abonnementen Azure ophalen.

        Get-AzureRmSubscription

2. Geef het abonnement dat u wilt gebruiken. 

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
        
3. Een resourcegroep maken.

        New-AzureRmResourceGroup -Name "ForcedTunneling" -Location "North Europe"

4. Een virtueel netwerk maken en subnetten opgeven. 

        $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
        $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
        $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
        $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
        $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4

5. Het lokale netwerkgateways maken.

        $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
        $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
        $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
        $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
        
6. De routetabel en route-regel maken.

        New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
        $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
        Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
        Set-AzureRmRouteTable -RouteTable $rt


7. Koppel de routetabel naar het subnet van de Midtier en back-end.

        $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
        Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

8. De Gateway met een standaard-site maken. Deze stap duurt enige tijd, soms 45 minuten of langer, omdat het maken en configureren van de gateway.<br> De `-GatewayDefaultSite` de cmdlet-parameter waarmee de geforceerde routeringsconfiguratie te werken, dus zorg voor het configureren van deze instelling correct is. Deze parameter is beschikbaar in PowerShell 1.0 of hoger.

        $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
        $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
        New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewayDefaultSite $lng1 -EnableBgp $false

9. De Site naar Site VPN-verbindingen tot stand brengen.

        $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
        $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
        $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
        $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
        $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 

        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"

        Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
        




<properties 
   pageTitle="Geforceerde tunneling voor verbindingen van Site naar Site met behulp van het implementatiemodel klassiek configureren | Microsoft Azure"
   description="Het omleiden of 'dwingen' alle Internet-verkeer naar de locatie van het on-premises."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Geforceerde tunneling met behulp van het implementatiemodel klassiek configureren

> [AZURE.SELECTOR]
- [PowerShell - klassiek](vpn-gateway-about-forced-tunneling.md)
- [PowerShell - Resource Manager](vpn-gateway-forced-tunneling-rm.md)

Geforceerde tunneling kunt u omleiding of "kracht" alle Internet-verkeer terug naar uw locatie on-premises via een Site naar Site VPN-tunnel voor inspectie en controle. Dit is een essentiële vereiste voor de meeste zakelijke IT beleid. 

Zonder geforceerde tunneling zal Internet-verkeer dat uit uw VMs in Azure altijd Bladeren van Azure netwerkinfrastructuur rechtstreeks uit met het Internet, zonder de optie om te controleren of het verkeer wilt controleren. Onbevoegde toegang tot Internet kan mogelijkerwijs leiden tot vrijgeven van informatie of andere soorten inbreuken op de beveiliging.

Dit artikel helpt u bij het configureren gedwongen tunneling voor virtuele netwerken die zijn gemaakt met behulp van het implementatiemodel klassiek. 

**Over de Azure-implementatie**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Van implementatiemodellen en hulpmiddelen voor geforceerde tunneling**

Een geforceerde tunneling verbinding kan worden geconfigureerd voor zowel het model Klassiek deployment en Resource Manager-implementatiemodel. Zie de volgende tabel voor meer informatie. Deze tabel bijgewerkt zodra nieuwe artikelen en nieuwe implementatiemodellen extra hulpprogramma's beschikbaar voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we rechtstreeks op uit de tabel.

[AZURE.INCLUDE [vpn-gateway-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="requirements-and-considerations"></a>Vereisten en overwegingen

Geforceerde tunneling in Azure is geconfigureerd via een virtueel netwerk door gebruiker gedefinieerde routes (UDR). Verkeer doorsturen naar een site op het bedrijf wordt uitgedrukt als een standaardroute naar de Azure VPN-gateway. In het volgende gedeelte geeft een overzicht van de huidige beperking van de routing tabel en routes voor een virtueel netwerk Azure:


-  Elk subnet virtueel netwerk heeft een ingebouwde, systeem-routeringstabel. De routeringstabel systeem heeft de volgende drie groepen van routes:

    - **VNet lokale routes:** Direct naar de bestemming VMs in hetzelfde virtuele netwerk
    
    - **Op locatie routes:** Op de Azure VPN-gateway
    
    - **Standaardroute:** Rechtstreeks met het Internet. Pakketten die bestemd zijn voor de particuliere IP-adressen die niet vallen onder de vorige twee routes worden weggehaald.


-  U kunt met de release van de gebruiker gedefinieerde routes maken een routeringstabel om een standaardroute toevoegen en vervolgens de routeringstabel naar de subnetten VNet inschakelen gedwongen tunneling op deze subnetten koppelen.

- U moet een "standaard-site' tussen de lokale sites meerdere ruimten verbonden met het virtuele netwerk.

- Geforceerde tunneling moet worden gekoppeld aan een VNet met een dynamische routering VPN-gateway (geen statische gateway).
 
- ExpressRoute gedwongen tunneling via dit mechanisme niet is geconfigureerd, maar in plaats daarvan door het adverteren van een standaardroute via de peering ExpressRoute BGP-sessies is ingeschakeld. Zie de [Documentatie bij de ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) voor meer informatie.



## <a name="configuration-overview"></a>Configuratie, overzicht

In het volgende voorbeeld wordt de Frontend subnet niet gedwongen getunneld. De werklast in de Frontend-subnet kunnen blijven accepteren en rechtstreeks reageren op aanvragen van klanten via Internet. De middelste laag en de back-end subnetten worden gedwongen tunnel. Alle uitgaande Internet-verbindingen van deze twee subnetten worden gedwongen of omgeleid naar een site op locatie via een van de S2S VPN-tunnels.

Hierdoor kunt u beperken en toegang tot het Internet vanuit uw virtuele machines te controleren of services in Azure, cloud terwijl u doorgaat met het inschakelen van de architectuur met meerdere tier-service die is vereist. Ook kunt u toepassen gedwongen tunneling met de hele virtuele netwerken als er geen internetverbinding werklasten in uw virtuele netwerken.


![Gedwongen Tunneling](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)



## <a name="before-you-begin"></a>Voordat u begint

Controleer of u de volgende items voordat u de configuratie.

- Een abonnement op Azure. Als u niet al een Azure-abonnement hebt, kunt u uw [MSDN-abonnee vergoedingen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of teken omhoog voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).

- Een virtueel netwerk geconfigureerd. 

- De meest recente versie van de Azure PowerShell-cmdlets. [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor meer informatie over het installeren van de PowerShell-cmdlets.


## <a name="configure-forced-tunneling"></a>Geforceerde tunnels configureren

De volgende procedure kunt u gedwongen tunneling voor een virtueel netwerk opgeven. De configuratiestappen overeenkomen met het configuratiebestand VNet netwerk.



    <VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>

In dit voorbeeld wordt het virtuele netwerk 'MultiTier VNet' heeft drie subnetten: *front-end*, *Midtier*en *back-end* subnetten, met vier cross lokalen verbindingen: *DefaultSiteHQ*en drie *vertakkingen*. 

De stappen wordt de *DefaultSiteHQ* ingesteld als standaardverbinding voor site gedwongen tunneling en configureren van de Midtier en back-end subnetten gebruiken gedwongen tunneling.


1. Maak een routeringstabel. De volgende cmdlet gebruik te maken van de routetabel.

        New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"

2. Voeg een standaardroute toe aan de routeringstabel. 

    In het volgende voorbeeld wordt een standaardroute toegevoegd aan de routeringstabel in stap 1 hebt gemaakt. Merk op dat de route alleen ondersteund is het voorvoegsel van de bestemming van '0.0.0.0/0' naar de volgende hop van 'VPNGateway'.
 
        Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway

3. De routeringstabel naar de subnetten koppelen. 

    Nadat een routeringstabel gemaakt en wordt een route toegevoegd, met het volgende voorbeeld kunt toevoegen of koppelen de routetabel op een subnet van VNet. Het voorbeeld wordt de tabel 'MyRouteTable' naar de subnetten Midtier en back-end VNet MultiTier-VNet.

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"

4. Een standaard-site toewijzen voor tunneling gedwongen. 

    In de vorige stap de cmdlet voorbeeldscripts de routeringstabel gemaakt en de routetabel op twee van de subnetten VNet is gekoppeld. De laatste stap is een lokale site tussen de verbindingen met meerdere locaties van het virtuele netwerk selecteren als de standaardlocatie of tunnel.

        $DefaultSite = @("DefaultSiteHQ")
        Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"

## <a name="additional-powershell-cmdlets"></a>Extra PowerShell-cmdlets

### <a name="to-delete-a-route-table"></a>Een tabel verwijderen

    Remove-AzureRouteTable -Name <routeTableName>

### <a name="to-list-a-route-table"></a>Een routeringstabel weergeven

    Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]

### <a name="to-delete-a-route-from-a-route-table"></a>Een route verwijderen uit een tabel

    Remove-AzureRouteTable –Name <routeTableName>

### <a name="to-remove-a-route-from-a-subnet"></a>Een route van een subnet verwijderen

    Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Om de tabel die is gekoppeld aan een subnet
    
    Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>Een standaard-site verwijderen uit een VNet VPN-gateway

    Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>







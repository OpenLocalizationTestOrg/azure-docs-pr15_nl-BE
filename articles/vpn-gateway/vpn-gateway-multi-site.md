<properties 
   pageTitle="Een virtueel netwerk verbinding met meerdere sites met behulp van VPN-Gateway en PowerShell | Microsoft Azure"
   description="Dit artikel helpt u bij het verbinding maken met meerdere lokale op ruimten sites met een virtueel netwerk met een VPN-Gateway voor de klassieke implementatiemodel."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/11/2016"
   ms.author="yushwang" />

# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Een verbinding van Site naar Site toevoegen aan een VNet met een bestaande verbinding van VPN-gateway

> [AZURE.SELECTOR]
- [Resource Manager - Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Klassiek - PowerShell](vpn-gateway-multi-site.md)

Dit artikel begeleidt u bij verbindingen van Site naar Site (S2S) toevoegen aan een VPN-gateway met een bestaande verbinding met behulp van PowerShell. Dit type verbinding wordt vaak een configuratie 'meerdere vestigingen' genoemd. 

Dit artikel is van toepassing op de virtuele netwerken die zijn gemaakt met behulp van het implementatiemodel klassiek (ook bekend als Service Management). Deze stappen zijn niet van toepassing op ExpressRoute/Site naar Site naast elkaar bestaande verbinding configuraties. Zie [ExpressRoute/S2S naast elkaar bestaande verbindingen](../expressroute/expressroute-howto-coexist-classic.md) voor informatie over het naast elkaar bestaande verbindingen.

### <a name="deployment-models-and-methods"></a>Van implementatiemodellen en methoden

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

We werken in deze tabel als nieuwe artikelen en extra's beschikbaar komen voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we rechtstreeks naar uit deze tabel.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 



## <a name="about-connecting"></a>Over het aansluiten van

Met een enkele virtuele netwerk kunt u meerdere sites voor op gebouwen. Dit is met name aantrekkelijk voor hybride cloud oplossingen. Maken van een verbinding met meerdere locaties op uw gateway Azure virtueel netwerk is vergelijkbaar met andere verbindingen van Site naar Site maken. In feite kunt u een bestaande Azure VPN-gateway als de gateway dynamisch is (route gebaseerd).

Als u al een statische gateway verbinding met het virtuele netwerk hebt, kunt u het type gateway naar een dynamische zonder opnieuw op te bouwen van het virtuele netwerk om geschikt voor meerdere locaties. Voordat u wijzigt het routetype, zorg dat uw op ruimten VPN-gateway route gebaseerde VPN-configuraties ondersteunt. 

![diagram met meerdere locaties] (./media/vpn-gateway-multi-site/multisite.png "meerdere locaties")

## <a name="points-to-consider"></a>Punten te houden

**Niet mogelijk de Azure klassieke Portal gebruiken om deze virtueel netwerk te wijzigen.** In deze versie moet u wijzigingen aanbrengen in het configuratiebestand netwerk in plaats van de klassieke Azure-Portal. Als u wijzigingen in de klassieke Portal Azure aanbrengt, gaat zij de verwijzing naar meerdere locaties-instellingen voor dit virtuele netwerk overschrijven. 

Voelt heel vertrouwd met het gebruik van het configuratiebestand van het netwerk op het moment dat u de procedure met meerdere locaties hebt voltooid. Echter, als er meerdere mensen werken aan de netwerkconfiguratie van uw, moet u om ervoor te zorgen dat iedereen op de hoogte van deze beperking. Dit betekent niet dat u de klassieke Portal Azure helemaal niet gebruiken. Kunt u voor al het andere, behalve het maken van configuratiewijzigingen met dit bepaalde virtuele netwerk.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u een configuratie, controleert u of het volgende:

- Een abonnement op Azure. Als u niet al een Azure-abonnement hebt, kunt u uw [MSDN-abonnee vergoedingen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of teken omhoog voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).

- Compatibele VPN-hardware voor elke op-lokalen locatie. Controleer [Over VPN-apparaten voor verbinding met het virtuele netwerk](vpn-gateway-about-vpn-devices.md) om te controleren of het apparaat dat u wilt gebruiken is iets die bekend is om compatibel te zijn.

- Een extern gerichte openbare IPv4 IP-adres voor elke VPN-apparaat. Het IP-adres kan niet zich bevinden achter een NAT bevindt. Dit is een vereiste.

- U moet de meest recente versie van de Azure PowerShell cmdlets installeert. [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor meer informatie over het installeren van de PowerShell-cmdlets.

- Iemand die op de VPN-hardware configureren kundig is. Niet mogelijk de automatisch gegenereerde VPN om scripts te gebruiken vanaf de klassieke Portal Azure VPN-apparaten configureren. Hierdoor hebt u een sterke kennis hebben van het configureren van het apparaat voor VPN- of werken met iemand die is.

- De IP-adresbereiken die u gebruiken voor het virtuele netwerk wilt (als u dit nog niet hebt gemaakt). 

- IP-adresbereiken voor elk van de lokale netwerk-sites die u verbinding met maken kunt. U moet om ervoor te zorgen dat het IP-adres bereik voor elk van de LAN-sites die u wilt verbinding maken met elkaar niet overlappen. Anders weigert de Azure klassieke Portal of de REST API de configuratie die u wilt uploaden. 

    Als er twee lokale netwerksites dat zowel het IP-adres bereik 10.2.3.0/24 bevatten en er een pakket met een doeladres 10.2.3.3, wouldn't Azure welke site u het pakket niet verzenden wilt omdat de adresbereiken worden overlappende weten. Voorkom problemen met routing Azure is niet toegestaan voor het uploaden van een configuratiebestand met overlappende bereiken.



## <a name="1-create-a-site-to-site-vpn"></a>1. Maak een VPN van Site naar Site

Als u al een Site naar Site VPN met een dynamische routering gateway geweldig! U kunt doorgaan met [het virtuele netwerk configuratie-instellingen exporteren](#export). Als dat niet het geval is, doet u het volgende:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Als u al een virtueel netwerk van Site naar Site, maar een statische routering (op basis van beleid)-gateway heeft:

1. Wijzigen gateway in een dynamische routering. Een site met meerdere VPN-netwerk is een dynamische routering (ook bekend als route gebaseerde)-gateway vereist. Als u wilt uw gateway-type wijzigt, moet u eerst de bestaande gateway verwijderen en vervolgens maakt u een nieuwe. Zie [het type VPN-routering voor uw gateway wijzigen](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md#how-to-change-the-vpn-routing-type-for-your-gateway)voor instructies.  

2. Uw nieuwe gateway configureren en maken van uw VPN-tunnel. Zie [een VPN-Gateway in de klassieke Portal Azure configureren](vpn-gateway-configure-vpn-gateway-mp.md)voor instructies. Eerst wijzigen gateway in een dynamische routering. 

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Als u geen virtueel netwerk website:

1. Maak uw virtuele netwerk deze instructies van Site naar Site: [een virtueel netwerk met een VPN-verbinding van Site naar Site in Azure klassieke Portal maken](vpn-gateway-site-to-site-create.md).  

2. Configureer een dynamische routering gateway deze instructies: [een VPN-Gateway configureren](vpn-gateway-configure-vpn-gateway-mp.md). Zorg ervoor dat **de dynamische routering** voor uw gateway-type selecteren.

## <a name="export"></a>2. het netwerk configuratie-bestand exporteren 

Uw netwerk configuratiebestand exporteren. Het bestand dat u exporteert wordt gebruikt voor het configureren van de nieuwe instellingen voor meerdere locaties. Als u instructies voor het exporteren van een bestand nodig hebt, raadpleegt u de sectie in het artikel: [het maken van een VNet met een netwerk-configuratiebestand in de Portal Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="3-open-the-network-configuration-file"></a>3. open het configuratiebestand van het netwerk

Open het configuratiebestand van het netwerk dat u hebt gedownload in de laatste stap. Een XML-editor die u wilt gebruiken. Het bestand ziet er ongeveer als volgt:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. meerdere verwijzingen naar websites toevoegen

Wanneer u toevoegen of verwijderen van de site informatie, maakt u wijzigingen in de configuratie voor de ConnectionsToLocalNetwork/LocalNetworkSiteRef. Het toevoegen van een nieuwe lokale site verwijzing triggers Azure voor het maken van een nieuwe tunnel. In het volgende voorbeeld wordt de netwerkconfiguratie voor een verbinding met één site. Sla het bestand op als u klaar bent met het aanbrengen van wijzigingen.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

    To add additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in the example below: 

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

## <a name="5-import-the-network-configuration-file"></a>5. de netwerk configuratie-bestand importeren

Het netwerk configuratiebestand importeren. Bij het importeren van dit bestand met de wijzigingen die worden de nieuwe tunnels toegevoegd. De tunnels gebruikt de dynamische gateway die u eerder hebt gemaakt. Als u instructies voor het importeren van het bestand, raadpleegt u de sectie in het artikel: [het maken van een VNet met een netwerk-configuratiebestand in de Portal Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="6-download-keys"></a>6. toetsen downloaden

Als uw nieuwe tunnels zijn toegevoegd, gebruikt u PowerShell-cmdlet `Get-AzureVNetGatewayKey` om het IPsec IKE/vooraf-gedeelde sleutels voor elke tunnel.

Bijvoorbeeld:

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"

Als u wilt, kun je de REST *Ophalen virtuele netwerk Gateway Shared Key* API om de vooraf-gedeelde sleutels.

## <a name="7-verify-your-connections"></a>7. Controleer uw verbindingen

Controleer de status van de tunnel met meerdere locaties. Na het downloaden van de sleutels voor elke tunnel, zult u verbindingen controleren. Gebruik `Get-AzureVnetConnection` voor een lijst van virtueel netwerk van tunnels, zoals in het volgende voorbeeld wordt getoond. VNet1 is de naam van de VNet.

    Get-AzureVnetConnection -VNetName VNET1
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over VPN-Gateways, [Over VPN-Gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md).


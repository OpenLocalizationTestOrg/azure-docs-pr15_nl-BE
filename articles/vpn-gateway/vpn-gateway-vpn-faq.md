<properties 
   pageTitle="Virtueel netwerk VPN-Gateway Veelgestelde vragen | Microsoft Azure"
   description="De VPN-Gateway Veelgestelde vragen. Veelgestelde vragen over Microsoft Azure virtuele cross lokalen netwerkverbindingen, hybride configuratie verbindingen en VPN-Gateways"
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/10/2016"
   ms.author="yushwang" />

# <a name="vpn-gateway-faq"></a>VPN-Gateway Veelgestelde vragen

## <a name="connecting-to-virtual-networks"></a>Verbinding maken met virtuele netwerken

### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Kan ik een virtuele netwerken in verschillende gebieden van Azure aansluiten?
Ja. In feite is er geen beperking voor de regio. Een virtueel netwerk kunt aansluiten op een andere virtuele netwerk in dezelfde regio of in een andere regio Azure.

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Kan ik een virtuele netwerken in verschillende abonnementen aansluiten?
Ja.

### <a name="can-i-connect-to-multiple-sites-from-a-single-virtual-network"></a>Kan ik verbinding maken met meerdere sites vanuit één virtueel netwerk?

Kunt u meerdere sites met behulp van Windows PowerShell en de Azure REST API's. Zie de sectie [meerdere sites en VNet-VNet - connectiviteit](#multi-site-and-vnet-to-vnet-connectivity) Veelgestelde vragen.
## <a name="what-are-my-cross-premises-connection-options"></a>Wat zijn de opties van de verbinding tussen ruimten?

De volgende verbindingen met meerdere gebouwen worden ondersteund:

- [Site naar Site](vpn-gateway-site-to-site-create.md) : VPN-verbinding via IPsec (IKE v1 en v2 IKE). Dit type verbinding is een VPN-apparaat of RRAS vereist.

- Punt-tot-Site [-](vpn-gateway-point-to-site-create.md) -VPN-verbinding via SSTP (Secure Socket Tunneling Protocol). Deze verbinding is een VPN-apparaat niet vereist.

- [VNet met VNet](virtual-networks-configure-vnet-to-vnet-connection.md) : dit type verbinding is hetzelfde als een configuratie van Site naar Site. VNet voor VNet is een VPN-verbinding via IPsec (IKE v1 en v2 IKE). Het hoeft niet een VPN-apparaat.

- [Meerdere vestigingen](vpn-gateway-multi-site.md) – dit is een variatie op een configuratie van Site naar Site waarmee u verbinding maken met meerdere sites voor op het bedrijf een virtueel netwerk.

- [ExpressRoute](../expressroute/expressroute-introduction.md) – ExpressRoute is een directe verbinding met Azure van uw WAN, niet via het openbare Internet. Zie het [Technisch overzicht van ExpressRoute](../expressroute/expressroute-introduction.md) en de [ExpressRoute Veelgestelde vragen](../expressroute/expressroute-faqs.md) voor meer informatie.

Zie voor meer informatie over verbindingen [Over VPN-Gateway](vpn-gateway-about-vpngateways.md).

### <a name="what-is-the-difference-between-a-site-to-site-connection-and-point-to-site"></a>Wat is het verschil tussen een verbinding van Site naar Site en punt-naar-Site?

Verbindingen van **Site naar Site** , kunt u verbinding maken tussen een van de computers in uw bedrijf aan een virtuele machine of een exemplaar van de rol binnen het virtuele netwerk, afhankelijk van hoe u routering configureren. Het is een geweldige optie voor een verbinding altijd beschikbaar voor cross-ruimten en is zeer geschikt voor hybride configuraties. Dit type verbinding is gebaseerd op een IPsec VPN-toestel (hardware of zachte toestel), die aan de rand van het netwerk moet worden geïmplementeerd. Om dit type verbinding maakt, moet u de vereiste VPN-hardware en een extern gerichte IPv4-adres hebben.

**Punt-naar-Site** verbindingen kunnen u vanaf één computer overal verbinding maken op een andere waarde opgeslagen in het virtuele netwerk. De Windows-box VPN-client wordt gebruikt. U installeert als onderdeel van de configuratie van het punt-tot-Site, een certificaat en een VPN-client configuratiepakket bevat de instellingen waarmee de computer verbinding maken met een virtuele machine of een exemplaar van de rol binnen het virtuele netwerk. Het is geweldig wanneer u verbinding met een virtueel netwerk wilt maken, maar zijn niet gevonden in ruimten. Het is ook een goede optie als u geen toegang tot VPN-hardware of een extern gerichte IPv4-adres hebt, die beide vereist voor een verbinding van Site naar Site zijn. 

U kunt het virtuele netwerk gelijktijdig, zowel website en punt-naar-Site gebruiken op voorwaarde dat u de Site naar Site verbinding maken met een route-gebaseerde VPN-type voor uw gateway. Route-gebaseerde VPN-typen worden dynamische gateways genoemd in de klassieke implementatiemodel.

### <a name="what-is-expressroute"></a>Wat is ExpressRoute?

ExpressRoute kunt u particuliere verbindingen maken tussen infrastructuur die op uw locatie of in een omgeving met collocatie en datacenters van Microsoft. U kunt met ExpressRoute, tot stand brengen van verbindingen met Microsoft cloud services zoals Microsoft Azure en Office 365 een faciliteit ExpressRoute partner collocatie of rechtstreeks verbinding te maken van het bestaande WAN-netwerk (zoals een MPLS VPN verstrekt door een netwerkprovider).

ExpressRoute verbindingen bieden een betere beveiliging, meer betrouwbaarheid, hogere bandbreedte en lagere vertragingstijden dan normale verbindingen via het Internet. In sommige gevallen ExpressRoute verbindingen gebruiken om gegevens te verzenden tussen uw netwerk op gebouwen en Azure kan ook worden verkregen aanzienlijke kostenvoordelen. Als u al een verbinding tussen lokalen van uw netwerk op gebouwen naar Azure gemaakt hebt, kunt u migreren naar een ExpressRoute verbinding terwijl het virtuele netwerk intact blijft.

Raadpleeg de [Veelgestelde vragen over ExpressRoute](../expressroute/expressroute-faqs.md) voor meer informatie.

## <a name="site-to-site-connections-and-vpn-devices"></a>Verbindingen van site naar Site- en VPN-apparaten

### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>Wat moet ik rekening houden bij het selecteren van een VPN-apparaat?

We hebben een aantal standaard Site naar Site VPN-apparaten in partnerschap met leveranciers van apparaat gevalideerd. Een lijst met bekende compatibele VPN-apparaten, de bijbehorende configuratie-instructies of voorbeelden en specificaties van het apparaat vindt u [hier](vpn-gateway-about-vpn-devices.md). Alle apparaten in het apparaat families vermeld als bekende compatibel moeten met Virtual Network werken. Om te helpen uw VPN-apparaat configureren, verwijzen naar het apparaat configuratie monster of koppeling die overeenkomt met de juiste device-familie.

### <a name="what-do-i-do-if-i-have-a-vpn-device-that-isnt-in-the-known-compatible-device-list"></a>Wat moet ik doen als ik een VPN-apparaat hebt dat niet in de apparatenlijst met bekende compatibel?

Als het apparaat vermeld als een bekende VPN-apparaat niet wordt weergegeven en u wilt gebruiken voor uw VPN-verbinding, moet u controleren of deze voldoet de ondersteunde IPsec IKE/configuratie-opties en parameters vermeld [hier](vpn-gateway-about-vpn-devices.md#devices-not-on-the-compatible-list)aan. Inrichtingen die voldoen aan de minimale vereisten moeten goed werken met VPN-gateways. Neem contact op met de fabrikant van het apparaat voor verdere ondersteuning en configuratie-instructies.

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Waarom mijn beleid gebaseerde VPN-tunnel omlaag gaan wanneer verkeer gebruikt wordt?

Dit is normaal voor op basis van beleid (ook bekend als statische routering) VPN-gateways. Wanneer het verkeer dat via de tunnel voor meer dan 5 minuten niet actief is, wordt de tunnel naar beneden worden gescheurd. Wanneer het verkeer in beide richtingen stroomt is gestart, wordt de tunnel onmiddellijk worden gebracht.

### <a name="can-i-use-software-vpns-to-connect-to-azure"></a>Kan ik verbinding maken met Azure VPN software gebruiken?

We ondersteunen Windows Server 2012 Routering en RAS (RRAS), servers voor de configuratie van Site naar Site meerdere ruimten.

Andere VPN-oplossingen moeten samenwerken met onze gateway, mits zij aan de industrie standaard IPSec-implementaties voldoen. Neem contact op met de leverancier van de software voor instructies voor configuratie en ondersteuning.

## <a name="point-to-site-connections"></a>Punt-tot-Site-verbindingen

### <a name="what-operating-systems-can-i-use-with-point-to-site"></a>Welke besturingssystemen kan ik met een punt-naar-Site gebruiken?

De volgende besturingssystemen worden ondersteund:

- Windows 7 (32-bits en 64-bits)

- Windows Server 2008 R2 (alleen 64-bits)

- Windows 8 (32-bits en 64-bits)

- Windows 8.1 (32-bits en 64-bits)

- Windows Server 2012 (alleen 64-bits)

- Windows Server 2012 R2 (alleen 64-bits)

- Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Kan ik een VPN-softwareclient gebruiken voor punt-naar-Site die SSTP ondersteunt?

Nr. Ondersteuning is beperkt tot de Windows besturingssystemen die hierboven worden vermeld.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Eindpunten van het aantal VPN-client kan ik in mijn configuratie punt-naar-Site hebben?

Ondersteuning van maximaal 128 VPN-clients kunnen verbinding maken met een virtueel netwerk op hetzelfde moment.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Kan ik mijn eigen interne PKI basiscertificeringsinstantie voor een punt-naar-Site verbinding gebruiken?

Ja. Voorheen kunnen alleen zelfondertekende basiscertificaten worden gebruikt. U kunt nog steeds 20 basiscertificaten uploaden.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Kan ik bladeren proxy's en firewalls met mogelijkheid voor punt-naar-Site?

Ja. SSTP (Secure Socket Tunneling Protocol) gebruiken we tunnel door firewalls. Deze tunnel wordt weergegeven als een HTTPs-verbinding.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Als ik een clientcomputer geconfigureerd voor punt-naar-Site opnieuw opstart, wordt de VPN-verbinding automatisch opnieuw verbinden?

Standaard, wordt de client-computer niet hersteld de VPN-verbinding automatisch.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Biedt ondersteuning voor punt-naar-Site auto-reconnect en DDNS op de VPN-clients?

Auto-reconnect en DDNS worden momenteel niet ondersteund in Point-to-Site VPN-verbindingen.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Kan ik de Site naar Site hebben en punt-naar-Site-configuraties voor hetzelfde virtuele netwerk naast elkaar gebruiken?

Ja. Deze beide oplossingen werkt als u beschikt over een RouteBased VPN-type voor uw gateway. Voor het implementatiemodel klassiek moet u een dynamische gateway. We doen geen ondersteuning voor punt-naar-Site voor statische routering VPN-gateways of -VpnType PolicyBased-gateways.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Kan ik een punt-naar-Site-client verbinding maken met meerdere virtuele netwerken op hetzelfde moment configureren?

Ja, het is mogelijk. Maar de virtuele netwerken kunnen overlappende IP-voorvoegsels en moeten de punt-naar-Site-adres spaties tussen de virtuele netwerken elkaar niet overlappen.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Hoeveel doorvoer kan ik verwachten door middel van verbindingen van Site naar Site of punt-naar-Site?

Het is moeilijk om te houden van de exacte doorvoer van de VPN-tunnels. IPsec en SSTP zijn crypto zware VPN-protocollen. Doorvoer wordt ook beperkt door de latentie en bandbreedte tussen uw bedrijf en het Internet.

## <a name="gateways"></a>Gateways

### <a name="what-is-a-policy-based-static-routing-gateway"></a>Wat is een gateway (statische routering) op basis van beleid?

Gateways op basis van beleid implementeren voor VPN-verbindingen op basis van beleid. VPN-verbindingen op basis van beleid coderen en directe pakketten door het IPSec-tunnels op basis van de combinatie van adresprefixen tussen uw netwerk op gebouwen en de Azure-VNet. Het beleid (of het verkeer Selector) wordt meestal gedefinieerd als een toegangslijst in de VPN-configuratie.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Wat is een gateway (dynamische routering) op basis van een route?

Op basis van route-gateways implementeren de route-VPN's. Route-VPN's gebruiken "routes" in de IP-doorsturen of routeringstabel pakketten direct in de bijbehorende tunnel-interfaces. De tunnelinterfaces vervolgens coderen of decoderen van de pakketten en naar de tunnels. De kiezer beleid of het verkeer van de route op basis van VPN-verbindingen zijn geconfigureerd als een toepassing te (of jokertekens).

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Kan ik mijn IP-adres van de VPN-gateway krijgen voordat ik deze maken?

Nr. U moet uw gateway om het IP-adres eerst te maken. Het IP-adres wordt gewijzigd als u het verwijderen en opnieuw maken van uw VPN-gateway.

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>Hoe mijn VPN-tunnel ophalen geverifieerd?

Azure VPN wordt gebruikt voor verificatie met vooraf gedeelde sleutel (vooraf-gedeelde sleutel). We een vooraf gedeelde sleutel (PSK) gegenereerd wanneer we de VPN-tunnel maken. Kunt u de automatisch gegenereerde PSK naar uw eigen met de Set vooraf-gedeelde sleutel PowerShell-cmdlet of REST API.

### <a name="can-i-use-the-set-pre-shared-key-api-to-configure-my-policy-based-static-routing-gateway-vpn"></a>Kan ik de API vooraf-gedeelde sleutel instellen gebruiken voor het configureren van mijn beleid op basis van VPN-gateway (statische routering)?

Ja, kan de vooraf-gedeelde sleutel API instellen en PowerShell-cmdlet worden gebruikt Azure-beleid (statische) VPN- en route-(dynamische) routering VPN's configureren.

### <a name="can-i-use-other-authentication-options"></a>Kan ik de andere verificatieopties gebruiken?

Wij zijn beperkt tot het gebruik van vooraf-gedeelde sleutels (PSK) voor verificatie.

### <a name="what-is-the-gateway-subnet-and-why-is-it-needed"></a>Wat is het subnet"gateway" en waarom is het nodig?

We hebben een gatewayservice die we uitvoeren om de verbindingen tussen bedrijven. 

U moet een gateway subnet maken voor uw VNet een VPN-gateway configureren. Alle subnetten van de gateway moeten de naam GatewaySubnet goed te laten werken. Geen naam uw subnet gateway iets anders. En VMs of iets anders op het subnet van de gateway niet implementeren.

De minimale grootte van gateway subnet is volledig afhankelijk van de configuratie die u wilt maken. Hoewel het voor het maken van een subnet gateway /29 voor sommige configuraties zo klein mogelijk is, raden wij aan dat u een gateway-subnet van /28 of groter maken (/ 28, /27, /26 enz.). 

### <a name="can-i-deploy-virtual-machines-or-role-instances-to-my-gateway-subnet"></a>Kan ik virtuele Machines of rol exemplaren aan mijn subnet gateway implementeren?

Nr.

### <a name="how-do-i-specify-which-traffic-goes-through-the-vpn-gateway"></a>Hoe geef ik die verkeer via de VPN-gateway gaat?

Als u de klassieke Portal Azure toevoegen elk bereik dat u wilt voor het virtuele netwerk op de pagina netwerken onder lokale netwerken via de gateway wordt verzonden.

### <a name="can-i-configure-forced-tunneling"></a>Kan ik de gedwongen tunnels configureren?

Ja. Zie [Configure gedwongen tunneling](vpn-gateway-about-forced-tunneling.md).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-to-connect-to-my-on-premises-network"></a>Er mijn eigen VPN-server in Azure instellen en verbinding maken met mijn netwerk op ruimten gebruiken?

Ja, u kunt uw eigen gateways voor VPN- of servers in Azure van de markt Azure of het maken van uw eigen VPN-routers implementeren. U moet de gebruiker gedefinieerde routes in het virtuele netwerk zodat verkeer correct wordt doorgestuurd tussen de netwerken in gebouwen en de subnetten virtueel netwerk configureren.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Waarom zijn bepaalde poorten geopend op Mijn VPN-gateway?

Deze zijn vereist voor communicatie-infrastructuur Azure. Ze zijn beveiligd (vergrendeld) door Azure certificaten. Zonder de juiste certificaten, externe entiteiten, met inbegrip van de klanten van de gateways worden niet kunnen veroorzaken geen effect op de eindpunten.

Een VPN-gateway is in feite een multihomed apparaat met één NIC toegang te krijgen tot het particuliere netwerk van de klant en één NIC met het openbare netwerk. Azure infrastructuur entiteiten kunnen niet gebruik te maken van klant particuliere netwerken om redenen van compatibiliteit, zodat ze nodig hebben om het gebruik van openbare eindpunten voor de infrastructuur voor communicatie. De openbare eindpunten worden periodiek gescand door Azure beveiligingscontrole.


### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>Meer informatie over gateway typen, voorschriften en doorvoer

Zie voor meer informatie [Over VPN-Gateway-instellingen](vpn-gateway-about-vpn-gateway-settings.md).

## <a name="multi-site-and-vnet-to-vnet-connectivity"></a>Meerdere locaties en VNet-VNet-connectiviteit

### <a name="which-type-of-gateways-can-support-multi-site-and-vnet-to-vnet-connectivity"></a>Welk type gateways ondersteunt meerdere locaties en VNet-VNet-connectiviteit?

Alleen route op basis van VPN-verbindingen (dynamische routering).

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>Kan ik verbinding maken met een VNet met een VPN-Type RouteBased met een PolicyBased VPN-type op een andere VNet?

Nee, beide virtuele netwerken moeten gebruiken op basis van route (dynamische routering) VPN's.

### <a name="is-the-vnet-to-vnet-traffic-secure"></a>De VNet-VNet-verkeer veilig is?

Ja, het is beveiligd door IPsec/IKE-codering.

### <a name="does-vnet-to-vnet-traffic-travel-over-the-azure-backbone"></a>VNet-VNet-verkeer via de backbone Azure reizen?

Ja.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Hoeveel op gebouwen-sites en virtuele netwerken kan een virtueel netwerk verbinding maken met?

Max. 10 gecombineerd voor de Basic en dynamische routering standaard-gateways; 30 voor hoge prestaties VPN-gateways.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Kan ik gebruik Point-to-Site VPN-verbindingen met het virtuele netwerk met meerdere VPN-tunnels

Ja, VPN's punt-naar-Site (P2S) kunnen worden gebruikt met een VPN-verbinding te maken met meerdere sites voor op gebouwen en andere virtuele netwerken gateways.

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Kan ik meerdere tunnels configureren tussen mijn virtuele netwerk en Mijn op gebouwen-site met meerdere site VPN

Nee, redundante tunnels tussen een Azure virtual network en een site op de ruimten worden niet ondersteund.

### <a name="can-there-be-overlapping-address-spaces-among-the-connected-virtual-networks-and-on-premises-local-sites"></a>Kan er worden overlappende adresruimten onder de virtuele netwerken verbonden en lokale sites op het bedrijf?

Nr. Overlappende adresruimten zal de netwerk configuratie bestand uploaden of een 'virtueel netwerk maken"mislukt.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Krijg ik meer bandbreedte met meer Site naar Site VPN's dan voor één virtueel netwerk?

Nee, alle VPN-tunnels, met inbegrip van de Point-to-Site VPN-verbindingen, delen dezelfde Azure VPN-gateway en de beschikbare bandbreedte.

### <a name="can-i-use-azure-vpn-gateway-to-transit-traffic-between-my-on-premises-sites-or-to-another-virtual-network"></a>Kan ik op de doorvoer van verkeer tussen Mijn sites op het bedrijf of naar een ander virtueel netwerk Azure VPN-gateway gebruiken?

**Klassieke implementatiemodel.**<br>
Doorgaand verkeer via Azure VPN-gateway is mogelijk met behulp van het implementatiemodel klassiek, maar afhankelijk van het statisch gedefinieerde adresruimten in het configuratiebestand van het netwerk. BGP is nog niet ondersteund met Azure virtuele netwerken en VPN-gateways met behulp van het implementatiemodel klassiek. Zonder BGP is transit-adresruimten handmatig definiëren helemaal fout gevoelig en niet aanbevolen.<br>
**Het implementatiemodel Resource Manager**<br>
Als u het implementatiemodel Resource Manager, Zie de sectie [BGP](#bgp) voor meer informatie.

### <a name="does-azure-generate-the-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-the-same-virtual-network"></a>Genereert Azure dezelfde IPsec/IKE vooraf-gedeelde sleutel voor Mijn VPN-verbindingen voor hetzelfde virtuele netwerk

Nee, genereert Azure standaard verschillende vooraf-gedeelde sleutels voor de verschillende VPN-verbindingen. Kunt u de Set VPN-Gateway sleutel REST API of PowerShell-cmdlet de waarde van de sleutel die u wilt instellen. De sleutel moet de alfanumerieke tekenreeks met een lengte tussen 1 tot 128 tekens.

### <a name="does-azure-charge-for-traffic-between-virtual-networks"></a>Is Azure voor verkeer tussen virtuele netwerken brengen?

Voor verkeer tussen verschillende Azure virtuele netwerken, Azure brengt alleen voor het verkeer doorlopen van de ene Azure regio naar de andere. Het toeslagpercentage wordt vermeld in de Azure [VPN-Gateway prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway/) pagina.


### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-to-my-expressroute-circuit"></a>Kan ik een virtueel netwerk aansluiten met IPsec VPN's op mijn ExpressRoute circuit?

Ja, dit wordt ondersteund. Zie voor meer informatie, [ExpressRoute configureren en van Site naar Site VPN-verbindingen die worden gecombineerd](../expressroute/expressroute-howto-coexist-classic.md).

## <a name="bgp"></a>BGP

[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 



## <a name="cross-premises-connectivity-and-vms"></a>Cross-premises connectiviteit en VMs

### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-to-the-vm"></a>Als mijn virtuele machine in een virtueel netwerk en er een verbinding tussen ruimten, hoe moet ik verbinding maken met de VM?

U hebt een paar opties. Als u RDP ingeschakeld en u een eindpunt hebt gemaakt, kunt u aan uw virtuele machine met behulp van de VIP. In dat geval geeft u het VIP en de poort waarmee u verbinding wilt maken. U moet de poort configureren op de virtuele machine voor het verkeer. Normaal gesproken zou u gaat u naar de klassieke Azure-Portal en de instellingen voor de RDP-verbinding op uw computer opslaan. De instellingen bevatten de benodigde verbindingsinformatie.

Als u een virtueel netwerk met meerdere gebouwen verbinding geconfigureerd hebt, kunt u aan uw virtuele machine via de interne DIP of een particulier IP-adres. U kunt ook verbinding maken met de virtuele machine door interne DIP vanaf een andere virtuele machine die zich op het virtuele netwerk bevindt. U kunt RDP niet aan uw virtuele machine met behulp van de DIP als u verbinding vanaf een locatie buiten het virtuele netwerk maakt. Bijvoorbeeld, als u een punt-naar-Site van een virtueel netwerk geconfigureerd en u geen verbinding vanaf uw computer maken, kunt u de virtuele machine met DIP.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-the-traffic-from-my-vm-go-through-that-connection"></a>Als de virtuele machine in een virtueel netwerk met meerdere gebouwen connectiviteit, het verkeer van mijn VM gaat via die verbinding?

Nr. Alleen het verkeer een bestemming IP die deel uitmaakt van het virtuele netwerk lokaal netwerk IP-adresbereiken die u hebt opgegeven gaat via de gateway virtueel netwerk. Verkeer is een bestemming die zich binnen het virtuele netwerk IP binnen het virtuele netwerk blijft. Het andere verkeer wordt verzonden via de taakverdeling met openbare netwerken of als gedwongen tunneling wordt gebruikt, verzonden via de Azure VPN-gateway. Als u oplossen wilt, is het belangrijk om ervoor te zorgen dat u beschikt over alle bereiken die worden vermeld in het lokale netwerk dat u wilt verzenden via de gateway. Controleer of dat het lokale netwerk adresbereiken elkaar niet met een van de adresbereiken in het virtuele netwerk overlappen. Bovendien wilt u controleren of dat de DNS-server die u gebruikt de naam wordt omgezet naar het juiste IP-adres.


## <a name="virtual-network-faq"></a>Virtueel netwerk Veelgestelde vragen

U weergeven extra virtueel netwerkgegevens in de [Veelgestelde vragen over het virtuele netwerk](../virtual-network/virtual-networks-faq.md).
 

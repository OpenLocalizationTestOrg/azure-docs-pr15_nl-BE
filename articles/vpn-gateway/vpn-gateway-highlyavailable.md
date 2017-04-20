<properties
   pageTitle="Overzicht van de maximaal beschikbare configuraties met Azure VPN-Gateways | Microsoft Azure"
   description="Dit artikel bevat een overzicht van de maximaal beschikbare configuratieopties met Azure VPN-Gateways."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/24/2016"
   ms.author="yushwang"/>

# <a name="highly-available-cross-premises-and-vnet-to-vnet-connectivity"></a>Maximaal beschikbare Cross-ruimten en VNet-VNet-connectiviteit

Dit artikel bevat een overzicht van de maximaal beschikbare configuratieopties voor de cross-ruimten en VNet-VNet-connectiviteit met Azure VPN-gateways.

## <a name = "activestandby"></a>Over Azure VPN-gateway redundantie

Elke Azure VPN-gateway bestaat uit twee exemplaren in een actieve stand-by-configuratie. Voor gepland onderhoud of niet-geplande onderbrekingen die de actieve instantie, zou de stand-by-instantie (failover) automatisch overnemen en de VNet-VNet-verbindingen of VPN-S2S hervatten. De schakeloptie via zal een korte onderbreking. Voor gepland onderhoud, moet de connectiviteit binnen 10 tot 15 seconden worden teruggezet. Voor problemen met niet-gepland, wordt het herstel van de verbinding niet langer, ongeveer 1 minuut tot 1 en een half minuten in het ergste geval. Voor P2S VPN-clientverbindingen met de gateway, de P2S verbindingen wordt verbroken en de gebruikers moeten vanaf de clientcomputers opnieuw verbinding te maken.

![Actieve stand-by-](./media/vpn-gateway-highlyavailable/active-standby.png)

## <a name="highly-available-cross-premises-connectivity"></a>Connectiviteit met maximaal beschikbare Cross-gebouwen

Om betere beschikbaarheid voor verbindingen met meerdere gebouwen, zijn er een aantal opties beschikbaar:

- Meerdere op ruimten VPN-apparaten
- Actieve Azure VPN-gateway
- Combinatie van beide

### <a name = "activeactiveonprem"></a>Meerdere op ruimten VPN-apparaten

U kunt meerdere VPN-apparaten uit uw netwerk op gebouwen Azure VPN-gateway verbinding maken zoals in het volgende diagram:

![Meerdere On-Premises VPN](./media/vpn-gateway-highlyavailable/multiple-onprem-vpns.png)

Deze configuratie biedt meerdere actieve tunnels van de dezelfde Azure VPN-gateway naar uw apparatuur op locatie op dezelfde locatie. Er zijn enkele beperkingen en vereisten:

1. U moet meerdere S2S VPN-verbindingen maken van uw VPN-apparaten naar Azure. Wanneer u meerdere VPN-apparaten in hetzelfde netwerk op ruimten naar Azure, moet u één LAN gateway voor elke VPN-apparaat en één verbinding maken via de Azure VPN-gateway naar de gateway voor het lokale netwerk.

2. Het lokale netwerkgateways die overeenkomt met uw VPN-apparaten moeten unieke openbare IP-adressen in de eigenschap 'GatewayIpAddress' hebben.

3. BGP is vereist voor deze configuratie. Elke lokale netwerkgateway voor een VPN-apparaat moet een unieke BGP peer IP-adres dat is opgegeven in de eigenschap 'BgpPeerIpAddress' hebben.

4. Het veld van de eigenschap AddressPrefix in elke lokale netwerkgateway moet niet overlappen. Moet u de "BgpPeerIpAddress" in /32 CIDR-indeling in het veld AddressPrefix, bijvoorbeeld 10.200.200.254/32.

5. BGP dient u dezelfde prefixen van hetzelfde-lokale netwerkprefixen voor de Azure VPN-gateway, en het verkeer doorgestuurd via deze tunnels tegelijk adverteren.

6. Elke verbinding wordt afgeboekt op het maximum aantal tunnels voor de Azure VPN-gateway, 10 voor Basic en Standard SKU's, en 30 voor de SKU HighPerformance. 

In deze configuratie is de Azure VPN-gateway nog steeds in de actieve stand-by-modus zodat het dezelfde overname bij storing en een korte onderbreking nog steeds als beschreven [hierboven gebeurt](#activestandby). Maar deze instelling beschermt tegen storingen of onderbrekingen op uw netwerk op gebouwen en VPN-apparaten.
 
### <a name="active-active-azure-vpn-gateway"></a>Actieve Azure VPN-gateway

U kunt nu een Azure VPN-gateway maken in een actieve configuratie, waarbij beide exemplaren van de VMs gateway S2S VPN-tunnels op het VPN-apparaat op ruimten maken zoals in het volgende diagram wordt weergegeven:

![Actieve](./media/vpn-gateway-highlyavailable/active-active.png)

In deze configuratie, elk exemplaar Azure gateway heeft een unieke openbare IP-adres en elk een IPsec/IKE S2S VPN-tunnel naar uw op ruimten VPN-apparaat is opgegeven in uw lokale netwerkgateway en een verbinding maken. Houd er rekening mee dat beide VPN-tunnels daadwerkelijk deel van dezelfde verbinding uitmaken. Toch moet u uw apparaat op ruimten VPN accepteren of stand van twee S2S VPN-tunnels die twee Azure VPN-gateway openbare IP-adressen configureren.

Omdat de Azure gateway exemplaren in de actieve configuratie, worden het verkeer van uw Azure virtueel netwerk naar uw netwerk op ruimten gerouteerd via beide tunnels tegelijk, zelfs als uw apparaat op ruimten VPN een tunnel kan voorkeur boven de andere. Opmerking Hoewel de dezelfde TCP- of UDP-stroom wordt altijd via de tunnel of de pad, tenzij een gebeurtenis onderhoud wordt uitgevoerd op een van de instanties.

Wanneer een geplande onderhoud of een niet-geplande gebeurtenis met een gateway-instantie gebeurt, wordt de IPSec-tunnel uit die sessie met het apparaat op ruimten VPN-verbinding verbroken. De bijbehorende routes op uw VPN-apparaten moeten worden verwijderd of automatisch wordt ingetrokken, zodat het verkeer zal worden overgeschakeld op de andere actieve IPSec-tunnel. De Azure betreft, de schakeloptie via gebeurt automatisch uit het desbetreffende exemplaar met het actieve exemplaar.

### <a name="dual-redundancy-active-active-vpn-gateways-for-both-azure-and-on-premises-networks"></a>Dual-redundantie: actieve VPN-gateways voor Azure- en op gebouwen-netwerken

De meest betrouwbare optie is een combinatie actieve gateways op het netwerk en de Azure, zoals in het onderstaande diagram wordt weergegeven.

![Dual redundantie](./media/vpn-gateway-highlyavailable/dual-redundancy.png)

Hier maken en instellen van de Azure VPN-gateway in de configuratie van een actieve en twee lokale netwerkgateways maken en twee aansluitingen voor de twee op-premises VPN-apparaten zoals hierboven beschreven. Het resultaat is een volledig mesh connectiviteit van 4 IPSec-tunnels tussen uw Azure virtueel netwerk en uw netwerk op gebouwen.

Gateways en -tunnels zijn actief de Azure kant, zodat het verkeer worden verdeeld tussen alle 4 tunnels tegelijk, hoewel elke TCP- of UDP-stroom opnieuw dezelfde tunnel of pad van de Azure kant volgt. Hoewel door de spreiding van het verkeer, kan er iets hogere doorvoersnelheid via de IPSec-tunnels, is het voornaamste doel van deze configuratie voor maximale beschikbaarheid. En door de statistische aard van de verspreiding, is het moeilijk voor de meting van hoe verschillende toepassing verkeer voorwaarden zijn van invloed op de geaggregeerde doorvoer.

Deze topologie moet twee lokale netwerkgateways en twee verbindingen met het paar voor bedrijfsruimten VPN-apparaten ondersteunen en BGP-protocol is vereist voor de twee verbindingen op hetzelfde netwerk als op locatie. Deze vereisten zijn hetzelfde als de [hierboven](#activeactiveonprem). 

## <a name="highly-available-vnet-to-vnet-connectivity-through-azure-vpn-gateways"></a>Maximaal beschikbare VNet-VNet-connectiviteit via Azure VPN-Gateways

Dezelfde actieve configuratie kan ook van toepassing op verbindingen Azure VNet naar VNet. U kunt actieve VPN-gateways voor zowel virtuele netwerken maken en ze met elkaar verbinden aan de dezelfde volledige mesh connectiviteit formulier 4 tunnels tussen de twee VNets, zoals in het onderstaande diagram:

![VNet-VNet-](./media/vpn-gateway-highlyavailable/vnet-to-vnet.png)

Op deze manier zijn altijd een paar tunnels tussen de twee virtuele netwerken op gebeurtenissen gepland onderhoud, nog betere beschikbaarheid te bieden. Hoewel de dezelfde topologie voor cross-lokalen connectiviteit twee verbindingen vereist, moet de VNet-VNet-topologie hierboven slechts één verbinding voor elke gateway. BGP is ook optioneel tenzij transit routering via de VNet-VNet-verbinding vereist is.


## <a name="next-steps"></a>Volgende stappen

Zie [Actieve VPN-Gateways voor Cross-ruimten en VNet-VNet - verbindingen configureren](vpn-gateway-activeactive-rm-powershell.md) voor de stappen om actieve cross-ruimten en VNet-VNet-verbindingen te configureren.

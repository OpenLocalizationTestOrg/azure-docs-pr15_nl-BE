<properties
   pageTitle="Overzicht van BGP met Azure VPN-Gateways | Microsoft Azure"
   description="Dit artikel biedt een overzicht van BGP met Azure VPN-Gateways."
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
   ms.date="06/16/2016"
   ms.author="yushwang"/>

# <a name="overview-of-bgp-with-azure-vpn-gateways"></a>Overzicht van BGP met Azure VPN-Gateways

Dit artikel biedt een overzicht van BGP (Border Gateway Protocol)-ondersteuning in Azure VPN-Gateways.

## <a name="about-bgp"></a>Over BGP

BGP is de standaard routeringsprotocol vaak gebruikt op het Internet voor het uitwisselen van informatie voor Routering en bereikbaarheid tussen twee of meer netwerken. Wanneer gebruikt in de context van een virtuele netwerken Azure, BGP Azure VPN-Gateways kan en uw op ruimten VPN-apparaten, wel BGP peers neighbors voor de uitwisseling van "routes" die beide gateways op de beschikbaarheid en bereikbaarheid van de voorvoegsels te gaan via de routers of gateways informeert betrokken. BGP kunt transit routering tussen meerdere netwerken doorgeven van een gateway BGP van een BGP peer aan alle andere BGP peers hoort routes verder.
 
### <a name="why-use-bgp"></a>BGP is het nut?

BGP is een optionele functie die u met Azure Route gebaseerde VPN-gateways gebruiken kunt. U moet ook controleren of op gebouwen VPN-apparaten ondersteunen BGP voordat u de functie inschakelen. U kunt blijven gebruiken VPN-Azure gateways en VPN-apparaten op gebouwen zonder BGP. Het is het equivalent van het gebruik van statische routes (zonder BGP) *vergeleken* met behulp van dynamische routering met BGP tussen netwerken en Azure.

Er zijn verschillende nieuwe mogelijkheden met BGP en voordelen:

#### <a name="support-automatic-and-flexible-prefix-updates"></a>Ondersteuning voor automatische en flexibele voorvoegsel updates

Met BGP hoeft u alleen een minimale voorvoegsel declareren naar een specifieke BGP peer over de IPSec-S2S VPN-tunnel. Kan zijn zo klein als een voorvoegsel voor de host (/ 32) van het BGP peer IP-adres van uw apparaat op ruimten VPN. U kunt bepalen welke op-premises netwerkprefixen die u adverteren voor Azure wilt zodat uw Azure Virtual Network voor toegang tot.
    
U kunt ook reclame maken voor een grotere voorvoegsels waarin sommige van uw VNet-adresprefixen, zoals een grote particuliere IP-adresruimte (bijv. 10.0.0.0/8). Opmerking: hoewel de voorvoegsels mag niet identiek zijn met een van de VNet-voorvoegsels. Deze routes die identiek zijn aan uw VNet voorvoegsels wordt afgewezen.

>[AZURE.IMPORTANT] Op dit moment worden de standaardroute (0.0.0.0/0) naar Azure VPN-gateways reclame geblokkeerd. Verdere update zal worden verstrekt zodra deze mogelijkheid is ingeschakeld.

#### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a>Ondersteuning voor meerdere tunnels tussen een VNet en een site op gebouwen met automatische failover op basis van BGP

U kunt meerdere verbindingen tot stand brengen tussen uw Azure-VNet en op ruimten VPN-apparaten op dezelfde locatie. Deze mogelijkheid biedt meerdere tunnels (paden) tussen de twee netwerken in een actieve configuratie. Als een van de tunnels wordt verbroken, wordt de bijbehorende routes via BGP ingetrokken en het verkeer automatisch verplaatst naar de overige tunnels.
    
In het volgende diagram ziet u een eenvoudig voorbeeld van deze instellingen beschikbaar:
    
![Meerdere paden van actieve](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

#### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a>Ondersteuning voor douanevervoer tussen de netwerken op gebouwen en meerdere Azure VNets routering

BGP kan meerdere gateways te leren en doorgeven van voorvoegsels van verschillende netwerken, ongeacht of zij rechtstreeks of indirect verbonden bent. Hierdoor kunnen onderweg routering met Azure VPN-gateways tussen de sites in ruimten of in meerdere virtuele netwerken van Azure.
    
In het volgende diagram ziet u een voorbeeld van een topologie met meerdere paden die u kunt verkeer tussen de twee netwerken op locatie via gateways binnen de Microsoft Networks Azure VPN-doorvoer met meerdere hops:

![Met meerdere hops doorvoer](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faqs"></a>BGP-Veelgestelde vragen


[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 




## <a name="next-steps"></a>Volgende stappen

Zie [aan de slag met BGP op Azure VPN-gateways](./vpn-gateway-bgp-resource-manager-ps.md) voor stappen BGP voor uw cross-ruimten en VNet-VNet-verbindingen configureren.


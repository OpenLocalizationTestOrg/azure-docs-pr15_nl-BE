<properties 
   pageTitle="VPN-Gateway planning en ontwerp | Microsoft Azure"
   description="Meer informatie over VPN-Gateway planning en ontwerp voor meerdere gebouwen en hybride VNet-VNet-verbindingen"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc"/>

# <a name="planning-and-design-for-vpn-gateway"></a>Planning en ontwerp voor een VPN-Gateway

Plannen en ontwerpen van uw cross-ruimten en VNet-VNet-configuraties kunnen eenvoudig of ingewikkeld zijn, afhankelijk van de netwerkvereisten te worden. Dit artikel begeleidt u bij de algemene overwegingen voor planning en ontwerp.

## <a name="planning"></a>Planning


### <a name="compare"></a>Cross-premises connectiviteitsopties

Als u uw sites op ruimten veilige verbinding met een virtueel netwerk wilt, hebt u drie verschillende manieren om dit te doen: Site naar Site punt-naar-Site en ExpressRoute. Vergelijk de verschillende cross-premises verbindingen die beschikbaar zijn. De optie die u kiest hangt af van diverse overwegingen, zoals:


- Wat voor soort doorvoersnelheid is voor uw oplossing nodig?
- Wilt u communiceren via het openbare Internet via een veilig VPN, of een particuliere verbinding?
- Hebt u een openbaar IP-adres kunnen worden gebruikt?
- U wilt werken met een VPN-apparaat? Als dit het geval is, is het compatibel?
- Bent u een paar computers verbinden of wilt u een permanente verbinding voor uw site?
- Wat voor soort VPN-gateway is vereist voor de oplossing die u wilt maken?
- Welke gateway SKU moet gebruiken?


De volgende tabel kunt u bepalen van de beste optie voor connectiviteit voor uw oplossing.


[AZURE.INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]



### <a name="gwrequire"></a>Vereisten voor gateway VPN-type en SKU

[AZURE.INCLUDE [vpn-gateway-gwsku](../../includes/vpn-gateway-gwsku-include.md)]

Zie voor meer informatie over SKU's gateway [VPN-Gateway-instellingen](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

#### <a name="aggregate-throughput-by-sku-and-vpn-type"></a>Geaggregeerde doorvoer per SKU en VPN-type

De volgende tabel ziet u de typen gateway en de geschatte totale doorvoer. De geschatte totale doorvoer mogelijk een beslissende factor voor het ontwerp.
Prijzen is het verschil tussen de gateway SKU's. Zie [VPN-Gateway prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway/)voor informatie over prijzen. Deze tabel geldt voor de resourcemanager en de van klassieke implementatiemodellen.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

#### <a name="supported-configurations-by-sku-and-vpn-type"></a>Ondersteunde configuraties op SKU's en VPN-type

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 

### <a name="wf"></a>Workflow

De volgende lijst geeft een overzicht van de algemene workflow voor cloud verbindingen:

1.  Ontwerpen en plannen van uw topologie connectiviteit en lijst van de adresruimte van alle netwerken die u wilt verbinden.
2.  Een Azure virtueel netwerk maken. 
3.  Een VPN-gateway voor het virtuele netwerk maken.
4.  Maken en verbindingen met netwerken op lokalen of andere virtuele netwerken configureren (indien nodig).
5.  Verbinding maken en configureren een punt-naar-Site voor uw Azure VPN-gateway (indien nodig).
 

## <a name="design"></a>Ontwerp

### <a name="topologies"></a>Verbinding topologieën

Bekijk eerst de diagrammen die u in het artikel [Over VPN-Gateway](vpn-gateway-about-vpngateways.md) . Het artikel bevat basisdiagrammen de implementatiemodellen voor elke topologie (Resource Manager of klassiek) en welke implementatie van programma's die u kunt gebruiken voor de implementatie van uw configuratie.   

### <a name="designbasics"></a>Grondbeginselen van het ontwerp

De volgende secties worden de basisbeginselen van VPN-gateway. Bedenk ook [beperkingen voor VPN services](../articles/azure-subscription-service-limits.md#networking-limits).


#### <a name="subnets"></a>Over subnetten

Als u verbindingen maken wilt, moet u rekening houden met het bereiken van uw subnet. U kunt geen overlappende adresbereiken subnet. Een overlappende subnet is als een virtueel netwerk of locatie op ruimten bevat dezelfde adresruimte met de andere locatie. Dit betekent dat u moet uw netwerk engineers voor uw lokale op gebouwen-netwerken kunnen fungeren van een bereik te gebruiken voor uw Azure IP-adressering ruimte/subnetten. U moet de adresruimte die niet wordt gebruikt in het netwerk van lokale op gebouwen. 

Vermijden van elkaar overlappende subnetten is ook belangrijk wanneer u met VNet-VNet-verbindingen werkt. Als de subnetten overlappen en een IP-adres in zowel de verzendende en de bestemming, VNets bestaat, VNet-VNet-het lukt niet verbindingen. Azure kan niet de gegevens doorsturen naar de andere VNet omdat het doeladres deel van de verzendende VNet uitmaakt. 

VPN-Gateways moeten met een specifiek subnet, een gateway-subnet genoemd. Alle subnetten van de gateway moeten de naam GatewaySubnet goed te laten werken. Zorg ervoor dat u niet een andere naam voor uw gateway-subnet naam en VMs of iets anders op het subnet van de gateway niet implementeren. Zie [Gateway subnetten](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>Over het lokale netwerkgateways

De gateway voor het lokale netwerk staat doorgaans op uw locatie op gebouwen. In het implementatiemodel klassiek de gateway voor het lokale netwerk een lokale netwerksite genoemd. Wanneer u een lokale netwerk-gateway configureert, u een naam geven, geeft u het openbare IP-adres van de inrichting van ruimten VPN- en de adresprefixen die zich op de locatie op de locatie opgeven. Azure wordt gekeken naar de bestemming adresprefixen voor netwerkverkeer, raadpleegt de configuratie die u hebt opgegeven voor het lokale netwerkgateway en routeert pakketten dienovereenkomstig. U kunt deze adresprefixen zo nodig wijzigen. Zie [lokale netwerkgateways](vpn-gateway-about-vpn-gateway-settings.md#lng)voor meer informatie.


#### <a name="gwtype"></a>Over gateway typen

De juiste gateway tekst selecteren voor uw topologie is belangrijk. Als u het verkeerde type, werkt de gateway niet goed. Het gateway-type geeft aan hoe de gateway zelf is verbonden en een vereiste configuratie-instelling voor het implementatiemodel Resource Manager.

De gateway zijn:

- VPN
- ExpressRoute

#### <a name="connectiontype"></a>Over verbindingstypen

Elke configuratie is vereist voor een specifiek verbindingstype. De verbindingstypen zijn:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient


#### <a name="vpntype"></a>Over VPN-typen

Elke configuratie vereist een specifieke VPN-type. Als u twee configuraties, zoals het maken van een verbinding van Site naar Site en een punt-naar-Site verbinding met de dezelfde VNet wilt combineren, moet u een VPN-type die voldoet aan de eisen van beide verbindingen gebruiken.

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)] 

De volgende tabellen tonen de VPN-type zoals deze wordt toegewezen aan elke verbindingsconfiguratie. Controleer of de VPN-type voor uw gateway overeenkomt met de configuratie die u wilt maken. 


[AZURE.INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)] 

### <a name="devices"></a>VPN-apparaten voor verbindingen van Site naar Site

Voor het configureren van een verbinding van Site naar Site, ongeacht het implementatiemodel, moet u de volgende items:

- Een VPN-apparaat dat compatibel is met Azure VPN-gateways
- Een openbare IPv4 IP-adres dat niet achter een NAT-apparaat

U moet uw VPN-apparaat configureren ervaring hebben of iemand die het apparaat voor u configureren kunt. Zie voor meer informatie over VPN-apparaten, [over VPN-apparaten](vpn-gateway-about-vpn-devices.md). VPN-apparaten artikel bevat informatie over apparaten gevalideerde, eisen voor apparaten die niet zijn gevalideerd en koppelingen naar apparaat configuratiedocumenten, indien beschikbaar.

### <a name="forcedtunnel"></a>Houd rekening met geforceerde tunnel routering

Voor de meeste configuraties, kunt u gedwongen tunneling. Geforceerde tunneling kunt u omleiding of "kracht" alle Internet-verkeer terug naar uw locatie on-premises via een Site naar Site VPN-tunnel voor inspectie en controle. Dit is een essentiële vereiste voor de meeste zakelijke IT beleid. 

Zonder geforceerde tunneling zal Internet-verkeer dat uit uw VMs in Azure altijd Bladeren van Azure netwerkinfrastructuur rechtstreeks uit met het Internet, zonder de optie om te controleren of het verkeer wilt controleren. Onbevoegde toegang tot Internet kan mogelijkerwijs leiden tot vrijgeven van informatie of andere soorten inbreuken op de beveiliging.

**Gedwongen tunneling-diagram**

![Verbinding gedwongen Tunneling] (./media/vpn-gateway-plan-design/forced-tunnel.png "gedwongen tunneling")

Een geforceerde tunneling verbinding kan worden geconfigureerd in beide implementatiemodellen en met behulp van verschillende hulpmiddelen. Zie de volgende tabel voor meer informatie. Deze tabel bijgewerkt zodra nieuwe artikelen en nieuwe implementatiemodellen extra hulpprogramma's beschikbaar voor deze configuratie. Wanneer een artikel beschikbaar is, koppelen we rechtstreeks op uit de tabel.

[AZURE.INCLUDE [vpn-gateway-table-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 



## <a name="next-steps"></a>Volgende stappen

Zie de [Veelgestelde vragen over het VPN-Gateway](vpn-gateway-vpn-faq.md) en [Over VPN-Gateway](vpn-gateway-about-vpngateways.md) -artikelen voor meer informatie om u te helpen bij uw ontwerp.

Zie voor meer informatie over specifieke gateway-instellingen, [Over VPN-Gateway-instellingen](vpn-gateway-about-vpn-gateway-settings.md).





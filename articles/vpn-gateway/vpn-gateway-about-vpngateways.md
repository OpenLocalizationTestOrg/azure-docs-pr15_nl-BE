<properties 
   pageTitle="Over VPN-Gateway | Microsoft Azure"
   description="Informatie over verbindingen van VPN-Gateway voor virtuele netwerken Azure."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway"></a>Over VPN-Gateway


Een virtueel netwerkgateway wordt gebruikt voor het verzenden van netwerkverkeer tussen Azure virtuele netwerken en locaties op gebouwen en tussen virtuele netwerken in Azure (VNet VNet). Wanneer u een VPN-gateway configureert, moet u het maken en configureren van een virtueel netwerkgateway en een virtueel netwerk gateway-verbinding.

In het implementatiemodel Resource Manager als u een virtuele gateway netwerkbron, maakt opgeven u verschillende instellingen. Een van de vereiste instellingen is '-GatewayType'. Er zijn twee virtuele gateway: VPN- en ExpressRoute. 

Wanneer netwerkverkeer op een specifieke persoonlijke verbinding worden verzonden, gebruikt u het type gateway 'ExpressRoute'. Dit is ook een gateway ExpressRoute genoemd. Wanneer netwerkverkeer wordt verzonden versleuteld via een openbare verbinding, gebruikt u het type gateway 'Vpn'. Dit is een VPN-gateway genoemd. Site naar Site punt-naar-Site en VNet-VNet-verbindingen gebruiken voor een VPN-gateway.

Elke virtuele netwerk kan slechts één virtuele netwerkgateway per type gateway hebben. U kunt bijvoorbeeld één virtueel netwerkgateway die wordt gebruikt - GatewayType ExpressRoute en met - GatewayType Vpn hebt. Dit artikel richt zich voornamelijk op de VPN-Gateway. Zie voor meer informatie over ExpressRoute, [ExpressRoute technisch overzicht](../expressroute/expressroute-introduction.md).

## <a name="pricing"></a>Prijzen

[AZURE.INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)] 


## <a name="gateway-skus"></a>Gateway-SKU 's

[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

Zie voor meer informatie over SKU's gateway, [Gateway SKU's](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

### <a name="estimated-aggregate-throughput-by-sku"></a>Geschatte totale doorvoer per SKU

De volgende tabel ziet u de typen gateway en de geschatte totale doorvoer. Deze tabel geldt voor de resourcemanager en de van klassieke implementatiemodellen.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

## <a name="configuring-a-vpn-gateway"></a>Een VPN-Gateway configureren

De instructies die u gebruikt wanneer u een VPN-gateway configureert, is afhankelijk van het implementatiemodel die u gebruikt voor het maken van het virtuele netwerk. Bijvoorbeeld als u uw VNet met behulp van het implementatiemodel klassiek hebt gemaakt, kunt u de richtlijnen en instructies voor het klassieke implementatiemodel maken en configureren van de VPN-gateway-instellingen. Zie voor meer informatie over de implementatie van [Wat Resource Manager en klassieke implementatiemodellen](../resource-manager-deployment-model.md).

Een VPN-gateway is afhankelijk van meerdere resources die zijn geconfigureerd met specifieke instellingen. Bijna alle bronnen kan worden geconfigureerd afzonderlijk, hoewel ze in een bepaalde volgorde in sommige gevallen moeten worden geconfigureerd. U kunt maken en configureren van bronnen met een hulpprogramma zoals de Azure portal configuratie starten. U kunt vervolgens later besluit om over te schakelen naar een ander programma, zoals PowerShell, voor het configureren van aanvullende informatie of voor het wijzigen van bestaande middelen, indien van toepassing. Op dit moment configureren niet u elke resource en de broninstelling in Azure portal. De instructies in de artikelen voor elke verbinding topologie opgeven wanneer u een specifieke configuratiehulpprogramma nodig is. Zie voor informatie over de afzonderlijke resources en de instellingen voor VPN-Gateway, [over VPN-Gateway-instellingen](vpn-gateway-about-vpn-gateway-settings.md).

In de volgende secties bevatten tabellen met:

- implementatiemodel beschikbaar
- hulpprogramma's voor configuratie beschikbaar
- koppelingen waarmee u rechtstreeks naar een artikel, indien beschikbaar

Met diagrammen en beschrijvingen te selecteren de topologie verbinding aan uw behoeften voldoen. De diagrammen geven de belangrijkste basislijn topologieën, maar is het mogelijk te maken van meer complexe configuraties met behulp van de diagrammen als richtsnoer.

## <a name="site-to-site-and-multi-site"></a>Website en meerdere locaties

### <a name="site-to-site"></a>Site naar Site

Een verbinding van Site naar Site (S2S) VPN-gateway is een verbinding via IPsec/IKE (IKEv1 of IKEv2) VPN-tunnel. Dit type verbinding moet een VPN-apparaat zich bevindt op-ruimten met een openbaar IP-adres toegewezen en zich niet achter een NAT bevindt. S2S-verbindingen kunnen worden gebruikt voor cross-lokalen en hybride configuraties.   

![S2S verbinding] (./media/vpn-gateway-about-vpngateways/demos2s.png "site naar site")


### <a name="multi-site"></a>Meerdere locaties

U kunt maken en configureren van een VPN-gateway verbinding tussen uw VNet en netwerken met meerdere op gebouwen. Als u werkt met meerdere verbindingen, moet u een RouteBased VPN-type (dynamische gateway voor klassieke VNets). Omdat een VNet slechts één VPN-gateway hebben kan, delen alle verbindingen via de gateway de beschikbare bandbreedte. Dit is vaak een verbinding 'meerdere vestigingen' genoemd.
 

![Verbinding met meerdere locaties] (./media/vpn-gateway-about-vpngateways/demomulti.png "meerdere locaties")

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Distributie modellen en methoden voor Site naar Site en meerdere sites

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## <a name="vnet-to-vnet"></a>VNet-VNet-

Een virtueel netwerk verbinding te maken met een andere virtuele netwerk is (VNet VNet) vergelijkbaar met een VNet verbinding te maken met een locatie in de ruimten. Een VPN-gateway beide typen verbindingen gebruiken om een beveiligde tunnel met IPsec/IKE. VNet-VNet-communicatie met configuraties met meerdere locaties verbinding kunt u ook combineren. Hiermee kunt u netwerktopologieën die meerdere ruimten connectiviteit met de netwerkverbinding tussen de virtuele combineren tot stand brengen.

De VNets die u verbinding kunt maken zijn:

- in dezelfde of een andere regio 's
- in dezelfde of verschillende abonnementen 
- in de dezelfde verschillende implementatiemodellen


![VNet VNet-verbinding] (./media/vpn-gateway-about-vpngateways/demov2v.png "vnet-vnet-")

#### <a name="connections-between-deployment-models"></a>Verbindingen tussen implementatiemodellen

Azure heeft momenteel twee implementatiemodellen: klassieke en Resource Manager. Als u eerder hebt gebruikt Azure gedurende een bepaalde periode, hebt u waarschijnlijk Azure VMs en exemplaar rollen uitgevoerd in een klassieke VNet. De nieuwere VMs en rol exemplaren mogelijk in een VNet gemaakt in Resource Manager wordt uitgevoerd. U kunt een verbinding maken tussen het VNets zodat de bronnen in een VNet om te communiceren rechtstreeks met de bronnen in het andere.

#### <a name="vnet-peering"></a>Peering VNet

U kunt mogelijk gebruik VNet peering wilt maken van uw verbinding, zolang het virtuele netwerk aan bepaalde vereisten voldoet. Peering VNet, wordt een virtueel netwerkgateway niet gebruikt. Zie [peering VNet](../virtual-network/virtual-network-peering-overview.md)voor meer informatie.


### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Distributie modellen en methoden voor het VNet op VNet

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 


## <a name="point-to-site"></a>Punt-tot-Site

Een punt-naar-Site (P2S) VPN-gateway kunt u een beveiligde verbinding maken met het virtuele netwerk vanaf een werkstation. P2S is een VPN-verbinding via SSTP (Secure Socket Tunneling Protocol). P2S verbindingen hoeven niet een VPN-apparaat of een publieke IP-adres om te werken. U kunt de VPN-verbinding maken vanaf de clientcomputer. Deze oplossing is handig wanneer u wilt verbinding maken met uw VNet vanaf een externe locatie, zoals thuis of een vergadering, of als u alleen een paar-clients die verbinding moeten maken met een VNet. P2S verbindingen kunnen worden gebruikt in combinatie met S2S verbindingen via dezelfde VPN-gateway, op voorwaarde dat alle van de configuratievereisten voor beide verbindingen compatibel zijn.


![Punt-naar-site verbinding] (./media/vpn-gateway-about-vpngateways/demop2s.png "punt-tot-site")

### <a name="deployment-models-and-methods-for-point-to-site"></a>Van implementatiemodellen en methoden voor punt-naar-Site

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## <a name="expressroute"></a>ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

In een verbinding ExpressRoute is een virtueel netwerkgateway geconfigureerd met het gateway-type 'ExpressRoute' in plaats van 'Vpn'. Zie voor meer informatie over ExpressRoute, [ExpressRoute technisch overzicht](../expressroute/expressroute-introduction.md).


## <a name="site-to-site-and-expressroute-coexisting-connections"></a>Naast elkaar bestaande verbindingen van site naar Site en ExpressRoute

ExpressRoute is een directe, speciale verbinding tussen uw WAN (niet via het openbare Internet) en Microsoft-Services, met inbegrip van Azure. Site naar Site VPN-verkeer reis gecodeerd via het openbare Internet. Kan voor het configureren van verbindingen van Site naar Site VPN en ExpressRoute voor hetzelfde virtuele netwerk heeft verschillende voordelen.

U kunt een Site naar Site VPN configureren als een veilige failover-pad voor ExpressRoute of Site naar Site VPN-verbindingen gebruiken voor verbinding met sites die geen deel uitmaken van het netwerk, maar die zijn verbonden via ExpressRoute. Aankondiging dat hiervoor twee virtuele netwerkgateways voor hetzelfde virtuele netwerk, een met en de andere - GatewayType ExpressRoute - GatewayType Vpn.


![De verbinding gebruiken] (./media/vpn-gateway-about-vpngateways/demoer.png "expressroute site2site")


### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Van implementatiemodellen en methoden voor S2S en ExpressRoute

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## <a name="next-steps"></a>Volgende stappen

Plan uw gateway VPN-configuratie. Zie [VPN-Gateway Planning en ontwerp](vpn-gateway-plan-design.md) en [verbinding maken met uw netwerk op ruimten Azure](../guidance/guidance-connecting-your-on-premises-network-to-azure.md).








 

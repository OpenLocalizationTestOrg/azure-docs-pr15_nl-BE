<properties
   pageTitle="Overzicht van Azure Virtual Network (VNet)"
   description="Meer informatie over virtuele netwerken (VNets) in Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="virtual-network-overview"></a>Virtueel netwerk-overzicht

Azure virtueel netwerk (VNet) is een weergave van uw eigen netwerk in de cloud.  Het is een logische isolatie van de Azure cloud voor uw abonnement. U kunt volledig bepalen de blokken van IP-adres, DNS-instellingen voor beveiligingsbeleid en routetabellen binnen dit netwerk. Verder het segmenteren van uw VNet in de subnetten en Azure IaaS virtuele machines (VMs) starten en/of [Cloud-diensten (PaaS rol exemplaren)](../cloud-services/cloud-services-choose-me.md). Bovendien kunt u het virtuele netwerk op uw netwerk op ruimten is met een van de [connectiviteitsopties](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) in Azure. In principe kunt u uw netwerk uitbreiden naar Azure, met volledige controle op IP-adresblokken met het voordeel van de onderneming schaal die Azure biedt.

Om VNets beter te begrijpen, kijk eens naar de afbeelding hieronder, waarin een vereenvoudigde op gebouwen-netwerk.

![Netwerk van gebouwen](./media/virtual-networks-overview/figure01.png)

In de bovenstaande afbeelding toont een op gebouwen-netwerk dat is verbonden met het openbare Internet via een router. U ziet ook een firewall tussen de router en een DMZ hosten van een DNS-server en een web server-farm. De server-farm is een taakverdeling met behulp van een hardware-taakverdeling die toegankelijk is via Internet en systeembronnen van de interne subnet. Intern subnet is met een andere firewall en hosts Active Directory-domeincontroller, databaseservers en toepassingsservers gescheiden van de DMZ.

Hetzelfde netwerk kan worden ondergebracht in Azure, zoals in de onderstaande afbeelding.

![Azure virtueel netwerk](./media/virtual-networks-overview/figure02.png)

U ziet hoe de infrastructuur Azure neemt de rol van de router, zodat toegang van uw VNet met het openbare Internet zonder de noodzaak van een configuratie. Firewalls kunnen worden vervangen door netwerk-beveiligingsgroepen (NSGs) toegepast op elke afzonderlijke subnet. En fysieke Netwerktaakverdeling worden vervangen door internet gerichte en interne Netwerktaakverdeling in Azure.

>[AZURE.NOTE] Er zijn twee implementatiemodi in Azure: klassiek (ook bekend als Service Management) en Azure Resource Manager (ARM). Klassieke VNets kan worden toegevoegd aan een groep affiniteit of als een regionale VNet gemaakt. Als er een VNet in een groep affiniteit, wordt het aanbevolen om te [migreren naar een regionale VNet](virtual-networks-migrate-to-regional-vnet.md).

## <a name="virtual-network-benefits"></a>Voordelen van Virtual Network

- **Isolatie**. VNets zijn volledig van elkaar gescheiden. Die u kunt maken voor het ontwikkelen, testen en productie niet met elkaar verbonden netwerken die gebruikmaken van de dezelfde CIDR-adresblokken.

- **Toegang tot het openbare Internet**. Alle VMs IaaS en PaaS rol exemplaren in een VNet toegang tot het openbare Internet standaard. U kunt de toegang met behulp van beveiligingsgroepen netwerk (NSGs).

- **Toegang tot de VMs binnen de VNet**. PaaS rol exemplaren en IaaS VMs kan worden gestart in hetzelfde virtuele netwerk en een verbinding kunnen maken met elkaar met behulp van particuliere IP-adressen, zelfs als deze zich in verschillende subnetten hoeft te configureren van een gateway of openbare IP-adressen gebruiken.

- De **naamomzetting**. Azure biedt interne naamomzetting voor VMs IaaS en PaaS rol exemplaren in uw VNet geïmplementeerd. Ook kunt u uw eigen DNS-servers implementeren en configureren van de VNet voor het gebruik van deze.

- **Beveiliging**. Verkeer binnenvaren en verlaten van de virtuele machines en PaaS rol exemplaren in een VNet kan worden beheerd met behulp van beveiligingsgroepen netwerk.

- **Connectiviteit**. VNets kan worden verbonden met elkaar via netwerkgateways of peering VNet. VNets kan worden gekoppeld aan datacenters voor ruimten door middel van site naar site VPN-netwerken of Azure ExpressRoute. Bezoek voor meer informatie over VPN-verbindingen van site naar site, [over VPN-gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site). Bezoek voor meer informatie over ExpressRoute, [ExpressRoute technisch overzicht](../expressroute/expressroute-introduction.md). Meer informatie over peering VNet, gaat u naar [peering VNet](virtual-network-peering-overview.md).

    >[AZURE.NOTE] Zorg ervoor dat u een VNet maken voor alle VMs IaaS en PaaS rol exemplaren op uw Azure-omgeving implementeren. Op basis van ARM VMs vereisen een VNet en als u geen waarde voor een bestaande VNet opgeeft, Azure maakt een standaard VNet die mogelijk een CIDR adres blokkeren-conflict met uw netwerk op gebouwen. Onmogelijkheid voor u uw VNet aansluiten op uw netwerk op gebouwen.

## <a name="subnets"></a>Subnetten

Subnet is een bereik van IP-adressen in de VNet, kunt u een VNet verdelen over meerdere subnetten voor organisatie en beveiliging. VMs en PaaS rol exemplaren geïmplementeerd op subnetten (dezelfde of verschillende) binnen een VNet kunnen met elkaar communiceren zonder extra configuratie. U kunt ook routetabellen en NSGs in een subnet.

## <a name="ip-addresses"></a>IP-adressen


Er zijn twee typen IP-adressen die zijn toegewezen aan resources in Azure: *openbare* en *particuliere*. Openbare IP-adressen toestaan Azure middelen om te communiceren met het Internet en andere Azure publieke diensten zoals [Azure bestand Vgx. Cache](https://azure.microsoft.com/services/cache/), [Azure gebeurtenis Hubs](https://azure.microsoft.com/documentation/services/event-hubs/). Particuliere IP-adressen maakt communicatie mogelijk tussen de bronnen in een virtueel netwerk, de verbonden via een VPN, zonder een routeerbaar Internet IP-adres.

Bezoek voor meer informatie over IP-adressen in Azure, [IP-adressen in een virtueel netwerk](virtual-network-ip-addresses-overview-arm.md)

## <a name="azure-load-balancers"></a>Azure Netwerktaakverdeling

Virtuele machines en cloud services in een virtueel netwerk kunnen worden blootgesteld aan Internet met behulp van netwerktaakverdeling Azure. Zakelijke toepassingen die inwaarts gerichte zijn kan alleen taakverdeling met behulp van interne taakverdeling worden.

- **Externe taakverdeling**. U kunt een externe taakverdeling hoge beschikbaarheid bieden voor VMs IaaS en PaaS rol gevallen via het openbare Internet.

- **Interne balancer laadt**. U kunt een interne taakverdeling bieden van hoge beschikbaarheid voor VMs IaaS en PaaS rol exemplaren van andere services in uw VNet geopend.

Meer informatie over taakverdeling in Azure, gaat u naar [overzicht van de verdeling van belasting](../load-balancer/load-balancer-overview.md).

## <a name="network-security-group-nsg"></a>Network Security Group (NSG)

Kunt u NSGs binnenkomende en uitgaande toegang tot netwerkinterfaces (NIC's), VMs en subnetten. Elke NSG bevat een of meer regels op te geven of geen verkeer wordt goedgekeurd of geweigerd op basis van IP-adres, bronpoort IP-doeladres en doelpoort. Voor meer informatie over NSGs, Ga naar [Wat is een groep netwerk](virtual-networks-nsg.md).

## <a name="virtual-appliances"></a>Virtuele apparaten

Een virtuele toestel is gewoon een VM in de VNet die een functie van het toestel op basis van software, zoals firewall, intrusion detection of WAN-optimalisatie wordt uitgevoerd. U kunt een route maken in Azure voor het routeren van verkeer VNet via een virtueel toestel gebruik van de mogelijkheden.

NSGs kan bijvoorbeeld worden gebruikt om beveiliging te bieden op uw VNet. NSGs bieden echter laag 4 lijst ACL (Access Control) voor inkomende en uitgaande pakketten. Als u gebruiken een beveiligingsmodel van laag 7 wilt, moet u een firewall toestel gebruiken.

Virtuele apparaten, is afhankelijk van de [door de gebruiker gedefinieerde routes en doorsturen via IP](virtual-networks-udr-overview.md).

## <a name="limits"></a>Limieten
Er zijn grenzen aan het aantal virtuele netwerken die zijn toegestaan in een abonnement, Zie [limieten Azure toegang](../azure-subscription-service-limits.md#networking-limits) voor meer informatie.

## <a name="pricing"></a>Prijzen
Er is geen extra kosten voor het gebruik van virtuele netwerken in Azure. De compute exemplaren gestart binnen de Vnet brengt de standaardtarieven zoals beschreven in [Azure VM prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/). De [VPN-Gateways](https://azure.microsoft.com/pricing/details/vpn-gateway/) en -[openbare IP-adressen] (https://azure.microsoft.com/pricing/details/ip-addresses/) gebruikt in de VNet worden ook standaardtarieven in rekening gebracht.

## <a name="next-steps"></a>Volgende stappen

- [Een VNet maken](virtual-networks-create-vnet-arm-pportal.md) en subnetten.
- [Maak een VM in een VNet](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Meer informatie over [NSGs](virtual-networks-nsg.md).
- Meer informatie over de [door de gebruiker gedefinieerde routes en doorsturen via IP](virtual-networks-udr-overview.md).

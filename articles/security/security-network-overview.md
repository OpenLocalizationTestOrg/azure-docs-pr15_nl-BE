<properties
   pageTitle="Azure netwerk beveiliging-overzicht | Microsoft Azure"
   description=" In dit artikel kunt u begrijpen wat Microsoft Azure te bieden op het gebied van de beveiliging van het netwerk heeft. Wij bieden eenvoudige uitleg voor core netwerk-begrippen en vereisten en informatie Azure heeft aan te bieden in elk van deze gebieden. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-network-security-overview"></a>Azure netwerk beveiliging-overzicht

Microsoft Azure bevat een krachtige netwerk infrastructuur ter ondersteuning van uw toepassing en servicevereisten voor connectiviteit. Verbinding met het netwerk mogelijk is tussen bronnen in Azure, tussen op gebouwen en Azure bronnen, gehost en en naar het Internet en Azure.

Het doel van dit artikel is het makkelijker voor u om te begrijpen wat Microsoft Azure te bieden op het gebied van de beveiliging van het netwerk heeft. Wij bieden hier basic verklaringen voor core network security begrippen en vereisten. We ook vindt u informatie over Azure heeft aan te bieden in elk van deze gebieden. Er zijn vele koppelingen naar andere inhoud waarmee u een beter begrip van de gebieden waarin u geïnteresseerd bent.

In dit artikel overzicht van de beveiliging van de Azure zal zich richten op het volgende:

- Azure netwerken
- Netwerktoegangsbeheer
- Veilige externe toegang en cross-premises connectiviteit
- Beschikbaarheid
- Logboekregistratie
- Naamomzetting
- DMZ-architectuur
- Azure Security Center

## <a name="azure-networking"></a>Azure netwerken

Virtuele machines moet verbinding met het netwerk. Azure vereist ter ondersteuning van die eis, virtuele machines zijn aangesloten op een virtueel netwerk Azure. Een virtueel netwerk Azure is een logische constructie die is gebaseerd op het fysieke netwerk van Azure weefsel. Elke logische Azure Virtual Network wordt gescheiden van alle andere Azure virtuele netwerken. Dit helpt ervoor zorgen dat netwerkverkeer in de distributie niet toegankelijk voor andere klanten van Microsoft Azure is.

Meer informatie:

- [Virtueel netwerk-overzicht](../virtual-network/virtual-networks-overview.md)

## <a name="network-access-control"></a>Network Access Control
Netwerktoegangsbeheer is de handeling van de verbindingen van en naar specifieke apparaten of subnetten binnen een virtueel netwerk Azure beperken. Het doel van network access control is om ervoor te zorgen dat uw virtuele machines en services toegankelijk aan alleen gebruikers en apparaten waarnaar ze toegankelijk zijn. Toegang tot besturingselementen zijn gebaseerd op toestaan of weigeren van beschikkingen voor verbindingen van en naar de virtuele machine of service.

Azure ondersteunt verschillende typen van network access control. Deze omvatten:

- Network layer-besturingselement
- Route controle en gedwongen tunneling
- Beveiligingsapparaten virtueel netwerk

### <a name="network-layer-control"></a>Network Layer Control
Een veilig implementeren vereist een bepaalde vorm van netwerktoegangsbeheer. Het doel van network access control is om ervoor te zorgen dat uw virtuele machines en de netwerkservices die worden uitgevoerd op deze virtuele machines alleen met andere netwerkapparaten communiceren kunnen die ze nodig hebben om te communiceren met en andere verbindingspogingen worden geblokkeerd.

Als u Basisnetwerk toegangsbeheer op gebruikersniveau (op basis van IP-adres en het TCP- of UDP-protocol) nodig hebt, kunt u beveiligingsgroepen netwerk gebruiken. Een Network Security Group (NSG) is een eenvoudige stateful packet filter firewall en zorgt ervoor dat u de toegang op basis van een [5-tuple](https://www.techopedia.com/definition/28190/5-tuple). NSGs bieden geen toepassing laag inspectie of Geverifieerde toegangscontrole.

Meer informatie:

- [Netwerk-beveiligingsgroepen](../virtual-network/virtual-networks-nsg.md)

### <a name="route-control-and-forced-tunneling"></a>Route controle en gedwongen Tunneling
De mogelijkheid om de werking van routering op de virtuele netwerken Azure is een essentiële beveiliging en toegangsbeheer besturingselement netwerkmogelijkheden. Als routering niet juist is geconfigureerd, toepassingen en services op de virtuele machine kunnen verbinding maken met apparaten die u niet wilt dat deze verbinding wil maken, met inbegrip van hulpmiddelen eigendom van en wordt beheerd door de potentiële aanvallers.

Azure netwerken ondersteunt de mogelijkheid voor het aanpassen van de werking van routering voor netwerkverkeer op uw virtuele netwerken Azure. Hiermee kunt u de standaardvermeldingen in de routeringstabel in uw Azure virtuele netwerk wijzigen. Controle van de werking van routering kunt u ervoor zorgen dat al het verkeer van een bepaald apparaat of groep apparaten wordt ingevoerd of het virtuele netwerk Azure via een specifieke locatie verlaat.

Bijvoorbeeld, wellicht een virtueel netwerk beveiliging toestel op het virtuele netwerk Azure. U wilt dat om ervoor te zorgen dat alle verkeer van en naar het virtuele netwerk Azure tot en met dat toestel virtuele beveiliging gaat. U kunt dit doen door de [Gebruiker gedefinieerde Routes](../virtual-network/virtual-networks-udr-overview.md) configureren in Azure.

[Gedwongen tunneling](https://www.petri.com/azure-forced-tunneling) is een mechanisme dat u gebruiken kunt om ervoor te zorgen dat uw diensten mogen niet tot stand brengen van een verbinding met andere apparaten op het Internet. Houd er rekening mee dat dit verschilt van binnenkomende verbindingen accepteert en vervolgens reageren op deze. Front-end-webservers moeten reageren op aanvraag via Internet-hosts en zodat Internet-die afkomstig zijn van verkeer is toegestaan inkomend verkeer naar deze webservers en webservers zijn toegestaan om te reageren.

Wat u niet wilt toestaan, is een front-end-webserver een uitgaande aanvraag starten. Dergelijke aanvragen kunnen een beveiligingsrisico kunnen duiden omdat deze verbindingen kunnen worden gebruikt voor het downloaden van malware. Zelfs als u dat deze front-end servers wilt starten uitgaande verzoeken tot het Internet, kunt u zorgen dat ze de webproxy op gebouwen doorlopen zodat u kunt profiteren van URL filtering en aan te melden.

In plaats daarvan wilt u gedwongen tunneling gebruiken om dit te voorkomen. Wanneer u gedwongen tunneling inschakelt, worden alle verbindingen met het Internet via de gateway op gebouwen gedwongen. U kunt gedwongen tunneling door gebruik te maken van de gebruiker gedefinieerde Routes.

Meer informatie:

- [Wat zijn de gebruiker gedefinieerde Routes en IP-doorsturen](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Beveiligingsapparaten virtueel netwerk
Netwerk-beveiligingsgroepen Routes voor gebruiker gedefinieerd en gedwongen tunneling zorgen voor u een niveau van beveiliging op het netwerk- en lagen van het [OSI-model](https://en.wikipedia.org/wiki/OSI_model), het kan gebeuren wanneer u beveiliging wilt inschakelen op een niveau hoger dan het netwerk.

Bijvoorbeeld, kunnen uw beveiligingsvereisten opnemen:

- Verificatie en machtiging voor toegang tot uw toepassing
- Intrusion detection en intrusion antwoord
- Application layer inspectie van high-level protocols
- URL-filtering
- Netwerk niveau antivirus- en antimalware
- Bescherming tegen bot
- Toegangsbeheer voor toepassingen
- Extra DDoS bescherming (boven de DDoS bescherming de Azure fabric zelf verstrekt)

Toegang tot deze beveiligingsfuncties van het uitgebreide netwerk met behulp van een partneroplossing voor Azure. U vindt de meest recente Azure partner oplossingen voor netwerkbeveiliging de [Azure Marketplace](https://azure.microsoft.com/marketplace/) bezoeken en zoeken naar 'beveiliging' en 'network security'.

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Veilige externe toegang en verbindingen tussen ruimten
Installatie, configuratie en beheer van uw Azure bronnen moet u op afstand. Bovendien kunt u implementeren [hybride IT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) -oplossingen die onderdelen van gebouwen zijn en in de openbare Azure cloud. Deze scenario's moeten de veilige externe toegang.

Azure netwerken ondersteunt de volgende scenario's voor veilige externe toegang:

- Individuele werkstations verbinding met een virtueel netwerk Azure
- Uw netwerk op gebouwen met een Azure virtueel netwerk met een VPN verbinding
- Verbinding maken met uw netwerk op gebouwen met een virtueel netwerk Azure met een specifieke WAN-verbinding
- Azure virtuele netwerken met elkaar verbinden

### <a name="connect-individual-workstations-to-an-azure-virtual-network"></a>Individuele werkstations verbinding met een virtueel netwerk van Azure
Het is mogelijk dat u inschakelen van de individuele ontwikkelaars of bewerkingen personeel wilt voor het beheren van virtuele machines en services in Azure. Bijvoorbeeld, moet u toegang tot een virtuele machine op een virtueel netwerk Azure en uw beveiligingsbeleid kan geen RDP of SSH remote toegang tot afzonderlijke virtuele machines. In dit geval kunt u een point-to-site VPN-verbinding.

De point-to-site VPN-verbinding wordt gebruikt de [SSTP VPN-](https://technet.microsoft.com/library/cc731352.aspx) protocol voor het instellen van een vertrouwelijke en veilige verbinding tussen de gebruiker en het virtuele netwerk Azure. Zodra de VPN-verbinding tot stand is gebracht, de gebruiker kan worden RDP of SSH via de VPN-verbinding in een virtuele machine op het virtuele netwerk Azure (ervan uitgaande dat het de gebruiker kan worden geverifieerd en gemachtigd).

Meer informatie:

- [Een punt-naar-Site verbinding met een virtueel netwerk met PowerShell configureren](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-vpn"></a>Verbinding maken met uw On-Premises-netwerk met een Azure virtueel netwerk met een VPN
U kunt verbinding maken met uw bedrijfsnetwerk hele of gedeelten hiervan, met een virtueel netwerk Azure. Dit is gebruikelijk in hybride IT scenario's waarin bedrijven [hun datacenter op gebouwen in Azure uitbreiden](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). In veel gevallen host bedrijven delen van een service in Azure en webonderdelen op-ruimten, zoals wanneer een oplossing front-end-webservers in Azure en back-end databases op ruimten bevat. Deze soort verbindingen "cross-lokalen" beheer van Azure zich ook maken bronnen beveiligen en meer scenario's zoals Active Directory-domeincontrollers uit te breiden naar Azure inschakelen.

Eén manier om dit te doen is met een [VPN van site naar site](https://www.techopedia.com/definition/30747/site-to-site-vpn). Het verschil tussen een VPN website en een point-to-site VPN is een point-to-site VPN een enkel apparaat verbindt met een virtueel netwerk Azure terwijl een VPN website een heel netwerk (zoals uw netwerk op ruimten) verbinding met een virtueel netwerk Azure. Website VPN-verbindingen met een virtueel netwerk Azure zwaarbeveiligde IPSec-tunnelmodus VPN-protocol gebruiken.

Meer informatie:

- [Een Resource Manager VNet maken met een site naar site VPN-verbinding via de Portal Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Planning en ontwerp voor een VPN-gateway](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-dedicated-wan-link"></a>De On-premises netwerk verbinden met een Azure Virtual Network met een specifieke WAN-verbinding
Punt-tot-site en de site naar site VPN-verbindingen zijn van kracht voor het inschakelen van connectiviteit tussen ruimten. Echter, sommige organisaties denkt dat zij de volgende nadelen hebben:

- VPN-verbindingen verplaatsen van gegevens via het Internet: dit beschrijft deze verbindingen met potentiële beveiligingsproblemen die betrokken zijn bij het verplaatsen van gegevens via een openbaar netwerk. Bovendien worden niet betrouwbaarheid en beschikbaarheid voor Internet-verbindingen gegarandeerd.
- VPN-verbindingen met Azure virtuele netwerken kunnen worden geacht aan de bandbreedte beperkt voor bepaalde toepassingen en doeleinden als ze max uit op rond 200Mbps.

Organisaties die het hoogste niveau van beveiliging en beschikbaarheid voor hun verbindingen tussen bedrijven doorgaans gebruik specifieke WAN-verbindingen verbinding maken met externe sites. Azure biedt u een specifieke WAN-verbinding kunt u verbinding maken met uw netwerk in ruimten met een virtueel netwerk Azure gebruiken. Dit is ingeschakeld via de Azure ExpressRoute.

Meer informatie:

- [ExpressRoute-technisch overzicht](../expressroute/expressroute-introduction.md)

### <a name="connect-azure-virtual-networks-to-each-other"></a>Azure virtuele netwerken met elkaar verbinden
Het is mogelijk veel Azure virtuele netwerken voor de distributie gebruikt. Er zijn vele redenen waarom u dit kunt doen. Een van de redenen mogelijk te vereenvoudigen; een ander mogelijk om veiligheidsredenen. Ongeacht de motivatie en de motivering voor het plaatsen van bronnen in verschillende Azure virtuele netwerken worden vaak als u wilt dat resources op elk van de netwerken te verbinden met elkaar.

Een optie zou zijn voor services op één Azure virtueel netwerk verbinding maken met services op een andere Azure Virtual Network in "een lus terug" via het Internet. De verbinding zou beginnen op een Azure virtueel netwerk via het Internet en teruggaan naar de bestemming Azure Virtual Network. Met deze optie stelt de verbinding met de beveiligingsproblemen die inherent zijn aan Internet-communicatie.

Een betere optie misschien een Azure virtueel netwerk naar Azure Virtual Network site naar site VPN maken. Deze Azure virtueel netwerk naar Azure Virtual Network site naar site VPN gebruikt hetzelfde protocol voor [de IPSec-tunnelmodus](https://technet.microsoft.com/library/cc786385.aspx) als de cross-premises site naar site VPN-verbinding hierboven vermeld.

Het voordeel van een Azure virtueel netwerk naar Azure Virtual Network site naar site VPN is dat de VPN-verbinding tot stand is gebracht via de fabric Azure netwerk; het maakt geen verbinding via het Internet. Dit biedt een extra beveiligingslaag ten opzichte van site naar site VPN's die verbinding via het Internet maken.

Meer informatie:

- [VNet-VNet-verbinding configureren met behulp van bronbeheer Azure en PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Beschikbaarheid
Beschikbaarheid is een belangrijk onderdeel van een beveiligingsprogramma. Als uw gebruikers en systemen geen toegang krijgen wat ze nodig hebben via het netwerk toegang tot tot, de service in gevaar gebracht kan worden beschouwd. Azure is een netwerktechnologie die ondersteuning bieden voor de volgende mechanismen voor hoge beschikbaarheid:

- HTTP-gebaseerde taakverdeling
- Niveau van netwerktaakverdeling
- Algemene taakverdeling

Netwerktaakverdeling is een mechanisme dat is ontworpen voor het gelijkmatig verdelen van verbindingen tussen meerdere apparaten. De doelstellingen van netwerktaakverdeling zijn:

- Beschikbaarheid: verhogen wanneer u saldo verbindingen via meerdere apparaten laden, een of meer van de apparaten niet beschikbaar zijn en de services die worden uitgevoerd op de overige apparaten online kunnen blijven voldoen aan de inhoud van de service
- De prestaties verbeteren: wanneer u saldo verbindingen via meerdere apparaten laadt, één apparaat hoeft te nemen van de processor hit. In plaats daarvan wordt de verwerking en geheugen vereisten voor de inhoud verspreid over meerdere apparaten.

### <a name="http-based-load-balancing"></a>HTTP-gebaseerde taakverdeling
Organisaties die webservices worden vaak uitgevoerd willen hebben van een HTTP-gebaseerde taakverdeling voor deze webservices om te zorgen dat passende niveaus van prestaties en hoge beschikbaarheid. In tegenstelling tot traditionele netwerk gebaseerde Netwerktaakverdeling, zijn de beslissingen van de HTTP-gebaseerde voor taakverdeling Netwerktaakverdeling gebaseerd op de kenmerken van het HTTP-protocol, niet op het netwerk en transport layer-protocollen.

Als u HTTP-gebaseerde taakverdeling bieden voor de webservices, biedt Azure u de toepassingsgateway Azure. De Gateway Azure toepassing ondersteunt:

- HTTP-gebaseerde taakverdeling – load balancing beslissingen worden gemaakt op basis van kenmerkende speciale het HTTP-protocol
- Op basis van een cookie sessie affiniteit – Hierdoor kunt u zorgen dat verbindingen tot stand gebracht met een van de servers achter deze taakverdeling tussen de client en de server intact blijft. Dit verzekert stabiliteit van transacties.
- SSL-offload – wanneer de clientverbinding wordt gemaakt met de taakverdelingsvoorziening sessie tussen de client en de verdeling van de belasting is gecodeerd met behulp van het HTTPS (SSL /) protocol. Echter om prestaties te verbeteren, hebt u de optie om de verbinding tussen de taakverdeling en de webserver achter de load balancer gebruik het protocol HTTP (ongecodeerd). Dit wordt aangeduid als "SSL offload" omdat de webservers achter de taakverdeling niet van de processoroverhead die betrokken zijn bij de codering optreden en dus kunnen aanvragen sneller moeten.
- Op URL gebaseerde inhoud routering: deze functie maakt het mogelijk voor de taakverdeling om beslissingen te nemen voor het doorsturen van verbindingen op basis van de doel-URL. Dit biedt veel meer flexibiliteit dan de oplossingen die de tegenrekening besluiten op basis van IP-adressen worden geladen.

Meer informatie:

- [Gateway: overzicht](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Niveau van netwerktaakverdeling
In tegenstelling tot HTTP-gebaseerde load balancing, Netwerktaakverdeling voor niveau load balancing besluiten op basis van IP-adres en poort (TCP of UDP) getallen maakt.
U kunt de voordelen van het niveau van netwerktaakverdeling in Azure met behulp van de Load Balancer voor Azure toegang. Enkele belangrijke kenmerken van de taakverdeling Azure omvatten:

- Netwerktaakverdeling voor niveau op basis van IP-adres en poort nummers
- Ondersteuning voor elk protocol application layer
- Saldi Azure virtuele machines laden en cloud services-rol exemplaren
- Voor Internet- en internetverbinding (externe taakverdeling) kunnen worden gebruikt voor toepassingen (interne load balancing) en virtuele machines
- Controle eindpunt dat wordt gebruikt om te bepalen als een van de diensten achter de taakverdeling zijn niet meer beschikbaar

Meer informatie:

- [Internet Facing taakverdeling tussen meerdere virtuele Machines en services](../load-balancer/load-balancer-internet-overview.md)
- [Interne Load Balancer-overzicht](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Wereldwijde Load Balancing
Sommige organisaties willen het hoogste niveau van beschikbaarheid mogelijk. Een manier om dit doel te bereiken is host voor toepassingen in wereldwijd verspreide datacentra. Wanneer een toepassing wordt gehost in datacenters over de hele wereld te vinden, is het mogelijk voor een gehele geopolitieke regio niet meer beschikbaar zijn en de toepassing nog steeds omhoog en wordt uitgevoerd.

Naast de beschikbaarheid van voordelen u krijgt door datacenters wereldwijd gedistribueerde toepassingen die als host fungeert, kunt u ook voordelen krijgen. Deze prestatievoordelen kunnen worden verkregen met behulp van een mechanisme dat zorgt ervoor dat de aanvragen voor het datacenter die het dichtst bij het apparaat dat het verzoek van de service.

Algemene taakverdeling kan bieden die beide van deze voordelen. In Azure, kunt u de voordelen van globale taakverdeling met Azure verkeer Manager krijgen.

Meer informatie:

- [Wat is verkeer Manager?](../traffic-manager/traffic-manager-overview.md)

## <a name="logging"></a>Logboekregistratie
Logboekregistratie op een netwerk is een belangrijke functie voor elk netwerk beveiliging. U kunt zich aanmelden voor beveiligingsgroepen netwerk netwerkniveau registratie informatie ophalen verkregen gegevens in Azure. Met logboekregistratie van de NSG zoeken informatie:

- Controlelogboeken – deze logboeken worden gebruikt voor alle bewerkingen die worden verzonden naar uw Azure abonnementen weergeven. Deze logboekbestanden zijn standaard ingeschakeld en kunnen worden gebruikt binnen de portal Azure.
- Gebeurtenislogboeken – deze logboeken geven informatie over welke regels NSG zijn toegepast.
- Teller logboeken – deze logboeken kunnen zien hoe vaak elke NSG-regel is toegepast om te weigeren of toestaan van verkeer.

U kunt ook [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), een krachtige data visualisatie tool, bekijken en analyseren van deze logboeken.

Meer informatie:

- [Logboek Analytics voor beveiligingsgroepen netwerk (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)

## <a name="name-resolution"></a>Naamomzetting
Naamomzetting is een essentiële functie voor alle services die u in Azure host. Vanuit een oogpunt van beveiliging worden inbreuk van de functie naam resolutie kan leiden tot een aanvaller omleiden van aanvragen van uw sites van een aanvaller. Beveiligde naamomzetting is een vereiste voor alle services in de cloud gehost.

Er zijn twee soorten adres u moet naamomzetting:

- Interne naamomzetting – interne naamomzetting wordt gebruikt door services op uw virtuele netwerken Azure of uw netwerken op gebouwen. Namen die worden gebruikt voor interne naamomzetting zijn niet toegankelijk via het Internet. Voor een optimale beveiliging is het belangrijk dat de interne naam resolutie schema niet toegankelijk voor externe gebruikers is.
- Externe naamomzetting – externe naamomzetting wordt gebruikt door mensen en apparaten buiten uw lokale en Azure virtuele netwerken. Dit zijn de namen die zijn zichtbaar op het Internet en worden gebruikt voor een directe verbinding met uw cloud-gebaseerde services.

Voor interne naamomzetting hebt u twee mogelijkheden:

- Een Azure virtuele netwerk DNS-server: als u een nieuwe Azure virtueel netwerk, maakt een DNS-server is gemaakt. Deze DNS-server kan de namen van de computers zich op een virtueel netwerk Azure kunt oplossen. Deze DNS-server kan niet worden geconfigureerd en beheerd door de Azure fabric, zodat een veilige naam resolutie oplossing.
- Breng uw eigen DNS-server: hebt u de mogelijkheid van het plaatsen van een DNS-server van uw eigen keuze in het virtuele netwerk Azure. Deze DNS-server kan worden op dat een Active Directory geïntegreerde DNS-server of een specifieke DNS-serveroplossing geleverd door een Azure partner, u bij de markt Azure verkrijgen kunt.

Meer informatie:

- [Virtueel netwerk-overzicht](../virtual-network/virtual-networks-overview.md)
- [Beheren van DNS-Servers die worden gebruikt door een virtueel netwerk (VNet)](../virtual-network/virtual-networks-manage-dns-in-vnet.md)

Voor externe DNS-omzetting hebt u twee mogelijkheden:

- Host uw eigen externe DNS-server op-locatie
- Host uw eigen externe DNS-server met een Internet-provider

Veel grote organisaties host hun eigen DNS-servers op-terrein. Zij kunnen hiervoor omdat ze hebben de netwerken expertise en wereldwijde aanwezigheid om dit te doen.

In de meeste gevallen is het beter voor het hosten van uw DNS-services voor naamomzetting met een Internet-provider. Deze serviceproviders hebben de expertise netwerk en wereldwijde aanwezigheid om zeer hoge beschikbaarheid garanderen voor de services voor naamomzetting. Beschikbaarheid is essentieel voor DNS-services, omdat als de services voor naamomzetting niet, niemand zal kunnen bereiken van de services van uw internetverbinding.

Azure biedt u een hoge mate van beschikbaarheid en zodat externe DNS-oplossing in de vorm van Azure DNS. Deze resolutie externe naam oplossing maakt gebruik van de wereldwijde Azure DNS-infrastructuur. U kunt als host voor uw domein in Azure met behulp van dezelfde referenties, API's, hulpmiddelen en facturering als uw Azure services. Als onderdeel van Azure, ook neemt de besturingselementen voor sterke beveiliging ingebouwd in het platform.

Meer informatie:

- [Azure DNS-overzicht](../dns/dns-overview.md)

## <a name="dmz-architecture"></a>DMZ-architectuur
Veel ondernemingen gebruiken DMZs om segmenten hun netwerken te maken van een bufferzone tussen het Internet en hun diensten. Het DMZ gedeelte van het netwerk wordt beschouwd als een laag beveiligingsniveau zone en geen hoge waarde activa in dat netwerksegment worden geplaatst. Meestal ziet u beveiliging netwerkapparaten hebben een netwerkinterface op het segment DMZ en andere netwerkinterface aangesloten op een netwerk met virtuele machines en services die het accepteren van binnenkomende verbindingen vanaf het Internet.

Er zijn een aantal variaties van DMZ ontwerp en het besluit tot een DMZ, distribueren en vervolgens welk type DMZ te gebruiken als u wilt gebruiken, is gebaseerd op uw beveiligingsvereisten voor het netwerk.

Meer informatie:

- [Microsoft Cloud Services en netwerkbeveiliging](../best-practices-network-security.md)

## <a name="azure-security-center"></a>Azure Security Center
Beveiligingscentrum helpt u bij het voorkomen, opsporen en reageren op bedreigingen en biedt dat u meer inzicht in, en controle over, de beveiliging van uw resources Azure. Het biedt geïntegreerde beveiliging beleid en het toezicht op verschillende de Azure abonnementen, helpt detecteren bedreigingen die anders opgemerkt en werkt met een brede selectie van beveiligingsoplossingen.

Azure Beveiligingscentrum kunt u optimaliseren en beveiliging van het netwerk door te controleren:

- Aanbevelingen voor de beveiliging te bieden
- De status van uw netwerkconfiguratie beveiliging controleren
- U wordt gewaarschuwd voor het netwerk op basis van bedreigingen, zowel op het niveau van het eindpunt en netwerk

Meer informatie:

- [Inleiding tot Azure Security Center](../security-center/security-center-intro.md)

<properties
   pageTitle="Azure Network Security Best Practices | Microsoft Azure"
   description="Dit artikel bevat een set van best practices voor het gebruik van netwerk-beveiliging ingebouwd in Azure mogelijkheden."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="swadhwa"
   editor="TomShinder"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/25/2016"
   ms.author="TomSh"/>

# <a name="azure-network-security-best-practices"></a>Azure Network Security Best Practices

Microsoft Azure kunt u virtuele machines en apparaten verbinding maken met andere apparaten in het netwerk door te brengen Azure virtuele netwerken. Een virtueel netwerk Azure is de constructie van een virtueel netwerk waarmee u verbinding maken met virtuele netwerkkaarten met een virtueel netwerk om TCP/IP-gebaseerde communicatie tussen netwerkapparaten ingeschakeld. Azure virtuele Machines gekoppeld aan een virtueel netwerk Azure kunnen verbinding maken met apparaten op hetzelfde Azure virtueel netwerk, andere Azure virtuele netwerken op het Internet of zelfs op uw eigen netwerken in gebouwen.

In dit artikel bespreken we een collectie van Azure network security best practices. Deze best practices zijn afgeleid van onze ervaring met Azure netwerkmogelijkheden en de ervaringen van klanten zoals uzelf.

Voor elke beste leggen we:

- Wat de beste manier is
- Waarom u wilt dat het beste inschakelen
- Wat kan het resultaat zijn als u niet het beste inschakelen
- Mogelijke alternatieven voor de beste praktijken
- Hoe leert u het beste inschakelen

In dit artikel Azure Network Security Best Practices is gebaseerd op een consensus-advies en Azure platformmogelijkheden en functiesets, die zijn op het moment dat dit artikel werd geschreven. Adviezen en technologieën veranderen en in dit artikel wordt op gezette tijden met deze wijzigingen bijgewerkt.

Azure netwerk aanbevolen beveiligingsprocedures besproken in dit artikel zijn:

- Logisch segment subnetten
- De werking van Routering beheren
- Geforceerde Tunneling inschakelen
- Virtueel netwerk toestellen gebruiken
- DMZs voor inzake beveiliging implementeren
- Vermijd blootstelling via een specifieke WAN-verbindingen met het Internet
- Uptime en prestaties optimaliseren
- Gebruik globale taakverdeling
- RDP-toegang tot Azure Virtual Machines schakelen
- Azure-Beveiligingscentrum inschakelen
- Uitbreiden van uw datacenter in Azure


## <a name="logically-segment-subnets"></a>Logisch segment subnetten

[Azure virtuele netwerken](https://azure.microsoft.com/documentation/services/virtual-network/) zijn vergelijkbaar met een LAN in uw netwerk op gebouwen. Het idee achter een virtueel netwerk Azure is die u maakt één particuliere IP-adres ruimte gebaseerde netwerk waarop u alle uw [Azure virtuele Machines](https://azure.microsoft.com/services/virtual-machines/)kunt plaatsen. De particuliere IP-adresruimten beschikbaar zijn in klasse A (10.0.0.0/8), (172.16.0.0/12) van klasse B en klasse C (192.168.0.0/16) bereiken.

Vergelijkbaar met wat u op het bedrijf, wilt u een grotere adresruimte segmenteren in subnetten. [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gebaseerd subnetten beginselen kunt u de subnetten maken.

Routeren tussen subnetten gebeurt automatisch en u hoeft niet handmatig te configureren routeringstabellen. De standaardinstelling is echter dat er geen netwerk access besturingselementen zijn tussen de subnetten die u in het virtuele netwerk Azure maakt. Om het netwerk toegang tot besturingselementen tussen subnetten maken, moet u iets tussen de subnetten gezet.

Een van de dingen die u gebruiken kunt om dit te bewerkstelligen is een [Network Security Group](../virtual-network/virtual-networks-nsg.md) (NSG). NSGs zijn eenvoudige stateful packet inspectie apparaten die gebruikmaken van de 5-tupel (het bron-IP, bronpoort, doel-IP-, doelpoort en 4-layer protocol) benadering voor het maken van toestaan/weigeren van regels voor netwerkverkeer. U kunt toestaan of weigeren van verkeer van en naar een enkel IP-adres naar en van meerdere IP-adressen of zelfs aan en van de gehele subnetten.

NSGs kunt voor network access control tussen subnets u resources die bij de beveiligingszone dezelfde of een rol in hun eigen subnet horen te plaatsen. Bijvoorbeeld, beschouw een eenvoudige 3-tier-toepassing met een weblaag, een toepassingslaag logica en een databaselaag. U plaatst de virtuele machines die deel uitmaken van elk van deze niveaus in hun eigen subnetten. Vervolgens gebruikt u NSGs verkeer tussen de subnetten beheren:

- Web tier virtuele machines kunt alleen verbindingen maken met de toepassing logic machines en accepteert alleen verbindingen vanaf het Internet
- Toepassing logica virtuele machines alleen verbindingen met de databaselaag kunt starten en accepteert alleen verbindingen van de weblaag
- Database-tier virtuele machines kan geen verbinding met iets buiten hun eigen subnet tot stand brengen en accepteert alleen verbindingen van de toepassingslaag logica

Lees het artikel voor meer informatie over beveiligingsgroepen netwerk en hoe u ze kunt gebruiken bij het segmenteren van uw virtuele netwerken Azure logisch, [Wat is een Network Security Group](../virtual-network/virtual-networks-nsg.md) (NSG).

## <a name="control-routing-behavior"></a>De werking van Routering beheren

Wanneer u een virtuele machine op een virtueel netwerk Azure, zult u merken dat de virtuele machine verbinding met andere virtuele machine op de Azure virtueel netwerk maken kunt, zelfs als de andere virtuele machines in verschillende subnetten. De reden waarom dit mogelijk is, is er een collectie van systeem routes die standaard zijn ingeschakeld, waardoor dit type communicatie. Deze standaardroutes kunnen virtuele machines op hetzelfde Azure virtuele netwerk om verbindingen te maken met elkaar en met het Internet (voor uitgaand verkeer op het Internet).

De standaard systeem routes zijn nuttig voor veel implementatiescenario's, maar er zijn tijden wanneer u het wilt aanpassen van de configuratie van routering voor uw implementatie. Deze aanpassingen kunt u voor het configureren van het next-hopadres voor het bereiken van specifieke bestemmingen.

Wij raden aan dat de gebruiker gedefinieerd Routes configureren wanneer u een virtueel netwerk security-toestel, dat we in een latere beste bespreken implementeert.

> [AZURE.NOTE] gebruiker gedefinieerde Routes zijn niet vereist en de standaardroutes systeem werkt in de meeste gevallen.

U kunt meer informatie over de gebruiker gedefinieerde Routes en configureren voor het lezen van het artikel [Wat zijn gebruiker gedefinieerd Routes en doorsturen via IP](../virtual-network/virtual-networks-udr-overview.md).

## <a name="enable-forced-tunneling"></a>Geforceerde Tunneling inschakelen

Om beter te begrijpen gedwongen tunneling, is het nuttig om te begrijpen welke 'gesplitste tunneling' is.
Het bekendste voorbeeld van een gesplitste tunneling wordt met VPN-verbindingen weergegeven. Stel dat u een VPN-verbinding vanuit uw hotelkamer met uw bedrijfsnetwerk. Deze verbinding kunt u verbinding maken met bronnen op het netwerk van uw bedrijf en alle communicatie met bronnen op het bedrijfsnetwerk gaan via de VPN-tunnel.

Wat gebeurt er wanneer u wilt verbinding maken met bronnen op het Internet? Als gesplitste tunneling is ingeschakeld, gaat deze verbindingen rechtstreeks met het Internet en niet via de VPN-tunnel. Sommige beveiligingsexperts kunt u overwegen dit een potentieel risico en raden u daarom aan gesplitste tunneling worden uitgeschakeld en alle verbindingen, die bestemd zijn voor het Internet en die bestemd zijn voor bedrijfsnetwerken, gaat u via de VPN-tunnel. Het voordeel van deze manier is dat Internet-verbindingen vervolgens via het bedrijfsnetwerk beveiliging-apparaten, die zou niet het geval als de VPN-client is verbonden met het Internet buiten de VPN-tunnel worden gedwongen.

Nu brengen we dit terug aan virtuele machines op een virtueel netwerk Azure. De standaardroutes voor een virtueel netwerk Azure kunnen virtuele machines om verkeer naar het Internet. Dit te staat een beveiligingsrisico omdat deze uitgaande verbindingen de kwetsbaarheid van een virtuele machine kunnen worden vergroot en worden gebruikt door aanvallers.
Daarom is het raadzaam te schakelen gedwongen tunneling op uw virtuele machines wanneer u meerdere ruimten verbinding tussen uw Azure virtueel netwerk en uw netwerk op gebouwen hebt. We zullen hebben over meerdere ruimten connectiviteit verderop in dit Azure netwerken best practices-document.

Als er geen verbinding tussen ruimten, zorg ervoor dat u profiteren van de netwerk-beveiligingsgroepen (hierboven beschreven) of Azure virtual network security toestellen (volgende besproken) om te voorkomen dat uitgaande verbindingen met het Internet vanuit uw Azure virtuele Machines.

Voor meer informatie over het geforceerd tunneling en inschakelen, lees het artikel [configureren gedwongen Tunneling met behulp van PowerShell en Azure Resource Manager](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md).

## <a name="use-virtual-network-appliances"></a>Virtueel netwerk toestellen gebruiken

Netwerk-beveiligingsgroepen en de gebruiker gedefinieerd routering kan zorgen voor een zekere mate van beveiliging van netwerken op het netwerk- en lagen van het [OSI-model](https://en.wikipedia.org/wiki/OSI_model), gaan er zich situaties voordoen waarin u wilt of moet de beveiliging op hoog niveau van de stack inschakelen. In dergelijke situaties is het raadzaam implementeren van beveiligingsapparaten virtueel netwerk verstrekt door Azure partners.

Beveiligingsapparaten Azure netwerk bieden aanzienlijk verbeterde niveaus van beveiliging op hetgeen wordt verstrekt door netwerkfuncties niveau. Het netwerk beveiliging mogelijkheden van beveiligingsapparaten virtueel netwerk onder meer:

- Gebruik
- Intrusion detection inbraak preventie
- Beveiligingslek met betrekking tot beheer
- Controle van de toepassing
- Afwijking op basis van het netwerk opsporen
- Web filteren
- Antivirus
- Botnet beveiliging

Als u nodig hebt met een hoger niveau van netwerkbeveiliging dan kunt u met het toegangsniveau van de netwerkfuncties, vervolgens het beste u onderzoeken en implementeren van beveiligingsapparaten Azure virtueel netwerk.

Als u wilt weten over welke beveiligingsapparaten Azure virtueel netwerk beschikbaar zijn, en hun mogelijkheden, gaat u naar de [Azure Marketplace](https://azure.microsoft.com/marketplace/) en zoek naar 'beveiliging' en 'network security'.

##<a name="deploy-dmzs-for-security-zoning"></a>DMZs voor inzake beveiliging implementeren
Een DMZ of "perimeternetwerk" is een fysieke of logische netwerksegment dat geschikt is voor een extra beveiligingslaag tussen de activa en het Internet bieden. De bedoeling van de DMZ is gespecialiseerde access control netwerkapparaten plaatsen op de rand van de DMZ-netwerk, zodat alleen het gewenste verkeer is toegestaan na de beveiligingsvoorziening en in het virtuele netwerk Azure.

DMZs zijn handig omdat u uw netwerk beheren, controle, registratie en rapportage op de apparaten op de rand van het virtuele netwerk Azure zich kunt richten. U zou hier meestal inschakelen DDoS-preventie, Intrusion Detection/inbraak preventie systemen (id/IP-Adressen), firewall-regels en beleid, webfilters, netwerk antimalware en meer. De netwerkapparaten beveiliging tussen het Internet en het virtuele netwerk Azure en beschikt over een interface op beide netwerken.

Dit is de opzet van een DMZ, maar er zijn veel verschillende DMZ ontwerpen, zoals dubbelzijdig, tri-homed, multi-homed en anderen.

Wij raden voor alle implementaties met hoge beveiliging implementeren een DMZ ter verbetering van het niveau van netwerkbeveiliging voor uw resources Azure.

Lees voor meer informatie over DMZs en hoe u ze in Azure, het artikel [Microsoft Cloud Services en netwerkbeveiliging](../best-practices-network-security.md).

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Vermijd blootstelling via een specifieke WAN-verbindingen met het Internet
Veel organisaties hebben de hybride IT route gekozen. In hybride IT zijn sommige van de activa van de informatie van het bedrijf in Azure, terwijl anderen in de lokalen blijven. In veel gevallen zal bepaalde onderdelen van een service worden uitgevoerd in Azure terwijl andere onderdelen op ruimten zijn.

In de hybride IT scenario zich bevindt meestal bepaalde type verbinding tussen bedrijven. Deze cross-premises connectiviteit kan het bedrijf hun netwerken op locatie verbinding met virtuele netwerken Azure. Er zijn twee verschillende ruimten connectiviteitsoplossingen:

- VPN van site naar site
- ExpressRoute

[VPN van site naar site](../vpn-gateway/vpn-gateway-site-to-site-create.md) vertegenwoordigt een virtuele particuliere verbinding tussen uw netwerk op gebouwen en een virtueel netwerk Azure. Deze verbinding vindt plaats via het Internet en u kunt informatie in een gecodeerde verbinding tussen uw netwerk en Azure 'tunnel'. VPN website is een volwassen, veilige technologie die is geïmplementeerd door ondernemingen van alle formaten voor decennia. Tunnel-versleuteling wordt uitgevoerd met behulp van [de IPSec-tunnelmodus](https://technet.microsoft.com/library/cc786385.aspx).

VPN van site naar site is een vertrouwde, betrouwbare en gevestigde technologie, verkeer in de tunnel door het Internet bladeren. Bandbreedte is ook relatief beperkt tot een maximum van over 200Mbps.

Als u een uitzonderlijk niveau van beveiliging of prestaties voor verbindingen met meerdere gebouwen vereist, wordt u aangeraden Azure ExpressRoute te gebruiken voor de verbinding tussen bedrijven. ExpressRoute is een exclusieve WAN verbinding tussen uw locatie op bedrijven of een Exchange-hostingprovider. Omdat dit een telco-verbinding, worden uw gegevens niet via Internet worden verzonden en dus niet aan de potentiële risico's die inherent zijn aan Internet-communicatie wordt blootgesteld.

Lees voor meer informatie over de werking van Azure ExpressRoute en implementatie, [ExpressRoute technisch overzicht](../expressroute/expressroute-introduction.md)van het artikel.

## <a name="optimize-uptime-and-performance"></a>Uptime en prestaties optimaliseren
Vertrouwelijkheid, integriteit en beschikbaarheid (CIA) bestaat uit de drie van de meest invloedrijke beveiligingsmodel van vandaag. Vertrouwelijkheid is informatie over codering en privacy, integriteit is ervoor te zorgen dat gegevens niet door onbevoegden worden gewijzigd en beschikbaarheid ervoor te zorgen is dat gemachtigde gebruikers toegang kunnen krijgen tot de informatie die zij gemachtigd voor toegang tot zijn. Fout in een van deze gebieden vertegenwoordigt een mogelijke inbreuk op de beveiliging.

Beschikbaarheid kan worden beschouwd als zijnde over uptime en prestaties. Als een service niet actief is, kan de informatie kan niet worden geopend. Als de prestaties zo slecht dat de gegevens niet bruikbaar is, dan we rekening met de gegevens houden kunnen niet meer toegankelijk. Vanuit een oogpunt van beveiliging moeten we dus doen wat we kunnen om ervoor te zorgen dat onze diensten hebben optimale uptime en prestaties.
Een populaire en effectieve methode voor het verbeteren van de beschikbaarheid en prestaties is het gebruik van taakverdeling. Netwerktaakverdeling is een methode van netwerkverkeer verdeeld over de servers die deel van een service uitmaken. Bijvoorbeeld, als u front-end-webservers als onderdeel van uw service, kunt u Netwerktaakverdeling het verkeer over de meerdere front-end-webservers verdelen.

Deze verdeling van het verkeer verhoogt beschikbaarheid omdat als een van de webservers niet beschikbaar is, de taakverdeling wordt meer verkeer naar die server verzenden en verkeer omleiden naar servers die nog steeds on line zijn. Taakverdeling helpt ook de prestaties, omdat de processor, netwerk en geheugen overhead voor het voldoen aan aanvragen wordt verdeeld over alle load balanced servers.

Wij raden aan dat u gebruikmaken van taakverdeling zoveel mogelijk en afhankelijk van uw diensten. We zullen geschiktheid in de volgende secties behandeld.
Op het niveau van Azure Virtual Network biedt Azure dat u met drie primaire balancing opties laden:

- HTTP-gebaseerde taakverdeling
- Externe taakverdeling
- Interne taakverdeling

## <a name="http-based-load-balancing"></a>HTTP-gebaseerde taakverdeling
Taakverdeling HTTP-gebaseerde besluiten over welke server voor het verzenden van verbindingen met de kenmerken van het HTTP-protocol is gebaseerd. Azure is een HTTP-taakverdeling die door de naam van de Gateway van de toepassing gaat.

Het is raadzaam dat u ons een toepassingsgateway Azure wanneer:

- Toepassingen waarvoor aanvragen van dezelfde gebruiker/client sessie dezelfde back-end virtuele machine te bereiken. Voorbeelden van deze zou worden shopping cart apps en web-e-mailservers.
- Toepassingen die u vrijmaken van webserver-farms van SSL-beëindiging overhead wilt door gebruik te maken van [SSL-offload](https://f5.com/glossary/ssl-offloading) functie van Application-Gateway.
- Toepassingen, zoals een netwerk leveren van inhoud waarvoor meerdere HTTP-aanvragen op de langdurige TCP-verbinding worden doorgestuurd of laden aan verschillende backend-servers in evenwicht.

Lees het artikel [Application Gateway-overzicht](../application-gateway/application-gateway-introduction.md)voor meer informatie over de werking van Azure Application Gateway en hoe u het kunt gebruiken in uw implementaties.

## <a name="external-load-balancing"></a>Externe Load Balancing
Externe taakverdeling plaatsvindt wanneer binnenkomende verbindingen vanaf het Internet verdeeld tussen de servers die zich in een virtueel netwerk Azure worden. Verdeling van de externe belasting Azure kan bieden deze mogelijkheid en het is raadzaam dat u deze gebruiken wanneer u niet nodig de sticky sessies of SSL-offload.

In tegenstelling tot HTTP-gebaseerde load balancing gebruikt externe laden de verdeling van de informatie op de netwerk- en lagen van het OSI-model voor het netwerk om te bepalen welke server verbinding saldo te laden.

Wij raden u aan u gebruikt externe taakverdeling wanneer er [stateless toepassingen](http://whatis.techtarget.com/definition/stateless-app) accepteren van binnenkomende aanvragen via Internet.

Lees voor meer informatie over hoe de Azure externe taakverdeling werkt en hoe u implementeren kunt, moet u het artikel [aan de slag maken van een Internet Facing taakverdeling in Bronbeheer met PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="internal-load-balancing"></a>Interne taakverdeling
Interne taakverdeling is vergelijkbaar met externe taakverdeling en gebruikt hetzelfde mechanisme voor saldo verbindingen met de servers achter deze laden. Het enige verschil is dat de taakverdeling in dit geval verbindingen accepteert van virtuele machines die niet op het Internet. In de meeste gevallen worden de verbindingen die zijn goedgekeurd voor taakverdeling in gang gezet door apparaten op een virtueel netwerk Azure.

Wij raden u aan gebruik te maken van interne taakverdeling voor scenario's die van deze mogelijkheid gebruikmaken, bijvoorbeeld wanneer u verbindingen met SQL-Servers of interne webservers saldo te laden.

Lees voor meer informatie over hoe Azure interne taakverdeling werkt en hoe u deze kunt implementeren, het artikel [aan de slag maken van een interne taakverdeling met PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md#update-an-existing-load-balancer).

## <a name="use-global-load-balancing"></a>Gebruik globale taakverdeling
Openbare cloud computing maakt het mogelijk voor de implementatie van globaal gedistribueerde toepassingen die onderdelen die zich in datacenters over de hele wereld. Dit is mogelijk op Microsoft Azure als gevolg van Azure datacenter wereldwijde aanwezigheid. In tegenstelling tot de taakverdeling van technologieën die eerder genoemde maakt algemene taakverdeling het mogelijk voor services zelfs hele datacenters mogelijk niet meer beschikbaar.

U kunt dit soort globale taakverdeling in Azure door gebruik te maken van [Azure verkeer Manager](https://azure.microsoft.com/documentation/services/traffic-manager/). Verkeer Manager maakt worden geladen saldo verbindingen met uw diensten op basis van de locatie van de gebruiker.

Bijvoorbeeld als de gebruiker het verzoek naar de service van de EU, is de verbinding gericht op de services zich bevinden in een EU-datacenter. Dit deel van verkeer Manager globale laden balancing helpt de prestaties te verbeteren omdat de verbinding te maken met de dichtstbijzijnde datacenter sneller dan verbinding maken met datacenters die ver weg is.

Aan de beschikbaarheid van zorgt globale taakverdeling ervoor dat de service beschikbaar is, zelfs als een gehele datacenter moet nu beschikbaar zijn.

Bijvoorbeeld, als een Azure datacenter moet niet meer beschikbaar om milieutechnische redenen of als gevolg van storingen (zoals regionale netwerkfouten), verbindingen met uw service zou worden omgeleid naar de dichtstbijzijnde online datacenter. Deze algemene taakverdeling wordt gedaan door gebruik te maken van DNS-beleid dat u in beheer van verkeer maken kunt.

U wordt aangeraden verkeer Manager voor een cloud oplossing ontwikkelen die een sterk gedistribueerde scope heeft in meerdere regio's en het hoogst mogelijke uptime is vereist.

Lees meer informatie over Azure verkeer Manager en de implementatie van het artikel [Wat verkeer Manager is](../traffic-manager/traffic-manager-overview.md).

## <a name="disable-rdpssh-access-to-azure-virtual-machines"></a>RDP/SSH toegang tot Azure Virtual Machines schakelen
Het is mogelijk om tot Azure virtuele Machines met behulp van [Remote Desktop Protocol](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) en de protocollen [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH). Deze protocollen maken het mogelijk om virtuele machines te beheren vanaf een externe locatie en zijn standaard bij de berekening van het datacenter.

Het mogelijke beveiligingsprobleem met behulp van deze protocollen via het Internet is dat aanvallers verschillende [brute kracht](https://en.wikipedia.org/wiki/Brute-force_attack) technieken gebruiken kunnen voor toegang tot Azure virtuele Machines. Zodra de aanvallers toegang krijgen, kunnen de virtuele machine gebruiken als een startpunt voor andere computers op uw Azure virtueel netwerk in gevaar brengt of zelfs aanvallen apparaten in het netwerk buiten Azure.

Daarom is het raadzaam uit te schakelen direct RDP en SSH toegang tot uw Azure virtuele Machines vanaf het Internet. Nadat u directe RDP en SSH-toegang vanaf het Internet is uitgeschakeld, hebt u andere opties die u gebruiken kunt voor toegang tot deze virtuele machines voor extern beheer:

- Point-to-site VPN
- VPN van site naar site
- ExpressRoute

[Point-to-site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) is een andere term voor een RAS-VPN-client/server-verbinding. Een point-to-site VPN kunt één gebruiker via Internet verbinding maken met een virtueel netwerk Azure. Nadat de verbinding punt-naar-site is gemaakt, wordt de gebruiker mogelijk met RDP- of SSH verbinding maken met een virtuele machines in Azure virtuele netwerk waarmee de gebruiker via point-to-site VPN verbonden bevindt. Hierbij wordt ervan uitgegaan dat de gebruiker is gemachtigd om tot deze virtuele machines.

Point-to-site VPN is veiliger dan direct RDP of SSH-verbindingen, omdat de gebruiker moet verifiëren tweemaal voordat u verbinding maakt met een virtuele machine. Ten eerste moet de gebruiker te verifiëren (gemachtigd) vast te stellen van de point-to-site VPN-verbinding; ten tweede moet de gebruiker te verifiëren (mogen) de RDP- of SSH-sessie tot stand brengen.

Een [VPN van site naar site](../vpn-gateway/vpn-gateway-site-to-site-create.md) verbindt een heel netwerk met een ander netwerk via het Internet. U kunt een VPN website verbinding maken met uw netwerk op gebouwen met een virtueel netwerk Azure. Als u een VPN website, de gebruikers implementeren op uw netwerk op ruimten kan verbinding maken met virtuele machines op uw Azure virtueel netwerk met behulp van het RDP- of SSH-protocol via de VPN-verbinding van site naar site en hoeft u directe toegang tot RDP of SSH toestaan via het Internet.

U kunt ook een specifieke WAN-verbinding bieden functionaliteit vergelijkbaar met de VPN-verbinding van site naar site. De belangrijkste verschillen zijn 1. de specifieke WAN-verbinding via niet het Internet en 2. specifieke WAN-verbindingen worden meestal meer stabiel en zodat. Azure biedt een oplossing voor specifieke WAN-verbinding in de vorm van [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="enable-azure-security-center"></a>Azure-Beveiligingscentrum inschakelen
Azure Beveiligingscentrum helpt u bij het voorkomen, opsporen en reageren op bedreigingen en biedt dat u meer inzicht in, en controle over, de beveiliging van uw resources Azure. Het biedt geïntegreerde beveiliging beleid en het toezicht op verschillende de Azure abonnementen, helpt detecteren bedreigingen die anders opgemerkt en werkt met een brede selectie van beveiligingsoplossingen.

Azure Beveiligingscentrum kunt u optimaliseren en beveiliging van het netwerk door te controleren:

- Aanbevelingen voor de beveiliging te bieden
- De status van uw netwerkconfiguratie beveiliging controleren
- U wordt gewaarschuwd voor het netwerk op basis van bedreigingen, zowel op het niveau van het eindpunt en netwerk

We raden u Azure Beveiligingscentrum inschakelen voor al uw Azure-implementaties.

Lees voor meer informatie over Azure Beveiligingscentrum en hoe u kunt instellen voor de distributie, het artikel [Inleiding tot Azure Security Center](../security-center/security-center-intro.md).

## <a name="securely-extend-your-datacenter-into-azure"></a>Veilig uw datacenter uitbreiden naar Azure
Veel zakelijke IT organisaties willen uitbreiden naar de cloud in plaats van in toenemende mate hun datacentra in gebouwen. Deze uitbreiding geeft een uitbreiding van bestaande IT-infrastructuur in de openbare cloud. Door gebruik te maken van cross-ruimten connectiviteitsopties kan uw virtuele netwerken Azure behandelen als een ander subnet voor uw netwerkinfrastructuur op gebouwen.

Er is echter veel planning en ontwerp van de problemen die moeten eerst worden aangepakt. Dit is vooral belangrijk op het gebied van netwerkbeveiliging. Een van de beste manieren om te begrijpen hoe u zo'n ontwerp benadering is een voorbeeld.

Microsoft heeft gemaakt in het [Datacenter Extension Reference Architecture Diagram](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84#content) en ondersteunen van onderpand om te begrijpen wat een datacenter verlenging eruit zou zien. Dit biedt een voorbeeld van de referentie implementatie die u gebruiken kunt voor het plannen en ontwerpen van een veilige datacenter-extensie naar de cloud. We raden u dit document krijgt u een indruk van de belangrijkste onderdelen van een veilige oplossing.

Raadpleeg voor meer informatie over het uitbreiden van uw datacenter veilig in Azure, de video [Uitbreiden van uw Datacenter met Microsoft Azure](https://www.youtube.com/watch?v=Th1oQQCb2KA).

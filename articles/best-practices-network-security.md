<properties
   pageTitle="Microsoft cloud services en network security | Microsoft Azure"
   description="Meer informatie over sommige van de belangrijkste functies beschikbaar in Azure voor het maken van beveiligde netwerkomgevingen"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jonor;sivae"/>

# <a name="microsoft-cloud-services-and-network-security"></a>Microsoft cloud services en netwerk beveiliging

Microsoft cloud services bieden ' hyperscale '-services en -infrastructuur, enterprise-grade mogelijkheden en veel mogelijkheden voor hybride verbindingen. Klanten kunnen kiezen voor toegang tot deze services via het Internet of met Azure ExpressRoute waarmee verbinding met het particuliere netwerk. De Microsoft Azure-platform kan klanten probleemloos hun infrastructuur uitbreiden naar de cloud en bedrijfstoepassingen met meerdere lagen architecturen bouwen. Bovendien kunnen derden inschakelen uitgebreide mogelijkheden bieden beveiligingsservices en virtuele apparaten. Dit artikel biedt een overzicht van beveiligings- en bouwkundige problemen die klanten wordt aangeraden wanneer u Microsoft cloud services via ExpressRoute wordt geopend. Dit omvat ook veiliger services in Azure virtuele netwerken maken.

## <a name="fast-start"></a>Snel starten
De volgende logica grafiek kunt u een specifiek voorbeeld van de vele beveiligingstechnieken met de Azure platform rechtstreeks. Referentie, vinden in het voorbeeld dat het beste past bij uw aanvraag. Voor meer uitleg, gaan door middel van het papier.
![Stroomdiagram voor beveiliging opties][0]

[Voorbeeld 1: Maak een perimeternetwerk (ook wel DMZ, gedemilitariseerde zone en gecontroleerd subnet) ter bescherming van toepassingen met beveiligingsgroepen netwerk (NSGs).](#example-1-build-a-simple-dmz-with-nsgs)</br>
[Voorbeeld 2: Maak een perimeternetwerk ter bescherming van toepassingen met een firewall en NSGs.](#example-2-build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs)</br>
[Voorbeeld 3: Bouw een perimeternetwerk ter beveiliging van netwerken met een firewall, door de gebruiker gedefinieerde route (UDR) en NSG.](#example-3-build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg)</br>
[Voorbeeld 4: Een hybride verbinding met een toestel van site naar site, virtuele virtueel particulier netwerk (VPN) toevoegen.](#example-4-adding-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Voorbeeld 5: Een hybride verbinding met een site naar site, Azure-gateway VPN toevoegen.](#example-5-adding-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn)</br>
[Voorbeeld 6: Een hybride verbinding met ExpressRoute toevoegen.](#example-6-adding-a-hybrid-connection-with-expressroute)</br>
Voorbeelden voor het toevoegen van verbindingen tussen virtuele netwerken, hoge beschikbaarheid en chaining-service wordt toegevoegd aan dit document over de komende paar maanden.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Naleving van regelgeving en infrastructuur bescherming van Microsoft
Microsoft heeft genomen een leidende positie naleving initiatieven vereist enterprise-klanten ondersteunen. Hieronder worden enkele van de certificaten van overeenstemming voor Azure: ![Azure naleving badges][1]

Zie de informatie naleving in het [Vertrouwenscentrum van Microsoft](https://azure.microsoft.com/support/trust-center/compliance/)voor meer informatie.

Microsoft heeft een integrale aanpak ter bescherming van cloud-infrastructuur die nodig is voor het uitvoeren van ' hyperscale ' global services. Microsoft cloud infrastructuur omvat hardware, software, netwerken, en administratieve en operationele personeel, naast de fysieke datacenters.

![Azure beveiligingsfuncties][2]

Deze benadering biedt een veilige basis voor klanten die hun diensten in de Microsoft cloud implementeren. De volgende stap is voor klanten te ontwerpen en maken van de beveiligingsarchitectuur ter bescherming van deze services.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Beveiliging van traditionele architecturen en perimeternetwerken
Hoewel Microsoft zwaar investeert in de cloud-infrastructuur te beveiligen, dient ook klanten beschermen hun cloud diensten en resourcegroepen. Een benadering van meerdere lagen beveiliging biedt de beste verdediging. Interne netwerkbronnen beschermt een beveiligingszone omtrek netwerk tegen een niet-vertrouwd netwerk. Een perimeternetwerk verwijst naar de randen of delen van het netwerk tussen Internet en de beschermde enterprise IT-infrastructuur.

De basisinfrastructuur is in typische bedrijfsnetwerken, sterk verbeterd op zowel met meerdere lagen van beveiligingsapparaten. De grens van elke laag bestaat uit apparaten en uitvoeringspunten beleid. Apparaten kunnen het volgende omvatten: firewalls, Distributed Denial of Service (DDoS) voorkomen, inbraakdetectie of VPN-apparaten en systemen ter bescherming van (id/IP-Adressen). Afdwingen kan de vorm aannemen van firewallbeleid, ACL's (toegangsbeheerlijsten) of specifiek bewerkingsplan. De eerste verdedigingslinie in het netwerk, rechtstreeks acceptatie van inkomend verkeer vanaf het Internet, is een combinatie van deze mechanismen aanvallen blokkeren en schadelijk verkeer terwijl verdere legitieme verzoeken in het netwerk. Deze wegen rechtstreeks tot bronnen in het perimeternetwerk. Die bron kan vervolgens "praten" tot bronnen in het netwerk, de volgende grens voor validatie die dieper eerste. De buitenste laag heet omdat dit deel van het netwerk wordt blootgesteld aan het Internet, meestal met een vorm van bescherming aan beide zijden van het perimeternetwerk. In de volgende afbeelding ziet u een voorbeeld van een perimeternetwerk één subnet in een netwerk, met twee beveiligingsgrenzen.

![Een perimeternetwerk in een bedrijfsnetwerk.][3]

Er zijn vele architecturen gebruikt voor het implementeren van een perimeternetwerk, van een eenvoudige load balancer voor een web-farm met meerdere subnet perimeternetwerk met uiteenlopende mechanismen bij elke grens te blokkeren verkeer en de diepere lagen van het bedrijfsnetwerk te beschermen. Hoe het perimeternetwerk wordt gemaakt, is afhankelijk van de specifieke behoeften van de organisatie en de algehele risicotolerantie.

Als klanten hun werkbelasting wolken op openbare plaatsen, is het van essentieel belang voor de ondersteuning van vergelijkbare mogelijkheden voor netwerkarchitectuur omtrek in Azure om te voldoen aan de naleving van regelgeving en vereisten voor de beveiliging. Dit document bevat richtlijnen over hoe klanten een beveiligde netwerkomgeving in Azure kunnen samenstellen. Het richt zich op het perimeternetwerk, maar bevat ook een uitgebreide bespreking van de vele aspecten van de beveiliging van het netwerk. Deze discussie in kennis van de volgende vragen:

- Hoe kan een perimeternetwerk in Azure opgebouwd?
- Wat zijn de Azure functies beschikbaar zijn voor het perimeternetwerk bouwen?
- Hoe back-end werklasten worden beveiligd?
- Hoe worden Internet-communicatie om de werklast in Azure geregeld?
- Hoe de netwerken op ruimten worden beschermd tegen implementaties in Azure?
- Wanneer moeten native Azure beveiligingsfuncties worden gebruikt in plaats van externe apparaten of services?

Het volgende diagram toont de verschillende lagen van beveiliging die Azure klanten biedt. Deze lagen zijn zowel native in de Azure platform zelf en door de klant gedefinieerde functies:

![Azure beveiligingsarchitectuur][4]

Binnenkomende Internet Azure DDoS helpt tegen grootschalige aanvallen op Azure beschermen. De volgende laag is door de klant gedefinieerde openbare eindpunten, die worden gebruikt om te bepalen welk verkeer via de service cloud kunt doorgeven aan het virtuele netwerk. Native Azure virtuele netwerkisolatie garandeert volledige isolatie en alle andere netwerken, en dat alleen verkeersstromen door de gebruiker geconfigureerde paden en methoden. Deze paden en methoden zijn de volgende laag, waar NSGs, UDR en toestellen voor virtuele netwerken maken beveiligingsgrenzen ter bescherming van de implementatie van de toepassing in het beveiligde netwerk kunnen worden gebruikt.

De volgende sectie wordt een overzicht van Azure virtuele netwerken. Deze virtuele netwerken worden gemaakt door klanten en wat hun werkbelasting geïmplementeerd zijn verbonden zijn. Virtuele netwerken vormen de basis van alle netwerkbeveiligingsfuncties vereist is voor een perimeternetwerk ter beveiliging van implementaties bij klanten in Azure.

## <a name="overview-of-azure-virtual-networks"></a>Overzicht van Azure virtuele netwerken
Voordat u Internet-verkeer kunt u met de Azure virtuele netwerken, zijn er twee lagen van beveiliging die inherent zijn aan het Azure platform:

1.  **DDoS bescherming**: DDoS bescherming is een laag van het Azure fysieke netwerk dat de Azure platform zelf tegen grootschalige aanvallen via Internet beschermt. Deze aanvallen gebruik meerdere knooppunten van "bot" in een poging tot een internetservice overstelpen. Azure is een krachtige DDoS bescherming net op alle binnenkomende verbinding hebben met Internet. Deze laag DDoS bescherming heeft geen configureerbare kenmerken van de gebruiker en niet toegankelijk is voor de klant. Dit beschermt Azure als een platform van grootschalige aanvallen, maar beschermt echter niet rechtstreeks toepassing van de individuele klant. Extra lagen van veerkracht kunnen worden geconfigureerd door de klant voor een gelokaliseerde aanval. Als een klant met een grootschalige DDoS-aanval op een openbare eindpunt is aangevallen, blokkeert Azure verbindingen met de service. Een klant kan failover uitgevoerd naar een ander virtueel netwerk of service endpoint niet betrokken bij de aanval om service te herstellen. Opgemerkt moet worden dat hoewel een klant kan worden beïnvloed op dat eindpunt, geen andere diensten buiten dat eindpunt zou worden beïnvloed. Zie ook de andere klanten en services zou geen gevolgen van deze aanval.
2.  **Service-endpoints**: eindpunten toestaan cloud services of resource groepen openbare Internet-IP-adressen en poorten open zijn. Het eindpunt wordt NAT (Network Address Translation) gebruikt voor het routeren van verkeer naar het interne adres en de poort op de Azure virtueel netwerk. Dit is het primaire pad voor extern verkeer in het virtuele netwerk. De eindpunten zijn gebruiker worden geconfigureerd om te bepalen welk verkeer wordt doorgegeven en hoe en waar het wordt omgezet in het virtuele netwerk.

Zodra het verkeer het virtuele netwerk bereikt, zijn er veel functies die een rol spelen. Azure virtuele netwerken vormen de basis voor klanten om hun werkbelasting en waar van toepassing op netwerkniveau basisbeveiliging is te koppelen. Het is een privé-netwerk (een virtueel netwerk overlay) in Azure voor klanten met de volgende functies en eigenschappen:
 
- **Verkeer-isolatie**: een virtueel netwerk is de grens van de isolatie verkeer op de Azure-platform. Virtuele machines (VMs) in een virtueel netwerk kan niet communiceren rechtstreeks naar VMs in een verschillende virtueel netwerk, zelfs als beide virtuele netwerken worden gemaakt van dezelfde klant. Dit is een belangrijke eigenschap die ervoor zorgt dat de klant VMs en communicatie in een virtueel netwerk privé blijft.
- **Multitier topologie**: virtuele netwerken kunnen klanten multitier topologie te definiëren door afzonderlijke adresruimten voor verschillende elementen of 'lagen' van hun werkbelasting te ontwerpen bij het toewijzen van subnetten. Deze logische groeperingen topologieën kunnen klanten verschillende-beleid op basis van het type werklast definiëren en regelen ook verkeersstromen tussen de lagen.
- **Cross-premises connectiviteit**: klanten kunnen tot stand brengen tussen lokalen verbinding tussen een virtueel netwerk en meerdere sites voor op lokalen of andere virtuele netwerken in Azure. Hiertoe kunnen klanten Azure VPN-Gateways of virtuele apparaten van derden gebruiken. Azure ondersteunt-website (S2S) VPN-verbindingen met behulp van standaard IPsec/IKE-protocollen en ExpressRoute particuliere verbindingen.
- **NSG** kunnen klanten maken regels (ACL's) op het gewenste niveau van granulatie: netwerk interfaces, afzonderlijke VMs of virtuele subnetten. Klanten kunnen toegang beheren door toestaan of weigeren van communicatie tussen de werkbelasting in een virtueel netwerk, van systemen op de netwerken van de klant via connectiviteit tussen ruimten, of directe Internet-communicatie.
- **UDR** en **Doorsturen via IP** kunnen klanten de communicatiepaden tussen verschillende lagen van een virtueel netwerk definiëren. Klanten kunnen implementeren van een firewall, id's / IP-Adressen en andere toestellen van een virtuele en routeren van netwerkverkeer via deze toestellen beveiliging voor beveiliging grens wordt afgedwongen, controle en inspectie.
- **Netwerk virtuele apparaten** in de markt Azure: beveiligingsapparaten, zoals firewalls en netwerktaakverdeling id/IP-Adressen beschikbaar zijn in de markt Azure en de VM afbeeldinggalerie. Klanten kunnen deze apparaten in hun virtuele netwerken en met name op de beveiligingsgrenzen van hun (met inbegrip van de omtrek subnetten) implementeren om een omgeving met een beveiligd netwerk.

Met deze functies en mogelijkheden is een voorbeeld van hoe een perimeter network-architectuur kan worden opgebouwd in Azure als volgt:

![Een perimeternetwerk in een virtueel netwerk van Azure][5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Perimeter network kenmerken en vereisten
Het perimeternetwerk is ontworpen om te worden van de voorkant van het netwerk, de communicatie via het Internet rechtstreeks in aanraking komt. Binnenkomende pakketten moeten gaan door het beveiligingsapparaten, zoals de firewall, id's en IP-Adressen, alvorens de back-end servers. Internet-gebonden pakketten van de werklast kunnen ook door de beveiliging toestellen in het perimeternetwerk voor afdwingen, inspectie- en controledoeleinden, vóór het verlaten van het netwerk stromen. Bovendien kunt u het perimeternetwerk cross lokalen VPN-gateways tussen klant virtuele netwerken en netwerken in gebouwen onderbrengen.

### <a name="perimeter-network-characteristics"></a>Omtrek-netwerkeigenschappen
Verwijzen naar de vorige afbeelding, zijn sommige van de kenmerken van een goede perimeternetwerk als volgt:

- Internetverbinding:
    - De perimeter network subnet zelf is internetverbinding rechtstreeks communiceren met het Internet.
    - Openbare IP-adressen, VIP's en/of service-endpoints geven Internet-verkeer op de front-netwerk en apparaten.
    - Het binnenkomende gegevensverkeer van het Internet passeert beveiligingsapparaten voordat andere bronnen op de front-end-netwerk.
    - Als de beveiliging van uitgaande gegevens is ingeschakeld, verkeer wordt toegelaten beveiligingsapparaten, als de laatste stap, voordat u doorgeven aan het Internet.
- Beveiligd netwerk:
    - Er is geen directe pad van het Internet naar de basisinfrastructuur.
    - Kanalen aan de basisinfrastructuur moeten passeren via beveiligingsapparaten, zoals NSGs, firewalls en VPN-apparaten.
    - Internet en de basisinfrastructuur moeten geen brug in andere apparaten.
    - Beveiligingsapparaten op het beveiligde netwerk naar de grenzen van het perimeternetwerk (bijvoorbeeld de twee firewall pictogrammen weergegeven in de vorige afbeelding) en de internetverbinding mogelijk daadwerkelijk één virtueel toestel met gedifferentieerde regels of interfaces voor elke grens. (Dat wil zeggen één apparaat, logisch gescheiden, verwerking van de belasting voor beide grenzen van het perimeternetwerk.)
- Andere algemene praktijken en beperkingen:
    - Werklast moeten essentiële bedrijfsgegevens niet opslaan.
    - Toegang en updates op omtrek netwerkconfiguraties en implementaties zijn beperkt tot alleen gemachtigde beheerders.

### <a name="perimeter-network-requirements"></a>Omtrek netwerkvereisten
Volg deze richtlijnen op een virtueel netwerkvereisten voor het implementeren van een succesvolle perimeternetwerk zodat deze kenmerken:

- **Subnet architectuur:** Het virtuele netwerk opgeven dat een hele subnet als het perimeternetwerk, gescheiden van andere subnetten in hetzelfde virtuele netwerk is toegewezen. Hierdoor wordt het verkeer tussen het perimeternetwerk en andere interne of particuliere subnet lagen stromen via een firewall of virtueel toestel id/IP-Adressen op de grenzen van het subnet met door de gebruiker gedefinieerde routes.
- **NSG:** De perimeter network subnet zelf moet openstaan voor de communicatie met het Internet, maar dat betekent niet dat klanten moeten worden via NSGs. Volg de algemene procedures voor beveiliging te minimaliseren van de oppervlakken van het netwerk toegankelijk via Internet. De externe adresbereiken krijgen toegang tot de installaties of de specifieke toepassingsprotocollen en poorten die geopend zijn vergrendeld. Er zijn echter omstandigheden waarin dit niet altijd mogelijk is. Bijvoorbeeld als klanten een externe website in Azure hebben, het perimeternetwerk moet de binnenkomende webverzoeken van openbare IP-adressen toestaan, maar alleen de web application poorten moet openen: TCP:80 en TCP:443.
- **Routing tabel:** De perimeter network subnet zelf moet kunnen communiceren rechtstreeks met het Internet, maar mag geen directe communicatie van en naar de back end of op gebouwen netwerken zonder tussenkomst van een firewall of toestel.
- **Toestel beveiligingsconfiguratie:** Apparaten kunnen worden multi-homed om pakketten tussen het perimeternetwerk en de rest van de beveiligde netwerken, de beveiligingsapparaten, zoals firewall-id en IP-Adressen te inspecteren en te routeren. Ze kunnen afzonderlijke NIC's voor het perimeternetwerk en de back-end subnetten hebben. De NIC's in het perimeternetwerk communiceren rechtstreeks van en naar het Internet, met de bijbehorende NSGs en de perimeter network-routeringstabel. De NIC's verbinding te maken met de back-end subnetten hebben meer beperkte NSGs en in de routeringstabellen van de bijbehorende subnetten met back-end.
- **Toestel beveiligingsfunctionaliteit:** De gewoonlijk geïmplementeerd in het perimeternetwerk beveiligingsapparaten voert u de volgende functionaliteit:
    - Firewall: Afdwingen van firewall-regels of beleid voor het beheren van toegang voor binnenkomende aanvragen.
    - Dreiging van detectie en preventie: detecteren en schadelijke aanvallen vanaf het Internet te beperken.
    - Controle en logboekregistratie: handhaving van gedetailleerde logboeken voor controle en analyse.
    - Reverse-proxy: de inkomende aanvragen omleiden naar de corresponderende back-end servers. Dit omvat de toewijzing en meestal vertalen de doeladressen van de front-apparaten, firewalls, naar de back-end-serveradressen.
    - Proxy doorsturen: NAT bieden en het uitvoeren van controles voor communicatie die afkomstig zijn van binnen het virtuele netwerk met het Internet.
    - Router: Doorsturen van verkeer van inkomende en cross-subnet binnen het virtuele netwerk.
    - VPN-apparaat: fungeert als VPN-gateways cross lokalen voor cross-ruimten VPN-verbindingen tussen netwerken van klanten op gebouwen en Azure virtuele netwerken.
    - VPN-server: accepteren van VPN-clients verbinding maken met Azure virtuele netwerken.

>[AZURE.TIP] Bewaar de volgende twee groepen: de personen die toegang hebben tot de perimeter network security vistuig en de personen gemachtigd als beheerders voor ontwikkeling, implementatie of bewerkingen. Deze groepen apart houden zorgt voor een scheiding van taken en voorkomen dat één persoon zowel beveiliging van toepassingen en besturingselementen voor de beveiliging.

### <a name="questions-to-be-asked-when-building-network-boundaries"></a>Vragen om te worden gevraagd bij het samenstellen van de grenzen van het netwerk
In deze sectie, tenzij uitdrukkelijk wordt vermeld, verwijst de term "netwerken" naar Azure virtuele privénetwerken gemaakt door een beheerder van een abonnement. De term verwijst niet naar de onderliggende fysieke netwerken in Azure.

Azure virtuele netwerken worden ook vaak gebruikt voor traditionele op ruimten netwerken uitbreiden. Het is mogelijk op te nemen van site naar site of ExpressRoute hybride netwerkoplossingen met omtrek netwerkarchitecturen. Dit is een belangrijke overweging bij het bouwen van netwerk beveiligingsgrenzen.

De volgende drie vragen zijn te beantwoorden als u bij het bouwen van een netwerk met een perimeternetwerk en meerdere beveiligingsgrenzen kritiek.

#### <a name="1-how-many-boundaries-are-needed"></a>1) hoeveel grenzen nodig zijn?
Het eerste punt dat besluit is om te beslissen hoeveel beveiligingsgrenzen nodig zijn in een bepaald scenario:

- Een enkele rand: een op de front-end-perimeternetwerk tussen het virtuele netwerk en het Internet.
- Twee grenzen: een op het Internet aan de zijkant van het perimeternetwerk en een andere tussen de perimeter network subnet en de back-end subnetten in de Azure virtuele netwerken.
- De grenzen van drie: één aan de kant van Internet van het perimeternetwerk, tussen het perimeternetwerk en de back-end subnetten en één tussen de back-end-subnetten en het netwerk op gebouwen.
- N grenzen: het nummer van een variabele. Afhankelijk van de beveiligingsvereisten is er echt een willekeurig aantal beveiligingsgrenzen die in een bepaald netwerk kan worden toegepast.

Het aantal en type van de grenzen nodig zal afhankelijk van risicotolerantie van een bedrijf en de specifieke scenario wordt geïmplementeerd. Dit is vaak een gezamenlijk besluit van meerdere groepen in een organisatie, vaak met inbegrip van risico's en naleving team, een netwerk en platform team en het ontwikkelteam van een toepassing. Mensen met kennis van de beveiliging, de betrokken gegevens en de gebruikte technologieën moeten inspraak hebben in deze beschikking om de juiste koers voor elke implementatie te waarborgen.

>[AZURE.TIP] Gebruik het kleinste aantal begrenzingen die voldoen aan de beveiligingsvereisten voor een bepaalde situatie. Met meer grenzen moeilijker bewerkingen en het oplossen van problemen kunnen worden, en de overhead die betrokken zijn bij het beheren van meerdere grens beleid na verloop van tijd. Echter verhogen onvoldoende grenzen risico. Zoeken naar het saldo is essentieel.

![Hybride netwerk met drie beveiligingsgrenzen][6]

De voorgaande afbeelding ziet u een globaal overzicht gegeven van een netwerk met drie beveiliging grens. De grens ligt tussen het perimeternetwerk en het Internet, de Azure front-end en back-end particuliere subnetten, en de Azure back-end-subnet en het bedrijfsnetwerk op gebouwen.

#### <a name="2-where-are-the-boundaries-located"></a>2) waar de grenzen zich bevinden?
Als het nummer van de grenzen worden besloten, is waar voor de tenuitvoerlegging ervan het volgende besluit. In het algemeen zijn er drie opties:
- Met behulp van een Internet-gebaseerde intermediaire-service (bijvoorbeeld een cloud-gebaseerde Web application firewall, die niet in dit document wordt besproken)
- Met behulp van ingebouwde functies en/of virtuele netwerkapparaten in Azure
- Fysieke apparaten op het netwerk op ruimten gebruiken

De opties zijn in zuiver Azure netwerken Azure functies (bijvoorbeeld Azure netwerktaakverdeling) of virtuele toestellen voor netwerken uit de rijke partner ecosysteem van Azure (bijvoorbeeld Check Point firewall).

Als een grens tussen Azure en een netwerk op locatie nodig is, kunnen de beveiligingsapparatuur zich bevinden aan weerszijden van de verbinding (of beide zijden). Dus moet een besluit geschieden op de locatie plaatsen beveiliging vistuig.

In de vorige afbeelding is de Internet-perimeternetwerk en de grenzen van de voorzijde back end-volledig zijn opgenomen in Azure, en moeten Azure functies of netwerk virtuele apparaten. Beveiligingsapparaten op de grens tussen Azure (back-end subnet) en het bedrijfsnetwerk kunnen worden op de Azure zijde of de zijde van lokalen of zelfs een combinatie van de apparaten op beide zijden. Kunnen er aanzienlijke voordelen en nadelen van beide opties die serieus moet worden overwogen.

Met behulp van bestaande fysieke veiligheid vistuig aan de kant van lokalen netwerk heeft bijvoorbeeld het voordeel dat er geen nieuwe versnelling nodig is. Deze moet nieuwe configuratie. Het nadeel is echter dat al het verkeer van Azure met het netwerk op ruimten terugkomen moet kunnen worden bekeken door de versnelling van de beveiliging. Dus verkeer Azure naar Azure aanzienlijke vertraging kan oplopen en invloed hebben op prestaties en de gebruiker ervaring, als deze is gedwongen terug naar het netwerk van ruimten voor beveiliging wordt afgedwongen.

#### <a name="3-how-are-the-boundaries-implemented"></a>3) hoe worden de grenzen geïmplementeerd?
Elke beveiligingsgrens zal waarschijnlijk gelden verschillende mogelijkheden (bijvoorbeeld id's en firewall-regels aan de kant van Internet van het perimeternetwerk, maar de ACL's tussen het perimeternetwerk en de back-end-subnet). Bepalen welke apparaten te gebruiken, is afhankelijk van de vereisten van de scenario en beveiliging. In de volgende sectie besproken voorbeelden 1, 2 en 3 bepaalde opties die kunnen worden gebruikt. Controleren van de netwerkfuncties Azure eigen en de apparaten van het ecosysteem van partners in Azure beschikbaar ziet u allerlei opties voor het oplossen van vrijwel elk scenario.

Een ander punt van de uitvoering van belangrijke beslissing is hoe u kunt verbinding maken met het netwerk op ruimten met Azure. Moet u de Azure virtuele gateway of een virtueel netwerk-toestel gebruiken? Deze opties worden besproken in meer details in de volgende sectie (4, 5 en 6 voorbeelden).

Bovendien verkeer tussen virtuele netwerken in Azure nodig zijn. Deze scenario's wordt op een later tijdstip toegevoegd.

Als u eenmaal het antwoord op de vorige vragen weet, kunt de sectie [Snel starten](#fast-start) achterhalen welke voorbeelden zijn het meest geschikt is voor een bepaald scenario.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Voorbeelden: Beveiligingsgrenzen met Azure virtuele netwerken maken
### <a name="example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs"></a>Voorbeeld 1: Een perimeternetwerk ter bescherming van toepassingen met NSGs maken
[Terug naar Fast start](#fast-start) | [gedetailleerde instructies voor dit voorbeeld bouwen][Example1]

![Inkomende perimeternetwerk met NSG][7]

#### <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement dat het volgende bevat:

- Twee cloud services: "FrontEnd001" en "BackEnd001"
- Een virtueel netwerk, "CorpNetwork", met twee subnetten: 'FrontEnd' en 'End'
- Een netwerk-beveiligingsgroep die wordt toegepast op beide subnetten
- Een Windows-server met een web application server ("IIS01")
- Twee Windows-servers die toepassing back-end-servers ("AppVM01", "AppVM02")
- Een Windows-server met een DNS-server ("DNS01")

Zie de [build voor gedetailleerde instructies]voor scripts en een sjabloon Azure Resource Manager[Example1].

#### <a name="nsg-description"></a>Beschrijving van de NSG
In dit voorbeeld wordt een groep NSG gebouwd en vervolgens geladen met zes regels.

>[AZURE.TIP] In het algemeen maakt u uw specifieke regels voor 'Toestaan' eerst, gevolgd door de algemene regels van de "Deny". De opgegeven prioriteit bepaalt welke regels worden eerst geëvalueerd. Als verkeer toe te passen op een bepaalde regel wordt gevonden, worden geen verdere regels worden geëvalueerd. NSG-regels kunnen in de binnenkomende of uitgaande richting (vanuit het perspectief van het subnet) toepassen.

Declaratief, worden de volgende regels voor binnenkomend verkeer gebouwd:

1.  Interne DNS-verkeer (poort 53) is toegestaan.
2.  RDP-verkeer (poort 3389) van het Internet met een virtuele Machine is toegestaan.
3.  HTTP-verkeer (poort 80) van de Internet web server (IIS01) is toegestaan.
4.  Al het verkeer (alle poorten) van IIS01 naar AppVM1 is toegestaan.
5.  Al het verkeer (alle poorten) van het Internet naar het volledige virtuele netwerk (beide subnetten) is geweigerd.
6.  Al het verkeer (alle poorten) van de front-subnet aan de back-end-subnet is geweigerd.

Met deze regels gebonden aan elk subnet als een HTTP-aanvraag inkomende vanaf het Internet naar de webserver, worden beide regels 3 is (toestaan) en 5 (weigeren) van toepassing. Maar omdat de regel 3 heeft een hogere prioriteit, alleen zou worden toegepast, en regel 5 niet zou komen in het spel. Dus de HTTP-aanvraag naar de webserver mogen. Als dat hetzelfde verkeer is probeert te bereiken van de server DNS01, regel 5 (weigeren) zou de eerste om toe te passen, en het verkeer niet mogen doorgeven aan de server. Regel 6 (weigeren) blokkeert het front-subnet in gesprekken met de back-end-subnet (met uitzondering van toegestane netwerkverkeer in de regels 1 en 4). Dit beschermt de back-end-netwerk als een aanvaller de webtoepassing op de front-endtoepassing te storen. De aanvaller zou beperkte toegang hebben tot de back-end ' beveiligde' netwerk (alleen bronnen beschikbaar op de server AppVM01).

Er is een standaard uitgaande regel die verkeer van het Internet. In dit voorbeeld zijn we uitgaand verkeer toestaan en uitgaande regels niet wijzigen. Om het verkeer in beide richtingen worden vergrendeld, door de gebruiker gedefinieerde bewerkingsplan vereist is (Zie voorbeeld 3).

#### <a name="conclusion"></a>Conclusie
Dit is een relatief eenvoudige en gemakkelijke manier van het subnet van de back-end van inkomend verkeer te isoleren. Zie voor meer informatie de [build met gedetailleerde instructies][Example1]. Deze instructies bevatten:

- Het bouwen van deze perimeternetwerk met PowerShell-scripts.
- Hoe deze perimeternetwerk Azure Resource Manager de sjabloon maakt.
- Gedetailleerde beschrijvingen van elke opdracht NSG.
- Gedetailleerde verkeer stroom scenario's, hoe verkeer wordt toegestaan of geweigerd in elke laag weergegeven.


 ### <a name="example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs"></a>Voorbeeld 2: Een perimeternetwerk ter bescherming van toepassingen met een firewall en NSGs bouwen
[Terug naar Fast start](#fast-start) | [gedetailleerde instructies voor dit voorbeeld bouwen][Example2]

![Inkomende perimeternetwerk met NVA en NSG][8]

#### <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement dat het volgende bevat:

- Twee cloud services: "FrontEnd001" en "BackEnd001"
- Een virtueel netwerk, "CorpNetwork", met twee subnetten: 'FrontEnd' en 'End'
- Een netwerk-beveiligingsgroep die wordt toegepast op beide subnetten
- Een virtueel netwerkapparaat, in dit geval een firewall, verbonden met het front-subnet
- Een Windows-server met een web application server ("IIS01")
- Twee Windows-servers die toepassing back-end-servers ("AppVM01", "AppVM02")
- Een Windows-server met een DNS-server ("DNS01")

Zie de [build voor gedetailleerde instructies]voor scripts en een sjabloon Azure Resource Manager[Example2].

#### <a name="nsg-description"></a>Beschrijving van de NSG
In dit voorbeeld wordt een groep NSG gebouwd en vervolgens geladen met zes regels.

>[AZURE.TIP] In het algemeen maakt u uw specifieke regels voor 'Toestaan' eerst, gevolgd door de algemene regels van de "Deny". De opgegeven prioriteit bepaalt welke regels worden eerst geëvalueerd. Als verkeer toe te passen op een bepaalde regel wordt gevonden, worden geen verdere regels worden geëvalueerd. NSG-regels kunnen in de binnenkomende of uitgaande richting (vanuit het perspectief van het subnet) toepassen.

Declaratief, worden de volgende regels voor binnenkomend verkeer gebouwd:

1.  Interne DNS-verkeer (poort 53) is toegestaan.
2.  RDP-verkeer (poort 3389) van het Internet met een virtuele Machine is toegestaan.
3.  Alle Internet-verkeer (alle poorten) op het netwerk virtueel toestel (firewall) is toegestaan.
4.  Al het verkeer (alle poorten) van IIS01 naar AppVM1 is toegestaan.
5.  Al het verkeer (alle poorten) van het Internet naar het volledige virtuele netwerk (beide subnetten) is geweigerd.
6.  Al het verkeer (alle poorten) van de front-subnet aan de back-end-subnet is geweigerd.

Met deze regels gebonden aan elk subnet als een HTTP-aanvraag inkomende vanaf het Internet naar de firewall, worden beide regels 3 is (toestaan) en 5 (weigeren) van toepassing. Maar omdat de regel 3 heeft een hogere prioriteit, alleen zou worden toegepast, en regel 5 niet zou komen in het spel. Het HTTP-verzoek zou dus de firewall worden toegestaan. Als dat hetzelfde verkeer is probeert te bereiken van de server IIS01, ook al dit op de front-end-subnet is, regel 5 (weigeren) zou gelden, en het verkeer niet mogen doorgeven aan de server. Regel 6 (weigeren) blokkeert het front-subnet in gesprekken met de back-end-subnet (met uitzondering van toegestane netwerkverkeer in de regels 1 en 4). Dit beschermt de back-end-netwerk als een aanvaller de webtoepassing op de front-endtoepassing te storen. De aanvaller zou beperkte toegang hebben tot de back-end ' beveiligde' netwerk (alleen bronnen beschikbaar op de server AppVM01).

Er is een standaard uitgaande regel die verkeer van het Internet. In dit voorbeeld zijn we uitgaand verkeer toestaan en uitgaande regels niet wijzigen. Om het verkeer in beide richtingen worden vergrendeld, door de gebruiker gedefinieerde bewerkingsplan vereist is (Zie voorbeeld 3).

#### <a name="firewall-rule-description"></a>Beschrijving van de firewall regel
Op de firewall, moet doorsturen van regels worden gemaakt. Regel is nodig omdat in dit voorbeeld in gebonden aan de firewall voor Internet-verkeer alleen routes en vervolgens naar de webserver één doorsturen netwerkadresomzetting (NAT).

De regel doorsturen accepteert alle inkomende bronadressen die de firewall probeert te bereiken, HTTP (poort 80 of 443 voor HTTPS). Verzonden niet op de lokale interface van de firewall is en omgeleid naar de server met het IP-adres van de 10.0.1.5.

#### <a name="conclusion"></a>Conclusie
Dit is een relatief eenvoudige manier van beveiligen van uw toepassing met een firewall en het subnet van de back-end van inkomend verkeer te isoleren. Zie voor meer informatie de [build met gedetailleerde instructies][Example2]. Deze instructies bevatten:

- Het bouwen van deze perimeternetwerk met PowerShell-scripts.
- Het bouwen van dit voorbeeld de sjabloon Azure Resource Manager.
- Gedetailleerde beschrijvingen van elke NSG opdracht- en firewall-regel.
- Gedetailleerde verkeer stroom scenario's, hoe verkeer wordt toegestaan of geweigerd in elke laag weergegeven.

### <a name="example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-udr-and-nsg"></a>Voorbeeld 3: Een perimeternetwerk ter beveiliging van netwerken met een firewall, UDR en NSG bouwen
[Terug naar Fast start](#fast-start) | [gedetailleerde instructies voor dit voorbeeld bouwen][Example3]

![Bi-directionele perimeternetwerk met NVA, NSG en UDR][9]

#### <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement dat het volgende bevat:

- Drie cloud services: "SecSvc001", "FrontEnd001" en "BackEnd001"
- Een virtueel netwerk, en 'CorpNetwork', met drie subnetten: 'SecNet', 'FrontEnd' en 'End'
- Een virtueel netwerkapparaat, in dit geval een firewall, aangesloten op het subnet van de SecNet
- Een Windows-server met een web application server ("IIS01")
- Twee Windows-servers die toepassing back-end-servers ("AppVM01", "AppVM02")
- Een Windows-server met een DNS-server ("DNS01")

Zie de [build voor gedetailleerde instructies]voor scripts en een sjabloon Azure Resource Manager[Example3].

#### <a name="udr-description"></a>UDR beschrijving
De volgende systeem routes worden standaard gedefinieerd als:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

De VNETLocal is altijd de prefix(es) gedefinieerd adres van het virtuele netwerk voor dat specifieke netwerk (dat wil zeggen, verandert van het virtuele netwerk op een virtueel netwerk, afhankelijk van hoe elke specifieke virtuele netwerk wordt gedefinieerd). De resterende systeem routes zijn statisch en standaard aangegeven in de tabel.

In dit voorbeeld worden twee routeringstabellen gemaakt, één voor de front-end- en back-end-subnetten. Elke tabel wordt geladen met statische routes die geschikt zijn voor een bepaald subnet. In dit voorbeeld heeft elke tabel drie routes die alle verkeer (0.0.0.0/0) via de firewall (volgende hop = virtuele toestel IP-adres):

1. Lokale subnetverkeer met geen volgende Hop gedefinieerd voor het lokale subnetverkeer op de firewall te passeren.
2. Virtueel netwerkverkeer met een volgende Hop gedefinieerd als een firewall; Dit overschrijft de standaardregel die lokale virtuele netwerkverkeer doorsturen rechtstreeks.
3. Alle resterende verkeer (0/0) met een volgende Hop gedefinieerd als de firewall.

>[AZURE.TIP] Niet met de vermelding van het lokale subnet in de UDR verbroken communicatie lokale subnet. 
> - In ons voorbeeld 10.0.1.0/24 VNETLocal aan te wijzen is van essentieel belang als anderszins, wanneer u pakket bestemd is voor een andere lokale server (bijvoorbeeld) 10.0.1.25 webserver (10.0.1.4) mislukken zal als ze naar de NVA die het naar het subnet stuurt, en het subnet wordt opnieuw naar de NVA enzovoort.
> - Kans om een routeringslus is meestal hoger op toestellen met meerdere NIC's die rechtstreeks zijn aangesloten op elk subnet dat zij communiceert, die vaak van traditionele, op-gebouwen, toestellen. 

Zodra de routeringstabellen zijn gemaakt, zijn ze gebonden aan hun subnetten. De front-subnet routeringstabel, eenmaal gemaakt en afhankelijk van het subnet, eruit als volgt:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

>[AZURE.NOTE] UDR kan nu worden toegepast op de gateway-subnet waarop het ExpressRoute circuit is verbonden.
>
> Voorbeelden van het inschakelen van uw perimeternetwerk met ExpressRoute of netwerken van site naar site worden weergegeven in de voorbeelden van 3 en 4.


#### <a name="ip-forwarding-description"></a>Doorsturen via IP-beschrijving
Doorsturen via IP is een aanvullende functie voor UDR. Dit is een instelling op een virtueel toestel waarmee het verkeer niet specifiek gericht op het toestel ontvangen en doorgestuurd dat verkeer naar de uiteindelijke bestemming.

Bijvoorbeeld, als u verkeer van AppVM01 een aanvraag naar de server DNS01, zou UDR route dit met de firewall. Doorsturen via IP ingeschakeld, wordt het verkeer voor de bestemming van de DNS01 (10.0.2.4) geaccepteerd door het toestel (adres 10.0.0.4) en vervolgens doorgestuurd naar de uiteindelijke bestemming (10.0.2.4). Doorsturen via IP op de firewall ingeschakeld, zonder dat zou verkeer niet aanvaardbaar zijn door het toestel, hoewel de routetabel de firewall als de volgende hop is. Voor het gebruik van een virtueel toestel, is het belangrijk om te onthouden IP-doorsturen inschakelen in combinatie met UDR.

#### <a name="nsg-description"></a>Beschrijving van de NSG
In dit voorbeeld wordt een groep NSG gebouwd en vervolgens met een enkele regel worden geladen. Deze groep wordt vervolgens uitsluitend gekoppeld aan de front-end- en back-end-subnetten (niet SecNet). Declaratief wordt opgebouwd voor de volgende regel:

- Al het verkeer (alle poorten) van het Internet naar het volledige virtuele netwerk (alle subnetten) is geweigerd.

Hoewel in dit voorbeeld NSGs gebruikt worden, is het belangrijkste doel als een secundaire verdedigingslinie tegen onjuiste voor handmatige configuratie. Het doel is voor het blokkeren van al het binnenkomende verkeer vanaf het Internet naar de front-end- of back-end subnetten. Verkeer alleen via het subnet SecNet met de firewall moet de stroming (en, in voorkomend geval, op de front-end- of back-end subnetten). Plus, met de UDR regels, zou alle verkeer dat het in de front-end- of back-end subnetten maken worden geleid uit met de firewall (dankzij UDR). De firewall ziet dit als een asymmetrische stroom en het uitgaande verkeer zou dalen. Er zijn dus drie lagen van beveiliging beschermt de subnetten:
- Geen open eindpunten op de FrontEnd001 en BackEnd001 cloud services.
- NSGs verkeer vanaf het Internet te weigeren.
- De firewall weghalen asymmetrische verkeer.

Een interessant punt met betrekking tot de NSG in dit voorbeeld is slechts één regel, die is het blokkeren van Internet-verkeer op het gehele virtuele netwerk, met inbegrip van de beveiliging subnet bevat. Echter, aangezien de NSG alleen aan de front-end- en back-end subnetten gebonden is, de regel niet verwerkt op verkeer in het subnet van de beveiliging. Als gevolg hiervan zullen verkeer naar het subnet beveiliging vloeien.

#### <a name="firewall-rules"></a>Firewall-regels
Op de firewall, moet doorsturen van regels worden gemaakt. Aangezien de firewall blokkeren of doorsturen van alle inkomende, uitgaande en binnen virtuele netwerkverkeer is, zijn veel firewall-regels nodig. Bovendien wordt al het binnenkomende verkeer Security Service openbare IP-adres (op verschillende poorten), druk moet worden verwerkt door de firewall. Beste is het diagram van de logische stroom voor het instellen van de subnetten en firewall-regels, om te voorkomen dat later bijwerken. In de volgende afbeelding is een logische weergave van de firewall-regels voor dit voorbeeld:
 
![Logische weergave van de firewall-regels][10]

>[AZURE.NOTE] Op basis van het virtuele netwerk-toestel gebruikt, zal de poorten voor beheertaken variëren. In dit voorbeeld wordt een Firewall Barracuda NextGen naar wordt verwezen, welke poorten 22, 801 en 807 gebruikt. Raadpleeg de documentatie van de leverancier toestel om te vinden welke poorten gebruikt voor het beheer van het apparaat wordt gebruikt.

#### <a name="firewall-rules-description"></a>Beschrijving van firewall-regels
Het subnet beveiliging wordt niet weergegeven in de voorgaande logisch diagram. Dit is omdat de firewall de enige bron op dat subnet is, en in dit diagram wordt de firewall-regels en hoe ze logisch toestaan of verkeersstromen, niet de werkelijke gerouteerde pad weigeren weergegeven. Ook de externe poorten geselecteerd voor het RDP-verkeer zijn hogere varieerde poorten (8014 – 8026) en enigszins uitlijnen met de laatste twee octets van het lokale IP-adres voor de leesbaarheid gemakkelijker zijn geselecteerd (bijvoorbeeld adres van de lokale server 10.0.1.4 is gekoppeld aan een externe poort 8014). Alle poorten hoger niet-conflicterende kunnen echter worden gebruikt.

In dit voorbeeld moeten we zeven soorten regels:

- Externe regels (voor binnenkomend verkeer):
  1.    Management firewallregel: deze App omleiden regel kan verkeer worden doorgegeven aan de poorten voor beheertaken van het virtuele netwerk-toestel.
  2.    RDP-regels (voor elke Windows server): deze vier regels (één voor elke server) kunt u beheer van de afzonderlijke servers via RDP. Dit kan ook worden gebundeld in één regel, afhankelijk van de mogelijkheden van het virtuele netwerk-toestel wordt gebruikt.
  3.    Regels voor het verkeer van toepassing: Er zijn twee van deze regels, de eerste voor het verkeer van Internet gebaseerde front-end- en de tweede voor de back-end-verkeer (bijvoorbeeld webserver gegevenslaag). De configuratie van deze regels is afhankelijk van de netwerkarchitectuur (waar uw servers worden geplaatst) en stromen verkeer (welke richting de verkeersstromen en welke poorten worden gebruikt).
      - De eerste regel kan het verkeer toepassing kunnen bereiken van de application server. Terwijl de andere voorschriften voor beveiliging en beheer, zijn regels voor het verkeer van toepassing wat kunnen externe gebruikers of services voor toegang tot de toepassingen. Voor dit voorbeeld is één webserver op poort 80. Aldus wordt een firewallregel voor één toepassing inkomend verkeer omgeleid naar het externe IP-adres, aan het interne IP-adres van de web-servers. De sessie omgeleid verkeer zou via NAT worden omgezet naar de interne server.
      - De tweede regel is de regel back-end om de webserver contact opnemen met de AppVM01-server (maar niet AppVM02) via een willekeurige poort.
- Interne regels (voor intra virtuele netwerkverkeer)
  4.    Uitgaande Internet-regel: deze regel staat verkeer van elk netwerk worden doorgegeven aan de geselecteerde netwerken. Deze regel is meestal een standaardregel voor reeds op de firewall, maar uitgeschakeld. Deze regel moet worden ingeschakeld voor dit voorbeeld.
  5.    DNS-regel: deze regel alleen DNS (poort 53) verkeer worden doorgegeven aan de DNS-server is toegestaan. De meeste verkeer van de front-end naar de back-end is geblokkeerd voor deze omgeving. Deze regel staat specifiek DNS van een lokaal subnet.
  6.    Subnet, subnet regel: deze regel is dat elke server op de back-end subnet verbinding maken met een server op de front-end-subnet (maar niet andersom).
- Fail-Safe regel (voor verkeer dat niet voldoet aan een van de vorige):
  7.    Regel voor alle verkeer te weigeren: dit moet altijd de laatste regel (in termen van prioriteit), en als zodanig als netwerkverkeer niet overeenkomt met een van de voorgaande regels genegeerd door deze regel. Dit is een standaardregel en meestal geactiveerd. Er zijn geen wijzigingen nodig zijn in het algemeen.

>[AZURE.TIP] Op de tweede toepassing verkeer regel ter vereenvoudiging van dit voorbeeld wordt is een willekeurige poort toegestaan. In een echte scenario moeten de meest specifieke poort en de adresbereiken worden gebruikt ter vermindering van de kwetsbaarheid van deze regel.

Nadat u alle voorgaande regels hebt gemaakt, is het belangrijk dat u de prioriteit van elke regel zodat het verkeer wordt toegestaan of geweigerd indien gewenst. In dit voorbeeld worden de regels in volgorde van prioriteit.

#### <a name="conclusion"></a>Conclusie
Dit is een meer complexe maar volledige manier beschermen en te isoleren van het netwerk dan de vorige voorbeelden. (Voorbeeld 2 beschermt alleen de toepassing en alleen geïsoleerd subnetten voorbeeld 1). Dit ontwerp zorgt voor het verkeer in beide richtingen, controle en beschermt niet alleen de inkomende application server maar netwerk wordt beveiligingsbeleid afgedwongen voor alle servers in dit netwerk. Ook, afhankelijk van het toestel gebruikt om verkeer volledige controle en kennis kunnen worden bereikt. Zie voor meer informatie de [build met gedetailleerde instructies][Example3]. Deze instructies bevatten:

- Het bouwen van dit voorbeeld perimeternetwerk met PowerShell scripts.
- Het bouwen van dit voorbeeld de sjabloon Azure Resource Manager.
- Gedetailleerde beschrijvingen van elke UDR, NSG opdracht en firewallregel.
- Gedetailleerde verkeer stroom scenario's, hoe verkeer wordt toegestaan of geweigerd in elke laag weergegeven.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-virtual-private-network-vpn"></a>Voorbeeld 4: Een hybride verbinding met een toestel van site naar site, virtuele virtueel particulier netwerk (VPN) toevoegen
[Terug naar Fast start](#fast-start) | Gedetailleerde instructies voor build beschikbaar spoedig

![Perimeternetwerk met NVA verbonden hybride netwerk][11]

#### <a name="environment-description"></a>Beschrijving van de omgeving
Hybride netwerk met behulp van een virtueel netwerkapparaat (NVA) kan worden toegevoegd aan een van de typen perimeternetwerk dat wordt beschreven in de voorbeelden 1, 2 of 3.

Zoals in de voorgaande afbeelding, wordt een VPN-verbinding via het Internet (naar website) verbinding maken met een netwerk van gebouwen met een Azure virtueel netwerk via een NVA gebruikt.

>[AZURE.NOTE] Als u ExpressRoute met Azure openbare Peering is ingeschakeld, moet u een statische route maken. Dit moet NVA VPN-IP-adres van uw Internet-bedrijf en niet via de ExpressRoute WAN-route. Het NAT-apparaat vereist op de optie ExpressRoute Azure openbare Peering kunt de VPN-sessie verbreken.

Nadat de VPN-verbinding aanwezig is, wordt de NVA de centrale hub voor alle netwerken en subnetten. Het doorsturen van firewall-regels bepalen welke verkeersstromen zijn toegestaan, worden omgezet via de NAT, worden omgeleid of worden neergezet (zelfs voor verkeersstromen tussen het netwerk van lokalen en Azure).

Verkeersstromen moeten zorgvuldig worden overwogen als ze kunnen worden geoptimaliseerd of aangetast door dit ontwerppatroon, afhankelijk van de specifieke use-case.

Met behulp van het milieu in voorbeeld 3 gebouwd en vervolgens een netwerkverbinding site naar site VPN-hybride produceert het volgende ontwerp:

![Perimeternetwerk met NVA verbonden via een VPN website][12]

De router op locatie of een ander netwerkapparaat dat compatibel is met de NVA voor VPN, is de VPN-client. Dit apparaat is verantwoordelijk voor het initiëren en onderhouden van de VPN-verbinding met de NVA.

Logisch om de NVA ziet het netwerk er vier afzonderlijke "beveiligingszones" met de regels op de NVA wordt de primaire directeur van verkeer tussen deze zones:

![Logisch netwerk vanuit het oogpunt van de NVA][13]

#### <a name="conclusion"></a>Conclusie
De toevoeging van een site naar site VPN-hybride netwerkverbinding met een Azure virtual network kunt het netwerk op ruimten uitbreiden naar Azure op een veilige manier. Bij het gebruik van een VPN-verbinding het verkeer is gecodeerd en doorgestuurd via het Internet. De NVA in dit voorbeeld biedt een centrale locatie voor het beheren van het beveiligingsbeleid af te dwingen. Raadpleeg de instructies van de gedetailleerde build (bewezen) voor meer informatie. Deze instructies bevatten:

- Het bouwen van dit voorbeeld perimeternetwerk met PowerShell scripts.
- Het bouwen van dit voorbeeld de sjabloon Azure Resource Manager.
- Gedetailleerde verkeer stroom scenario's wordt weergegeven hoe verkeer loopt via dit ontwerp.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn"></a>Voorbeeld 5: Een hybride verbinding met een site naar site, Azure-gateway VPN toevoegen
[Terug naar Fast start](#fast-start) | Gedetailleerde instructies voor build beschikbaar spoedig

![Perimeternetwerk met gateway verbonden hybride netwerk][14]

#### <a name="environment-description"></a>Beschrijving van de omgeving
Hybride netwerk via een VPN-Azure gateway kan worden toegevoegd aan elk type perimeternetwerk dat wordt beschreven in de voorbeelden 1 of 2.

Zoals in de voorgaande afbeelding, wordt een VPN-verbinding via het Internet (naar website) verbinding maken met een netwerk op ruimten een Azure virtueel netwerk via een VPN-Azure gateway gebruikt.

>[AZURE.NOTE] Als u ExpressRoute met Azure openbare Peering is ingeschakeld, moet u een statische route maken. Dit moet NVA VPN-IP-adres van uw Internet-bedrijf en niet via de ExpressRoute WAN-route. Het NAT-apparaat vereist op de optie ExpressRoute Azure openbare Peering kunt de VPN-sessie verbreken.

De volgende afbeelding ziet de twee randen van netwerk in deze optie. Op de eerste rand bepalen de NVA en NSGs verkeersstromen voor intra Azure netwerken en tussen Azure en het Internet. De tweede rand is de Azure VPN-gateway, de rand van een volledig afzonderlijk en geïsoleerd netwerk tussen lokale en Azure.

Verkeersstromen moeten zorgvuldig worden overwogen als ze kunnen worden geoptimaliseerd of aangetast door dit ontwerppatroon, afhankelijk van de specifieke use-case.

Met behulp van het milieu in voorbeeld 1 is gemaakt en vervolgens een netwerkverbinding site naar site VPN-hybride produceert het volgende ontwerp:

![Perimeternetwerk met gateway met elkaar verbonden via een verbinding ExpressRoute][15]

#### <a name="conclusion"></a>Conclusie
De toevoeging van een site naar site VPN-hybride netwerkverbinding met een Azure virtual network kunt het netwerk op ruimten uitbreiden naar Azure op een veilige manier. Met behulp van de oorspronkelijke Azure VPN-gateway, het verkeer is IPSec gecodeerd en routes via het Internet. Met behulp van de Azure VPN-gateway kan ook een optie voor lagere kosten (geen extra licenties kosten zoals bij derden NVAs) bieden. Dit is zo economisch in voorbeeld 1, waar geen NVA wordt gebruikt. Raadpleeg de instructies van de gedetailleerde build (bewezen) voor meer informatie. Deze instructies bevatten:

- Het bouwen van dit voorbeeld perimeternetwerk met PowerShell scripts.
- Het bouwen van dit voorbeeld de sjabloon Azure Resource Manager.
- Gedetailleerde verkeer stroom scenario's wordt weergegeven hoe verkeer loopt via dit ontwerp.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>Voorbeeld 6: Een hybride verbinding met ExpressRoute toevoegen
[Terug naar Fast start](#fast-start) | Gedetailleerde instructies voor build beschikbaar spoedig

![Perimeternetwerk met gateway verbonden hybride netwerk][16]

#### <a name="environment-description"></a>Beschrijving van de omgeving
Hybride netwerk met een ExpressRoute private peering verbinding kan worden toegevoegd aan elk type perimeternetwerk dat wordt beschreven in de voorbeelden 1 of 2.

Zoals in de voorgaande afbeelding, biedt ExpressRoute private peering een directe verbinding tussen uw netwerk op gebouwen en de Azure virtueel netwerk. Verkeer transits alleen de network serviceprovider en het netwerk van Microsoft Azure, nooit aan het Internet.

>[AZURE.NOTE] Er gelden bepaalde beperkingen wanneer UDR met ExpressRoute, vanwege de complexiteit van dynamische routering in de Azure virtuele gateway gebruikt. Deze zijn als volgt:
>
>- UDR moet niet worden toegepast op de gateway-subnet waarop de ExpressRoute gekoppelde Azure virtuele gateway is aangesloten.
>- De ExpressRoute gekoppelde Azure virtuele gateway kan niet afhankelijk is van het apparaat NextHop voor andere UDR subnetten.
>
>
<br />

>[AZURE.TIP] ExpressRoute blijft corporate netwerkverkeer van Internet voor een betere beveiliging en prestaties aanzienlijk verhoogd. Daarnaast kunt u voor serviceovereenkomsten van uw provider ExpressRoute. De Gateway Azure kan maximaal 2 Gb/s met ExpressRoute, doorgeven dat de maximale doorvoer van Azure Gateway met VPN-verbindingen van site naar site, 200 Mb/s.

Zoals in het volgende diagram weergegeven, met deze optie heeft het milieu nu twee randen van het netwerk. De NVA en NSG bepalen verkeersstromen voor intra Azure netwerken en tussen Azure en het Internet, terwijl de gateway een volledig afzonderlijk en geïsoleerd netwerk rand tussen lokale en Azure is.

Verkeersstromen moeten zorgvuldig worden overwogen als ze kunnen worden geoptimaliseerd of aangetast door dit ontwerppatroon, afhankelijk van de specifieke use-case.

Met behulp van het milieu in voorbeeld 1 is gemaakt en vervolgens een netwerkverbinding ExpressRoute hybride produceert het volgende ontwerp:

![Perimeternetwerk met gateway met elkaar verbonden via een verbinding ExpressRoute][17]

#### <a name="conclusion"></a>Conclusie
De toevoeging van een Peering ExpressRoute particuliere netwerkverbinding kunt het netwerk op ruimten uitbreiden naar Azure in een veilige, lagere latentie, hoger wijze uitvoeren. Ook biedt de native Azure Gateway, zoals in dit voorbeeld, een lagere kostprijs-optie (geen extra licenties bij derden NVAs). Raadpleeg de instructies van de gedetailleerde build (bewezen) voor meer informatie. Deze instructies bevatten:

- Het bouwen van dit voorbeeld perimeternetwerk met PowerShell scripts.
- Het bouwen van dit voorbeeld de sjabloon Azure Resource Manager.
- Gedetailleerde verkeer stroom scenario's wordt weergegeven hoe verkeer loopt via dit ontwerp.

## <a name="references"></a>Verwijzingen
### <a name="helpful-websites-and-documentation"></a>Nuttige websites en documentatie
- Access Azure met Azure Resource Manager:
- Azure met PowerShell openen: [https://azure.microsoft.com/documentation/articles/powershell-install-configure/](./powershell-install-configure.md)
- Informatie over virtuele netwerken: [https://azure.microsoft.com/documentation/services/virtual-network/](https://azure.microsoft.com/documentation/services/virtual-network/)
- Network security groep documentatie: [https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/](./virtual-network/virtual-networks-nsg.md)
- Door de gebruiker gedefinieerde routering documentatie: [https://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/](./virtual-network/virtual-networks-udr-overview.md)
- Azure virtual gateways: [https://azure.microsoft.com/documentation/services/vpn-gateway/](https://azure.microsoft.com/documentation/services/vpn-gateway/)
- VPN-verbindingen van site naar Site: [https://azure.microsoft.com/documentation/articles/vpn-gateway-create-site-to-site-rm-powershell](./vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
- ExpressRoute documentatie (Zorg ervoor dat de secties 'Introductie' en 'How To' uitchecken): [https://azure.microsoft.com/documentation/services/expressroute/](https://azure.microsoft.com/documentation/services/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "Stroomdiagram voor beveiliging opties"
[1]: ./media/best-practices-network-security/compliancebadges.png "Azure naleving Badges"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "Azure beveiligingsfuncties"
[3]: ./media/best-practices-network-security/dmzcorporate.png "Een DMZ in een bedrijfsnetwerk."
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "Azure beveiligingsarchitectuur"
[5]: ./media/best-practices-network-security/dmzazure.png "Een DMZ in een virtueel netwerk van Azure"
[6]: ./media/best-practices-network-security/dmzhybrid.png "Hybride netwerk met drie beveiligingsgrenzen"
[7]: ./media/best-practices-network-security/example1design.png "Inkomende DMZ met NSG"
[8]: ./media/best-practices-network-security/example2design.png "Inkomende DMZ met NVA en NSG"
[9]: ./media/best-practices-network-security/example3design.png "Bi-directionele DMZ met NVA, NSG en UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "Logische weergave van de Firewall-regels"
[11]: ./media/best-practices-network-security/example4designoptions.png "DMZ met NVA hybride netwerk verbonden"
[12]: ./media/best-practices-network-security/example4designs2s.png "DMZ met NVA met elkaar verbonden via een VPN website"
[13]: ./media/best-practices-network-security/example4networklogical.png "Logisch netwerk vanuit het oogpunt van de NVA"
[14]: ./media/best-practices-network-security/example5designoptions.png "DMZ met Azure Gateway verbonden-website hybride netwerk"
[15]: ./media/best-practices-network-security/example5designs2s.png "DMZ met Azure met behulp van Site naar Site VPN-Gateway"
[16]: ./media/best-practices-network-security/example6designoptions.png "DMZ met Azure Gateway verbonden ExpressRoute hybride netwerk"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "DMZ met Azure Gateway met een verbinding ExpressRoute"

<!--Link References-->
[Example1]: ./virtual-network/virtual-networks-dmz-nsg-asm.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md

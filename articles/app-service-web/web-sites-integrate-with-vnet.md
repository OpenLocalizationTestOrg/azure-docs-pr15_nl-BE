<properties 
    pageTitle="Een app integreren in een virtueel netwerk van Azure" 
    description="Hoe u verbinding maken met een app in Azure App-Service een nieuwe of bestaande Azure virtual network" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor="cephalin"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="ccompy"/>

# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Uw app integreren in een virtueel netwerk van Azure #

Dit document beschrijft de Azure App virtueel netwerk integratiefunctie en wordt aangegeven hoe instellen met apps in [Azure App-Service](http://go.microsoft.com/fwlink/?LinkId=529714).  Als u niet bekend bent met Azure virtuele netwerken (VNETs), is dit een mogelijkheid waarmee u veel van uw Azure bronnen in een netwerk met internet-routeable die u toegang tot plaatsen.  Deze netwerken kunnen dan worden aangesloten op uw op lokale netwerken met verschillende VPN-technologie.  Voor meer informatie over Azure virtuele netwerken start met de informatie hier: [Azure Virtual Network overzicht][VNETOverview].  

De App Azure Service heeft twee vormen.  

1. De huurder van meerdere systemen die ondersteuning bieden voor het volledige bereik van de prijzen plannen
1. De functie van de App Service omgeving (ASE) premie die in uw VNET.  

Dit document gaat door middel van integratie van VNET en geen App-omgeving.  Als u meer informatie wilt over de functie ASE en start u met de informatie hier: [Inleiding App-omgeving][ASEintro].

VNET integratie geeft uw web app toegang tot bronnen in het virtuele netwerk, maar geen persoonlijke toegang verleent tot uw web app van het virtuele netwerk.  Toegang tot de persoonlijke site is alleen beschikbaar als er een ASE geconfigureerd met een interne Load Balancer (ILB).  Start met het artikel hier voor meer informatie over het gebruik van een ASE ILB: [maken en gebruiken van een ASE ILB][ILBASE]. 

Een algemeen scenario u waar VNET integratie gebruikt is het inschakelen van uw web app toegang heeft tot een database of een webservices op een virtuele machine in uw Azure virtueel netwerk.  Door de integratie van VNET hoeft niet een openbare eindpunt voor toepassingen op uw VM, maar gebruik in plaats daarvan de particuliere routeerbaar internet-adressen worden blootgesteld.  

De functie Integratie met VNET:

- vereist een Standard of Premium prijzen plan 
- werken met Classic(V1) of Resource Manager(V2) VNET 
- TCP als UDP ondersteunt
- werken met het Web, mobiele en API apps
- Hiermee kunt een app verbinding maken met slechts 1 VNET per keer
- Hiermee kunt maximaal 5 VNETs met geïntegreerd worden in een App Service Plan 
- Hiermee kunt u de dezelfde VNET worden gebruikt door meerdere toepassingen in een App Service Plan
- ondersteunt een SLA 99,9% als gevolg van een beroep op de Gateway VNET

Er zijn enkele dingen die integratie van VNET met inbegrip van niet ondersteunt:

- een station koppelen
- AD-integratie 
- NetBios
- toegang tot de persoonlijke site

### <a name="getting-started"></a>Aan de slag ###
Hier volgen enkele dingen rekening moet houden voordat u uw web app verbinding met een virtueel netwerk:

- VNET integratie werkt alleen met apps in een **Standard** of **Premium** prijzen plan.  Als u de functie inschakelt en vervolgens uw serviceplan App naar een niet-ondersteunde prijsstelling plan te schalen verliest uw apps hun verbindingen aan de VNETs die ze gebruiken.  
- Als uw virtuele netwerk al bestaat, moet het point-to-site VPN met een gateway voor dynamische routering ingeschakeld voordat deze kan worden aangesloten op een app hebben.  U kunt point-to-site virtueel particulier netwerk (VPN) niet inschakelen als de gateway is geconfigureerd met statische routering.
- De VNET moet in het abonnement hetzelfde als uw Service App Plan(ASP).  
- De toepassingen die kunnen worden geïntegreerd met een VNET wordt de DNS-server die is opgegeven voor die VNET gebruiken.
- Standaard wordt de geïntegreerde apps alleen verkeer routeren in uw VNET op basis van de routes die zijn gedefinieerd in de VNET.  


## <a name="enabling-vnet-integration"></a>VNET-integratie inschakelen ##

Dit document is primair gericht op de Azure-Portal gebruiken voor de integratie van VNET.  Om te schakelen VNET integratie met uw app met PowerShell, volg de instructies hier: [verbinding maken met uw app tot het virtuele netwerk via PowerShell][IntPowershell].

U hebt de optie verbinding maken met uw app in een nieuwe of bestaande virtueel netwerk.  Als u een nieuw netwerk als onderdeel van uw integratie vervolgens naast het maken van de VNET maken, wordt een dynamische routering gateway vooraf geconfigureerd voor u en wijst u Site VPN wordt ingeschakeld.  

>[AZURE.NOTE] Configureren van een nieuwe virtuele netwerkintegratie kan enkele minuten duren.  

VNET integratie open uw app instellingen en selecteer netwerk.  De gebruikersinterface die wordt geopend, biedt drie netwerken keuzemogelijkheden.  Deze handleiding is alleen gaan naar VNET integratie Hoewel hybride verbindingen en App serviceomgevingen worden verderop in dit document besproken.  

Als uw app is niet in de juiste prijsstelling plan kunt de gebruikersinterface voor het moment u voor het schalen van uw plan om een plan voor hogere prijzen van uw keuze.


![][1]
 
###<a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>VNET integratie met een bestaande VNET inschakelen###
De gebruikersinterface van de integratie van VNET kunt u kiezen uit een lijst van uw VNETs.  Het klassieke VNETs wordt aangegeven dat ze deze met het woord 'Klassieke' tussen haakjes naast de naam van de VNET zijn.  De lijst is gesorteerd dat de Resource Manager VNETs worden als eerste weergegeven.  In de onderstaande afbeelding ziet u dat er slechts één VNET kan worden geselecteerd.  Er zijn meerdere redenen dat een VNET zal worden grijs met inbegrip van:

- de VNET is in een ander abonnement dat je account toegang tot heeft
- de VNET heeft geen punt aan de Site is ingeschakeld
- de VNET beschikt niet over een dynamische routering gateway


![][2]

Als u integratie klikt u op de VNET die u wilt integreren.  Nadat u de VNET, uw app wordt automatisch opnieuw opgestart om de wijzigingen van kracht te laten worden.  

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>Wijs Site in een klassieke VNET inschakelen #####
Als uw VNET geen gateway heeft noch punt naar Site heeft hebt u die eerst instellen.  Voor dit een klassieke VNET, gaat u naar de [Portal Azure] [ AzurePortal] en de lijst met virtuele Networks(classic).  Vanaf hier Klik op het netwerk dat u wilt integreren in en klikt u op het grote vak onder Essentials genaamd VPN-verbindingen.  Hier kunt u het punt om de site VPN en hebben zelfs het maken van een gateway.  Nadat u door het punt naar een site met een gateway maken ervaring worden ongeveer 30 minuten voordat het gereed is.  

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>Wijs Site in een bronnenbeheerder VNET inschakelen #####

Een VNET Resource Manager configureren met een gateway en wijs Site u PowerShell moet zoals beschreven hier, [een punt-naar-Site verbinding configureren met een virtueel netwerk met PowerShell][V2VNETP2S].  De gebruikersinterface voor het uitvoeren van deze functie is nog niet beschikbaar. 

### <a name="creating-a-pre-configured-vnet"></a>Een vooraf geconfigureerde VNET maken ###
Als u een nieuwe VNET die is geconfigureerd met een gateway en punt-naar-Site maakt wilt, is de gebruikersinterface networking App-Service de mogelijkheid om te doen maar alleen voor een resourcemanager VNET.  Als u wenst te maken van een klassieke VNET met een gateway en punt-naar-Site moet u dit handmatig doen via de gebruikersinterface van het netwerk. 

Als u een Resource Manager VNET via de gebruikersinterface van de integratie van VNET, gewoon Selecteer **Nieuw virtueel netwerk maken** en bieden de:

- Virtuele-netwerknaam
- Adresblok virtueel netwerk
- Subnetnaam
- Adresblok subnet
- Adresblok gateway
- Punt-tot-Site Adresblok

Als u wilt dat deze VNET van het andere netwerk verbinding vervolgens geen IP-adresruimte die met deze netwerken overlapt te verzamelen.  

>[AZURE.NOTE] Resource Manager VNET maken met een gateway duurt ongeveer 30 minuten en op dat moment wordt niet integreren in de VNET uw app.  Nadat de VNET is gemaakt met de gateway moet u terugkomen op uw app VNET integratie UI en selecteert u uw nieuwe VNET.

![][3]

Azure VNETs worden normaal gesproken in particulier-netwerkadressen gemaakt.  De integratie van VNET standaard stuurt functie al het verkeer dat bestemd is voor de IP-adresbereiken in uw VNET.  De particuliere IP-adresbereiken zijn:

- -Dit is hetzelfde als 10.0.0.0 - 10.0.0.0/8 10.255.255.255
- 172.16.0.0/12 - dit is hetzelfde als 172.16.0.0 - 172.31.255.255 
- -Dit is hetzelfde als 192.168.0.0 - 192.168.0.0/16 192.168.255.255
 
De VNET-adresruimte moet worden opgegeven in de CIDR-notatie.  Als u niet bekend met CIDR-notatie bent, is een methode voor het opgeven van adresblokken, met behulp van een IP-adres en een geheel getal dat staat voor het netwerkmask. Een snelle verwijzing kunt u dat 10.1.0.0/24 256 adressen en 10.1.0.0/25 128 adressen zou zijn.  Een IPv4-adres met een /32 is nog maar 1 adres.  

Als u de DNS-servergegevens hier vervolgens die wordt ingesteld voor uw VNET.  Na het maken van VNET kunt u deze informatie wordt uit de ervaringen van de gebruiker VNET.

Bij het maken van een klassieke VNET met behulp van de gebruikersinterface van de integratie van VNET, maakt het een VNET in dezelfde bronnengroep als uw app. 

## <a name="how-the-system-works"></a>De werking van het systeem ##
Achter de deze functie is gebaseerd op het Point-to-Site VPN-technologie uw app verbinding met uw VNET.   Apps in Azure App Service hebben een met meerdere huurder systeemarchitectuur die een app rechtstreeks in een VNET ingericht verhindert, zoals dat gebeurt met virtuele machines.  Door te bouwen op een punt-naar-site technologie we netwerktoegang beperken tot alleen de virtuele machine die als host fungeert voor de toepassing.  Toegang tot het netwerk wordt verder beperkt op die hosts app zodat uw apps alleen toegang tot de netwerken waarmee u deze toegang configureren.  

![][4]
 
Als u een DNS-server niet hebt geconfigureerd met het virtuele netwerk moet u IP-adressen te gebruiken.  Houd er rekening mee dat het grote voordeel van deze functie is dat u kunt hiermee de particuliere adressen in uw particuliere netwerk gebruiken tijdens het gebruik van IP-adressen.  Als u uw app Gebruik openbare IP-voor één van de VMs adressen en u de functie Integratie met VNET niet gebruikt en via het internet communiceren.


##<a name="managing-the-vnet-integrations"></a>Beheer van de integratie van VNET##

De mogelijkheid om te maken en verbreken met een VNET is op het niveau van app.  Bewerkingen die betrekking hebben op de integratie van de VNET over meerdere apps zijn op het niveau van ASP.  Uit de gebruikersinterface die wordt weergegeven op het niveau van de app kunt u informatie krijgen over de VNET.  De meeste van dezelfde informatie wordt ook weergegeven op het niveau van ASP.  

![][5]

Op de pagina Netwerkstatus functie kunt u zien of uw toepassing is verbonden met uw VNET.  Als uw gateway VNET is niet beschikbaar voor welke reden dan ook vervolgens dit zou worden weergegeven als niet-verbonden.  

De informatie die u hebt nu beschikbaar in de app die niveau VNET integratie UI is hetzelfde als gedetailleerde informatie u van de ASP-code krijgt.  Hier zijn de artikelen:

- Naam van de VNET - met deze koppeling opent de gebruikersinterface van het netwerk
- Locatie - dit geeft de locatie van uw VNET.  Het is mogelijk om met een VNET op een andere locatie te integreren.
- Certificaatstatus - er zijn certificaten die worden gebruikt voor het beveiligen van de VPN-verbinding tussen de app en de VNET.  Dit weerspiegelt een test, zodat ze synchroon zijn.
- De Status van de gateway - de gateways moeten omlaag om enigerlei reden vervolgens uw app geen toegang tot bronnen in het VNET.  
- VNET-adresruimte - dit is de IP-adresruimte voor uw VNET.  
- Wijs Site-adresruimte - dit is het punt aan de IP-adresruimte site voor uw VNET.  Uw app weergegeven communicatie die afkomstig is van een van de IP-adressen in deze adresruimte.  
- Site op site-adresruimte - Site naar Site VPN-verbindingen kunt u verbinding maken met uw VNET op lokale resources of aan andere VNETs.  U hebt die geconfigureerd en vervolgens de IP-adresbereiken gedefinieerd met behulp van VPN-verbinding hier ziet.
- DNS-Servers - als u beschikt over DNS-Servers die zijn geconfigureerd met de VNET en vervolgens worden deze hier weergegeven.
- IPs doorgestuurd naar de VNET - er wordt een lijst met IP-adressen dat uw VNET gedefinieerd heeft voor routering.  Deze adressen worden hier weergegeven.  

De enige bewerking die van de app weergave van de integratie van uw VNET kunt u uw is uw app verbreken in de VNET momenteel met verbonden is.  U doet dit klikt u op verbinding verbreken boven.  Deze actie wordt de VNET niet gewijzigd.  De VNET en de configuratie, met inbegrip van de gateways blijft ongewijzigd.  Als u vervolgens wilt verwijderen van uw VNET moet u eerst de bronnen in deze inclusief gateways verwijderen.  

De weergave plannen van App-Service heeft een aantal extra bewerkingen.  Het wordt ook anders benaderd dan vanuit de app.  Voor het bereiken van openen de ASP-Networking UI gewoon uw ASP-UI en scroll naar beneden.  Er is een gebruikersinterface-element netwerkstatus functie aangeroepen.  Enkele kleine details rond de integratie van uw VNET krijgt.  Op deze gebruikersinterface te klikken opent de functie Status gebruikersinterface voor het netwerk.  Als u op 'Klik hier klikt voor het beheren van' opent u de gebruikersinterface waarin de integratie van VNET in deze ASP.

![][6]

De locatie van het ASP is goed om te onthouden bij het onderzoeken van de locaties van de VNETs u met integreert.  Wanneer de VNET op een andere locatie is bent u waarschijnlijk veel meer Zie latentieproblemen met.  

De VNETs is geïntegreerd met een herinnering op het aantal VNETs in uw toepassingen zijn geïntegreerd in deze ASP- en hoeveel u kunt is.  

Om extra details op elke VNET ziet, klik op de VNET waarin u geïnteresseerd bent.  Naast de informatie die eerder is al opgemerkt dat ook ziet u een lijst van de toepassingen die van de VNET die gebruikmaken in deze ASP.  

Er zijn twee primaire acties met betrekking tot acties.  De eerste is de mogelijkheid om de routes die verkeer dat vanaf uw app in uw VNET toevoegen.  De tweede actie is de mogelijkheid om te synchroniseren van certificaten en netwerkgegevens.

![][7]

**Routering** Zoals eerder opgemerkt zijn de routes die zijn gedefinieerd in de VNET wat wordt gebruikt voor het routeren van verkeer in de VNET van uw app.  Er zijn enkele toepassingen waarin klanten wilt versturen meer uitgaand verkeer vanuit een app in de VNET en ze deze mogelijkheid wordt geleverd.  Wat gebeurt er met het verkeer nadat die is aan hoe de klant wordt geconfigureerd met hun VNET.  

**Certificaten** De Status van het certificaat komt overeen met een controle wordt uitgevoerd door de App Service om te valideren dat de certificaten die we voor de VPN-verbinding nog steeds goed zijn.  VNET-integratie is ingeschakeld, als dit de eerste integratie op die VNET van alle toepassingen in deze ASP is een vereiste uitwisseling van certificaten om ervoor te zorgen de beveiliging van de verbinding.  Samen met de certificaten krijgen we de DNS-configuratie, routes en andere soortgelijke zaken over het netwerk.
Als deze certificaten of netwerkinformatie wordt gewijzigd moet u Klik op 'Netwerk synchroniseren'.  **Opmerking**: wanneer u klikt op 'Sync netwerk' en vervolgens wordt er een korte stroomstoring in verbinding tussen uw app en uw VNET.  Terwijl u uw app niet opnieuw gestart het verlies van connectiviteit kan ervoor zorgen dat uw site niet correct.  

##<a name="accessing-on-premise-resources"></a>Toegang tot lokale bronnen##

Een van de voordelen van de functie Integratie met VNET is als de VNET is verbonden met uw op lokale netwerk met een VPN van Site tot Site en vervolgens de apps toegang tot lokale resources op van uw app hebben kunnen.  Dit werkt, maar moet u uw on premise VPN-gateway bijwerken met de routes voor de IP-Site variëren.  Wanneer de VPN-verbinding van Site naar Site eerst is ingesteld moeten de scripts gebruikt voor het configureren van het instellen van routes, met inbegrip van het punt op de VPN-Site.  Als u het punt toevoegen aan de Site VPN na het maken van uw Site naar Site VPN, hoeft u de routes handmatig bijwerken.  Meer informatie over hoe u dat kunt doen verschilt per gateway en worden hier niet beschreven.  

>[AZURE.NOTE] Terwijl de functie Integratie met VNET werkt met een Site naar Site VPN toegang krijgen tot lokale bronnen van het momenteel niet geschikt is voor een VPN ExpressRoute hetzelfde te doen.  Dit geldt bij het integreren met een klassiek of een resourcemanager VNET.  Als u toegang tot bronnen via een VPN ExpressRoute kunt u een ASE die in uw VNET kunt uitvoeren. 

##<a name="pricing-details"></a>Prijsdetails##
Er zijn enkele nuances bevatten die u moet zich bewust zijn van wanneer u de functie Integratie met VNET prijzen.  Er zijn 3 gerelateerde kosten voor het gebruik van deze functie:

- ASP prijzen laag eisen
- Kosten van de gegevensoverdracht
- VPN-Gateway-kosten.

Voor uw toepassingen kunnen deze functie wilt gebruiken, moeten ze worden in een Standard of Premium App Service Plan.  Kunt u meer details op deze kosten hier bekijken: [App serviceprijzen][ASPricing]. 

Door de wijze waarop punt tot de Site VPN-verbindingen worden verwerkt, u hebt altijd een toeslag voor uitgaande gegevens via de verbinding VNET integratie zelfs als de VNET in het midden met dezelfde gegevens.  Om te zien wat deze toeslagen zijn Kijk hier: [Data Transfer prijzen Details][DataPricing].  

Het laatste item is de kostprijs van de VNET-gateways.  Als u geen gateways voor iets anders, zoals VPN's van Site naar Site vervolgens betaalt u voor gateways ter ondersteuning van de functie Integratie met VNET.  Er zijn details over deze kosten hier: [VPN-Gateway prijzen][VNETPricing].  

##<a name="troubleshooting"></a>Het oplossen van problemen##

Dit wil niet zeggen dat je ervaring probleem vrij zal zijn wanneer de functie eenvoudig is te installeren.  U zijn problemen met het openen van uw gewenste eindpunt er sommige hulpprogramma's die kunt u verbindingen vanaf de console app testen.  Er zijn twee console ervaringen die u kunt gebruiken.  Een van de Kudu-console is en de andere is de console die u in de Portal Azure bereiken kunt.  Als u de console Kudu vanuit uw app naar Extra -> Kudu.  Dit is hetzelfde als [sitenaam]. scm.azurewebsites.net.  Zodra die gewoon geopend, gaat u naar het tabblad foutopsporing console.  Als u naar de Azure portal gehoste console en vervolgens vanuit de app naar Extra -> Console.  


####<a name="tools"></a>Hulpprogramma 's####

De hulpprogramma's ping, nslookup en tracert werkt niet via de console vanwege beveiligingsbeperkingen.  De void er zijn twee afzonderlijke hulpprogramma's om op te vullen.  Een hulpprogramma met de naam nameresolver.exe toegevoegd om de DNS-functionaliteit te testen.  De syntaxis is:

    nameresolver.exe hostname [optional: DNS Server]

U kunt nameresolver controleren de hostnamen die afhankelijk zijn van uw app.  Op deze manier kunt u als u iets verkeerd geconfigureerd met DNS of misschien geen toegang tot de DNS-server hebben testen.

De volgende tool kunt u testen op TCP-verbinding met een host en poort-combinatie.  Dit programma heet tcpping.exe en de syntaxis is:

    tcpping.exe hostname [optional: port]

Deze tool vertelt u als u een specifieke host en poort kunt bereiken, maar niet dat u met het ICMP dezelfde taak wordt uitgevoerd op basis van hulpprogramma ping.  De opdracht ping ICMP ziet u als de host is.  Met tcpping weten u als u toegang hebt tot een specifieke poort op een host.  


####<a name="debugging-access-to-vnet-hosted-resources"></a>Toegang tot VNET voor foutopsporing gehost resources####

Er zijn een aantal dingen die u kunt voorkomen dat uw app tot een specifieke host en poort.  De meeste gevallen is het een van de drie dingen:

- **Er is een firewall in de manier waarop**  Als u een firewall op de manier wordt u de time-out voor TCP-hits.  Dat is in dit geval 21 seconden.  Het hulpprogramma tcpping gebruiken om te controleren.  TCP-time-outs kunnen worden door veel dingen voorbij firewalls maar er.  
- **DNS is niet toegankelijk**  De DNS-time-out is 3 seconden per DNS-server.  Als er 2 DNS-servers is dat 6 seconden.  Gebruik nameresolver om te controleren of DNS werkt.  Denk eraan dat u niet nslookup gebruiken zoals die maakt geen gebruik van de DNS-server die is geconfigureerd met de VNET.
- **Ongeldige P2S IP-bereik** Het punt naar site IP-adresbereik moet in de particuliere IP-adresbereiken van RFC 1918 (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255) als het bereik IP-adressen buiten die gebruikt vervolgens dingen niet werken.  

Als deze artikelen geen antwoord op je probleem, er eerst voor de eenvoudige zaken:  

- Is de Gateway als u in de Portal weergeven?
- Worden certificaten weergegeven als de wordt gesynchroniseerd?
- Iedereen de netwerkconfiguratie wijzigen zonder een 'Sync-netwerk' in de desbetreffende ASP's? 

Als uw gateway omlaag brengt u deze back-up.  Als uw certificaten niet synchroon zijn gaat u naar de ASP-weergave van de integratie van uw VNET en Raak 'Netwerk synchroniseren'.  Als u vermoedt dat er een wijziging aangebracht in de configuratie van de VNET is en het synchroniseren is niet zou met uw ASP's vervolgens gaat u naar de ASP-weergave van de integratie van VNET en klik op 'Netwerk synchroniseren' net zoals een herinnering, hierdoor wordt een korte stroomstoring met de verbinding van uw VNET en uw apps.  

Als dat is prima moet u een beetje dieper graven:

- Zijn er andere apps VNET integratie van bronnen in de dezelfde VNET te bereiken? 
- Kunt u Ga naar de console app en tcpping gebruiken voor communicatie met andere bronnen in uw VNET?  

Als een van de bovenstaande voorwaarden voldaan wordt is de integratie van de VNET fijn en het probleem ergens anders is.  Dit is waar wordt meer van een uitdaging zijn, omdat er geen eenvoudige manier om te zien waarom u een host: poort niet bereiken.  Enkele van de oorzaken zijn:

- u hebt een firewall van op de host die toegang tot de poort van de toepassing van uw site IP-bereik, wijs voorkomen.  Kruisende subnetten vaak vereist toegang van het publiek.
- de doelhost is niet beschikbaar
- uw toepassing niet actief is.
- u hebt het verkeerde IP of hostnaam
- uw toepassing luistert op een andere poort dan u had verwacht.  U kunt dit controleren door te gaan naar die host en 'netstat - aon' met behulp van de cmd-prompt.  Hier ziet u welk proces ID op welke poort luistert.  
- uw netwerk beveiligingsgroepen zijn geconfigureerd op zodanige wijze dat toegang tot uw toepassinghost en poort van uw site IP-bereik, wijs

Weet u welke IP niet weet uw punt naar Site IP-adresbereik dat uw app zodat u de toegang van het hele bereik moet worden gebruikt.  

Extra stappen zijn:

- aanmelden bij een andere VM in uw VNET en probeert te krijgen tot de bron-host: poort vanaf daar.  Er zijn bepaalde TCP-ping hulpprogramma's u kunt gebruiken voor dit doel of zelfs telnet kunt gebruiken als moeten worden.  Het doel hier is gewoon om te bepalen of connectiviteit er van deze andere VM is. 
- een toepassing op een andere VM en test toegang op die host en poort openen vanuit de console van uw app  
####<a name="on-premise-resources"></a>Lokale bronnen####
Als uw bronnen op de lokale niet bereiken, dan is het eerste wat u moet controleren als u een bron in uw VNET kunt bereiken.  Als dat werkt zijn de volgende stappen zijn heel gemakkelijk.  U moet proberen te bereiken van de in toepassing van de vooronderstelling uit een VM in uw VNET.  U kunt telnet of een TCP-hulpprogramma ping.  Als uw VM kan niet uw op lokale resource bereiken en vervolgens u eerst controleert werkt uw VPN-verbinding van Site naar Site.  Werken deze vervolgens dezelfde mogelijkheden als hierboven is aangegeven en de configuratie van de lokale standaardgateway en de status te controleren.  

Nu als host voor uw VNET VM kunt bereiken uw op lokaal systeem maar de app kan niet de oorzaak is waarschijnlijk een van de volgende:
- de routes zijn niet geconfigureerd met de IP-adresbereiken in lokale gateway op site, wijs
- de beveiligingsgroepen van uw netwerk blokkeert toegang voor uw Site IP-bereik, wijs
- uw on premise firewalls blokkeren verkeer van uw Site IP-bereik
- u hebt een Route(UDR) gebruiker gedefinieerd in uw VNET dat verhindert dat uw Site gebaseerd verkeer uw on premise netwerk bereiken

## <a name="hybrid-connections-and-app-service-environments"></a>Hybride verbindingen en App Service-omgevingen##
Er zijn 3 functies die toegang tot bronnen die worden gehost VNET.  Deze zijn:

- VNET integratie
- Hybride verbindingen
- De Service App omgevingen

Hybride verbindingen moet u een relay-agent de hybride verbinding Manager(HCM) genoemd in het netwerk installeren.  De HCM moet verbinding maken met Azure en ook met uw toepassing.  Deze oplossing is met name ideaal van een extern netwerk, zoals het op lokale netwerk of zelfs een andere cloud gehost netwerk omdat zonder van een internet toegankelijk eindpunt tussenkomst.  De HCM kan alleen worden uitgevoerd op Windows en u kunt maximaal 5 exemplaren die worden uitgevoerd voor maximale beschikbaarheid.  Hybride verbindingen ondersteunt alleen TCP via en elk eindpunt HC moet overeenkomen met een specifieke host: poort-combinatie.  

De functie App-omgeving kunt u een exemplaar van de App Azure Service uitvoeren in uw VNET.  Hiermee kunt uw apps toegang tot bronnen in uw VNET zonder extra maatregelen.  Enkele voordelen van een App-omgeving is die u kunt gebruiken, 8 core gericht werknemers met 14 GB RAM.  Een ander voordeel is dat u het systeem aan uw behoeften kunt schalen.  In tegenstelling tot de huurder met meerdere omgevingen waar de ASP een omvang beperkte is, in een ASE bepalen u hoeveel resources u wilt geven aan het systeem.  Met betrekking tot het netwerk de focus van dit document is een van de dingen die u met een ASE u niet door de integratie van VNET echter dat deze met een VPN ExpressRoute werken kan.  

Hoewel er dat enkele kleine overlapping gebruiken, kan geen van deze functie een van de andere te vervangen.  Weten welke functie gebruiken is gebonden aan uw behoeften en hoe is het verstandig om het te gebruiken.  Bijvoorbeeld:

- Als u een ontwikkelaar bent en gewoon wilt uitvoeren van een site in Azure en hebben zij toegang tot de database op het werkstation onder uw bureau is het eenvoudigste wat gebruiken hybride verbindingen.  
- Als u een grote organisatie die wil een groot aantal eigenschappen van het web in de publieke cloud en beheren in uw eigen netwerk en u wilt gaan met de App-omgeving.  
- Als u beschikt over App Service apps gehost en gewoon wilt toegang tot bronnen in uw VNET en vervolgens VNET integratie is de manier om te gaan.  

Buiten de use-cases er zijn sommige eenvoud gerelateerde aspecten.  Als uw VNET al is verbonden met het op lokaal netwerk met behulp van integratie met VNET of een App-omgeving een eenvoudige manier is om lokale bronnen verbruiken.  Aan de andere kant, als uw VNET niet met het op lokale netwerk verbonden is is veel meer overhead voor het instellen van een VPN van site naar site met uw VNET in vergelijking met het installeren van de HCM.  

Naast de functionele verschillen er zijn ook prijzen verschillen.  De functie App-omgeving is een Premium service aangeboden maar biedt de meest netwerk configuratiemogelijkheden naast andere geweldige functies.  VNET-integratie kan worden gebruikt met de Standard of Premium ASP's en is ideaal voor het verbruik van bronnen in de VNET van de meerdere huurder App Service veilig.  Hybride verbindingen hangt momenteel een BizTalk account met niveaus die gratis starten en vervolgens geleidelijk duurder prijzen op basis van het bedrag dat u nodig hebt.  Wanneer het gaat om al in veel netwerken werken, kan niet worden andere zoals hybride verbindingen waarmee u toegang krijgt tot bronnen in afzonderlijke netwerken ook meer dan 100.    


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[IntPowershell]: http://azure.microsoft.com/documentation/articles/app-service-vnet-integration-powershell/
[ASEintro]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/

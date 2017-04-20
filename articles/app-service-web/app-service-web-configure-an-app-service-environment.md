<properties
    pageTitle="Het configureren van een App-omgeving | Microsoft Azure"
    description="Configuratie, beheer en controle van de milieus voor App-Service"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>


# <a name="configuring-an-app-service-environment"></a>Een omgeving van App-Service configureren #

## <a name="overview"></a>Overzicht ##

Op een hoog niveau bestaat een Azure App-omgeving uit verschillende hoofdonderdelen:

- Bronnen die actief zijn in de App-omgeving gehoste service berekenen
- Opslag
- Een database
- Een virtueel netwerk Classic(V1) of Resource Manager(V2) Azure (VNet) 
- Een subnet met de App-omgeving gehoste service wordt uitgevoerd in het

### <a name="compute-resources"></a>Resources te berekenen

U gebruikt de compute-bronnen voor uw vier resourcegroepen.  Elke App Service omgeving (ASE) heeft een set front-ends en groepen van drie mogelijke werknemer. U hoeft niet te worden alle groepen van drie werknemer--gebruiken als u wilt, kunt u gewoon gebruiken één of twee.

De hosts in de resourcegroepen (front-ends en werknemers) zijn niet rechtstreeks toegankelijk voor huurders. U kan Remote Desktop Protocol (RDP) gebruiken om te verbinden met hen, hun aanbod wijzigen of fungeren als beheerder op deze.

U kunt de resource pool hoeveelheid en de grootte instellen. In een ASE hebt u vier opties, die P1 via P4 worden aangeduid. Zie de [serviceprijzen App](../app-service/app-service-value-prop-what-is.md)voor meer informatie over deze formaten en hun prijzen.
De hoeveelheid of het formaat wijzigen van een schaal-bewerking genoemd.  Slechts één schaal bewerking kan worden uitgevoerd op een tijdstip.

**Voorkant eindigt**: de front-ends worden de HTTP/HTTPS-eindpunten voor uw toepassingen die worden vastgehouden in de ASE. U uitvoeren werkbelasting niet in de front-ends.

- Een ASE begint met twee P2s, die voldoende is voor dev/test werkbelasting en de werkbelasting van de productie op een laag niveau. Wij raden P3s voor normaal tot productie van zware werklasten.
- Voor normaal tot productie van zware werklast, is het raadzaam dat u ten minste vier P3s hebt om ervoor te zorgen er voldoende-front-ends gepland onderhoud plaatsvindt. Gepland onderhoudsactiviteiten brengt u een front-end tegelijk. Hierdoor wordt over het algemeen beschikbare capaciteit van front-end tijdens onderhoudsactiviteiten.
- U toevoegen niet meteen een nieuwe front-instantie. Tussen 2 en 3 uur naar de voorziening kunnen nemen.
- Verdere schaal nauwkeurig instellen, moet u de CPU-percentage, percentage geheugen en actieve aanvragen maatstaven voor de front-end-toepassingen controleren. Als de percentages CPU of geheugen boven 70 procent wanneer P3s wordt uitgevoerd, toevoegen meer front-ends. Als het gemiddelde van de waarde van de actieve aanvragen uit naar 15.000 tot 20.000 aanvragen per-front-end, kunt u ook meer front-ends toevoegen. De algemene doelstelling is om onderstaande percentages van CPU en geheugen 70% en actieve aanvragen gemiddeld tot onder 15.000 aanvragen per voorkant beëindigen wanneer u P3s.  

**Werknemers**: de werknemers worden waar je apps daadwerkelijk uitvoeren. Als u van uw App Service plannen schalen, die gebruikmaakt van werknemers in de groep met gekoppelde werknemer.

- U kunt geen werknemers direct toevoegen. Zij zijn 2 tot 3 uur naar de voorziening kan nemen, ongeacht hoeveel worden toegevoegd.
- Schalen het formaat van een resource voor alle toepassingen compute duurt 2-3 uur per domein bijwerken. Er zijn 20 update domeinen in een ASE. Als u de grootte berekenen van een werknemer met 10 exemplaren van toepassingen geschaald, kan dit tussen 20 tot 30 uur in beslag nemen.
- Als u de grootte van de compute-bronnen die worden gebruikt in een groep met werknemer wijzigt, wordt u koude start van de toepassingen die worden uitgevoerd in die groep werknemers.

De snelste manier om de compute resource grootte wijzigen van een werknemer van toepassingen die niet alle apps actief is:

- Verminder de telling van het exemplaar op 0. Het duurt ongeveer 30 minuten uw sessies de toewijzing.
- Selecteer de nieuwe compute grootte en aantal exemplaren. Vanaf hier duurt het tussen de 2 en 3 uur in beslag.

Als uw toepassingen vereist compute resource groter is, kan niet u profiteren van de eerdere richtlijnen. U kunt in plaats van het wijzigen van de grootte van de werknemer van toepassingen die als host voor deze apps fungeert, een andere werknemer toepassingen met werknemers van de gewenste grootte te vullen en uw apps naar die groep verplaatsen.

- De extra exemplaren van de grootte van de benodigde berekenen in een andere werknemer toepassingen maken. Dit zal duren van 2 tot 3 uur.
- Uw App Service plannen waarop de toepassingen die een groter aan de groep nieuw-geconfigureerd werknemer moeten toewijzen. Dit is een snelle bewerking moet minder dan een minuut in beslag nemen.  
- De eerste werknemer pool verkleinen als u niet meer nodig hebt die ongebruikte exemplaren. Deze bewerking duurt ongeveer 30 minuten te voltooien.

**AutoScaling**: een van de hulpprogramma's die u voor het beheren van uw verbruik berekenen autoscaling is. U kunt autoscaling voor front- of groepen van werknemers. U kunt handelingen bijvoorbeeld verhoging uw sessies van elk type van toepassingen in de ochtend en 's avonds te verkleinen. Of misschien kunt u exemplaren toevoegen wanneer het aantal werknemers die beschikbaar in een groep werknemers zijn zakt tot onder een bepaalde drempel.

Als u wilt dat automatisch schalen regels rond compute resource pool parameters in te stellen, vervolgens Houd rekening met de tijd die vereist ingericht is. Zie voor meer informatie over autoscaling App Service-omgevingen, [automatisch schalen in een App-omgeving configureren][ASEAutoscale].

### <a name="storage"></a>Opslag

Elke ASE is geconfigureerd met 500 GB opslagruimte. Deze ruimte wordt gebruikt voor alle toepassingen in de ASE. Deze opslagruimte is een onderdeel van de ASE en op dit moment niet worden omgezet voor het gebruik van uw opslagruimte. Als u correcties aan uw virtuele netwerkroutering of beveiliging aanbrengen wilt, moet u nog steeds toegang tot opslag Azure--toestaan of de ASE niet functioneren.

### <a name="database"></a>Database

De database bevat de informatie die het milieu, alsmede de gegevens over de toepassingen die worden uitgevoerd binnen het definieert. Dit is een onderdeel van het abonnement Azure gehouden. Het is niet iets wat u hebt een directe mogelijkheid om te bewerken. Als u correcties aan uw virtuele netwerkroutering of beveiliging aanbrengen wilt, moet u nog steeds toegang bieden tot SQL Azure-- of de ASE niet functioneren.

### <a name="network"></a>Netwerk

De VNet die wordt gebruikt met de ASE is één die u hebt aangebracht toen u de ASE gemaakt of die u vooraf hebt aangebracht. Wanneer u het subnet tijdens het maken van ASE maken, u gedwongen wordt de ASE in dezelfde bronnengroep als het virtuele netwerk. Als u de resourcegroep die door uw ASE anders is dan die van de VNet gebruikt, moet u uw ASE met behulp van een resource manager-sjabloon maken.

Er zijn enkele beperkingen voor het virtuele netwerk dat wordt gebruikt voor een ASE:
 
- Het virtuele netwerk moet een regionale virtueel netwerk.
- Er moet een subnet met 8 of meer adressen waar de ASE is geïmplementeerd.
- Nadat u een subnet wordt gebruikt voor het hosten van een ASE, kan niet het adresbereik van het subnet worden gewijzigd. Om deze reden is het raadzaam dat het subnet bevat ten minste 64-adressen ten behoeve van de toekomstige groei van ASE.
- Kan er niets in het subnet maar de ASE.

In tegenstelling tot de gehoste service de ASE, het [virtuele netwerk] met[ virtualnetwork] en subnet zijn onder gebruikersbesturing.  U kunt het virtuele netwerk via de gebruikersinterface van het virtuele netwerk of via PowerShell beheren.  Een ASE kan worden geïmplementeerd in een klassieke of Resource Manager VNet.  De portal en API ervaringen verschillen enigszins tussen klassieke en Resource Manager VNets, maar de ASE gebruikersbeleving is hetzelfde.

De VNet die wordt gebruikt voor het hosten van een ASE een particuliere RFC1918 IP-adressen kunt gebruiken of het openbare IP-adressen kunt gebruiken.  Als u wilt gebruiken een IP-adresbereik dat niet wordt gedekt door RFC1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) moet u uw VNet en het subnet worden gebruikt door uw ASE voor ASE maken op maken.

Omdat deze mogelijkheid de Azure App-Service in het virtuele netwerk plaatst, betekent dit dat uw toepassingen die worden gehost in uw ASE nu toegang hebben tot bronnen die beschikbaar zijn via ExpressRoute of van site naar site virtuele particuliere netwerken (VPN's) rechtstreeks. De apps die binnen uw omgeving App-Service vereist geen aanvullende netwerkfuncties toegang krijgen tot bronnen die beschikbaar zijn voor het virtuele netwerk die als host voor uw App-omgeving fungeert. Dit betekent dat u niet moet met VNET integratie of hybride verbindingen toegang krijgen tot bronnen in of verbonden zijn met het virtuele netwerk. Kunt u beide van die functies al voor toegang tot bronnen in netwerken die niet zijn met het virtuele netwerk verbonden.

U kunt bijvoorbeeld VNET integratie te integreren in een virtueel netwerk dat is in uw abonnement, maar niet is gekoppeld aan het virtuele netwerk waarmee uw ASE is in. U kunt ook nog steeds hybride verbindingen gebruiken voor toegang tot bronnen in andere netwerken, zoals gewoonlijk.  

Als u het virtuele netwerk is geconfigureerd met een VPN ExpressRoute hebt, moet u zich bewust zijn van een aantal van de routeringstabel behoeften met een ASE. Er zijn enkele door de gebruiker gedefinieerde route (UDR)-configuraties die niet compatibel met een ASE zijn. Voor meer details over het uitvoeren van een ASE in een virtueel netwerk met ExpressRoute, [uitvoeren van een App-omgeving in een virtueel netwerk met ExpressRoute][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Inkomend verkeer beveiligen

Er zijn twee primaire methoden voor het beheren van binnenkomend verkeer op de ASE.  U kunt netwerk beveiliging Groups(NSGs) gebruiken om te bepalen welke IP adressen toegang heeft tot uw ASE zoals hier wordt beschreven [hoe binnenkomende verkeer in een App-omgeving](app-service-app-service-environment-control-inbound-traffic.md) en u kunt ook uw ASE configureren met een interne Balancer(ILB) van de belasting.  Deze functies kunnen ook samen worden gebruikt als u toegang wilt beperken met behulp van NSGs naar uw ILB ASE.

Wanneer u een ASE maakt, maakt het een VIP in uw VNet.  Er zijn twee VIP typen externe en interne.  Wanneer u een ASE met een externe VIP maakt vervolgens zijn uw apps in uw ASE toegankelijk via een routeerbaar internet IP-adres. Wanneer u uw ASE interne selecteert worden geconfigureerd met een ILB en niet rechtstreeks toegankelijk internet.  Een ASE ILB nog steeds een externe VIP vereist, maar het wordt alleen gebruikt voor toegang tot de Azure beheer en onderhoud.  

Tijdens het maken van het ILB ASE bieden het subdomein gebruikt door het ILB ASE en hebben voor het beheren van DNS voor het subdomein dat u opgeeft.  Omdat u de naam van het subdomein ingesteld moet u ook het certificaat gebruikt voor HTTPS-toegang beheren.  Na het maken van ASE wordt u gevraagd om het certificaat.  Voor meer informatie over het maken en gebruiken van een ASE ILB lezen [met behulp van een interne taakverdeling met een App-omgeving][ILBASE]. 


## <a name="portal"></a>Portal

U kunt beheren en controleren van uw App-omgeving met behulp van de gebruikersinterface in de portal Azure. Als er een ASE, vervolgens bent u waarschijnlijk het symbool App-Service op de zijbalk. Dit symbool wordt gebruikt voor App serviceomgevingen in Azure portal:

![Symbool voor App-omgeving][1]

U opent de gebruikersinterface waarin u alle van uw App Service-omgevingen, kunt u het pictogram of de dubbele punthaken selecteren (">" symbool) onder aan de zijbalk App serviceomgevingen te selecteren. Als u een van de genoemde ASEs, opent u de gebruikersinterface die wordt gebruikt om te controleren en deze te beheren.

![De gebruikersinterface voor het controleren en beheren van uw App-omgeving][2]

Deze eerste blade bevat enkele eigenschappen van de ASE, met een metrische grafiek per resourcegroep. Sommige van de eigenschappen die worden weergegeven in het blok **Essentials** zijn ook hyperlinks die de bladeserver die is gekoppeld aan het wordt geopend. Bijvoorbeeld, kunt u de **Virtuele** -netwerknaam voor het openen van de gebruikersinterface die is gekoppeld aan het virtuele netwerk die wordt uitgevoerd in de ASE. **App serviceplannen** en **Apps** openen blades waarbij deze items in uw ASE.  

### <a name="monitoring"></a>Monitoring

De grafieken kunnen u tal van prestatiegegevens in elke groep. U kunt de gemiddelde CPU- en geheugengebruik controleren voor de front-end-toepassingen. U kunt controleren de hoeveelheid die is gebruikt en de hoeveelheid die beschikbaar is voor groepen van de werknemer.

Meerdere App Service plannen kunnen maken gebruik van de werknemers in een groep werknemers. De werkbelasting wordt niet gedistribueerd op dezelfde manier als met de front-end-servers, zodat de CPU- en geheugengebruik niet veel leveren heeft nuttige informatie op het gebied. Het is belangrijker om bij te houden hoeveel werknemers die u hebt gebruikt en zijn beschikbaar, vooral als u bij het beheren van dit systeem voor andere gebruikers.  

Ook kunt u alle van de criteria die kunnen worden getraceerd in de grafieken voor het instellen van waarschuwingen. Instellen van waarschuwingen hier werkt hetzelfde als elders in de App-Service. U kunt een waarschuwing instellen uit het **waarschuwingen** UI gedeelte of boren in parameters UI en **Waarschuwing toevoegen**te selecteren.

![UI-parameters][3]

De cijfers die zojuist zijn beschreven, zijn cijfers over de App-omgeving. Er zijn ook cijfers die beschikbaar op het niveau van het plan App-Service zijn. Dit is waar controleren CPU en geheugen maakt veel zin.

In een ASE zijn de App Service plannen speciale App serviceplannen. Dat houdt in dat de enige apps die worden uitgevoerd op de hosts die zijn toegewezen aan dat App serviceplan zijn de apps die App serviceplan. Als details wilt bekijken over uw serviceplan App, geven uw serviceplan App vanaf een van de lijsten in de gebruikersinterface van ASE of **Bladeren App serviceplannen** (die al deze lijsten).   

### <a name="settings"></a>Instellingen

Binnen de blade ASE is er een sectie met **Instellingen** die verschillende belangrijke mogelijkheden bevat:

**Instellingen** > **Eigenschappen**: de blade **Instellingen** automatisch geopend wanneer u uw blade ASE weer. Aan de bovenkant is **Eigenschappen**. Er zijn een aantal artikelen hier op wat u in de **Essentials ziet**redundante, maar wat is zeer nuttig is **Virtueel IP-adres**als **Uitgaande IP-adressen**.

![Eigenschappen en instellingen voor blade][4]

**Instellingen** > **IP-adressen**: als u een IP-Secure Sockets Layer (SSL)-app in de ASE maakt, moet u een SSL IP-adres. Voor het verkrijgen van een moet de ASE SSL IP adressen waarvan deze eigenaar is, dat kunnen worden verdeeld. Bij het maken van een ASE heeft één SSL IP-adres voor dit doel, maar u kunt meer toevoegen. Er is een toeslag voor extra IP SSL-adressen, zoals in [App serviceprijzen] [ AppServicePricing] (in de sectie op SSL-verbindingen). De extra prijs is de prijs IP SSL.

**Instellingen** > **Front End-toepassingen** / **Werknemer Pools**: elk van deze resource pool blades biedt de mogelijkheid om informatie te zien alleen op die groep, naast de besturingselementen volledig schalen die resourcegroep.  

De basis blade voor elke groep wordt een grafiek met maatstaven voor die groep. Net als met de grafieken van de bladeserver ASE kunt u gaan in de grafiek en waarschuwingen naar wens instellen. Een waarschuwing instellen vanaf de ASE-blade voor een bepaalde resourcegroep doet hetzelfde als het doen van de resourcegroep. Van de werknemer groep **Instellingen** blade, hebt u toegang tot alle Apps of App plannen waarop deze werknemer van toepassingen.

![Werknemer de instellingen voor de gebruikersinterface][5]

### <a name="portal-scale-capabilities"></a>Portal schaal mogelijkheden  

Er zijn drie schalen bewerkingen:

- Het aantal IP-adressen in de ASE die beschikbaar voor gebruik van SSL IP zijn wijzigen.
- De grootte wijzigen van de compute-bron die wordt gebruikt in een resourcegroep.
- Het aantal compute resources die worden gebruikt in een resourcegroep handmatig of via autoscaling wijzigen.

Er zijn drie manieren om te bepalen hoeveel servers die u in uw resourcegroepen hebt in de portal:

- Een bewerking van de schaal van de belangrijkste ASE blade boven. Kunt u meerdere schaal wijzigingen in de configuratie op de front-end- en werknemer van toepassingen. Ze worden toegepast als een enkele bewerking.
- Een bewerking handmatig schalen van de afzonderlijke resource pool **schaal** blade, onder **Instellingen**.
- AutoScaling, die u met de afzonderlijke resource pool **schaal** blade instelt.

Sleep de schuifregelaar de hoeveelheid die u wilt gebruiken en opslaan voor het gebruik van de werking van de schaal op de ASE-blade. Deze gebruikersinterface biedt ook ondersteuning voor het wijzigen van de grootte.  

![Schaal UI][6]

Ga naar de **Instellingen**voor het gebruik van de mogelijkheden voor handmatig of automatisch schalen in een bepaalde resourcegroep, > **Front End-toepassingen** / **Pools werknemer** zo nodig. Open vervolgens de groep die u wilt wijzigen. Ga naar **Instellingen** > **Scale Out** - of **-Instellingen** > **vergroten**. De **Schaal van** blade kunt u bepalen de hoeveelheid exemplaar. **Schaal van** kunt u grootte van de resource.  

![Schaal settings-gebruikersinterface][7]

## <a name="fault-tolerance-considerations"></a>Fouttolerantie overwegingen

U kunt een App-omgeving om maximaal 55 compute totale middelen te gebruiken. Van deze 55 compute-middelen kunnen slechts 50 naar host werklasten worden gebruikt. De reden hiervoor is tweeledig. Er is een minimum van 2 front-end compute-bronnen.  Die blijven tot 53 ter ondersteuning van de toewijzing van werknemer-toepassingen. Met het oog op de fouttolerantie, moet u een extra compute bron hebt die is toegewezen op basis van de volgende regels:

- Elke werknemer toepassingen moet ten minste 1 extra compute resource die niet beschikbaar is voor een werklast zijn toegewezen.
- Wanneer de hoeveelheid van bronnen in een groep werknemers compute boven een bepaalde waarde gaat, zijn een andere compute resource is vereist voor fouttolerantie. Dit is niet het geval in de front-end-toepassingen.

Binnen elke groep één werknemer zijn de vereisten voor fouttolerantie die voor een bepaalde waarde van X resources toegewezen aan een werknemer van toepassingen:

- Als X tussen 2 en 20 is, is de hoeveelheid bruikbaar compute-bronnen die u voor een standaardwerkbelasting gebruiken kunt X-1.
- Als X tussen 21 en 40, is de hoeveelheid bruikbaar compute-bronnen die u voor een standaardwerkbelasting gebruiken kunt X-2.
- Als X tussen 41 en 53, is de hoeveelheid bruikbaar compute-bronnen die u voor een standaardwerkbelasting gebruiken kunt X-3.

De minimale footprint heeft 2 front-end-servers en 2 werknemers.  Met de bovenstaande instructies vervolgens zijn hier een paar voorbeelden ter verduidelijking:  

- Als u in een enkele groep met 30 werknemers, kan naar host werklasten 28 van deze gebruikt.
- Als u in een enkele groep 2 werknemers, kan aan de werklast van de host 1 gebruikt.
- Als u in een enkele groep met 20 werknemers, kan 19 worden gebruikt om de werkbelasting van de host.  
- Als u in een enkele groep met 21 werknemers, kan vervolgens nog enige 19 naar host werklasten worden gebruikt.  

De fouttolerantie aspect is belangrijk, maar u moet er rekening mee houden als u de schaal boven bepaalde drempels. Als u wilt meer capaciteit van 20 exemplaren toe te voegen, ga dan naar 22 of hoger omdat 21 niet een grotere capaciteit toevoegen. Hetzelfde geldt gaat boven 40, waarbij het volgende nummer die capaciteit toegevoegd 42 is.  

## <a name="deleting-an-app-service-environment"></a>Verwijderen van een App-omgeving ##

Als u verwijderen van een App-omgeving wilt, gewoon gebruikt u de actie **verwijderen** aan de bovenkant van het blad App-omgeving. Wanneer u dit doet, wordt u gevraagd om de naam van uw App-omgeving te bevestigen dat u werkelijk dit wilt doen. Houd er rekening mee dat wanneer u een App-omgeving verwijdert, u alle inhoud in het ook verwijdert.  

![Verwijderen van een Service App omgeving UI][9]  

## <a name="getting-started"></a>Aan de slag

Zie [het maken van een App-omgeving](app-service-web-how-to-create-an-app-service-environment.md)aan de slag met App Service-omgevingen.

Zie voor meer informatie over het platform Azure App Service [Azure App-Service](../app-service/app-service-value-prop-what-is.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/

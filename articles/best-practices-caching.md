<properties
   pageTitle="Richtlijnen voor caching | Microsoft Azure"
   description="Richtlijnen voor caching ter verbetering van de prestaties en schaalbaarheid."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/14/2016"
   ms.author="masashin"/>


# <a name="caching-guidance"></a>Richtlijnen voor caching

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

Caching is een veelvoorkomende techniek die beoogt te verbeteren de prestaties en schaalbaarheid van een systeem. Dit gebeurt door vaak opgevraagde gegevens tijdelijk kopiëren naar een snelle opslag die zich dicht bij de toepassing. Als deze snelle opslag zich dichter bij de toepassing dan de oorspronkelijke bron bevindt, kan vervolgens opslaan in de cache aanzienlijk verbeteren responstijden voor client-toepassingen door gegevens sneller te bedienen.

Caching is vooral effectief wanneer een clientexemplaar herhaaldelijk dezelfde gegevens leest, met name als de volgende voorwaarden van toepassing op de oorspronkelijke data store:
- Het blijft tamelijk statisch.
- Het is traag vergeleken met de snelheid van de cache.
- Dit kan een hoog niveau van een conflict.
- Het is ver weg netwerkvertraging, kan worden langzaam.

## <a name="caching-in-distributed-applications"></a>Opslaan in cache in gedistribueerde toepassingen

Gedistribueerde toepassingen meestal implementeert een of beide van de volgende strategieën bij het cachen van gegevens:

- Met behulp van een persoonlijke cache, waar gegevens lokaal is opgeslagen op de computer waarop een exemplaar van een toepassing of service.
- Met behulp van een gedeelde cache, die als gemeenschappelijke bron die door meerdere processen en/of machines kan worden benaderd.

In beide gevallen kan opslaan in de cache worden uitgevoerd aan de clientzijde en/of aan de serverzijde. Caching aan clientzijde is gedaan door het proces waarmee de gebruikersinterface voor een systeem, zoals een webbrowser of een bureaubladtoepassing.
Caching op de server wordt uitgevoerd door het proces dat de zakelijke diensten die op afstand worden uitgevoerd.

### <a name="private-caching"></a>Privé-caching

De meest eenvoudige type cache is een winkel in het geheugen. Gehouden in de adresruimte van één proces heeft en rechtstreeks toegankelijk voor de code die wordt uitgevoerd in het proces. Dit type cache is zeer snelle toegang. Het biedt ook een zeer doeltreffend middel voor de opslag van kleine hoeveelheden statische gegevens, omdat de grootte van een cache is meestal beperkt door de hoeveelheid geheugen die beschikbaar is op de computer die als host fungeert voor het proces.

Als u meer informatie dan fysiek mogelijk is in het geheugen in de cache, kunt u gegevens in de cache op het lokale bestandssysteem. Dit zijn lager dan de gegevens die in het geheugen worden vastgehouden tot, maar moet nog sneller en betrouwbaarder dan het ophalen van gegevens via een netwerk.

Als er meerdere exemplaren van een toepassing die gebruikmaakt van dit model tegelijkertijd worden uitgevoerd, heeft elke toepassingsinstantie een eigen onafhankelijke cache houdt een eigen kopie van de gegevens.

Zie een cache als een momentopname van de oorspronkelijke gegevens op een bepaald moment in het verleden. Als deze gegevens niet statisch is, is het waarschijnlijk dat verschillende toepassingsexemplaren verschillende versies van de gegevens in de cache bevatten. Daarom kan dezelfde query uitgevoerd door deze instanties genereren verschillende resultaten, zoals in figuur 1.

![Met behulp van een cache in het geheugen in verschillende exemplaren van een toepassing](media/best-practices-caching/Figure1.png)

_Afbeelding 1: Met behulp van een cache in het geheugen in verschillende exemplaren van een toepassing_

### <a name="shared-caching"></a>Gedeelde caching

Met behulp van een gedeelde cache kunt verlichten bezorgdheid dat de gegevens in elke cache, die zich met cache in het geheugen voordoen kan kan verschillen. Gedeelde cache zorgt ervoor dat andere exemplaren dezelfde weergave van gegevens in de cache zien. Dit gebeurt door het vinden van de cache op een aparte locatie, meestal als onderdeel van een afzonderlijke service worden gehost als afgebeeld in figuur 2.

![Met behulp van een gedeelde cache](media/best-practices-caching/Figure2.png)

_Afbeelding 2: Met behulp van een gedeelde cache_

Een belangrijk voordeel van de gedeelde cache aanpak is de schaalbaarheid biedt. Veel cache voor gedeelde services worden geïmplementeerd met behulp van een cluster van servers en software die worden de gegevens over het cluster verdeeld op transparante wijze kunnen gebruiken. Een toepassingsexemplaar gewoon verzonden een verzoek naar de cache-service.
De onderliggende infrastructuur is verantwoordelijk voor het bepalen van de locatie van de gegevens in de cache in het cluster. U kunt eenvoudig de cache schalen door meer servers toe te voegen.

Er zijn twee belangrijke nadelen van de gedeelde cache methode:
- De cache is langzamer openen, omdat het niet langer lokaal op elk toepassingsexemplaar wordt gehouden.
- De verplichting om een afzonderlijke cache service implementeren kan complexiteit toevoegen aan de oplossing.

## <a name="considerations-for-using-caching"></a>Overwegingen bij het gebruik van caching

De volgende secties worden in meer detail de overwegingen bij het ontwerpen en gebruiken van een cache.

### <a name="decide-when-to-cache-data"></a>Bepalen wanneer gegevens in de cache

Caching kan enorm verbeteren prestaties, schaalbaarheid en beschikbaarheid. Hoe meer gegevens die u hebt en hoe groter het aantal gebruikers die toegang krijgen tot deze gegevens, hoe groter de voordelen moeten van in het cachegeheugen worden. Dat komt doordat de cache minder latentie en conflicten die is gekoppeld aan het verwerken van grote hoeveelheden gelijktijdige aanvragen in het oorspronkelijke gegevensarchief.

Een database kan bijvoorbeeld ondersteuning voor een beperkt aantal gelijktijdige verbindingen. Gegevens ophalen uit een gedeelde cache, maar maakt in plaats van de onderliggende database het mogelijk voor een clienttoepassing toegang tot deze gegevens zelfs als het aantal beschikbare verbindingen uitgeput is. Bovendien, als de database niet beschikbaar is, clienttoepassingen mogelijk om door te gaan met behulp van de gegevens die worden vastgehouden in de cache.

U kunt het cachen van gegevens die vaak lezen maar af en toe (bijvoorbeeld gegevens die een groter aantal leesbewerkingen dan schrijfbewerkingen) gewijzigd. Echter raadzaam niet gebruik te maken van de cache als de gezaghebbende opslag van essentiële informatie. In plaats daarvan, ervoor te zorgen dat alle wijzigingen die de toepassing zich niet veroorloven te verliezen altijd worden opgeslagen met een blijvende gegevensopslag. Dit betekent dat als de cache niet beschikbaar is, de toepassing nog steeds werken blijven kunt met behulp van de data store en u belangrijke gegevens gaan niet verloren.

### <a name="determine-how-to-cache-data-effectively"></a>Bepalen hoe gegevens in de cache effectief

De sleutel tot het effectief gebruik van een cache ligt in de meest geschikte gegevens in cache te bepalen en het in cache plaatsen op het juiste moment. De gegevens kunnen worden toegevoegd aan de cache op verzoek de eerste keer dat wordt overgenomen door een toepassing. Dit betekent dat de toepassing moet de gegevens slechts één keer ophalen uit het gegevensarchief en die later toegang met behulp van de cache kan worden voldaan.

Ook kan een cache gedeeltelijk of volledig gevuld met gegevens van tevoren meestal wanneer de toepassing wordt gestart (een aanpak seeding genoemd). Echter niet mogelijk aan te raden te implementeren voor een grote cache enten omdat deze aanpak een plotselinge, hoge belasting van de oorspronkelijke gegevensopslag kan worden opgelegd wanneer de toepassing wordt gestart.

Vaak kunt een analyse van de gebruikspatronen u bepalen of u een cache vooraf invullen, geheel of gedeeltelijk en kiest u de gegevens in cache. Bijvoorbeeld, kan het handig zijn bij de seeding van de cache met de statische gebruikersprofielgegevens voor klanten die regelmatig de toepassing (bijvoorbeeld elke dag) gebruiken, maar niet voor klanten die gebruikmaken van de toepassing slechts een keer per week.

Caching meestal werkt goed samen met de gegevens onveranderbaar of die niet vaak veranderen. Voorbeelden zijn informatie zoals product- en prijsgegevens in een e-commerce-toepassing of gedeelde statische bronnen die zijn goedkoper te maken. Sommige of alle van deze gegevens kunnen worden geladen in de cache bij het opstarten van de toepassing te minimaliseren de belasting van bronnen en de prestaties te verbeteren. Mogelijk hebben een achtergrondproces dat periodiek referentie bijgewerkt gegevens in de cache zodat deze up-to-date is, of dat de cache wordt vernieuwd wanneer verwijzen naar gegevens wijzigingen.

Caching is minder handig voor dynamische gegevens, hoewel er enkele uitzonderingen op deze vergoeding (Zie de sectie Cache zeer dynamische gegevens verderop in dit artikel voor meer informatie). Wanneer de oorspronkelijke gegevens regelmatig worden gewijzigd, de gegevens in de cache is zeer snel verlopen of de overhead van de cache gesynchroniseerd met het oorspronkelijke gegevensarchief vermindert de doeltreffendheid van de cache.

Houd er rekening mee dat een cache geen neemt u de volledige gegevens voor een entiteit. Bijvoorbeeld als een gegevensitem een object met meerdere waarden zoals een klant van de bank met een naam, adres en het saldo vertegenwoordigt, kan sommige van deze elementen statisch blijven (zoals naam en adres), terwijl anderen (het rekeningsaldo) mogelijk meer dynamische. In situaties als deze is het handig om het statische gedeelte van de gegevens in de cache en alleen de resterende gegevens halen (of berekenen) wanneer dat nodig is.

Het is raadzaam u verrichten prestaties testen en het gebruik van analyses om te bepalen of vooraf vullen of op aanvraag laden van de cache of een combinatie van beide, geschikt is. Het besluit moet worden gebaseerd op de volatiliteit en het gebruikspatroon van de gegevens. Cache-gebruik en de prestaties van de analyse is vooral belangrijk in toepassingen die een zware belasting optreden en uiterst schaalbaar moeten zijn. Bijvoorbeeld, in hoge mate schaalbaar scenario's kan het zinvol bij de seeding van de cache om de werklast verminderen van de data store tijdens piekuren.

Opslaan in de cache kan worden gebruikt berekeningen herhaling terwijl de toepassing actief is. Als een bewerking gegevens worden omgezet, of een ingewikkelde berekening wordt uitgevoerd, kan deze de resultaten van de bewerking opslaan in de cache. Als dezelfde berekening achteraf vereist is, kan de toepassing de resultaten gewoon ophalen uit de cache.

Een toepassing kan gegevens die opgeslagen in een cache te wijzigen. We raden u echter aan denken van de cache als een tijdelijke gegevensopslag die kan op elk moment verdwijnen. Waardevolle gegevens niet opslaan in de cache. Zorg ervoor dat u de gegevens in het oorspronkelijke gegevensarchief en beheren. Dit betekent dat als de cache niet beschikbaar is, verkleint u de kans dat gegevens verloren gaan.

### <a name="cache-highly-dynamic-data"></a>Cache zeer dynamische gegevens

Wanneer u snel veranderende informatie in een blijvende gegevensopslag opslaat, kunnen een overhead op het systeem worden opgelegd. Neem bijvoorbeeld een apparaat dat voortdurend status of een andere meting rapporten. Als een toepassing niet wil op basis van de dat de gegevens in de cache bijna altijd verouderde worden wordt gegevens in de cache, kan hetzelfde true wanneer op te slaan en deze gegevens ophalen uit het gegevensarchief zijn. In de tijd die nodig is voor het opslaan en ophalen van gegevens, het mogelijk gewijzigd.

U kunt de voordelen van de dynamische gegevens rechtstreeks in de cache in plaats van in het archief van de permanente gegevens op te slaan in een situatie zoals dit. Als de gegevens niet-kritieke worden en niet hoeft te controleren, vervolgens het maakt niet uit als de incidentele wijziging verloren gegaan is.

### <a name="manage-data-expiration-in-a-cache"></a>Vervaldatum van de gegevens in een cache beheren

Gegevens die in het cachegeheugen worden vastgehouden wordt in de meeste gevallen een kopie van de gegevens die in het oorspronkelijke gegevensarchief wordt bewaard. De gegevens in het oorspronkelijke gegevensarchief kan worden gewijzigd nadat het in de cache is, waardoor de gegevens in de cache verlopen. Veel systemen van caching kunnen u voor het configureren van de cache is verlopen gegevens en het verminderen van de periode waarvoor gegevens zijn mogelijk verouderd.

Gegevens in de cache is verlopen, wordt deze verwijderd uit de cache als de toepassing gegevens moet ophalen uit de oorspronkelijke data store (het kunt terughalen de nieuw opgehaalde gegevens in cache). U kunt een verloopbeleid standaard instellen wanneer u de cache configureren. In veel webcache-services, kunt u ook de verloopperiode voor afzonderlijke objecten bepalen wanneer u deze via programmacode opslaan in de cache.
Sommige caches kunnen u opgeven hoe lang deze periode als een absolute waarde, of als een verschuivende waarde die ervoor zorgt dat het item wordt verwijderd uit de cache als dit niet binnen de opgegeven tijd wordt gestart. Deze instelling overschrijft de hele cache verloopbeleid, maar alleen voor de opgegeven objecten.

> [AZURE.NOTE] De verloopperiode voor de cache en de objecten erin zorgvuldig overwegen. Als u het te kort, objecten te snel verloopt en verkleint u de voordelen van het gebruik van de cache. Als u de periode te lang, kunnen de gegevens verouderd.

Het is ook mogelijk de cache kan terechtkomen als gegevens kan gedurende lange tijd aanwezig blijven. In dit geval sommige items automatisch worden verwijderd in een proces dat bekend staat als een verwijdering ervoor zorgen dat aanvragen voor nieuwe items toevoegen aan de cache. Webcache-services meestal gegevens op basis van least recently used (Recentelijk) weghalen, maar meestal dit beleid opheffen en voorkomen dat items worden verwijderd. Echter als u overstapt op deze benadering, loopt u het risico van meer dan de hoeveelheid geheugen die beschikbaar is in de cache. Een toepassing die u probeert een item wilt toevoegen aan de cache mislukt met een uitzondering.

Sommige implementaties caching mogelijk extra verwijdering beleid bieden. Er zijn verschillende soorten beleid voor verwijdering. Deze omvatten:
- Laatst gebruikte beleid (in de verwachting dat de gegevens niet vereist opnieuw zijn zal).
- Een beleid voor first in first out (oudste gegevens verwijderd eerst).
- Een beleid voor expliciete verwijderen op basis van een gebeurtenis te activeren (zoals de gegevens worden gewijzigd).

### <a name="invalidate-data-in-a-client-side-cache"></a>Gegevens in een client-side cache ongeldig

Gegevens die worden vastgehouden in een client-side cache is over het algemeen beschouwd als buiten het kader van de service waarmee de gegevens naar de client. Een service kan niet direct een client toevoegen of verwijderen van gegevens uit de cache aan de clientzijde dwingen.

Dit betekent dat het voor een client die een slecht geconfigureerde cache gebruikt om door te gaan met het gebruik van verouderde gegevens mogelijk is. Bijvoorbeeld als het verloopbeleid van de cache worden niet juist uitgevoerd, een client kunnen worden gebruikt verouderde gegevens die lokaal wordt opgeslagen wanneer de gegevens in de oorspronkelijke gegevensbron is gewijzigd.

Als u een webtoepassing die gegevens via een HTTP-verbinding fungeert, kunt u impliciet een webclient (zoals een browser of een webproxy) forceren voor het ophalen van de meest recente informatie. U kunt dit doen als een resource wordt door een wijziging in de URI voor die resource bijgewerkt. Webclients gebruiken gewoonlijk de URI van een resource als de sleutel in de cache van de client, zodat de WebClient wordt genegeerd als de URI wordt gewijzigd, een eerder opgeslagen versies van een resource en in plaats daarvan de nieuwe versie kan worden opgehaald.

## <a name="managing-concurrency-in-a-cache"></a>Beheren van gelijktijdige uitvoering in een cache

Caches zijn vaak ontworpen om te worden gedeeld door meerdere exemplaren van een toepassing. Elk toepassingsexemplaar kunt lezen en wijzigen van gegevens in de cache. Als gevolg daarvan dezelfde gelijktijdigheid van problemen die zich kunnen met elk archief gedeelde gegevens voordoen ook van toepassing op een cache. In een situatie waarin een toepassing moet wijzigen, gegevens die in het cachegeheugen worden vastgehouden, mogelijk moet u ervoor zorgen dat updates gemaakt door één exemplaar van de toepassing niet de wijzigingen die zijn aangebracht door een ander exemplaar overschrijven.

Afhankelijk van de aard van de gegevens en de kans op conflicten, kunt u een van de twee benaderingen van gelijktijdigheid vast:

- __Optimistische.__ Voor het bijwerken van de gegevens, de toepassing controleert of de gegevens in de cache is gewijzigd sinds deze is opgehaald. Als de gegevens nog steeds hetzelfde is, kan de wijziging worden gemaakt. Anders heeft de toepassing om te bepalen of deze bij te werken. (De bedrijfslogica die dit besluit zijn toepassingsspecifiek.) Deze aanpak is geschikt voor situaties waarin updates zijn incidentele of waar er conflicten optreden zijn.
- __Pessimistische.__ Bij het ophalen van de gegevens, de toepassing worden vergrendeld in de cache om te voorkomen dat een ander exemplaar wijzigen. Dit proces zorgt ervoor dat conflicten niet kunnen zijn, maar ze kunnen ook andere instanties die nodig zijn voor het verwerken van dezelfde gegevens blokkeren. Volledige vergrendeling kan invloed hebben op de schaalbaarheid van een oplossing en wordt alleen aanbevolen voor tijdelijk bewerkingen. Deze benadering is misschien geschikt voor situaties waarin conflicten waarschijnlijker, met name als een toepassing meerdere items in de cache bijgewerkt en ervoor zorgen moet dat deze wijzigingen worden toegepast.

### <a name="implement-high-availability-and-scalability-and-improve-performance"></a>Implementeren van hoge beschikbaarheid en schaalbaarheid en de prestaties verbeteren

Vermijd het gebruik van een cache als primaire opslagplaats van gegevens; Dit is de rol van het oorspronkelijke gegevensarchief waaruit de cache wordt gevuld. Het oorspronkelijke gegevensarchief is verantwoordelijk voor de persistentie van de gegevens.

Zorg ervoor dat u geen essentiële afhankelijk is van de beschikbaarheid van een gedeelde cache-service in uw oplossingen te introduceren. Een toepassing moet kunnen blijven functioneren als de service voor gedeelde cache niet beschikbaar is. De toepassing moet niet vastlopen of mislukken tijdens het wachten op de cache-service te hervatten.

Daarom zijn de toepassing voorbereid op de beschikbaarheid van de cache-service detecteren en terugschakelen naar de oorspronkelijke data store als de cache niet toegankelijk is. De [stroomonderbreker patroon](http://msdn.microsoft.com/library/dn589784.aspx) is handig voor het verwerken van dit scenario. De service waarmee de cache kan worden hersteld en zodra deze beschikbaar de cache opnieuw gegevens vormen het oorspronkelijke gegevensarchief na een strategie zoals de [aanleg van de Cache-patroon](http://msdn.microsoft.com/library/dn589799.aspx)wordt gelezen kan worden ingevuld.

Echter, kunnen er een schaalbaarheid invloed op het systeem als de toepassing terug naar de oorspronkelijke gegevensopslag valt als de cache tijdelijk niet beschikbaar is.
Terwijl het gegevensarchief worden hersteld, wordt het oorspronkelijke gegevensarchief kan worden veel tijd gaan zitten met verzoeken om gegevens, waardoor de time-outs en verbindingen is mislukt.

Kunt u de implementatie van een lokale cache in elk exemplaar van een toepassing, samen met de gedeelde cache die overal van toepassing. Wanneer een item wordt opgehaald, kunt controleren eerst in de lokale cache en klik in de gedeelde cache en ten slotte in de oorspronkelijke gegevens opgeslagen. De lokale cache kan worden gevuld met de gegevens in de gedeelde cache of in de database als de gedeelde cache niet beschikbaar is.

Deze benadering vergt de configuratie zorgvuldig om te voorkomen dat de lokale cache te verouderd ten opzichte van de gedeelde cache. Echter, de lokale cache als buffer fungeert als de gedeelde cache onbereikbaar is. In figuur 3 ziet deze structuur.

![Met behulp van een lokale cache met een gedeelde cache](media/best-practices-caching/Caching3.png)
_in figuur 3: gebruik van een lokale cache met een gedeelde cache_

Sommige webcache-services bieden ter ondersteuning van grote caches die relatief lang duren gegevens bevatten een optie met hoge beschikbaarheid die automatische failover worden geïmplementeerd als de cache niet beschikbaar is. Deze aanpak betekent meestal dat de gegevens in de cache die opgeslagen op een cacheserver primaire naar een secundaire cacheserver repliceren en overschakelen naar de secundaire server als de primaire server is mislukt of de verbinding wordt verbroken.

Als u wilt beperken de vertraging die is gekoppeld aan het schrijven naar meerdere bestemmingen, kan de replicatie naar de secundaire server optreden asynchroon wanneer gegevens worden geschreven naar de cache op de primaire server. Deze benadering leidt tot de mogelijkheid dat sommige gegevens in de cache mogelijk verloren bij een storing, maar de verhouding van deze gegevens klein in vergelijking met de totale grootte van de cache moet.

Als een gedeelde cache hoog is, kan het zinvol zijn om gegevens in de cache op de knooppunten te verminderen de kans op conflicten en de schaalbaarheid te verbeteren zijn. Veel gedeelde cache ondersteuning voor dynamisch toevoegen (of verwijderen) op knooppunten en de gegevens opnieuw in partities. Deze aanpak betrekt clustering, waarin de verzameling knooppunten wordt gepresenteerd voor clienttoepassingen een naadloze, één cache. Intern, maar de gegevens worden verdeeld over tussen knooppunten na een vooraf gedefinieerde distributiestrategie die de werklast gelijkmatig. De [partitionering leidraad voor gegevens](http://msdn.microsoft.com/library/dn589795.aspx) op de Microsoft-website vindt u meer informatie over mogelijke partitionering strategieën.

Clustering, kunt ook de beschikbaarheid van de cache vergroten. Als een knooppunt uitvalt, wordt de rest van de cache nog steeds toegankelijk is.
Clustering wordt vaak gebruikt in combinatie met replicatie en -failover. Elk knooppunt kan worden gerepliceerd en de replica kan snel on line worden gebracht als het knooppunt mislukt.

Veel lees- en schrijfbewerkingen kunnen betrekking hebben op gegevenswaarden van enkele of objecten zijn. Echter, soms kan het nodig zijn voor het opslaan of ophalen van grote hoeveelheden gegevens snel.
Bijvoorbeeld een cache seeding kan betrekking hebben op honderden of duizenden artikelen schrijven naar de cache. Een toepassing moet misschien ook een groot aantal verwante items opgehaald uit de cache als onderdeel van één verzoek.

Veel grootschalige caches bevatten batchbewerkingen voor deze doeleinden. Dit kan een clienttoepassing verpakken van een groot aantal items in een enkele aanvraag en vermindert de overhead die is gekoppeld aan het uitvoeren van een groot aantal kleine aanvragen.

## <a name="caching-and-eventual-consistency"></a>Caching en uiteindelijke algehele consistentie

Voor de cache braakgelegde patroon om te werken, moet het exemplaar van de toepassing die u invult op de cache toegang hebben tot de meest recente en consistente versie van de gegevens. In een systeem dat de uiteindelijke algehele consistentie (zoals een gerepliceerd gegevensarchief) implementeert dit mogelijk niet het geval.

Een exemplaar van een toepassing kan een item met gegevens wijzigen en versie van het item in de cache ongeldig maken. Een ander exemplaar van de toepassing probeert te lezen van dit item uit een cache die ervoor zorgt een cache-missers, dat zodat het de gegevens uit het gegevensarchief leest en aan de cache toegevoegd. Echter kan, als de data store is niet volledig gesynchroniseerd met andere replica's, exemplaar van de toepassing lezen en vullen van de cache met de oude waarde.

Zie de pagina [gegevens consistent primer](http://msdn.microsoft.com/library/dn589800.aspx) op de Microsoft-website voor meer informatie over het afhandelen van de consistentie van de gegevens.

### <a name="protect-cached-data"></a>In cache opgeslagen gegevens beveiligen

Ongeacht de cache-service gebruiken, bepalen hoe de gegevens die worden vastgehouden in de cache tegen toegang door onbevoegden te beveiligen. Er zijn twee belangrijke problemen:

- De privacy van de gegevens in de cache
- De privacy van gegevens zoals deze loopt tussen de cache en de toepassing die van de cache gebruikmaakt

Ter bescherming van gegevens in de cache, kan de cache service implementeren een verificatiemechanisme dat vereist is dat toepassingen de volgende opgeeft:
- Welke identiteiten toegang tot de gegevens in de cache.
- Welke bewerkingen (lezen en schrijven) deze identiteiten mogen uit te voeren.

Om overhead die is gekoppeld aan het lezen en schrijven van gegevens, nadat een identiteit is verleend, schrijven en/of leestoegang tot de cache, dat identiteit alle gegevens in de cache kan gebruiken.

Als u nodig hebt om toegang te beperken tot subsets met gegevens in de cache, kunt u het volgende doen:

- De cache in partities opsplitsen (met behulp van verschillende cache servers) en alleen toegang tot de identiteit van de partities die ze mogen gebruiken.
- De gegevens in elke subset coderen met behulp van verschillende toetsen en de coderingssleutels alleen voor identiteiten die toegang tot elke subset hebben moeten bieden. Een clienttoepassing mogelijk nog steeds alle gegevens in de cache ophalen, maar dit is alleen mogelijk voor het decoderen van de gegevens waarvoor de sleutels.

Als het loopt en naar de cache, moet u de gegevens beveiligen. U doet dit door u is afhankelijk van de beveiligingsfuncties van de netwerkinfrastructuur die clienttoepassingen verbinding naar de cache te gebruiken. Als de cache wordt geïmplementeerd met behulp van een server op locatie binnen dezelfde organisatie die als host fungeert voor de clienttoepassingen, vervolgens de isolatie van het netwerk zelf niet moet u mogelijk aanvullende stappen uitvoeren. Als de cache bevindt zich op afstand en een TCP of HTTP-verbinding via een openbaar netwerk (zoals Internet vereist), kunt u de implementatie van SSL.

## <a name="considerations-for-implementing-caching-with-microsoft-azure"></a>Overwegingen voor de implementatie van Microsoft Azure-caching

Azure biedt de Azure bestand Vgx. Cache. Dit is een implementatie van de open source bestand Vgx. cache die wordt uitgevoerd als een service in een datacenter Azure. Het biedt in het cachegeheugen die toegankelijk is vanuit elke toepassing Azure, of de toepassing wordt geïmplementeerd als een cloud-service, een website of in een Azure virtuele machine. Caches kunnen worden gedeeld door toepassingen die beschikt over de juiste sleutel.

Azure bestand Vgx. Cache is een krachtige cache-oplossing waarmee de beschikbaarheid, schaalbaarheid en beveiliging. Normaal gesproken wordt uitgevoerd als een service die verspreid over een of meer specifieke computers. Probeert op te slaan zoveel informatie kan het in het geheugen om snel toegang te waarborgen. Deze architectuur is bedoeld om lage latentie en hoge doorvoersnelheden doordat de noodzaak langzaam i/o-bewerkingen uit te voeren.

 Azure bestand Vgx. Cache is compatibel met veel van de verschillende API's die door toepassingen worden gebruikt. Als u bestaande toepassingen die al gebruikmaken van Azure bestand Vgx. Cache op locatie uitgevoerd, de Azure bestand Vgx. Cache een snelle migratiepad bevat naar caching in de cloud.

> [AZURE.NOTE] Azure biedt ook de Cache Managed Service. Deze service is gebaseerd op de motor Azure Service Fabric-Cache. U kunt hiermee een gedistribueerde cache die kan worden gedeeld door de flexibele toepassingen maken. De cache wordt gehost op servers in een datacenter Azure hoge prestaties.
Deze optie wordt echter niet meer wordt aanbevolen en is alleen beschikbaar voor de ondersteuning van bestaande toepassingen die zijn gebouwd om het te gebruiken. Ontwikkeling van alle nieuwe, Azure bestand Vgx. Cache gebruiken.
>
> Bovendien ondersteunt Azure in rol in de cache opslaan. Deze functie kunt u een cache die specifiek is voor een cloud-service maken.
De cache wordt gehost door exemplaren van een web- of werknemer en is alleen toegankelijk voor de functies die zijn die deel uitmaken van dezelfde cloud-service implementatie. (Een implementatie-eenheid is de set rol exemplaren die zijn geïmplementeerd als een cloud-service aan een specifieke regio). De cache is gegroepeerd en alle exemplaren van de rol binnen dezelfde implementatie-eenheid die fungeert als host voor de cache deel uitmaken van hetzelfde cluster cache. Deze optie wordt echter niet meer wordt aanbevolen en is alleen beschikbaar voor de ondersteuning van bestaande toepassingen die zijn gebouwd om het te gebruiken. Ontwikkeling van alle nieuwe, Azure bestand Vgx. Cache gebruiken.
>
> Azure-Service beheerd Cache zowel Azure In rol Cache zijn momenteel ondersteuningsperiode voor uittreding op 16e November 2016.
Het wordt aanbevolen om te migreren naar Azure bestand Vgx. Cache ter voorbereiding van dit pensioen. Voor meer informatie gaat u naar de pagina   [Wat is Azure bestand Vgx. Cache aangeboden en op welk formaat moet ik gebruiken?](redis-cache/cache-faq.md#what-redis-cache-offering-and-size-should-i-use) op de website van Microsoft.


### <a name="features-of-redis"></a>Functies van het bestand Vgx.

 Bestand Vgx. is meer dan een eenvoudige cacheserver. Het biedt een gedistribueerde database voor in het geheugen met een uitgebreide opdrachtset die veel voorkomende scenario's worden ondersteund. Deze worden verderop in dit document, in de sectie met behulp van het bestand Vgx. caching beschreven. Deze sectie bevat een overzicht van enkele van de belangrijkste functies die bestand Vgx. biedt.

### <a name="redis-as-an-in-memory-database"></a>Bestand als een in-memory database Vgx.

Bestand Vgx. ondersteunt zowel lees- en schrijfbewerkingen. In het bestand Vgx., worden schrijfbewerkingen beschermd tegen systeemfouten door periodiek worden opgeslagen in een lokale snapshot-bestand of in een logboekbestand alleen toevoegen. Dit is niet het geval is in veel caches (die overwogen moeten worden opgeslagen gegevens van tijdelijke aard).

 Alle schrijfbewerkingen zijn asynchroon en Blokkeer niet lezen en schrijven van gegevens van clients. Wanneer bestand Vgx. starten, leest de gegevens van de momentopname of log-bestand en gebruikt voor het maken van de cache in het geheugen. Voor meer informatie Zie [bestand Vgx. persistentie](http://redis.io/topics/persistence) van bestand Vgx.-website.

> [AZURE.NOTE] Bestand Vgx. kan niet garanderen dat alle schrijfbewerkingen worden opgeslagen bij een catastrofale storing, maar in het ergste geval kan verloren gaan maar een paar seconden waard van gegevens. Houd er rekening mee dat een cache is niet bedoeld om te fungeren als een gezaghebbende bron, en het is de verantwoordelijkheid van de toepassingen die de cache gebruiken om ervoor te zorgen dat kritieke gegevens met succes naar een winkel van de juiste gegevens opgeslagen. Zie voor meer informatie de [grond cache-patroon](http://msdn.microsoft.com/library/dn589799.aspx).

#### <a name="redis-data-types"></a>Gegevens van het type bestand Vgx.

Bestand Vgx. is een sleutel / waarde-winkel, waar de waarden typen van eenvoudige of complexe gegevensstructuren, zoals hashes, lijsten bevatten kunnen, en wordt ingesteld. Een set van atomaire bewerkingen op deze gegevenstypen ondersteund. Sleutels kunnen worden permanent of gecodeerd met een beperkt aantal time-to-live, op welk punt de sleutel en de bijbehorende waarde wordt automatisch uit de cache verwijderd. Ga naar de pagina [een inleiding tot het bestand Vgx. gegevenstypen en abstracties](http://redis.io/topics/data-types-intro) op de website van bestand Vgx. voor meer informatie over het bestand Vgx. sleutels en waarden.

#### <a name="redis-replication-and-clustering"></a>Bestand Vgx replicatie en clusters.

Bestand Vgx. ondersteunt master/slave replicatie voor beschikbaarheid en onderhouden van doorvoer. Schrijven bewerkingen aan een bestand Vgx. master knooppunt naar een of meer onderliggende knooppunten worden gerepliceerd. Leesbewerkingen kunnen worden bediend door de kapitein of een van de onderliggende niveaus.

In geval van een netwerkpartitie kunnen onderliggende shapes blijven gegevens dienen en vervolgens transparant synchronisatie met het model als de verbinding is hersteld. Bezoek de [replicatie](http://redis.io/topics/replication) voor meer informatie op de website van bestand Vgx..

Bestand Vgx. Bovendien clustering, waarmee u transparant gegevens verdelen in shards voor servers en de belasting wordt verdeeld. Deze functie verbetert de schaalbaarheid, omdat het nieuwe bestand Vgx. servers kunnen worden toegevoegd en de gegevens opnieuw worden gepartitioneerd als de grootte van de cache wordt verhoogd.

Bovendien kan elke server in het cluster worden gerepliceerd via master/slave replicatie. Hierdoor beschikbaarheid op elk knooppunt in het cluster. Ga naar het [bestand Vgx. cluster zelfstudie pagina](http://redis.io/topics/cluster-tutorial) op de website van bestand Vgx. voor meer informatie over clusters en sharding.

### <a name="redis-memory-use"></a>Geheugengebruik bestand Vgx.

Een cache bestand Vgx. heeft een beperkte grootte die afhankelijk van de beschikbare bronnen op de hostcomputer is. Wanneer u een bestand Vgx. server configureert, kunt u de maximale hoeveelheid geheugen die kan worden gebruikt. U kunt ook een sleutel in een cache bestand Vgx. hebben een verloopdatum waarna deze automatisch verwijderd uit de cache. Deze functie kan voorkomen dat het cachegeheugen vullen met oude of verouderde gegevens.

Als het geheugen vol raakt, kan bestand Vgx. automatisch weghalen sleutels en waarden aan de hand van een aantal beleidsregels. De standaardwaarde is Recentelijk (laatst gebruikt), maar u kunt ook andere beleidsregels voor willekeurige sleutels verwijderen of een verwijdering helemaal uitschakelen (in welke, case pogingen items toevoegen aan de cache mislukt als het volledige) selecteren. De pagina [Met behulp van het bestand Vgx. Als een cache Recentelijk](http://redis.io/topics/lru-cache) vindt u meer informatie.

### <a name="redis-transactions-and-batches"></a>Bestand Vgx transacties en -batches.

Bestand Vgx. kunt een clienttoepassing aan het indienen van een reeks bewerkingen die gegevens lezen en schrijven in de cache van een atomic-transacties. De opdrachten in de transactie zijn gegarandeerd worden na elkaar uitgevoerd en wordt geen opdrachten van andere clients gelijktijdig worden ineengevlochten ertussen.

Dit zijn echter niet true transacties zoals een relationele database wilt uitvoeren. Verwerking van transacties bestaat uit twee fasen: de eerste is wanneer u de opdrachten in de wachtrij geplaatst en de tweede als de opdrachten worden uitgevoerd. Tijdens de opdracht queuing, worden de opdrachten die in de transactie zijn opgenomen door de client verzonden. Als de fout zich voordoet op dit punt (bijvoorbeeld een syntaxisfout of een onjuist aantal parameters) vervolgens bestand Vgx. weigert de hele transactie verwerkt en verwijdert alle.

Bestand Vgx. tijdens de uitvoering, wordt elke opdracht die in de wachtrij in volgorde uitgevoerd. Als een opdracht tijdens deze fase mislukt, bestand Vgx. verder met de volgende opdracht in de wachtrij en herstelt geen de effecten van alle opdrachten die u al hebt uitgevoerd. Deze vereenvoudigde vorm van de transactie kunt u prestaties bijhouden en problemen met de prestaties die worden veroorzaakt door een conflict te voorkomen.

Bestand Vgx. wordt geïmplementeerd door een vorm van beperkt vergrendelen om te helpen bij het onderhouden van de consistentie. Ga naar de [pagina transacties](http://redis.io/topics/transactions) op de website van bestand Vgx. voor meer informatie over transacties en met het bestand Vgx. vergrendelen.

Bestand Vgx. ondersteunt ook niet-transactionele batchen van aanvragen. Het bestand Vgx. protocol die clients gebruiken om opdrachten te verzenden naar een bestand Vgx. server kan clients een reeks bewerkingen als onderdeel van de dezelfde aanvraag verzenden. Dit kan helpen verminderen pakket fragmentatie op het netwerk. Als de batch wordt verwerkt, wordt elke opdracht wordt uitgevoerd. Als een van deze opdrachten verkeerd, ze zullen worden afgewezen (dit gebeurt met een transactie niet), maar de resterende opdrachten worden uitgevoerd. Er is ook geen garantie over de volgorde waarin de opdrachten in de batch wordt verwerkt.

### <a name="redis-security"></a>Beveiliging bestand Vgx.

Bestand Vgx. richt zich uitsluitend op de snelle toegang tot gegevens en is ontworpen om te worden uitgevoerd in een vertrouwde omgeving die kan alleen worden benaderd door vertrouwde clients. Bestand Vgx. ondersteunt een beperkte beveiligingsmodel gebaseerd op wachtwoordverificatie. (Het is mogelijk om verificatie volledig te verwijderen maar dit wordt niet aanbevolen.)

Alle geverifieerde clients hetzelfde wachtwoord algemene delen en toegang hebben tot dezelfde bronnen. Als u uitgebreidere-in beveiliging nodig hebt, moet u uw eigen beveiligingslaag voor het bestand Vgx. server implementeren en alle aanvragen van clients deze extra laag moeten passeren. Bestand Vgx. moet niet rechtstreeks blootgesteld aan niet-vertrouwde of niet-geverifieerde clients.

U kunt toegang tot opdrachten beperken door ze uit te schakelen of de naam van deze (en door te voorzien van de nieuwe namen alleen beschermde clients).

Bestand Vgx. ondersteunt niet elke vorm van gegevens te coderen, zodat alle codering moet worden uitgevoerd door de client-toepassingen. Bovendien biedt bestand Vgx. geen enige vorm van transportbeveiliging. Als u nodig hebt om gegevens te beschermen als deze in het netwerk doorloopt, raden wij de uitvoering van een SSL-proxy.

Ga naar de pagina [bestand Vgx. beveiliging](http://redis.io/topics/security) op de website van bestand Vgx. voor meer informatie.

> [AZURE.NOTE] Azure Cache bestand Vgx. biedt een eigen beveiligingsniveau waarmee clients verbinding maken. Het onderliggende bestand Vgx. servers niet naar het openbare netwerk beschikbaar zijn.

### <a name="using-the-azure-redis-cache"></a>Met behulp van de cache voor het bestand Vgx Azure.

De Cache voor het bestand Vgx. van Azure biedt toegang tot het bestand Vgx. servers worden uitgevoerd op servers die zijn ondergebracht op een Azure datacenter; het fungeert als een gevel die toegangsbeheer en beveiliging biedt. U kunt een cache met behulp van de portal Management Azure inrichten. De portal biedt een aantal vooraf gedefinieerde configuraties, variërend van een 53GB cache wordt uitgevoerd als een speciale service ondersteunt SSL-communicatie (voor privacy) en master/slave replicatie met een SLA van de beschikbaarheid van 99,9%, tot een 250 MB cache zonder herhaling (geen waarborgen van de beschikbaarheid) uitvoert op gedeelde hardware.

Met behulp van de portal Management Azure, kunt u ook een verwijdering van de cache-beleid configureren en toegang tot de cache beheren door gebruikers toe te voegen aan de rollen opgegeven. Lezer, Inzender, eigenaar. Deze rollen definiëren de bewerkingen die leden kunnen uitvoeren. Bijvoorbeeld leden van de rol van eigenaar hebt volledige controle over de cache (inclusief beveiliging) en de inhoud van leden van de rol van Inzender kunnen lezen en schrijven van gegevens in de cache en leden van de rol van lezer kunnen alleen gegevens ophalen uit de cache.

De meeste beheertaken worden uitgevoerd via de portal Management Azure en daarom veel van de administratieve opdrachten die beschikbaar zijn in de standaard versie van het bestand Vgx. niet beschikbaar zijn, met inbegrip van de mogelijkheid om de configuratie wijzigen via programmacode, afsluiten van de server bestand Vgx. aanvullende slaves of geforceerd gegevens opslaan op schijf configureren.

De portal Azure management bevat een handige grafische weergave waarmee u de prestaties van de cache controleren. Bijvoorbeeld kunt u het aantal verbindingen worden weergeven, het aantal aanvragen is uitgevoerd, het volume van de lees- en schrijfbewerkingen, en het aantal cache treffers en missers in cache. Met behulp van deze informatie, kunt u de effectiviteit van de cache bepalen en indien nodig naar een andere configuratie overschakelen of het beleid een verwijdering wijzigen. Bovendien kunt u waarschuwingen die e-mailberichten naar een beheerder verzenden als u een of meer essentiële maatstaven buiten een verwachte bereik valt. Bijvoorbeeld, als het aantal missers in cache groter is dan een opgegeven waarde in het laatste uur, kan een beheerder worden gewaarschuwd als de cache te klein zijn kan of gegevens worden dat u te snel verwijdert kunnen.

U kunt ook controleren CPU, geheugen en het netwerkgebruik voor de cache.

Voor meer informatie en voorbeelden waarin wordt getoond hoe maken en configureren van een Cache Azure bestand Vgx. Bezoek de [Lap rondom Azure bestand Vgx. Cache](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) op de Azure blog.

## <a name="caching-session-state-and-html-output"></a>Caching sessie- en HTML-uitvoer

Als u ASP.NET bouwen van webtoepassingen dat uitvoeren met behulp van Azure web rollen, kunt u opslaan session state-informatie en de HTML-uitvoer in een Cache Azure bestand Vgx.. De Provider sessie staat voor Azure bestand Vgx. Cache kunt u een sessiegegevens uitwisselen tussen verschillende exemplaren van een ASP.NET-webtoepassing en is erg handig in web-farm situaties waarbij de client / server-affiniteit is niet beschikbaar en sessie gegevens in het geheugen opslaan in de cache niet dienstig zou zijn.

De sessie staat Provider gebruiken met Azure bestand Vgx. Cache biedt verschillende voordelen, zoals:

- Deze kunt sessie staat onder een groot aantal exemplaren van een ASP.NET-webtoepassing, bieden verbeterde schaalbaarheid delen
- Gecontroleerde, gelijktijdige toegang tot dezelfde sessiestatusgegevens worden ondersteund voor meerdere lezers en een enkele schrijver en
- Deze compressie geheugen besparen en kunt de netwerkprestaties verbeteren.

Bezoek de [Provider voor Azure bestand Vgx. Cache voor ASP.NET sessie staat](redis-cache/cache-aspnet-session-state-provider.md) op de website van Microsoft voor meer informatie.

> [AZURE.NOTE] Gebruik niet de Provider sessie staat voor Azure bestand Vgx. Cache voor ASP.NET-toepassingen die worden uitgevoerd buiten de Azure-omgeving. De latentie van de toegang tot de cache van buiten Azure kunt elimineren de prestatievoordelen van gegevens in het cachegeheugen.

De Provider van de uitvoer-Cache voor Azure bestand Vgx. Cache op dezelfde manier kunt u de HTTP-antwoorden dat is gegenereerd door een ASP.NET-webtoepassing opslaan. Met behulp van de uitvoer-Cache Provider met Azure bestand Vgx. Cache kunt verbeteren de reactietijden van toepassingen waarmee complexe HTML-uitvoer; toepassingsexemplaren genereren dergelijke reacties kunnen maken gebruik van de uitvoer van gedeelde fragmenten in de cache in plaats van dit HTML-uitvoer steeds weer opnieuw genereren.  Ga naar de [ASP.NET uitvoer-Cache Provider voor Azure bestand Vgx. Cache](redis-cache/cache-aspnet-output-cache-provider.md) -pagina op de Microsoft-website voor meer informatie.

### <a name="azure-redis-cache"></a>Azure cache bestand Vgx.

Azure Cache bestand Vgx. biedt toegang tot het bestand Vgx. servers worden gehost in een datacenter Azure. Het fungeert als een gevel die toegangsbeheer en beveiliging biedt. U kunt een cache met behulp van de portal Azure inrichten.

De portal biedt een aantal vooraf gedefinieerde configuraties. Deze variëren van een 53 GB cache wordt uitgevoerd als een speciale service die SSL-communicatie (voor privacy) en ondersteunt master/slave replicatie met een SLA van de beschikbaarheid van 99,9%, naar een 25 0 MB cache zonder herhaling (geen waarborgen van de beschikbaarheid) uitvoert op gedeelde hardware.

Met behulp van de portal Azure, kunt u ook een verwijdering van de cache-beleid configureren en toegang tot de cache beheren door gebruikers toe te voegen aan de rollen geleverd.  Deze rollen waarin de bewerkingen die leden kunnen uitvoeren, zijn de eigenaar van Inzender en Reader. Bijvoorbeeld leden van de rol van eigenaar hebt volledige controle over de cache (inclusief beveiliging) en de inhoud van leden van de rol van Inzender kunnen lezen en schrijven van gegevens in de cache en leden van de rol van lezer kunnen alleen gegevens ophalen uit de cache.

De meeste beheertaken worden uitgevoerd via de portal Azure. Daarom zijn veel van de administratieve opdrachten die beschikbaar in de standaard versie van het bestand Vgx zijn. niet beschikbaar, waaronder de mogelijkheid om de configuratie wijzigen via programmacode, sluit het bestand Vgx. server, extra onderliggende shapes configureren of geforceerd gegevens opslaan op schijf.

De Azure portal bevat een handige grafische weergave waarmee u de prestaties van de cache controleren. U kunt bijvoorbeeld het aantal verbindingen worden gemaakt, het aantal aanvragen dat wordt uitgevoerd, de omvang van de lees- en schrijfbewerkingen, en het aantal treffers en missers in cache cache bekijken. Met deze informatie kunnen bepalen van de effectiviteit van de cache en overschakelen naar een andere configuratie of het beleid van een verwijdering wijzigen indien nodig.

Bovendien kunt u waarschuwingen die e-mailberichten naar een beheerder verzenden als u een of meer essentiële maatstaven buiten een verwachte bereik valt. U wilt bijvoorbeeld een beheerder een waarschuwing als het aantal missers in cache groter is dan een opgegeven waarde in het afgelopen uur omdat betekent mogelijk dat de cache te klein of gegevens kan zijn dat u verwijdert te snel.

U kunt ook controleren de CPU, geheugen en netwerkgebruik voor de cache.

Voor meer informatie en voorbeelden waarin wordt getoond hoe maken en configureren van een Cache Azure bestand Vgx. Bezoek de [Lap rondom Azure bestand Vgx. Cache](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) op de Azure blog.

## <a name="caching-session-state-and-html-output"></a>Caching sessie- en HTML-uitvoer

Als u bouwt ASP.NET-webtoepassingen dat uitvoeren met behulp van Azure web rollen, kun je een sessie-informatie en de HTML-uitvoer in een Cache Azure bestand Vgx. staat. De provider sessie staat voor Azure bestand Vgx. Cache kunt u een sessiegegevens uitwisselen tussen verschillende exemplaren van een ASP.NET-webtoepassing en is erg handig in web-farm situaties waarbij de client / server-affiniteit is niet beschikbaar en sessie gegevens in het geheugen opslaan in de cache niet dienstig zou zijn.

De sessie staat provider gebruiken met Azure bestand Vgx. Cache biedt verschillende voordelen, zoals:

- Status van de sessie delen met een groot aantal exemplaren van ASP.NET-webtoepassingen.
- Verbeterde schaalbaarheid biedt.
- Ondersteunende gecontroleerde, gelijktijdige toegang tot dezelfde sessiestatusgegevens voor meerdere lezers en een enkele schrijver.
- Met behulp van compressie geheugen besparen en de prestaties van het netwerk te verbeteren.

Bezoek de [provider voor Azure bestand Vgx. Cache voor ASP.NET sessie staat](redis-cache/cache-aspnet-session-state-provider.md) op de website van Microsoft voor meer informatie.

> [AZURE.NOTE] Gebruik niet de provider sessie staat voor Azure bestand Vgx. Cache met ASP.NET-toepassingen die worden uitgevoerd buiten de Azure-omgeving. De latentie van de toegang tot de cache van buiten Azure kunt elimineren de prestatievoordelen van gegevens in het cachegeheugen.

De provider van de uitvoer-cache voor Azure bestand Vgx. Cache op dezelfde manier kunt u de HTTP-antwoorden dat is gegenereerd door een ASP.NET-webtoepassing opslaan. De reactietijden van de toepassingen die complexe HTML-uitvoer weer kan worden verbeterd met behulp van de uitvoer-cache provider met Azure bestand Vgx. Cache. Toepassingsexemplaren die soortgelijke reacties te genereren kunnen u het gebruik van de uitvoer van gedeelde fragmenten in de cache in plaats van dit HTML-uitvoer steeds weer opnieuw genereren. Ga naar de [ASP.NET-uitvoer-cache provider voor Azure bestand Vgx. Cache](redis-cache/cache-aspnet-output-cache-provider.md) -pagina op de Microsoft-website voor meer informatie.

## <a name="building-a-custom-redis-cache"></a>Bouwen van een aangepaste cache voor het bestand Vgx.

Azure Cache bestand Vgx. fungeert als een gevel met de onderliggende bestand Vgx. servers. Momenteel ondersteunt een vaste set van configuraties maar biedt geen bestand Vgx. clustersysteem. Als u een geavanceerde configuratie die niet wordt gedekt door de cache Azure bestand Vgx. (zoals een groter zijn dan 53 GB cache) te bouwen en hosten van uw eigen servers bestand Vgx. met behulp van Azure virtual machines.

Dit is een mogelijk complex proces, omdat u maken verschillende VMs op te treden als master en onderliggende knooppunten wilt als u wilt dat om replicatie te implementeren. Bovendien, als u wenst te maken van een cluster, moet u meerdere modellen en de ondergeschikte servers. Een minimale geclusterde replicatietopologie die een hoge mate van beschikbaarheid en schaalbaarheid biedt omvat ten minste zes VMs georganiseerd als drie paren van master/slave-servers (een cluster moet ten minste drie master knooppunten bevatten).

Elk paar master/slave zich dicht bij elkaar bevinden om de Latentie minimaliseren. Elke set van paren kan echter worden uitgevoerd in verschillende Azure datacentra zich in verschillende regio's, als u wilt zoeken in cache opgeslagen gegevens dicht bij de toepassingen die waarschijnlijk om het te gebruiken. De pagina [Waarop het bestand Vgx. op een CentOS Linux VM in Azure](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) op de website van Microsoft helpt bij een voorbeeld van het maken en configureren van een bestand Vgx. knooppunt wordt uitgevoerd als een Azure VM.

[AZURE.NOTE] Houd er rekening mee dat als u uw eigen cache bestand Vgx. op deze manier worden geïmplementeerd, u verantwoordelijk bent voor toezicht, beheren en beveiligen van de service.

## <a name="partitioning-a-redis-cache"></a>Partitionering van een cache bestand Vgx.

Partitioneren van de cache bestaat uit de cache te splitsen over meerdere computers. Deze structuur biedt u verschillende voordelen boven met een met één cacheserver, met inbegrip van:

- Maken van een cache die is veel groter dan op één server kan worden opgeslagen.
- Verspreiden van gegevens over verbetering van de beschikbaarheid van servers. Als een server uitvalt of niet toegankelijk zijn, de gegevens die deze bevat is niet beschikbaar, maar de gegevens op de resterende servers nog steeds toegankelijk. Voor een cache is dit niet cruciaal omdat de gegevens in de cache is slechts een tijdelijke kopie van de gegevens die in een database worden vastgehouden. Gegevens op een server die niet toegankelijk zijn in de cache opgeslagen kan worden op een andere server in plaats daarvan.
- Verspreiding van de belasting over servers, waardoor betere prestaties en schaalbaarheid.
- Geolocating-gegevens sluit aan de gebruikers die toegang tot dit, dus korte wachttijden.

Voor een cache is de meest voorkomende vorm van partitionering sharding. In deze strategie is elke partitie (of shard) een cache bestand Vgx. in zijn eigen recht. Gegevens is gericht op een specifieke partitie via sharding logica die verschillende benaderingen voor het distribueren van de gegevens kunt gebruiken. Het [patroon Sharding](http://msdn.microsoft.com/library/dn589797.aspx) vindt u meer informatie over het implementeren van sharding.

Als u wilt implementeren in een cache bestand Vgx. partitioneren, kunt u een van de volgende manieren uitvoeren:

- _Server-side query routering._ Bij deze methode verzendt een clienttoepassing een aanvraag naar een bestand Vgx.-servers die bestaat uit de cache (waarschijnlijk de dichtstbijzijnde server). Elke server bestand Vgx. opgeslagen metagegevens die worden beschreven van de partitie die het bevat, en bevat ook informatie over welke partities bevinden zich op andere servers. Het bestand Vgx. server onderzoekt de aanvraag van de client. Als het kan lokaal worden opgelost, wordt het de gevraagde bewerking niet uitvoeren. Anders wordt de aanvraag op de juiste server doorgestuurd. Dit model wordt geïmplementeerd door het bestand Vgx. clustering en wordt meer gedetailleerd beschreven op de pagina [bestand Vgx. cluster zelfstudie](http://redis.io/topics/cluster-tutorial) op de website van bestand Vgx.. Bestand Vgx. clustering is transparant voor toepassingen en extra bestand Vgx. servers kunnen worden toegevoegd aan het cluster (en de gegevens opnieuw gepartitioneerd) zonder dat u de configuratie van de clients.

- _Client-side partitioneren._ In dit model, de clienttoepassing bevat logica (eventueel in de vorm van een bibliotheek) die aanvragen worden doorgestuurd naar het juiste bestand Vgx. server. Deze benadering kan worden gebruikt met Azure bestand Vgx. Cache. Meerdere Azure bestand Vgx. Caches (één voor elke gegevenspartitie) maken en implementeren van de client-side logica waarmee de aanvragen worden doorgestuurd naar de juiste cache. Als het partitieschema verandert (als extra Azure bestand Vgx. Caches worden gemaakt, bijvoorbeeld), moet client-toepassingen mogelijk opnieuw worden geconfigureerd.

- _Proxy ondersteunde partitioneren._ In dit schema bestand client-toepassingen verzenden aanvragen bij een tussenliggende proxyservice die begrijpt hoe de gegevens is gepartitioneerd en stuurt de aanvraag vervolgens door naar de juiste server Vgx.. Deze benadering kan ook worden gebruikt met Azure Cache bestand Vgx.; de proxy-service kan worden geïmplementeerd als een Azure cloud-service. Deze aanpak vereist een extra niveau van complexiteit bij de service implementeren en aanvragen duurt langer dan het gebruik van client-side partitioneren.

De pagina [partitionering: het opsplitsen van de gegevens tussen meerdere exemplaren bestand Vgx.](http://redis.io/topics/partitioning) op het bestand Vgx. website vindt u meer informatie over het implementeren van met het bestand Vgx. partitioneren.

### <a name="implement-redis-cache-client-applications"></a>Uitvoering van clienttoepassingen cache bestand Vgx.

Bestand Vgx. ondersteunt toepassingen die in verschillende programmeertalen zijn geschreven. Als u nieuwe toepassingen met behulp van het .NET Framework, is de aanbevolen werkwijze met client-bibliotheek van de StackExchange.Redis. Deze bibliotheek biedt een .NET Framework-objectmodel dat abstracts van de details voor verbinding met een server bestand Vgx., opdrachten verzenden en ontvangen van antwoorden. Is beschikbaar in Visual Studio als NuGet-pakket. Deze dezelfde bibliotheek kunt u verbinding maken met een Cache Azure bestand Vgx. of een aangepast bestand Vgx. cache die wordt gehost op een VM.

Verbinding maken met een server bestand Vgx. gebruikt u de statische `Connect` methode van de `ConnectionMultiplexer` klasse. Deze methode maakt u de verbinding is ontworpen om te worden gebruikt gedurende de levensduur van de clienttoepassing en de verbinding kan worden gebruikt door meerdere gelijktijdige threads. Niet opnieuw verbinding maken en verbreken wanneer u een bestand Vgx.-bewerking niet uitvoeren omdat dit kan de prestaties nadelig beïnvloeden.

U kunt de verbindingsparameters, bijvoorbeeld het adres van de host bestand Vgx. en het wachtwoord opgeven. Als u Azure bestand Vgx. Cache gebruikt, is het wachtwoord of de primaire of secundaire sleutel die is gegenereerd voor Azure bestand Vgx. Cache via de portal Management Azure.

Nadat u verbinding hebt gemaakt met de server bestand Vgx., kunt u een ingang voor het bestand Vgx.-database die als de cache fungeert. Het bestand Vgx. verbinding kunnen de `GetDatabase` methode om dit te doen. Vervolgens items uit de cache ophalen en opslaan van gegevens in de cache met behulp van de `StringGet` en `StringSet` methoden. Deze methoden verwachten een sleutel als parameter en retourneert het item in de cache met een overeenkomende waarde (`StringGet`) of het item toevoegen aan de cache met deze sleutel (`StringSet`).

Afhankelijk van de locatie van het bestand Vgx. server kunnen veel bewerkingen sommige vertraging oplopen terwijl een aanvraag wordt verzonden naar de server een antwoord naar de client wordt geretourneerd. De StackExchange bibliotheek biedt asynchrone versies van veel van de methoden die worden getoond om clienttoepassingen blijven reageren. Deze methoden ondersteunen de [Asynchrone patroon op basis van een taak](http://msdn.microsoft.com/library/hh873175.aspx) in het .NET Framework.

Het volgende codefragment ziet u een methode met de naam `RetrieveItem`. Dit illustreert een implementatie van de grond cache-patroon op basis van het bestand Vgx. en de StackExchange-bibliotheek. De methode wordt een string-waarde voor sleutel en probeert op te halen van het overeenkomstige item uit de cache bestand Vgx. door het aanroepen van de `StringGetAsync` methode (de asynchrone versie van `StringGet`).

Als het item niet wordt gevonden, wordt opgehaald uit de onderliggende gegevensbron met behulp van de `GetItemFromDataSourceAsync` methode (dit is een lokale methode en die geen deel uitmaakt van de StackExchange-bibliotheek). Wordt vervolgens toegevoegd aan de cache met behulp van de `StringSetAsync` methode zodat deze kan worden opgehaald sneller wanneer.

```csharp
// Connect to the Azure Redis cache
ConfigurationOptions config = new ConfigurationOptions();
config.EndPoints.Add("<your DNS name>.redis.cache.windows.net");
config.Password = "<Redis cache key from management portal>";
ConnectionMultiplexer redisHostConnection = ConnectionMultiplexer.Connect(config);
IDatabase cache = redisHostConnection.GetDatabase();
...
private async Task<string> RetrieveItem(string itemKey)
{
    // Attempt to retrieve the item from the Redis cache
    string itemValue = await cache.StringGetAsync(itemKey);

    // If the value returned is null, the item was not found in the cache
    // So retrieve the item from the data source and add it to the cache
    if (itemValue == null)
    {
        itemValue = await GetItemFromDataSourceAsync(itemKey);
        await cache.StringSetAsync(itemKey, itemValue);
    }

    // Return the item
    return itemValue;
}
```

De `StringGet` en `StringSet` methoden zijn niet beperkt tot het ophalen of string-waarden op te slaan. Items die als een matrix van bytes serienummer kunnen nemen. Als u een .NET-object op te slaan, kunt u deze als een bytestream serialiseren en gebruik de `StringSet` methode te schrijven naar de cache.

Op dezelfde manier kunt u een object uit de cache gelezen met behulp van de `StringGet` methode en het deserialisatie van een .NET-object. De volgende code toont een reeks uitbreidingsmethoden voor de IDatabase-interface (de `GetDatabase` methode van een bestand Vgx. verbinding wordt een `IDatabase` object), en sommige voorbeeldcode waarin deze methoden voor het lezen en schrijven een `BlogPost` object naar de cache:

```csharp
public static class RedisCacheExtensions
{
    public static async Task<T> GetAsync<T>(this IDatabase cache, string key)
    {
        return Deserialize<T>(await cache.StringGetAsync(key));
    }

    public static async Task<object> GetAsync(this IDatabase cache, string key)
    {
        return Deserialize<object>(await cache.StringGetAsync(key));
    }

    public static async Task SetAsync(this IDatabase cache, string key, object value)
    {
        await cache.StringSetAsync(key, Serialize(value));
    }

    static byte[] Serialize(object o)
    {
        byte[] objectDataAsStream = null;

        if (o != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream())
            {
                binaryFormatter.Serialize(memoryStream, o);
                objectDataAsStream = memoryStream.ToArray();
            }
        }

        return objectDataAsStream;
    }

    static T Deserialize<T>(byte[] stream)
    {
        T result = default(T);

        if (stream != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream(stream))
            {
                result = (T)binaryFormatter.Deserialize(memoryStream);
            }
        }

        return result;
    }
}
```

De volgende code illustreert een methode met de naam `RetrieveBlogPost` die deze uitbreidingsmethoden gebruikt om te lezen en schrijven een geserialiseerd `BlogPost` object naar de cache van de grond cache-patroon te volgen:

```csharp
// The BlogPost type
[Serializable]
private class BlogPost
{
    private HashSet<string> tags = new HashSet<string>();

    public BlogPost(int id, string title, int score, IEnumerable<string> tags)
    {
        this.Id = id;
        this.Title = title;
        this.Score = score;
        this.tags = new HashSet<string>(tags);
    }

    public int Id { get; set; }
    public string Title { get; set; }
    public int Score { get; set; }
    public ICollection<string> Tags { get { return this.tags; } }
}
...
private async Task<BlogPost> RetrieveBlogPost(string blogPostKey)
{
    BlogPost blogPost = await cache.GetAsync<BlogPost>(blogPostKey);
    if (blogPost == null)
    {
        blogPost = await GetBlogPostFromDataSourceAsync(blogPostKey);
        await cache.SetAsync(blogPostKey, blogPost);
    }

    return blogPost;
}
```

Bestand Vgx. ondersteunt de opdracht pipelining als een clienttoepassing meerdere asynchrone opdrachten verzendt. Bestand Vgx. kan de aanvragen met behulp van de verbinding in plaats van ontvangst en reageert op opdrachten in een reeks strikte multiplexen.

Deze aanpak voorkomt vertraging door efficiënter gebruik van het netwerk verminderen. Het volgende codefragment ziet u een voorbeeld waarin de details van de twee klanten tegelijkertijd opgehaald. De code dient twee aanvragen en vervolgens worden enkele andere verwerkt (niet weergegeven) voordat u wacht op de resultaten. De `Wait` methode van het object cache is vergelijkbaar met het .NET Framework `Task.Wait` methode:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
var task1 = cache.StringGetAsync("customer:1");
var task2 = cache.StringGetAsync("customer:2");
...
var customer1 = cache.Wait(task1);
var customer2 = cache.Wait(task2);
```

De pagina [Azure bestand Vgx. Cache-documentatie](https://azure.microsoft.com/documentation/services/cache/) op de website van Microsoft vindt u meer informatie over het schrijven van clienttoepassingen die de Azure bestand Vgx. Cache kunnen gebruiken. Meer informatie is beschikbaar op het [basisgebruik pagina](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) op de website StackExchange.Redis.

De pagina op de website van de [pijpleidingen en multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) vindt u meer informatie over asynchrone bewerkingen en pipelining met bestand Vgx. en de StackExchange-bibliotheek.  De volgende sectie in dit artikel, met behulp van bestand Vgx. Caching, vindt u voorbeelden van enkele van de meer geavanceerde technieken die u kunt toepassen op gegevens die worden vastgehouden in een cache bestand Vgx..

## <a name="using-redis-caching"></a>Met behulp van caching bestand Vgx.

Het eenvoudigste gebruik van het bestand Vgx. voor caching bezorgdheid is sleutel / waarde-paren waarvan de waarde is een niet-geïnterpreteerde reeks van willekeurige lengte dat binaire gegevens kan bevatten. (Het is in feite een matrix van bytes die kunnen worden behandeld als een tekenreeks). In dit scenario is geïllustreerd in de sectie implementeren bestand Vgx. Cache clienttoepassingen eerder in dit artikel.

Houd er rekening mee dat sleutels ook gegevens van niet-geïnterpreteerde, bevatten zodat u geen binaire gegevens als de sleutel kunt gebruiken. Hoe langer de sleutel is, des te meer ruimte het duurt om op te slaan en des te langer duurt het lookup-bewerkingen uit te voeren. Voor de bruikbaarheid en eenvoudig onderhoud uw keyspace zorgvuldig ontwerp en zinvolle (maar niet uitgebreid) toetsen gebruiken.

Gestructureerde toetsen zoals "klant": 100 bijvoorbeeld gebruiken voor het weergeven van de sleutel voor de klant met 100-ID in plaats van gewoon "100". Dit schema kunt u gemakkelijk onderscheid maken tussen de waarden van verschillende gegevenstypen worden opgeslagen. U kan bijvoorbeeld ook de sleutel "orders: 100" voor de sleutel voor de order met 100-ID gebruiken.

Eendimensionale binaire tekenreeksen, met uitzondering van kan een waarde in een bestand Vgx. sleutel-waardepaar ook bevatten meer gestructureerde gegevens, zoals lijsten, wordt ingesteld (gesorteerd en niet gesorteerd) en een hash-waarde. Bestand Vgx. biedt uitgebreide opdracht die dergelijke kunt manipuleren en veel van deze opdrachten zijn beschikbaar voor .NET Framework-toepassingen via een clientbibliotheek bijvoorbeeld StackExchange. De pagina [een inleiding tot het bestand Vgx. gegevenstypen en abstracties](http://redis.io/topics/data-types-intro) op de website van bestand Vgx. biedt een gedetailleerd overzicht van de opdrachten die u kunt ze bewerken en deze typen.

Deze sectie bevat een overzicht van enkele veelvoorkomende gevallen gebruik van deze gegevenstypen en opdrachten.

### <a name="perform-atomic-and-batch-operations"></a>Atomaire uitvoeren en batch-bewerkingen

Bestand Vgx. ondersteunt een reeks van atomaire get en set bewerkingen op de string-waarden. Deze bewerkingen verwijderen de race mogelijke gevaren die zich voordoen kunnen bij het gebruik van aparte `GET` en `SET` opdrachten. De bewerkingen die beschikbaar zijn onder andere:

- `INCR`, `INCRBY`, `DECR`, en `DECRBY`, die atomische verhogen of verlagen bewerkingen uitvoeren op gegevens numerieke waarden. De StackExchange bibliotheek biedt overbelaste versies van de `IDatabase.StringIncrementAsync` en `IDatabase.StringDecrementAsync` methoden voor het uitvoeren van deze bewerkingen en de resulterende waarde die is opgeslagen in de cache. Het volgende codefragment ziet u hoe u deze methoden gebruiken:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  await cache.StringSetAsync("data:counter", 99);
  ...
  long oldValue = await cache.StringIncrementAsync("data:counter");
  // Increment by 1 (the default)
  // oldValue should be 100

  long newValue = await cache.StringDecrementAsync("data:counter", 50);
  // Decrement by 50
  // newValue should be 50
  ```

- `GETSET`, waarbij de waarde die is gekoppeld aan een sleutel en wijzigingen aanbrengt in een nieuwe waarde wordt opgehaald. De bibliotheek van StackExchange kunt u deze bewerking beschikbaar via de `IDatabase.StringGetSetAsync` methode. Het onderstaande stukje code toont een voorbeeld van deze methode. Deze code geeft als resultaat de huidige waarde die is gekoppeld aan de sleutel 'gegevens: counter' uit het vorige voorbeeld. Vervolgens stelt de waarde voor deze sleutel opnieuw terug naar nul, allemaal als onderdeel van dezelfde bewerking:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  string oldValue = await cache.StringGetSetAsync("data:counter", 0);
  ```

- `MGET`en `MSET`, die of te wijzigen als een set tekenreekswaarden als een enkele bewerking. De `IDatabase.StringGetAsync` en `IDatabase.StringSetAsync` methoden overbelast zijn om deze functionaliteit, zoals in het volgende voorbeeld wordt getoond:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  // Create a list of key-value pairs
  var keysAndValues =
      new List<KeyValuePair<RedisKey, RedisValue>>()
      {
          new KeyValuePair<RedisKey, RedisValue>("data:key1", "value1"),
          new KeyValuePair<RedisKey, RedisValue>("data:key99", "value2"),
          new KeyValuePair<RedisKey, RedisValue>("data:key322", "value3")
      };

  // Store the list of key-value pairs in the cache
  cache.StringSet(keysAndValues.ToArray());
  ...
  // Find all values that match a list of keys
  RedisKey[] keys = { "data:key1", "data:key99", "data:key322"};
  RedisValue[] values = null;
  values = cache.StringGet(keys);
  // values should contain { "value1", "value2", "value3" }
  ```

U kunt ook meerdere bewerkingen in één transactie bestand Vgx. combineren, zoals beschreven in het bestand Vgx. transacties en batches sectie eerder in dit artikel. De StackExchange bibliotheek biedt ondersteuning voor transacties via de `ITransaction` interface.

U maakt een `ITransaction` object met behulp van de `IDatabase.CreateTransaction` methode. Aanroepen van opdrachten aan de transactie met behulp van de methoden die door de `ITransaction` object.

De `ITransaction` -interface biedt toegang tot een set methoden die vergelijkbaar is met die benaderd door de `IDatabase` interface, met dien verstande dat alle methoden asynchroon zijn. Dit betekent dat ze alleen worden uitgevoerd wanneer de `ITransaction.Execute` methode wordt aangeroepen. De waarde die wordt geretourneerd door de `ITransaction.Execute` methode geeft aan of de transactie is gemaakt (true) of als deze niet (false).

Het volgende codefragment toont een voorbeeld dat twee items verhoogd of verlaagd als onderdeel van één transactie:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
ITransaction transaction = cache.CreateTransaction();
var tx1 = transaction.StringIncrementAsync("data:counter1");
var tx2 = transaction.StringDecrementAsync("data:counter2");
bool result = transaction.Execute();
Console.WriteLine("Transaction {0}", result ? "succeeded" : "failed");
Console.WriteLine("Result of increment: {0}", tx1.Result);
Console.WriteLine("Result of decrement: {0}", tx2.Result);
```

Houd er rekening mee dat bestand Vgx. transacties in tegenstelling tot transacties in relationele databases zijn. De `Execute` methode wachtrijen alleen de opdrachten die bestaan uit de transactie moet worden uitgevoerd en de transactie gestopt als deze verkeerd is. Als u alle opdrachten in de wachtrij is geplaatst is, wordt elke opdracht asynchroon uitgevoerd.

Als de opdracht niet werkt, de andere gewoon kunnen blijven verwerken. Als u controleren of een opdracht is voltooid wilt, moet u de resultaten van de opdracht ophalen met behulp van de eigenschap **Result** van de bijbehorende taak, zoals in het bovenstaande voorbeeld. Lezen van de eigenschap **Result** blokkeren de thread nadat de taak is voltooid.

Zie de pagina [transacties in het bestand Vgx.](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) op de website StackExchange.Redis voor meer informatie.

Bij het uitvoeren van batchbewerkingen, kunt u de `IBatch` -interface van de StackExchange-bibliotheek. Deze interface biedt toegang tot een set methoden vergelijkbaar met de toegang tot de `IDatabase` interface, met dien verstande dat alle methoden asynchroon zijn.

U maakt een `IBatch` object met behulp van de `IDatabase.CreateBatch` methode en voert de batch met behulp van de `IBatch.Execute` methode, zoals in het volgende voorbeeld wordt getoond. Deze code gewoon stelt een string-waarde, verhoogd of verlaagd in het vorige voorbeeld gebruikt dezelfde items en de resultaten worden weergegeven:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
IBatch batch = cache.CreateBatch();
batch.StringSetAsync("data:key1", 11);
var t1 = batch.StringIncrementAsync("data:counter1");
var t2 = batch.StringDecrementAsync("data:counter2");
batch.Execute();
Console.WriteLine("{0}", t1.Result);
Console.WriteLine("{0}", t2.Result);
```

Het is belangrijk te begrijpen dat in tegenstelling tot een transactie als een opdracht in een batch is mislukt omdat het verkeerd ingedeelde, de andere opdrachten kunnen nog steeds worden uitgevoerd. De `IBatch.Execute` methode retourneert geen enkele indicatie van het slagen of mislukken.

### <a name="perform-fire-and-forget-cache-operations"></a>Uitvoeren van brand en vergeet cache bewerkingen

Bestand Vgx. ondersteunt brand en vergeet bewerkingen met behulp van opdracht-vlaggen. In dit geval de client gewoon een bewerking wordt gestart, maar heeft geen belang bij het resultaat en wacht niet totdat de opdracht is voltooid. In het volgende voorbeeld ziet u hoe de opdracht INCR als brand uitvoeren en vergeet bewerking:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
await cache.StringSetAsync("data:key1", 99);
...
cache.StringIncrement("data:key1", flags: CommandFlags.FireAndForget);
```

### <a name="specify-automatically-expiring-keys"></a>Automatisch verlopende sleutels opgeven

Wanneer u een item in een bestand Vgx. cache opslaat, kunt u een time-out waarna het item wordt automatisch verwijderd uit de cache. U kunt ook zoeken hoe veel meer tijd een sleutel is voordat deze via verloopt de `TTL` opdracht. Deze opdracht is beschikbaar voor StackExchange toepassingen met behulp van de `IDatabase.KeyTimeToLive` methode.

Het volgende codefragment ziet u hoe een verlooptijd van 20 seconden op een toets instellen en opvragen van de resterende levensduur van de sleutel:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration time of 20 seconds
await cache.StringSetAsync("data:key1", 99, TimeSpan.FromSeconds(20));
...
// Query how much time a key has left to live
// If the key has already expired, the KeyTimeToLive function returns a null
TimeSpan? expiry = cache.KeyTimeToLive("data:key1");
```

U kunt ook de verlooptijd instellen op een specifieke datum en tijd met de opdracht VERLOOPT, dat beschikbaar in de bibliotheek StackExchange is de `KeyExpireAsync` methode:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration date of midnight on 1st January 2015
await cache.StringSetAsync("data:key1", 99);
await cache.KeyExpireAsync("data:key1",
    new DateTime(2015, 1, 1, 0, 0, 0, DateTimeKind.Utc));
...
```

> _Tip:_ U kunt handmatig een item uit de cache verwijderen met de opdracht DEL, die beschikbaar via de bibliotheek StackExchange als is de `IDatabase.KeyDeleteAsync` methode.

### <a name="use-tags-to-cross-correlate-cached-items"></a>Tags worden gebruikt om cross-verwijzen van items in cache

Een bestand Vgx. set is een verzameling van meerdere items met één sleutel. U kunt een set maken met de opdracht SADD. Met de opdracht SMEMBERS kunt u de items in een set ophalen. De bibliotheek StackExchange de opdracht SADD en implementeert de `IDatabase.SetAddAsync` methode en de SMEMBERS-opdracht met de `IDatabase.SetMembersAsync` methode.

U kunt ook bestaande sets nieuwe sets maken met behulp van de SDIFF (set verschil), GESINTERD (set doorsnede) en SUNION (set Unie) opdrachten combineren. De StackExchange-bibliotheek bundelt deze bewerkingen in de `IDatabase.SetCombineAsync` methode. De eerste parameter voor deze methode bepaalt de set-bewerking uit te voeren.

De volgende codefragmenten weergeven hoe sets kunnen handig zijn voor het snel opslaan en ophalen van verzamelingen van gerelateerde items. Deze code wordt de `BlogPost` type dat is beschreven in de sectie implementeren bestand Vgx. Cache toepassingen die eerder in dit artikel.

A `BlogPost` -object bevat vier velden: een ID, een titel, een rangorde score en een verzameling tags. Het eerste codefragment hieronder toont de voorbeeldgegevens die wordt gebruikt voor het invullen van een C#-lijst van `BlogPost` objecten:

```csharp
List<string[]> tags = new List<string[]>()
{
    new string[] { "iot","csharp" },
    new string[] { "iot","azure","csharp" },
    new string[] { "csharp","git","big data" },
    new string[] { "iot","git","database" },
    new string[] { "database","git" },
    new string[] { "csharp","database" },
    new string[] { "iot" },
    new string[] { "iot","database","git" },
    new string[] { "azure","database","big data","git","csharp" },
    new string[] { "azure" }
};

List<BlogPost> posts = new List<BlogPost>();
int blogKey = 0;
int blogPostId = 0;
int numberOfPosts = 20;
Random random = new Random();
for (int i = 0; i < numberOfPosts; i++)
{
    blogPostId = blogKey++;
    posts.Add(new BlogPost(
        blogPostId,               // Blog post ID
        string.Format(CultureInfo.InvariantCulture, "Blog Post #{0}",
            blogPostId),          // Blog post title
        random.Next(100, 10000),  // Ranking score
        tags[i % tags.Count]));   // Tags--assigned from a collection
                                  // in the tags list
}
```

U kunt de codes opslaan voor elke `BlogPost` als een set in een cache bestand Vgx. object en elke set koppelen aan de ID van de `BlogPost`. Hiermee kunt snel zoeken naar alle codes die deel uitmaken van een specifiek blogbericht van een toepassing. Als u wilt zoeken in de tegengestelde richting inschakelen en alle blogberichten die delen van een specifieke tag zoeken, kunt u een andere set met blogberichten verwijst naar de label-ID in de sleutel:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Tags are easily represented as Redis Sets
foreach (BlogPost post in posts)
{
    string redisKey = string.Format(CultureInfo.InvariantCulture,
        "blog:posts:{0}:tags", post.Id);
    // Add tags to the blog post in Redis
    await cache.SetAddAsync(
        redisKey, post.Tags.Select(s => (RedisValue)s).ToArray());

    // Now do the inverse so we can figure how which blog posts have a given tag
    foreach (var tag in post.Tags)
    {
        await cache.SetAddAsync(string.Format(CultureInfo.InvariantCulture,
            "tag:{0}:blog:posts", tag), post.Id);
    }
}
```

Deze structuren kunnen u veel algemene query's zeer efficiënt uitvoeren. U kunt bijvoorbeeld zoeken en weergeven van de tags voor het blogbericht 1 als volgt:

```csharp
// Show the tags for blog post #1
foreach (var value in await cache.SetMembersAsync("blog:posts:1:tags"))
{
    Console.WriteLine(value);
}
```

U vindt alle codes die gemeenschappelijk voor de blog zijn 1 en blog post 2 door het uitvoeren van een bewerking van het snijpunt instellen als volgt boeken:

```csharp
// Show the tags in common for blog posts #1 and #2
foreach (var value in await cache.SetCombineAsync(SetOperation.Intersect, new RedisKey[]
    { "blog:posts:1:tags", "blog:posts:2:tags" }))
{
    Console.WriteLine(value);
}
```

En vindt u alle blogberichten die een specifieke tag bevatten:

```csharp
// Show the ids of the blog posts that have the tag "iot".
foreach (var value in await cache.SetMembersAsync("tag:iot:blog:posts"))
{
    Console.WriteLine(value);
}
```

### <a name="find-recently-accessed-items"></a>Zoeken naar recentelijk geopende items

Een algemene taak van de vele toepassingen vereist is om te zoeken naar de meest recent gebruikte items. Een blog site wil bijvoorbeeld informatie over de meest recent gelezen blog-updates weergeven.

U kunt deze functionaliteit implementeren met behulp van een bestand Vgx. lijst. Een bestand Vgx. lijst bevat verschillende items met dezelfde sleutel. De lijst fungeert als een tweepuntige wachtrij. U kunt items aan beide uiteinden van de lijst met behulp van de LPUSH (push links) en RPUSH (juiste push) opdrachten push. U kunt items begin of einde van de lijst ophalen met behulp van de opdrachten LPOP en RPOP. U kunt ook een reeks elementen terugkeren met behulp van de opdrachten LRANGE en SCHIKKEN.

De volgende codefragmenten weergeven hoe u deze bewerkingen kan uitvoeren met behulp van de bibliotheek StackExchange. Deze code wordt de `BlogPost` type uit de voorgaande voorbeelden. Als u een blogbericht wordt gelezen door een gebruiker, de `IDatabase.ListLeftPushAsync` methode duwt de titel van het blogbericht op een lijst die is gekoppeld aan de sleutel in de cache voor het bestand Vgx. "blog:recent_posts".

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:recent_posts";
BlogPost blogPost = ...; // Reference to the blog post that has just been read
await cache.ListLeftPushAsync(
    redisKey, blogPost.Title); // Push the blog post onto the list
```

Meer blogberichten worden gelezen, worden de titels op dezelfde lijst geduwd. De lijst is gesorteerd op de volgorde waarin de titels zijn toegevoegd. De meest recent gelezen blogberichten zijn naar de linkerkant van de lijst. (Als het dezelfde blogbericht meer dan één keer lezen is, heeft dit meerdere vermeldingen in de lijst.)

U kunt de titels van de meest recent gelezen berichten weergeven met behulp van de `IDatabase.ListRange` methode. Deze methode heeft de sleutel met de lijst, een beginpunt en een eindpunt. De volgende code haalt de titels van de 10 blogberichten (artikelen van 0 tot en met 9) aan het einde links van de lijst:

```csharp
// Show latest ten posts
foreach (string postTitle in await cache.ListRangeAsync(redisKey, 0, 9))
{
    Console.WriteLine(postTitle);
}
```

Houd er rekening mee dat de `ListRangeAsync` methode worden items niet verwijderen uit de lijst. Hiervoor kunt u de `IDatabase.ListLeftPopAsync` en `IDatabase.ListRightPopAsync` methoden.

Om te voorkomen dat de lijst voor onbepaalde tijd groeien, kunt u regelmatig items ten door de lijst. Het onderstaande stukje code ziet u hoe alles verwijderen, maar de vijf meest linkse items uit de lijst:

```csharp
await cache.ListTrimAsync(redisKey, 0, 5);
```

### <a name="implement-a-leader-board"></a>Een leader board implementeren

Standaard worden de items in een set niet vastgehouden in een bepaalde volgorde. U kunt een geordende verzameling maken met de opdracht ZADD (de `IDatabase.SortedSetAdd` -methode in de bibliotheek StackExchange). De artikelen worden besteld met behulp van een numerieke waarde met de naam score wordt geleverd als een parameter voor de opdracht.

Het volgende codefragment wordt de titel van een blog als een geordende lijst toegevoegd. In dit voorbeeld wordt is elk blogbericht veld een score met de positie van het blogbericht.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:post_rankings";
BlogPost blogPost = ...; // Reference to a blog post that has just been rated
await cache.SortedSetAddAsync(redisKey, blogPost.Title, blogpost.Score);
```

U kunt ophalen de blog post titels en scores in oplopende volgorde score met behulp van de `IDatabase.SortedSetRangeByRankWithScores` methode:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(redisKey))
{
    Console.WriteLine(post);
}
```

> [AZURE.NOTE] De StackExchange bibliotheek biedt ook de `IDatabase.SortedSetRangeByRankAsync` methode, die de gegevens retourneert in volgorde van de score, maar niet de scores weer.

U kunt ook items in aflopende volgorde van scores ophalen en beperken het aantal items dat wordt geretourneerd door middel van aanvullende parameters voor de `IDatabase.SortedSetRangeByRankWithScoresAsync` methode. In het volgende voorbeeld worden de titels en scores van de top 10 gerangschikte blog-updates:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(
                               redisKey, 0, 9, Order.Descending))
{
    Console.WriteLine(post);
}
```

In het volgende voorbeeld wordt de `IDatabase.SortedSetRangeByScoreWithScoresAsync` methode, die u gebruiken kunt om de artikelen die worden geretourneerd die binnen een bepaalde score vallen te beperken bereik:

```csharp
// Blog posts with scores between 5000 and 100000
foreach (var post in await cache.SortedSetRangeByScoreWithScoresAsync(
                               redisKey, 5000, 100000))
{
    Console.WriteLine(post);
}
```

### <a name="message-by-using-channels"></a>Bericht met behulp van kanalen

Fungeert als een gegevenscache, met uitzondering van zorgt een bestand Vgx. server voor berichtafhandeling via een krachtige publisher/abonnee mechanisme. Clienttoepassingen kunnen zich abonneren op een kanaal en andere toepassingen of services berichten publiceren naar het kanaal. Abonneren toepassingen deze berichten ontvangt en deze verwerken.

Bestand Vgx. biedt de opdracht ABONNEREN voor clienttoepassingen gebruiken om u te abonneren op kanalen. Met deze opdracht verwacht de naam van een of meer kanalen waarop de toepassing berichten accepteert. Bevat de bibliotheek StackExchange de `ISubscription` -interface kunt een .NET Framework-toepassing abonneren en publiceren naar kanalen.

U maakt een `ISubscription` object met behulp van de `GetSubscriber` methode van de verbinding met de server bestand Vgx.. En u naar berichten die op een kanaal met behulp van luistert de `SubscribeAsync` methode van dit object. In het volgende voorbeeld ziet u hoe u zich abonneert op een kanaal met de naam "berichten: blogPosts":

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
await subscriber.SubscribeAsync("messages:blogPosts", (channel, message) =>
{
    Console.WriteLine("Title is: {0}", message);
});
```

De eerste parameter voor de `Subscribe` methode is de naam van het kanaal. Deze naam volgt dezelfde conventies die worden gebruikt door de sleutels in de cache. De naam mag binaire gegevens, maar het is raadzaam ervoor te zorgen dat de goede prestaties en onderhoud met relatief korte, duidelijke tekenreeksen.

U ziet ook dat de naamruimte die wordt gebruikt door de kanalen door sleutels die losstaat van. Dit betekent dat er kanalen en sleutels die dezelfde naam hebben, maar het is mogelijk uw toepassingscode moeilijker dat te onderhouden.

De tweede parameter is een gemachtigde actie. Deze gemachtigde asynchroon uitgevoerd telkens wanneer een nieuw bericht wordt weergegeven op het kanaal. In dit voorbeeld wordt het bericht gewoon op de console (het bericht bevat de titel van een blog).

Als u wilt publiceren naar een kanaal, kunt een toepassing gebruiken met de opdracht Publiceren bestand Vgx. De bibliotheek StackExchange biedt de `IServer.PublishAsync` methode voor het uitvoeren van deze bewerking. Het volgende codefragment ziet u hoe een bericht publiceren naar het kanaal "berichten: blogPosts":

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
BlogPost blogpost = ...;
subscriber.PublishAsync("messages:blogPosts", blogPost.Title);
```

Er zijn enkele punten die u over het mechanisme voor publiceren/abonneren weten moet:

- Meerdere abonnees kunnen zich abonneren op hetzelfde kanaal en ze alle ontvangen berichten die in dat kanaal worden gepubliceerd.
- Abonnees ontvangen alleen berichten die zijn gepubliceerd nadat zij zich hebben geabonneerd. Kanalen zijn niet gebufferd en als een bericht is gepubliceerd, de infrastructuur bestand Vgx. duwt het bericht aan elke abonnee en wordt deze verwijderd.
- Standaard worden berichten ontvangen door abonnees in de volgorde waarin ze zijn verzonden. In een zeer actieve systeem met een groot aantal berichten en groot aantal abonnees en uitgevers kunt gegarandeerde sequentiële bezorging van berichten vertragen van het systeem. Als elk bericht onafhankelijk is en de volgorde niet belangrijk is, kunt u de gelijktijdige verwerking inschakelen door het bestand Vgx. systeem die helpen kan bij het verbeteren van de respons. Dit kunt u bereiken in een client StackExchange door de PreserveAsyncOrder van de verbinding die wordt gebruikt door de abonnee op false instellen:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
redisHostConnection.PreserveAsyncOrder = false;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
```

## <a name="related-patterns-and-guidance"></a>Gerelateerde patronen en richtlijnen

Het volgende patroon kan ook relevant zijn voor uw scenario wanneer u caching in uw toepassingen implementeert:

- [Aanleg van cache-patroon](http://msdn.microsoft.com/library/dn589799.aspx): dit patroon wordt beschreven hoe u gegevens op aanvraag laden in een cache van een gegevensarchief. Dit patroon kunt u ook de consistentie tussen gegevens die in het cachegeheugen worden vastgehouden en de gegevens in het oorspronkelijke gegevensarchief.
- Het [patroon Sharding](http://msdn.microsoft.com/library/dn589797.aspx) vindt u informatie over het implementeren van horizontaal partitioneren om te verbeteren, schaalbaarheid bij de opslag en toegang tot grote hoeveelheden gegevens.

## <a name="more-information"></a>Meer informatie

- De [klasse MemoryCache](http://msdn.microsoft.com/library/system.runtime.caching.memorycache.aspx) pagina op de website van Microsoft
- De pagina [Azure bestand Vgx. Cache-documentatie](https://azure.microsoft.com/documentation/services/cache/) op de website van Microsoft
- De [Azure bestand Vgx. Cache FAQ](redis-cache/cache-faq.md) -pagina op de website van Microsoft
- De pagina [configuratie-model](http://msdn.microsoft.com/library/windowsazure/hh914149.aspx) op de website van Microsoft
- De pagina [Asynchrone patroon taak gebaseerd](http://msdn.microsoft.com/library/hh873175.aspx) op de website van Microsoft
- De pagina [pijpleidingen en multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) op de StackExchange.Redis GitHub repo
- Het [bestand Vgx. persistentie](http://redis.io/topics/persistence) pagina op de website van bestand Vgx.
- De [replicatie-pagina](http://redis.io/topics/replication) op de website van bestand Vgx.
- De pagina [bestand Vgx. cluster zelfstudie](http://redis.io/topics/cluster-tutorial) op de website van bestand Vgx.
- De [partitionering: het opsplitsen van de gegevens tussen meerdere exemplaren bestand Vgx.](http://redis.io/topics/partitioning) pagina op de website van bestand Vgx.
- De pagina [Met behulp van het bestand Vgx. Als een Cache Recentelijk](http://redis.io/topics/lru-cache) op de website van bestand Vgx.
- De [transacties](http://redis.io/topics/transactions) -pagina op de website van bestand Vgx.
- De pagina [bestand Vgx. beveiliging](http://redis.io/topics/security) op de website van bestand Vgx.
- De [Lap rondom Azure bestand Vgx. Cache](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) -pagina op de Azure blog
- De pagina [Waarop het bestand Vgx. op een CentOS Linux VM in Azure](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) op de website van Microsoft
- De [provider voor ASP.NET sessie staat voor Azure bestand Vgx. Cache](redis-cache/cache-aspnet-session-state-provider.md) -pagina op de website van Microsoft
- De [ASP.NET-uitvoer-cache provider voor Azure bestand Vgx. Cache](redis-cache/cache-aspnet-output-cache-provider.md) -pagina op de website van Microsoft
- De pagina [An Introduction to bestand Vgx. gegevenstypen en abstracties](http://redis.io/topics/data-types-intro) op de website van bestand Vgx.
- De [voornaamste gebruiksmogelijkheden](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) pagina op de website van StackExchange.Redis
- De pagina [transacties in het bestand Vgx.](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) op de StackExchange.Redis repo
- De [Data partities guide](http://msdn.microsoft.com/library/dn589795.aspx) op de website van Microsoft

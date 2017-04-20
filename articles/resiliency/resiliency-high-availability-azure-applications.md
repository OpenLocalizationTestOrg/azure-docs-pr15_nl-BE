<properties
   pageTitle="Hoge beschikbaarheid voor Azure toepassingen | Microsoft Azure"
   description="Technisch overzicht en gedetailleerde informatie over het ontwerpen en bouwen van toepassingen voor hoge beschikbaarheid op Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="high-availability-for-applications-built-on-microsoft-azure"></a>Hoge beschikbaarheid voor toepassingen die zijn gebouwd op Microsoft Azure

Een maximaal beschikbare toepassing absorbeert schommelingen in de beschikbaarheid, laden en tijdelijke storingen in de hardware en de afhankelijke services. De toepassing blijft werken om een acceptabele gebruiker en antwoord systemische niveau, zoals gedefinieerd door de zakelijke vereisten of toepassing service level agreements (Sla's).

##<a name="azure-high-availability-features"></a>Azure functies voor hoge beschikbaarheid

Azure heeft veel ingebouwde functies die ondersteuning bieden voor toepassingen met maximale beschikbaarheid. In deze sectie worden enkele van de belangrijkste functies beschreven. Zie voor een uitgebreidere analyse van het platform [Azure resiliency technische richtsnoeren](./resiliency-technical-guidance.md).

###<a name="fabric-controller"></a>Fabric-controller

De controller Azure fabric bepalingen en monitoren de conditie van de Azure compute exemplaren. De controller fabric controleert de status van de hardware en software van de host en Gast machine exemplaren. Wanneer er een fout wordt ontdekt, wordt automatisch verplaatst de instanties VM serviceovereenkomsten afgedwongen. Het concept van probleem- en domeinen verder ondersteunt de compute SLA.

Wanneer u meerdere exemplaren van de Service Cloud rol worden geïmplementeerd, implementeert Azure gevallen veroorzaakt op verschillende domeinen. De grens van een fout met betrekking tot domein is in feite een rack van andere hardware in hetzelfde gebied. Fout met betrekking tot domeinen verminderen de kans dat de service van een toepassing wordt onderbroken door een gelokaliseerde hardwarefout opgetreden. Het nummer van de fout met betrekking tot domeinen die zijn toegewezen aan de werknemer of web-rollen kunt u niet beheren. De controller fabric gebruikt specifieke bronnen die losstaan van Azure gehoste toepassingen. Omdat het dient als de kern van het systeem Azure heeft uptime van 100 procent. Deze worden gecontroleerd en beheerd rol exemplaren in verschillende domeinen veroorzaakt.

Het volgende diagram toont de Azure gedeelde bronnen dat de controller fabric implementeert en in fout met betrekking tot andere domeinen beheert.

![Vereenvoudigde weergave van domeinisolatie veroorzaakt](./media/resiliency-high-availability-azure-applications/fault-domain-isolation.png)

Upgraden van domeinen zijn vergelijkbaar met de fout met betrekking tot domeinen in functie, maar ondersteuning van upgrades in plaats van fouten. Een domein upgraden is een logische eenheid exemplaar scheiding die bepaalt welke exemplaren in een bepaalde service op een punt in de tijd worden bijgewerkt. Standaard voor uw implementatie gehoste service zijn vijf upgraden domeinen gedefinieerd. Echter, kunt u die waarde in het definitiebestand. Stel bijvoorbeeld dat u beschikt over acht exemplaren van uw web. Er zijn twee exemplaren in de drie domeinen voor upgrade en twee exemplaren in een bepaald domein upgraden. Azure bepaalt de volgorde van de update, maar het gebaseerd op het aantal domeinen upgraden. Zie voor meer informatie over het upgraden van domeinen, [een cloud service bijwerken](../cloud-services/cloud-services-update-azure-service.md).

###<a name="features-in-other-services"></a>Functies in andere services

Naast de platforms die ondersteuning bieden voor hoge compute beschikbaarheid, sluit Azure functies voor hoge beschikbaarheid in de andere services. Azure opslag heeft bijvoorbeeld drie kopieën van alle blob-, tabel- en wachtrijgegevens. U kunt ook de optie voor het opslaan van back-ups van BLOB's en tabellen in een gebied met secundaire geo-replicatie. De Azure Content Delivery Network kan BLOB's in de cache opgeslagen over de hele wereld voor schaalbaarheid en redundantie. Azure SQL-Database houdt ook meerdere replica's.

Naast de [technische begeleiding van tolerantie](https://aka.ms/bctechguide) reeks artikelen, Zie de [Aanbevolen procedures voor het ontwerpen van Large-Scale Services op Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/) papier. Deze documenten bieden een diepere discussie over de beschikbaarheid van Azure platform.

Azure biedt verschillende functies die ondersteuning bieden voor hoge beschikbaarheid, is het belangrijk te begrijpen hun beperkingen:

- Voor het berekenen garandeert Azure dat uw rollen beschikbaar en worden uitgevoerd zijn, maar hebben als uw toepassing uitgevoerd of overladen wordt.
- Voor Azure SQL-Database, gegevens synchroon gerepliceerd binnen het gebied. U kunt actieve geo-replicatie kunt u maximaal vier extra kopieën in hetzelfde gebied (of de verschillende regio's). Deze database replica's zijn geen point-in-time back-ups. SQL-databases bieden mogelijkheden voor point-in-time back-up. Lees voor meer informatie over de mogelijkheden van SQL-Database point-in-time, [Azure SQL Database punt in tijd herstellen](https://azure.microsoft.com/blog/azure-sql-database-point-in-time-restore/).
- Als de blob-gegevens worden gerepliceerd voor opslag van Azure, standaard naar een andere regio. Echter, u geen toegang tot de replica's totdat Microsoft wil een failover uitvoeren naar de andere site. Een storing regio alleen in het geval van een langdurige onderbrekingen van het hele gebied en er is geen SLA voor geo-failover-tijd. Het is ook belangrijk te weten dat beschadigde gegevens snel verspreidt zich de replica's.

Om deze redenen, moet u de beschikbaarheid van het platform met specifieke functies aanvullen. Toepassingsspecifieke beschikbaarheid bevatten de functie blob momentopname wilt maken van de point-in-time back-ups van blob-gegevens.

###<a name="availability-sets-for-azure-virtual-machines"></a>Beschikbaarheid stelt voor Azure virtuele Machines

Het grootste gedeelte van dit artikel ligt de nadruk op cloud-services, die een platform als een service (PaaS)-model. Er zijn echter ook bepaalde beschikbaarheid voor Azure virtuele Machines, die een infrastructuur als servicemodel voor (IaaS gebruikt). Voor hoge beschikbaarheid met virtuele Machines, moet u beschikbaar stelt. Een set beschikbaarheid dient een soortgelijke functie probleem- en domeinen. Binnen een set beschikbaarheid plaatst Azure de virtuele machines op een manier die voorkomt dat gelokaliseerde hardwarestoringen en onderhoudsactiviteiten waardoor alle computers in die groep. Beschikbaarheid sets zijn vereist voor de Azure SLA voor de beschikbaarheid van virtuele Machines te bereiken.

In het volgende diagram is een weergave van twee sets van de beschikbaarheid van die groep websites en virtuele machines voor SQL Server, respectievelijk.

![Beschikbaarheid stelt voor Azure virtuele Machines](./media/resiliency-high-availability-azure-applications/availability-set-for-azure-virtual-machines.png)

>[AZURE.NOTE] In het voorgaande diagram is SQL Server geïnstalleerd en wordt uitgevoerd op virtuele machines. Dit is anders dan de vorige discussie van Azure SQL-Database, een database als een beheerde service biedt.

##<a name="application-strategies-for-high-availability"></a>Toepassing van strategieën voor maximale beschikbaarheid

Strategieën voor de meeste toepassingen voor hoge beschikbaarheid omvatten redundantie of het verwijderen van harde afhankelijkheden tussen componenten. Ontwerp van toepassingen moet fouttolerantie ondersteunen tijdens enkel geval uitvaltijd van Azure of services van andere leveranciers. De volgende secties worden de patronen van toepassing voor het bevorderen van de beschikbaarheid van de cloud-services.

###<a name="asynchronous-communication-and-durable-queues"></a>Asynchrone communicatie en duurzame wachtrijen

U kunt voor asynchrone communicatie tussen losjes gekoppelde services om de beschikbaarheid in Azure toepassingen te verhogen. In dit patroon, berichten naar opslag wachtrijen of wachtrijen voor latere verwerking Azure Service Bus te schrijven. Bij het schrijven van het bericht naar de wachtrij, keert onmiddellijk terug naar de afzender van het bericht. Een andere laag van de toepassing is verantwoordelijk voor het bericht verwerkt, gewoonlijk geïmplementeerd als een functie van de werknemer. Als de rol van de werknemer zich begeeft, verzamelt de berichten in de wachtrij totdat de service voor verwerking is hersteld. Als de wachtrij beschikbaar is, is er geen directe afhankelijkheid tussen front-end-zender en de processor van de. Hierdoor wordt de eis voor synchrone serviceverzoeken die een knelpunt doorvoer in gedistribueerde toepassingen kunnen worden.

Een variatie Azure opslag (BLOB's, tabellen, wachtrijen) of Service Bus wachtrijen als failover-locatie voor mislukte databaseaanroepen gebruikt. Bijvoorbeeld mislukt een synchrone aanroepen vanuit een toepassing naar een andere service (zoals Azure SQL-Database) herhaaldelijk. U kunt mogelijk gegevens serialiseren in duurzame opslag. Op een later moment wanneer de service of de database weer on line is versturen de toepassing opnieuw het verzoek van de opslag. Het verschil in dit model is de locatie is niet een constant deel van de werkstroom toepassing. Het wordt alleen gebruikt in scenario's voor storingen.

In beide scenario's, asynchrone communicatie en tussentijdse opslag te voorkomen dat een uitgevallen back-end-service waardoor u de gehele toepassing. Wachtrijen fungeren als een logische intermediair. Zie voor meer informatie over het kiezen van de juiste wachtrijservice [Azure wachtrijen en wachtrijen met Azure Service Bus--vergeleken en tegenstelling tot](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

###<a name="fault-detection-and-retry-logic"></a>Fout met betrekking tot opsporing en probeer het opnieuw logica

Een belangrijk punt in het ontwerp van toepassingen maximaal beschikbaar is logic code opnieuw met een service die tijdelijk niet beschikbaar is zonder problemen te verwerken. Het [Tijdelijke fout afhandeling toepassing blokkeren](https://msdn.microsoft.com/library/hh680934.aspx), ontwikkeld door het team van Microsoft Patterns and Practices, helpt ontwikkelaars van toepassingen in dit proces. Het woord "tijdelijk": een tijdelijke toestand die slechts voor een relatief korte tijd duurt. In het kader van dit artikel is tijdelijke fouten afhandelen deel van een maximaal beschikbare toepassing ontwikkelen. Voorbeelden van tijdelijke situaties zijn netwerk af en toe fouten en verloren databaseverbindingen.

De tijdelijke fout afhandeling toepassing blokkeren is een vereenvoudigde manier voor het verwerken van storingen in uw code op een correcte manier. U kunt het verbeteren van de beschikbaarheid van uw toepassingen door krachtige tijdelijke fout afhandeling logica toe te voegen. In de meeste gevallen logica opnieuw verwerkt de korte onderbreking en de afzender en de ontvanger na mislukte pogingen voor een of meer hersteld. Een succesvolle nieuwe poging gaat meestal ongemerkt naar gebruikers van toepassingen.

Ontwikkelaars hebben drie opties voor het beheer van hun logica opnieuw: incrementele, vaste interval, en exponentieel. Incrementele wacht opnieuw langer voordat elk op een toenemende lineaire wijze (bijvoorbeeld 1, 2, 3 en 4 seconden). Vast interval wacht de dezelfde hoeveelheid tijd tussen nieuwe pogingen (bijvoorbeeld 2 seconden). Voor een meer willekeurige optie wacht de exponentiële back-off langer tussen nieuwe pogingen. Het wordt echter exponentiële gedrag (bijvoorbeeld 2, 4, 8 en 16 seconden).

De strategie op hoog niveau in uw code is:

1. Uw strategie voor opnieuw proberen en beleid definiëren.
1. Voer de bewerking die leiden een tijdelijke fout tot kan.
1. Als een tijdelijke fout zich voordoet, het beleid opnieuw worden aangeroepen.
1. Als alle pogingen mislukken, werkelijk een laatste uitzondering.

Test uw logica opnieuw gesimuleerde storingen om ervoor te zorgen dat nieuwe pogingen op opeenvolgende verrichtingen niet in een onverwachte langdurige vertraging resulteren in. Dit doen voordat u besluit de algemene opdracht mislukt.

###<a name="reference-data-pattern-for-high-availability"></a>Verwijzing gegevenspatroon voor maximale beschikbaarheid

Verwijzingsgegevens zijn de gegevens alleen-lezen van een toepassing. Deze gegevens vindt u de zakelijke context waarbinnen de toepassing transactionele gegevens in de loop van een bedrijfsactiviteit genereert. Transactionele gegevens is een point-in-time-functie van de verwijzingsgegevens. Daarom de integriteit ervan is afhankelijk van de momentopname van de referentiegegevens op het moment van de transactie. Dit is een beetje losse definitie, maar het moet voldoende zijn voor ons doel hier.

Referentiegegevens in de context van een toepassing is nodig voor de werking van de toepassing. De respectieve toepassingen maken en beheren van referentiegegevens; master data management (MDM) systemen uitvoeren vaak deze functie. Deze systemen zijn verantwoordelijk voor de gehele levenscyclus van de verwijzingsgegevens. Voorbeelden van verwijzingsgegevens zijn productcatalogus, werknemer master, model voor delen en apparatuur model. Verwijzingsgegevens kunnen ook afkomstig zijn van buiten de organisatie, bijvoorbeeld postcodes of btw-tarieven. Strategieën voor verbetering van de beschikbaarheid van verwijzingsgegevens zijn doorgaans minder moeilijk zijn dan die voor transactionele gegevens. Referentiegegevens heeft het voordeel van voornamelijk onveranderbaar.

U kunt Azure web en werknemer rollen die tijdens runtime autonome referentiegegevens verbruiken door het implementeren van de verwijzingsgegevens samen met de toepassing. Als de grootte van de lokale opslag dergelijk systeem toestaat, is dit een ideale staat. Ingesloten databases (SQL, NoSQL) of XML-bestanden die zijn geïmplementeerd op een lokaal bestandssysteem zal helpen met de autonomie van Azure compute schaaleenheden. U moet wel een mechanisme voor het bijwerken van de gegevens in elke rol zonder opnieuw installeren. Hiertoe plaatst u de updates van de verwijzingsgegevens naar een cloud opslag eindpunt (bijvoorbeeld Azure Blob-opslag of SQL-Database). Code toevoegen aan elke rol die u de Gegevensupdates in de compute nodes bij het opstarten van de rol downloadt. U kunt ook programmacode toevoegen die kan een beheerder voor het uitvoeren van een geforceerde download in de rol van exemplaren.

Om de beschikbaarheid te verhogen, moeten de rollen ook een set van referentiegegevens bevatten als opslag is niet beschikbaar. Hierdoor zijn de rollen om te beginnen met een basis van referentiegegevens totdat de resource opslag beschikbaar voor de updates.

![Beschikbaarheid van toepassing via de autonome computerknooppunten](./media/resiliency-high-availability-azure-applications/application-high-availability-through-autonomous-compute-nodes.png)

Een overweging van dit patroon is de implementatie en de snelheid van opstarten voor de rollen. Hierdoor kan de hoeveelheid tijd die nodig is voor het draaien van nieuwe installaties of rol exemplaren toenemen als implementeren of downloaden van grote hoeveelheden verwijzingsgegevens bij het opstarten. Dit is mogelijk een aanvaardbare verhouding voor de autonomie van de verwijzingsgegevens onmiddellijk beschikbaar op elke rol hebben in plaats van afhankelijk van de opslagservices van externe.

###<a name="transactional-data-pattern-for-high-availability"></a>Transactionele gegevenspatroon voor hoge beschikbaarheid

Transactionele gegevens zijn de gegevens waarmee de toepassing wordt gegenereerd in een zakelijke context. Transactionele gegevens is een combinatie van de set bedrijfsprocessen die de toepassing wordt geïmplementeerd en de referentiegegevens die deze processen worden ondersteund. Voorbeelden van transactionele gegevens kunnen orders, geavanceerde aankondigingen van verzending, facturen en klant relatie management (CRM) mogelijkheden zijn. De transactionele gegevens dus gegenereerd zal worden gevoed met externe systemen voor registratiedoeleinden of voor verdere verwerking.

Houd er rekening mee dat verwijzing gegevens binnen de systemen die verantwoordelijk voor deze gegevens zijn kunt wijzigen. Om deze reden moet transactionele gegevens de gegevenscontext referentie punt in tijd opslaan zodat er een minimale externe afhankelijkheden voor de semantische consistentie. Neem bijvoorbeeld het verwijderen van een product uit de catalogus van een paar maanden nadat een order is uitgevoerd. De beste manier is om zo veel mogelijk context verwijst gegevens als haalbaar insluiten in de transactie. Hierdoor blijft de semantiek van de transactie, zelfs als de referentiegegevens worden gewijzigd nadat de transactie is vastgelegd.

Zoals eerder vermeld, lenen architecturen die gebruikmaken van losse koppeling en asynchrone communicatie zich voor de hogere niveaus van beschikbaarheid. Dit geldt voor transactionele gegevens ook, maar de uitvoering is complexer. Traditionele transactionele begrippen zijn meestal afhankelijk van de database voor de transactie. Als u tussenliggende lagen introduceren, moet de gegevens op verschillende lagen te zorgen voor voldoende consistentie en duurzaamheid goed verwerkt door de toepassingscode.

Een workflow die het vastleggen van transactionele gegevens van de verwerking gescheiden een beschrijving van de volgende volgorde:

1. Web-computerknooppunt: deze verwijzen naar gegevens.
1. Externe opslag: tussenliggende transactionele gegevens opslaan.
1. Web-computerknooppunt: Voltooi de transactie van de eindgebruiker.
1. Web-computerknooppunt: voltooide transactionele gegevens, samen met de context verwijst gegevens verzenden naar duurzame tijdelijke die gegarandeerd een voorspelbare reactie geven.
1. Web-computerknooppunt: signaal van de eindgebruiker-voltooiing van de transactie.
1. Achtergrond knooppunt berekenen: de transactionele gegevens ophalen en verzenden naar de laatste opslaglocatie in het huidige systeem verwerk deze indien nodig.

Het volgende diagram ziet één mogelijke tenuitvoerlegging van dit ontwerp in een gehoste Azure cloud-service.

![Hoge beschikbaarheid door middel van losse koppeling](./media/resiliency-disaster-recovery-high-availability-azure-applications/application-high-availability-through-loose-coupling.png)

De onderbroken de pijlen in het voorgaande diagram geven asynchrone verwerking. De rol van front-is niet op de hoogte van deze asynchrone verwerking. Dit leidt tot de opslag van de transactie op de eindbestemming ten opzichte van het huidige systeem. Als gevolg van de vertraging die dit model asynchrone introduceert, is transactionele gegevens niet onmiddellijk beschikbaar zijn voor de query. Daarom moet elke eenheid van de transactionele gegevens worden opgeslagen in een cache of een sessie van de gebruiker om te voldoen aan de gebruikersinterface van direct nodig heeft.

De Webrol is autonoom van de rest van de infrastructuur. De van beschikbaarheidsprofiel is een combinatie van de Webrol en de Azure wachtrij en niet de hele infrastructuur. Deze benadering kan naast hoge beschikbaarheid, de rol van de webpagina voor het horizontaal schalen onafhankelijk van de back-end opslagruimte. Dit model met hoge beschikbaarheid kan invloed hebben op de economische aspecten van bewerkingen. Aanvullende onderdelen zoals Azure wachtrijen en rollen van de werknemer kunnen invloed hebben op de maandelijkse gebruikskosten.

Houd er rekening mee dat het vorige diagram ziet u één implementatie van dit losgekoppelde benadering van transactionele gegevens. Er zijn veel mogelijke implementaties. De volgende lijst bevat enkele alternatieven:

 * De rol van een werknemer kan worden geplaatst tussen de Webrol en de wachtrij voor de opslag.
 * Een wachtrij Service Bus kan worden gebruikt in plaats van een wachtrij Azure opslag.
 * De uiteindelijke bestemming mogelijk Azure-opslag of een andere databaseprovider.
 * Azure-Cache kan worden gebruikt op de weblaag om caching vereisten na de transactie.

###<a name="scalability-patterns"></a>Schaalbaarheid patronen

Het is belangrijk te weten dat de schaalbaarheid van de cloud-service rechtstreeks betrekking heeft op beschikbaarheid. Als de toegenomen belasting zorgt ervoor dat de service niet meer reageert, is de indruk van de gebruiker de toepassing niet actief is. Volg de aanbevolen procedures voor schaalbaarheid op basis van de verwachte toepassing laden en toekomstige verwachtingen. De hoogste schaal omvat vele overwegingen, zoals het gebruik van een enkele waarde versus meerdere opslag accounts delen in meerdere databases en caching strategieën. Zie [Aanbevolen procedures voor het ontwerpen van Large-Scale Services op Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)voor een diepgaande blik op deze patronen.

##<a name="next-steps"></a>Volgende stappen

Dit artikel maakt deel uit van een serie artikelen gericht op [herstel na storingen en hoge beschikbaarheid voor toepassingen die zijn gebouwd op Microsoft Azure](./resiliency-disaster-recovery-high-availability-azure-applications.md). Het volgende artikel in deze serie is [herstel na noodgevallen voor toepassingen die zijn gebouwd op Microsoft Azure](./resiliency-disaster-recovery-azure-applications.md).

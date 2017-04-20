<properties
   pageTitle="Herstel na noodgevallen voor Azure toepassingen | Microsoft Azure"
   description="Technisch overzicht en gedetailleerde informatie over het ontwerpen van toepassingen voor noodherstel op Microsoft Azure."
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

#<a name="disaster-recovery-for-applications-built-on-microsoft-azure"></a>Herstel na noodgevallen voor toepassingen die zijn gebouwd op Microsoft Azure

Overwegende dat hoge beschikbaarheid informatie over het beheer van tijdelijke storing is, is noodherstel (DR) het onherstelbare verlies van functionaliteit. Neem bijvoorbeeld het scenario waarbij een regio uitvalt. In dit geval moet u uw toepassing uitvoert of toegang tot uw gegevens buiten de regio Azure een plan hebben. Uitvoering van dit plan betrekking heeft op mensen, processen en ondersteunende toepassingen waarmee het systeem werkt. De eigenaren van bedrijven en technologie die de operationele modus van het systeem voor een ramp definiëren bepalen eveneens het niveau van functionaliteit voor de service tijdens een ramp. Het niveau van functionaliteit kan een paar vormen aannemen: volledig uitschakelen, gedeeltelijk beschikbaar (verslechterd functionaliteit of verwerking wordt vertraagd), of volledig beschikbaar.

##<a name="azure-disaster-recovery-features"></a>Azure disaster recovery-functies

Net als bij de beschikbaarheid van overwegingen, heeft Azure [resiliency technische richtsnoeren](./resiliency-technical-guidance.md) die is ontworpen ter ondersteuning van herstel na noodgevallen. Er is ook een relatie tussen sommige van de beschikbaarheidfuncties van Azure en noodherstel. Het beheer van functies voor fouttolerantie domeinen neemt bijvoorbeeld de beschikbaarheid van een toepassing. Zonder dat het management, zou een hardwarefout niet-verwerkte worden van een scenario "disaster". Zodat de juiste toepassing van de van beschikbaarheidsfuncties en strategieën een belangrijk onderdeel van de ramp proofing is uw toepassing. Echter, dit artikel gaat verder dan problemen met de algemene beschikbaarheid op ernstige (en zeldzame) ramp gebeurtenissen.

##<a name="multiple-datacenter-regions"></a>Meerdere regio's het datacenter

Azure onderhoudt datacenters in veel regio's over de hele wereld. Deze infrastructuur ondersteunt verschillende disaster recovery scenario's, zoals het systeem geo-replicatie van Azure opslag tot secundaire gebieden. Het betekent ook dat u gemakkelijk en goedkoop een cloud-service naar verschillende locaties over de hele wereld implementeren kunt. Vergelijk dit met de kosten en moeite van het uitvoeren van uw eigen datacenters in meerdere regio's. Gegevens en -services implementeren op meerdere gebieden voorkomen dat uw toepassing grote storingen in een bepaalde regio.

##<a name="azure-traffic-manager"></a>Azure verkeer Manager

Wanneer een regio-specifieke fout optreedt, moet u verkeer omleiden naar services of implementaties in een ander gebied. Kunt u het bewerkingsplan handmatig doen, maar het is efficiënter gebruik van een geautomatiseerd proces. Azure verkeer Manager is ontworpen voor deze taak. U kunt het automatisch beheren van failover van de gebruikersverkeer naar een andere regio in het geval de primaire regio mislukt. Omdat verkeer management een belangrijk onderdeel van de algemene strategie is, is het belangrijk dat u de basisbeginselen van verkeer Manager.

In het volgende diagram worden de gebruikers verbinding met een URL die is opgegeven voor het verkeer Manager (__http://myATMURL.trafficmanager.net__) en de samenvattingen die de werkelijke URL's (__http://app1URL.cloudapp.net__ en __http://app2URL.cloudapp.net__). Op basis van hoe u de criteria voor configureren wanneer u aan de route-gebruikers, wordt gebruikers verzonden naar de juiste site werkelijke wanneer het beleid bepaalt. De beleidsopties zijn round-robin, prestaties of failover. Voor dit artikel zijn wij betrokken met alleen de failover-optie.

![Routering via Azure verkeer Manager](./media/resiliency-disaster-recovery-azure-applications/routing-using-azure-traffic-manager.png)

Bij het beheer van verkeer configureren, kunt u een nieuwe Manager verkeer DNS-voorvoegsel opgeven. Dit is de URL-voorvoegsel dat u aan uw gebruikers toegang krijgen tot uw service. Beheer van netwerkverkeer abstracts nu één niveau omhoog en niet op het regioniveau taakverdeling. De DNS Manager verkeer wordt toegewezen aan een CNAME voor alle implementaties die u beheert.

Binnen verkeer Manager geeft u de prioriteit van de installaties die aan gebruikers worden doorgestuurd wanneer een fout optreedt. Verkeer Manager bewaakt de eindpunten van de implementaties en notities wanneer de primaire implementatie mislukt. Bij storing, verkeer Manager analyseert de prioriteitenlijst van implementaties en gebruikers worden doorgestuurd naar de volgende dia in de lijst.

Hoewel het verkeer Manager waar u in een failover-besluit, kunt u bepalen of uw domein failover slapende of actief is terwijl u niet in failover-modus bent. Deze functionaliteit heeft niets te maken met Azure verkeer Manager. Beheer van verkeer detecteert een storing in de primaire site en de failover-site over rolt. Beheer van netwerkverkeer rolt over ongeacht of deze site is momenteel voor de gebruikers of niet.

Raadpleeg voor meer informatie over de werking van Azure verkeer Manager:

 * [Beheer van netwerkverkeer-overzicht](../traffic-manager/traffic-manager-overview.md)
 * [Failover-methode configureren](../traffic-manager/traffic-manager-configure-failover-routing-method.md)

##<a name="azure-disaster-scenarios"></a>Azure herstel na storing scenario 's

De volgende secties beschreven voor verschillende soorten herstel na storing scenario's. Verstoring van de hele regio service zijn niet de enige oorzaak voor het mislukken van de gehele toepassing. Slecht ontwerp of administratieve fouten kunnen ook leiden tot storingen. Het is belangrijk rekening te houden met de mogelijke oorzaken van een fout tijdens de ontwerp- en testfase van het herstelplan. Een goed plan maakt gebruik van Azure functies en wordt ze met specifieke strategieën. Het gekozen antwoord wordt bepaald door het belang van de toepassing, de doelstelling van herstel punt (vrijgegeven Productieorder) en de doelstelling van herstel tijd (RTO).

###<a name="application-failure"></a>Toepassingsfout

Azure verkeer Manager verwerkt automatisch, mislukken als gevolg van de onderliggende hardware of besturingssysteem software in de virtuele hostcomputer. Azure maakt een nieuw exemplaar van de rol van een werkende server en voegt deze toe aan de rotatie van de verdeling van de belasting. Als er meer dan één rol exemplaren, verplaatst Azure verwerking bij het vervangen van het knooppunt naar de andere actieve rol exemplaren.

Er zijn ernstige fouten die onafhankelijk van de hardware of besturingssysteem fouten gebeuren. De toepassing kan mislukken als gevolg van de fatale uitzonderingen veroorzaakt door onjuiste logica of problemen met data integriteit. U moet voldoende telemetrie integreren in de code, zodat een systeem van toezicht kan detecteren van storingen en contact op met de toepassingsbeheerder van een. Een beheerder die beschikt over volledige kennis van de processen disaster recovery kunt u een besluit aan te roepen van een failover-procedure. Een beheerder kan ook gewoon een storing beschikbaarheid om de kritieke fouten op te lossen.

###<a name="data-corruption"></a>Beschadiging van gegevens

Azure slaat uw gegevens Azure SQL-Database en Azure opslag driemaal toch toe in fout met betrekking tot andere domeinen in hetzelfde gebied. Als u de geo-replicatie gebruikt, kunnen de gegevens extra driemaal worden opgeslagen in een andere regio. Echter, als uw gebruikers of uw toepassing beschadigd door de gegevens in de primaire versie, de gegevens snel wordt gerepliceerd naar de andere kopieën. Helaas, dit resulteert in drie exemplaren van beschadigde gegevens.

Mogelijke beschadiging van uw gegevens beheren, hebt u twee opties. Ten eerste kunt u een aangepaste back-upstrategie beheren. U kunt de back-ups opslaan in Azure of op-lokalen, afhankelijk van uw zakelijke behoeften of governance-regelingen. Een andere mogelijkheid is het gebruik van de nieuwe restore point-in-time-optie voor het herstellen van een SQL-database. Zie de sectie op [gegevens strategieën voor herstel na noodgevallen](#data-strategies-for-disaster-recovery)voor meer informatie.

###<a name="network-outage"></a>Netwerkstoring

Wanneer gedeelten van de Azure netwerk niet toegankelijk zijn, kunt u mogelijk niet voor uw toepassing of gegevens opvragen. Als een of meer exemplaren van de rol niet beschikbaar wegens netwerkproblemen, Azure maakt gebruik van de resterende beschikbare exemplaren van uw toepassing. Als uw toepassing geen toegang de gegevens door een netwerkstoring Azure tot, kunt u mogelijk in gedegradeerde modus uitvoeren lokaal met behulp van de gegevens in de cache. U moet aan het ontwerp van het noodherstelplan voor slechter in uw toepassing wordt uitgevoerd. Voor sommige toepassingen, kan dit niet praktisch zijn.

Een andere mogelijkheid is om gegevens op een andere locatie opgeslagen totdat de verbinding is hersteld. Als niet slechter een optie is, worden de resterende opties uitvaltijd van toepassingen of failover naar een ander gebied. Het ontwerp van een toepassing wordt uitgevoerd in gedegradeerde modus is zo veel mogelijk een zakelijke beslissing als een technische. Dit wordt besproken in de sectie op de [functionaliteit van de toepassing verslechterd](#degraded-application-functionality)verder.

###<a name="failure-of-a-dependent-service"></a>Storing van een afhankelijke service.

Azure biedt tal van services die periodieke uitvaltijd kunnen ondervinden. [Azure bestand Vgx. Cache](https://azure.microsoft.com/services/cache/) beschouwen als een voorbeeld. Deze service met meerdere huurder biedt mogelijkheden voor uw toepassing. Het is belangrijk rekening te houden met wat er gebeurt in uw toepassing als afhankelijke service niet beschikbaar is. Dit scenario is in veel opzichten vergelijkbaar met het scenario voor het netwerk uitvalt. Overwegende echter elke service afzonderlijk leidt tot mogelijke verbeteringen van het algehele plan.

Azure bestand Vgx. Cache levert caching aan uw toepassing vanuit de cloud service implementeren die disaster recovery voordelen biedt. Eerst de service wordt nu uitgevoerd op rollen die lokaal zijn voor de implementatie. Daarom kun je je beter bewaken en beheren van de status van de cache als onderdeel van de algemene processen voor de cloud-service. Dit type cache beschrijft ook nieuwe functies. Een van de nieuwe functies is de beschikbaarheid van gegevens in de cache. Hiermee kunt gegevens in de cache wordt behouden als een enkel knooppunt mislukt door dubbele exemplaren op andere knooppunten.

Houd er rekening mee dat hoge beschikbaarheid, verkleint u de doorvoer en latentie vergroot door het bijwerken van de secundaire kopie bij het schrijven. Dat ook de hoeveelheid geheugen die wordt gebruikt voor elk item wordt verdubbeld, dus voor dat plan. In dit specifieke voorbeeld wordt elke afhankelijke service wellicht mogelijkheden ter verbetering van de algemene beschikbaarheid en weerstand tegen kritieke fouten optreden.

Met elke afhankelijke service, moet u begrijpen de gevolgen van een verstoring van de service. In het voorbeeld in de cache mogelijk toegang tot de gegevens rechtstreeks in een database totdat u het cachegeheugen herstellen. In termen van prestaties slechter zou zijn, maar zou bieden volledige functionaliteit met betrekking tot de gegevens.

###<a name="region-wide-service-disruption"></a>Onderbrekingen van het hele gebied

De vorige storingen zijn voornamelijk storingen die kunnen worden beheerd in hetzelfde gebied Azure. U moet echter ook de mogelijkheid dat er sprake is van een verstoring van de service van het gehele gebied voorbereiden. Als een hele regio onderbrekingen optreedt, zijn de lokale kopieën van uw gegevens niet beschikbaar. Als u de geo-replicatie hebt ingeschakeld, zijn er drie extra exemplaren van de BLOB's en tabellen in een andere regio. Als Microsoft de regio verloren verklaart, remaps Azure alle DNS-vermeldingen op de regio geo gerepliceerd.

>[AZURE.NOTE] Let erop dat u niet elke controle over dit proces hebt en deze doet zich alleen voor de hele regio onderbrekingen. Daarom moet u vertrouwen op een andere toepassingsspecifieke back-upstrategieën te bereiken van het hoogste niveau van beschikbaarheid. Zie de sectie op [gegevens strategieën voor herstel na noodgevallen](#data-strategies-for-disaster-recovery)voor meer informatie.

###<a name="azure-wide-service-disruption"></a>Onderbrekingen van het hele Azure

In de planning voor noodgevallen, moet u rekening houden met het volledige bereik van mogelijke rampen. Een van de meest ernstige verstoring van de service zou betrekking hebben op alle gebieden van Azure tegelijk. Net als bij andere serviceonderbrekingen, misschien dat u in dat geval het risico van downtime tijdelijke zult neemt. Ruime serviceonderbrekingen die regio's omvatten moeten veel zeldzame dan geïsoleerde serviceonderbrekingen met betrekking afhankelijke services of enkele regio's tot worden.

Echter voor bepaalde essentiële toepassingen kunnen u besluiten dat er een back-upplan ook in dit scenario moet zijn. Het plan voor deze gebeurtenis kan bevatten niet via services in een [alternatieve cloud](#alternative-cloud) of een [hybride van lokalen en cloud oplossing](#hybrid-on-premises-and-cloud-solution).

###<a name="degraded-application-functionality"></a>Functionaliteit van de slechtere toepassing

Een goed ontworpen toepassing wordt meestal voorzien van een verzameling van modules die met elkaar via de toepassing van informatie-uitwisseling Losse patronen communiceren. Een toepassing DR-vriendelijke moet scheiding van taken op moduleniveau. Dit is om te voorkomen dat de verstoring van een afhankelijke service waardoor u de gehele toepassing. Neem bijvoorbeeld een webtoepassing commerce voor bedrijf Y. De volgende modules kunnen inhouden van de toepassing:

 * __Productcatalogus__ kunnen gebruikers bladeren producten.
 * __Winkelwagentje__ kunnen gebruikers producten in de winkelwagen toevoegen/verwijderen.
 * __Status van bestelling__ geeft de verzendstatus van orders van de gebruiker.
 * __Indienen van de order__ wordt de winkelsessie door het indienen van de order met de betaling.
 * __Orderverwerking__ valideert de volgorde voor de integriteit van gegevens en een aantal beschikbaarheidscontrole uitvoert.

Wanneer u een afhankelijke van een module in deze toepassing uitvalt, hoe de module werkt totdat dat deel hersteld? Een goede architectuur systeem geïmplementeerd isolatie grenzen door scheiding van taken zowel in de ontwerpfase als tijdens runtime. U kunt elke storing terug te vorderen en niet-herstelbare categoriseren. Niet-herstelbare fouten zal nemen naar de module, maar kan het risico van een onherstelbare fout door alternatieven. Zoals besproken in de sectie met hoge beschikbaarheid, kunt u enkele problemen van gebruikers verbergen door het afhandelen van fouten en het nemen van andere acties. Bij een ernstige verstoring van de service kan de toepassing niet volledig beschikbaar. Een derde optie is echter blijven onderhouden gebruikers in gedegradeerde modus.

Als de database voor het hosten van orders uitvalt, verliest de module orderverwerking, bijvoorbeeld de mogelijkheid om de verwerking van verkooptransacties. De architectuur misschien het moeilijk of zelfs onmogelijk voor het indienen van de Order en orderverwerking onderdelen van de toepassing om verder te gaan. Als de toepassing niet is ontworpen voor het verwerken van dit scenario, kan de gehele toepassing off line gaan.

In dit hetzelfde scenario is het echter mogelijk dat de productgegevens op een andere locatie is opgeslagen. In dat geval kan de productcatalogus module nog steeds worden gebruikt voor het weergeven van producten. De toepassing blijft in gedegradeerde modus beschikbaar voor gebruikers van de beschikbare functies zoals de productcatalogus weergeven. Andere delen van de toepassing, zijn echter niet beschikbaar is, zoals query's bestellen of de voorraad.

Een andere variatie van slechter gericht op prestaties in plaats van mogelijkheden. Neem bijvoorbeeld een scenario waarbij de productcatalogus in de cache via Azure bestand Vgx. Cache. Als in de cache opslaan niet beschikbaar is, kan de toepassing gaat rechtstreeks naar de storage server product catalogusgegevens op te halen. Maar deze toegang mogelijk langzamer dan de versie in de cache. Hierdoor worden de prestaties van toepassingen is verslechterd totdat de cache-service volledig is hersteld.

Bepalen hoeveel van een toepassing blijft de functie in gedegradeerde modus is zowel een zakelijke beslissing en een technische beslissing. De toepassing moet ook bepalen hoe de gebruikers van de tijdelijke problemen. In dit voorbeeld wordt de toepassing mogelijk producten bekijken en zelfs deze toe te voegen aan uw winkelwagen. Echter, wanneer de gebruiker probeert een aankoop te doen, de toepassing wordt de gebruiker gewaarschuwd dat de module sales tijdelijk niet toegankelijk is. Het is niet ideaal voor de klant, maar voorkomt dat een verstoring van de gehele toepassing service.

##<a name="data-strategies-for-disaster-recovery"></a>Gegevens van strategieën voor noodherstel

Correct verwerken van gegevens is het moeilijkst gebied direct in gaan. Gegevens herstellen is ook het gedeelte van het herstelproces wordt meestal de meeste tijd. Verschillende opties in de afbraak resulteert in moeilijke uitdagingen om gegevens te herstellen van systeemfouten en consistentie na een storing.

Een van de factoren is de noodzaak om te zetten of een kopie van de gegevens van de toepassing. U deze gegevens gebruikt voor verwijzing en transactionele toepassing op een secundaire site. Een instelling op ruimten moet een dure en langdurige planningsproces voor het implementeren van een noodherstelplan meerdere regio. De meeste cloud-aanbieders, waaronder Azure, kunnen gemakkelijk gemakkelijk de implementatie van toepassingen voor meerdere regio's. Deze gebieden worden geografisch verspreid op zodanige wijze dat onderbrekingen meerdere regio extreem zeldzame moet worden. De strategie voor het verwerken van gegevens tussen de regio's is een van de factoren voor het succes van een calamiteitenplan.

In de volgende secties besproken disaster recovery technieken gerelateerd aan back-ups van gegevens, referentiegegevens en transactionele gegevens.

###<a name="backup-and-restore"></a>Back-up en terugzetten

Regelmatig back-ups van toepassingsgegevens, kunnen sommige herstel na storing scenario's ondersteunen. Van verschillende opslagbronnen vereisen verschillende technieken.

Voor de niveaus Basic, Standard en Premium SQL-Database, kunt u profiteren van een point-in-time herstel van de database herstellen. Zie voor meer informatie [Overzicht: business continuity en database noodherstel met SQL-Database van de wolk](../sql-database/sql-database-business-continuity.md). Een andere mogelijkheid is Active Geo-replicatie gebruikt voor SQL-Database. Dit repliceert automatisch wijzigingen in de database met secundaire databases in hetzelfde gebied, Azure of zelfs in een andere regio Azure. Dit biedt een mogelijke alternatief voor enkele van de meer handmatige data synchronisatie technieken in dit artikel beschreven. Zie voor meer informatie [Overzicht: SQL Database actief Geo-replicatie](../sql-database/sql-database-geo-replication-overview.md).

U kunt ook meer handmatig gebruiken voor back-up en terugzetten. De opdracht COPY DATABASE gebruiken voor het maken van een kopie van de database. U moet deze opdracht gebruiken om een back-up met transactionele consistentie. Ook kunt u de service voor importeren/exporteren van Azure SQL-Database. Dit ondersteunt databases exporteren Bacpac-bestanden die zijn opgeslagen in Azure Blob-opslag.

De ingebouwde redundantie van Azure opslag maakt twee replica's van het back-upbestand in hetzelfde gebied. De frequentie van de back-up bepaalt echter uw vrijgegeven Productieorder, wordt de hoeveelheid gegevens die in herstel na storing scenario's kunnen verloren gaan. Stel bijvoorbeeld dat u een back-up aan het begin van het uur en twee minuten vóór het begin van het uur door een ramp zich voordoet. U verliest 58 minuten dat is gebeurd na de laatste reservekopie is gemaakt. Ter bescherming tegen een hele regio onderbrekingen, moet u ook Bacpac-bestanden kopiëren naar een andere regio. Vervolgens hebt u de mogelijkheid van het terugzetten van de back-ups in de andere regio. Zie voor meer informatie [Overzicht: business continuity en database noodherstel met SQL-Database van de wolk](../sql-database/sql-database-business-continuity.md).

U kunt voor opslag van Azure, ontwikkel uw eigen aangepaste back-up of gebruikt u een van de vele back-upprogramma's van derden. Houd er rekening mee dat de meeste toepassingen ontwerpen extra complexiteit waar opslagbronnen verwijzen naar elkaar. Neem bijvoorbeeld een SQL-database met een kolom die is gekoppeld aan een blob in Azure opslag. Als de back-ups niet tegelijkertijd worden uitgevoerd, is de database mogelijk de aanwijzer naar een blob die geen back-up is vóór de storing. De toepassing of het calamiteitenplan moet implementeren van processen voor het afhandelen van deze inconsistentie na een herstel.

###<a name="reference-data-pattern-for-disaster-recovery"></a>Verwijzing gegevenspatroon voor noodherstel

Verwijzingsgegevens zijn alleen-lezen gegevens die functies ondersteunt. Deze meestal niet vaak worden gewijzigd. Hoewel back-up en terugzetten is een methode voor het verwerken van de hele regio serviceonderbrekingen, is de RTO relatief lang. Wanneer u de toepassing naar een secundaire regio implementeert, kunnen u sommige strategieën de RTO voor referentiegegevens verbeteren.

Omdat wijzigingen in gegevens verwijzen naar de RTO kunt u af en toe door het bijhouden van een kopie van de referentiegegevens in de secundaire regio verbeteren. Hierdoor wordt de benodigde tijd voor het terugzetten van back-ups in het geval van een ramp. Om te voldoen aan meerdere regio disaster recovery, moet u de toepassing en de referentiegegevens samen in meerdere regio's implementeren. Zoals vermeld in de [verwijzing gegevenspatroon voor hoge beschikbaarheid](./resiliency-high-availability-azure-applications.md#reference-data-pattern-for-high-availability), kunt u verwijzingen naar de functie zelf, naar een extern opslagmedium of naar een combinatie van beide implementeren.

Het model gegevens implementatie binnen computerknooppunten is impliciet voldoet aan de eisen van disaster recovery. Verwijzing gegevens implementeren met SQL-Database moet u een kopie van de verwijzingsgegevens implementeren voor elke regio. De dezelfde strategie is van toepassing op Azure opslag. U kunt een kopie van een verwijzing naar gegevens die zijn opgeslagen in Azure opslag naar de primaire en secundaire gebieden moet implementeren.

![Verwijzing naar de primaire en secundaire gebieden publicatie](./media/resiliency-disaster-recovery-azure-applications/reference-data-publication-to-both-primary-and-secondary-regions.png)

U moet uw eigen toepassingsspecifieke back-upplan voor alle gegevens, met inbegrip van referentiegegevens implementeren. Geo-gerepliceerde kopieën tussen de regio's worden alleen gebruikt in een hele regio onderbrekingen. Om te voorkomen dat een langdurige storingstijd, de essentiële onderdelen van de toepassingsgegevens op de secundaire regio te implementeren. Zie voor een voorbeeld van deze topologie de [actief-passief model](#active-passive).

###<a name="transactional-data-pattern-for-disaster-recovery"></a>Transactionele gegevenspatroon voor noodherstel

Uitvoering van de strategie van een volledig functionele ramp modus vereist asynchrone replicatie van de transactionele gegevens naar het secundaire regio. Kenmerken van de vrijgegeven Productieorder van de toepassing bepaalt de praktische tijdvensters waarbinnen de replicatie kan plaatsvinden. U kunt nog steeds de gegevens verloren gaan uit de primaire regio tijdens de replicatie-venster herstellen. U kunt mogelijk ook later samenvoegen met de secundaire regio.

De volgende voorbeelden van de architectuur bieden sommige ideeën op verschillende manieren van de afhandeling van transactionele gegevens in een failover-scenario. Het is belangrijk te weten dat deze voorbeelden niet volledig zijn. Tussentijdse opslaglocaties zoals wachtrijen kunnen bijvoorbeeld worden vervangen door Azure SQL-Database. De wachtrijen zelf mogelijk Azure opslag of Azure Service Bus wachtrijen (Zie [Azure wachtrijen en wachtrijen met Bus Service--vergeleken en tegenstelling tot](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)). Bestemmingen voor Server-opslag kunnen ook variëren, zoals Azure tabellen in plaats van een SQL-Database. Rollen van de werknemer kunnen bovendien worden ingevoegd als tussenpersoon in verschillende stappen. Het belangrijkste is dat niet precies deze architecturen emuleren, maar houden verschillende alternatieven in het herstel van transactionele gegevens en bijbehorende modules.

####<a name="replication-of-transactional-data-in-preparation-for-disaster-recovery"></a>Replicatie van transactionele gegevens in voorbereiding voor noodherstel

U kunt een toepassing die gebruikmaakt van Azure opslag wachtrijen voor transactionele gegevens. Hiermee kunt functies voor het verwerken van de transactionele gegevens naar de serverdatabase in een losgekoppelde architectuur werknemer. Hiervoor gebruikt u een vorm van tijdelijke cache als de front-end-rollen de onmiddellijke query van de gegevens vereist van de transacties. Afhankelijk van de tolerantie voor verlies van gegevens kunt u de wachtrijen, de database of alle van de opslagmiddelen worden gerepliceerd. Alleen databasereplicatie als de primaire regio uitvalt, kunt u nog steeds de gegevens herstellen in de wachtrijen bij de primaire regio terugkomt.

In het volgende diagram ziet u een architectuur waarin de server-database wordt gesynchroniseerd tussen de regio's.

![Replicatie van transactionele gegevens in voorbereiding voor noodherstel](./media/resiliency-disaster-recovery-azure-applications/replicate-transactional-data-in-preparation-for-disaster-recovery.png)

De grootste uitdaging voor de implementatie van deze architectuur is de replicatiestrategie voor tussen regio's. De Azure SQL-gegevens Sync-service kan dit soort replicatie. De service is echter nog steeds in de voorbeeldweergave en wordt niet aanbevolen voor productieomgevingen. Zie voor meer informatie [Overzicht: business continuity en database noodherstel met SQL-Database van de wolk](../sql-database/sql-database-business-continuity.md). Voor productietoepassingen, moet u uw eigen logica replicatie in code maken of investeren in een oplossing van derden. Afhankelijk van de architectuur, de replicatie mogelijk bidirectioneel is ook complexer.

Een mogelijke implementatie mogelijk maken gebruik van de tussentijdse wachtrij in het vorige voorbeeld. De rol van de werknemer die de gegevens naar de bestemming van de definitieve opslag verwerkt kan wijzigingen aanbrengen in de regio van de primaire en de secundaire regio. Dit zijn geen alledaagse taken en complete richtsnoeren voor replicatie-code valt buiten het bestek van dit artikel. Belangrijk punt is dat veel van uw tijd en testen moet richten op hoe u uw gegevens naar de secundaire regio repliceren. Extra verwerking en testen ervoor de failover- en herstel processen goed verwerkt alle mogelijke inconsistenties of dubbele transacties.

>[AZURE.NOTE] De meeste van deze white paper richt zich op platform als service (PaaS). Extra opties voor de replicatie en beschikbaarheid voor hybride toepassingen gebruiken echter Azure virtuele Machines. Infrastructuur als een service (IaaS) deze hybride toepassingen gebruiken om SQL Server als host voor virtuele machines in Azure. Hierdoor beschikbaarheid van traditionele methoden in SQL Server, zoals AlwaysOn beschikbaarheidsgroepen of Log-Shipping. Sommige technieken als AlwaysOn, werken alleen tussen SQL Server-exemplaren op gebouwen en Azure virtuele machines. Zie voor meer informatie, [hoge beschikbaarheid en noodherstel voor SQL Server in Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

####<a name="degraded-application-mode-for-transaction-capture"></a>Gedegradeerde toepassingsmodus voor het vastleggen van de transactie

U kunt een tweede architectuur die wordt toegepast in gedegradeerde modus. De toepassing van de secundaire regio deactiveert alle functionaliteit, zoals rapportage, business intelligence (BI), of een verwerkingsstop wachtrijen. Accepteert de belangrijkste soorten transactionele werkstromen, zoals gedefinieerd door de zakelijke vereisten. Het systeem de transacties worden vastgelegd en schrijft deze naar wachtrijen. Het systeem kan het verwerken van de gegevens in de eerste fase van de onderbrekingen uitstellen. Als het systeem op de primaire regio opnieuw binnen de verwachte tijdvenster wordt geactiveerd, kunnen de werknemer rollen in de primaire regio de wachtrijen tappen. Dit proces hoeven voor het samenvoegen van de database. Als de primaire regio onderbrekingen verder dan de maximaal toelaatbare venster gaat, starten de toepassing de wachtrijen verwerken.

In dit scenario bevat de database op de secundaire incrementele transactionele gegevens die moeten worden samengevoegd nadat de primaire opnieuw wordt geactiveerd. In het volgende diagram ziet u deze strategie voor transactionele gegevens tijdelijk worden opgeslagen totdat de primaire regio is hersteld.

![Gedegradeerde toepassingsmodus voor het vastleggen van de transactie](./media/resiliency-disaster-recovery-azure-applications/degraded-application-mode-for-transaction-capture.png)

Zie voor meer discussie van data management technieken voor robuuste toepassingen voor Azure [Failsafe: richtlijnen voor robuuste architecturen voor Cloud](https://channel9.msdn.com/Series/FailSafe).

##<a name="deployment-topologies-for-disaster-recovery"></a>Implementatietopologieën voor noodherstel

U moet verbetert de mogelijkheid van een hele regio onderbrekingen voorbereiden. U doen dit door het opnemen van een strategie voor de implementatie van meerdere regio in de operationele planning.

Implementaties voor meerdere regio betrekt IT pro processen voor het publiceren van de toepassings- en gegevens naar de secundaire regio na een ramp. Als de toepassing instant failover, het implementatieproces kan betrekking hebben op een actieve/passieve instelling of een actieve/actieve setup. Dit type implementatie heeft bestaande exemplaren van de toepassing wordt uitgevoerd in de andere regio. Een bewerkingsplangereedschap zoals Azure verkeer Manager biedt services op het niveau van DNS-taakverdeling. Deze serviceonderbrekingen detecteert en de gebruikers doorsturen naar verschillende regio's als dat nodig is.

Onderdeel van een succesvolle Azure noodherstel is goede toepassingsarchitectuur te ontwerpen dat herstel in de oplossing van het begin. De cloud biedt aanvullende opties voor het herstellen van fouten tijdens een ramp die niet beschikbaar in een traditionele hostingprovider zijn. Specifiek, kunt u snel en dynamisch resources toewijzen aan een andere regio. Daarom Betaal je geen veel voor niet-actieve resources terwijl u voor een fout wacht optreden.

De volgende secties beschreven verschillende topologieën voor noodherstel. Er is meestal een verhouding in de extra kosten of complexiteit voor extra beschikbaarheid.

###<a name="single-region-deployment"></a>Regio-implementatie

De implementatie van een regio is niet echt een topologie disaster recovery, maar is bedoeld als contrast met de andere architecturen. Regio-implementaties kunnen worden gebruikt voor toepassingen in Azure. Dit type implementatie is echter niet een ernstige rivaal voor een calamiteitenplan.

In het volgende diagram ziet u een toepassing wordt uitgevoerd in een bepaalde regio Azure. Azure verkeer-beheer en het gebruik van domeinen probleem- en verhogen de beschikbaarheid van de toepassing in het gebied.

![Regio-implementatie](./media/resiliency-disaster-recovery-azure-applications/single-region-deployment.png)

Hier is het duidelijk dat de database een potentieel risico is. Zelfs als de gegevens worden gerepliceerd tussen domeinen veroorzaakt verschillende interne replica's van Azure, is dit alles vindt plaats in hetzelfde gebied. De toepassing kan niet bestand zijn tegen een onherstelbare fout. Als het gebied begeeft, gaan alle domeinen veroorzaakt omlaag--inclusief alle exemplaren van de service- en opslagbronnen.

U moet een plan voor de implementatie van uw toepassingen over meerdere regio's ontwikkelen voor alle maar de minst kritieke toepassingen. U moet ook overwegen RTO en de beperkingen bij de beoordeling welke topologie van de implementatie wilt gebruiken.

Laten we nu op specifieke patronen voor failover ondersteuning tussen de verschillende regio's. Twee regio's bij deze voorbeelden gebruikt om het proces te beschrijven.

###<a name="redeployment-to-a-secondary-azure-region"></a>Microsoft CRM opnieuw installeren om een gebied met secundaire Azure

Alleen de primaire regio heeft in het patroon van Microsoft CRM opnieuw installeren om een gebied met secundaire toepassingen en databases worden uitgevoerd. Het tweede gebied is niet ingesteld voor een automatische failover. Als een ramp plaatsvindt, kunt u moet dus voor draaien om de onderdelen van de service in het nieuwe gebied. Dit omvat een cloud service uploaden naar Azure, het implementeren van de cloud-service, de gegevens terug te zetten en wijzigen van DNS het verkeer wordt omgeleid.

Dit is de meest betaalbare opties meerdere regio, heeft de slechtste RTO-kenmerken. In dit model, de service-pakket en de database back-ups worden opgeslagen in ruimten of in de Azure Blob storage instantie van de secundaire regio. U moet echter een nieuwe service implementeren en terugzetten van de gegevens voordat de bewerking wordt gehaald. Zelfs als u de overdracht van gegevens uit back-up opslag volledig automatiseren, neemt de nieuwe databaseomgeving draaien veel tijd. Gegevens uit de schijfopslag van back-up verplaatsen naar de lege database op het gebied van secundaire is het duurste onderdeel van het herstelproces. U moet hiervoor echter om de nieuwe database in een operationele status omdat deze niet is gerepliceerd.

De beste manier is voor het opslaan van de servicepakketten in Blob-opslag in de secundaire regio. Hierdoor hoeft u het pakket uploaden naar Azure, dat wat er gebeurt is wanneer u vanaf een ontwikkelcomputer op gebouwen implementeren. U kunt snel de servicepakketten met een nieuwe wolk service implementeren van Blob-opslag met behulp van PowerShell scripts.

Deze optie is alleen voor niet-kritieke toepassingen die een hoge RTO kunnen tolereren praktische. Dit kan bijvoorbeeld werkt voor een toepassing die kan u enkele uren maar binnen 24 uur opnieuw moet worden uitgevoerd.

![Microsoft CRM opnieuw installeren om een gebied met secundaire Azure](./media/resiliency-disaster-recovery-azure-applications/redeploy-to-a-secondary-azure-region.png)

###<a name="active-passive"></a>Actief-passief

Het patroon van de actief-passief is de keuze ten gunste van veel bedrijven. Dit patroon bevat verbeteringen voor de RTO met een relatief kleine stijging van de kosten via het patroon opnieuw installeren.
In dit scenario is er een primaire en een secundaire Azure regio. Al het verkeer gaat naar de actieve implementatie van de primaire regio. Het tweede gebied is beter voorbereid op herstel na storingen omdat de database wordt uitgevoerd op beide regio's. Bovendien is een mechanisme voor synchronisatie plaats tussen hen. Deze stand-by-aanpak kan betrekking hebben op twee varianten: een benadering van de database alleen-lezen of een volledige implementatie in de secundaire regio.

####<a name="database-only"></a>Alleen de database

In de eerste variant van het patroon van de actief-passief heeft alleen de primaire regio een geïmplementeerde cloud service-toepassing. In tegenstelling tot het patroon opnieuw installeren, de beide regio's worden gesynchroniseerd met de inhoud van de database. (Zie de sectie over [transactionele gegevenspatroon voor noodherstel](#transactional-data-pattern-for-disaster-recovery)voor meer informatie.) Als een ramp plaatsvindt, zijn er minder vereisten voor activering. U start de toepassing in de secundaire regio verbindingsreeksen wijzigen in de nieuwe database en wijzigen van de DNS-vermeldingen verkeer wordt omgeleid.

Net als het patroon opnieuw installeren, moet u al in hebt opgeslagen de servicepakketten Azure Blob-opslag in de secundaire regio voor een snellere implementatie. In tegenstelling tot het patroon opnieuw installeren tot niet u de meerderheid van de overhead die herstellen van database is vereist. De database is klaar en wordt uitgevoerd. Dit bespaart een aanzienlijke hoeveelheid tijd, waardoor dit een betaalbare DR patroon. Het is ook de meest populaire DR patroon.

![Alleen actieve-passieve, database](./media/resiliency-disaster-recovery-azure-applications/active-passive-database-only.png)

####<a name="full-replica"></a>Volledige replica

In de tweede variant van het actief-passief patroon hebben zowel de regio van de primaire en de secundaire regio een volledige implementatie. Deze installatie omvat de cloud-services en een database gesynchroniseerd. Alleen de primaire regio is echter actief netwerkverzoeken van gebruikers afhandelen. De secundaire regio actief wordt alleen weergegeven als de primaire regio een verstoring van de service ondervindt. In dat geval doorsturen alle nieuwe netwerkaanvragen naar de secundaire regio. Azure verkeer Manager kunt automatisch deze failover beheren.

Failover plaatsvindt sneller dan de variant van de database alleen-lezen omdat de services al zijn geïmplementeerd. Dit patroon bevat een zeer lage RTO. De regio secundaire failover moet onmiddellijk na een storing van de primaire regio klaar zijn.

Dit patroon is samen met een snellere responstijd het voordeel van het vooraf toewijzen en back-services implementeren. U hebt geen zorgen te maken over een gebied dat niet de ruimte voor het toewijzen van nieuwe exemplaren in een noodsituatie. Dit is belangrijk als uw secundaire Azure regio capaciteit nadert. Er is geen garantie (serviceniveau overeenkomst) dat u direct zal kunnen voor de implementatie van een aantal nieuwe cloud services in elke regio.

Voor de snelste responstijd van dit model, moet u dezelfde schaal (aantal exemplaren van de rol) in de primaire en secundaire gebieden hebben. Ondanks de voordelen, betaalt voor het berekenen van ongebruikte exemplaren is kostbaar en toch de meest voorzichtige financiële keuze. Daarom is het algemene gebruik van een enigszins gereduceerde versie van cloud-diensten op het gebied van de secundaire. Vervolgens kunt u snel failover en schaal van de secundaire distributie indien nodig. U kunt de failover-procedure te automatiseren, zodat nadat de primaire regio niet toegankelijk is, u extra exemplaren, afhankelijk van de belasting activeert. Dit kan betrekking hebben op het gebruik van een mechanisme voor autoscaling als de [schaal van de virtuele machine wordt ingesteld](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

In het volgende diagram ziet u het model waarin de primaire en secundaire gebieden een volledig geïmplementeerde cloud-service in een actieve-passieve patroon bevatten.

![Actief-passief, volledige replica](./media/resiliency-disaster-recovery-azure-applications/active-passive-full-replica.png)

###<a name="active-active"></a>Actieve

Nu u bent waarschijnlijk bij beslissingen over de ontwikkeling van de patronen: verlaagt de RTO verhoogt de kosten en complexiteit. Deze tendens met betrekking tot kosten, de actieve oplossing daadwerkelijk verbroken.

In het patroon van een actieve, cloud services en database zijn volledig is geïmplementeerd in beide regio's. In tegenstelling tot het model actief-passief worden in beide regio's gebruikersverkeer ontvangen. Deze optie levert de snelste hersteltijd. De diensten voor het verwerken van een gedeelte van de belasting op elke regio al geschaald. DNS is de secundaire regio gebruiken al ingeschakeld. Er is extra complexiteit om te bepalen hoe u gebruikers naar de juiste regio. Round robin planning mogelijk. Het is waarschijnlijk dat bepaalde gebruikers een bepaalde regio waar de primaire kopie van de gegevens zich bevindt wilt gebruiken.

In het geval van failover, gewoon DNS op de primaire regio uitschakelen. Dit routeert alle verkeer naar de secundaire regio.

Zelfs in dit model zijn er enkele variaties. In het volgende diagram ziet u bijvoorbeeld een model waarin de primaire regio eigenaar is van het originele exemplaar van de database. De cloud-services in beide regio's naar de primaire database schrijven. De secundaire distributie kunt van de primaire of gerepliceerde database lezen. Replicatie in het volgende voorbeeld één manier gebeurt.

![Actieve](./media/resiliency-disaster-recovery-azure-applications/active-active.png)

Er is een nadeel aan de actieve architectuur in het voorgaande diagram. Het tweede gebied moet toegang krijgen tot de database in het eerste gebied omdat er bij het origineel zich bevindt. Prestaties aanzienlijk verdwijnt deze bij toegang gegevens die zich buiten een gebied tot. In regio cross database-aanroepen, moet u overwegen een type batchen strategie ter verbetering van de prestaties van deze oproepen. Zie voor meer informatie [hoe u met de batchverwerking ter verbetering van de prestaties SQL-Database](../sql-database/sql-database-use-batching-to-improve-performance.md).

Een alternatieve architectuur kan betrekking hebben op elke regio zijn eigen database rechtstreeks te openen. In dit model, een type voor bidirectionele replicatie moet synchroniseren van databases in elke regio.

In het actieve patroon moet u mogelijk niet zo veel exemplaren van de primaire regio zoals in het patroon van de actief-passief. Als u 10 exemplaren van de primaire regio in de architectuur van een actief-passief, moet u mogelijk alleen 5 in elke regio in een actieve architectuur. Beide regio's delen nu de belasting. Mogelijk een kostenbesparingen op het patroon van de actief-passief als u een warme stand-by op de passieve regio met 10 exemplaren wachten voor failover.

Realiseert zich dat totdat u de primaire regio herstellen, de secundaire regio kan er een plotselinge toename van nieuwe gebruikers. Als er 10.000 gebruikers op elke server de primaire regio ondervindt een verstoring van de service, heeft de secundaire regio plotseling maximaal 20.000 gebruikers verwerken. Monitoring van de regels op het gebied van de secundaire moet detecteren deze verhoging en dubbele exemplaren in de secundaire regio. Zie de sectie op de [opsporing van storingen](#failure-detection)voor meer informatie hierover.

##<a name="hybrid-on-premises-and-cloud-solution"></a>Hybride op gebouwen en cloud oplossing

Een aanvullende strategie voor noodherstel is aan het ontwerp van een hybride-toepassing die wordt uitgevoerd op bedrijven en in de cloud. Afhankelijk van de toepassing, kan de primaire regio zijn beide locaties. Houd rekening met de vorige architecturen en stel de primaire of secundaire regio als een locatie op de locatie.

Er zijn enkele uitdagingen in deze hybride architecturen. De meeste van dit artikel is eerste PaaS architectuur patronen gericht. Typische PaaS toepassingen in Azure vertrouwen op Azure-specifieke constructies zoals rollen, cloud services en beheer van netwerkverkeer. Als u wilt maken van een oplossing in ruimten voor dit type toepassing PaaS vereist een sterk verschillende architectuur. Dit is mogelijk niet haalbaar is in een management of het perspectief van de kosten.

Een hybride oplossing voor noodherstel is echter minder uitdagingen voor de traditionele architecturen die gewoon naar de cloud verplaatst. Dit geldt voor architecturen die gebruikmaken van IaaS. IaaS toepassingen gebruikt virtuele machines in de cloud die direct op gebouwen-equivalenten. U kunt ook virtuele netwerken verbinding maken met computers in de cloud met netwerkbronnen voor gebouwen. Hiermee opent u verschillende mogelijkheden die niet mogelijk zijn met alleen PaaS-toepassingen. Bijvoorbeeld kunt SQL Server profiteren van disaster recovery oplossingen zoals AlwaysOn beschikbaarheidsgroepen en database-mirroring. Zie voor details, [hoge beschikbaarheid en noodherstel voor SQL Server in Azure virtuele machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

IaaS oplossingen bieden ook een eenvoudiger pad voor toepassingen voor bedrijfsruimten Azure als de failover-optie gebruiken. Mogelijk hebt u een volledig functionerende toepassing in een bestaande regio voor op gebouwen. Maar wat gebeurt er als u niet over de middelen te handhaven van een geografisch afzonderlijke regio voor failover? U kunt virtuele machines en virtuele netwerken gebruiken om de toepassing die wordt uitgevoerd in Azure. In dat geval definiëren van processen die naar de cloud synchroniseren. De Azure-implementatie klikt, wordt het secundaire regio voor failover. De primaire regio blijft de toepassing op ruimten. Raadpleeg de [documentatie van de virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)voor meer informatie over IaaS architecturen en de mogelijkheden.

##<a name="alternative-cloud"></a>Alternatieve cloud

Er zijn situaties waar zelfs de robuustheid van het Microsoft Cloud voldoet niet aan naleving van interne regels of beleid dat uw organisatie nodig heeft. Zelfs de beste voorbereiding en het ontwerp voor de uitvoering van back-up systemen tijdens een ramp tekortschieten als er een verstoring van de wereldwijde service van een cloud serviceprovider.

U zult willen vergelijken beschikbaarheid eisen met de kosten en complexiteit van een hogere mate van beschikbaarheid. Uitvoeren van een risicoanalyse en de RTO en vrijgegeven Productieorder definiëren voor uw oplossing. Als uw toepassing niet kan uitvaltijd tolereren, kan het zinvol voor u overwegen om een andere cloud-oplossing. Tenzij het hele Internet uitvalt, een andere cloud-oplossing mogelijk toch beschikbaar als Azure globaal ontoegankelijk.

Net als bij het scenario hybride kunnen de failover-implementaties in de vorige disaster recovery architecturen ook bestaan in een andere cloud-oplossing. Alternatieve wolk DR sites moeten alleen worden gebruikt voor oplossingen waarvan RTO kan zeer weinig of geen, downtime. Houd er rekening mee dat een oplossing op basis van een site DR buiten Azure meer werk vereist te configureren, ontwikkelen, implementeren en onderhouden. Het is ook moeilijker te implementeren in een cross-cloud-architectuur. Hoewel wolk platforms vergelijkbare concepten van hoog niveau hebben, zijn de API's en architecturen verschillend.

Als u uw DR tussen verschillende platforms wordt gesplitst besluit, zou het zinvol te bouwen abstraction lagen in het ontwerp van de oplossing. Als u dit doet, hoeft u niet te ontwikkelen en onderhouden van twee verschillende versies van dezelfde toepassing voor verschillende cloud-platforms in geval van calamiteiten. Als bij het scenario hybride gebruik van Azure Virtual Machines of Azure Container Service mogelijk in deze gevallen gemakkelijker dan het gebruik van specifieke cloud-PaaS-modellen.

##<a name="automation"></a>Automatisering

Sommige van de patronen die we net hebben gezien vereist snelle activering van offline-implementaties en het herstellen van specifieke onderdelen van een systeem. Automatisering of scripts, ondersteunt de resources op verzoek activeren en snel oplossingen implementeren. In dit artikel, DR-gerelateerde automatisering met [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)is gelijkgesteld, maar de [REST API voor Management-Service](https://msdn.microsoft.com/library/azure/ee460799.aspx) is ook een optie.

Ontwikkelen van scripts kunt u de onderdelen van DR die Azure transparant niet verwerkt beheren. Het voordeel van het produceren van consistente resultaten elke keer dat de kans op menselijke fouten tot een minimum beperkt is. DR scripts hoeft vooraf ook minder tijd om te bouwen van een systeem en de samenstellende delen door een ramp. U wilt probeert handmatig uitvinden hoe uw site herstellen terwijl het omlaag en verliezende geld elke minuut niet.

Nadat u de scripts maken, testen herhaaldelijk van begin tot eind. Nadat u de basisfunctionaliteit controleren, moet u deze [ramp simulatie](#disaster-simulation)testen. Hierdoor worden fouten in scripts of processen aan het licht komen.

Beste automatisering is een opslagplaats van PowerShell scripts of scripts voor noodherstel Azure opdrachtregelinterface (CLI) maken. Duidelijk markeren en ze te categoriseren voor eenvoudig opzoeken. Aanwijzen van één persoon in de opslagplaats en versiebeheer scripts beheren. Document dat ze goed met uitleg van de parameters en voorbeelden van het gebruik van script. Zorg er ook voor dat u deze documentatie synchroon met uw Azure-implementaties houden. Dit onderstrepingstekens het doel van een persoon die verantwoordelijk is voor alle delen van de bibliotheek met.

##<a name="failure-detection"></a>Foutdetectie

Problemen met de beschikbaarheid en noodherstel correct verwerken, moet u kunnen opsporen en onderzoeken van fouten. U kunt geavanceerde server- en implementatie controleren zodat u snel kunt weten wanneer een systeem of onderdelen ervan plotseling naar beneden zijn moet doen. Hulpprogramma's die op de algehele gezondheid van de cloud-service en de bijbehorende afhankelijkheden kunt deel van dit werk uitvoeren. Een hulpprogramma van Microsoft is [System Center 2016](https://www.microsoft.com/en-us/server-cloud/products/system-center-2016/). Hulpprogramma's van derden, biedt ook controlefuncties. De meeste bewakingsoplossingen bijhouden prestatie-items en de beschikbaarheid van de service.

Hoewel deze hulpmiddelen essentieel zijn, niet als vervanging van de noodzaak om te plannen voor foutenopsporing en rapportage in een cloud-service. U moet plannen goed Azure diagnostische gegevens te gebruiken. Aangepaste prestatiemeters of logboekvermeldingen van gebeurtenis kunnen ook deel uitmaken van de algemene strategie. Dit biedt meer gegevens bij defecten snel een diagnose en volledige functionaliteit te herstellen. Het biedt ook extra statistieken controleprogramma's kunnen gebruiken om te bepalen van de gezondheid van toepassing. Zie [Azure Diagnostics inschakelen in Azure Cloud Services](../cloud-services/cloud-services-dotnet-diagnostics.md)voor meer informatie. Zie voor een bespreking van het plannen van een algemeen "model van de gezondheid", [Failsafe: richtlijnen voor robuuste architecturen voor Cloud](https://channel9.msdn.com/Series/FailSafe).

##<a name="disaster-simulation"></a>Simulatie van rampen

Simulatie testen moeten worden gemaakt van kleine praktijksituaties op de vloer werk om te zien hoe de teamleden reageren. Simulaties ook weergeven hoe doeltreffend de oplossingen zijn in het herstelplan. Simulaties op zodanige wijze dat de gemaakte scenario's zakelijke niet verstoren, terwijl nog steeds gevoel dat reële situaties te verrichten.

U kunt een soort "schakelbord" in de toepassing voor het simuleren van problemen met de beschikbaarheid handmatig aanpassen. Bijvoorbeeld via een zachte switch-trigger database access uitzonderingen voor een module bestellen door waardoor het niet goed meer werkt. U kunt vergelijkbare lichtgewicht benaderingen voor andere modules nemen op het niveau van de netwerkinterface.

Simulatie van de benadrukt problemen die voorlichting waren. De gesimuleerde scenario's moet volledig bestuurbaar zijn. Dit betekent dat, zelfs als het herstelplan lijkt te zijn opgetreden, u de situatie terug naar normale herstellen kunt zonder significante schade veroorzaakt. Het is ook belangrijk dat u kennis op een hoger niveau van beheer over wanneer en hoe de simulatieoefeningen wordt uitgevoerd. Dit plan moet informatie bevatten over de tijd of bronnen die u niet-productieve uren worden kunnen terwijl de simulatie-test wordt uitgevoerd. Wanneer u het calamiteitenplan aan een test onderwerpen bent, is het ook belangrijk om te definiëren hoe succes wordt gemeten.

Er zijn verschillende andere technieken die u gebruiken kunt voor het testen van disaster recovery plannen. De meeste van hen zijn echter alleen gewijzigde versies van deze elementaire technieken. De belangrijkste motive achter deze testen is om te evalueren hoe haalbaar is en hoe haalbaar het herstelplan. Herstel na storing herstel test richt zich op de details te ontdekken gaten in het herstelplan basic.

##<a name="next-steps"></a>Volgende stappen

Dit artikel maakt deel uit van een serie artikelen gericht op [herstel na storingen en hoge beschikbaarheid voor toepassingen die zijn gebouwd op Microsoft Azure](./resiliency-disaster-recovery-high-availability-azure-applications.md). Het vorige artikel in deze serie is een [hoge mate van beschikbaarheid voor toepassingen die zijn gebouwd op Microsoft Azure](./resiliency-high-availability-azure-applications.md).

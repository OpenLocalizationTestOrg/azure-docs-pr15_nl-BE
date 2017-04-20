<properties
   pageTitle="Patronen voor multitenant SaaS-toepassingen en Azure SQL Database ontwerpen | Microsoft Azure"
   description="Dit artikel worden de vereisten en algemene gegevens architectuur patronen van multitenant database moeten rekening houden met toepassingen die worden uitgevoerd in een cloud-omgeving en de verschillende optimalisatie die zijn gekoppeld aan deze patronen. Ook wordt uitgelegd hoe Azure SQL-Database, met een elastische van toepassingen en hulpmiddelen voor elastische helpen deze eisen op een manier zonder adres."
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-design"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-sql-database"></a>Patronen voor multitenant SaaS-toepassingen en Azure SQL Database ontwerpen

In dit artikel leert u over de vereisten en de gemeenschappelijke gegevens architectuur patronen van multitenant-software als een service (SaaS) database-toepassingen die worden uitgevoerd in een cloud-omgeving. Ook wordt de factoren die u rekening moet houden en de afweging van verschillende ontwerppatronen beschreven. Elastische pools en elastische extra in Azure SQL-Database kunt u uw specifieke behoeften zonder verlies van andere doelstellingen.

Ontwikkelaars maken soms keuzes die tegen hun belang op lange termijn werken bij het ontwerpen van pacht modellen voor de onderlaag van de gegevens van multitenant-toepassingen. In eerste instantie ten minste kan een ontwikkelaar zien eenvoudig ontwikkeling en lagere kosten voor cloud serviceprovider belangrijker dan de huurder isolatie of de schaalbaarheid van een toepassing. Deze keuze kan leiden tot de zorgen van de tevredenheid van klanten en een dure cursus-correctie later.

Een multitenant-toepassing is een toepassing die wordt gehost in een cloud-omgeving en die dezelfde reeks services voor honderden of duizenden huurders die niet delen of van andere gegevens bevat. Een voorbeeld is een SaaS-toepassing die services aan huurders in een wolk gehoste omgeving levert.

## <a name="multitenant-applications"></a>Multitenant-toepassingen

In multitenant-toepassingen, kunnen gegevens en werkbelasting gemakkelijk worden gepartitioneerd. U kunt partitioneren gegevens en werkbelasting, bijvoorbeeld langs de grenzen van de huurder, omdat de meeste aanvragen worden uitgevoerd binnen de grenzen van een huurder. Die eigenschap is die inherent zijn aan de gegevens en de werklast en het geeft voorrang aan de toepassing patronen die in dit artikel besproken.

Ontwikkelaars gebruiken dit type toepassing binnen het hele spectrum van cloud-gebaseerde toepassingen, waaronder:

- Partner-databasetoepassingen die als SaaS-toepassingen zijn wordt overgestapt naar de cloud
- SaaS-toepassingen die zijn opgebouwd voor de wolk boven het wegdek
- Directe, klantgerichte toepassingen
- Werknemers gerichte zakelijke toepassingen

SaaS-toepassingen die zijn ontworpen voor de cloud en de wortels zijn meestal databasetoepassingen partner multitenant-toepassingen. Deze SaaS-toepassingen bieden een toepassing met speciale software als een service aan hun huurders. Huurders toegang tot de application service en volledige eigendom van de bijbehorende gegevens opgeslagen als onderdeel van de toepassing. Maar als u wilt profiteren van de voordelen van SaaS, huurders enige controle over hun eigen gegevens moeten leveren. Ze vertrouwen de SaaS-provider om te voorkomen dat hun gegevens veilig en geïsoleerde andere huurders gegevens. Voorbeelden van dit soort multitenant SaaS-applicatie zijn MYOB, SnelStart en Salesforce.com. Elk van deze toepassingen kan worden gepartitioneerd langs de grenzen van de huurder en de ondersteuning voor de toepassing in dit artikel bespreken we patronen ontwerpen.

Toepassingen die directe dienstverlening aan klanten of werknemers binnen een organisatie (vaak aangeduid als de gebruikers in plaats van huurders) zijn een andere categorie in het spectrum multitenant-toepassing. Klanten abonneren op de service en geen eigenaar van de gegevens die door de serviceprovider worden verzameld en opgeslagen. Serviceproviders hebben minder strenge eisen om hun klanten gegevens geïsoleerd is voorbij de overheid voorgeschreven privacyrichtlijnen van elkaar te houden. Voorbeelden van dit soort klantgerichte multitenant toepassing zijn media-aanbieders als Netflix, Spotify en Xbox LIVE. Andere voorbeelden van toepassingen eenvoudig partitionable klantgerichte, Internet-toepassingen of toepassingen Internet dingen (IoT) in elke klant- of kan dienen als een partitie. Partitie grenzen kunnen afzonderlijke gebruikers en apparaten.

Niet alle toepassingen partitie gemakkelijk langs een enkele eigenschap, zoals de huurder, klant, gebruiker of apparaat. Een complexe enterprise resource planning (ERP)-toepassing heeft bijvoorbeeld producten, orders en klanten. Deze heeft meestal een complexe schema met duizenden tabellen sterk met elkaar verbonden.

Geen enkele partitie strategie kunt toepassen op alle tabellen en werken via de werkbelasting van een toepassing. In dit artikel ligt de nadruk op multitenant toepassingen die gegevens gemakkelijk partitionable en werkbelasting hebben.

## <a name="multitenant-application-design-trade-offs"></a>Ontwerp-en nadelen multitenant-toepassing

Het ontwerppatroon die een ontwikkelaar van toepassingen multitenant meestal kiest is gebaseerd op een rekening wordt gehouden met de volgende factoren:

-   **Isolatie van de huurder**. De ontwikkelaar moet er voor zorgen dat de huurder geen ongewenste toegang tot gegevens van andere huurders heeft. De isolatie-eis strekt zich uit tot andere eigenschappen, zoals een bescherming van luide buren, kunnen gegevens van de huurder herstellen en implementeren van huurder specifieke aanpassingen.
-   **Wolk resourcekosten**. Een SaaS-toepassing moet kosten concurrerend. Een ontwikkelaar van toepassingen multitenant kunt optimaliseren voor lagere kosten van het gebruik van de cloud-bronnen, zoals de opslag en het berekenen van kosten.
-   **Gemak van DevOps**. Een ontwikkelaar van toepassingen multitenant moet opnemen, bescherming van isolatie, onderhouden, controleren van de status van de toepassing en het databaseschema en huurder problemen. Complexiteit van de ontwikkeling van toepassingen en de bewerking wordt omgezet in extra kosten rechtstreeks en uitdagingen worden verwezenlijkt met tevredenheid van de huurder.
-   **Schaalbaarheid**. De mogelijkheid te voegen meer huurders en capaciteit voor huurders die behoefte hebben aan het is noodzakelijk om een succesvolle SaaS-bewerking.

Elk van deze factoren heeft e-mailverkeer in vergelijking met andere. De laagste kosten cloud bieden mogelijk niet de meest geschikte ontwikkeling ervaring bieden. Het is belangrijk voor een ontwikkelaar geïnformeerde keuzes maken over deze opties en hun e-mailverkeer tijdens het ontwerpproces van toepassing.

Een populaire ontwikkeling patroon is meerdere huurders inpakken in een of meer databases. De voordelen van deze aanpak zijn goedkoper omdat u voor enkele databases en de relatieve eenvoud betalen van het werken met een beperkt aantal databases. Maar na verloop van tijd een multitenant ontwikkelaar SaaS realiseren dat deze keuze in de isolatie van de huurder en de schaalbaarheid van de aanzienlijke nadelen heeft. Als de huurder isolatie is belangrijk, extra inspanning moet huurder gegevens in gedeelde opslag te beschermen tegen onbevoegde toegang of luide buren. Deze extra inspanning kan aanzienlijk vergroten ontwikkelingsinspanningen en onderhoudskosten isolatie. Op dezelfde manier als het toevoegen van huurders is vereist, vereist dit ontwerppatroon meestal expertise te distribueren huurder gegevens in databases voor het schalen van de gegevenslaag van de toepassing correct.  

Vaak huurder isolatie is een fundamentele vereiste in multitenant SaaS-toepassingen die geschikt zijn voor bedrijven en organisaties. Een ontwikkelaar kan in de verleiding komen door de verwachte voordelen in eenvoud en kosten huurder isolatie en schaalbaarheid. Deze wisselwerking kan bewijzen complexe en dure als de service groeit en huurder geïsoleerd moeten worden beheerd op de toepassingslaag en meer belangrijk. Echter, in multitenant toepassingen die een directe, consument gerichte service aan klanten verlenen, huurder isolatie mogelijk een lagere prioriteit dan optimaliseren voor resourcekosten wolk.

## <a name="multitenant-data-models"></a>Multitenant data-modellen

Gemeenschappelijke ontwerp-procedures voor het plaatsen van gegevens van de huurder voert u drie verschillende modellen afgebeeld in figuur 1.


  ![Multitenant application data-modellen](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)
 figuur 1: algemene ontwerppraktijken voor gegevensmodellen multitenant

-   **Database per huurder**. Elke huurder heeft een eigen database. Alle huurder-specifieke gegevens worden beperkt tot de database van de huurder en geïsoleerd van andere huurders en hun gegevens.
-   **Gedeelde database een laptopgeheugen**. Meerdere huurders delen een van meerdere databases. Een verschillend aantal huurders wordt toegewezen aan elke database met behulp van een strategie voor partitionering zoals hash, het bereik of de lijst partitioneren. Deze gegevens distributiestrategie vaak heet sharding.
-   **Gedeelde database-enkel**. Een soms grote database bevat gegevens voor alle huurders die in een kolom huurder zijn disambiguated.

> [AZURE.NOTE] Een ontwikkelaar van toepassingen kunt verschillende huurders in een andere database schema's en gebruik vervolgens de naam van het schema te onderscheiden van de andere huurder. Deze benadering wordt niet aanbevolen omdat deze het gebruik van dynamische SQL is meestal nodig en kan het niet effectief in het plan in cache opslaan. We richten op de aanpak van gedeelde tabel voor deze categorie van multitenant-toepassing in de rest van dit artikel.

## <a name="popular-multitenant-data-models"></a>Populaire multitenant gegevensmodellen

Het is belangrijk voor de evaluatie van de verschillende soorten multitenant gegevensmodellen in termen van de toepassing ontwerpen compromis die al gevonden. Deze factoren helpen bij het karakteriseren van de drie meest voorkomende multitenant gegevensmodellen die eerder zijn beschreven en hun database gebruik zoals aangegeven in figuur 2.

-   **Isolatie**. De mate van isolatie tussen huurders zijn geeft aan hoeveel huurder isolatie een gegevensmodel realiseert.
-   **Wolk resourcekosten**. Het bedrag van de bronnen delen tussen huurders kunt optimaliseren wolk resourcekosten. Een resource kan worden gedefinieerd als de kosten voor opslag en rekenkracht.
-   **Kosten van DevOps**. Het gemak van ontwikkeling, implementatie en beheer minder algemene bewerkingskosten van SaaS.  

In figuur 2 bevat de Y-as de mate van isolatie van de huurder. De X-as geeft het niveau van de gedeelde bron. De grijze, diagonale pijl in het midden geeft de richting van de kosten van DevOps, stationskleppen vergroten of verkleinen.

![Populaire multitenant toepassing ontwerppatronen](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png) in figuur 2: populaire multitenant gegevensmodellen

De rechterbenedenhoek in afbeelding 2 ziet u een patroon van toepassing die gebruikmaakt van een potentieel grote één gedeelde database en de gedeelde tabel (of apart schema) benadering. Het is goed voor resources delen omdat alle huurders dezelfde database resources (CPU, geheugen, invoer/uitvoer) in één database gebruikt. Maar huurder isolatie is beperkt. Moet u mogelijk aanvullende stappen uitvoeren om te beschermen van huurders van elkaar op de toepassingslaag. Deze extra stappen kunnen de DevOps kosten van ontwikkeling en beheer van de toepassing aanzienlijk verbeteren. Schaalbaarheid wordt beperkt door de omvang van de hardware die fungeert als host voor de database.

Het kwadrant linksonder in afbeelding 2 ziet u een laptopgeheugen van meerdere huurders over meerdere databases (doorgaans verschillende hardware eenheden schalen). Elke database bevat een subset van huurders, waarin de bezorgdheid van de schaalbaarheid van andere patronen. Als er meer capaciteit is vereist voor meer huurders, kunt u eenvoudig de huurder op nieuwe databases die worden toegewezen aan nieuwe hardware schaaleenheden plaatsen. Echter, het bedrag van de gedeelde bron wordt verminderd. Alleen huurders geplaatst op dezelfde schaaleenheden resources delen. Deze benadering biedt weinig verbetering voor pachters isolatie omdat veel huurders worden nog steeds collocated zonder automatisch worden beschermd tegen acties van de ander. Complexiteit van toepassing blijft hoog.

Het Kwadrant linksboven in figuur 2 is de derde benadering. De gegevens van elke huurder wordt in een eigen database. Deze benadering heeft goede huurder-isolatie-eigenschappen maar beperkt resource bij elke database een eigen toegewezen bronnen heeft delen. Deze aanpak is geschikt als alle huurders voorspelbare werkbelasting hebben. Als huurder werklasten minder voorspelbaar worden, de provider niet worden geoptimaliseerd door resources delen. Onvoorspelbaarheid is gebruikelijk voor SaaS-toepassingen. De provider moet ofwel overdreven bepaling te voldoen aan de vraag of lagere resources. Een actie leidt tot hogere kosten of lagere tevredenheid van de huurder. Een hogere mate van de huurders delen wordt wenselijk om de oplossing meer rendabele. Het aantal databases ook verhoogt, kosten van DevOps om te implementeren en onderhouden van de toepassing. Ondanks deze problemen biedt deze methode de beste en eenvoudigste isolatie voor huurders.

Deze factoren ook van invloed zijn op het ontwerppatroon een klant kiest:

-   **Eigendom van de gegevens van de huurder**. Het patroon van één database per huurder meer gericht op een toepassing waarin huurder eigenaar van hun eigen gegevens blijven.
-   **Schaal**. Een toepassing die is bedoeld voor honderden of duizenden of miljoenen huurders geeft voorrang aan de database delen benaderingen zoals sharding. Vereisten voor isolatie kunnen nog steeds uitdagingen vormen.
-   **Waarde en het business model**. Als een toepassing per-huurder opbrengst als zinvol klein (minder dan een dollar), geïsoleerd moeten minder kritiek en een gedeelde database. Als de opbrengst per huurder een paar euro of meer is, is een database per huurder model haalbaar. Deze kan ontwikkelingskosten verminderen.

Gezien de ontwerp-en nadelen afgebeeld in figuur 2, moet een ideale multitenant model goed huurder isolatie-eigenschappen opnemen met optimale bronnen delen tussen huurders. Dit model past in de categorie die wordt beschreven in het Kwadrant rechtsboven van figuur 2.

## <a name="multitenancy-support-in-azure-sql-database"></a>Multitenancy ondersteuning in Azure SQL-Database

Azure SQL-Database ondersteunt alle multitenant toepassing patronen die worden beschreven in figuur 2. Ondersteunt ook een patroon van toepassing die goede bronnen worden gedeeld combineert met elastische van toepassingen en benadering van de voordelen van de isolatie van de database per huurder (Zie het Kwadrant rechtsboven in figuur 3). Verlaag de kosten voor het ontwikkelen en exploiteren van een toepassing met veel databases (zoals weergegeven in het grijze gebied in figuur 3) elastische database tools en functies in een SQL-Database. Deze hulpprogramma's kunt u bouwen en beheren van toepassingen die van de database met meerdere patronen gebruikmaken.

![Patronen in Azure SQL-Database](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png) in figuur 3: Multitenant toepassing patronen in Azure SQL-Database

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Database per huurder model met elastische van toepassingen en hulpprogramma 's

Huurder isolatie combineren elastische van toepassingen in de SQL-Database met bronnen delen tussen databases voor betere ondersteuning huurder de database per huurder aanpak. SQL-Database is een oplossing voor laag voor SaaS-aanbieders die multitenant toepassingen maken. De werklast van resources delen tussen huurders verplaatst van de application-laag naar de databaselaag van de service. De complexiteit van het beheren en uitvoeren van query's op schaal in databases wordt met elastische taken, elastische query elastische transacties en de elastische database client-bibliotheek vereenvoudigd.

| Vereisten van de toepassing | Mogelijkheden van SQL-database |
| ------------------------ | ------------------------- |
| Isolatie van de huurder en resources delen | [Elastische groepen](sql-database-elastic-pool.md): een geheugen van de SQL-Database en de bronnen delen tussen verschillende databases. Ook tekenen afzonderlijke databases zoveel bronnen uit de groep indien nodig aangepast aan de capaciteit vraag pieken als gevolg van wijzigingen in de werklast van de huurder. De elastische toepassingen zelf kan worden geschaald omhoog of omlaag wanneer dat nodig is. Elastische groepen ook een eenvoudige beheerbaarheid en monitoring en het oplossen van problemen op het niveau van toepassingen. |
| DevOps in de databases te vereenvoudigen | [Elastische groepen](sql-database-elastic-pool.md): zoals eerder is opgemerkt.|
||[Elastische query](sql-database-elastic-query-horizontal-partitioning.md): Query verschillende databases voor reporting of cross-huurder analyse.|
||[Elastische taken](sql-database-elastic-jobs-overview.md): pakket en veilig onderhoud databasebewerkingen of wijzigingen in de database schema te implementeren op meerdere databases.|
||[Elastische transacties](sql-database-elastic-transactions-overview.md): proces met diverse databases worden gewijzigd op een manier atomisch en geïsoleerd. Elastische nodig wanneer toepassingen 'Alles of niets' garanties via meerdere databasebewerkingen moeten. |
||[Elastische database client library](sql-database-elastic-database-client-library.md): verdelingen gegevens beheren en toewijzen van huurders aan databases. |

## <a name="shared-models"></a>Gedeelde modellen

Zoals eerder beschreven, voor de meeste providers SaaS, opleveren een benadering gedeeld model voor de problemen met de huurder isolatie problemen en complexiteit met de ontwikkeling van toepassingen en onderhoud. Echter, voor multitenant toepassingen die een service aan de consument aanbieden, huurder isolatie eisen mogelijk niet als hoge prioriteit als de kosten tot een minimum beperken. Ze mogelijk huurders inpakken in een of meer databases met een hoge dichtheid kosten te verlagen. Gedeelde database modellen met een enkele database of meerdere een laptopgeheugen databases kunnen leiden tot meer efficiëntie in delen en de totale kosten van de resource. Azure SQL-Database bevat enkele functies die klanten helpen isolatie voor betere beveiliging en beheer op schaal in de gegevenslaag maken.

| Vereisten van de toepassing | Mogelijkheden van SQL-database |
| ------------------------ | ------------------------- |
| Isolatie van beveiligingsfuncties | [Beveiliging](https://msdn.microsoft.com/library/dn765131.aspx) |
|| [Databaseschema](https://msdn.microsoft.com/library/dd207005.aspx) |
| DevOps in de databases te vereenvoudigen | [Elastische query](sql-database-elastic-query-horizontal-partitioning.md) |
|| [Elastische taken](sql-database-elastic-jobs-overview.md) |
|| [Elastische transacties](sql-database-elastic-transactions-overview.md) |
|| [Elastische database client library](sql-database-elastic-database-client-library.md) |
|| [Elastische database splitsen en samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Samenvatting

Huurder geïsoleerd moeten zijn van belang voor de meeste multitenant SaaS-toepassingen. De beste optie voor isolatie leans sterk ten opzichte van de database per huurder aanpak. De twee benaderingen moeten investeringen in complexe toepassingslagen waarvoor ontwikkeling bekwaam personeel te isoleren, die aanzienlijk meer kosten en risico's bieden. Als isolatie eisen niet van vroeg in de serviceontwikkeling verantwoorden, aanpassing ze worden zelfs duurder in de eerste twee modellen. De belangrijkste nadelen die is gekoppeld aan de database per huurder model zijn gerelateerd aan resourcekosten wolk toegenomen als gevolg van verminderde delen en onderhouden en beheren van veel databases. Ontwikkelaars van SaaS regelmatig te maken hebben wanneer zij deze afweging maken.

Hoewel e-mailverkeer mogelijk grote belemmeringen bij de meeste cloud database serviceproviders, elimineert Azure SQL-Database de barrières, met elastische toepassingen en mogelijkheden van elastische database. SaaS-ontwikkelaars kunnen de isolatie-eigenschappen van een model database per huurder combineren en delen van bronnen en de verbetering van de beheerbaarheid van veel databases optimaliseren met behulp van elastische van toepassingen en de bijbehorende hulpprogramma's.

Multitenant leveranciers die geen huurder geïsoleerd moeten hebben en die huurders kunt inpakken in een database op een hoge dichtheid kunnen vinden dat gedeelde gegevens resultaat van meer efficiëntie in het delen van bronnen modellen en verlagen de totale kosten. Azure SQL-Database elastische databasehulpmiddelen, sharding bibliotheken en beveiligingsfuncties helpen SaaS providers ontwikkelen en beheren van multitenant-toepassingen.

## <a name="next-steps"></a>Volgende stappen

[Aan de slag met elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md) met een monster app waarop u de clientbibliotheek.

Een [aangepast dashboard voor SaaS elastische toepassingen](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) maken met een monster app die gebruikmaakt van groepen van elastisch voor een kosteneffectieve, schaalbare database-oplossing.

Gebruik de Azure SQL-Database-hulpprogramma voor het [migreren van bestaande databases schaalt](sql-database-elastic-convert-to-use-elastic-tools.md).

Onze zelfstudie bekijken over het [maken van een elastische toepassingen](sql-database-elastic-pool-create-portal.md).  

Meer informatie over hoe u kunt [controleren en beheren van een elastische toepassingen](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>Aanvullende bronnen

- [Wat is een elastische Azure toepassingen?](sql-database-elastic-pool.md)
- [Geschaald uitbreiden met Azure SQL-Database](sql-database-elastic-scale-introduction.md)
- [Multitenant toepassingen met elastische Databasehulpmiddelen en beveiliging](sql-database-elastic-tools-multi-tenant-row-level-security.md)
- [Verificatie bij multitenant-toepassingen met behulp van Azure Active Directory en OpenID verbinding](../guidance/guidance-multitenant-identity-authenticate.md)
- [Toepassing van Tailspin enquêtes](../guidance/guidance-multitenant-identity-tailspin.md)
- [Snelle oplossing wordt gestart](sql-database-solution-quick-starts.md)

## <a name="questions-and-feature-requests"></a>Vragen en functie-aanvragen

Voor vragen, vindt u ons in het [forum SQL-Database](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Het verzoek om een functie in het [Feedbackforum voor SQL-Database](https://feedback.azure.com/forums/217321-sql-database/)toevoegen.

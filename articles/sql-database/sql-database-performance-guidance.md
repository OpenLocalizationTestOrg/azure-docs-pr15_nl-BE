<properties
    pageTitle="Azure SQL-Database en prestaties voor enkele databases | Microsoft Azure"
    description="In dit artikel kunt u vaststellen welke servicelaag om te kiezen voor uw toepassing. Het adviseert ook manieren om uw toepassing om optimaal te profiteren van Azure SQL-Database optimaliseren."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/13/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-and-performance-for-single-databases"></a>Azure SQL-Database en prestaties voor enkele databases

Azure SQL-Database biedt drie [niveaus van service](sql-database-service-tiers.md): Basic, Standard en Premium. Elke servicelaag geïsoleerd strikt de bronnen van de SQL-database kunt gebruiken en voorspelbare prestaties voor die service-niveau garandeert. In dit artikel bieden we richtlijnen waarmee u het niveau van de service voor uw toepassing te kiezen. Bespreken we ook manieren dat u uw toepassing optimaal van Azure SQL-Database kunt afstemmen.

>[AZURE.NOTE] In dit artikel ligt de nadruk op prestaties richtlijnen voor enkele databases in Azure SQL-Database. Zie [Overwegingen voor prijs- prestatieverhouding voor elastische database toepassingen](sql-database-elastic-pool-guidance.md)voor prestaties richtlijnen betrekking hebben op groepen van elastische database. Houd er rekening mee, maar u kunt veel van de tuning aanbevelingen in dit artikel van toepassing op databases in een elastische database toepassingen en voordelen van vergelijkbare prestaties.

Dit zijn de drie Servicelagen van Azure SQL-Database die u kunt kiezen uit (prestaties worden gemeten in eenheden van database doorvoer of [DTUs](sql-database-what-is-a-dtu.md):

- **Basic**. De voorspelbaarheid van Basic service tier biedt goede prestaties voor elke database, uur gedurende een uur. In een eenvoudige database ondersteuning voldoende middelen voor goede prestaties in een kleine database die geen meerdere gelijktijdige aanvragen.
- **Standaard**. De standaard service-laag biedt een verbeterde prestaties voorspelbaarheid en biedt voor databases met meerdere gelijktijdige aanvragen, zoals een werkgroep-en webtoepassingen. Wanneer u een database standaard service tier kiest, kunt u de databasetoepassing op basis van voorspelbare prestaties, groot minuut via minuut.
- **Premium**. De service Premium biedt voorspelbare prestaties, tweede over het tweede, voor elke database Premium. Als u de laag Premium service kiest, kunt u de databasetoepassing op basis van de belasting van die database te groot. Het plan wordt gevallen welke prestaties afwijking leiden kleine query's duurt langer tot kan dan verwacht in latentie gevoelige bewerkingen verwijderd. Dit model kan de ontwikkelings- en validatie cycli voor toepassingen die sterke instructies over de benodigde resources piek, prestaties afwijking of latentie van query moeten aanzienlijk vereenvoudigen.

Op elke servicelaag van de stelt het prestatieniveau van de, zodat u de flexibiliteit om te betalen alleen voor de capaciteit die u nodig hebt. U kunt [capaciteit aanpassen](sql-database-scale-up.md), omhoog of omlaag, als wijzigingen van de werklast. Als uw database werkbelasting tijdens het winkelen seizoen terug naar school hoog is, kunt u het prestatieniveau van de van de database vergroten voor een bepaalde periode juli tot en met September. Wanneer uw piek seizoen eindigt, kunt u deze verminderen. U kunt minimaliseren u betaalt met uw cloud-omgeving naar het seizoensgebonden karakter van uw bedrijf te optimaliseren. Dit model is ook geschikt voor software product release cycli. Een testteam kan capaciteit toewijzen tijdens deze wordt uitgevoerd test en deze capaciteit laat wanneer ze klaar bent met testen. In een model van de aanvraag capaciteit betaalt u voor capaciteit als u nodig hebt en besparen op specifieke resources die u zelden gebruikt.

## <a name="why-service-tiers"></a>Waarom voor service niveaus?

Hoewel de databasewerklast voor elke verschillen kan, is het doel van service niveaus voor voorspelbaarheid van de prestaties op verschillende prestatieniveaus. Klanten met een grote database resourcevereisten kunnen werken in een meer specifieke computeromgeving.

### <a name="common-service-tier-use-cases"></a>Algemene service tier use-cases

#### <a name="basic"></a>Basic

- **U bent net aan de slag met Azure SQL-Database**. Toepassingen die in ontwikkeling vaak hoeft niet hoge prestaties. Eenvoudig databases zijn een ideale omgeving voor de databaseontwikkeling van een, tegen een lage prijs.
- **U hebt een database met één gebruiker**. Toepassingen die meestal één gebruiker aan een database koppelen geen hoge eisen voor gelijktijdigheid en prestaties. Deze toepassingen zijn kandidaten voor de basisdienst laag.

#### <a name="standard"></a>Standaard

- **De database bevat meerdere gelijktijdige aanvragen**. Toepassingen die meer dan één gebruiker tegelijk meestal service moeten hogere prestaties. Websites die u normaal verkeer of afdelingen toepassingen die meer bronnen nodig hebben, zijn bijvoorbeeld geschikt voor de standaard service-laag.

#### <a name="premium"></a>Premium

Premium service tier gebruik meestal zijn een of meer van de volgende kenmerken:

- **Hoge piek laden**. Een toepassing waarvan een groot aantal CPU, geheugen of invoer/uitvoer (I/O) om de bewerkingen te voltooien is vereist voor een speciale hoge prestaties. Bijvoorbeeld is een databasebewerking bekend meerdere CPU-cores voor langere tijd in beslag neemt een kandidaat voor de Premium-tier-service.
- **Aantal gelijktijdige aanvragen**. Sommige databasetoepassingen service vele gelijktijdige aanvragen, bijvoorbeeld bij het bedienen van een website die is een intensief netwerkverkeer. Basic en Standard service niveaus beperken het aantal gelijktijdige aanvragen per database. Toepassingen waarvoor meer verbindingen nodig om de reserveringsgrootte van een nodig voor het verwerken van het maximum aantal benodigde aanvragen te kiezen.
- **Lage latentie**. Sommige toepassingen moeten garanderen een reactie van de database in een zo kort mogelijke tijd. Als een specifieke opgeslagen procedure wordt aangeroepen als onderdeel van een bredere klant bewerking, mogelijk een behoefte te hebben een rendement van die aanroep in 99 procent van de tijd niet meer dan 20 milliseconden. Dit type toepassing profiteert van de laag van een Premium service, om ervoor te zorgen dat de vereiste rekenkracht beschikbaar is.

Het serviceniveau van de die u nodig hebt voor de SQL-database is afhankelijk van de eisen van de piek belasting voor elke resourcedimensie. Sommige toepassingen gebruiken een trivial bedrag van één resource, maar belangrijke vereisten voor andere bronnen.

## <a name="service-tier-capabilities-and-limits"></a>Laag mogelijkheden en beperkingen
Elk serviceniveau laag en de prestaties is gekoppeld aan verschillende limieten en prestatie-eigenschappen. Deze tabel wordt beschreven van deze kenmerken voor een enkele database.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

In de volgende secties hebt meer informatie over het weergeven van gebruik verband houdt met deze beperkingen.

### <a name="maximum-in-memory-oltp-storage"></a>Maximale In Memory OLTP-opslag

De weergave van de **sys.dm_db_resource_stats** kunt u uw Azure In het geheugen opslag gebruik te controleren. Zie voor meer informatie over het controleren van [Monitor In Memory OLTP-opslag](sql-database-in-memory-oltp-monitoring.md).

>[AZURE.NOTE] Op dit moment wordt Azure In het geheugen online transaction processing (OLTP)-voorbeeld alleen ondersteund voor databases op één. U niet gebruiken in databases in elastische database-toepassingen.

### <a name="maximum-concurrent-requests"></a>Maximum aantal gelijktijdige aanvragen

Overzicht van het aantal gelijktijdige aanvragen deze Transact-SQL-query in de SQL-database worden uitgevoerd:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Voor het analyseren van de werklast van een SQL Server-database op locatie wijzigen deze query kunt u filteren op de specifieke database die u wilt analyseren. Bijvoorbeeld als er een op gebouwen-database met de naam MijnDatabase, deze Transact-SQL-query als resultaat het aantal gelijktijdige aanvragen in de database:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Dit is slechts een momentopname op één punt in de tijd. Als u een beter beeld van uw werkbelasting en gelijktijdige aanvraag vereisten, moet u veel om monsters te nemen na verloop van tijd.

### <a name="maximum-concurrent-logins"></a>Maximale gelijktijdige aanmeldingen

U kunt de gebruikers- en patronen krijgt u een indruk van de frequentie van de aanmeldingen analyseren. U kunt ook werkelijke belasting in een testomgeving om er zeker van te zijn dat u bent niet kunt u door deze of andere beperkingen die in dit artikel we bespreken uitvoeren. Er is niet een enkele query of beheer van dynamische weergave (DMV) waarin u gelijktijdige telt aanmelding of geschiedenis.

Als u meerdere clients gebruiken dezelfde verbindingsreeks opgeeft, wordt de service geverifieerd voor elke aanmelding. Als u 10 gebruikers tegelijk verbinding met een database met behulp van dezelfde gebruikersnaam en hetzelfde wachtwoord, zou er 10 gelijktijdige aanmeldingen. Deze limiet geldt alleen voor de duur van de aanmelding en verificatie. Als u dezelfde 10 gebruikers verbinding met de database na elkaar, zou het aantal gelijktijdige aanmeldingen nooit groter zijn dan 1.

>[AZURE.NOTE] Momenteel is deze beperking niet van toepassing op databases in elastische database toepassingen.

### <a name="maximum-sessions"></a>Maximum aantal sessies

Overzicht van het nummer van de huidige actieve sessies uitvoeren deze Transact-SQL-query in de SQL-database:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Als u bent een werkbelasting van de SQL Server op locatie geanalyseerd, wijzigt u de query op te richten op een specifieke database. Deze query kunt u mogelijke behoeften voor de database bepalen als u overweegt te verplaatsen naar Azure SQL-Database.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Nogmaals, deze query's geteld point-in-time. Als u meerdere monsters na verloop van tijd verzamelen, hebt u het beste inzicht in uw sessie gebruiken.

Voor de analyse van de SQL-Database krijgt u historische statistieken op sessies. **Sys.resource_stats**opvragen en gebruik de kolom **active_session_count** . Zie de volgende sectie voor meer informatie over het gebruik van deze weergave.

## <a name="monitor-resource-use"></a>Brongebruik controleren
Twee weergaven kunt u het bronnengebruik voor een SQL-database ten opzichte van het niveau van de service:

- [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
- [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
U kunt de weergave van [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) in elke SQL-database. De weergave **sys.dm_db_resource_stats** bevat recente gegevens resource gebruiken ten opzichte van het niveau van de service. Gemiddelde percentages voor CPU, I/O gegevens schrijft log en geheugen om de 15 seconden worden geregistreerd en worden onderhouden voor 1 uur.

Omdat deze weergave een nauwkeurigere blik op het gebruik van bronnen biedt, gebruiken **sys.dm_db_resource_stats** eerste voor een analyse van de huidige status of het oplossen van problemen. Met deze query worden bijvoorbeeld het gebruik van gemiddelde en maximale resource voor de huidige database in het afgelopen uur:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data I/O in percent',
        MAX(avg_data_io_percent) AS 'Maximum data I/O in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Zie de voorbeelden in de [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)voor andere query's.

### <a name="sysresourcestats"></a>sys.resource_stats

De weergave van [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) in **de hoofddatabase** biedt extra informatie kunt u de prestaties van de SQL-database op de specifieke service tier en prestaties niveau controleren. De gegevens worden verzameld elke 5 minuten en ongeveer 35 dagen wordt bijgehouden. Deze weergave is handig voor een langere historische analyse van hoe bronnen worden gebruikt in de SQL-database.

Het volgende diagram ziet u de CPU gebruik van bronnen voor een database van de premie en het prestatieniveau P2 voor elk uur in een week. Deze grafiek op een maandag start, ziet u 5 werkdagen en geeft vervolgens een weekend, wanneer veel minder op de toepassing gebeurt.

![Gebruik van bronnen voor SQL-database](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Van de gegevens, heeft deze database momenteel een piek CPU-belasting van iets meer dan 50 procent CPU-gebruik ten opzichte van het prestatieniveau P2 (twaalf uur 's middags op dinsdag). Als de CPU is de dominante factor bij het profiel van de bron van de toepassing, dan is het misschien dat P2 de juiste prestatieniveau om ervoor te zorgen is dat de werklast altijd past. Als u een toepassing wilt blijven groeien, is het een goed idee om een extra bron buffer hebben, zodat de toepassing niet de maximale prestaties niveau ooit bereikt. Als u het prestatieniveau van de verhogen, kunt u klant zichtbare fouten die optreden kunnen als een database geen voldoende stroom om aanvragen te verwerken effectief, met name in omgevingen met gevoelige vertraging te vermijden. Een voorbeeld is een database die door een toepassing die u tekent u webpagina's op basis van de resultaten van het database-aanroepen worden ondersteund.

Houd er rekening mee dat andere types applicatie dezelfde grafiek anders kunnen interpreteren. Bijvoorbeeld als een toepassing probeert te verwerken gegevens van elke dag heeft dezelfde grafiek, kunt dit soort "batchverwerking" model doen prima op een niveau van P1 prestaties. Het prestatieniveau P1 heeft 100 DTUs vergeleken met 200 DTUs op het niveau van de prestaties van P2. Het niveau van P1 prestaties biedt de helft van de prestaties van het prestatieniveau van P2. 50 procent van de CPU-gebruik in P2 is dus gelijk aan 100 procent CPU-gebruik in P1. Als de toepassing geen time-outs, maakt het niet uit als een taak duurt 2 uur of 2,5 uur te voltooien, als het vandaag de dag wordt gedaan. Een toepassing in deze categorie kan waarschijnlijk een P1 prestatieniveau gebruiken. U kunt profiteren van het feit dat er perioden gedurende de dag als gebruik van bronnen lager, is zodat een 'groot 'piek in een van de holten later op de dag passen kan. Het prestatieniveau P1 mogelijk goed voor dit soort van toepassing (en geld besparen) zolang de taken op tijd elke dag kunnen worden voltooid.

Azure SQL-Database worden gegevens over resources voor elke actieve database in de weergave van de **sys.resource_stats** van **de hoofddatabase in elke server** verbruikt. De gegevens in de tabel worden samengevoegd voor 5 minuten. Met de niveaus Basic, Standard en Premium-service kan de gegevens worden weergegeven in de tabel, zodat deze gegevens vooral nuttig om historische analyse in plaats van in de buurt van de real-time analyse is meer dan 5 minuten duren. Query uitvoeren op de **sys.resource_stats** voor een overzicht van de recente historie van een database en wilt controleren of de reservering die u hebt gekozen geleverde prestaties van de gewenste weergave wanneer dit nodig is.

>[AZURE.NOTE] U moet worden verbonden met de **master** -database van de logische SQL databaseserver opvragen van **sys.resource_stats** in de volgende voorbeelden.

In dit voorbeeld ziet u hoe de gegevens in deze weergave wordt weergegeven:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![De weergave van de catalogus sys.resource_stats](./media/sql-database-performance-guidance/sys_resource_stats.png)

In het volgende voorbeeld ziet u verschillende manieren kunt u de weergave van de catalogus **sys.resource_stats** voor informatie over hoe bronnen worden gebruikt in de SQL-database:

1. Te kijken naar de bron van de afgelopen week voor de userdb1 van de database, kunt u deze query uitvoeren:

        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;

2. Om te beoordelen hoe goed uw werkbelasting past bij het prestatieniveau van de, moet u inzoomen op elk aspect van de parameters bron: CPU, leesbewerkingen schrijfbewerkingen, aantal werknemers en het aantal sessies. Dit is een herziene query **sys.resource_stats** met de gemiddelde en maximale waarden van de parameters voor deze resource wordt gerapporteerd:

        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data I/O use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data I/O use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

3. Met deze gegevens over de gemiddelde en maximale waarden van elke resource metrische gegevens kunt u beoordelen hoe goed uw werkbelasting past in het prestatieniveau dat u hebt gekozen. Meestal bieden gemiddelde waarden van **sys.resource_stats** een goede basis te gebruiken ten opzichte van de doelgrootte. Uw primaire meting stick moet worden. Voor een voorbeeld, u gebruikt de standaard service-laag met S2 prestatieniveau. De gemiddelde percentages gebruiken voor CPU en i/o-leesbewerkingen en schrijfbewerkingen zijn onder de 40 procent, het gemiddelde aantal werknemers is dan 50 en het gemiddelde aantal sessies lager is dan 200. Uw werkbelasting mogelijk passen in het prestatieniveau van S1. Het is eenvoudig om te zien of de database in de sessie en werknemer grenzen past. Als u wilt zien of een database op een lager prestatieniveau met betrekking tot de CPU aansluit, leest en schrijft, verdeelt u het nummer van het lagere prestatieniveau door het aantal DTU van uw huidige prestatieniveau van de DTU en het resultaat vervolgens vermenigvuldigd met 100:

    * *S1 DTU / S2 DTU* 100 = 20 / 50* 100 = 40 **

    Het resultaat is de relatieve prestaties verschil tussen de twee prestaties in een percentage. Als uw gebruik van de resource niet hoger is dan dit bedrag, kan uw werkbelasting past in het lagere prestatieniveau. Echter, moet u alle bereiken van waarden voor het gebruik van resources bekijken en vaststellen, door het percentage, hoe vaak uw werkbelasting database zou passen in het lagere prestatieniveau. De volgende query levert het aanpassingspercentage per resourcedimensie, gebaseerd op de drempel van 40 procent die we in dit voorbeeld berekend:

        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Op basis van uw database service level doelstelling (SLO), kunt u bepalen of uw werkbelasting in het lagere prestatieniveau past. Als uw database werkbelasting SLO 99,9 procent en de voorgaande query geeft als waarden groter dan 99,9 procent voor alle drie resourcedimensies resultaat, past het lagere prestatieniveau van uw werkbelasting waarschijnlijk.

    Het aanpassingspercentage kijken biedt ook inzicht in of u hebt om te verplaatsen naar het eerstvolgende hogere prestatieniveau aan de SLO. Userdb1 geeft bijvoorbeeld de volgende CPU-gebruik voor de afgelopen week:

  	| Gemiddelde CPU-percentage | Maximumpercentage CPU |
  	|---|---|
  	| 24,5 | 100,00 |

    De gemiddelde CPU is ongeveer een kwart van de limiet van het prestatieniveau dat zijn voor het prestatieniveau van de database geschikt zou. Maar de maximale waarde wordt aangegeven dat de database de limiet van het prestatieniveau bereikt. Moet u naar het eerstvolgende hogere prestatieniveau? U hebt om te kijken hoe vaak uw werkbelasting bereikt 100 procent en vergelijk deze met uw database werkbelasting SLO.

        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent’
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data I/O fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Als deze query geeft als resultaat een waarde minder dan 99,9 procent voor elk van de drie resourcedimensies, kunt u beide verplaatsen naar het eerstvolgende hogere prestatieniveau of afstemming van toepassingen technieken ter vermindering van de belasting op de SQL-database.

4. In deze oefening ook rekening gehouden met de stijging van de verwachte werklast in de toekomst.

## <a name="tune-your-application"></a>Afstemmen van uw toepassing

In traditionele op ruimten SQL Server, wordt het proces van het eerste capaciteitsplanning vaak gescheiden van het proces van het uitvoeren van een toepassing in de productie. Hardware- en licenties worden gekocht, eerste en daarna het afstemmen van prestaties is gedaan. Wanneer u met Azure SQL-Database, is het een goed idee om het proces van het uitvoeren van een toepassing en het afstemmen van het interweave. Met het model van betalen voor capaciteit op verzoek, kunt u uw toepassing de minimaal vereiste bronnen nodig nu, in plaats van op hardware gebaseerd op wat van plannen voor toekomstige groei voor een toepassing, die vaak onjuiste worden overprovisioning afstemmen. Sommige klanten kunnen ervoor kiezen niet voor het afstemmen van een toepassing, maar in plaats daarvan overprovision hardwarebronnen. Deze benadering kan een goed idee zijn als u niet wilt dat een belangrijke toepassing in een drukke periode aan te passen. Maar een toepassing afstemmen kunt minimaliseren resourcevereisten en maandelijkse rekeningen te verlagen wanneer u de service niveaus in Azure SQL-Database.

### <a name="application-characteristics"></a>Kenmerken van toepassingen

Hoewel Azure SQL Database service niveaus zijn ontworpen voor het verbeteren van prestaties, stabiliteit en voorspelbaarheid voor een toepassing, kunt aanbevolen procedures u de toepassing beter gebruik te maken van de bronnen op een prestatieniveau afstemmen. Hoewel veel toepassingen aanzienlijke prestatieverhogingen hebben gewoon door over te schakelen naar een hoger prestatieniveau of service tier, sommige toepassingen moeten extra afstemmen om te profiteren van een hoger niveau van dienstverlening. Voor betere prestaties kunt u eventueel extra toepassing afstemmen voor toepassingen die deze kenmerken hebben:

- **Toepassingen die traag vanwege 'chatty' gedrag hebben**. Chatty toepassingen maken veel gegevens toegangsbewerkingen die gevoelig voor netwerkvertraging zijn. Mogelijk moet u dit soort toepassingen te verminderen van het aantal gegevens toegang tot de SQL-database wijzigen. U kan bijvoorbeeld toepassingsprestaties verbeteren met behulp van technieken zoals batchen van ad-hoc query's of de query's verplaatsen naar opgeslagen procedures. Zie [Batch query's](#batch-queries)voor meer informatie.
- **Databases met een intensieve belasting die door een hele enkele machine kan niet worden ondersteund**. Databases die groter zijn dan de middelen van het hoogste niveau van Premium prestaties kunnen profiteren van geschaalde uitbreiding van de werkbelasting. Zie [meerdere databases sharding](#cross-database-sharding) en [functionele partitioneren](#functional-partitioning)voor meer informatie.
- **Toepassingen die een optimale query's hebben**. Toepassingen, met name de in de gegevenstoegangslaag die query's niet goed afgestemd niet nuttig kan zijn van een hoger prestatieniveau. Dit geldt ook voor query's die niet over een WHERE-component ontbreekt de indexen zijn, of verouderde statistieken. Deze toepassingen profiteren van de standaardoperators voor query's performance tuning technieken. Zie voor meer informatie, [ontbrekende indexen](#missing-indexes) en [Query tuning en coderingstips](#query-tuning-and-hinting).
- **Toepassingen die niet-optimale gegevens hebben toegang tot ontwerp**. Toepassingen die gegevens inherente gelijktijdigheid problemen, bijvoorbeeld deadlocking, hebben mogelijk niet profiteren van een hoger prestatieniveau. Beperk retouren tegen de Azure SQL-Database door gegevens van caching op de client met de Azure Caching-service of een ander caching-technologie. Zie de [Application-laag caching](#application-tier-caching).

## <a name="tuning-techniques"></a>Tuning technieken
In deze sectie bekijken we enkele technieken die u gebruiken kunt om af te stemmen Azure SQL-Database om de beste prestaties voor uw toepassing te krijgen en deze op het laagste niveau van de mogelijke prestaties. Sommige van deze technieken overeenkomen met traditionele SQL Server afstemmen van beste praktijken, maar andere zijn specifiek voor Azure SQL-Database. In sommige gevallen kunt u de verbruikte resources voor een database om te zoeken naar gebieden verder afstemmen en technieken met traditionele SQL Server werkt in Azure SQL-Database uitbreiden te bestuderen.

### <a name="azure-portal-tools"></a>Azure portal-hulpprogramma 's
Hebt u twee extra in Azure portal waarmee u kunt analyseren en oplossen van prestatieproblemen met de SQL-database:

- [Query prestaties inzicht](sql-database-query-performance.md)
- [SQL-Database-adviseur](sql-database-advisor.md)

De Azure portal bevat meer informatie over beide van deze hulpprogramma's en hoe u ze gebruikt. Efficiënt opsporen en corrigeren van problemen, wij raden u aan eerst de hulpprogramma's in de portal Azure. Wij raden u aan gebruik te maken van de handleiding van de benaderingen die we vervolgens bespreken indexen en query tuning in bijzondere gevallen ontbrekende afstemmen.

### <a name="missing-indexes"></a>Ontbrekende indexen
Een veelvoorkomend probleem in de prestaties van de OLTP-databases in relatie tot het ontwerp van de fysieke database. Database schema's zijn vaak ontworpen en getest op schaal (hetzij in laden of gegevensvolume) verzonden. De prestaties van een queryplan kan helaas op kleine schaal aanvaardbaar, maar aanzienlijk afnemen onder productie niveau gegevensvolumes. De meest voorkomende oorzaak van dit probleem is het ontbreken van de juiste indexen om te voldoen aan de filters of andere beperkingen die in een query. Vaak scannen ontbrekende indexen manifesten als een tabel bij het zoeken van een index kan worden volstaan.

In dit voorbeeld wordt de geselecteerde query-plan een scan wanneer seek zouden volstaan:

    DROP TABLE dbo.missingindex;
    CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
    SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;

![Een queryplan met ontbrekende indexen](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Azure SQL-Database kunt u zoeken en fix common ontbrekende index voorwaarden. DMVs die zijn ingebouwd in Azure SQL-Database bekijken querycompilaties waarin een index aanzienlijk sneller de geschatte kosten voor een query uitvoeren. Tijdens de uitvoering van de query, SQL-Database wordt bijgehouden hoe vaak elk queryplan is uitgevoerd en de geschatte kloof tussen het queryplan uitvoerende en de imagined bijgehouden waar die index bestond. U kunt deze DMVs snel achterhalen welke wijzigingen in het ontwerp van de fysieke database kunnen de totale kosten van de werklast voor een database en de reële werklast verbeteren.

U kunt deze query voor de evaluatie van potentiële ontbrekende indexen:

    SELECT CONVERT (varchar, getdate(), 126) AS runtime,
        mig.index_group_handle, mid.index_handle,
        CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
                (migs.user_seeks + migs.user_scans)) AS improvement_measure,
        'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
                  CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
                  (' + ISNULL (mid.equality_columns,'')
                  + CASE WHEN mid.equality_columns IS NOT NULL
                              AND mid.inequality_columns IS NOT NULL
                         THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '')
                  + ')'
                  + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
        migs.*,
        mid.database_id,
        mid.[object_id]
    FROM sys.dm_db_missing_index_groups AS mig
    INNER JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    INNER JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
    ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC

In dit voorbeeld wordt de query heeft geresulteerd in deze suggestie:

    CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  

Nadat deze gemaakt, picks die dezelfde SELECT-instructie in een ander schema en welke seek gebruikt in plaats van een scan en vervolgens het plan efficiënter wordt uitgevoerd:

![Een queryplan met gecorrigeerde indexen](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

De belangrijkste inzicht is dat de i/o-capaciteit van een gedeelde, grondstoffen-systeem beperkter dan die van een speciale servermachine is. Er is een premie op het minimaliseren van onnodige I/O profiteren maximaal van het systeem in de DTU van elk prestatieniveau van de SQL-Database Azure service niveaus. Juiste fysieke databaseontwerp opties kunnen u de wachttijden voor afzonderlijke query's aanzienlijk verbeteren de doorvoer van gelijktijdige aanvragen verwerkt per schaaleenheid verbeteren en minimaliseren van de kosten die nodig zijn om te voldoen aan de query. Zie [sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx)voor meer informatie over de ontbrekende index DMVs.

### <a name="query-tuning-and-hinting"></a>Query afstemmen en coderingstips
Queryoptimalisatie in Azure SQL-Database is vergelijkbaar met de traditionele query optimizer voor SQL Server. De meeste van de beste praktijken voor het afstemmen van query's en informatie over de redenering model beperkingen voor de query optimizer ook van toepassing op Azure SQL-Database. Als u query's in SQL-Database Azure afstemmen, krijgt u mogelijk het extra voordeel van statistische systeembronnen te verminderen. Uw toepassing mogelijk uit te voeren tegen lagere kosten dan een ruiseffect gelijkwaardige omdat deze kan worden uitgevoerd op een lager prestatieniveau.

Een voorbeeld dat is gebruikelijk in SQL Server en dat geldt ook voor Azure SQL-Database is hoe de query optimizer "bithandtekeningen" parameters. De query optimizer geëvalueerd tijdens de compilatie, de huidige waarde van een parameter om te bepalen of er meer optimale queryplan genereren. Hoewel deze strategie vaak tot een queryplan dat is aanzienlijk sneller dan een plan gecompileerd zonder bekende parameterwaarden leiden kan, momenteel werkt dat beide in SQL Server en Azure SQL-Database. Soms wordt de parameter niet sniffed, en soms de parameter is sniffed maar het gegenereerde plan is voor de volledige set van de parameterwaarden in de werkbelasting van een niet-optimale. Microsoft levert de query hints (richtlijnen) zodat u kunt meer bewust intentie opgeven en het standaardgedrag van de parameter sniffing overschrijven. Als u de aanbevolen basisservers gebruikt, kunt u vaak gevallen waarin het standaardgedrag voor SQL Server- of Azure SQL-Database voor een bepaalde klant werkbelasting gebrekkige is oplossen.

In het volgende voorbeeld wordt gedemonstreerd hoe de queryprocessor een plan dat is een niet-optimale zowel voor prestaties en resourcevereisten kunt genereren. In dit voorbeeld wordt ook weergegeven als u een geheugensteun query gebruikt, u uitvoeren query veel tijd en middelen eisen voor uw SQL-database verkleinen kunt:

    DROP TABLE psptest1;
    CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));

    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO psptest1(col2) values (1);
        INSERT INTO psptest1(col2) values (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION
    CREATE INDEX i1 on psptest1(col2);
    GO

    CREATE PROCEDURE psp1 (@param1 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1
        WHERE col2 = @param1
        ORDER BY col2;
    END
    GO

    CREATE PROCEDURE psp2 (@param2 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
        ORDER BY col2
        OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
    END
    GO

    CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
    GO

De Setupcode maakt een tabel die de verdeling van de gegevens is scheef. De optimale queryplan verschilt op basis van welke parameter is geselecteerd. Helaas het plan in cache plaatsen van gedrag niet altijd opnieuw compileren van de query is gebaseerd op de meest voorkomende parameterwaarde. Het is dus mogelijk voor een niet-optimale plan worden in de cache opgeslagen en gebruikt voor vele waarden, zelfs als u een ander schema misschien een betere keuze voor plan gemiddeld. Het queryplan maakt twee opgeslagen procedures die identiek zijn, met dien verstande dat een een speciale queryaanwijzing heeft.

**Voorbeeld, deel 1**

    -- Prime Procedure Cache with scan plan
    EXEC psp1 @param1=1;
    TRUNCATE TABLE t1;

    -- Iterate multiple times to show the performance difference
    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp1 @param1=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

**Voorbeeld, deel 2**

(Aangeraden minimaal 10 minuten voordat u begint met deel 2 van het voorbeeld te wachten, zodat de resultaten zijn te onderscheiden in de resulterende telemetriegegevens.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

Elk onderdeel van dit voorbeeld probeert uit te voeren met parameters invoeginstructie 1000 keer (voor het genereren van een voldoende belasting moet worden gebruikt als een data test set). Bij het uitvoeren van opgeslagen procedures, onderzoekt de queryprocessor waarde van de parameter die wordt doorgegeven aan de procedure tijdens de eerste compilatie (parameter ' sniffing"). De processor slaat de resulterende plan en wordt gebruikt voor latere aanroepen, zelfs als de waarde van de parameter is anders. De optimale indeling mogelijk niet in alle gevallen worden gebruikt. Soms moet u helpen om de optimizer om te selecteren, een plan dat is beter voor het gemiddelde geval in plaats van het specifieke geval van wanneer de query voor het eerst is gecompileerd. In het volgende voorbeeld genereert het oorspronkelijke plan een plan 'scan' die alle rijen om elke waarde die overeenkomt met de parameter wordt gelezen:

![Query's afstemmen met behulp van een scan-plan](./media/sql-database-performance-guidance/query_tuning_1.png)

Omdat we de procedure uitgevoerd met de waarde 1, het resulterende plan is optimaal voor de waarde 1, maar is een niet-optimale voor alle waarden in de tabel. Het resultaat waarschijnlijk niet wat u wilt als u willekeurig, elk plan kiezen omdat het plan wordt langzamer uitgevoerd en meer bronnen worden gebruikt.

Als u de test met `SET STATISTICS IO` ingesteld op `ON`, de logische scan werk in dit voorbeeld wordt uitgevoerd op de achtergrond. U ziet dat er 1,148 leest gedaan door het plan (dit is inefficiënt als de gemiddelde zaak is om terug te keren maar één rij):

![Query's afstemmen met behulp van een logische scan](./media/sql-database-performance-guidance/query_tuning_2.png)

Het tweede gedeelte van het voorbeeld wordt een queryaanwijzing vertellen de optimizer is op een specifieke waarde gebruiken tijdens het compileren. In dat geval wordt u gedwongen wordt de queryprocessor negeert de waarde die wordt doorgegeven als parameter, en in plaats daarvan over te nemen `UNKNOWN`. Dit is een waarde die de gemiddelde frequentie in de tabel heeft (niet scheeftrekken). Het resulterende plan is een op basis van seek plan dat is sneller en gebruikt minder resources gemiddeld dan het plan in deel 1 van dit voorbeeld:

![Query afstemmen met behulp van een queryaanwijzing](./media/sql-database-performance-guidance/query_tuning_3.png)

Ziet u het effect in de tabel **sys.resource_stats** (Er is een vertraging vanaf het moment dat u de test en wanneer de gegevens in de tabel gevuld uitvoeren). Dit bijvoorbeeld uitgevoerd tijdens het tijdvenster 22:25:00 voor deel 1 en deel 2 uitgevoerd om 22:35:00. Houd er rekening mee dat de eerdere tijdvenster meer bronnen in dat venster keer hoger dan de (dankzij verbeteringen van de efficiëntie plan) gebruikt.

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Tuning voorbeeld queryresultaten](./media/sql-database-performance-guidance/query_tuning_4.png)

>[AZURE.NOTE] Hoewel het volume in dit voorbeeld opzettelijk kleine is, kan het effect van niet-optimale parameters, met name op grotere databases aanzienlijk zijn. Het verschil in extreme gevallen kan liggen tussen seconden snel gevallen en uren voor trage aanvragen.

Bekijk de **sys.resource_stats** om te bepalen of de bron voor een test resources meer of minder dan een andere test gebruikt. Bij het vergelijken van gegevens tussen de timing van de proeven zodat deze zich niet in hetzelfde venster van 5 minuten in de weergave van de **sys.resource_stats** . Het doel van de oefening is tot een minimum beperken van het totale bedrag van de bronnen die worden gebruikt en niet op de piek-resources minimaliseren. In het algemeen minder een stukje code voor latentie optimaliseren ook verbruik. Zorg ervoor dat de wijzigingen die u in een toepassing aanbrengt nodig zijn en de wijzigingen niet een negatieve invloed op de gebruikerservaring voor iemand die mogelijk met hints van de query in de toepassing.

Als een werkbelasting een aantal query's herhalen heeft, logischer vaak vastleggen en valideren van de optimalisatie van uw keuzes plan omdat deze de de minimale resource grootte-eenheid vereist voor het hosten van de database. Nadat u deze valideren klikken af en toe de plannen om te helpen u ervoor te zorgen dat zij niet zijn aangetast. U kunt meer informatie over [query hints (Transact-SQL)](https://msdn.microsoft.com/library/ms181714.aspx).

### <a name="cross-database-sharding"></a>Cross-database sharding
Omdat Azure SQL-Database wordt uitgevoerd op de hardware van de grondstoffen, zijn de grenzen van de capaciteit voor één database lager dan bij traditionele op ruimten installatie van SQL Server. Sommige klanten gebruiken technieken sharding databasebewerkingen verspreid over meerdere databases wanneer de bewerkingen niet binnen de grenzen van één database in Azure SQL-Database past. De meeste klanten die gebruikmaken van technieken voor sharding in Azure SQL-Database splitsen hun gegevens op één dimensie over meerdere databases. U moet begrijpen OLTP-toepassingen voeren vaak transacties die betrekking hebben op slechts één rij of naar een kleine groep van rijen in het schema voor deze benadering.

>[AZURE.NOTE] SQL-Database biedt nu een bibliotheek om te helpen bij het sharding. Zie [overzicht van elastische Database client library](sql-database-elastic-database-client-library.md)voor meer informatie.

Bijvoorbeeld, als een database heeft de naam van de klant, orders en Orderinformatie (zoals in het voorbeeld van de traditionele voorbeelddatabase die wordt geleverd bij SQL Server), kan u splitsen deze gegevens in meerdere databases door het groeperen van een klant met de verwante order en order details. U kunt garanderen dat de gegevens van de klant in één database blijft. De toepassing zou verschillende klanten verschillende databases, effectief de belasting te spreiden over meerdere databases opsplitsen. Klanten niet alleen kunnen voorkomen dat u de maximale limiet databasegrootte met sharding, maar Azure SQL-Database ook werklast die aanzienlijk groter dan de grenzen van de verschillende prestatieniveaus zijn, zolang elke afzonderlijke database in de DTU past kan verwerken.

Database sharding niet verminderen de totale resourcecapaciteit voor een oplossing, is maar het zeer effectief bij zeer grote oplossingen die verspreid zijn over meerdere databases ondersteunen. Elke database kunt een verschillende prestatie-niveau ter ondersteuning van zeer grote "effectieve" databases met hoge resourcevereisten uitvoeren.

### <a name="functional-partitioning"></a>Functionele partitioneren
SQL Server-gebruikers combineren vaak veel functies in één database. Als een toepassing logica voor het beheren van de voorraad voor een winkel heeft, kan die database logica die is gekoppeld aan de voorraad, inkooporders, opgeslagen procedures en weergaven voor geïndexeerde of gerealiseerde die het einde van de maand rapportage beheren bijhouden hebt. Deze techniek kunt u gemakkelijker bewerkingen, zoals back-up van de database te beheren, maar ook moet u het formaat van de hardware voor het afhandelen van de belasting over alle functies van een toepassing.

Als u een scale-out architectuur in Azure SQL-Database gebruikt, is het een goed idee om de verschillende functies van een toepassing in verschillende databases splitsen. Met behulp van deze techniek, elke toepassing afzonderlijk geschaald. Als een toepassing veelgebruikte wordt (en de belasting van de database verhoogt), kunt de beheerder van de onafhankelijke prestatieniveaus voor elke functie in de toepassing. Een toepassing kan worden groter is dan een machine met één product kan worden verwerkt omdat de belasting wordt verdeeld over meerdere computers aan de grens, met deze architectuur.

### <a name="batch-queries"></a>Batch-query 's
Voor toepassingen die toegang hebben tot gegevens met behulp van grote hoeveelheden, vaak ad hoc query wordt uitgevoerd, een aanzienlijke hoeveelheid tijd antwoord is besteed aan netwerkcommunicatie tussen de toepassingslaag en de databaselaag Azure SQL. Zelfs als de toepassing en de Azure SQL-Database in het datacenter van hetzelfde zijn, kan de netwerkvertraging tussen de twee worden vergroot door een groot aantal gegevens toegankelijk. Het netwerk verminderen trips-ronde voor de data access-bewerkingen, kunt u met de optie batch-ofwel de ad-hoc query's of voor het compileren van als opgeslagen procedures. Als u batch de ad hoc-query's, kunt u meerdere query's verzenden als één grote batch in een enkele reis naar Azure SQL-Database. Als u bij het compileren van ad-hoc query's in een opgeslagen procedure kan leveren hetzelfde resultaat als wanneer u deze batchverwerking. Een opgeslagen procedure kunt u ook het voordeel van het verhogen van de kans dat de query's in Azure SQL-Database opslaan in de cache zodat u de opgeslagen procedure opnieuw kunt gebruiken.

Sommige toepassingen zijn veel schrijven. Soms kunt u de totale i/o-belasting van een database op basis van de batch schrijft verminderen. Dit is vaak net zo eenvoudig als expliciete transacties gebruiken in plaats van de transacties automatisch doorvoeren in opgeslagen procedures en ad hoc-batches. U kunt voor een evaluatie van de verschillende technieken gebruiken, Zie [Batching technieken voor toepassingen in Azure SQL-Database](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx). Experimenteren met uw eigen werkbelasting te vinden van het juiste model voor batchverwerking. Zorg ervoor dat er mogelijk iets anders transactionele consistentie waarborgen een model begrijpen. Zoeken naar de juiste werkbelasting die tot een minimum gebruik van bronnen beperkt moet zoeken naar de juiste combinatie van consistentie en prestaties-en nadelen.

### <a name="application-tier-caching"></a>Toepassingslaag caching
Bepaalde databasetoepassingen hebben alleen zware werkbelasting. Lagen caching kan de werklast verminderen van de database en mogelijk het prestatieniveau van de vereist voor de ondersteuning van een database met behulp van Azure SQL-Database kan worden verlaagd. Met [Azure bestand Vgx. Cache](https://azure.microsoft.com/services/cache/), hebt u een lezen zware werkbelasting, vindt u de gegevens eenmaal (of bijvoorbeeld eenmaal per toepassing-tier machine, afhankelijk van de manier waarop deze is geconfigureerd), en die gegevens buiten de SQL-database op te slaan. Dit is een manier te verminderen database laden (CPU en I/O lezen), maar er is een effect op de transactionele consistentie omdat de gegevens worden gelezen uit de cache gesynchroniseerd met de gegevens in de database. Hoewel in veel toepassingen sommige inconsistentie aanvaardbaar is, dat geldt niet voor alle werkbelasting. Vereisten van de toepassing moet u volledig begrijpen voordat u een toepassingslaag caching-strategie implementeren.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over service tiers [SQL Database-opties en prestaties](sql-database-service-tiers.md)
- Zie voor meer informatie over groepen van elastische database [Wat is een database met Azure elastische toepassingen?](sql-database-elastic-pool.md)
- Zie voor meer informatie over prestaties en elastische database toepassingen [Wanneer kunt u een elastische database toepassingen](sql-database-elastic-pool-guidance.md)

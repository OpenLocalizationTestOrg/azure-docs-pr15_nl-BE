<properties
    pageTitle="Compatibiliteitsniveau, beoordelen | Microsoft Azure"
    description="Stappen en hulpmiddelen om te bepalen welk compatibiliteitsniveau is het meest geschikt voor uw op Azure SQL-Database of Microsoft SQL Server-database"
    services="sql-database"
    documentationCenter=""
    authors="alainlissoir"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.devlang="NA"
    ms.tgt_pltfrm="NA"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="alainl"/>


# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>Verbeterde prestaties van query's en compatibiliteit niveau 130 in Azure SQL-Database


Azure SQL-Database wordt uitgevoerd transparant honderdduizenden databases op veel verschillende compatibiliteitsniveaus behouden en de compatibiliteit met de bijbehorende versie van Microsoft SQL Server te garanderen voor alle klanten!

Daarom niets wordt voorkomen dat klanten die geen bestaande databases op het niveau van de recentste compatibiliteit van profiteren van de nieuwe query optimizer en processorfuncties query wijzigen. Als een herinnering van de geschiedenis, de uitlijning van de SQL standaard compatibiliteitsniveaus versies zijn:

- 100: in SQL Server 2008 en Azure SQL Database V11:.
- 110: in SQL Server 2012 en Azure SQL Database V11:.
- 120: in SQL Server 2014 en Azure SQL Database V12.
- 130: in SQL Server 2016 en Azure SQL Database V12.


> [AZURE.IMPORTANT] Vanaf **medio juni 2016**in Azure SQL-Database, worden het compatibiliteitsniveau standaard 130 niet 120 voor **nieuwe** databases.
> 
> Databases die zijn gemaakt vóór medio juni 2016 zal *niet* worden beïnvloed en hun huidige niveau van compatibiliteit (100, 110 of 120) wordt onderhouden. Databases die migreren van Azure SQL-Database versie dat V11: naar V12 hebben hun niveau van compatibiliteit niet gewijzigd.


In dit artikel besproken voor de voordelen van het compatibiliteitsniveau van 130 en hoe u kunt profiteren van deze voordelen. We pakken de mogelijke neveneffecten op de prestaties van query's voor de bestaande SQL-toepassingen.


## <a name="about-compatibility-level-130"></a>Over het niveau van compatibiliteit 130


Eerst, als u weten van het huidige compatibiliteitsniveau van uw database wilt, Transact-SQL-instructie uitgevoerd.


```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Voordat deze wijziging tot 130 voor databases die **zojuist** gemaakt gebeurt, gaan we bekijken welke deze wijziging heeft tot en met enkele eenvoudige query voorbeelden en Zie hoe iedereen kan profiteren van deze.

Verwerken van query's in relationele databases kan zeer complex zijn en kan leiden tot veel computerwetenschappen en wiskunde te begrijpen van de inherente ontwerpkeuzen en het gedrag. In dit document, is de inhoud opzettelijk vereenvoudigd zodat iedereen met een minimale technische achtergrond kan wat de gevolgen van de wijzigingen in de compatibiliteit en bepalen hoe deze toepassingen kan profiteren.

We hebben een beknopte beschrijving van het compatibiliteitsniveau 130 aan de tabel brengt.  U kunt meer informatie vinden op [ALTER databasecompatibiliteitsniveau (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), maar hier volgt een kort overzicht:

- De Insert-bewerking van een Insert select-instructie kan met meerdere threads of kan een parallelle plannen terwijl voordat deze bewerking één thread is.
- Geheugen optimaliseren tabel en de tabel variabelen query's kunnen nu hebben parallelle plannen terwijl voordat deze bewerking is ook één thread.
- Statistieken voor tabel geoptimaliseerd geheugen kan nu worden bemonsterd en worden automatisch bijgewerkt. Zie [Wat is er nieuw in de Database-Engine: In Memory OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) voor meer informatie.
- Batch-modus-v/s rij modus gewijzigd met kolom winkel indexen
  - Hiermee sorteert u op een tabel met een kolom archief index zijn nu in de batchmodus.
  - Aggregaten Windowing werken nu in de batchmodus zoals TSQL vertraging/LEAD overzichten.
  - Query's op kolom winkel tabellen met meerdere afzonderlijke componenten worden uitgevoerd in de batchmodus.
  - Query's die worden uitgevoerd onder de DOP = 1 of met een seriële plan ook uit te voeren in de batchmodus.
- Laatste kardinaliteit schatting verbeteringen daadwerkelijk afkomstig zijn met het compatibiliteitsniveau van 120, maar voor mensen met een lager niveau compatibiliteit (d.w.z. 100 of 110), het verplaatsen naar compatibiliteit niveau 130 ook brengt deze verbeteringen en deze ook de prestaties van uw toepassingen kunnen profiteren.


## <a name="practicing-compatibility-level-130"></a>Het compatibiliteitsniveau van 130 oefenen


Eerst gaan we krijgen sommige tabellen, indexen en willekeurige gegevens gemaakt om te oefenen met enkele van deze nieuwe mogelijkheden. De scriptvoorbeelden TSQL kunnen worden uitgevoerd onder SQL Server 2016 of onder Azure SQL-Database. Echter bij het maken van een Azure SQL-database, moet u ten minste een database P2 omdat u moet ten minste een aantal cores multithreading en dus profiteren van deze functies.


```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


Nu hebben we een kijkje naar bepaalde functies van het verwerken van Query afkomstig zijn met het compatibiliteitsniveau van 130.


## <a name="parallel-insert"></a>Parallelle invoegen


De onderstaande TSQL-instructies uitvoeren, voert de invoegbewerking onder het niveau van compatibiliteit 120 en 130, die respectievelijk de INSERT-bewerking wordt uitgevoerd in één thread model (120) en in een multithread-model (130).


```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Door de werkelijke het queryplan kijken naar de grafische weergave of de XML-inhoud, kunt u bepalen welke kardinaliteit schatting functie voor games is. De plannen naast elkaar weergegeven in figuur 1, kunnen we duidelijk zien dat de kolom winkel invoegen uitvoering van seriële in 120 in parallel in 130 gaat. Let er ook op dat de wijziging van het iterator-pictogram in het 130-indeling met twee parallelle pijlen ter illustratie van het feit dat nu de iterator uitvoering immers parallel is. Als er grote INSERT-bewerkingen te voltooien, wordt de parallelle uitvoering, gekoppeld aan het aantal core u tot uw beschikking staan voor de database hebt, sneller; tot een 100 keer sneller zijn, afhankelijk van uw situatie!


*Figuur 1: Invoegbewerking wordt gewijzigd van seriële naar parallelle en compatibiliteit niveau 130.*


![Figuur 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)


## <a name="serial-batch-mode"></a>SERIËLE Batch-modus


Op dezelfde manier kan verplaatsen naar het niveau van compatibiliteit 130 bij de verwerking van rijen met gegevens modus batch-verwerking. Bewerkingen van batch-modus zijn eerst alleen beschikbaar wanneer er een winkel kolomindex in plaats. Ten tweede een batch vertegenwoordigt meestal ~ 900 rijen, en gebruikt de logica van een code geoptimaliseerd voor multicore CPU, geheugen sneller worden verwerkt en direct maakt gebruik van de gecomprimeerde gegevens van het archief van de kolom waar mogelijk. Onder deze omstandigheden 2016 van SQL Server kan worden verwerkt ~ 900 rijen in één keer in plaats van 1 rij op het moment, en bijgevolg de algemene overheadkosten van de bewerking nu wordt gedeeld door de gehele partij, waardoor de totale kosten per rij. Deze gedeelde hoeveelheid bewerkingen die in principe in combinatie met de kolom winkel compressie vermindert de vertraging die betrokken zijn bij een bewerking selecteert batch-modus. U kunt meer informatie vinden over de kolom winkel en batch-modus met [Columnstore indexen Guide](https://msdn.microsoft.com/library/gg492088.aspx).


```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

– The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Als hieronder zichtbaar is, met inachtneming van de query plannen naast elkaar op figuur 2, kunnen wij is Zie dat de verwerkingsmodus is gewijzigd met het niveau van compatibiliteit, en derhalve bij het uitvoeren van de query's in zowel het compatibiliteitsniveau helemaal niet, we dat het merendeel van de verwerkingstijd zien kunnen besteed in de rij-modus (86%) ten opzichte van de batchmodus (14%), waarbij 2 batches zijn verwerkt. De dataset te verhogen, de vergoeding zal toenemen.


*Afbeelding 2: Selecteer bewerking wijzigingen in seriële naar de batchmodus met het compatibiliteitsniveau van 130.*


![Figuur 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)


## <a name="batch-mode-on-sort-execution"></a>Batchmodus op sorteren kan worden uitgevoerd


Net als hierboven, maar toegepast op een sorteerbewerking de overgang van rij-modus (niveau van compatibiliteit 120) naar de batch-modus (niveau van compatibiliteit 130) verbetert de prestaties van de sorteerbewerking om dezelfde redenen.


```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Zichtbare side-by-side in figuur 3, kunnen we zien dat de sort-bewerking in de modus van de rij staat voor 81% van de kosten, terwijl de batchmodus slechts 19% van de kosten (respectievelijk 81% en 56% van de sortering zelf) vertegenwoordigt.


*Afbeelding 3: Sorteerbewerking verandert rij in batchmodus met het compatibiliteitsniveau van 130.*


![Figuur 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)


Natuurlijk, deze voorbeelden bevat alleen tienduizenden rijen, is er niets bij het onderzoeken van de gegevens die beschikbaar zijn in de meeste SQL-Servers deze dagen. Deze tegen miljoenen rijen in plaats daarvan alleen project en dit kan vertalen in enkele minuten van de uitvoering van elke dag in afwachting van de aard van uw werkbelasting gespaard.


## <a name="cardinality-estimation-ce-improvements"></a>Verbeteringen in de kardinaliteit raming (CE)


Geïntroduceerd in SQL Server 2014, een database die wordt uitgevoerd op een niveau van compatibiliteit 120 of boven zal gebruik maken van de nieuwe raming van de kardinaliteit van functionaliteit. De kardinaliteit van schatting is in wezen de logica die wordt gebruikt om te bepalen hoe een query op basis van de geschatte kosten door SQL server wordt uitgevoerd. De raming wordt berekend op basis van invoer van de statistieken die zijn gekoppeld aan objecten die betrokken zijn in die query. Praktisch, op hoog niveau, schatting kardinaliteit functies zijn ramingen van aantal rij met gegevens over de verdeling van de waarden van verschillende waarde telt, en de dubbele tellingen uit de tabellen en objecten waarnaar wordt verwezen in de query. Aan deze schattingen onjuist, kan leiden tot onnodige schijf I/O vanwege onvoldoende geheugen subsidies (dat wil zeggen TempDB morsen van vloeistoffen) of op een selectie van de uitvoering van een plan voor seriële via een parallelle planning wordt uitgevoerd, om een paar te noemen. Sluiting, verkeerde schattingen kunnen leiden tot een achteruitgang van de algemene prestaties van de uitvoering van de query. Aan de andere kant, betere schattingen, nauwkeuriger ramingen, leidt tot betere query uitvoeringen!

Zoals eerder query-optimalisatie en schattingen een ingewikkelde aangelegenheid zijn, maar als u meer informatie over query's en estimator kardinaliteit wilt, kunt u verwijzen naar het document in [Uw Query-plannen met de SQL Server 2014 kardinaliteit Estimator optimaliseren](https://msdn.microsoft.com/library/dn673537.aspx) voor een diepere kennismaking.


## <a name="which-cardinality-estimation-do-you-currently-use"></a>Welke kardinaliteit schatting momenteel gebruik je?


Om te bepalen onder welke kardinaliteit schatting uw query's worden uitgevoerd, zullen we gewoon gebruiken met de onderstaande voorbeelden query. Houd er rekening mee dat het eerste voorbeeld wordt uitgevoerd onder het compatibiliteitsniveau van 110, wat impliceert het gebruik van de functies van de oude kardinaliteit schatting.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Zodra de uitvoering is voltooid, klikt u op de XML-link en bekijk de eigenschappen van de eerste iterator zoals hieronder wordt weergegeven. Noteer de naam van de eigenschap naam CardinalityEstimationModelVersion die is ingesteld op 70. Het betekent niet dat het compatibiliteitsniveau van de database is ingesteld op de SQL Server 7.0-versie (deze is ingesteld op 110 als zichtbaar in de hierboven genoemde TSQL-instructies), maar de waarde 70 gewoon de oude kardinaliteit schatting functionaliteit beschikbaar sinds SQL Server 7.0 die geen belangrijke wijzigingen tot SQL Server 2014 staat hadden (die wordt geleverd met een compatibiliteitsniveau van 120).


*Figuur 4: De CardinalityEstimationModelVersion is ingesteld op 70 bij gebruik van een compatibiliteitsniveau van 110 of onder.*


![Figuur 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)


U kunt ook het compatibiliteitsniveau 130 wijzigen en het gebruik van de nieuwe raming kardinaliteit functie uitschakelen met behulp van de LEGACY_CARDINALITY_ESTIMATION ingesteld op ON met [ALTER DATABASE binnen het bereik van configuratie](https://msdn.microsoft.com/library/mt629158.aspx). Dit is precies hetzelfde als tijdens het gebruik van de laatste query verwerking compatibiliteitsniveau 110 van een raming van de kardinaliteit-functie. Als u dit doet, kunt u profiteren van de nieuwe query processing functies in aantocht met de meest recente compatibiliteitsniveau (dat wil zeggen de batchmodus), maar nog steeds afhankelijk van de functionaliteit van de oude kardinaliteit schatting indien nodig.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Verplaatsen naar het compatibiliteitsniveau van 120 of 130, kunt de nieuwe functionaliteit van de raming van de kardinaliteit. In dat geval de standaard CardinalityEstimationModelVersion wordt ingesteld overeenkomstig 120 of 130 als hieronder weergegeven.


```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


*Figuur 5: De CardinalityEstimationModelVersion ingesteld op 130 wanneer u het compatibiliteitsniveau van 130.*


![Figuur 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)


## <a name="witnessing-the-cardinality-estimation-differences"></a>Den met de schatting van de kardinaliteit van verschillen


Nu voeren we iets meer complexe query waarbij een INNER JOIN met een WHERE-component met sommige predikaten en we kijken naar de rij count raming van de oude kardinaliteit schatting functie eerst.


```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


200.704 rijen, efficiënt uitvoeren van deze query worden geretourneerd tijdens de raming van de rij met de functionaliteit van de oude kardinaliteit schatting 194,284 rijen beweert. Uiteraard zoals gezegd vóór, deze rij aantal resultaten is ook afhankelijk van hoe vaak u de vorige voorbeelden hebt die de voorbeeldtabellen steeds opnieuw op elke uitvoering vult. Uiteraard de predikaten in uw query zal ook van invloed zijn op de werkelijke schatting wordt alleen de tabelvorm, inhoud, gegevens en hoe deze gegevens daadwerkelijk met elkaar overeenstemmen.


*Figuur 6: De raming van de telling rij is 194,284 of 6.000 rijen uit vanaf de 200.704 rijen verwacht.*


![Figuur 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)


Op dezelfde manier, laten we nu voeren dezelfde query met de nieuwe functionaliteit van de raming van de kardinaliteit.


```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Kijken naar de, nu zien we dat de raming van de rij 202,877, of veel dichter en hoger is dan de oude kardinaliteit schatting is.

*Figuur 7: Het aantal rij schatting is nu 202,877 in plaats van 194,284.*


![Figuur 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)


In werkelijkheid de resultaatset is 200.704 rijen (maar alles hangt af van hoe vaak u de query's van de vorige voorbeelden hebt uitgevoerd, maar nog belangrijker is, omdat de TSQL de instructie ASELECT() gebruikt, de werkelijke waarden die kunnen variëren van één uitvoeren naar de volgende). In dit voorbeeld wordt biedt de nieuwe raming van de kardinaliteit daarom een betere taak bij het schatten van het aantal rijen omdat veel dichter bij 200,704, dan 194,284 202,877! Laatste als u de WHERE-component op gelijkheid predikaten (plaats ' > ' bijvoorbeeld), hierdoor kan u de ramingen tussen de oude en nieuwe kardinaliteit functie nog meer afhankelijk van het aantal komt overeen met u kunt krijgen.

Uiteraard in dit geval geeft worden rijen ~ 6000 af van het werkelijke aantal geen grote hoeveelheden gegevens in bepaalde situaties. Transponeren dit miljoenen rijen in verschillende tabellen en complexe query's, en soms de raming is uitgeschakeld door miljoenen rijen en daarom het risico het verkeerde uitvoeringsplan pick-up of er is onvoldoende geheugen subsidies leiden tot TempDB morsen van vloeistoffen, en dus meer I/O, vraagt zijn nu veel hoger.

Als u de verkoopkans, oefenen deze vergelijking met de meest gangbare query's en datasets, en Zie voor uzelf door hoeveel sommige van de oude en de nieuwe schattingen worden beïnvloed, terwijl sommige alleen meer af van de realiteit of sommige anderen gewoon simpelweg dichter bij de feitelijke rij worden kan daadwerkelijk in het resultaat wordt geretourneerd telt. Alles hangt af van de vorm van uw query's, de kenmerken van Azure SQL-database, de aard en de grootte van uw datasets en de statistische gegevens beschikbaar over deze. Als u uw exemplaar van Azure SQL-Database hebt gemaakt, moet de query optimizer bouwen de knowledge helemaal opnieuw gebruikt statistieken die uit de vorige query is uitgevoerd. De ramingen zijn dus zeer contextuele en bijna specifiek voor elke situatie en de toepassing. Het is een belangrijk aspect rekening mee moet houden!


## <a name="some-considerations-to-take-into-account"></a>Enkele punten waarmee u rekening te houden


Hoewel de meeste werklasten zouden kunnen profiteren van het compatibiliteitsniveau 130, voordat u tot vaststelling van het compatibiliteitsniveau van voor uw productieomgeving hebt u in principe 3 mogelijkheden:

1. U het compatibiliteitsniveau van 130 naar, en Zie hoe dingen uitvoeren. Als u merkt dat sommige behalen u gewoon simpelweg de verenigbaarheid niveau weer instellen op het oorspronkelijke niveau of 130 behouden en alleen omgekeerde kardinaliteit schatting terug naar de oudere modus (zoals hierboven vermeld, dit alleen kan besteden aan de kwestie).
2. U grondig testen van uw bestaande toepassingen bij soortgelijke productie belasting, afstellen en de prestaties voordat u productie valideren. In geval van problemen, hetzelfde als hierboven, u kunt altijd teruggaan naar het oorspronkelijke compatibiliteitsniveau, of gewoon de kardinaliteit raming omkeren naar de oudere modus.
3. Als laatste optie en de meest recente manier om deze vragen is om te profiteren van het archief van de Query. Dat is de aanbevolen optie vandaag! Bij de analyse van uw query's onder compatibiliteit 120 niveau of onder versus 130, kan niet raden we u genoeg aan het archief van de Query gebruiken. Archief van de query is beschikbaar met de meest recente versie van Azure SQL Database V12 en ontworpen om u te helpen met het oplossen van query prestaties. De Query winkel beschouwen als een flight data recorder voor uw database verzamelen en presenteren van gedetailleerde historische informatie over alle query's. Dit vereenvoudigt forensics prestaties door het verminderen van de tijd voor het opsporen en oplossen van problemen. U vindt meer informatie op [Query winkel: een flight data recorder voor de database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).


AT het op hoog niveau, als u al een set van databases die worden uitgevoerd op het niveau van compatibiliteit 120 of onder, en van plan bent om ze naar 130, of omdat uw werkbelasting inrichten automatisch nieuwe databases die worden binnenkort worden standaard ingesteld op 130, houd rekening met de volgende elementen:

- Inschakelen voordat u deze wijzigt op het niveau van de nieuwe compatibiliteit in productie, Query opslaan. U kunt verwijzen naar [de compatibiliteitsmodus Database en de Query-winkel gebruiken](https://msdn.microsoft.com/library/bb895281.aspx) voor meer informatie.
- Test vervolgens alle kritieke werklast met representatieve gegevens en query's van een productie-achtige omgeving, en vergelijk de prestaties ervaren en zoals aangegeven door de Query opslaan. Als u sommige behalen, kunt u identificeren van de regressed's met het archief van de Query en het plan dat optie uit het archief van de Query gebruiken (aka plannen vastmaken). In een dergelijk geval u definitief blijven bij het compatibiliteitsniveau 130, en het voormalige queryplan als voorgesteld door de Query-winkel.
- Als u profiteren van nieuwe functies en mogelijkheden van Azure SQL-Database wilt (die werkt met SQL Server 2016), maar gevoelig voor wijzigingen gebracht door het compatibiliteitsniveau 130, als laatste redmiddel zijn kunt u ook gedwongen het compatibiliteitsniveau terug naar het niveau dat aansluit bij uw werkbelasting met behulp van een instructie ALTER DATABASE. Maar eerst, houd er rekening mee dat de Query winkel plan optie vast uw beste optie is omdat in principe niet met 130 op het functionaliteitsniveau van een oudere versie van SQL Server verblijft.
- Als er meerdere databases spanning multitenant-toepassingen, kan het nodig zijn voor het bijwerken van de provisioning logica van uw databases om ervoor te zorgen een consistente compatibiliteitsniveau in alle databases. oud en nieuw ingerichte die. Prestaties van uw werkbelasting kan gevoelig zijn voor het feit dat sommige databases op op verschillende compatibiliteitsniveaus worden uitgevoerd en compatibiliteit niveau consistentie in elke database kan dus nodig zijn om te kunnen bieden dezelfde mogelijkheden voor uw klanten alle over de hele linie. Houd er rekening mee dat het is niet verplicht, deze echt is afhankelijk van hoe uw toepassing wordt beïnvloed door het niveau van compatibiliteit.
- Laatste, met betrekking tot de kardinaliteit schatting en net als het wijzigen van het compatibiliteitsniveau voordat u verder gaat in productie, verdient het testen van uw werkbelasting productie onder de nieuwe voorwaarden om te bepalen als uw toepassing van de raming van de kardinaliteit van verbeteringen gebruikmaakt.


## <a name="conclusion"></a>Conclusie


Azure SQL-Database gebruiken om te profiteren van alle verbeteringen in SQL Server 2016 kunt u uw query uitvoeringen duidelijk verbeteren. Net als-is! Natuurlijk als een nieuwe functie, moet een juiste evaluatie doen om te bepalen van de precieze voorwaarden waaronder de werkbelasting van de database het beste werkt. De ervaring leert dat de meeste werkbelasting verwachting voor ten minste transparant compatibiliteitsniveau 130, tijdens het gebruik van de nieuwe query verwerking van functies en nieuwe kardinaliteit schatting. Dat gezegd, realistisch, er zijn altijd enkele uitzonderingen en het uitvoeren van de juiste vervaldatum toewijding inzetten voor een belangrijke beoordeling om te bepalen hoeveel u kunt profiteren van deze verbeteringen. En opnieuw, het archief van de Query kan worden van een deskundige hulp in dit werk!

U kunt een compatibiliteitsniveau 140 zoals SQL Azure ontwikkeld, in de toekomst verwachten. Als de tijd van toepassing is, begint we praten over wat deze toekomstige compatibiliteitsniveau 140 brengt, net zoals we kort hier besproken welke compatibiliteitsniveau 130 brengt vandaag.

Nu, laten we niet vergeten, begin juni 2016, Azure SQL-Database wordt gewijzigd het standaardniveau voor compatibiliteit van 120 naar 130 voor nieuwe databases. Let op!


## <a name="references"></a>Verwijzingen


- [Wat is nieuw in de Database-Engine](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)

- [Blog: Query winkel: een flight data recorder voor uw database, door Borko Novakovic, 8 juni-2016](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)

- [ALTER databasecompatibiliteitsniveau (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)

- [ALTER DATABASE BINNEN HET BEREIK VAN CONFIGURATIE](https://msdn.microsoft.com/library/mt629158.aspx)

- [Het niveau van compatibiliteit 130 voor Azure SQL Database V12](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)

- [Plan uw zoekopdracht optimaliseren met de SQL Server 2014 kardinaliteit Estimator](https://msdn.microsoft.com/library/dn673537.aspx)

- [Indexen-handleiding Columnstore](https://msdn.microsoft.com/library/gg492088.aspx)

- [Blog: Verbeterde prestaties van query's met het niveau van compatibiliteit 130 in Azure SQL-Database door aan Alain Lissoir, mei 6 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)



<!--
Improved Query Performance with Compatibility Level 130 in Azure SQL Database

May 6, 2016 by Alain Lissoir (AlainL), on GitHub 'alainlissoir'.

https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/

..... Now, above.
....................
..... Soon, below?

CAPS / MSDN ideally, but instead on ACom:
.. # Assess effects of latest compatibility level on query performance, how to

sql-database-compatibility-level-query-performance-130.md

genemi = MightyPen , 2016-05-20  Friday  17:00pm
-->

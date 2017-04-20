<properties
   pageTitle="Distributie van tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Aan de slag met de distributie van tabellen in Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="distributing-tables-in-sql-data-warehouse"></a>Distributie van tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Overzicht][]
- [Gegevenstypen][]
- [Distribueren][]
- [Index][]
- [Partitie][]
- [Statistieken][]
- [Tijdelijke][]

SQL Data Warehouse is dat een massively parallel processing (MPP) gedistribueerd databasesysteem.  Gegevens delen en verwerken van mogelijkheden op verschillende knooppunten, biedt SQL Data Warehouse enorme schaalbaarheid - tot ver voorbij een enkel systeem.  Bepalen hoe de gegevens in uw magazijn SQL-gegevens distribueren is een van de belangrijkste factoren om optimale prestaties te bereiken.   De sleutel tot optimale prestaties is de verplaatsing van gegevens minimaliseren en op zijn beurt de sleutel tot het minimaliseren van de verplaatsing van gegevens te selecteren de juiste distributiestrategie.

## <a name="understanding-data-movement"></a>Wat verplaatsing van gegevens?

In een MPP-systeem de gegevens uit elke tabel gedeeld over verschillende onderliggende databases.  Het meest geoptimaliseerde query's op een systeem met MPP kunnen alleen worden doorgegeven via uit te voeren op de afzonderlijke gedistribueerde databases zonder interactie tussen de andere databases.  Stel dat u een database met verkoopgegevens bevat twee tabellen, verkopen en klanten hebben.  Als u een query die moet deelnemen aan de tabel sales aan de klantentabel hebt en u verdeelt uw verkoop- en de klantentabellen omhoog door het nummer van de klant, elke klant te zetten in een afzonderlijke database, kunnen query's die deelnemen aan de verkoop- en in elke database met geen kennis van de andere databases worden opgelost.  Daarentegen, als u uw verkoopgegevens gedeeld door het bestelnummer en uw klantgegevens op klantnummer, vervolgens een bepaalde database geen overeenkomstige gegevens voor elke klant en dus als u deelnemen aan de verkoopgegevens voor uw klantgegevens wilt, moet u de gegevens worden opgehaald voor elke klant in de andere databases.  In dit tweede voorbeeld moet verplaatsing van gegevens plaatsvinden om de gegevens van de klant verplaatst naar de verkoopgegevens zodat de twee tabellen kunnen worden gekoppeld.  

Verplaatsing van gegevens is niet altijd slecht, het is soms nodig voor het oplossen van een query.  Maar wanneer dit een extra stap kan worden vermeden, natuurlijk de query sneller uitgevoerd.  Verplaatsing van gegevens ontstaat meestal wanneer tabellen worden gekoppeld of samenvoegingen worden uitgevoerd.  Vaak moet u beide doen, dus terwijl u mogelijk optimaliseren voor een scenario, zoals een join, u nog steeds verplaatsen van gegevens moet om u op te lossen voor het scenario, zoals een samenvoeging.  De truc is uitzoeken welke minder werk is.  In de meeste gevallen is distributie van grote feitentabellen op een algemeen gekoppelde kolom de meest effectieve methode voor het verminderen van de meeste verplaatsing van gegevens.  Distribueren van gegevens op de join-kolommen is een veel meer veelgebruikte methode voor het verminderen van de verplaatsing van gegevens dan het distribueren van gegevens in kolommen die zijn betrokken bij een samenvoeging.

## <a name="select-distribution-method"></a>Distributiemethode selecteren

Uw gegevens worden in SQL Data Warehouse achter de schermen wordt opgedeeld in 60-databases.  Elke afzonderlijke database een **verdeling**genoemd.  Wanneer de gegevens in elke tabel wordt geladen, heeft SQL Data Warehouse te weten hoe uw gegevens delen via deze 60 verdelingen.  

De verdeling wordt gedefinieerd op het tabelniveau van de en er zijn momenteel twee mogelijkheden:

1. **Round-robin** die gegevens distribueren gelijkmatig maar willekeurig.
2. **Distributed Hash** die wordt verdeeld op basis van de hash-waarden in één kolom

Standaard wanneer u geen een methode voor distributie definieert, wordt de tabel gedistribueerd met behulp van de methode van **round-robin** distributie.  Echter, naarmate u meer geavanceerde in uw implementatie, zult u **gedistribueerde hash** tabellen met verplaatsing van gegevens die op zijn beurt prestaties van query's optimaliseert te minimaliseren.

### <a name="round-robin-tables"></a>Round Robin tabellen

Met de methode Round Robin van het distribueren van gegevens is zeer veel hoe het klinkt.  Als uw gegevens zijn geladen, wordt elke rij gewoon verzonden naar de volgende verdeling.  Deze methode voor het distribueren van de gegevens wordt willekeurig altijd de gegevens zeer gelijkmatig verdelen over alle van de verdelingen.  Er is geen gedaan tijdens de round robin die de gegevens plaatst sorteren.  Een round-robin distributie is daarom wel een willekeurige hash.  Met een round robin gedistribueerde tabel is niet nodig om te begrijpen van de gegevens.  Round-Robin met tabellen kunnen daarom vaak laden voor goede doelen.

Standaard als geen distributiemethode wordt gekozen, de methode van round-robin distributie gebruikt.  Round-robin tabellen zijn eenvoudig te gebruiken omdat de gegevens worden willekeurig verdeeld in het gehele systeem betekent dit dat het systeem welke verdeling kan niet garanderen is elke rij echter op.  Het systeem moet hierdoor soms aan te roepen van een verplaatsing van gegevens om uw gegevens beter te organiseren voordat deze een query kunt oplossen.  Deze extra stap kan uw query's vertragen.

Overweeg het gebruik van Round-Robin distributie voor de tabel in de volgende scenario's:

- Wanneer u aan de slag als eenvoudige uitgangspunt
- Als er geen duidelijke gekoppelde sleutel
- Als er geen goede kandidaat kolom voor de hash in de tabel verdelen
- Als de tabel een gemeenschappelijk join-sleutel niet met andere tabellen deelt
- Als de join minder belangrijk dan andere joins in de query is
- Wanneer de tabel wordt een tijdelijke staging-tabel

Beide voorbeelden maakt een Round Robin-tabel:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [AZURE.NOTE] Wanneer round robin het standaardtype tabel wordt expliciet in de DDL wel aanbevolen is dat de bedoelingen van de indeling van uw tabel wissen aan anderen.

### <a name="hash-distributed-tables"></a>Hash-tabellen gedistribueerde

Met behulp van een **distributed Hash** -algoritme voor het distribueren van uw tabellen, kan de prestaties voor veel scenario's verbeteren door verplaatsing van gegevens bij query.  Distributed hash-tabellen zijn tabellen die worden gedeeld tussen de gedistribueerde databases met behulp van een hash-algoritme op een enkele kolom die u selecteert.  De kolom distributie is wat bepaalt hoe de gegevens wordt verdeeld over de gedistribueerde databases.  De hash-functie gebruikt de distributie kolom rijen toewijzen aan distributies.  De hash-algoritme en de resulterende verdeling is deterministisch.  Dat is dezelfde waarde met hetzelfde gegevenstype heeft altijd voor de dezelfde verdeling.    

In dit voorbeeld wordt een tabel die verdeeld over de id maken:

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a>Selecteer de kolom distributie

Wanneer u voor het **distribueren van hash** een tabel kiest, moet u een distributie met één kolom te selecteren.  Bij het selecteren van een distributie-kolom, zijn er drie belangrijke factoren te overwegen.  

Selecteer één kolom wordt:

1. Niet worden bijgewerkt
2. Verdelen gegevens, gegevens schuintrekken te vermijden
3. Verplaatsing van gegevens beperken

### <a name="select-distribution-column-which-will-not-be-updated"></a>Selecteer kolom verdeling worden niet bijgewerkt

Distributie-kolommen kunnen daarom niet worden bijgewerkt, selecteert u een kolom met statische waarden.  Als een kolom worden bijgewerkt moet, is het meestal niet een goede verdeling kandidaat.  Als er een geval waarin u een kolom distributie moet bijwerken, kunt u dit doen door de rij eerst verwijderen en vervolgens een nieuwe rij invoegen.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Selecteer verdeling kolom waarin gegevens worden gelijkmatig verdelen

Aangezien een gedistribueerd systeem alleen net zo snel als de traagste verdeling wordt uitgevoerd, is het belangrijk dat het verdeelt u het werk gelijkmatig over de verdelingen om te komen tot evenwichtige worden uitgevoerd in het gehele systeem.  De manier waarop die het werk is verdeeld in een gedistribueerd systeem is gebaseerd op waar de gegevens voor elke distributie woont.  Hierdoor is het belangrijk dat u selecteert de kolom rechts verdeling voor de gegevens verdelen zodat elke distributie werk gelijk is en is de dezelfde tijd om haar deel van het werk te voltooien.  Wanneer het werk is goed verdeeld over het systeem, worden de gegevens wordt verdeeld over de verdelingen.  Wanneer de gegevens niet gelijkmatig is verdeeld, noemen we deze **gegevens laten hellen**.  

Om gegevens gelijkmatig te verdelen en te voorkomen dat gegevens scheeftrekken, overweeg het volgende bij het selecteren van uw distributie kolom:

1. Selecteer een kolom met een groot aantal verschillende waarden.
2. Vermijd het distribueren van gegevens in kolommen met een paar afzonderlijke waarden. 
3. Vermijd het distribueren van gegevens in kolommen met een hoge frequentie van null-waarden.
4. Voorkomen dat gegevens op datumkolommen verdelen.

Omdat elke waarde is 1 van 60 distributies hashing, gelijke verdeling te bereiken zult u een kolom die is zeer uniek en bevat meer dan 60 unieke waarden selecteren.  Ter illustratie, kunt u overwegen een geval waarin een kolom alleen 40 unieke waarden bevat.  Als deze kolom is geselecteerd als de sleutel voor de distributie, zou de gegevens voor die tabel neerzetten op 40 verdelingen ten hoogste 20 verdelingen met geen gegevens en geen verwerking te doen verlaten.  De 40 verdelingen moet echter meer werk te doen als de gegevens is meer dan 60 verdelingen gelijkmatig verdeeld.  Dit scenario is een voorbeeld van gegevens scheeftrekken.

In de MPP-systeem wacht elke stap van de query alle verdelingen aan hun aandeel in het werk te voltooien.  Als een distributie doet meer werk dan de andere, vervolgens de bron van de andere distributies zijn in feite verspild gewoon wachten op de drukke verdeling.  Wanneer het werk is niet gelijkmatig verspreid over alle verdelingen, noemen we deze **verwerking scheeftrekken**.  Verwerking scheeftrekken, worden query's trager dan als de werkbelasting gelijkmatig kan worden verspreid via de verdelingen.  Schuintrekken gegevens zal leiden tot de verwerking scheeftrekken.

Vermijd zeer kolom distribueren als u de null-waarden zullen terechtkomen op de dezelfde verdeling. Distribueren op een datumkolom kan ook veroorzaken verwerking scheeftrekken omdat alle gegevens voor een bepaalde datum wordt op de uitkering van hetzelfde land. Als verschillende gebruikers query's uitvoert alle filteren op dezelfde datum, en slechts 1 van de 60 verdelingen doet al het werk sinds een bepaalde datum alleen worden op een uitkering. In dit scenario wordt de query's waarschijnlijk 60 keer langzamer dan als de gegevens gelijkmatig zijn verdeeld over alle van de verdelingen uitgevoerd. 

Als er geen goede kandidaat kolommen bestaan, overwegen met behulp van round-robin distributie methode.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Selecteer verdeling kolom verplaatsen van gegevens te minimaliseren

Verplaatsing van gegevens beperken door de kolom rechts verdeling is een van de belangrijkste strategieën voor het optimaliseren van de prestaties van de SQL.  Verplaatsing van gegevens ontstaat meestal wanneer tabellen worden gekoppeld of samenvoegingen worden uitgevoerd.  Kolommen die worden gebruikt `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` en `HAVING` componenten alle zorg voor een **goede** hash-distributie kandidaten. 

Anderzijds, kolommen in de `WHERE` component kan **niet** voor een goede hash-kolom kandidaten omdat zij beperken welke verdelingen deelnemen aan de query, waardoor verwerking scheeftrekken.  Een goed voorbeeld van een kolom die mogelijk op verleidelijk, maar vaak kan ertoe leiden dat deze verwerking schuintrekken is een datumkolom.

In het algemeen hebt u twee grote feitentabellen vaak betrokken in een join, krijgt u de meeste prestaties door beide tabellen op een van de join-kolommen te verdelen.  Als u een tabel die nooit is gekoppeld aan een andere grote feitentabel hebben, zoek naar kolommen die zijn vaak in de `GROUP BY` component.

Er zijn een paar belangrijke criteria die moeten worden genomen om te voorkomen dat de verplaatsing van gegevens tijdens een join:

1. De tabellen die betrokken zijn in de join moeten worden gedistribueerd op **een** van de kolommen die deel uitmaken van de join hash.
2. De gegevenstypen van de join-kolommen moeten overeenkomen tussen beide tabellen.
3. De kolommen moeten worden gekoppeld met een operator is gelijk aan.
4. Het jointype is niet mogelijk een `CROSS JOIN`.


## <a name="troubleshooting-data-skew"></a>Het scheeftrekken van gegevens oplossen

Wanneer gegevens in een tabel wordt gedistribueerd met behulp van de distributiemethode hash is er een kans dat sommige distributies wordt vervormd als u niet goed meer gegevens dan andere. Veel gegevens scheeftrekking kan effect hebben op prestaties van query's omdat het eindresultaat van een gedistribueerde query tot de langstlopende verdeling wachten moet te voltooien. Afhankelijk van de mate van het hellen gegevens moet u mogelijk aanpakken.

### <a name="identifying-skew"></a>Identificeert schuintrekken

Een eenvoudige manier naar een tabel die scheef te gebruiken is `DBCC PDW_SHOWSPACEUSED`.  Dit is een zeer snelle en eenvoudige manier om te zien het aantal rijen dat in elk van de 60 verdelingen van de database zijn opgeslagen.  Houd er rekening mee dat voor de meest evenwichtige prestaties, de rijen in een gedistribueerde moeten gelijkmatig gespreid over alle verdelingen.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Als u een query uitvoert op de Azure SQL Data Warehouse management dynamische weergaven (DMV) kunt u een meer gedetailleerde analyse uitvoeren.  Maak eerst de weergave [dbo.vTableSizes][] weergave met behulp van de SQL-code uit de [Tabel overzicht][Overzicht] artikel.  Als de weergave is gemaakt, deze query uitvoeren om te bepalen welke tabellen u hebt meer dan 10% gegevens scheeftrekken.

```sql
select *
from dbo.vTableSizes 
where two_part_name in 
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a>Het omzetten van gegevens schuintrekken

Niet alle schuintrekken is genoeg om een correctie te rechtvaardigen.  In sommige gevallen de prestaties van een tabel in een enkele query's zwaarder wegen dan de schade van gegevens scheeftrekken.  Als u gegevens moet oplossen om schuintrekken in een tabel, moet u begrijpen zoveel mogelijk over de gegevensvolumes en query's in uw werkbelasting.   Volg de stappen in het artikel [Query controleren][] om te controleren van de impact van het scheeftrekken op prestaties van query's en met name de gevolgen voor hoe lang query's uitvoeren op de afzonderlijke verdelingen nemen is één manier kijken naar de gevolgen van het scheeftrekken.

Distribueren van gegevens is een kwestie van het vinden van de juiste verhouding tussen de scheefheid van de gegevens minimaliseren en verplaatsing van gegevens tot een minimum beperken. Deze doelstellingen kunnen tegengestelde en soms wilt u uw gegevens schuintrekken te beperken van de verplaatsing van gegevens. Bijvoorbeeld wanneer de kolom distributie vaak de gedeelde kolom in joins en aggregaties is, u zal worden tot een minimum beperken verplaatsing van gegevens. Het voordeel van de verplaatsing van de minimale gegevens mogelijk zwaarder wegen dan de impact van de gegevens laten hellen. 

De gebruikelijke manier voor het oplossen van gegevens schuintrekken is de tabel met een kolom verschillende distributielijsten opnieuw maken. Omdat er geen manier om de distributie kolom aan een bestaande tabel, de manier waarop de verdeling van een tabel wijzigen om deze opnieuw te maken met een [[CTAS]].  Hier volgen twee voorbeelden van het scheeftrekken gegevens oplossen:

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a>Voorbeeld 1: Maak de tabel met een nieuwe kolom voor distributie

[CTAS] [] wordt in dit voorbeeld een tabel met de kolom van een andere hash-distributielijsten opnieuw maken. 

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a>Voorbeeld 2: De tabel met behulp van round-robin distributie opnieuw maken

[CTAS] [] wordt in dit voorbeeld een round robin in plaats van een verdeling van de hash-tabel opnieuw te maken. Deze wijziging zal zelfs gegevensdistributie ten koste van hogere verkeer produceren. 

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het tabelontwerp, [verdelen][], [Index][], [partitie][], [Gegevenstypen][], [Statistieken][] en [Tijdelijke tabellen][tijdelijke] artikelen.

Voor een overzicht van aanbevolen procedures Zie [Aanbevolen procedures voor magazijn SQL-gegevens][].


<!--Image references-->

<!--Article references-->
[Overzicht]: ./sql-data-warehouse-tables-overview.md
[Gegevenstypen]: ./sql-data-warehouse-tables-data-types.md
[Distribueren]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partitie]: ./sql-data-warehouse-tables-partition.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md
[Tijdelijke]: ./sql-data-warehouse-tables-temporary.md
[Aanbevolen procedures voor magazijn SQL-gegevens]: ./sql-data-warehouse-best-practices.md
[Query controleren]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#querying-table-sizes

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->

<properties
   pageTitle="Statistieken over tabellen in SQL Data Warehouse beheren | Microsoft Azure"
   description="Aan de slag met statistieken over tabellen in Azure SQL Data Warehouse."
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
   ms.date="06/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a>Statistieken over tabellen in SQL Data Warehouse beheren

> [AZURE.SELECTOR]
- [Overzicht][]
- [Gegevenstypen][]
- [Distribueren][]
- [Index][]
- [Partitie][]
- [Statistieken][]
- [Tijdelijke][]

Hoe meer SQL Data Warehouse op de hoogte van uw gegevens, des te sneller query's op de gegevens kunnen worden uitgevoerd.  De manier waarop u SQL Data Warehouse vertellen over uw gegevens, is door het verzamelen van statistische gegevens over uw gegevens.  Met statistieken van uw gegevens is een van de belangrijkste dingen die u doen kunt om uw query's te optimaliseren.  SQL Data Warehouse is de meest optimale plan voor uw query's maken met behulp van statistieken.  Dit komt omdat het datawarehouse SQL query optimizer een optimizer kosten gebaseerd is.  Dat wil zeggen, vergelijkt de kosten van verschillende query's en kiest het plan met de laagste kosten, ook moet het plan dat het snelst zullen uitvoeren.

Statistieken kunnen worden gemaakt op een enkele kolom meerdere kolommen of een index van een tabel.  Statistische gegevens worden opgeslagen in een histogram bevat het bereik en de selectiviteit van de waarden.  Dit is met name van belang wanneer de optimizer moet evalueren JOINs, GROUP BY en HAVING- en WHERE-componenten in een query.  Bijvoorbeeld, als de optimizer schat dat de datum waarop u in uw query filtert retourneert 1 rij, kies een heel ander plan dan als deze optie ramingen datum die u hebt geselecteerd wordt 1 miljoen rijen.  Tijdens het maken van statistische gegevens zeer belangrijk is, is het belangrijk dat statistieken *nauwkeuriger* weerspiegelen de huidige status van de tabel.  Met up-to-date statistieken zorgt ervoor dat een goed plan is geselecteerd door de optimizer.  De plannen die worden gemaakt door de optimizer zijn alleen nuttig als de statistieken van uw gegevens.

Het proces van het maken en bijwerken van statistische gegevens is momenteel een handmatig proces, maar het is zeer eenvoudig om te doen.  Dit is in tegenstelling tot SQL Server die automatisch wordt gemaakt en bijgewerkt statistieken over enkele kolommen en indexen.  Met behulp van de onderstaande gegevens, kunt u het beheer van de statistieken aanzienlijk automatiseren voor uw gegevens. 

## <a name="getting-started-with-statistics"></a>Aan de slag met statistieken

 Bemonsterde statistieken maken voor elke kolom is een gemakkelijke manier om aan de slag met statistieken.  Aangezien het net zo belangrijk statistieken om up-to-date te houden, mogelijk een voorzichtige benadering voor het bijwerken van uw statistieken dagelijks of na elke belasting. Er zijn altijd e-mailverkeer tussen prestaties en de kosten voor het maken en bijwerken van statistische gegevens.  Als u dat het onderhouden van uw statistieken te lang duurt vindt, wilt u mogelijk proberen te nauwkeuriger kolommen die statistieken zijn of welke kolommen moeten regelmatig worden bijgewerkt.  U wilt bijwerken datumkolommen dagelijks nieuwe waarden kunnen worden toegevoegd in plaats van na elke belasting. Nogmaals, krijgt u het meeste voordeel door statistieken op kolommen die betrokken zijn in JOINs, GROUP BY en HAVING en WHERE-componenten.  Als u een tabel met een groot aantal kolommen die alleen worden gebruikt in de SELECT-component hebt, statistieken over deze kolommen niet kan helpen en uitgaven een beetje meer moeite om te bepalen alleen de kolommen waarin statistieken helpen, bespaart de tijd voor het onderhouden van uw statistieken.

## <a name="multi-column-statistics"></a>Statistieken met meerdere kolommen

Naast het maken van statistieken op kolommen, kan het gebeuren dat uw query's van statistieken met meerdere kolommen gebruikmaken.  Met meerdere kolommen statistieken worden statistieken gemaakt op een lijst met kolommen.  Ze bevatten één kolom statistieken in de eerste kolom in de lijst, plus enkele gegevens correlatie tussen kolom densiteit genoemd.  Bijvoorbeeld, als u een tabel die op twee kolommen samengevoegd hebben, wellicht dat SQL Data Warehouse beter het plan optimaliseren kunt wordt begrepen de relatie tussen de twee kolommen.   Statistieken met meerdere kolommen kunt verbeteren de prestaties van query's voor bepaalde bewerkingen, zoals samengestelde joins en groeperen op.

## <a name="updating-statistics"></a>De statistieken worden bijgewerkt

Statistieken bij te werken is een belangrijk onderdeel van uw database management routine.  Wanneer de verdeling van de gegevens in de database worden gewijzigd, moeten de statistieken worden bijgewerkt.  Verouderde statistieken zal leiden tot lagere optimale queryprestaties.

Één beste manier is om de statistieken voor de datumkolommen bijwerken elke dag als nieuwe datums worden toegevoegd.  Elke keer nieuwe rijen in het warehouse worden geladen, worden nieuwe belasting datums of transactiedatums toegevoegd. Deze de verdeling van de gegevens wijzigen en controleer de statistieken niet bijgewerkt. Daarentegen kan statistieken op een kolom country in de klantentabel nooit moeten worden bijgewerkt, zoals de verdeling van de waarden in het algemeen niet wijzigen. Ervan uitgaande dat de verdeling constante tussen klanten, is niet nieuwe rijen toe te voegen aan de variatie van de tabel gaat wijzigen de verdeling van de gegevens. Echter, als uw datawarehouse slechts één land bevat en u gegevens in een nieuw land importeren, waardoor de gegevens uit meerdere landen worden opgeslagen, zeker moet u statistieken op de kolom land.

Een van de eerste vragen die bij het oplossen van een query is, "Zijn de meest recente statistieken?"

Deze vraag wordt niet door de leeftijd van de gegevens kunnen worden beantwoord. Een object up-to-date statistieken mogelijk heel oud als is er geen wezenlijke verandering in de onderliggende gegevens. Als het aantal rijen dat aanzienlijk is gewijzigd of als er een wezenlijke verandering in de distributie van de waarden voor een bepaalde kolom *vervolgens* dan wordt het tijd voor het bijwerken van statistische gegevens.  

Voor een verwijzing naar **SQL Server** (niet SQL Data Warehouse) wordt automatisch statistieken in deze situaties:

- Als er geen rijen in de tabel wanneer u rijen toevoegt, krijgt u een automatische update van de statistieken
- Als u meer dan 500 rijen toevoegen aan een tabel met rijen van minder dan 500 (bv. bij het begin u hebt 499 en 500 rijen toevoegen aan een totaal van 999 rijen), krijgt u een automatische update 
- Als je eenmaal meer dan 500 rijen hebt u 500 extra rijen + 20% van de grootte van de tabel toevoegen voordat u een automatische update op de statistieken ziet

Omdat er geen DMV om te bepalen of de gegevens in de tabel is gewijzigd sinds de laatste statistieken zijn bijgewerkt, kan de leeftijd van uw statistieken weten bieden met een gedeelte van de afbeelding.  U kunt de volgende query om te bepalen voor het laatst de statistieken waar op elke tabel bijgewerkt.  

> [AZURE.NOTE] Denk eraan dat als er een wezenlijke verandering in de distributie van de waarden voor een bepaalde kolom, moet u bijwerken statistieken ongeacht de laatste keer dat ze zijn bijgewerkt.  

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

Datumkolommen in een gegevensmagazijn bijvoorbeeld meestal regelmatig hoeft te worden updates van de statistieken. Elke keer nieuwe rijen in het warehouse worden geladen, worden nieuwe belasting datums of transactiedatums toegevoegd. Deze de verdeling van de gegevens wijzigen en controleer de statistieken niet bijgewerkt.  Daarentegen zou statistieken op een kolom geslacht op een klantentabel nooit moeten worden bijgewerkt. Ervan uitgaande dat de verdeling constante tussen klanten, is niet nieuwe rijen toe te voegen aan de variatie van de tabel gaat wijzigen de verdeling van de gegevens. Echter, als uw datawarehouse slechts één geslacht en een nieuwe vereiste resulteert in verschillende geslachten bevat zeker moet u statistieken op de kolom geslacht.

Zie [Statistieken][] op MSDN voor meer uitleg.

## <a name="implementing-statistics-management"></a>Implementeren van management statistieken

Het is vaak een goed idee om uw proces om ervoor te zorgen dat de statistieken worden bijgewerkt aan het einde van de lading laden gegevens uitbreiden. Het laden van gegevens is als tabellen meest hun grootte en/of de distributie van de waarden wijzigen. Daarom is dit een logische plaats om bepaalde processen te implementeren.

Sommige leidende beginselen worden hieronder uw statistieken worden bijgewerkt tijdens het laden:

- Zorg ervoor dat elke tabel geladen ten minste één statistieken object bijgewerkt. Hiermee werkt u de informatie tabellen grootte (aantal rijen en aantal pagina's) als onderdeel van de statistieken.
- Gericht op kolommen die deel uitmaken van JOIN, GROEPEREN, sorteren en DISTINCT-componenten
- Kunt u kolommen 'oplopende sleutel' transactie datums vaker als deze waarden niet in het histogram statistieken opgenomen worden worden bijgewerkt.
- Kunt u statische verdeling kolommen minder vaak worden bijgewerkt.
- Denk eraan dat elke statistisch object wordt bijgewerkt in de reeks. Gewoon de toepassing van `UPDATE STATISTICS <TABLE_NAME>` mogelijk niet ideaal - vooral voor grote tabellen met veel objecten zijn statistieken.

> [AZURE.NOTE] Raadpleeg de SQL Server 2014 kardinaliteit schatting model whitepaper voor meer details over [oplopende sleutel].

Zie voor meer uitleg [Kardinaliteit schatting][] op MSDN.

## <a name="examples-create-statistics"></a>Voorbeelden: Maken van statistieken

Deze voorbeelden ziet u hoe u met verschillende opties voor het maken van statistieken. De opties die u voor elke kolom is afhankelijk van de kenmerken van uw gegevens en hoe de kolom wordt gebruikt in query's.

### <a name="a-create-single-column-statistics-with-default-options"></a>A. Statistieken voor één kolom met standaardopties maken

Verstrek om statistieken te maken voor een kolom, een naam voor het object statistieken en de naam van de kolom.

Deze syntaxis gebruikt alle standaardopties. SQL Data Warehouse monsters 20 procent van de tabel standaard bij het maken van statistieken.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Bijvoorbeeld:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a>B. Statistieken van één kolom maken door het onderzoeken van elke rij

De samplingfrequentie van standaard van 20 procent is voldoende voor de meeste situaties. U kunt echter de samplefrequentie aanpassen.

Om een voorbeeld van de volledige tabel met de volgende syntaxis:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Bijvoorbeeld:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-the-sample-size"></a>C. Statistieken voor één kolom maken door te geven van de grootte van de steekproef

U kunt ook de grootte van de steekproef opgeven als een percentage:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-the-rows"></a>D. Eén kolom statistieken op sommige rijen maken

Een andere optie kunt u statistische gegevens op een gedeelte van de rijen in de tabel. Dit heet een gefilterde statistiek.

U kunt gefilterde statistieken, bijvoorbeeld wanneer u van plan bent een specifieke partitie van een grote gepartitioneerde tabel opvragen. Statistieken over alleen de waarden van de partitie maakt, wordt de nauwkeurigheid van de statistieken verbeteren en daarom verbeteren de prestaties van query's.

In dit voorbeeld maakt de statistieken op een bereik van waarden. De waarden kunnen gemakkelijk worden gedefinieerd zodat deze overeenkomt met het bereik van waarden in een partitie.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [AZURE.NOTE] De query moet voor queryoptimalisatie overwegen om gefilterde statistieken wanneer het queryplan gedistribueerde gekozen passen binnen de definitie van het object statistieken. Met behulp van het vorige voorbeeld van de query waarin de component moet Kol1 waarden tussen 2000101 en 20001231.

### <a name="e-create-single-column-statistics-with-all-the-options"></a>E. Statistieken van één kolom maken met alle opties

Natuurlijk kunt u de opties samen combineren. In het volgende voorbeeld wordt een gefilterde statistieken-object gemaakt met een aangepaste sample-grootte:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Zie voor de volledige verwijzing [Maken statistieken][] op MSDN.

### <a name="f-create-multi-column-statistics"></a>F. Statistieken met meerdere kolommen maken

Als u wilt een statistieken met meerdere kolommen maken, gewoon gebruiken in de vorige voorbeelden, maar meer kolommen opgeven.

> [AZURE.NOTE] Het histogram voor het schatten van het aantal rijen in de resultaatset van de query wordt gebruikt, is alleen beschikbaar voor de eerste kolom in de definitie van het object statistieken weergegeven.

In dit voorbeeld wordt het histogram is op *product\_categorie*. Statistieken voor cross-kolom worden berekend op *product\_categorie* en *product\_sub_c\ategory*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Omdat er een correlatie is tussen het *product\_categorie* en *product\_sub\_categorie*, een stat met meerdere kolommen te gebruiken als u deze kolommen tegelijk worden benaderd.

### <a name="g-create-statistics-on-all-the-columns-in-a-table"></a>G. Statistieken voor de kolommen in een tabel maken

Opdrachten maken statistieken problemen is na het maken van de tabel één manier om statistieken te maken.

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="h-use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a>H. Een opgeslagen procedure gebruiken om statistieken te maken van alle kolommen in een database

SQL Data Warehouse heeft een systeem opgeslagen procedure gelijk aan [sp_create_stats] [] niet in SQL Server. Deze opgeslagen procedure maakt een enkele kolom statistieken op voor elke kolom van de database die nog niet de statistieken.

Hierdoor kunt u aan de slag met het ontwerp van de database. Altijd aan uw wensen aanpassen.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN   sys.[external_tables] e ON  e.[object_id]       = t.[object_id]
WHERE       l.[object_id] IS NULL
AND         e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

Om statistieken te maken voor alle kolommen in de tabel met deze procedure, belt u gewoon de procedure.

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a>Voorbeelden: update statistieken

Als u statistieken wilt bijwerken, kunt u het volgende doen:

1. Een object van de statistieken worden bijgewerkt. Geef de naam van de statistieken-object dat u wilt bijwerken.
2. Alle statistieken objecten in een tabel bijwerken. Geef de naam van de tabel in plaats van één specifieke statistieken-object.


### <a name="a-update-one-specific-statistics-object"></a>A. Een specifieke statistieken object bijwerken ###
Gebruik de volgende syntaxis voor het bijwerken van een object specifieke statistieken:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Bijvoorbeeld:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Statistieken voor specifieke objecten bijwerkt, kunt u de tijd en middelen die nodig zijn voor het beheren van statistieken te minimaliseren. Hiervoor moet zetten, maar om de beste statistieken bij te werken.


### <a name="b-update-all-statistics-on-a-table"></a>B. Alle statistieken op een tabel ###
Hier is een eenvoudige methode voor het bijwerken van alle statistieken-objecten in een tabel.

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Bijvoorbeeld:

```sql
UPDATE STATISTICS dbo.table1;
```

Deze verklaring is eenvoudig te gebruiken. Vergeet niet dit werkt u alle statistieken op de tabel en daarom meer werk dan nodig zou kunnen uitvoeren. Als de prestaties niet een probleem, maar dit is wel degelijk de eenvoudigste en meest volledige manier om te garanderen statistieken zijn bijgewerkt.

> [AZURE.NOTE] Wanneer u alle statistieken op een tabel bijwerkt, wordt SQL Data Warehouse een scan aan de tabel voor elke statistiek. Als de tabel is groot, veel statistieken en veel kolommen bevat, is het misschien efficiënter om afzonderlijke statistieken op basis van de noodzaak.

Voor een implementatie van een `UPDATE STATISTICS` procedure raadpleegt u het[tijdelijke] artikel [Tijdelijke tabellen]. De implementatie methode is iets anders aan de `CREATE STATISTICS` bovenstaande procedure, maar het eindresultaat is hetzelfde.

Zie [Update Statistics][] op MSDN voor de volledige syntaxis.

## <a name="statistics-metadata"></a>Metagegevens van statistieken
Er zijn verschillende functies kunt u informatie vinden over statistieken en systeem weergeven. Zo kunt u zien als een object statistieken mogelijk niet bijgewerkt met behulp van de functie stats datum zien wanneer de statistieken voor het laatst zijn gemaakt of bijgewerkt.

### <a name="catalog-views-for-statistics"></a>Catalogusweergaven voor statistieken
Deze systeemweergaven bevatten informatie over statistieken:

| Catalogus weergeven | Beschrijving |
| :----------- | :---------- |
| [sys.Columns][]  | Een rij voor elke kolom. |
| [sys.Objects][]  | Een rij voor elk object in de database. |  |
| [sys.schemas][]  | Een rij voor elk schema in de database. |  |
| [sys.Stats][] | Een rij voor elk object statistieken. |
| [sys.stats_columns][] | Een rij voor elke kolom in de statistieken-object. Koppelingen naar sys.columns. |
| [systeemtabellen][] | Een rij voor elke tabel (inclusief externe tabellen). |
| [sys.table_types][] | Een rij voor elk gegevenstype. |


### <a name="system-functions-for-statistics"></a>Systeemfuncties voor statistieken
Deze systeemfuncties zijn handig voor het werken met statistieken:

| De systeemfunctie | Beschrijving |
| :-------------- | :---------- |
| [STATS_DATE][]    | De datum waarop die de statistieken-object voor het laatst is bijgewerkt. |
| [DBCC SHOW_STATISTICS][] | Biedt overzicht niveau en gedetailleerde informatie over de distributie van de waarden zoals het object statistieken te begrijpen. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Functies en statistische kolommen combineren in één weergave

Deze weergave kolommen die betrekking op statistieken hebben brengt en het resultaat is van de functie van [STATS_DATE()] [] samen.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a>DBCC SHOW_STATISTICS()-voorbeelden

DBCC SHOW_STATISTICS() bevat de gegevens bewaard in een object statistieken. Deze gegevens zijn afkomstig uit drie delen.

1. Koptekst
2. Dichtheid Vector
3. Histogram

De koptekst van metagegevens over de statistieken. Het histogram geeft de verdeling van de waarden in de eerste kolom van de sleutel van het object statistieken. De vector dichtheid meet de correlatie tussen kolom. SQLDW berekent de kardinaliteit van ramingen met de gegevens in het object statistieken.

### <a name="show-header-density-and-histogram"></a>Koptekst, dichtheid en histogram weergeven

Dit eenvoudige voorbeeld ziet u alle drie de gedeelten van een object statistieken.

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Bijvoorbeeld:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>Een of meer gedeelten DBCC SHOW_STATISTICS();

Als u alleen geïnteresseerd bent in specifieke onderdelen te bekijken, gebruikt u de `WITH` component en opgeven welke onderdelen u wilt weergeven:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Bijvoorbeeld:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>DBCC SHOW_STATISTICS() verschillen
DBCC SHOW_STATISTICS() wordt meer strikt in SQL Data Warehouse vergeleken met SQL Server geïmplementeerd.

1. Niet-gedocumenteerde functies worden niet ondersteund.
- Stats_stream kan niet worden gebruikt.
- Resultaten voor specifieke subsets van de statistische gegevens bijvoorbeeld niet koppelen (STAT_HEADER JOIN DENSITY_VECTOR)
2. NO_INFOMSGS kan niet worden ingesteld voor het bericht onderdrukken
3. Vierkante haken rond statistieken namen kunnen niet worden gebruikt.
4. Kolomnamen niet gebruiken voor het identificeren van objecten statistieken
5. Aangepaste fout 2767 wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

Zie [DBCC SHOW_STATISTICS][] op MSDN voor meer informatie.  Voor meer informatie, Zie de artikelen op [Tabel overzicht][Overzicht], [Tabel gegevenstypen][Gegevenstypen], [distributie van een tabel][verdelen], [het indexeren van een tabel],[Index], [partitioneren van een tabel][partitie] en[tijdelijke] [Tijdelijke tabellen].  Voor meer informatie over aanbevolen procedures Zie [Aanbevolen procedures voor magazijn SQL-gegevens][].  

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

<!--MSDN references-->  
[De kardinaliteit van raming]: https://msdn.microsoft.com/library/dn600374.aspx
[MAKEN VAN STATISTIEKEN]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW_STATISTICS]:https://msdn.microsoft.com/library/ms174384.aspx
[Statistieken]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[sys.Columns]: https://msdn.microsoft.com/library/ms176106.aspx
[sys.Objects]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.Stats]: https://msdn.microsoft.com/library/ms177623.aspx
[sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[systeemtabellen]: https://msdn.microsoft.com/library/ms187406.aspx
[sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx
[STATISTIEKEN BIJWERKEN]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  

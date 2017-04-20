<properties
   pageTitle="Optimaliseren voor SQL Data Warehouse | Microsoft Azure"
   description="Best Practice richtlijnen voor efficiënte transactie updates schrijven in Azure SQL Data Warehouse"
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
   ms.date="07/31/2016"
   ms.author="jrj;barbkess"/>

# <a name="optimizing-transactions-for-sql-data-warehouse"></a>Optimaliseren voor SQL Data Warehouse

In dit artikel wordt uitgelegd hoe het optimaliseren van uw code transactionele minimaal risico voor lange terugdraaiversies.

## <a name="transactions-and-logging"></a>Registratie en transacties

Transacties zijn een belangrijk onderdeel van een relationele database-engine. SQL Data Warehouse transacties gebruikt tijdens wijziging van gegevens. Deze transacties kunnen worden expliciet of impliciet. Één `INSERT`, `UPDATE` en `DELETE` -instructies zijn allemaal voorbeelden van impliciete transacties. Expliciete transacties expliciet worden geschreven door een ontwikkelaar die met behulp van `BEGIN TRAN`, `COMMIT TRAN` of `ROLLBACK TRAN` en worden doorgaans gebruikt wanneer meerdere instructies voor een wijziging moeten worden gebundeld in één atomaire eenheid. 

Azure SQL Data Warehouse wijzigingen worden doorgevoerd in de database met behulp van transactielogboeken. Elke verdeling heeft een eigen transactielogboek. Transaction logboekschrijfbewerkingen zijn automatisch. Er is geen configuratie vereist. Terwijl dit proces zorgt ervoor dat het schrijven er een overhead introduceren in het systeem. U kunt deze gevolgen minimaliseren door transactioneel efficiënte code te schrijven. Transactioneel efficiënte code grotendeels worden onderverdeeld in twee categorieën.

- Minimale logboekregistratie constructies benutten waar mogelijk
- Procesgegevens binnen het bereik van batches enkelvoud langlopende transacties voorkomen
- Een patroon voor grote wijzigingen in een bepaalde partitie overschakelen naar een andere partitie vaststellen

## <a name="minimal-vs-full-logging"></a>Minimale vergeleken met volledige logboekregistratie.

In tegenstelling tot volledig geregistreerde bewerkingen die gebruikmaken van het transactielogboek voor het bijhouden van elke rijwijziging, minimaal geregistreerde bewerkingen bijhouden van toewijzingen omvang en meta-gegevens alleen worden gewijzigd. Daarom registreren alleen de informatie die nodig is voor het terugdraaien van de transactie in het geval van een storing of een expliciete aanvraag betrekking heeft op minimale registratie (`ROLLBACK TRAN`). Als u veel minder gegevens worden bijgehouden in het transactielogboek, wordt een minimaal geregistreerde bewerking beter dan een vergelijkbaar formaat volledig geregistreerde bewerking wordt uitgevoerd. Bovendien, aangezien minder schrijft het transactielogboek een veel kleinere hoeveelheid gegevens wordt gegenereerd en dus meer I/O efficiënt.

De transactie veiligheid alleen tijdslimieten volledig geregistreerde bewerkingen.

>[AZURE.NOTE] Minimaal geregistreerde bewerkingen kunnen in expliciete transacties deelnemen. Als u alle wijzigingen in de toewijzing structuren worden bijgehouden, is het mogelijk terug te draaien minimaal geregistreerde bewerkingen. Het is belangrijk te begrijpen dat de wijziging 'minimaal' wordt vastgelegd is niet niet geregistreerd.

## <a name="minimally-logged-operations"></a>Minimaal geregistreerde bewerkingen

De volgende bewerkingen zijn kan minimaal worden geregistreerd:

- TABEL AS SELECT ([CTAS][]) maken
- INVOEGEN... SELECTEER
- INDEX MAKEN
- ALTER INDEX OPNIEUW MAKEN
- DROP INDEX
- TABEL AFKAPPEN
- TABEL VERWIJDEREN
- PARTITIE VAN ALTER TABLE SWITCH

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

>[AZURE.NOTE] Interne data verkeer bewerkingen (zoals `BROADCAST` en `SHUFFLE`) worden niet beïnvloed door de transactie veiligheid limiet.

## <a name="minimal-logging-with-bulk-load"></a>Minimale logboekregistratie met bulksgewijs laden

`CTAS`en `INSERT...SELECT` zijn zowel in bulk load-bewerkingen. Echter, beide worden beïnvloed door de tabeldefinitie doel en is afhankelijk van het scenario van de belasting. Hieronder vindt u een tabel waarin wordt beschreven als de bulkbewerking volledig of minimaal geregistreerd:  

| Primaire Index               | Belasting-Scenario                                            | Logmodus |
| --------------------------- | -------------------------------------------------------- | ------------ |
| Heap                        | Alle                                                      | **Minimale**  |
| Gegroepeerde Index             | Lege doeltabel                                       | **Minimale**  |
| Gegroepeerde Index             | Geladen rijen elkaar niet overlappen met de bestaande pagina's in doel | **Minimale**  |
| Gegroepeerde Index             | Geladen rijen overlappen met de bestaande pagina's in doel        | Volledige         |
| Geclusterde Index voor Columnstore | Batch grootte > = 102,400 per partitie uitgelijnd distributie | **Minimale**  |
| Geclusterde Index voor Columnstore | Batch grootte < 102,400 per partitie uitgelijnd distributie  | Volledige         |

Het is vermeldenswaardig dat alle schrijfbewerkingen naar de secundaire of niet-geclusterde indexen bijwerken volledig geregistreerde bewerkingen is.

> [AZURE.IMPORTANT] SQL Data Warehouse heeft 60 verdelingen. Daarom, ervan uitgaande dat alle rijen worden gelijkmatig verdeeld en aanvoer in een enkele partitie, uw batch moet 6,144,000 rijen bevatten of minimaal zijn aangemeld bij het schrijven naar een Columnstore geclusterde Index groter. Als de tabel is gepartitioneerd en de rijen worden ingevoegd partitie grenzen omvatten, moet u 6,144,000 rijen per partitie grens ervan uitgaande dat zelfs gegevensdistributie. Elke partitie in elke distributie moet onafhankelijk groter zijn dan de drempel 102.400 rij voor de insert minimaal worden vastgelegd in de distributie.

Laden van gegevens naar een niet-lege tabel met een geclusterde index kan bevatten vaak een mengsel van volledig geregistreerde en minimaal geregistreerde rijen. Een gegroepeerde index is een evenwichtige structuur (b-structuur) van de pagina's. Als de pagina al bevat rijen uit een andere transactie wordt geschreven, wordt vervolgens deze schrijft volledig vastgelegd. Echter, als de pagina leeg is vervolgens het schrijven naar die pagina wordt minimaal geregistreerd.

## <a name="optimizing-deletes"></a>Hiermee verwijdert u optimaliseren

`DELETE`het is een volledig geregistreerd.  Als u een grote hoeveelheid gegevens in een tabel of een partitie te verwijderen, het vaak verstandiger aan `SELECT` de gegevens u behouden wilt, die als een minimaal geregistreerde bewerking kan worden uitgevoerd.  Om dit te bereiken, moet u een nieuwe tabel maken met [CTAS][].  Eenmaal hebt gemaakt, kunt [HERNOEMEN][] weghalen van uw oude tabel met de nieuwe tabel.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the 
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Optimaliseren van updates

`UPDATE`het is een volledig geregistreerd.  Als u nodig hebt voor het bijwerken van een groot aantal rijen in een tabel of een partitie kan vaak veel efficiënter gebruik van een minimaal geregistreerde bewerking zoals [CTAS][] te doen.

Update in het voorbeeld hieronder een volledige tabel is geconverteerd naar een `CTAS` dat minimale registratie mogelijk is.

In dit geval zijn we een kortingsbedrag achteraf toevoegen aan de verkoop in de tabel:

```sql
--Step 01. Create a new table containing the "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(   CLUSTERED INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [AZURE.NOTE] Grote tabellen opnieuw te maken kan profiteren van de functies voor magazijnbeheer voor SQL-gegevens werkbelasting. Raadpleeg voor meer informatie de sectie in het artikel [gelijktijdigheid][] werkbelasting.

## <a name="optimizing-with-partition-switching"></a>Optimaliseren met partitie-switching

Wanneer u wordt geconfronteerd met grote schaal wijzigingen in een [partitie tabel][], wordt een patroon overschakelen naar een andere partitie een heleboel zin. Als de wijziging van de gegevens is een belangrijke omvat meerdere partities, vervolgens worden gewoon met iteratie van de partities hetzelfde resultaat bereikt.

De stappen voor het uitvoeren van een switch partitie zijn als volgt:
1. Maak een leeg van partitie
2. De 'update' uitvoeren als een CTAS
3. Overschakelen van de bestaande gegevens in de tabel
4. Ga in de nieuwe gegevens.
5. Gegevens opschonen

Echter, om vast te stellen van de partities te schakelen eerst moeten we bouwen een helper procedure zoals hieronder. 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,   @table_name            NVARCHAR(128)
,   @boundary_value        INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
WITH CTE
AS
(
SELECT  s.name                          AS [schema_name]
,       t.name                          AS [table_name]
,       p.partition_number              AS [ptn_nmbr]
,       p.[rows]                        AS [ptn_rows]
,       CAST(r.[value] AS INT)          AS [boundary_value]
FROM        sys.schemas                 AS s
JOIN        sys.tables                  AS t    ON  s.[schema_id]       = t.[schema_id]
JOIN        sys.indexes                 AS i    ON  t.[object_id]       = i.[object_id]
JOIN        sys.partitions              AS p    ON  i.[object_id]       = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes       AS h    ON  i.[data_space_id]   = h.[data_space_id]
JOIN        sys.partition_functions     AS f    ON  h.[function_id]     = f.[function_id]
LEFT JOIN   sys.partition_range_values  AS r    ON  f.[function_id]     = r.[function_id] 
                                                AND r.[boundary_id]     = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT  *
FROM    CTE
WHERE   [schema_name]       = @schema_name
AND     [table_name]        = @table_name
AND     [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Deze procedure wordt gemaximaliseerd hergebruik van code en blijft de compactere voorbeeld overschakelen naar een andere partitie.

De volgende code ziet u de vijf stappen die hierboven wordt vermeld om een volledige partitie overschakelen naar een andere routine.

```sql
--Create a partitioned aligned empty table to switch out the data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE   OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]   SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))   +' TO [dbo].[FactInternetSales_out] PARTITION ' +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))   +' TO [dbo].[FactInternetSales] PARTITION '     +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Logboekregistratie met kleine batches minimaliseren

Voor veel bewerkingen voor het wijzigen, kan het zinvol de bewerking onderverdelen in segmenten of batches voor het bereik van de eenheid van het werk.

Hieronder vindt u een werkend voorbeeld. De batchgrootte is ingesteld op een getal trivial markeren van de techniek. In werkelijkheid zou de batchgrootte aanzienlijk groter worden. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,       SalesOrderNumber
,       SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE   [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE @seq_start      INT = 1
,       @batch_iterator INT = 1
,       @batch_size     INT = 50
,       @max_seq_nmbr   INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,       @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE   @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT  1
            FROM    #t t
            WHERE   seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND     FactInternetSales.SalesOrderNumber      = t.SalesOrderNumber
            AND     FactInternetSales.SalesOrderLineNumber  = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Onderbreken en schalen van richtlijnen

Azure SQL Data Warehouse kunt u pauzeren, hervatten en schalen van uw gegevensmagazijn op aanvraag. Bij onderbreken of schalen van uw SQL Data Warehouse is het belangrijk om te weten dat tijdens een vlucht transacties onmiddellijk afgesloten; veroorzaakt door openstaande transacties moet worden teruggedraaid. Als uw werkbelasting had uitgegeven een wijziging van de gegevens lang actief en onvolledige voorafgaand aan de bewerking onderbreken of schaal, moet dit werk ongedaan worden gemaakt. Dit kan invloed hebben op hoe lang die het duurt om te pauzeren en schalen van uw Azure SQL Data Warehouse-database. 

> [AZURE.IMPORTANT] Beide `UPDATE` en `DELETE` zijn volledig geregistreerde bewerkingen en zodat deze ongedaan maken/opnieuw bewerkingen kunnen aanzienlijk langer duren dan vergelijkbare bewerkingen minimaal vastgelegd. 

Het beste scenario is dat flight wijziging gegevenstransacties volledig onderbreken of scaling SQL Data Warehouse. Echter, dit kan niet altijd praktisch zijn. Zodat het risico van een lange terugdraaien kunt u op een van de volgende opties:

- Langdurige bewerkingen opnieuw schrijven met behulp van [CTAS][]
- De bewerking opsplitsen in stukken; werken op een subset van rijen

## <a name="next-steps"></a>Volgende stappen

Zie [transacties in SQL Data Warehouse][] voor meer informatie over transactionele limieten en isolatieniveaus.  Voor een overzicht van aanbevolen procedures Zie [Aanbevolen procedures voor magazijn SQL-gegevens][].

<!--Image references-->

<!--Article references-->
[Transacties in SQL datawarehouse]: ./sql-data-warehouse-develop-transactions.md
[partitie tabel]: ./sql-data-warehouse-tables-partition.md
[Gelijktijdigheid]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Aanbevolen procedures voor magazijn SQL-gegevens]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[NAAM WIJZIGEN]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->


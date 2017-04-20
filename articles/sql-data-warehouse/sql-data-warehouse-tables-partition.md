<properties
   pageTitle="Partitioneren van tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Aan de slag met de tabel partitioneren in Azure SQL Data Warehouse."
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
   ms.date="07/18/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="partitioning-tables-in-sql-data-warehouse"></a>Partitioneren van tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Overzicht][]
- [Gegevenstypen][]
- [Distribueren][]
- [Index][]
- [Partitie][]
- [Statistieken][]
- [Tijdelijke][]

Partitionering wordt ondersteund op alle SQL Data Warehouse tabeltypen; met inbegrip van geclusterde columnstore, geclusterde index en heap.  Partitionering wordt ondersteund op alle typen van distributie, met inbegrip van zowel hash of round robin verdeeld.  Hiermee kunt u uw gegevens opsplitsen in kleinere groepen van gegevens en in de meeste gevallen partitionering gepartitioneerd in een datumkolom.

## <a name="benefits-of-partitioning"></a>Voordelen van het partitioneren

Partities kan profiteren van onderhouds- en prestaties van gegevens.  Of beide of slechts één profiteren is afhankelijk van hoe de gegevens worden geladen en of dezelfde kolom kan worden gebruikt voor beide doeleinden, aangezien de partitionering kan alleen worden uitgevoerd op één kolom.

### <a name="benefits-to-loads"></a>Voordelen voor belastingen

Het belangrijkste voordeel van partitioneren in SQL Data Warehouse is verbetering van de efficiëntie en prestaties van de geladen gegevens door het gebruik van de partitie verwijderen, overschakelen en samenvoegen.  Gegevens in de meeste gevallen is gepartitioneerd op een datumkolom die nauw is verbonden met de volgorde waarin de gegevens in de database is geladen.  Een van de grootste voordelen van het gebruik van partities voor gegevens dat het vermijden van transactielogboeken.  Terwijl gewoon invoegen, bijwerken of verwijderen van gegevens de meest directe aanpak, met een beetje creativiteit en inzet is, met behulp van partities tijdens het laden kan aanzienlijk de prestaties verbeteren.

Overschakelen van de partitie kan worden gebruikt om snel te verwijderen of vervangen van een gedeelte van een tabel.  Een feitentabel verkoop bevatten mogelijk alleen gegevens voor de laatste 36 maanden.  Aan het einde van elke maand wordt de oudste maand van verkoopgegevens verwijderd uit de tabel.  Deze gegevens kan worden verwijderd met behulp van een delete-instructie te verwijderen van de gegevens voor de maand oud.  Echter kan verwijderen van een grote hoeveelheid gegevens-per-rij met een delete-instructie erg lang duren, evenals maken het risico van grote transacties lang te draaien duren kan als er iets misgaat.  Een meer optimale aanpak is het gewoon de oudste data-partitie.  Waar de afzonderlijke rijen verwijderen krijgen uren, kan verwijderen van een hele partitie seconden duren.

### <a name="benefits-to-queries"></a>Voordelen voor query 's

Partitionering kan worden gebruikt voor het verbeteren van de prestaties van query's.  Als een query wordt een filter op een gepartitioneerde kolom toegepast, kan dit de scan wordt alleen het in aanmerking komend partities die veel kleinere subset van de gegevens, het voorkomen van een tabelscan van de volledige te beperken.  Met de introductie van columnstore geclusterde indexen de prestatievoordelen predikaat schrapping minder gunstig zijn, maar in sommige gevallen kan er een vergoeding voor query's.  Bijvoorbeeld, als de feitentabel verkoop is verdeeld in 36 maanden, met behulp van het veld Verkoopdatum, vervolgens een query naar het filter op de verkoopdatum kunt overslaan in partities die niet overeenkomen met het filter zoeken.

## <a name="partition-sizing-guidance"></a>Partitie grootte richtlijnen

Terwijl de partitionering kan worden gebruikt om prestaties te verbeteren bepaalde scenario's, kan maakt een tabel met **te veel** partities prestaties onder bepaalde omstandigheden schade.  Deze problemen zijn met name het geval voor geclusterde columnstore tabellen.  Voor het partitioneren om u te helpen, is het belangrijk te begrijpen wanneer partitionering gebruikt en het aantal partities te maken.  Er is geen regel voor vaste snel over de vraag hoeveel partities te veel, dit is afhankelijk van uw gegevens en het aantal partities worden geladen om tegelijkertijd.  Maar denk als algemene stelregel 10s-100s van partities, niet 1000s toevoegen.

Bij het maken van partities op de **gegroepeerde columnstore** tabellen, is het belangrijk rekening te houden met het aantal rijen in elke partitie zal terechtkomen.  Voor optimale compressie en de prestaties van tabellen geclusterde columnstore, is een minimum van 1 miljoen rijen per distributie en partitie nodig.  Voordat de partities zijn gemaakt, verdeelt SQL Data Warehouse al elke tabel in 60 gedistribueerde databases.  De partitionering wordt toegevoegd aan een tabel is een aanvulling op de verdelingen gemaakt achter de schermen.  In dit voorbeeld wordt gebruikt als de verkoop feitentabel 36 maandelijkse partities bevat en gezien het feit dat SQL Data Warehouse 60 verdelingen heeft, vervolgens de verkoop feitentabel moet 60 miljoen rijen per maand, of 2,1 miljard rijen bevatten wanneer alle maanden zijn gevuld.  Als een tabel aanzienlijk minder rijen dan de aanbevolen minimum aantal rijen per partitie bevat, kunt u minder partities te maken met het aantal rijen per partitie vergroten.  Zie ook artikel[Index] [indexering]met query's op SQL Data Warehouse voor de beoordeling van de kwaliteit van de cluster columnstore indexen kunnen worden uitgevoerd.

## <a name="syntax-difference-from-sql-server"></a>Verschil van de syntaxis van SQL Server

SQL Data Warehouse introduceert een vereenvoudigde definitie van partities die enigszins van SQL Server verschilt.  Partitionering functies en schema's niet worden gebruikt in SQL Data Warehouse als in SQL Server.  In plaats daarvan hoeft u is gepartitioneerde kolom en de punten grens aangeven.  Hoewel de syntaxis van partitionering mogelijk iets anders dan SQL Server, zijn de basisconcepten hetzelfde.  Ondersteuning voor SQL Server en SQL Data Warehouse één partitiekolom per tabel die varieerden van partitie.  Zie voor meer informatie over het partitioneren, [gepartitioneerde tabellen en indexen][].

Het onderstaande voorbeeld van een instructie SQL Data Warehouse gepartitioneerde [tabel maken][] , partities in de tabel FactInternetSales in de kolom OrderDateKey:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
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
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Partitionering van SQL Server migreren

Gewoon partitie definities van SQL Server migreren naar SQL Data Warehouse:

- De SQL Server- [partitieschema][]elimineren.
- De definitie van de [partitiefunctie][] toevoegen aan uw tabel maken.

Als u een gepartitioneerde tabel van een SQL Server-exemplaar migreert de onderstaande SQL kunt u het aantal rijen in elke partitie kunnen verzoeken.  Houd er rekening mee dat als de granulatie voor dezelfde partities op SQL Data Warehouse wordt gebruikt, het aantal rijen per partitie met een factor van 60 verminderen zal.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Beheer van de werkbelasting

Een sluitstuk rekening te houden de beschikking van de partitie tabel is de [werkbelasting te beheren][].  Beheer van de werkbelasting in SQL Data Warehouse is vooral het beheer van geheugen en gelijktijdigheid.  De maximale hoeveelheid geheugen toegewezen aan elke distributie tijdens de uitvoering van de query is geregeld bij resource classes in SQL Data Warehouse.  In het ideale geval wordt de partities worden grote tegen andere factoren zoals de geheugenvereisten van columnstore geclusterde indexen maken.  Geclusterde indexen voordeel van columnstore aanzienlijk wanneer ze meer geheugen worden toegewezen.  Daarom wilt u ervoor zorgen dat de index opnieuw opbouwen van een partitie van geheugen niet is gezet. Verhogen van de hoeveelheid geheugen die beschikbaar is voor de query kan worden bereikt door het overschakelen van de standaardrol, smallrc, een van de andere functies zoals largerc.

Informatie over de toewijzing van geheugen per distributie is beschikbaar via query's in de resourceweergaven gouverneur-dynamic management. In werkelijkheid zal uw subsidie geheugen minder zijn dan de onderstaande figuren. Dit biedt echter een niveau van richtlijnen die u gebruiken kunt wanneer u het formaat van de partities van het beheer gegevens.  Probeer te voorkomen dat het formaat van uw partities groter dan de geheugen-subsidie verstrekt door de klasse extra groot resource. Als uw partities groter dan dit bedrag loopt u het risico van geheugendruk die op zijn beurt tot een minder optimale compressie leidt.

```sql
SELECT  rp.[name]                               AS [pool_name]
,       rp.[max_memory_kb]                      AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                 AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576              AS [mex_memory_gb]
,       rp.[max_memory_percent]                 AS [max_memory_percent]
,       wg.[name]                               AS [group_name]
,       wg.[importance]                         AS [group_importance]
,       wg.[request_max_memory_grant_percent]   AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups  wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools   rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Overschakelen van partitie

SQL Data Warehouse ondersteunt partitie splitsen, samenvoegen en schakelen. Elk van deze functies is excuted met de instructie [ALTER TABLE][] .

Als u wilt overschakelen van partities tussen twee tabellen moet u ervoor zorgen dat de partities op de grenzen van hun respectieve uitlijnen en dat de definities overeenkomen. Als check-beperkingen niet beschikbaar zijn voor het afdwingen van het bereik van waarden in een tabel is de brontabel mag de grenzen van de dezelfde partitie als de doeltabel. Als dit niet het geval is, wordt de schakeloptie partitie mislukken zal als de partitie-metagegevens worden niet gesynchroniseerd.

### <a name="how-to-split-a-partition-that-contains-data"></a>Het splitsen van een partitie met gegevens

De meest efficiënte methode voor het splitsen van een partitie die al gegevens bevat, is het gebruik van een `CTAS` instructie. Als de gepartitioneerde tabel een geclusterde columnstore is moet de partitie tabel leeg zijn voordat kan worden gesplitst.

Hieronder ziet u een voorbeeld columnstore gepartitioneerde tabel met één rij in elke partitie:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
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
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [AZURE.NOTE] De statistiek door object te maken, wij zorgen ervoor dat metagegevens van de tabel nauwkeuriger. Als we het maken van statistische gegevens weglaat, wordt SQL Data Warehouse standaardwaarden gebruikt. Raadpleeg voor meer informatie over statistieken, [Statistieken][].

We kunnen uitvoeren voor de rij tellen met de `sys.partitions` catalogus weergeven:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Als we deze tabel splitsen, krijgen we een fout:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Msg 35346 15 niveau 1 staat, regel 44 GESPLITST-component van de instructie ALTER partitie is mislukt omdat de partitie niet leeg is.  Alleen lege partities kunnen worden opgesplitst als een columnstore-index voor de tabel bestaat. Houd rekening met het uitschakelen van de index columnstore vóór de afgifte van de instructie ALTER partitie en vervolgens de columnstore-index opnieuw samenstellen nadat de partitie wijzigen is voltooid.

We kunnen echter gebruiken `CTAS` naar een nieuwe tabel maken om onze gegevens bevatten.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Een switch is toegestaan als de grenzen van de partitie worden uitgelijnd. Dit betekent dat de brontabel met een lege partitie die we later kunt splitsen.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Die nog moet doen is naar onze gegevens uitlijnen op de grenzen van de nieuwe partitie met `CTAS` en switch onze gegevens opnieuw in aan de hoofdtabel

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Als u klaar bent met het verplaatsen van de gegevens is een goed idee voor het vernieuwen van de statistiek van de doeltabel zodat deze precies overeenkomen met de nieuwe verdeling van de gegevens in hun respectieve partities:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Tabel bronbeheer partitioneren

Om te voorkomen dat uw definitie van **roesten** in uw bronbeheersysteem is het verstandig rekening te houden met de volgende benadering:

1. De tabel maken als een gepartitioneerde tabel, maar geen waarden partitie

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
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
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

2. `SPLIT`de tabel als onderdeel van het implementatieproces:

```sql
-- Create a table containing the partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over the partition boundaries and split the table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

Met deze aanpak van de code in het besturingselement statisch blijft en de partitionering grens-waarden zijn toegestaan dynamisch; na verloop van tijd aan het magazijn in ontwikkeling.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie, Zie de artikelen op [Tabel overzicht][Overzicht], [Tabel gegevenstypen][Gegevenstypen], [distributie van een tabel][verdelen], [indexeren van een tabel],[Index], [Onderhouden van tabelstatistieken],[Statistieken] en[tijdelijke] [Tijdelijke tabellen].  Voor meer informatie over aanbevolen procedures Zie [Aanbevolen procedures voor magazijn SQL-gegevens][].

<!--Image references-->

<!--Article references-->
[Overzicht]: ./sql-data-warehouse-tables-overview.md
[Gegevenstypen]: ./sql-data-warehouse-tables-data-types.md
[Distribueren]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partitie]: ./sql-data-warehouse-tables-partition.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md
[Tijdelijke]: ./sql-data-warehouse-tables-temporary.md
[beheer van de werkbelasting]: ./sql-data-warehouse-develop-concurrency.md
[Aanbevolen procedures voor magazijn SQL-gegevens]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Gepartitioneerde tabellen en indexen]: https://msdn.microsoft.com/library/ms190787.aspx
[TABEL WIJZIGEN]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[TABEL MAKEN]: https://msdn.microsoft.com/library/mt203953.aspx
[partitiefunctie]: https://msdn.microsoft.com/library/ms187802.aspx
[partitieschema]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->

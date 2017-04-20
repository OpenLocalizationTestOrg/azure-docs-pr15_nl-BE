<properties
   pageTitle="Tijdelijke tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Aan de slag met tijdelijke tabellen in Azure SQL Data Warehouse."
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
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="temporary-tables-in-sql-data-warehouse"></a>Tijdelijke tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Overzicht][]
- [Gegevenstypen][]
- [Distribueren][]
- [Index][]
- [Partitie][]
- [Statistieken][]
- [Tijdelijke][]

Tijdelijke tabellen zijn erg handig bij het verwerken van gegevens - met name tijdens de transformatie waarbij de tussenliggende resultaten tijdelijk zijn. In SQL Data Warehouse bestaan tijdelijke tabellen op het niveau van de sessie.  Ze zijn alleen zichtbaar voor de sessie waarin ze zijn gemaakt en worden automatisch verbroken wanneer die sessie zich afmeldt.  Tijdelijke tabellen bieden een vergoeding voor de prestaties omdat de resultaten op lokale en externe opslag niet worden geschreven.  Tijdelijke tabellen zijn enigszins afwijken in Azure SQL Data Warehouse Azure SQL-Database, zoals ze toegankelijk is vanuit een willekeurige plaats binnen de sessie, inclusief zowel binnen als buiten een opgeslagen procedure.

Dit artikel bevat richtlijnen voor het gebruik van tijdelijke tabellen voor essentiële en markeert u de beginselen van de sessie op tijdelijke tabellen. Met de informatie in dit artikel kunt u de code, verbetering van hergebruik en de vereenvoudiging van het onderhoud van uw code modularize.

## <a name="create-a-temporary-table"></a>Maak een tijdelijke tabel

Tijdelijke tabellen zijn gemaakt door het voorvoegsel gewoon de naam van de tabel met een `#`.  Bijvoorbeeld:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]       NVARCHAR(128) NOT NULL
,   [table_name]            NVARCHAR(128) NOT NULL
,   [stats_name]            NVARCHAR(128) NOT NULL
,   [stats_is_filtered]     BIT           NOT NULL
,   [seq_nmbr]              BIGINT        NOT NULL
,   [two_part_name]         NVARCHAR(260) NOT NULL
,   [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
```

Tijdelijke tabellen kunnen ook worden gemaakt met een `CTAS` met dezelfde aanpak:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

>[AZURE.NOTE] `CTAS`is een krachtige opdracht en het extra voordeel van zeer efficiënt in het gebruik van de transactie logboekruimte. 


## <a name="dropping-temporary-tables"></a>Tijdelijke tabellen neer te zetten

Wanneer een nieuwe sessie wordt gemaakt, zou geen tijdelijke tabellen bestaan.  Echter als u verbinding maakt met dezelfde opgeslagen procedure, een tijdelijk bestand gemaakt met dezelfde naam, om ervoor te zorgen dat uw `CREATE TABLE` instructies zijn geslaagd een eenvoudige controle vooraf bestaan met een `DROP` kan worden gebruikt als in het onderstaande voorbeeld:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Het is een goede gewoonte om dit patroon gebruiken voor tabellen en tijdelijke tabellen voor het coderen van consistentie.  Het is ook een goed idee om met `DROP TABLE` voor het verwijderen van tijdelijke tabellen als u klaar bent met hen in uw code.  Ontwikkeling is vrij gebruikelijk bij het drop-opdrachten die zijn samengevoegd aan het einde van een procedure om ervoor te zorgen deze objecten worden in de opgeslagen procedure opgeschoond.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Code modularizing

Aangezien tijdelijke tabellen u ergens in de sessie van een gebruiker ziet, kan dit u helpen uw toepassingscode modularize worden misbruikt.  Bijvoorbeeld combineert de volgende opgeslagen procedure het beste van boven DDL update van alle statistische gegevens in de database met de naam van de statistieken te genereren.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

In dit stadium is de enige actie die heeft plaatsgevonden, is het maken van een opgeslagen procedure die alleen wordt gegenereerd een tijdelijke tabel #stats_ddl, met een DDL-instructies.  Deze opgeslagen procedure wordt #stats_ddl weghalen als deze al bestaat, om ervoor te zorgen dat wordt niet als meer dan één keer wordt uitgevoerd in een sessie.  Echter, omdat er geen `DROP TABLE` aan het einde van de opgeslagen procedure, wanneer de opgeslagen procedure is voltooid, het laat de gemaakte tabel zodat deze buiten de opgeslagen procedure kan worden gelezen.  In SQL Data Warehouse is in tegenstelling tot andere SQL Server-databases, het mogelijk om met de tijdelijke tabel buiten de procedure die u hebt gemaakt.  Tijdelijke tabellen SQL Data Warehouse kunnen gebruikt **ergens** in de sessie worden. Dit kan leiden tot meer modulaire en beheerbare code als in het onderstaande voorbeeld:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Beperkingen van de tijdelijke tabel

SQL Data Warehouse een aantal beperkingen opleggen bij de uitvoering van tijdelijke tabellen.  Op dit moment alleen sessie bereik tijdelijke tabellen worden ondersteund.  Globale tijdelijke tabellen worden niet ondersteund.  Bovendien kunnen niet weergaven worden gemaakt op tijdelijke tabellen.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie, Zie de artikelen op [Tabel overzicht][Overzicht], [Tabel gegevenstypen][Gegevenstypen], [distributie van een tabel][verdelen], [indexeren van een tabel],[Index], [partitioneren van een tabel][partitie] en [Onderhouden van tabelstatistieken],[Statistieken].  Voor meer informatie over aanbevolen procedures Zie [Aanbevolen procedures voor magazijn SQL-gegevens][].

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

<!--Other Web references-->

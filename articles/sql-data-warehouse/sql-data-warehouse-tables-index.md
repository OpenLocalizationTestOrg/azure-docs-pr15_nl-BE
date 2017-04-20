<properties
   pageTitle="Tabellen in SQL Data Warehouse indexeren | Microsoft Azure"
   description="Aan de slag met tabel indexeren in Azure SQL Data Warehouse."
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
   ms.date="07/12/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="indexing-tables-in-sql-data-warehouse"></a>Tabellen in SQL Data Warehouse indexeren

> [AZURE.SELECTOR]
- [Overzicht][]
- [Gegevenstypen][]
- [Distribueren][]
- [Index][]
- [Partitie][]
- [Statistieken][]
- [Tijdelijke][]

SQL Data Warehouse biedt verschillende opties voor indexeren met inbegrip van [columnstore van geclusterde indexen][], [gegroepeerde indexen en niet geclusterde indexen][].  Daarnaast biedt het ook een geen indexoptie ook bekend als de [heap][].  Dit artikel heeft betrekking op de voordelen van elk indextype, alsmede tips voor het verkrijgen van de meeste prestaties van de indexen. Zie [tabelsyntaxis maken][] voor meer informatie over het maken van een tabel in SQL Data Warehouse.

## <a name="clustered-columnstore-indexes"></a>Columnstore geclusterde indexen

SQL Data Warehouse maakt standaard een index geclusterde columnstore wanneer er geen opties zijn opgegeven op een tabel. Geclusterde columnstore tabellen bieden zowel de hoogste mate van compressie als de beste algemene prestaties van query's.  Geclusterde columnstore tabellen wordt geclusterde index of heap-tabellen in het algemeen beter en zijn meestal de beste keuze voor grote tabellen.  Om deze redenen is de gegroepeerde columnstore de beste plaats om te starten als u niet zeker bent van het indexeren van uw tabel.  

Een geclusterde columnstore als tabel wilt maken, gewoon GECLUSTERDE INDEX voor COLUMNSTORE in de WITH-component opgeeft, of laat de WITH-component uit:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Er zijn enkele scenario's waar geclusterde columnstore niet mogelijk een goede optie:

- Columnstore tabellen ondersteunen geen secundaire niet-gegroepeerde indexen.  Overweeg heap of geclusterde index tabellen.
- Columnstore tabellen ondersteunen geen varchar(max), nvarchar(max) en varbinary(max).  Overweeg heap of geclusterde index.
- Columnstore tabellen mogelijk minder efficiënt voor tijdelijke gegevens.  Overweeg heap en misschien zelfs tijdelijke tabellen.
- Kleine tabellen met minder dan 100 miljoen rijen.  U kunt tabellen heap.

## <a name="heap-tables"></a>Heap-tabellen

Wanneer u worden tijdelijk gegevens op SQL Data Warehouse aanvoer, merkt u dat met behulp van een tabel heap het algehele proces sneller verloopt brengt.  Dit komt omdat de lasten heaps zijn sneller dan met index tabellen en in sommige gevallen die het lezen uit de cache kan worden uitgevoerd.  Als u gegevens alleen om fase-voordat u meer transformaties laadt, laden van de tabel op tabel heap worden veel sneller dan het laden van de gegevens op een geclusterde columnstore-tabel. Bovendien laden van gegevens naar een [tijdelijke tabel][tijdelijk] wordt ook veel sneller wordt geladen dan het laden van een tabel naar een permanente opslag.  

Voor kleine opzoektabellen, minder dan 100 miljoen rijen, logisch vaak heap tabellen.  Cluster columnstore tabellen beginnen met het behalen van optimale compressie zodra er meer dan 100 miljoen rijen is.

Een heap als tabel wilt maken, geeft u HEAP in de WITH-component:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Gegroepeerde en niet-geclusterde indexen

Gegroepeerde indexen kunnen tabellen geclusterde columnstore beter als een enkele rij moet snel worden opgehaald.  Voor query's waarvoor één of enkele rij opzoeken prestaties met extreme snelheid vereist is, kunt u overwegen een clusterindex of niet-geclusterde index voor secundaire.  Het nadeel van een geclusterde index is dat alleen de query's die met een zeer selectieve filter in de kolom geclusterde index zal profiteren.  Ter verbetering van de filter op andere kolommen kan een niet-geclusterde index worden toegevoegd aan andere kolommen.  Echter wordt elke index die wordt toegevoegd aan een tabel ruimte en de verwerkingstijd toevoegen aan belasting.

Een gegroepeerde index als tabel wilt maken, geeft u GEGROEPEERDE INDEX in de WITH-component:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Als een niet-geclusterde index voor een tabel toevoegen, gebruik gewoon de volgende syntaxis:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

> [AZURE.NOTE] Een niet-geclusterde index wordt standaard gemaakt wanneer CREATE INDEX wordt gebruikt. Voorts is een niet-geclusterde index alleen toegestaan op een tabel rij opslag (HEAP of GECLUSTERDE INDEX). Niet-geclusterde indexen op een GECLUSTERDE INDEX van COLUMNSTORE is op dit moment niet toegestaan.


## <a name="optimizing-clustered-columnstore-indexes"></a>Columnstore geclusterde indexen optimaliseren

Geclusterde columnstore tabellen zijn onderverdeeld in gegevens in segmenten.  Met segment van hoge kwaliteit is essentieel voor het bereiken van optimale queryprestaties in een tabel columnstore.  Segment-kwaliteit kan worden gemeten door het aantal rijen in een Rijengroep gecomprimeerd.  Quality segment is meest optimale wanneer er ten minste 100K rijen per rij gecomprimeerd groeperen en zo de prestaties als het aantal rijen per rij groep aanpak 1.048.576 rijen is de meeste rijen die een Rijengroep kan bevatten.

De volgende weergave kan worden gemaakt en die op uw systeem wordt gebruikt voor het berekenen van het gemiddelde aantal rijen per rij groeperen en indexen columnstore sub optimale cluster identificeren.  De laatste kolom in deze weergave wordt gegenereerd als de SQL-instructie die kan worden gebruikt om de indexen opnieuw opbouwen.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,   COUNT(DISTINCT rg.[partition_number])                   AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,   CEILING ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Nu dat u de weergave hebt gemaakt, deze query uitvoeren om te bepalen van tabellen met een rij met minder dan 100K rijen.  U wilt natuurlijk Verhoog de drempel van 100 kB als u naar meer segment van optimale kwaliteit zoekt. 

```sql
SELECT  *
FROM    [dbo].[vColumnstoreDensity]
WHERE   COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Als u de query die u beginnen kunt met de gegevens bekijken en analyseren van de resultaten hebben uitgevoerd. Deze tabel wordt uitgelegd wat u in uw groep rij analyse.


| Kolom                             | Deze gegevens gebruiken                                                                                                                                                                      |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [table_partition_count]            | Als de tabel is gepartitioneerd, kan vervolgens u verwachten hogere Open Rijengroep telt. Elke partitie in de verdeling kan in theorie een open Rijengroep gekoppeld hebben. Deze factor in uw analyse. Een kleine tabel die is is gepartitioneerd doordat de partitionering helemaal niet kan worden geoptimaliseerd als dit zou leiden tot betere compressie.                                                                        |
| [row_count_total]                  | Totale aantal rijen voor de tabel. Bijvoorbeeld, kunt u deze waarde om het percentage van de rijen in de gecomprimeerde staat te berekenen.                                                                      |
| [row_count_per_distribution_MAX]   | Als u alle rijen gelijkmatig verdeeld zijn deze waarde wordt het doel aantal rijen per distributie. Deze waarde vergelijken met de compressed_rowgroup_count.                                 |
| [COMPRESSED_rowgroup_rows]         | Totaal aantal rijen in de indeling voor de tabel columnstore.                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_AVG]     | Als het gemiddelde aantal rijen aanzienlijk minder dan het maximum aantal rijen voor een Rijengroep is, vervolgens overwegen CTAS of ALTER INDEX opnieuw maken om de gegevens te comprimeren                     |
| [COMPRESSED_rowgroup_count]        | Het aantal Rijgroepen in de indeling columnstore. Als dit aantal zeer hoog ten opzichte van de tabel is is een indicator dat de dichtheid van de columnstore laag is.                                  |
| [COMPRESSED_rowgroup_rows_DELETED] | In columnstore-indeling zijn logisch rijen verwijderd. Als het getal hoog is ten opzichte van de tabelgrootte is, kunt u de partitie opnieuw te maken of opnieuw samenstellen van de index deze fysiek verwijderd. |
| [COMPRESSED_rowgroup_rows_MIN]     | Gebruik deze in combinatie met AVG en het maximum aantal kolommen te begrijpen van het bereik van waarden voor de Rijgroepen in uw columnstore. Een laag getal de belasting overschrijdt (102,400 per partitie uitgelijnd distributie) suggereert dat de optimalisaties in het laden van gegevens beschikbaar zijn                                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_MAX]     | Hierboven                                                                                                                                                                                  |
| [OPEN_rowgroup_count]              | Open Rijgroepen zijn normaal. Een zou één OPEN Rijengroep per tabeldistributie (60) verwachten. Grote getallen voorgesteld gegevens laden van meerdere partities. Controleer de partities strategie om te controleren of deze is goed                                                                                                                                                                                                |
| [OPEN_rowgroup_rows]               | Elke rijgroep hebben 1.048.576 rijen in het hoogste. Gebruik deze waarde om te zien hoe vol de open Rijgroepen zijn op dit moment                                                                 |
| [OPEN_rowgroup_rows_MIN]           | Open groepen aangeven dat gegevens op nieuwe wordt geladen in de tabel of de vorige lading gemorst resterende rijen in deze Rijengroep is. Gebruik de MIN, MAX, AVG kolommen om te zien hoeveel gegevens is zat in OPEN rij groepen. Voor kleine tabellen wordt 100% van alle gegevens! In dat geval ALTER INDEX opnieuw maken om te zorgen dat de gegevens naar columnstore.                                                                       |
| [OPEN_rowgroup_rows_MAX]           | Hierboven                                                                                                                                                                                  |
| [OPEN_rowgroup_rows_AVG]           | Hierboven                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows]             | Bekijk de rij gesloten groep rijen als een controle.                                                                                                                                       |
| [CLOSED_rowgroup_count]            | Het aantal Rijgroepen gesloten moet laag als een geheel worden gezien. Gesloten Rijgroepen kunnen worden geconverteerd naar het gecomprimeerde rowg roups met behulp van de INDEX wijzigen... SANEREN opdracht. Dit is echter niet nodig. Gesloten groepen worden automatisch geconverteerd naar columnstore Rijgroepen door het proces van achtergrond "tupel mover".                                                                                               |
| [CLOSED_rowgroup_rows_MIN]         | Gesloten Rijgroepen moeten hebben een vulling met zeer hoge snelheid. Als de snelheid van de opvulling voor een Rijengroep gesloten laag is, zijn verdere analyse van de columnstore is vereist.                                   |
| [CLOSED_rowgroup_rows_MAX]         | Hierboven                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows_AVG]         | Hierboven                                                                                                                                                                                  |
| [Rebuild_Index_SQL]         | SQL columnstore-index voor een tabel opnieuw samenstellen                                                                                                                                                     |

## <a name="causes-of-poor-columnstore-index-quality"></a>Oorzaken van slechte columnstore index kwaliteit

Als u tabellen hebt geïdentificeerd met segment van slechte kwaliteit, zult u de oorzaak bepalen.  Hieronder vindt u enkele algemene oorzaken van slechte segment quaility:

1. Geheugenbelasting wanneer de index is gemaakt.
2. Groot aantal DML-bewerkingen
3. Kleine of druppelen binnen operations laden
4. Te veel partities

Deze factoren kunnen leiden tot een columnstore-index hebben aanzienlijk kleiner is dan de optimale 1 miljoen rijen per Rijengroep van de.  Zij kunnen ook worden rijen naar de groep delta rij in plaats van een Rijengroep gecomprimeerd. 

### <a name="memory-pressure-when-index-was-built"></a>Geheugenbelasting wanneer de index is gemaakt.

Het aantal rijen per gecomprimeerd Rijengroep rechtstreeks verband houden met de breedte van de rij en de hoeveelheid geheugen die beschikbaar is voor het verwerken van de Rijengroep.  Wanneer de rijen worden geschreven naar columnstore tabellen geheugenbelasting, columnstore segment kwaliteit nadelig beïnvloeden.  Daarom is de beste manier is om de sessie die u naar de columnstore index tabellen toegang tot zoveel mogelijk geheugen schrijft.  Omdat er een compromis tussen geheugen en gelijktijdigheid, de richtsnoeren voor de toewijzing van het juiste geheugen is afhankelijk van de gegevens in elke rij van de tabel, het bedrag van de DWU die u hebt toegewezen aan het systeem en het bedrag van gelijktijdigheid van "slots" die u kunt geven met de sessie die gegevens naar de tabel schrijft.  Beste, het beste beginnen met xlargerc als u DW300 of minder largerc als u DW400 DW600 en mediumrc als DW1000 en hierboven.

### <a name="high-volume-of-dml-operations"></a>Groot aantal DML-bewerkingen

Een groot aantal DML-bewerkingen bijwerken en verwijderen van rijen kan leiden tot inefficiëntie in de columnstore. Dit geldt met name als de meerderheid van de rijen in een Rijengroep worden gewijzigd.

- Alleen logisch verwijderen van een rij uit een Rijengroep gecomprimeerd, wordt de rij gemarkeerd als verwijderd. De rij pas in de Rijengroep gecomprimeerde de partitie of de tabel opnieuw wordt samengesteld.
- Rij invoegen van een rij worden toegevoegd aan een interne rowstore-tabel een rij delta groep genoemd. De ingevoegde rij wordt niet geconverteerd naar columnstore totdat de rij delta groep vol is en is gemarkeerd als gesloten. Rijgroepen worden gesloten zodra deze de maximale capaciteit van 1.048.576 rijen bereikt. 
- Bijwerken van een rij in de columnstore wordt verwerkt als een logische delete en insert. De ingevoegde rij kan worden opgeslagen in het archief van de delta.

Update batch verwerkt en bewerkingen die groter zijn dan de drempelwaarde bulk 102.400 rijen per partitie uitgelijnde distributie rechtstreeks naar de indeling van de columnstore wordt geschreven in te voegen. Echter, als een gelijke verdeling, moet u meer dan 6.144 miljoen rijen in één bewerking hiervoor wijzigt. Als het aantal rijen voor een bepaalde partitie uitgelijnd is distributie minder dan 102,400 rijen gaan naar de winkel delta en er blijven totdat voldoende rijen hebt ingevoegd of gewijzigd voor het sluiten van de Rijengroep of de index opnieuw is opgebouwd.

### <a name="small-or-trickle-load-operations"></a>Kleine of druppelen binnen operations laden

Kleine geladen stroom in SQL Data Warehouse worden soms ook bekend als druppelen binnen wordt geladen. Meestal staan voor een bijna constante gegevensstroom wordt ingenomen door het systeem. Deze stroom bij continu is het volume van rijen is echter niet bijzonder groot. De gegevens zijn vaak aanzienlijk onder de drempel die nodig zijn voor een directe belasting naar de indeling columnstore.

In deze gevallen is het vaak beter op grond van de gegevens eerst in Azure blob-opslag en laat deze vóór het laden. Deze techniek wordt vaak aangeduid als *micro batchen*.

### <a name="too-many-partitions"></a>Te veel partities

Een andere factor is de impact van de partities op uw geclusterde columnstore-tabellen.  Voordat de partitionering verdeelt SQL Data Warehouse al uw gegevens in databases 60.  Partitioneren van uw gegevens verder wordt verdeeld.  Als u uw gegevens staan, vervolgens zult u rekening houden dat **elke** partitie moet ten minste 1 miljoen rijen om te profiteren van een columnstore voor geclusterde index.  Als u de tabel in partities 100 verdelen, moet de tabel ten minste 6 miljard rijen om te profiteren van een columnstore voor geclusterde index (60 verdelingen *100 partities* 1 miljoen rijen). Als u uw partitietabel 100 geen rijen 6 miljard, verminderen het aantal partities of gebruik in plaats hiervan een heap-tabel.

Zodra uw tabellen met enkele gegevens zijn geladen, volgt u de onderstaande stappen te volgen voor het identificeren en tabellen met sub optimale cluster columnstore indexen opnieuw maken.

## <a name="rebuilding-indexes-to-improve-segment-quality"></a>Opnieuw opbouwen van indexen voor het verbeteren van kwaliteit van segment

### <a name="step-1-identify-or-create-user-which-uses-the-right-resource-class"></a>Stap 1: Bepalen of gebruiker met de juiste resource klasse maken

Een snelle manier onmiddellijk segment om kwaliteit te verbeteren, is de index opnieuw maken.  De SQL-code die wordt geretourneerd door de bovenstaande weergave als resultaat een instructie ALTER INDEX opnieuw maken, die kan worden gebruikt om de indexen opnieuw opbouwen.  Bij het opnieuw samenstellen van de indexen, zorg dat u voldoende geheugen aan de sessie die wordt de index opnieuw toewijzen.  Hiertoe vergroten de resource-klasse van een gebruiker met machtigingen voor het opnieuw samenstellen van de index in deze tabel is het aanbevolen minimum.  De bronklasse van de database-eigenaar gebruiker kan niet worden gewijzigd, dus als u een gebruiker op het systeem hebt gemaakt, moet u dat eerst doen.  Het wordt aangeraden minimum is xlargerc als u DW300 of minder largerc als u DW400 DW600 en mediumrc als DW1000 en boven.

Hieronder ziet u een voorbeeld van hoe meer geheugen toewijzen aan een gebruiker doordat de klasse van de resource.  Voor meer informatie over de resource classes en het maken van een nieuwe gebruiker kunnen u vinden in het [beheer van gelijktijdigheid en werkbelasting] [ Concurrency] artikel.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>Stap 2: Columnstore geclusterde indexen opnieuw opbouwen met hogere resource klasse gebruiker
Aanmelden als de gebruiker in stap 1 (bv. LoadUser), die wordt nu met behulp van een hogere klasse van de resource en voer de instructies ALTER INDEX.  Zorg ervoor dat deze gebruiker heeft de machtiging wijzigen aan de tabellen waarop de index wordt opnieuw opgebouwd.  Deze voorbeelden ziet u hoe de hele columnstore index opnieuw maken of het opnieuw opbouwen van een enkele partitie. Op grote tabellen is het meer praktische tot het opnieuw opbouwen van indexen één partitie tegelijk.

In plaats van het opnieuw samenstellen van de index, kan u ook de tabel kopiëren naar een nieuwe tabel met [CTAS][].  Welke het beste is? Voor grote hoeveelheden gegevens is [CTAS][] meestal sneller dan [ALTER INDEX][]. Voor kleinere hoeveelheden gegevens [ALTER INDEX][] is eenvoudiger te gebruiken en won't, moet u de tabel vervangen.  Zie **Rebuilding indexen met CTAS en partitie switching** hieronder voor meer informatie over het opnieuw opbouwen van indexen met CTAS.

```sql
-- Rebuild the entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

Opnieuw maken van een index in het datawarehouse SQL is een off line bewerking.  Zie de sectie ALTER INDEX opnieuw maken in de [Indexen defragmentatie van Columnstore][] en het onderwerp syntaxis [ALTER INDEX][]voor meer informatie over het opnieuw maken van indexen.
 
### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>Stap 3: Controleer of geclusterde columnstore segment kwaliteit is verbeterd.
Opnieuw uitvoeren van de query welke tabel geïdentificeerd met slechte kwaliteit segment en segment kwaliteit controleren is verbeterd.  Als quality segment is niet verbeterd, kan het zijn dat de rijen in de tabel extra breed zijn.  Overweeg het gebruik van een hogere klasse van de resource of DWU bij het opnieuw samenstellen van de indexen.

 
## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>Opnieuw opbouwen van indexen met CTAS en het wijzigen van de partitie

In dit voorbeeld wordt de [CTAS][] en partitie overschakelt naar een partitie tabel opnieuw samenstellen. 

```sql
-- Step 1: Select the partition of data and write it out to a new table using CTAS
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
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Create a SWITCH out table
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
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT the data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN the rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2;
```

Voor meer informatie over het opnieuw maken van partities met `CTAS`, Zie het artikel van de [partitie][] .

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie, Zie de artikelen op [Tabel overzicht][Overzicht], [Tabel gegevenstypen][Gegevenstypen], [distributie van een tabel][verdelen], [partitioneren van een tabel][partitie], [Onderhouden van tabelstatistieken],[Statistieken] en[tijdelijke] [Tijdelijke tabellen].  Zie voor meer informatie over beste praktijken, [Best Practices voor magazijn SQL-gegevens][].

<!--Image references-->

<!--Article references-->
[Overzicht]: ./sql-data-warehouse-tables-overview.md
[Gegevenstypen]: ./sql-data-warehouse-tables-data-types.md
[Distribueren]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partitie]: ./sql-data-warehouse-tables-partition.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md
[Tijdelijke]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Aanbevolen procedures voor magazijn SQL-gegevens]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[INDEX WIJZIGEN]: https://msdn.microsoft.com/library/ms188388.aspx
[heap]: https://msdn.microsoft.com/library/hh213609.aspx
[gegroepeerde indexen en niet geclusterde indexen]: https://msdn.microsoft.com/library/ms190457.aspx
[de tabelsyntaxis van de maken]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore indexen defragmentatie]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[columnstore geclusterde indexen]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->

<properties
    pageTitle="SQL In het geheugen, aan de slag | Microsoft Azure"
    description="Technologieën voor SQL In het geheugen verbeteren aanzienlijk de prestaties van transactionele en analytics werkbelasting. Informatie over het gebruik van deze technologieën."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jodebrui"/>


# <a name="get-started-with-in-memory-preview-in-sql-database"></a>Aan de slag met In het geheugen (voorbeeld) in de SQL-Database

Functies in het geheugen verbeteren aanzienlijk de prestaties van transactionele en analytics werkbelasting in de juiste situaties.

Dit onderwerp ligt de nadruk op twee demonstraties, één voor de In Memory OLTP en één voor Analytics In het geheugen. Elke demo wordt geleverd compleet met de stappen en moet u de demo over het uitvoeren van code. U kunt op:

- Gebruik de code testen variaties om verschillen in prestatieresultaten; of
- De code te begrijpen van het scenario en voor het maken en gebruiken van de objecten In het geheugen gelezen.

> [AZURE.VIDEO azure-sql-database-in-memory-technologies]

- [Snelle Start 1: In Memory OLTP-technologieën voor snellere prestaties van T-SQL](http://msdn.microsoft.com/library/mt694156.aspx) -is een ander artikel kunt u aan de slag.

#### <a name="in-memory-oltp"></a>In Memory OLTP

De functies van de In Memory [OLTP](#install_oltp_manuallink) (online transaction processing) zijn:

- Tabellen geheugen wordt geoptimaliseerd.
- Opgeslagen procedures worden gecompileerd.


Een tabel geheugen geoptimaliseerd heeft een vertegenwoordiging van zichzelf in het actieve geheugen, naast de standaard weergave op een vaste schijf. Zakelijke transacties ten opzichte van de tabel wordt sneller uitgevoerd omdat ze rechtstreeks communiceren met alleen de weergave in het actieve geheugen.

Met de In Memory OLTP, kunt u bereiken van 30 keer krijgen in de doorvoercapaciteit voor transacties, afhankelijk van de specifieke kenmerken van de werkbelasting.


Opgeslagen procedures kunnen gecompileerde vereist minder machine-instructies tijdens de uitvoering dan traditionele geïnterpreteerd opgeslagen procedures. Wij hebben eigen compilatieresultaat in duur van 1/100th van de geïnterpreteerde duur.


#### <a name="in-memory-analytics"></a>In het geheugen Analytics 

De functie van de In het geheugen [Analytics](#install_analytics_manuallink) is:

Columnstore-indexen verbeteren de prestaties van deze analyses en rapporten, query's. 


#### <a name="real-time-analytics"></a>Real-Time Analytics

U combineert In Memory OLTP en Analytics voor [Real-Time Analytics](http://msdn.microsoft.com/library/dn817827.aspx) :

- Real-time zakelijk inzicht op basis van operationele gegevens.


#### <a name="availability"></a>Beschikbaarheid


GA, algemene beschikbaarheid:

- [Columnstore-indexen](http://msdn.microsoft.com/library/dn817827.aspx) die *op schijf*zijn.


Voorbeeld:

- In Memory OLTP
- Real-Time operationele Analytics


Overwegingen wanneer de functies In-Memory in voorbeeld zijn beschreven [verderop in dit onderwerp](#preview_considerations_for_in_memory).


> [AZURE.NOTE] Deze voorbeeld-functies zijn alleen beschikbaar voor [*Premium*](sql-database-service-tiers.md) -databases niet in groepen van elastische en niet beschikbaar zijn voor Basic of Standard-databases.  Ondersteuning voor databases in groepen van elastische Premium is binnenkort beschikbaar. 


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="a-install-the-in-memory-oltp-sample"></a>A. Het monster In Memory OLTP installeren

U kunt de voorbeelddatabase AdventureWorksLT [V12] maken door een paar klikken in de [portal Azure](https://portal.azure.com/). Klik in deze sectie wordt uitgelegd hoe verrijking van uw database AdventureWorksLT met:

- De tabellen in het geheugen.
- Een opgeslagen procedure worden gecompileerd.


#### <a name="installation-steps"></a>Installatiestappen

1. Maken in de [portal Azure](https://portal.azure.com/)een Premium-database op een server V12. Stel de **bron** op de voorbeelddatabase AdventureWorksLT [V12].
 - Voor gedetailleerde instructies ziet u [uw eerste Azure SQL-database maken](sql-database-get-started.md).

2. Verbinding maken met de database met SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. De [In Memory OLTP Transact-SQL-script](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) naar het Klembord kopiëren.
 - De T-SQL-script maakt de benodigde geheugen-objecten in de database AdventureWorksLT die u in stap 1 hebt gemaakt.

4. De T-SQL-script in SSMS te plakken en vervolgens het script uitvoeren.
 - Cruciaal is de `MEMORY_OPTIMIZED = ON` component CREATE TABLE-instructies, zoals in:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Fout 40536


Als u fout 40536 bij het uitvoeren van de T-SQL-script, voer het volgende T-SQL-script om te controleren of de database In het geheugen ondersteunt:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Een resultaat van **0** betekent In het geheugen wordt niet ondersteund en 1 dat wordt ondersteund. Het probleem kunnen onderzoeken:

- Controleer dat de database is gemaakt nadat u de functies In Memory OLTP actief is geworden voor het voorbeeld.
- Controleer de database in de laag Premium service.


#### <a name="about-the-created-memory-optimized-items"></a>Over de gemaakte geheugen geoptimaliseerd artikelen

**Tabellen**: het monster bevat de volgende geheugen geoptimaliseerd tabellen:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


U kunt tabellen via het **Object Explorer** in SSMS door geoptimaliseerd voor geheugen controleren:

- Klik met de rechtermuisknop op **tabellen** > **Filter** > **Filterinstellingen** > **Geheugen Is geoptimaliseerd** , is gelijk aan 1.


Of u kunt een query uitvoeren de catalogusweergaven, zoals:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Standaard gecompileerde opgeslagen procedure**: SalesLT.usp_InsertSalesOrder_inmem kan worden gecontroleerd via een query catalogus weergeven:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

## <a name="run-the-sample-oltp-workload"></a>Uitvoeren van de werklast van de OLTP monster

Het enige verschil tussen de volgende twee *opgeslagen procedures* is dat de eerste procedure geheugen geoptimaliseerde versie van de tabellen worden gebruikt, terwijl de tweede procedure wordt de gewone op schijf tabellen:

- **.** SalesLT usp_InsertSalesOrder**_inmem**
- **.** SalesLT usp_InsertSalesOrder**_ondisk**


In dit gedeelte ziet u hoe de twee procedures op stress niveaus met de handige **ostress.exe** -hulpprogramma. U kunt vergelijken hoe lang het duurt voordat de twee stress uitgevoerd om te voltooien.


Wanneer u ostress.exe uitvoert, raden we aan dat u de parameterwaarden zijn ontworpen om zowel doorgeven:

- Uitvoeren van een groot aantal gelijktijdige verbindingen via - n100.
- Elke verbinding lus honderden keren, door zijn gebruik - r500.


Echter, u wilt beginnen met veel kleinere waarden als - n10 en - r50 om ervoor te zorgen de alles werkt.


### <a name="script-for-ostressexe"></a>Script voor ostress.exe


Deze sectie toont de T-SQL-script dat is ingesloten in de opdrachtregel van onze ostress.exe. Het script maakt gebruik van items die zijn gemaakt door de T-SQL-script dat u eerder hebt geïnstalleerd.


Het volgende script voegt een monster verkooporder met vijf artikelen in de volgende geheugen geoptimaliseerd- *tabellen*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;
    
INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);
    
WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Als u de versie _ondisk van de voorgaande T-SQL voor ostress.exe, zou u beide exemplaren van de subtekenreeks *_inmem* gewoon vervangen door *_ondisk*. Deze vervangt de invloed op de namen van de tabellen en opgeslagen procedures.


### <a name="install-rml-utilities-and-ostress"></a>RML-hulpprogramma's en ostress installeren


In het ideale geval zou u van plan bent uit te voeren ostress.exe op een Azure VM. U maakt een [Azure Virtual Machine](https://azure.microsoft.com/documentation/services/virtual-machines/) in dezelfde Azure geografische regio waar de AdventureWorksLT-database zich bevindt. Maar u kunt ostress.exe uitvoeren op uw laptop in plaats daarvan.


Op de VM of op elke host u kiest, installeert u de Replay Markup Language (RML)-hulpprogramma's, waaronder ostress.exe.

- Zie de bespreking van de ostress.exe in de [In Memory OLTP-voorbeelddatabase](http://msdn.microsoft.com/library/mt465764.aspx).
 - Of [In Memory OLTP-voorbeelddatabase](http://msdn.microsoft.com/library/mt465764.aspx).
 - Of Zie de [Blog voor het installeren van ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx)



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Voer eerst de werkbelasting _inmem stress


Een *RML Cmd* -venster kunt u onze ostress.exe vanaf de opdrachtregel uitvoeren. De opdrachtregelparameters directe ostress naar:

- 100 verbindingen gelijktijdig worden uitgevoerd (-n100).
- Elke verbinding heeft 50 keer uitvoeren van de T-SQL-script (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


De voorgaande opdrachtregel voor ostress.exe uitvoeren:


1. De inhoud van de database opnieuw door de volgende opdracht uit te voeren in SSMS te verwijderen van de gegevens die door een eerder uitgevoerde is ingevoegd:
```
EXECUTE Demo.usp_DemoReset;
```

2. De tekst van de voorgaande opdrachtregel van ostress.exe naar het Klembord kopiëren.

3. Vervang de `<placeholders>` voor de parameters -S - U -P -d met de juiste reële waarden.

4. De bewerkte regel in een RML Cmd-venster worden uitgevoerd.


#### <a name="result-is-a-duration"></a>Resultaat is een duur


Wanneer ostress.exe is voltooid, worden de duur uitvoeren als de laatste regel van de uitvoer in het venster met Cmd RML geschreven. Bijvoorbeeld in een kortere test run ongeveer 1,5 minuten heeft geduurd:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Opnieuw instellen, bewerken voor _ondisk en vervolgens opnieuw uit te voeren


Nadat u het resultaat van het uitvoeren van de _inmem hebt, kunt u de volgende stappen uitvoeren voor het uitvoeren van _ondisk:


1. De database opnieuw instellen door de volgende opdracht uit te voeren in SSMS te verwijderen van de gegevens die door de vorige sessie is ingevoegd:
```
EXECUTE Demo.usp_DemoReset;
```

2. Bewerk de regel ostress.exe opdracht om alle *_inmem* vervangen door *_ondisk*.

3. Ostress.exe voor de tweede keer opnieuw, en het resultaat van de duur vast te leggen.

4. De database voor de verantwoordelijke verwijdering van wat een grote hoeveelheid testgegevens kan worden opnieuw ingesteld.


#### <a name="expected-comparison-results"></a>Met verwachte vergelijkingsresultaten

Onze tests In het geheugen is gebleken een prestatieverbetering **9 maal** voor deze eenvoudig werkbelasting met ostress op een Azure VM in Azure dezelfde regio als de database uitgevoerd.



<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;


## <a name="b-install-the-in-memory-analytics-sample"></a>B. Het monster In Memory Analytics installeren


In deze sectie Vergelijk de statistieken en i/o-resultaten bij gebruik van een columnstore-index ten opzichte van een traditionele b tree-index.


Real-time analytics op een werkbelasting OLTP is het vaak beter om een columnstore van niet-geclusterde index. Zie voor meer informatie [Columnstore indexen beschreven](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>De columnstore analytics test voorbereiden


1. Gebruik de Azure portal een database te maken vers AdventureWorksLT uit het monster.
 - Deze exacte naam gebruiken.
 - Kies een Premium service tier.

2. De [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) naar het Klembord kopiëren.
 - De T-SQL-script maakt de benodigde geheugen-objecten in de database AdventureWorksLT die u in stap 1 hebt gemaakt.
 - Het script wordt gemaakt in de tabel dimensie en twee feitentabellen. De feitentabellen gevuld met 3,5 miljoen rijen.
 - Het script kan 15 minuten duren.

3. De T-SQL-script in SSMS te plakken en vervolgens het script uitvoeren.
 - Cruciaal is het sleutelwoord **COLUMNSTORE** op de **CREATE INDEX** -instructie, zoals in:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. AdventureWorksLT instellen op het niveau van compatibiliteit 130:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`
 - 130 niveau is niet rechtstreeks verband houden met de functies In het geheugen. Maar in het algemeen biedt snellere query's dan 120 130 niveau.


#### <a name="crucial-tables-and-columnstore-indexes"></a>Cruciale tabellen en indexen voor columnstore


- dbo. FactResellerSalesXL_CCI is een tabel met een geclusterde **columnstore** -index, die over compressie op het niveau van *gegevens geavanceerde beschikt* .

- dbo. FactResellerSalesXL_PageCompressed is een tabel met een gelijkwaardige reguliere geclusterde index die op het niveau van de *pagina* is gecomprimeerd.


#### <a name="crucial-queries-to-compare-the-columnstore-index"></a>Cruciale query's naar de index columnstore vergelijken


[Hier](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) worden de verschillende typen van T-SQL query kan worden uitgevoerd voor een overzicht van betere prestaties. Vanaf stap 2 in de T-SQL-script is er een paar van query's die van direct belang zijn. De twee query's verschillen alleen op één regel:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Een columnstore voor geclusterde index is in de tabel FactResellerSalesXL**_CCI** .

Het volgende fragment van T-SQL script afdrukken statistieken voor i/o- en tijd voor de query voor elke tabel


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a Clustered Columnstore index CCI 
-- The comparison numbers are even more dramatic the larger the table is, this is a 11 million row table only.
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```



<a id="preview_considerations_for_in_memory" name="preview_considerations_for_in_memory"></a>


## <a name="preview-considerations-for-in-memory-oltp"></a>Overwegingen voor de In Memory OLTP Preview


De functies In Memory OLTP in Azure SQL-Database [voor het voorbeeld op 28 oktober 2015 actief](https://azure.microsoft.com/updates/public-preview-in-memory-oltp-and-real-time-operational-analytics-for-azure-sql-database/)is geworden.


In de huidige voorvertoning wordt In Memory OLTP alleen ondersteund voor:

- Databases die zich op een laag *Premium* service.

- Databases die zijn gemaakt nadat de In Memory OLTP-functies actief is geworden.
 - Een nieuwe database kan niet In Memory OLTP ondersteunen als dit is hersteld van een database die is gemaakt voordat de functies In Memory OLTP actief is geworden.


Bij twijfel kunt u altijd de volgende T-SQL SELECT om na te gaan of de database In Memory OLTP ondersteunt uitvoeren. Een resultaat van **1** betekent dat de database ondersteunt In Memory OLTP:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```


Als de query **1 retourneert**, OLTP In het geheugen wordt ondersteund in deze database en een databasekopie en de database terugzetten die zijn gemaakt op basis van deze database.


#### <a name="objects-allowed-only-at-premium"></a>Objecten mag alleen op de premie


Als een database een van de volgende soorten In Memory OLTP-objecten of typen bevat, wordt downgraden van de servicelaag van de database van Premium Basic- of Standard niet ondersteund. Als u wilt downgraden van de database, moet u eerst deze objecten weghalen:

- Tabellen geoptimaliseerd voor geheugen
- Geheugen geoptimaliseerd tabeltypen
- Standaard gecompileerde modules


#### <a name="other-relationships"></a>Andere relaties


- In Memory OLTP met behulp van functies met databases in elastische groepen wordt niet ondersteund tijdens een voorvertoning.
 - Als u wilt een database die is of heeft een elastische toepassingen In Memory OLTP-objecten verplaatsen, als volgt te werk:
  - 1. Alle tabellen geheugen geoptimaliseerd, tabeltypen en standaard gecompileerde modules van T-SQL in de database verwijderen
  - 2. De servicelaag van de database wijzigen in standaard
  - 3. De database wordt verplaatst naar de groep elastisch

- In Memory OLTP gebruiken met SQL Data Warehouse wordt niet ondersteund.
 - De functie columnstore index van Analytics In het geheugen wordt ondersteund in SQL Data Warehouse.

- Het archief van de Query wordt niet standaard gecompileerde modules binnen query's vastgelegd.

- Sommige Transact-SQL-functies worden niet ondersteund met OLTP In het geheugen. Dit geldt voor zowel Microsoft SQL Server en Azure SQL-Database. Zie voor meer informatie:
 - [Ondersteuning voor In Memory OLTP Transact-SQL](http://msdn.microsoft.com/library/dn133180.aspx)
 - [Transact-SQL-constructies die niet wordt ondersteund door de In Memory OLTP](http://msdn.microsoft.com/library/dn246937.aspx)


## <a name="next-steps"></a>Volgende stappen


- Probeer [Gebruik In Memory OLTP in een bestaande toepassing van Azure SQL.](sql-database-in-memory-oltp-migration.md)


## <a name="additional-resources"></a>Aanvullende bronnen

#### <a name="deeper-information"></a>Meer informatie

- [Meer informatie over de In Memory OLTP geldt voor zowel Microsoft SQL Server en Azure SQL-Database](http://msdn.microsoft.com/library/dn133186.aspx)

- [Meer informatie over Real-Time operationele Analytics op MSDN](http://msdn.microsoft.com/library/dn817827.aspx)

- Witboek over [algemene patronen van de werkbelasting en over migratie](http://msdn.microsoft.com/library/dn673538.aspx), waarin de werkbelasting patronen waar In Memory OLTP vaak aanzienlijke prestatieverhogingen biedt.

#### <a name="application-design"></a>Ontwerp van toepassingen

- [In Memory OLTP (In het geheugen optimalisatie)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Gebruik In Memory OLTP in een bestaande toepassing van Azure SQL.](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Hulpprogramma 's

- [SQL Server Data Tools Preview (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx), voor de meest recente maandelijkse versie.

- [Beschrijving van de Replay Markup Language (RML)-hulpprogramma's voor SQL Server](http://support.microsoft.com/en-us/kb/944837)

- [Monitor geheugenopslag](sql-database-in-memory-oltp-monitoring.md) voor In Memory OLTP.


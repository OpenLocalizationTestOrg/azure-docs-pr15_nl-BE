<properties
   pageTitle="Tabel selecteert (CTAS) in SQL Data Warehouse maken | Microsoft Azure"
   description="Tips voor het coderen met de tabel maken als de select-instructie (CTAS) in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Tabel maken als selecteren (CTAS) in SQL datawarehouse
Selecteer tabel maken of `CTAS` is een van de belangrijkste T-SQL-functies beschikbaar. Het is een volledig parallelized bewerking die u een nieuwe tabel op basis van de uitvoer van een SELECT-instructie maakt. `CTAS`is de snelste en eenvoudigste manier om een kopie maken van een tabel. U kunt deze versie van drukvulling `SELECT..INTO` als u wilt. Dit document bevat voorbeelden en de aanbevolen procedures voor het `CTAS`.

## <a name="using-ctas-to-copy-a-table"></a>Een tabel kopiëren met behulp van CTAS

Misschien een van de meest voorkomende toepassingen van `CTAS` maakt een exemplaar van een tabel zodat u de DDL kunt wijzigen. Als u bijvoorbeeld de tabel als oorspronkelijk gemaakt `ROUND_ROBIN` en nu wilt wijzigen in een tabel in een kolom, verdeeld `CTAS` is hoe u de distributie-kolom wilt wijzigen. `CTAS`kan ook worden gebruikt om te partitioneren, indexering of kolom wijzigen.

Stel dat u deze tabel met het standaardtype voor de distributie van gemaakt `ROUND_ROBIN` verdeeld, omdat er geen verdeling kolom is opgegeven in de `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Nu wilt u een nieuw exemplaar van deze tabel met Columnstore geclusterde indexen maken zodat u van de prestaties van tabellen geclusterd Columnstore profiteren kunt. U ook wilt verdelen in deze tabel op ProductKey omdat u zijn verbindingen in deze kolom anticiperen en verplaatsing van gegevens tijdens de joins in ProductKey te voorkomen. Tot slot wilt u ook op OrderDateKey partitioneren, zodat u snel kunt u oude gegevens verwijderen door slepen en neerzetten oude partities toevoegen. Hier is de instructie CTAS die de oude tabel in een nieuwe tabel wilt kopiëren.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Ten slotte kunt u de tabellen om te wisselen in de nieuwe tabel en zet uw oude tabel wijzigen.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [AZURE.NOTE] Azure SQL Data Warehouse biedt nog geen ondersteuning voor automatische maken of automatische update statistieken.  Om u de beste prestaties van uw query's, is het belangrijk dat statistieken op alle kolommen van alle tabellen worden gemaakt na de eerste keer worden geladen of belangrijke wijzigingen optreden in de gegevens.  Zie het onderwerp van de [Statistieken][] in de groep onderwerpen opstellen voor een gedetailleerde uitleg van de statistieken.

## <a name="using-ctas-to-work-around-unsupported-features"></a>Met behulp van CTAS omzeilen niet-ondersteunde functies

`CTAS`kan ook worden gebruikt om een aantal van de hieronder vermelde niet-ondersteunde voorzieningen te omzeilen. Dit kan vaak blijken een win/win situatie als niet alleen wordt de code compatibel maar deze wordt vaak sneller uitgevoerd op SQL Data Warehouse. Dit is als gevolg van het ontwerp volledig parallelized. Scenario's waarmee kunnen worden gewerkt rond CTAS zijn:

- SELECTEER... IN
- ANSI-JOINS updates
- ANSI-JOINs op verwijderen
- Instructie samenvoegen

> [AZURE.NOTE] Probeer te denken "CTAS eerste". Als u denkt dat u kunt oplossen door een probleem met `CTAS` die is meestal de beste benadering van deze - zelfs als u meer gegevens als gevolg hiervan worden geschreven.
>

## <a name="selectinto"></a>SELECTEER... IN
Kan `SELECT..INTO` wordt weergegeven in een aantal plaatsen in uw oplossing.

Hieronder volgt een voorbeeld van een `SELECT..INTO` instructie:

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

Converteren naar het bovenstaande `CTAS` nogal rechtlijnig is:

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

> [AZURE.NOTE] CTAS moet momenteel dat een kolom distributie worden opgegeven.  Als u niet opzettelijk probeert te wijzigen, de kolom distributie de `CTAS` de snelste wordt uitgevoerd als u een kolom distributie die hetzelfde is als de onderliggende tabel als deze strategie verplaatsing van gegevens voorkomt.  Als u een kleine tabel waar prestaties niet een factor is, dan kunt u `ROUND_ROBIN` niet hoeft te beslissen over een kolom voor distributie.

## <a name="ansi-join-replacement-for-update-statements"></a>ANSI join vervanging voor update-instructies

Wellicht hebt u een complexe update die meer dan twee tabellen gekoppeld maken met behulp van ANSI-syntaxis samenvoegen uit te voeren, bijwerken of verwijderen.

Stel, dat u had deze tabel bij te werken:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]                  SMALLINT        NOT NULL
,   [TotalSalesAmount]              MONEY           NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

De oorspronkelijke query misschien iets dergelijks hebben bekeken:

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Aangezien SQL Data Warehouse biedt geen ondersteuning voor ANSI joins in de `FROM` -component van een `UPDATE` -instructie, u niet deze code via kopiëren zonder deze te wijzigen iets.

U kunt een combinatie van een `CTAS` en een impliciete join vervangen deze code:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI join vervanging voor delete-instructie
Soms de beste manier voor het verwijderen van gegevens is gebruik van `CTAS`. Selecteer de gegevens die u wilt behouden in plaats van de gegevens te verwijderen. Dit met name het geval voor `DELETE` instructies voor het gebruik van ansi-syntaxis van gekoppelde aangezien SQL Data Warehouse biedt geen ondersteuning voor ANSI joins in de `FROM` -component van een `DELETE` instructie.

Hieronder vindt u een voorbeeld van een geconverteerde DELETE-instructie:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Instructies samenvoegen vervangen
Samenvoegen-instructies kunnen worden vervangen, ten minste gedeeltelijk met behulp van `CTAS`. U kunt consolideren de `INSERT` en de `UPDATE` in een enkele instructie. Verwijderde records moet worden afgesloten in een tweede instructie.

Een voorbeeld van een `UPSERT` hieronder:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>CTAS aanbeveling: staat expliciet gegevenstype en de optie voor nulwaarden in uitvoer

Bij het migreren van code wellicht hebt dat u via dit soort patroon code uitvoeren:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Instinctief denkt u deze code moet migreren naar een CTAS en u zou juist zijn. Er is echter een verborgen probleem hier.

De volgende code is niet hetzelfde resultaat opleveren:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Zoals u ziet, wordt de kolom "resultaat" vooruit het type en de optie voor nulwaarden waarden van de expressie. Dit kan leiden tot subtiele verschillen in waarden als u niet zorgvuldig.

Probeer het volgende als voorbeeld:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

De waarde voor het resultaat opgeslagen verschilt. Als u de blijvende waarde in de resultaatkolom wordt gebruikt in andere expressies wordt de fout nog aanzienlijk.

![][1]

Dit is vooral belangrijk voor de migratie van gegevens. Hoewel de tweede query Hoewel nauwkeuriger is is er een probleem. De gegevens verschillend zouden zijn ten opzichte van het bronsysteem en die leidt tot vragen van de integriteit van de migratie. Dit is een van die zeldzame gevallen waar het antwoord "verkeerde" eigenlijk het juiste is!

De reden dat we deze verschillen tussen de twee resultaten Zie is tot impliciete type casting. In het eerste voorbeeld wordt de tabel de kolomdefinitie. Een impliciete typeomzetting vindt plaats wanneer de rij wordt ingevoegd. In het tweede voorbeeld bestaat geen impliciete conversie van de expressie bepaalt het gegevenstype van de kolom. U ziet ook dat de kolom in het tweede voorbeeld is gedefinieerd als een kolom dat in het eerste voorbeeld het niet is. Wanneer de tabel is gemaakt met de optie Voorbeeld voor eerste kolom nulwaarden is expliciet gedefinieerd. In het tweede resulteert voorbeeld die is zojuist werd op de expressie en standaard dit in een NULL-definitie.  

Deze problemen op te lossen moet u expliciet instellen het typeconversie en de optie voor nulwaarden in de `SELECT` gedeelte van de `CTAS` instructie. U kunt deze eigenschappen niet instellen in de tabel maken.

In het volgende voorbeeld wordt getoond hoe de code vast:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Let op het volgende:
- CAST of CONVERT kan worden gebruikt
- ISNULL wordt gebruikt voor de optie voor nulwaarden niet COALESCE forceren
- ISNULL is de buitenste functie
- Het tweede deel van de ISNULL een constante is dus 0

> [AZURE.NOTE] Voor de optie voor nulwaarden worden goed ingesteld is essentieel voor het gebruik van `ISNULL` en niet `COALESCE`. `COALESCE`is niet een deterministische functie en dus het resultaat van de expressie is null-waarden bevatten. `ISNULL`is anders. Deterministisch is. Dus wanneer het tweede deel van de `ISNULL` functie is een constante of een letterlijke waarde is de resulterende waarde is NOT NULL.

Deze tip is niet alleen nuttig voor de waarborging van de integriteit van uw berekeningen. Het is ook belangrijk voor het schakelen tussen tabel partitie. Stel, dat u hebt deze tabel gedefinieerd als het feit:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Het waardeveld is echter een berekende expressie geen deel uitmaakt van de brongegevens.

De gepartitioneerde gegevensset die u kunt maken:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

De query wordt uitgevoerd geen bezwaar. Het probleem komt wanneer u probeert uit te voeren van de partitie-switch. De definities komen niet overeen. Als u wilt dat de tabeldefinities die overeenkomen met de CTAS moet worden gewijzigd.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

U ziet dus dat type consistentie en beheren van de eigenschappen van de optie voor nulwaarden op een CTAS is een goede technische praktijkgewoonten beste. Het helpt te zorgen voor de integriteit in de berekeningen en zorgt er tevens voor dat partitie schakelen mogelijk is.

Zie MSDN voor meer informatie over het gebruik van [CTAS][]. Het is een van de belangrijkste instructies in Azure SQL Data Warehouse. Zorg ervoor dat grondig begrijpen.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->

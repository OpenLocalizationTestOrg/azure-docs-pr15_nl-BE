<properties
   pageTitle="Groep met opties in SQL Data Warehouse | Microsoft Azure"
   description="Tips voor uitvoering van de groep met opties in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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

# <a name="group-by-options-in-sql-data-warehouse"></a>Groeperen op opties in het datawarehouse SQL

[GROUP BY][] -component wordt gebruikt om statistische gegevens aan een samenvatting set rijen. Het heeft ook een aantal opties die de functionaliteit die moeten worden gewerkt uitbreiden rond als ze niet direct worden ondersteund door Azure SQL Data Warehouse.

Deze opties zijn
- GROUP BY met UPDATEPAKKET
- GROEPEREN VAN SETS
- GROEPEREN op met kubus

## <a name="rollup-and-grouping-sets-options"></a>Rollup en groeperen worden opties ingesteld.
Hier de eenvoudigste optie is het gebruik `UNION ALL` in plaats daarvan voor het uitvoeren van het updatepakket en niet vertrouwen op de expliciete syntaxis. Het resultaat is precies hetzelfde

Hieronder wordt een voorbeeld van een groep met behulp van de instructie de `ROLLUP` optie:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

We hebben de volgende aggregaties aangevraagd via UPDATEPAKKET:
- Land en regio
- Land
- Eindtotaal

Voor vervanging van deze moet gebruiken `UNION ALL`; expliciet aan dezelfde resultaten geven de aggregaties vereist:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Voor groepering SETS moeten we alle nemen dezelfde belangrijkste maar alleen Unie alle secties voor de aggregatie van niveaus die we willen zien maken

## <a name="cube-options"></a>Kubusopties
Het is mogelijk een groep door met kubus maken met behulp van de methode UNION ALL. Het probleem is dat de code snel omslachtig en onhandig worden kan. Om dit te verminderen kunt u deze meer geavanceerde benadering.

We gebruiken het voorbeeld hierboven.

De eerste stap is het definiëren van de 'kubus' waarin de niveaus van aggregatie die we willen maken. Het is belangrijk te nemen van de CROSS JOIN van de twee afgeleide tabellen. Dit genereert alle niveaus voor ons. De rest van de code is er echt voor de opmaak.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

Hieronder ziet u de resultaten van de CTAS:

![][1]

De tweede stap is het opgeven van een doeltabel tussentijdse resultaten opslaan:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

De derde stap is het doorlopen van onze kubus van kolommen de samenvoeging uitvoeren. De query wordt eenmaal uitgevoerd voor elke rij in de tijdelijke tabel #Cube en de resultaten worden opgeslagen in de tijdelijke tabel #Results

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Ten slotte geven we de resultaten door te lezen uit de tijdelijke tabel #Results

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

De code wordt door de code van verdelen in secties en het genereren van een herhaling constructie beter worden beheerd en materiaal.


## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GROEPEREN OP]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->

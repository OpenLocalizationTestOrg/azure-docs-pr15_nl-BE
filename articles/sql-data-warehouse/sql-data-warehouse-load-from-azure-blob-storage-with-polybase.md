<properties
   pageTitle="Gegevens laden van Azure blob-opslag in SQL Data Warehouse (PolyBase) | Microsoft Azure"
   description="Informatie over het gebruik van PolyBase om gegevens te laden uit de Azure blobopslag in SQL Data Warehouse. Enkele tabellen uit openbare gegevens in het schema van Contoso Retail Data Warehouse worden geladen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a>Gegevens laden van Azure blob-opslag in SQL Data Warehouse (PolyBase)

> [AZURE.SELECTOR]
- [Data Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

PolyBase en T-SQL-opdrachten gebruiken om gegevens te laden uit de Azure blobopslag in Azure SQL Data Warehouse. 

Het gemak houden, laadt deze zelfstudie twee tabellen uit een openbare Azure opslag Blob in het schema Contoso Retail Data Warehouse. Voor het laden van de volledige gegevensset in het voorbeeld [de volledige Contoso Retail Data Warehouse worden geladen][] uit de opslagplaats monsters van Microsoft SQL Server worden uitgevoerd.

In deze zelfstudie wordt u:

1. PolyBase laden uit Azure blob-opslag configureren
2. Openbare gegevens in de database laden
3. Optimalisaties uitvoeren nadat het laden is voltooid.


## <a name="before-you-begin"></a>Voordat u begint
In deze zelfstudie wordt uitgevoerd, moet u een Azure account die al een SQL Data Warehouse-database. Als u niet reeds hebt, Zie [maken een datawarehouse SQL][].

## <a name="1-configure-the-data-source"></a>1. de gegevensbron configureren

Externe objecten T-SQL PolyBase gebruikt voor het definiëren van de locatie en de kenmerken van de externe gegevens. De definities extern object zijn opgeslagen in SQL Data Warehouse. De gegevens zelf zijn extern opgeslagen.

### <a name="11-create-a-credential"></a>1.1. Maak een referentie

**Deze stap overslaan** als u de Contoso openbare gegevens laadt. Hoeft u geen beveiligde toegang tot openbare gegevens omdat het al toegankelijk voor iedereen is.

**Sla deze stap niet over** als u deze zelfstudie als een sjabloon voor het laden van uw eigen gegevens. Het volgende script gebruiken voor het maken van een referentie binnen het bereik van database voor toegang tot gegevens via een referentie, en vervolgens gebruiken bij het definiëren van de locatie van de gegevensbron.


```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);
```

Ga verder met stap 2.

### <a name="12-create-the-external-data-source"></a>1.2. De externe gegevensbron maken

Gebruik deze opdracht [Externe gegevensbron maken][] voor het opslaan van de locatie van de gegevens en het type gegevens. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [AZURE.IMPORTANT] Als u uw azure blob storage containers openbaar maken, houd er rekening mee dat als de eigenaar van de gegevens wordt aangerekend voor gegevens egress toeslagen wanneer gegevens naar het datacenter. 

## <a name="2-configure-data-format"></a>2. gegevensindeling configureren

De gegevens worden opgeslagen in tekstbestanden in Azure blob-opslag en elk veld wordt gescheiden met een scheidingsteken. Deze [Externe BESTANDSINDELING maken][] opdracht geeft u de indeling van de gegevens in de tekstbestanden worden uitgevoerd. De gegevens voor Contoso wordt gedecomprimeerd en pijp gescheiden.

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,   FORMAT_OPTIONS  (   FIELD_TERMINATOR = '|'
                    ,   STRING_DELIMITER = ''
                    ,   DATE_FORMAT      = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,   USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-the-external-tables"></a>3. de externe tabellen maken

Nu u de opmaak van bron- en gegevens hebt opgegeven, bent u gereed om de externe tabellen te maken. 

### <a name="31-create-a-schema-for-the-data"></a>3.1. Maak een schema voor de gegevens. 

Om een plaats voor de Contoso-gegevens opslaan in een database maakt, moet u een schema maken.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-the-external-tables"></a>3.2. De externe tabellen maken. 

Voer dit script om de externe tabellen DimProduct en FactOnlineSales te maken. Alle wij hier doen is kolomnamen en de gegevenstypen definiëren en deze te binden aan de locatie en de indeling van de bestanden Azure blob-opslag. De definitie is opgeslagen in SQL Data Warehouse en de gegevens nog steeds in de opslag Azure Blob.

De **locatie** is de map onder de hoofdmap van de opslagruimte Azure Blob. Elke tabel wordt in een andere map.


```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
 
--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-the-data"></a>4. de gegevens worden geladen
Er is toegang tot externe gegevens op verschillende manieren.  U kunt gegevens rechtstreeks van de externe tabel opvragen, de gegevens in de nieuwe databasetabellen laden of externe gegevens toevoegen aan bestaande databasetabellen.  


### <a name="41-create-a-new-schema"></a>4.1. Een nieuw schema maken

CTAS maakt een nieuwe tabel die gegevens bevat.  Maak eerst een schema voor de contoso-gegevens.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-the-data-into-new-tables"></a>4.2. De gegevens worden geladen in nieuwe tabellen

Gegevens laden van Azure blob-opslag en opslaan in een tabel in uw database, gebruikt u de instructie [CREATE tabel AS SELECT (Transact-SQL)][] . Laden met CTAS maakt gebruik van de sterk getypeerde externe tabellen die u zojuist hebt gemaakt. Gebruik het om gegevens te laden in nieuwe tabellen, één [CTAS][] overzicht per tabel. 

CTAS wordt een nieuwe tabel gemaakt en gevuld met de resultaten van een select-instructie. CTAS definieert dezelfde kolommen en gegevenstypen hebben als de resultaten van de select-instructie in de nieuwe tabel. Als u alle kolommen in een externe tabel selecteert, worden de nieuwe tabel een replica van de kolommen en gegevenstypen in de externe tabel.

In dit voorbeeld maken we zowel de dimensie en de feitentabel als hash-tabellen gedistribueerd. 


```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-the-load-progress"></a>4.3 de voortgang van het laden volgen

U kunt de voortgang van het laden met behulp van beheer van dynamische weergaven (DMVs) volgen. 

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r;
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a>5. columnstore compressie optimaliseren

SQL Data Warehouse slaat standaard de tabel als een columnstore voor geclusterde index. Na voltooiing van een belasting zijn van de rijen met gegevens niet gecomprimeerd in de columnstore.  Er is een aantal redenen waarom dit kan gebeuren. Voor meer informatie, Zie [columnstore indexen beheren][].

Opnieuw optimaliseren prestaties van query's en columnstore compressie na belasting, ervoor zorgen dat de columnstore-index voor het comprimeren van alle rijen in de tabel. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Zie het artikel [columnstore indexen beheren][] voor meer informatie over het onderhouden van columnstore-indexen.

## <a name="6-optimize-statistics"></a>6. statistische gegevens optimaliseren

Het is raadzaam één kolom om statistieken te maken zodra een belasting. Er zijn enkele mogelijkheden voor statistieken. Bijvoorbeeld, als u één kolom statistieken voor elke kolom maken duurt het een lange tijd de statistische gegevens opnieuw samen te stellen. Als u dat bepaalde kolommen in de query predikaten niet gaat, kunt u de statistieken maken op deze kolommen overslaan.

Als u één kolom statistieken maken voor elke kolom van elke tabel, kunt u in het voorbeeld van de opgeslagen procedure `prc_sqldw_create_stats` in het artikel [statistiek][] .

In het volgende voorbeeld is een goed uitgangspunt voor het maken van statistieken. Statistieken voor één kolom wordt gemaakt van elke kolom in de dimensietabel en op elke gekoppelde kolom in de feitentabellen. U kunt altijd één of meerdere kolommen statistieken naar andere kolommen in de tabel feit later toevoegen.


```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a>Bereiken ontgrendeld!

Openbare gegevens is in Azure SQL Data Warehouse geladen. Fantastisch baan!

U kunt nu een query de tabellen met behulp van query's als volgt starten:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Volgende stappen
De volledige Data Warehouse van Contoso Retail om gegevens te laden, gebruikt u het script in voor meer tips voor ontwikkeling, Zie [SQL Data Warehouse ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[Maken van een SQL-datawarehouse]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL Data Warehouse ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md
[columnstore-indexen beheren]: sql-data-warehouse-tables-index.md
[Statistieken]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[EXTERNE GEGEVENSBRON MAKEN]: https://msdn.microsoft.com/en-us/library/dn935022.aspx
[EXTERNE BESTANDSINDELING MAKEN]: https://msdn.microsoft.com/en-us/library/dn935026.aspx
[Maak de tabel AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[De volledige Contoso Retail Data Warehouse worden geladen]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md

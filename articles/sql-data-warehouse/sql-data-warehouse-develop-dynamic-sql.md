<properties
   pageTitle="Dynamische SQL in SQL datawarehouse | Microsoft Azure"
   description="Tips voor het gebruik van dynamische SQL in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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

# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dynamische SQL in SQL datawarehouse
Bij het ontwikkelen van de toepassingscode voor SQL Data Warehouse wellicht u dynamische sql om flexibele, algemene en modulaire oplossingen te gebruiken. SQL Data Warehouse ondersteund blob-gegevenstypen op dit moment niet. Dit kan de grootte van de tekenreeksen als blob typen, varchar(max)-en nvarchar(max) zijn. Als u deze typen in uw toepassingscode gebruikt hebt bij het samenstellen van tekenreeksen met zeer grote, moet u de code op te splitsen in segmenten en de EXEC-instructie te gebruiken.

Een eenvoudig voorbeeld:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Als de tekenreeks kort is kunt u [sp_executesql][] als normaal.

> [AZURE.NOTE] Instructies die worden uitgevoerd als dynamische SQL worden nog steeds alle TSQL validatieregels.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->

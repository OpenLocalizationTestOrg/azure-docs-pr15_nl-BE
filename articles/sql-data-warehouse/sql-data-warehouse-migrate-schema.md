<properties
   pageTitle="Uw schema migreren naar SQL Data Warehouse | Microsoft Azure"
   description="Tips voor het migreren van uw schema naar Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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
   ms.date="08/25/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="migrate-your-schema-to-sql-data-warehouse"></a>Uw schema migreren naar SQL Data Warehouse#

De volgende overzichten kunnen u het verschil tussen SQL Server en SQL Data Warehouse voor het migreren van uw database.

## <a name="table-migration"></a>Migratie van tabel

Bij het migreren van uw tabellen, zult u vertrouwd zijn met de tabelfuncties van het datawarehouse SQL-tabellen.  De [tabel overzicht][] is een prima plek om te starten.  Dit artikel maakt u kennis met de belangrijkste aandachtspunten bij het maken van een tabel zoals tabel statistiek, distributie, partitioneren en te indexeren.  Omvat tevens enkele [met niet-ondersteunde tabelfuncties][] en oplossingen.

SQL Data Warehouse ondersteunt de algemene zakelijke gegevenstypen.  Zie het artikel [beschikbare gegevenstypen][] voor een lijst met ondersteunde en [niet-ondersteunde gegevenstypen][].  De [gegevens van het type][] artikel bevat ook een query voor [niet-ondersteunde gegevenstypen][].  Bij het converteren van de gegevenstypen, moet u Bekijk de [Aanbevolen procedures voor gegevenstype][].

## <a name="next-steps"></a>Volgende stappen
Zodra u het schema van de database is gemigreerd naar SQL Data Warehouse, gaat u verder met de volgende artikelen:

- [Uw gegevens migreren][]
- [Uw code migreren][]

Zie het artikel met [Aanbevolen procedures][] voor meer informatie over aanbevolen procedures voor SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[Uw code migreren]: ./sql-data-warehouse-migrate-code.md
[Uw gegevens migreren]: ./sql-data-warehouse-migrate-data.md
[aanbevolen procedures]: ./sql-data-warehouse-best-practices.md
[tabel-overzicht]: ./sql-data-warehouse-tables-overview.md
[met niet-ondersteunde tabelfuncties]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[gegevenstypen]: ./sql-data-warehouse-tables-data-types.md
[niet-ondersteunde gegevenstypen]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[aanbevolen procedures voor gegevenstype]: ./sql-data-warehouse-tables-data-types.md#data-type-best-practices

<!--MSDN references-->


<!--Other Web references-->

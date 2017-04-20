<properties
   pageTitle="Ontwerp-besluiten en coding technieken voor de ontwikkeling van SQL Data Warehouse | Microsoft Azure"
   description="Concepten van ontwikkeling, ontwerpbesluiten, aanbevelingen en coding technieken voor SQL Data Warehouse."
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
   ms.date="08/16/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Ontwerp en code technieken voor SQL Data Warehouse

Kijk via deze ontwikkeling artikelen voor een beter begrip van belangrijke besluiten, aanbevelingen en technieken van codering voor SQL Data Warehouse.

## <a name="key-design-decisions"></a>Belangrijke ontwerpbeslissingen
De volgende artikelen van enkele van de belangrijkste concepten en ontwerpbeslissingen die u moet weten voor de ontwikkeling van de gedistribueerde datawarehouse met SQL Data Warehouse markering:

- [verbindingen][]
- [gelijktijdigheid][]
- [transacties][]
- [door de gebruiker gedefinieerde schema 's][]
- [verdeling van de tabel][]
- [tabelindexen][]
- [Tabelpartities][]
- [CTAS][]
- [statistieken][]

## <a name="development-recommendations-and-coding-techniques"></a>Aanbevelingen bij de ontwikkeling en het coderingsschema technieken
Deze artikelen markeren specifieke code technieken, tips en aanbevelingen voor het ontwikkelen van het datawarehouse SQL:

- [opgeslagen procedures][]
- [labels][]
- [Weergaven][]
- [tijdelijke tabellen][]
- [dynamische SQL][]
- [herhalen][]
- [groep met opties][]
- [toewijzing van variabele][]

## <a name="next-steps"></a>Volgende stappen
Als u via de ontwikkeling artikelen zijn kijk via de pagina [Transact-SQL reference][] voor meer informatie over de ondersteunde syntaxis voor SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[gelijktijdigheid]: ./sql-data-warehouse-develop-concurrency.md
[verbindingen]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamische SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[groep met opties]: ./sql-data-warehouse-develop-group-by-options.md
[labels]: ./sql-data-warehouse-develop-label.md
[herhalen]: ./sql-data-warehouse-develop-loops.md
[statistieken]: ./sql-data-warehouse-tables-statistics.md
[opgeslagen procedures]: ./sql-data-warehouse-develop-stored-procedures.md
[verdeling van de tabel]: ./sql-data-warehouse-tables-distribute.md
[tabelindexen]: ./sql-data-warehouse-tables-index.md
[Tabelpartities]: ./sql-data-warehouse-tables-partition.md
[tijdelijke tabellen]: ./sql-data-warehouse-tables-temporary.md
[transacties]: ./sql-data-warehouse-develop-transactions.md
[door de gebruiker gedefinieerde schema 's]: ./sql-data-warehouse-develop-user-defined-schemas.md
[toewijzing van variabele]: ./sql-data-warehouse-develop-variable-assignment.md
[Weergaven]: ./sql-data-warehouse-develop-views.md
[Transact-SQL reference]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->

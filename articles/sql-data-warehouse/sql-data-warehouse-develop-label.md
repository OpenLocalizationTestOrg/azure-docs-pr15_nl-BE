<properties
   pageTitle="Labels voor instrument van query's in SQL Data Warehouse gebruiken | Microsoft Azure"
   description="Tips voor het gebruik van labels aan query's instrument in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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

# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>Labels voor instrument van query's in SQL Data Warehouse gebruiken
Datawarehouse SQL ondersteunt query labels zogenoemde. Voordat u gaat in de diepte gaan we kijken naar een voorbeeld van een:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Deze laatste regel tags de tekenreeks 'My Label' voor de Query op de query. Dit is vooral handig als het label query kunnen via de DMVs is. Dit biedt ons een mechanisme voor het bijhouden van query's probleem en om vast te stellen door een ETL uitvoeren.

Een goede naamgevingsconventie helpt echt hier. Als bijvoorbeeld "PROJECT: PROCEDURE: instructie: Opmerking ' zou helpen als unieke aanduiding voor de query in onder de code in het besturingselement.

U kunt de volgende query die gebruikmaakt van het beheer van dynamische weergaven gebruiken om te zoeken op label:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [AZURE.NOTE] Het is essentieel dat u vierkante haken of dubbele aanhalingstekens rond het label word langs bij het opvragen. Label is een gereserveerd woord en wordt een fout veroorzaakt als het niet is gescheiden.


## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->

<properties
   pageTitle="Beheren van rekenkracht in Azure SQL gegevens magazijn (REST) | Microsoft Azure"
   description="Transact-SQL (T-SQL) taken schalingsprestaties door DWUs aan te passen. Kosten besparen door de schaal terug tijdens niet-piekuren."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a>Beheren van rekenkracht in Azure SQL Data Warehouse (T-SQL)

> [AZURE.SELECTOR]
- [Overzicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Prestaties door schalen schaal berekenen bronnen en het geheugen om te voldoen aan de veranderende behoeften van uw werkbelasting. Kosten besparen door de schaal terug resources tijdens niet-piekuren of onderbreken compute helemaal. 

Deze verzameling taken maakt gebruik van T-SQL aan:

- Instellingen voor de huidige DWU
- Wijziging compute bronnen door DWUs aan te passen

Als u wilt onderbreken of hervatten van een database, een andere platform opties boven aan dit artikel te kiezen.

Voor meer informatie hierover, Zie [beheren energie overzicht berekenen][].

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Instellingen voor de huidige DWU

De huidige instellingen van de DWU voor uw databases bekijken:

1. SQL Server-Object Explorer openen in Visual Studio 2015.
2. Verbinding maken met de master-database die is gekoppeld aan de logische Database van SQL server.
2. Selecteer in de weergave sys.database_service_objectives dynamic management. Hier volgt een voorbeeld: 

```
SELECT
 db.name [Database],
 ds.edition [Edition],
 ds.service_objective [Service Objective]
FROM
 sys.database_service_objectives ds
 JOIN sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Schaal berekenen

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

De DWUs wijzigen:


1. Verbinding maken met de master-database die is gekoppeld aan de logische Database van SQL server.
2. Gebruik de instructie [ALTER DATABASE][] TSQL. In het volgende voorbeeld wordt het doel van service-niveau ingesteld op DW1000 voor de database MySQLDW. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Volgende stappen

Zie [beheer-overzicht][]voor andere beheertaken uitvoeren.

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Beheer-overzicht]: ./sql-data-warehouse-overview-manage.md
[Energiebeheer-overzicht compute beheren]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/

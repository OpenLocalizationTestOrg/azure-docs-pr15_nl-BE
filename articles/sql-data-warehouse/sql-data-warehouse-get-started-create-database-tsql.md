<properties
   pageTitle="Maak een SQL-datawarehouse met TSQL | Microsoft Azure"
   description="Informatie over het maken van een datawarehouse Azure SQL met TSQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>Een datawarehouse SQL-database maken met behulp van Transact-SQL (TSQL)

> [AZURE.SELECTOR]
- [Azure Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

In dit artikel wordt beschreven hoe u voor het maken van een SQL-datawarehouse met T-SQL.

## <a name="prerequisites"></a>Vereisten

U moet om te beginnen: 

- **Azure account**: Bezoek [Azure gratis proefperiode][] of [MSDN Azure verantwoording][] om een account te maken.
- **Azure SQL server**: Zie [maken van een logische server Azure SQL-Database met Azure Portal][] of [maken een logische server Azure SQL-Database met PowerShell][] voor meer informatie.
- **Resourcegroep**: dezelfde resourcegroep gebruiken als de Azure SQL server of [een resourcegroep][]te maken.
- **Omgeving voor het uitvoeren van T-SQL**: [Visual Studio]kunt u[Installing Visual Studio and SSDT], [sqlcmd][]of [SSMS][] T-SQL uitvoeren.

> [AZURE.NOTE] Maken van een SQL-datawarehouse kan resulteren in een nieuwe service te factureren.  Zie [SQL Data Warehouse prijzen][] voor meer informatie over prijzen.

## <a name="create-a-database-with-visual-studio"></a>Een database maken met Visual Studio

Als u bekend met Visual Studio bent, Zie het artikel [Query Azure SQL Data Warehouse (Visual Studio)][].  SQL Server-Object Explorer openen in Visual Studio wilt starten, en maak verbinding met de server die als host voor de SQL Data Warehouse-database fungeert.  Wanneer een verbinding, kunt u een SQL Data Warehouse maken door de volgende SQL-opdracht uitgevoerd op de **master** -database.  Met deze opdracht de MySqlDwDb van de database wordt gemaakt met een doelstelling van de DW400 van de Service en dat de database te laten groeien tot een maximale grootte van 10 TB.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Een database maken met sqlcmd

U kunt ook dezelfde opdracht met sqlcmd uitvoeren door de volgende opdracht bij een opdrachtprompt.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

De standaardsortering als opgegeven is SQL_Latin1_General_CP1_CI_AS gesorteerd.  De `MAXSIZE` van 250 GB tot en 240 TB.  De `SERVICE_OBJECTIVE` kan liggen tussen DW100 en DW2000 [DWU][].  Zie de MSDN-documentatie van de [DATABASE maken][]voor een lijst van alle geldige waarden.  De MAXSIZE en de SERVICE_OBJECTIVE kan worden gewijzigd met een T-SQL-opdracht [ALTER DATABASE][] .  De sortering van een database kan niet worden gewijzigd nadat het is gemaakt.   Voorzichtig moet worden gebruikt bij het wijzigen van de SERVICE_OBJECTIVE als het wijzigen van DWU zorgt ervoor dat het opnieuw opstarten van services, die alle query's in de vlucht wordt geannuleerd.  MAXSIZE wijzigen niet opnieuw wordt opgestart services is een eenvoudige metadata-bewerking.

## <a name="next-steps"></a>Volgende stappen

Zodra uw SQL Data Warehouse ingericht dat u kunt de [voorbeeldgegevens worden geladen][] of uitchecken [ontwikkelen][], [laden][]of [migreren][].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[how to create a SQL Data Warehouse from the Azure portal]: sql-data-warehouse-get-started-provision.md
[Query Azure SQL datawarehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migreren]: sql-data-warehouse-overview-migrate.md
[ontwikkelen]: sql-data-warehouse-overview-develop.md
[laden]: sql-data-warehouse-overview-load.md
[voorbeeldgegevens worden geladen]: sql-data-warehouse-load-sample-databases.md
[Een logische server Azure SQL-Database maken met de Portal Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Een logische server Azure SQL-Database maken met PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[het maken van een resourcegroep]: ../resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[Sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references--> 
[DATABASE MAKEN]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse prijzen]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure gratis proefversie]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

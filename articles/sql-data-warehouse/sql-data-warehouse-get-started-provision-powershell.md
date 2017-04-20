<properties
   pageTitle="SQL Data Warehouse maken met PowerShell | Microsoft Azure"
   description="SQL Data Warehouse met PowerShell maken"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-sql-data-warehouse-using-powershell"></a>SQL Data Warehouse met PowerShell maken

> [AZURE.SELECTOR]
- [Azure Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

In dit artikel wordt beschreven hoe u voor het maken van een SQL-datawarehouse met PowerShell.

## <a name="prerequisites"></a>Vereisten

U moet om te beginnen:

- **Azure account**: Bezoek [Azure gratis proefperiode][] of [MSDN Azure verantwoording][] om een account te maken.
- **Azure SQL server**: Zie [maken van een logische server Azure SQL-Database met Azure Portal][] of [maken een logische server Azure SQL-Database met PowerShell][] voor meer informatie.
- **Resourcegroep**: dezelfde resourcegroep gebruiken als de Azure SQL server of [een resourcegroep][]te maken.
- **PowerShell versie 1.0.3 groter of**: U kunt uw versie controleren door te voeren **Get Module - ListAvailable-naam Azure**.  De meest recente versie kan worden geïnstalleerd vanuit [Microsoft Web Platform Installer][].  Zie voor meer informatie over het installeren van de nieuwste versie [installeren en configureren van Azure PowerShell][].

> [AZURE.NOTE] Maken van een SQL-datawarehouse kan resulteren in een nieuwe service te factureren.  Zie [SQL Data Warehouse prijzen][] voor meer informatie over prijzen.

## <a name="create-a-sql-data-warehouse"></a>Maken van een SQL-datawarehouse

1. Open Windows PowerShell.
2. Deze cmdlet aanmelding Azure Resource Manager worden uitgevoerd.

    ```Powershell
    Login-AzureRmAccount
    ```
    
3. Selecteer het abonnement dat u wilt gebruiken voor de huidige sessie.

    ```Powershell
    Get-AzureRmSubscription -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```

4.  Database maken. In dit voorbeeld wordt een database met de naam 'mynewsqldw' met objectieve serviceniveau 'DW400', naar de server met de naam 'sqldwserver1', in de bronnengroep met de naam 'mywesteuroperesgp1' wordt gemaakt.

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
    ```

Vereiste Parameters zijn:

- **RequestedServiceObjectiveName**: het bedrag van de [DWU][] die u aanvraagt.  Ondersteunde waarden zijn: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 en DW6000.
- **Databasenaam**: de naam van het datawarehouse SQL die u maakt.
- **Servernaam**: de naam van de server die u voor het maken van gebruikt (V12 moet zijn).
- **ResourceGroupName**: resourcegroep die u gebruikt.  Groepen in uw abonnement gebruiken om te zoeken naar beschikbare bron Get-AzureResource.
- **Editie**: 'DataWarehouse' moet zijn voor het maken van een SQL-datawarehouse.

Optionele Parameters zijn:

- **CollationName**: de standaardsortering als niet wordt opgegeven, is SQL_Latin1_General_CP1_CI_AS.  Sortering kan niet worden gewijzigd in een database.
- **MaxSizeBytes**: de standaard maximale grootte van een database is 10 GB.


Zie voor meer informatie over parameteropties voor de [New AzureRmSqlDatabase][] en [Create Database (SQL-datawarehouse Azure)][].

## <a name="next-steps"></a>Volgende stappen

Nadat uw SQL Data Warehouse inrichten is voltooid kunt u [Voorbeeldgegevens][] laad of te [ontwikkelen][], te [laden][]of te [migreren][].

Als u geïnteresseerd in meer informatie over het programmatisch beheren van SQL Data Warehouse bent, kijk dan eens ons artikel over het gebruik van [PowerShell-cmdlets en REST API's][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[migreren]: ./sql-data-warehouse-overview-migrate.md
[ontwikkelen]: ./sql-data-warehouse-overview-develop.md
[laden]: ./sql-data-warehouse-load-with-bcp.md
[voorbeeldgegevens worden geladen]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell-cmdlets en REST API 's]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[Het installeren en configureren van Azure PowerShell]: ../powershell/powershell-install-configure.md
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Een logische server Azure SQL-Database maken met de Portal Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Een logische server Azure SQL-Database maken met PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[het maken van een resourcegroep]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[Nieuwe AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Database (SQL Azure datawarehouse) maken]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[SQL Data Warehouse prijzen]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure gratis proefversie]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

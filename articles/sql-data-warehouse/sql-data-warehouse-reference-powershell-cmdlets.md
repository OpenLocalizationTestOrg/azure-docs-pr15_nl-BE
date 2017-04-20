<properties
   pageTitle="PowerShell-cmdlets voor Azure SQL Data Warehouse"
   description="De bovenste PowerShell-cmdlets voor Azure SQL Data Warehouse, met inbegrip van het onderbreken en hervatten van een database niet vinden."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess;mausher"/>

# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShell-cmdlets en REST API's voor SQL Data Warehouse

Veel SQL Data Warehouse beheertaken kunnen worden beheerd via Azure PowerShell-cmdlets of REST API's.  Hieronder volgen enkele voorbeelden van PowerShell-opdrachten gebruiken voor het automatiseren van algemene taken in uw SQL Data Warehouse.  Zie voor voorbeelden REST goede [schaalbaarheid beheren met de REST][]van het artikel.

> [AZURE.NOTE]  Azure PowerShell SQL Data Warehouse gebruiken, moet u Azure PowerShell versie 1.0.3 of hoger.  U kunt uw versie controleren door het uitvoeren van **Get Module - ListAvailable-naam Azure**.  De meest recente versie kan worden geïnstalleerd vanuit [Microsoft Web Platform Installer][].  Zie voor meer informatie over het installeren van de nieuwste versie [installeren en configureren van Azure PowerShell][].

## <a name="get-started-with-azure-powershell-cmdlets"></a>Aan de slag met Azure PowerShell-cmdlets

1. Open Windows PowerShell. 
2. Bij de PowerShell-prompt uitvoeren van deze opdrachten te melden aan de bronnenbeheerder van Azure en selecteer uw abonnement.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Voorbeeld van onderbreken SQL gegevens magazijn

Onderbreken van een database met de naam "Database02" is ondergebracht op een server met de naam "Server01."  De server is in een groep Azure resource met de naam 'ResourceGroup1'. 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Een variatie op, in dit voorbeeld buizen het [Stand-by AzureRmSqlDatabase][]-object opgehaald.  Hierdoor wordt de database is onderbroken. De laatste opdracht toont de resultaten.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Start SQL gegevens magazijn-voorbeeld

Hervatten van een database met de naam "Database02" is ondergebracht op een server met de naam "Server01." De server deel uitmaakt van een groep met de naam 'ResourceGroup1'.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Een variatie daarop, in het volgende voorbeeld haalt een database met de naam 'Database02' van een server met de naam 'Server01", die deel uitmaakt van een groep met de naam 'ResourceGroup1'. Deze buizen het [Cv-AzureRmSqlDatabase][]-object opgehaald.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [AZURE.NOTE] Overigens als uw server is foo.database.windows.net, "foo" als - servernaam in de PowerShell-cmdlets gebruiken.

## <a name="frequently-used-powershell-cmdlets"></a>Vaak gebruikte PowerShell-cmdlets

Deze PowerShell-cmdlets worden vaak gebruikt met Azure SQL Data Warehouse.

- [Get-AzureRmSqlDatabase][]
- [Get-AzureRmSqlDeletedDatabaseBackup][]
- [Get-AzureRmSqlDatabaseRestorePoints][]
- [Nieuwe AzureRmSqlDatabase][]
- [Verwijderen AzureRmSqlDatabase][]
- [Herstel AzureRmSqlDatabase][] 
- [Cv-AzureRmSqlDatabase][]
- [Selecteer AzureRmSubscription][]
- [Set AzureRmSqlDatabase][]
- [Stand-by AzureRmSqlDatabase][]

## <a name="next-steps"></a>Volgende stappen
Zie voor meer voorbeelden van PowerShell:

- [Maken van een SQL-datawarehouse met PowerShell][]
- [Database terugzetten][]

Zie voor een lijst van alle taken die kunnen worden geautomatiseerd met PowerShell [-Cmdlets Azure SQL Database][].  Zie voor een lijst met taken die kunnen worden geautomatiseerd met REST, [bewerkingen voor Azure SQL-Databases][].

<!--Image references-->

<!--Article references-->
[Het installeren en configureren van Azure PowerShell]: ./powershell-install-configure.md
[Maken van een SQL-datawarehouse met PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Database terugzetten]: ./sql-data-warehouse-restore-database-powershell.md
[Schaalbaarheid met REST beheren]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database-Cmdlets]: https://msdn.microsoft.com/library/mt574084.aspx
[Bewerkingen voor Azure SQL-Databases]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[Nieuwe AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Verwijderen AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Herstel AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Cv-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Selecteer AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Stand-by AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps

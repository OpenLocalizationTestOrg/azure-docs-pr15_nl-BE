<properties
    pageTitle="Maak een nieuwe database elastische toepassingen met PowerShell | Microsoft Azure"
    description="Informatie over het gebruik van PowerShell naar Azure SQL-Database geschaald uitbreiden bronnen op een schaalbare elastische database-toepassingen voor het beheren van meerdere databases maken."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="create-a-new-elastic-database-pool-with-powershell"></a>Maak een nieuwe database elastische toepassingen met PowerShell

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Informatie over het maken van een [groep met elastische database](sql-database-elastic-pool.md) met PowerShell-cmdlets. 

Zie voor algemene foutcodes, [foutcodes van de SQL voor SQL-Database-clienttoepassingen: verbindingsfout en andere problemen](sql-database-develop-error-messages.md).

> [AZURE.NOTE] Elastische toepassingen zijn algemeen beschikbaar (GA) in alle gebieden van Azure, met uitzondering van North Central ons en West India waar deze momenteel voorbeeld wordt.  GA van elastische toepassingen in deze gebieden worden zo spoedig mogelijk geleverd. Ook ondersteunen groepen van elastische momenteel geen databases, [OLTP in het geheugen](sql-database-in-memory.md)of in het geheugen analytics.


U moet de Azure PowerShell 1.0 of hoger worden uitgevoerd. Zie voor gedetailleerde informatie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

## <a name="create-a-new-pool"></a>Een nieuwe groep maken

De cmdlet [New-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt619378.aspx) maakt een nieuwe groep. De waarden voor eDTU per groep van toepassingen, min en max Dtus worden beperkt door de service tier-waarde (basic, standard of premium). Zie [eDTU en opslag worden beperkt voor elastische pools en elastische databases](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

    New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


## <a name="create-a-new-elastic-database-in-a-pool"></a>Een nieuwe elastische database maken in een groep

Gebruik de cmdlet [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) en de parameter **ElasticPoolName** ingesteld op het doel van toepassingen. Als u een bestaande database verplaatsen naar een groep, Zie [een database in een elastische toepassingen verplaatsen](sql-database-elastic-pool-manage-powershell.md#Move-a-database-into-an-elastic-pool).

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="create-a-pool-and-populate-it-with-multiple-new-databases"></a>Een groep maken en deze vult met meerdere nieuwe databases 

Het maken van een groot aantal databases in een groep kan duren wanneer gedaan via de portal of PowerShell-cmdlets die slechts één database tegelijk maken. Automatiseren van het maken in een nieuwe groep, Zie [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).   

## <a name="example-create-a-pool-using-powershell"></a>Voorbeeld: een groep met PowerShell maken 

Dit script maakt een nieuwe resourcegroep Azure en een nieuwe server. Desgevraagd verstrekt een beheerder gebruikersnaam en wachtwoord voor de nieuwe server (niet de Azure referenties).

    $subscriptionId = '<your Azure subscription id>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $serverName = '<server name>'
    $poolName = '<pool name>'
    $databaseName = '<database name>'

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

    New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

    New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB



## <a name="next-steps"></a>Volgende stappen

- [Uw groep beheren](sql-database-elastic-pool-manage-powershell.md)
- [Elastische taken maken](sql-database-elastic-jobs-overview.md) Elastische taken kunnen u T-SQL-scripts uitvoeren op een onbeperkt aantal databases in de groep.
- [Geschaald uitbreiden met Azure SQL-Database](sql-database-elastic-scale-introduction.md): elastische databasehulpmiddelen om schalen te gebruiken.


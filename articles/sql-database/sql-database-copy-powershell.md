<properties 
    pageTitle="Met PowerShell Azure SQL-database kopiëren | Microsoft Azure" 
    description="Kopie van een met PowerShell Azure SQL-database maken" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/08/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-powershell"></a>Met PowerShell Azure SQL-database kopiëren


> [AZURE.SELECTOR]
- [Overzicht](sql-database-copy.md)
- [Azure portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

In dit artikel ziet u hoe een SQL-database met PowerShell kopiëren naar dezelfde server naar een andere server of een database in een [groep met elastische database](sql-database-elastic-pool.md)kopiëren. De kopieerbewerking database wordt gebruikt voor de cmdlet [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/mt603644.aspx) . 


Als u dit artikel wilt nodig u het volgende:

- Azure SQL-database (database kopiëren). Als u niet een SQL-database, maken een volgens de stappen in dit artikel: [de eerste Azure SQL-Database maken](sql-database-get-started.md).
- De meest recente versie van Azure PowerShell. Zie voor gedetailleerde informatie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).


Veel nieuwe functies van SQL-Database worden alleen ondersteund wanneer u het [implementatiemodel Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)zodat voorbeelden de [Azure SQL Database PowerShell-cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) voor Resource Manager gebruiken. De bestaande klassieke implementatiemodel [Azure SQL-Database (klassiek)-cmdlets](https://msdn.microsoft.com/library/azure/dn546723.aspx) worden ondersteund voor compatibiliteit met eerdere versies, maar wij adviseren dat u de Resource Manager-cmdlets gebruiken.


>[AZURE.NOTE] De kopieerbewerking kan enige tijd duren, afhankelijk van de grootte van de database.


## <a name="copy-a-sql-database-to-the-same-server"></a>Een SQL-database op dezelfde server kopiëren

Weglaten om de kopie op de server maakt, de `-CopyServerName` parameter (of deze instelt op dezelfde server).

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyDatabaseName "database1_copy"

## <a name="copy-a-sql-database-to-a-different-server"></a>Een SQL-database naar een andere server kopiëren

Opnemen om de kopie te maken op een andere server, de `-CopyServerName` parameter en wordt ingesteld op een andere server. De server *kopiëren* moet al bestaan. Als deze in een andere bronnengroep bevinden, dan moet u ook de `-CopyResourceGroupName` parameter.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyServerName "server2" -CopyDatabaseName "database1_copy"


## <a name="copy-a-sql-database-into-an-elastic-database-pool"></a>Een SQL-database kopiëren naar een elastische database toepassingen

Als u een kopie van een SQL-database in een groep, stelt de `-ElasticPoolName` parameter aan een bestaande toepassingen.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegoup1" -ServerName "server1" -DatabaseName "database1" -CopyResourceGroupName "poolResourceGroup" -CopyServerName "poolServer1" -CopyDatabaseName "database1_copy" -ElasticPoolName "poolName"


## <a name="resolve-logins"></a>Aanmeldingen oplossen

Raadpleeg [oplossen aanmeldingen](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes) aanmeldingen oplossen nadat de bewerking voor het kopiëren is voltooid


## <a name="example-powershell-script"></a>Voorbeeld van de PowerShell-script

Het volgende script wordt ervan uitgegaan dat alle resourcegroepen, servers, en de groep nog aanwezig is (de variabele waarden vervangen door uw bestaande resources). Alles moet aanwezig zijn, met uitzondering van het database-exemplaar.

    # Sign in to Azure and set the subscription to work with
    # ------------------------------------------------------
    $SubscriptionId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId
    
    
    # SQL database source (the existing database to copy)
    # ---------------------------------------------------
    $sourceDbName = "db1"
    $sourceDbServerName = "server1"
    $sourceDbResourceGroupName = "rg1"
    
    # SQL database copy (the new db to be created)
    # --------------------------------------------
    $copyDbName = "db1_copy"
    $copyDbServerName = "server2"
    $copyDbResourceGroupName = "rg2"
    
    # Copy a database to the same server
    # ----------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyDatabaseName $copyDbName
    
    # Copy a database to a different server
    # -------------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -CopyDatabaseName $copyDbName
    
    # Copy a database into an elastic database pool
    # ---------------------------------------------
    $poolName = "pool1"
    
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -ElasticPoolName $poolName -CopyDatabaseName $copyDbName



    

## <a name="next-steps"></a>Volgende stappen

- Zie [een Azure SQL-database kopiëren](sql-database-copy.md) voor een overzicht van het kopiëren van een SQL-Database Azure.
- Zie [met behulp van de portal Azure Azure SQL-database kopiëren](sql-database-copy-portal.md) voor het kopiëren van een database met behulp van de portal Azure.
- Zie [een Azure SQL-database kopiëren met behulp van T-SQL](sql-database-copy-transact-sql.md) voor het kopiëren van een database met behulp van Transact-SQL.
- Zie [het beheren van Azure SQL databasebeveiliging na noodherstel](sql-database-geo-replication-security-config.md) voor meer informatie over het beheren van gebruikers en aanmeldingen bij het kopiëren van een database naar een andere logische server.


## <a name="additional-resources"></a>Aanvullende bronnen

- [Nieuwe AzureRmSqlDatabase](https://msdn.microsoft.com/library/mt603644.aspx)
- [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/mt603687.aspx)
- [Aanmeldingen beheren](sql-database-manage-logins.md)
- [Verbinding maken met de SQL-Database met SQL Server Management Studio en een steekproef T-SQL-query uitvoeren](sql-database-connect-query-ssms.md)
- [De database exporteren naar een BACPAC](sql-database-export.md)
- [Business Continuity-overzicht](sql-database-business-continuity.md)
- [Documentatie voor SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure SQL Database PowerShell-Cmdlet referentie](https://msdn.microsoft.com/library/mt574084.aspx)

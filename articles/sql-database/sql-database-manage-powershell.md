<properties
    pageTitle="Azure SQL-Database met PowerShell beheren | Microsoft Azure"
    description="Azure SQL-Database beheren met PowerShell."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="sstein"/>

# <a name="manage-azure-sql-database-with-powershell"></a>Azure SQL-Database met PowerShell beheren


> [AZURE.SELECTOR]
- [Azure portal](sql-database-manage-portal.md)
- [Transact-SQL (SSMS)](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

Dit onderwerp bevat de PowerShell-cmdlets waarmee u veel taken voor Azure SQL-Database uit te voeren. Zie voor een volledige lijst [Azure SQL Database Cmdlets] (https://msdn.microsoft.com/library/mt574084(v=azure.300\).aspx).


## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken voor onze SQL-Database en de bijbehorende Azure resources met de [nieuw-AzureRmResourceGroup] (https://msdn.microsoft.com/library/azure/mt759837(v=azure.300\).aspx)-cmdlet.

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Zie [Azure PowerShell Azure Resource Manager gebruiken](../powershell-azure-resource-manager.md)voor meer informatie.
Zie voor een voorbeeld van een script, [maken een SQL-database PowerShell-script](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).

## <a name="create-a-sql-database-server"></a>Een SQL-Database-server maken

Maak een Database van SQL server met de [nieuw-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx)-cmdlet. Vervangt *server1* door de naam van uw server. Namen moeten uniek zijn voor alle servers met Azure SQL-Database. Als de naam van de server, al gebruikt wordt, krijgt u een fout. Deze opdracht kan enkele minuten in beslag nemen. De resourcegroep moet al bestaan in uw abonnement.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
$creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    

$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Zie [SQL-Database](sql-database-technical-overview.md)voor meer informatie. Zie voor een voorbeeld van een script, [maken een SQL-database PowerShell-script](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-server-firewall-rule"></a>Een firewallregel voor SQL-Database-server maken

Firewall regels maken om toegang tot de server met de [nieuw-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)-cmdlet. Voer de volgende opdracht, de begin- en IP-adressen te vervangen door geldige waarden voor de client. De resourcegroep en de server moet al bestaan in uw abonnement.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Een firewallregel maken voor andere Azure services toegang tot uw server, en zowel de `-StartIpAddress` en `-EndIpAddress` op **0.0.0.0**. Deze speciale regel staat alle Azure verkeer voor toegang tot de server.

Zie [Firewall voor Azure SQL-Database](https://msdn.microsoft.com/library/azure/ee621782.aspx)voor meer informatie. Zie voor een voorbeeld van een script, [maken een SQL-database PowerShell-script](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-blank"></a>Maak een SQL-database (leeg)

Een database maken met de [nieuw-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx)-cmdlet. De resourcegroep en de server moet al bestaan in uw abonnement. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Zie [SQL-Database](sql-database-technical-overview.md)voor meer informatie. Zie voor een voorbeeld van een script, [maken een SQL-database PowerShell-script](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="change-the-performance-level-of-a-sql-database"></a>Het prestatieniveau van een SQL-database wijzigen

Schaal van uw database omhoog of omlaag met de [Set-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx)-cmdlet. De resourcegroep, de server en de database moeten reeds bestaan in uw abonnement. Stel de `-RequestedServiceObjectiveName` een enkele spatie (zoals in het volgende fragment) voor eenvoudige laag. Stel deze in op de *S0*, *S1*, *P1*, *P6*, enz., zoals in het voorgaande voorbeeld voor andere lagen.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Zie voor meer informatie [SQL-Database-opties en prestaties: begrijpen wat beschikbaar is in elke servicelaag](sql-database-service-tiers.md). Zie voor een voorbeeld van een script, [monster PowerShell script wijzigen het serviceniveau voor laag en de prestaties van de SQL-database](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database).

## <a name="copy-a-sql-database-to-the-same-server"></a>Een SQL-database op dezelfde server kopiëren

Kopiëren van een SQL-database op dezelfde server met de [nieuw-AzureRmSqlDatabaseCopy] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx)-cmdlet. Stel de `-CopyServerName` en `-CopyResourceGroupName` op dezelfde waarden als uw database server en bron bronnengroep.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Zie voor meer informatie [Azure SQL-Database kopiëren](sql-database-copy.md). Zie voor een voorbeeldscript [kopiëren van een SQL-database PowerShell-script](sql-database-copy-powershell.md#example-powershell-script).


## <a name="delete-a-sql-database"></a>Een SQL-database verwijderen

Verwijderen van een SQL-database met de [software-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619368(v=azure.300\).aspx)-cmdlet. De resourcegroep, de server en de database moeten reeds bestaan in uw abonnement.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="delete-a-sql-database-server"></a>Een Database van SQL server verwijderen

Verwijderen van een server met de [software-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603488(v=azure.300\).aspx)-cmdlet.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="create-and-manage-elastic-database-pools-using-powershell"></a>Maken en beheren met PowerShell elastische database-toepassingen

Zie voor meer informatie over het maken van groepen van elastische database met PowerShell [maken een nieuwe elastische database toepassingen met PowerShell](sql-database-elastic-pool-create-powershell.md).

Zie voor meer informatie over het beheren van groepen van elastische database met PowerShell [controleren en beheren van een elastische database toepassingen met PowerShell](sql-database-elastic-pool-manage-powershell.md).



## <a name="related-information"></a>Verwante informatie

- [Cmdlets azure SQL Database] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [Azure Cmdlet referentie] (https://msdn.microsoft.com/library/azure/dn708514(v=azure.300\).aspx)

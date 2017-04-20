<properties
    pageTitle="Nieuwe SQL-Database-instelling met PowerShell | Microsoft Azure"
    description="Leer nu een SQL-database maken met PowerShell. Setup taken kunnen worden beheerd via PowerShell-cmdlets."
    keywords="nieuwe sql-database, database-instellingen maken"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/19/2016"
    ms.author="sstein"/>

# <a name="create-a-sql-database-and-perform-common-database-setup-tasks-with-powershell-cmdlets"></a>Een SQL-database maken en uitvoeren van gemeenschappelijke databasetaken met PowerShell-cmdlets


> [AZURE.SELECTOR]
- [Azure portal](sql-database-get-started.md)
- [PowerShell](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



Informatie over het maken van een SQL-database met behulp van PowerShell-cmdlets. (Zie [een nieuwe elastische database toepassingen met PowerShell maken](sql-database-elastic-pool-create-powershell.md)voor het maken van elastische databases.)


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="database-setup-create-a-resource-group-server-and-firewall-rule"></a>Database-instellingen: een resourcegroep, server en firewall-regel maken

Als u toegang hebt tot cmdlets uitvoeren voor het geselecteerde abonnement Azure, de volgende stap is de totstandbrenging van de resourcegroep waarop de server waar de database wordt gemaakt. Kunt u de volgende opdracht gebruiken een geldige locatie die u kiest. **Worden uitgevoerd (Get-AzureRmLocation | Waar-Object {$_. Providers - eq "Microsoft.Sql"}). Locatie** om een lijst met geldige locaties.

Voer de volgende opdracht om een resourcegroep te maken:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### <a name="create-a-server"></a>Een server maken

SQL-databases gemaakt in Azure SQL Database servers. Voer de [nieuw-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx) voor het maken van een server. De naam voor de server moet uniek zijn voor alle servers met Azure SQL-Database. Als de naam van de server, al gebruikt wordt, krijgt u een fout. Is ook vermeldenswaardig dat deze opdracht enige tijd duren kan om te voltooien. U kunt de opdracht voor het gebruik van een geldige locatie dat u kiest bewerken, maar moet u de locatie die u voor de resourcegroep in de vorige stap hebt gemaakt gebruikt.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Wanneer u deze opdracht uitvoert, wordt u gevraagd om uw gebruikersnaam en wachtwoord. Voer uw referenties Azure. In plaats daarvan voert u de gebruikersnaam en het wachtwoord maakt als beheerder van de server. Het script onder aan dit artikel ziet u hoe de referenties in de code instellen.

De Serverdetails worden weergegeven nadat de server is gemaakt.

### <a name="configure-a-server-firewall-rule-to-allow-access-to-the-server"></a>Een firewallregel server voor toegang tot de server configureren

Voor toegang tot de server, moet u een firewallregel maken. Voer de [nieuw-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)-opdracht, de begin- en IP-adressen te vervangen door geldige waarden voor uw computer.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

De details van de firewall-regels worden weergegeven nadat de regel is gemaakt.

Als u wilt toestaan dat andere Azure services toegang tot de server, Voeg een firewallregel en de StartIpAddress en de EndIpAddress ingesteld op 0.0.0.0. Deze regel staat Azure verkeer van een Azure-abonnement toegang krijgen tot de server.

Zie [Firewall voor Azure SQL-Database](sql-database-firewall-configure.md)voor meer informatie.


## <a name="create-a-sql-database"></a>Een SQL-database maken

U hebt nu een groep, een server en een firewallregel zodat u toegang hebt tot de server geconfigureerd.

De volgende [nieuw-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx)-opdracht maakt u een SQL-database (leeg) in de servicelaag-standaard met een prestatieniveau van S1:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


De informatie over de database worden weergegeven nadat de database is gemaakt.

## <a name="create-a-sql-database-powershell-script"></a>Een SQL-database PowerShell-script maken

De volgende PowerShell script maakt u een SQL-database en alle afhankelijke bronnen. Vervang alle `{variables}` met waarden die specifiek zijn voor uw abonnement en bronnen (de **{}** verwijderen wanneer u de waarden instellen).

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"
    
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation
    
    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"
    
    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    
    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
    
    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip
    
    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
    
    
    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"
    
    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo
    
   
    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## <a name="next-steps"></a>Volgende stappen
Nadat u een SQL-database maken en database van elementaire installatietaken uitvoeren, bent u klaar voor het volgende:

- [SQL-Database met PowerShell beheren](sql-database-manage-powershell.md)
- [Verbinding maken met de SQL-Database met SQL Server Management Studio en een steekproef T-SQL-query uitvoeren](sql-database-connect-query-ssms.md)


## <a name="additional-resources"></a>Aanvullende bronnen

- [Cmdlets azure SQL Database] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [Azure SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/)

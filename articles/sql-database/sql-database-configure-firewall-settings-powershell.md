<properties
    pageTitle="Azure SQL Database server niveau firewallregels configureren met behulp van PowerShell | Microsoft Azure"
    description="Informatie over het configureren van de firewall voor IP-adressen die toegang hebben tot Azure SQL-databases."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="sstein"/>


# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>Azure SQL Database server niveau firewallregels configureren met behulp van PowerShell


> [AZURE.SELECTOR]
- [Overzicht](sql-database-firewall-configure.md)
- [Azure portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)


Firewallregels Azure SQL-Database gebruikt om verbindingen met uw servers en databases. Niveau van de server en de database firewall-instellingen voor de master-database of een gebruikersdatabase kunt u definiëren in de SQL-databaseserver selectief toegang te verlenen tot de database.

> [AZURE.IMPORTANT] Als u toepassingen van Azure verbinding maken met de databaseserver, moeten Azure verbindingen zijn ingeschakeld. Zie voor meer informatie over de firewall-regels en -verbindingen inschakelen van Azure [Azure SQL Database Firewall](sql-database-firewall-configure.md). Als u de verbindingen binnen de grens van Azure cloud, wellicht enkele extra TCP-poorten. Voor meer informatie, Zie de "V12 van SQL-Database: buiten de vs binnen" sectie poorten [dan 1433 voor ADO.NET 4.5 en SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md).


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Server-firewall-regels maken

Niveau van de Server firewall regels kunnen worden gemaakt, bijgewerkt en verwijderd met behulp van Azure PowerShell.

Uitvoeren als u wilt een nieuwe firewallregel niveau van de server, de [nieuw-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)-cmdlet. Het volgende voorbeeld wordt een bereik van IP-adressen op de server Contoso.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'       

Als u wilt wijzigen een bestaande firewallregel serverniveau, voeren de [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx)-cmdlet. Het volgende voorbeeld wordt het bereik van geldige IP-adressen voor de regel met de naam ContosoFirewallRule.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' –StartIPAddress 192.168.1.4 –EndIPAddress 192.168.1.10 –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'

Uitvoeren als u wilt verwijderen van een bestaande firewallregel niveau van de server, de [software-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx)-cmdlet. Het volgende voorbeeld wordt de regel met de naam ContosoFirewallRule.

    Remove-AzureRmSqlServerFirewallRule –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>Firewall-regels beheren met PowerShell

U kunt ook PowerShell gebruiken voor het beheren van firewall-regels. Zie de volgende onderwerpen voor meer informatie:

* [Nieuwe AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)
* [AzureRmSqlServerFirewallRule verwijderen] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx)
* [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603586(v=azure.300\).aspx)


## <a name="next-steps"></a>Volgende stappen

Transact-SQL server- en database-niveau firewallregels maken Zie voor meer informatie over het gebruik van [Azure SQL-Database configureren van server- en database-niveau firewallregels met T-SQL](sql-database-configure-firewall-settings-tsql.md).

Zie voor informatie over het maken van firewallregels niveau van de server met behulp van andere methoden:

- [Azure SQL Database server niveau firewallregels met behulp van de portal Azure configureren](sql-database-configure-firewall-settings.md)
- [Azure SQL Database server niveau firewall-regels met de REST API configureren](sql-database-configure-firewall-settings-rest.md)

Zie voor een zelfstudie over het maken van een database [maken in minuten met behulp van de portal Azure een SQL-database](sql-database-get-started.md).
Zie [Client snelstartprogramma codevoorbeelden met SQL-Database](https://msdn.microsoft.com/library/azure/ee336282.aspx)voor informatie over verbinding maken met een Azure SQL-database via open-source of toepassingen van andere leveranciers.
Als u wilt weten hoe u kunt navigeren met databases, Zie [beheren van beveiliging van de database-aanmelding en -instellingen](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Aanvullende bronnen

- [Beveiliging van uw database](sql-database-security.md)
- [Security Center voor SQL Server-Database-Engine en Azure SQL-Database](https://msdn.microsoft.com/library/bb510589)


<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->

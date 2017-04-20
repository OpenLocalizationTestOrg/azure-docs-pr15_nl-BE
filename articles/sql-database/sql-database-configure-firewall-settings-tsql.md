<properties
    pageTitle="Azure-Database SQL server- en database-niveau firewallregels met T-SQL | Microsoft Azure"
    description="Informatie over het configureren van de firewall voor IP-adressen die toegang hebben tot Azure SQL-databases."
    services="sql-database"
    documentationCenter=""
    authors="BYHAM"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="rickbyh"/>


# <a name="configure-azure-sql-database-server-level-and-database-level-firewall-rules-using-t-sql"></a>Azure SQL Database server- en database-niveau firewallregels met T-SQL configureren


> [AZURE.SELECTOR]
- [Overzicht](sql-database-firewall-configure.md)
- [Azure Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)


Firewallregels Microsoft Azure SQL-Database gebruikt om verbindingen met uw servers en databases. U kunt server- en database-niveau firewall-instellingen voor de kapitein of een gebruikersdatabase in uw Azure SQL-databaseserver selectief toegang te verlenen tot de database definiëren.

> [AZURE.IMPORTANT] Als u toepassingen van Azure verbinding maken met de databaseserver, moeten Azure verbindingen zijn ingeschakeld. Zie voor meer informatie over de firewall-regels en -verbindingen inschakelen van Azure [Azure SQL Database Firewall](sql-database-firewall-configure.md). Als u de verbindingen binnen de grens van Azure cloud, wellicht enkele extra TCP-poorten. Zie voor meer informatie de **V12 van SQL-Database: buiten de vs binnen** sectie poorten [dan 1433 voor ADO.NET 4.5 en SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="server-level-firewall-rules"></a>Niveau van de Server firewall-regels

Alleen het niveau van de server principal aanmelding of Azure Active Directory-beheerder kunt u een firewallregel niveau van de server maken met behulp van Transact-SQL.

1. Een query-venster starten en verbinding maken met de virtuele master database via SQL Server Management Studio.
2. Niveau van de Server firewall-regels kunnen worden geselecteerd, gemaakt, bijgewerkt of verwijderd uit binnen het query-venster.
3. Uitvoeren als u wilt maken of bijwerken van firewallregels niveau van de server, de `sp_set_firewall_rule` opgeslagen procedure. Het volgende voorbeeld wordt een bereik van IP-adressen op de server Contoso.<br/>Start door te kijken welke regels bestaat.

        SELECT * FROM sys.firewall_rules ORDER BY name;

    Vervolgens voegt u een firewallregel.

        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'

    Opgeslagen procedure een firewallregel niveau van de server verwijderen, de sp_delete_firewall_rule uitvoeren. Het volgende voorbeeld wordt de regel met de naam ContosoFirewallRule.
 
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
 
 Zie voor meer informatie over deze opgeslagen procedures, [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) en [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx).

## <a name="database-level-firewall-rules"></a>Database-niveau firewall-regels

Alleen een databasegebruiker met de machtiging **beheren** in de database (zoals de eigenaar van de database) kunt maken van een firewallregel op databaseniveau.

1. Start na het maken van een niveau van de server firewall voor uw IP-adres, een queryvenster via de portal voor klassieke of SQL Server Management Studio.
2. Verbinding maken met de database waarvoor u een database niveau firewallregel maken.

    Uitvoeren van een nieuwe maken of bijwerken van een bestaande database niveau firewallregel, de `sp_set_database_firewall_rule` opgeslagen procedure. In het volgende voorbeeld wordt een nieuwe firewallregel met de naam ContosoFirewallRule gemaakt.
 
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
 
    Uitvoeren als u wilt verwijderen van een bestaande firewallregel databaseniveau, de `sp_delete_database_firewall_rule` opgeslagen procedure. Het volgende voorbeeld wordt de regel met de naam ContosoFirewallRule.
`
   EXEC sp_delete_database_firewall_rule @name = N'ContosoFirewallRule'

Zie voor meer informatie over deze opgeslagen procedures, [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) en [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx).

## <a name="next-steps"></a>Volgende stappen

Voor de manier waarop artikelen over het niveau van de server firewall-regels maken met behulp van andere methoden, Zie: 

- [Azure SQL Database server niveau firewallregels met behulp van de Portal Azure configureren](sql-database-configure-firewall-settings.md)
- [Azure SQL Database server niveau firewallregels met PowerShell configureren](sql-database-configure-firewall-settings-powershell.md)
- [Azure SQL Database server niveau firewall-regels met de REST API configureren](sql-database-configure-firewall-settings-rest.md)

Zie voor een zelfstudie over het maken van een database [maken in minuten met behulp van de portal Azure een SQL-database](sql-database-get-started.md).
Zie voor hulp bij het verbinding maken met een Azure SQL-database van open-source of toepassingen van andere leveranciers, [Client snelstartprogramma codevoorbeelden met SQL-Database](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Als u wilt weten hoe u kunt navigeren met databases, Zie [beheren van beveiliging van de database-aanmelding en -instellingen](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Aanvullende bronnen

- [Beveiliging van uw database](sql-database-security.md)
- [Security Center voor SQL Server-Database-Engine en Azure SQL-Database](https://msdn.microsoft.com/library/bb510589)

<properties
    pageTitle="Azure SQL-Database server niveau firewall-regels met de REST API | Microsoft Azure"
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


#  <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Azure SQL Database server niveau firewall-regels met de REST API configureren


> [AZURE.SELECTOR]
- [Overzicht](sql-database-firewall-configure.md)
- [Azure portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)


Firewallregels Microsoft Azure SQL-Database gebruikt om verbindingen met uw servers en databases. U kunt server- en database-niveau firewall-instellingen voor de kapitein of een gebruikersdatabase in uw Azure SQL-databaseserver selectief toegang te verlenen tot de database definiëren.

> [AZURE.IMPORTANT] Als u toepassingen van Azure verbinding maken met de databaseserver, moeten Azure verbindingen zijn ingeschakeld. Zie voor meer informatie over de firewall-regels en -verbindingen inschakelen van Azure [Azure SQL Database Firewall](sql-database-firewall-configure.md). Als u de verbindingen binnen de grens van Azure cloud, wellicht enkele extra TCP-poorten. Zie voor meer informatie de **V12 van SQL-Database: buiten de vs binnen** sectie poorten [dan 1433 voor ADO.NET 4.5 en SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Niveau van de server firewall-regels tot en met REST API beheren
1. Beheer van firewall-regels tot en met REST API moet worden geverifieerd. Zie [handleiding voor ontwikkelaars op toelating met de API Azure Resource Manager](../resource-manager-api-authentication.md)voor informatie.
2. Niveau van de server regels kunnen worden gemaakt, bijgewerkt of verwijderd met behulp van REST-API

    Te maken of een firewallregel niveau van de server bijwerken, voert u de methode PUT met behulp van de volgende:
 
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
    
    Hoofdgedeelte van de aanvraag

        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 
 

    U verwijdert een bestaande firewallregel niveau van de server, uitvoeren met de methode DELETE met behulp van de volgende:
     
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Beheren met de REST API firewall-regels

* [Maken of bijwerken van de firewallregel](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Firewallregel verwijderen](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Firewallregel ophalen](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [Lijst van alle Firewall-regels](https://msdn.microsoft.com/library/azure/mt604478.aspx)
 
## <a name="next-steps"></a>Volgende stappen

Zie voor een artikel over het gebruik van Transact-SQL server- en database-niveau firewall-regels maken het [Azure SQL-Database configureren van server- en database-niveau firewallregels met T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Voor de manier waarop artikelen over het niveau van de server firewall-regels maken met behulp van andere methoden, Zie: 

- [Azure SQL Database server niveau firewallregels met behulp van de portal Azure configureren](sql-database-configure-firewall-settings.md)
- [Azure SQL Database server niveau firewallregels met PowerShell configureren](sql-database-configure-firewall-settings-powershell.md)

Zie voor een zelfstudie over het maken van een database [maken in minuten met behulp van de portal Azure een SQL-database](sql-database-get-started.md).
Zie voor hulp bij het verbinding maken met een Azure SQL-database van open-source of toepassingen van andere leveranciers, [Client snelstartprogramma codevoorbeelden met SQL-Database](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Als u wilt weten hoe u kunt navigeren met databases, Zie [beheren van beveiliging van de database-aanmelding en -instellingen](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Aanvullende bronnen

- [Beveiliging van uw database](sql-database-security.md)
- [Security Center voor SQL Server-Database-Engine en Azure SQL-Database](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->

 

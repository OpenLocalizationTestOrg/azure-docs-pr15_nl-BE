<properties
    pageTitle="Een Database van SQL server-niveau firewallregel configureren | Microsoft Azure"
    description="Informatie over het configureren van de firewall voor IP-adressen die toegang hebben tot Azure SQL server."
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
    ms.author="rickbyh;carlrab"/>


# <a name="configure-an-azure-sql-database-server-level-firewall-rule-using-the-azure-portal"></a>Een Azure SQL Database server niveau firewallregel met behulp van de Portal Azure configureren


> [AZURE.SELECTOR]
- [Overzicht](sql-database-firewall-configure.md)
- [Azure Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)

Firewallregels Azure SQL server gebruikt voor verbindingen met de servers en databases. U kunt server- en database-niveau firewall-instellingen voor de kapitein of een gebruikersdatabase in de logische Azure SQL server-server selectief toegang te verlenen tot de database definiëren. In dit onderwerp wordt besproken niveau van de server firewall-regels.

> [AZURE.IMPORTANT] Als u toepassingen van Azure verbinding maken met uw Azure SQL server, moeten Azure verbindingen zijn ingeschakeld. Om te begrijpen hoe de firewallregels werk, Zie [het configureren van een firewall Azure SQL server \- overzicht](sql-database-firewall-configure.md). Als u de verbindingen binnen de grens van Azure cloud, wellicht enkele extra TCP-poorten. Zie voor meer informatie de **V12 van SQL-Database: buiten de vs binnen** sectie poorten [dan 1433 voor ADO.NET 4.5 en SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md)

**Aanbeveling:** Niveau van de server firewall-regels gebruiken voor beheerders en als er veel databases die dezelfde eisen toegang hebben, en u niet wilt dat opzien afzonderlijk configureren van elke database. Microsoft raadt het gebruik van de database niveau firewall-regels, indien mogelijk, ter verbetering van de beveiliging en kunt u de database meer draagbare.

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Bestaande firewallregels niveau van de server via de Azure portal beheren

Herhaal de stappen voor het beheren van het niveau van de server firewall-regels.

- Klik op client-IP-toevoegen als u de huidige computer.
- Extra IP-adressen, typt u in de naam van de regel, Start IP-adres en laatste IP-adres.
- Een bestaande regel wijzigen en klikt u op een van de velden in de regel wijzigen.
- Als u wilt verwijderen van een bestaande regel, plaatst u op de regel totdat de X wordt weergegeven aan het einde van de rij. Klik op X om de regel te verwijderen.

Klik op **Opslaan** om de wijzigingen opslaan.

## <a name="next-steps"></a>Volgende stappen

Zie voor een artikel over het gebruik van Transact-SQL server- en database-niveau firewall-regels maken het [Azure SQL-Database configureren van server- en database-niveau firewallregels met T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Voor de manier waarop artikelen over het niveau van de server firewall-regels maken met behulp van andere methoden, Zie: 

- [Azure SQL Database server niveau firewallregels met PowerShell configureren](sql-database-configure-firewall-settings-powershell.md)
- [Azure SQL Database server niveau firewall-regels met de REST API configureren](sql-database-configure-firewall-settings-rest.md)

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

 

<properties
   pageTitle="Een overzicht van SQL server firewall configureren | Microsoft Azure"
   description="Een SQL-database-firewall configureren met server- en database-niveau firewall-regels voor het beheren van toegang tot informatie."
   keywords="database-firewall"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor="cgronlun"
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/14/2016"
   ms.author="rickbyh"/>

# <a name="configure-azure-sql-database-firewall-rules---overview"></a>Azure SQL Database firewallregels configureren \- overzicht


> [AZURE.SELECTOR]
- [Overzicht](sql-database-firewall-configure.md)
- [Azure Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL-Database biedt de service van een relationele database voor Azure en andere Internet-toepassingen. Ter bescherming van uw gegevens, voorkomt een firewall alle toegang naar uw databaseserver totdat u opgeven welke computers zijn gemachtigd. De firewall verleent toegang tot databases die zijn gebaseerd op het oorspronkelijke IP-adres van elke aanvraag.

Configureren van uw firewall u firewall-regels die het bereiken van geldige IP-adressen opgeven. U kunt firewallregels maken op het niveau van de server en de database.

- **Niveau van de server firewall-regels:** Deze regels kunnen clients toegang tot de hele Azure SQL server, dat wil zeggen, alle databases in dezelfde logische server. Deze regels worden opgeslagen in **de hoofddatabase** . Niveau van de Server firewall-regels kunnen worden geconfigureerd met behulp van de portal of met behulp van Transact-SQL-instructies.
- **-Databaseniveau firewallregels:** Deze regels kunnen clients toegang tot afzonderlijke databases binnen de Azure SQL-databaseserver. U kunt deze regels maken voor elke database en deze worden opgeslagen in afzonderlijke databases. (Kunt u database-niveau firewall-regels voor de **master** -database.) Deze regels kunnen nuttig zijn bij het beperken van de toegang tot bepaalde (veilig) binnen dezelfde logische server-databases zijn. Firewallregels op databaseniveau kunnen alleen worden geconfigureerd met behulp van Transact-SQL-instructies.

**Aanbeveling:** Microsoft raadt het gebruik van de firewallregels databaseniveau zoveel mogelijk om beveiliging te verbeteren en meer draagbare maken van uw database. Niveau van de server firewall-regels gebruiken voor beheerders en als er veel databases die dezelfde eisen toegang hebben en u niet wilt dat opzien afzonderlijk configureren van elke database.


## <a name="firewall-overview"></a>Firewall-overzicht

In eerste instantie wordt alle Transact-SQL-toegang tot uw Azure SQL server geblokkeerd door de firewall. U moet om te beginnen met uw Azure SQL server gaat u naar de portal Azure en een of meer server-niveau firewallregels die toegang tot uw Azure SQL server opgeven. De firewall-regels gebruiken om op te geven welke IP-adresbereiken van het Internet zijn toegestaan en of Azure toepassingen verbinding mogen maken met uw Azure SQL server.

Om selectief toegang tot één van de Azure SQL server-databases, moet u een regel voor database-niveau voor de database nodig. Geef een IP-adresbereik voor de database firewallregel die buiten het bereik van IP-adres opgegeven in de firewallregel van het niveau van de server is en ervoor te zorgen dat de client het IP-adres in het bereik dat is opgegeven in de regel databaseniveau valt.

Verbindingspogingen van het Internet en Azure moeten eerst firewall passeren voordat ze uw Azure SQL server of SQL-Database bereikt, zoals in het volgende diagram wordt weergegeven.

   ![Diagram met een beschrijving van de firewallconfiguratie.][1]

## <a name="connecting-from-the-internet"></a>Verbinding maken via het Internet

Wanneer een computer verbinding probeert te maken met de databaseserver van het Internet, wordt het oorspronkelijke IP-adres van de aanvraag ten opzichte van de volledige set van firewall-regels eerst gecontroleerd door de firewall:

- Als het IP-adres van de aanvraag binnen een van de bereiken in het niveau van de server firewall-regels, wordt de verbinding met uw server Azure SQL-Database verleend.

- Als het IP-adres van de aanvraag niet binnen een van de bereiken in de firewallregel niveau van de server, worden de database niveau firewallregels gecontroleerd. Als het IP-adres van de aanvraag binnen een van de bereiken in de database niveau firewall-regels, wordt de verbinding slechts verleend aan de database een overeenkomende regel voor database-niveau is.

- Als het IP-adres van de aanvraag niet binnen de bereiken in een van de server- of database-niveau firewallregels, de aanvraag verbinding mislukt.

> [AZURE.NOTE] Voor toegang tot Azure SQL-Database vanaf de lokale computer, zorgen dat de firewall op het netwerk en de lokale computer kan uitgaande communicatie via TCP-poort 1433.


## <a name="connecting-from-azure"></a>Verbinding maken vanaf Azure

Wanneer een toepassing van Azure probeert verbinding met de databaseserver, de firewall controleert of dat Azure verbindingen zijn toegestaan. Een firewall instellen met begin- en eindadres op 0.0.0.0 gelijk geeft aan dat deze verbindingen zijn toegestaan. Als de verbindingspoging niet is toegestaan, komt de aanvraag niet aan de Azure SQL-databaseserver.

> [AZURE.IMPORTANT] Met deze optie configureert u de firewall voor alle verbindingen van Azure, inclusief verbindingen van de abonnementen van andere klanten. Wanneer u deze optie selecteert, controleert u of uw aanmelding en machtigingen van de gebruiker de toegang beperken tot alleen geautoriseerde gebruikers.

U kunt de verbindingen van Azure op twee manieren inschakelen:

- Selecteer in de [portal van Microsoft Azure](https://portal.azure.com/), het selectievakje **toestaan dat azure services toegangsserver** wanneer u een nieuwe server.

- Klik op **Ja** voor **Microsoft Azure Services**in de [klassieke portal](http://go.microsoft.com/fwlink/p/?LinkID=161793)op het tabblad **configureren** op een server in de sectie **Services toegestaan** .

## <a name="creating-the-first-server-level-firewall-rule"></a>Het eerste niveau van de server firewallregel maken

Het eerste niveau van de server firewall-instelling kan worden gemaakt met behulp van de [Azure portal](https://portal.azure.com/) of via programmacode met behulp van de REST API of Azure PowerShell. Volgende niveau van de server firewall-regels kunnen worden gemaakt en beheerd met behulp van deze methoden als via Transact-SQL. Om prestaties te verbeteren, niveau van de server firewall-regels tijdelijk worden opgeslagen op het databaseniveau van de. Zie [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx)te vernieuwen de cache. Zie voor meer informatie over het niveau van de server firewallregels [procedure: een met behulp van de portal Azure Azure SQL server-firewall configureren](sql-database-configure-firewall-settings.md).

## <a name="creating-database-level-firewall-rules"></a>Database-niveau firewall-regels maken

Nadat u het eerste niveau van de server firewall hebt geconfigureerd, kunt u de toegang beperken tot bepaalde databases. Als u een IP-adresbereik in de database niveau firewallregel valt buiten het bereik dat is opgegeven in de firewallregel van het niveau van de server opgeven, kunnen clients die op IP-adressen in het bereik van de database niveau hebben toegang tot de database. U kunt maximaal 128 databaseniveau firewallregels voor een database hebben. Databaseniveau firewallregels voor hoofd- en databases worden gemaakt en beheerd via Transact-SQL. Zie voor meer informatie over het configureren van de database niveau firewall-regels [sp_set_database_firewall_rule (Azure SQL-Databases)](https://msdn.microsoft.com/library/dn270010.aspx).

## <a name="programmatically-managing-firewall-rules"></a>Programmatisch beheer van firewall-regels

Naast de Azure portal kunnen firewallregels beheren via programmacode met behulp van Transact-SQL, REST API en Azure PowerShell. De volgende tabellen beschrijven de set opdrachten die beschikbaar zijn voor elke methode.


### <a name="transact-sql"></a>Transact-SQL

| Catalogusweergave of opgeslagen Procedure                                                           | Niveau     | Beschrijving                                          |
|--------------------------------------------------------------------------------------------|-----------|------------------------------------------------------|
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx)                   | Server    | Het huidige niveau van de server firewall-regels wordt weergegeven     |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx)             | Server    | Gemaakt of bijgewerkt, niveau van de server firewall-regels       |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx)          | Server    | Hiermee verwijdert u de server niveau firewall-regels                  |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx)        | Database  | Geeft de huidige database niveau firewall-regels   |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx)    | Database  | Gemaakt of bijgewerkt, de database niveau firewall-regels |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) | Databases | Hiermee verwijdert u databaseniveau firewall-regels                |

### <a name="rest-api"></a>REST-API

| API                  | Niveau  | Beschrijving                                                      |
|----------------------|--------|------------------------------------------------------------------|
| [Lijst met Firewall-regels](https://msdn.microsoft.com/library/azure/dn505715.aspx)  | Server | Het huidige niveau van de server firewall-regels wordt weergegeven                 |
| [Firewallregel maken](https://msdn.microsoft.com/library/azure/dn505712.aspx) | Server | Gemaakt of bijgewerkt, niveau van de server firewall-regels                   |
| [Firewallregel instellen](https://msdn.microsoft.com/library/azure/dn505707.aspx)    | Server | De eigenschappen van een bestaande firewallregel niveau van de server wordt bijgewerkt |
| [Firewallregel verwijderen](https://msdn.microsoft.com/library/azure/dn505706.aspx) | Server | Hiermee verwijdert u de server niveau firewall-regels                              |


### <a name="azure-powershell"></a>Azure PowerShell

| Cmdlet                                                                                              | Niveau  | Beschrijving                                                      |
|-----------------------------------------------------------------------------------------------------|--------|------------------------------------------------------------------|
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx)    | Server | Deze eigenschap retourneert het huidige niveau van de server firewall-regels                  |
| [Nieuwe AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx)    | Server | Hiermee maakt u een nieuwe firewallregel niveau van de server                         |
| [Set AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx)    | Server | De eigenschappen van een bestaande firewallregel niveau van de server wordt bijgewerkt |
| [Verwijderen AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) | Server | Hiermee verwijdert u de server niveau firewall-regels                              |

> [AZURE.NOTE] Kan er van maar liefst vijf minuten vertraging voor wijzigingen in de firewall-instellingen te activeren.

## <a name="troubleshooting-the-database-firewall"></a>Problemen met de firewall van de database

Wanneer u toegang tot de service Microsoft Azure SQL-Database niet meer werkt zoals u verwacht, moet u rekening houden met de volgende punten:

- **Lokale firewall-configuratie:** Voordat uw computer toegang Azure SQL-Database, moet u mogelijk een firewall-uitzondering voor TCP-poort 1433 op de computer maken. Als u verbindingen binnen de grens van Azure cloud maken, kunt u wellicht extra poorten openen. Zie voor meer informatie de **V12 van SQL-Database: buiten de vs binnen** sectie poorten [dan 1433 voor ADO.NET 4.5 en SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md).

- **Netwerkadresomzetting (NAT):** Door NAT, het IP-adres van uw computer verbinding maken met Azure SQL-Database mogelijk anders dan het IP-adres in de IP-configuratie-instellingen weergegeven. Als u het IP-adres dat uw computer wordt gebruikt om verbinding met Azure, aanmelden op de portal en Ga naar het tabblad **configureren** op de server die fungeert als host voor de database. In de sectie **Toegestane IP-adressen** , wordt het **Huidige IP-adres van de Client** weergegeven. Klik op **toevoegen** aan de **Toegestane IP-adressen** op deze computer toestaan om toegang tot de server.

- **Wijzigingen aan de lijst niet van kracht zijn nog:** Mogelijk zijn er maar liefst vijf minuten vertraging voor wijzigingen in de configuratie van de firewall Azure SQL-Database te activeren.

- **Is niet toegestaan, de aanmelding of een onjuist wachtwoord is gebruikt:** Als een aanmelding geen machtigingen op de server Azure SQL-Database of het wachtwoord onjuist is, wordt de verbinding met de Database van Azure SQL server is geweigerd. Biedt een firewall-instelling maken alleen clients met de mogelijkheid om te proberen verbinding te maken met uw server. elke client moet de noodzakelijke beveiligingsreferenties opgeven. Zie voor meer informatie over de voorbereiding van aanmeldingen beheren van Databases, aanmeldingsnamen en gebruikers in Azure SQL-Database.

- **Dynamisch IP-adres:** Als u een internetverbinding met dynamische IP-adressen zijn er problemen met het ophalen via de firewall, kunt u een van de volgende oplossingen:

 - Vraag uw Internet Service Provider (ISP) om het IP-adresbereik toegewezen aan de clientcomputers die toegang hebben tot de Azure SQL-databaseserver en vervolgens voegt u het IP-adresbereik toe een firewallregel.

 - Ophalen van statische IP-adressering in plaats daarvan voor de clientcomputers, en vervolgens de IP-adressen toevoegen als de firewall-regels.

## <a name="next-steps"></a>Volgende stappen

Voor de artikelen op het niveau van de server en database-niveau firewallregels maakt, Zie:

- [Azure SQL Database server niveau firewallregels met behulp van de portal Azure configureren](sql-database-configure-firewall-settings.md)
- [Azure SQL Database server- en database-niveau firewallregels met T-SQL configureren](sql-database-configure-firewall-settings-tsql.md)
- [Azure SQL Database server niveau firewallregels met PowerShell configureren](sql-database-configure-firewall-settings-powershell.md)
- [Azure SQL Database server niveau firewall-regels met de REST API configureren](sql-database-configure-firewall-settings-rest.md)

Zie voor een zelfstudie over het maken van een database [maken in minuten met behulp van de portal Azure een SQL-database](sql-database-get-started.md).
Zie voor hulp bij het verbinding maken met een Azure SQL-database van open-source of toepassingen van andere leveranciers, [Client snelstartprogramma codevoorbeelden met SQL-Database](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Als u wilt weten hoe u kunt navigeren met databases, Zie [beheren van beveiliging van de database-aanmelding en -instellingen](https://msdn.microsoft.com/library/azure/ee336235.aspx).



## <a name="additional-resources"></a>Aanvullende bronnen

- [Beveiliging van uw database](sql-database-security.md)
- [Security Center voor SQL Server-Database-Engine en Azure SQL-Database](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png

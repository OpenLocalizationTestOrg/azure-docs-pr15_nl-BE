<properties
    pageTitle="Actieve Geo-replicatie voor Azure SQL-Database"
    description="Actieve Geo-replicatie kunt u setup-4 replica's van de database in een van de Azure datacenters."
    services="sql-database"
    documentationCenter="na"
    authors="stevestein"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/26/2016"
    ms.author="sstein" />

# <a name="overview-sql-database-active-geo-replication"></a>Overzicht: SQL-Database actief Geo-replicatie

Actieve Geo-replicatie kunt u maximaal vier leesbaar secundaire databases configureren op de locaties in dezelfde of verschillende data center (regio's). Secundaire databases zijn beschikbaar voor het uitvoeren van query's en voor failover in geval van een stroomstoring van data center of het onvermogen om verbinding met de primaire database.

>[AZURE.NOTE] Actieve Geo-replicatie (secundaire servers kan worden gelezen) is nu beschikbaar voor alle databases in alle Servicelagen. In April 2017, wordt het niet-leesbare secundaire type ingetrokken en bestaande niet-leesbare databases automatisch worden bijgewerkt naar secundaire servers voor leesbaar.

 Actieve Geo-replicatie met behulp van de [portal Azure](sql-database-geo-replication-portal.md), [PowerShell](sql-database-geo-replication-powershell.md), [Transact-SQL](sql-database-geo-replication-transact-sql.md), kunt u of de [REST API - maken of Database Update](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Configureren: Azure portal](sql-database-geo-replication-portal.md)
- [Configureren: PowerShell](sql-database-geo-replication-powershell.md)
- [Configureren: T-SQL](sql-database-geo-replication-transact-sql.md)

Als voor een reden uw primaire database mislukt of gewoon moet worden off line genomen, *kunt u op een van de secundaire databases* . Als failover kan worden geactiveerd in een van de secundaire databases, worden andere secundaire servers worden automatisch gekoppeld aan de nieuwe primaire.

U kunt een failover naar een secundair via de [portal Azure](sql-database-geo-replication-failover-portal.md), [PowerShell](sql-database-geo-replication-failover-powershell.md), [Transact-SQL](sql-database-geo-replication-failover-transact-sql.md), de [REST API - geplande Failover](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)of [REST API - niet-geplande Failover](https://msdn.microsoft.com/library/azure/mt582027.aspx).


> [AZURE.SELECTOR]
- [Failover: Azure portal](sql-database-geo-replication-failover-portal.md)
- [Failover: PowerShell](sql-database-geo-replication-failover-powershell.md)
- [Failover: T-SQL](sql-database-geo-replication-failover-transact-sql.md)

Controleer dat de vereisten voor de server en de database zijn geconfigureerd op de nieuwe primaire na een failover. Zie [beveiliging voor SQL-Database na noodherstel](sql-database-geo-replication-security-config.md)voor meer informatie.


De functie Active Geo-replicatie implementeert een mechanisme voor redundantie database binnen hetzelfde Microsoft Azure regio of in verschillende regio's (geo-redundantie). Actieve Geo-replicatie repliceert asynchroon doorgevoerde transacties uit een database op maximaal vier kopieën van de database op verschillende servers, met behulp van vastgelegde momentopname-isolatie (RCSI) voor isolatie lezen. Wanneer Active Geo-replicatie is geconfigureerd, wordt een secundaire database gemaakt op de opgegeven server. De primaire database, wordt de oorspronkelijke database. De primaire database repliceert doorgevoerde transacties asynchroon aan elk van de secundaire databases. Alleen volledige transacties worden gerepliceerd. De secundaire database misschien iets achter de primaire database terwijl u op elk gewenst moment, de secundaire gegevens wordt gegarandeerd nooit gedeeltelijke transacties. De specifieke gegevens van de vrijgegeven Productieorder kan worden gevonden op [Overzicht van Business Continuity](sql-database-business-continuity.md).

Een van de belangrijkste voordelen van Active Geo-replicatie is dat u een databaseniveau disaster recovery-oplossing met lage hersteltijd. Wanneer u de secundaire database op een server in een andere regio, wordt het maximale schokbestendigheid toevoegen aan uw toepassing. Cross-regio redundantie kan toepassingen herstellen van een permanent verlies van een gehele datacenter of delen van een datacenter veroorzaakt door natuurrampen, ernstige menselijke fouten of schadelijke handelingen. De volgende afbeelding ziet dat een voorbeeld van actieve Geo-replicatie wordt geconfigureerd met een primair in de regio Zuid-centrale ons in de regio Noord centrale ons en secundaire.

![Geo-replicatie relatie](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Een ander belangrijk voordeel is dat de secundaire databases kunnen gelezen worden en kunnen worden gebruikt voor offload van alleen-lezen werkbelasting zoals taken melden. Als u alleen de secundaire database gebruiken voor netwerktaakverdeling, kunt u deze in dezelfde regio als de primaire. Maken van een secundair in hetzelfde gebied, vergroot de veerkracht van de toepassing op kritieke fouten optreden.  

Er zijn andere scenario's waarbij actieve Geo-replicatie kan worden gebruikt:

- **De databasemigratie**: U kunt actieve Geo-replicatie een database migreren van de ene server naar de andere on line met een minimale uitvaltijd.
- **Upgrades van toepassing**: kunt u een extra secundaire als een fail back up tijdens toepassingsupgrades.

Toevoeging van redundantie database tussen datacenters voor echte bedrijfscontinuïteit, is slechts een deel van de oplossing. Een toepassing (service) end-to-end herstellen na een onherstelbare fout herstel van alle onderdelen die de service en eventuele afhankelijke services vereist. Voorbeelden van deze componenten zijn de client-software (bijvoorbeeld een browser met een aangepaste JavaScript), web-front-ends, opslag en DNS. Het is essentieel dat alle onderdelen op de fouten van dezelfde robuuste en beschikbaar in de recovery time doelstelling (RTO) van uw toepassing. Daarom moet u alle afhankelijke services herkennen en begrijpen van de garanties en de mogelijkheden die ze bieden. Vervolgens moet u nemen voldoende om ervoor te zorgen dat de servicefuncties tijdens de overname van de diensten waarvan deze afhankelijk is. Zie voor meer informatie over het ontwerpen van oplossingen voor noodherstel, [Ontwerpen van Cloud oplossingen voor Disaster Recovery met actieve Geo-replicatie](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Mogelijkheden voor actieve Geo-replicatie
De functie Active Geo-replicatie biedt de volgende essentiële mogelijkheden:

- **Automatische asynchrone replicatie**: U kunt alleen een secundaire database maken door toe te voegen aan een bestaande database. De secundaire kan alleen worden gemaakt in een andere Azure SQL Database server. Eenmaal is gemaakt, wordt de secundaire database gevuld met de gegevens van de primaire database gekopieerd. Dit proces heet enten. Nadat de tweede database is gemaakt en overgeënt, worden updates voor de primaire database asynchroon naar de secundaire database automatisch gerepliceerd. Asynchrone replicatie betekent dat transacties worden vastgelegd op de primaire database voordat ze worden gerepliceerd naar de secundaire database. 

- **Meerdere secundaire databases**: twee of meer secundaire databases redundantie en het niveau van bescherming van de primaire database en de toepassing verbeteren. Als u meerdere secundaire databases bestaan, blijft de toepassing beveiligd zelfs als een van de secundaire databases niet. Als er slechts één secundaire database is mislukt, wordt de toepassing blootgesteld aan grotere risico's tot een nieuwe secundaire database is gemaakt.

- **Leesbare secundaire databases**: een toepassing toegang heeft tot een secundaire database voor alleen-lezen bewerkingen met dezelfde of een andere beveiligings-principals gebruikt voor toegang tot de primaire database. De secundaire databases werken in de isolatiemodus momentopname om replicatie van de updates van de primaire (replay logboek) niet wordt vertraagd door query's uitgevoerd op de secundaire server.

>[AZURE.NOTE] Het logboek opnieuw afspelen wordt vertraagd op de secundaire database als schema-updates die het ontvangt van de primaire waarvoor de schemavergrendeling van een van de secundaire database.

- **Actieve geo-replicatie elastische pool databases**: actieve Geo-replicatie kan worden geconfigureerd voor een database in een groep met elastische database. De secundaire database kan zich in een andere database elastische toepassingen. Voor gewone databases, kan de secundaire een elastische database toepassingen en vice versa zijn, zolang de service niveaus hetzelfde zijn. 

- **Configureerbare prestatieniveau van de secundaire database**: een secundaire database kan worden gemaakt met een lager prestatieniveau dan de primaire. Primaire en secundaire databases zijn hebben hetzelfde niveau van de service vereist. Deze optie wordt niet aanbevolen voor toepassingen met hoge database schrijfactiviteiten omdat de vertraging meer replicatie het risico van verlies van belangrijke gegevens na een failover verhoogt. De prestaties van de toepassing is bovendien na een failover tot de nieuwe primaire is opgewaardeerd naar een hoger prestatieniveau beïnvloed. Het logboek IO percentage overzicht op Azure portal biedt een goede manier om een schatting van de minimale prestatieniveau van de secundaire die nodig is voor het handhaven van de belasting van de replicatie. Bijvoorbeeld, als uw primaire database P6 (1000 DTU) en het logboek i/o-percentage is 50% moet ten minste de secundaire P4 (500 DTU). Ook kunt u de logboekgegevens van de i/o via [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) of [sys.dm_db_resource_stats]( https://msdn.microsoft.com/library/dn800981.aspx) databaseweergaven ophalen.  Zie voor meer informatie over de prestaties van de SQL-Database [SQL-Database-opties en prestaties](sql-database-service-tiers.md). 

- **Gebruiker gecontroleerde failover en failback**: een secundaire database kan expliciet worden verplaatst naar de rol van primair apparaat op elk gewenst moment door de toepassing of de gebruiker. Tijdens een stroomstoring real moet de 'niet-geplande' worden gebruikt, die onmiddellijk een secundair bevordert zijn de primaire. Wanneer de primaire mislukte hersteld en weer beschikbaar is, wordt het systeem automatisch worden gemarkeerd als een secundaire herstelde primaire en zet het up-to-date blijft met de nieuwe primaire. Door de asynchrone aard van replicatie, kan een kleine hoeveelheid gegevens worden verloren tijdens failover niet gepland als een primaire mislukt voordat deze wordt de meest recente wijzigingen gerepliceerd naar de secundaire. Als een primaire met een secundaire servers voor meerdere failover wordt uitgevoerd, wordt het systeem automatisch opnieuw geconfigureerd de replicatie relaties en de resterende secundaire servers voor de koppelingen naar de onlangs gepromoveerde primaire zonder tussenkomst van de gebruiker. Nadat de stroomstoring, waardoor failover wordt beperkt, kan het wenselijk dat de toepassing op de primaire regio zijn. Hiervoor moet de failover-opdracht worden aangeroepen met de optie 'gepland'. 

- **Referenties en firewall-regels synchroon houden**: met behulp van [de firewallregels database](sql-database-firewall-configure.md) voor geo-gerepliceerde databases zodat deze regels kan worden gerepliceerd met de database, zodat alle secundaire databases hebben dezelfde firewallregels als voor de primaire het beste. Deze aanpak hoeven klanten handmatig configureren en onderhouden van firewall-regels op servers waarop zowel de primaire en secundaire databases. Op dezelfde manier, zodat [die gebruikers](sql-database-manage-logins.md) gebruiken voor toegang tot gegevens garandeert zowel primaire als secundaire databases hebben altijd dezelfde gebruiker referenties tijdens een failover, is er geen verstoringen als gevolg van problemen met de aanmeldingen en wachtwoorden. Met de toevoeging van [Azure Active Directory](../active-directory/active-directory-whatis.md)kunnen klanten toegang tot de primaire en secundaire databases en hoeft u niet voor het beheren van referenties in databases helemaal beheren.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Upgraden of downgraden van een primaire database
U kunt upgraden of downgraden van een primaire database (binnen hetzelfde niveau service) naar een andere prestaties zonder eventuele secundaire databases wordt verbroken. Wanneer u een upgrade uitvoert, wordt aangeraden eerst de secundaire database bijwerken en vervolgens werkt u de primaire. Wanneer u uw abonnement beperkt, de volgorde: eerst de primaire downgraden en vervolgens gebruik maken van de secundaire. 

De secundaire database moet zich in hetzelfde niveau als de primaire, service zodat uw primaire database migreren naar een andere service-laag, u de koppeling geo-replicatie beëindigen moet en wijzig de naam van de secundaire database of u gewoon. Vervolgens de primaire migreren naar de nieuwe servicelaag en geo-replicatie opnieuw configureren. De nieuwe secundaire wordt standaard automatisch worden gemaakt met hetzelfde prestatieniveau als de primaire.

## <a name="preventing-the-loss-of-critical-data"></a>Het verlies van kritieke gegevens te voorkomen
Continue kopiëren gebruikt vanwege de hoge latentie van WAN's, een replicatiemechanisme voor de asynchrone. Asynchrone replicatie kunt u gegevensverlies onvermijdelijk is als er een fout optreedt. Sommige toepassingen kunnen evenwel eisen dat er geen gegevens verloren gaan. Ter bescherming van deze essentiële updates, kan een ontwikkelaar van toepassingen de procedure [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx) systeem aanroepen meteen na het doorvoeren van de transactie. Het aanroepen van **sp_wait_for_database_copy_sync** blokken de thread totdat de laatste transactie die is vastgelegd in de secundaire database is gerepliceerd. De procedure wacht totdat alle in de wachtrij transacties erkend door de secundaire database. **sp_wait_for_database_copy_sync** is binnen het bereik van de koppeling van een specifieke continue kopiëren. Elke gebruiker met de rechten van de verbinding met de primaire database kunt deze procedure aanroepen.

>[AZURE.NOTE] De vertraging die wordt veroorzaakt door een procedure-aanroep **sp_wait_for_database_copy_sync** kan aanzienlijk zijn. De vertraging is afhankelijk van de grootte van de transactie log lengte op het moment en dit gesprek niet terug totdat het hele logboek wordt gerepliceerd. Voorkomen dat u deze procedure aanroept tenzij absoluut noodzakelijk.

## <a name="programmatically-managing-active-geo-replication"></a>Programmatisch beheer van actieve Geo-replicatie

Zoals eerder besproken, kan actieve Geo-replicatie ook worden beheerd via programmacode met behulp van Azure PowerShell en de REST-API. De volgende tabellen beschrijven de set opdrachten die beschikbaar zijn.

- **Azure Resource Manager API en op rollen gebaseerde beveiliging**: actieve Geo-replicatie omvat een reeks [Azure Resource Manager API's]( https://msdn.microsoft.com/library/azure/mt163571.aspx) voor beheer, met inbegrip van de [op basis van de bronnenbeheerder Azure PowerShell-cmdlets](sql-database-geo-replication-powershell.md). Deze API's vereisen het gebruik van bronnengroepen en ondersteuning voor op rollen gebaseerde beveiliging (RBAC). Rollen Zie voor meer informatie over het implementeren van access [Azure_Role-Based Access Control](../active-directory/role-based-access-control-configure.md).

>[AZURE.NOTE] Veel nieuwe functies van Active Geo-replicatie worden alleen ondersteund met [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) op basis van [Azure SQL REST API](https://msdn.microsoft.com/library/azure/mt163571.aspx) en [Azure SQL Database PowerShell-cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx). De REST-API (klassiek)] (https://msdn.microsoft.com/library/azure/dn505719.aspx) en [Azure SQL-Database (klassiek)-cmdlets](https://msdn.microsoft.com/library/azure/dn546723.aspx) worden ondersteund voor achterwaartse compatibiliteit zodat met API's op basis van Azure Resource Manager worden aanbevolen. 


### <a name="transact-sql"></a>Transact-SQL

|Opdracht|Beschrijving|
|-------|-----------|
|[ALTER DATABASE (Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx)|SECUNDAIRE op SERVER toevoegen argument gebruiken om een secundaire database voor een bestaande database en het repliceren begint te maken|
|[ALTER DATABASE (Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx)|Failover- of FORCE_FAILOVER_ALLOW_DATA_LOSS gebruiken om te schakelen van een secundaire database voor het initiëren van failover primair
|[ALTER DATABASE (Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx)|Gebruik secundaire op SERVER verwijderen een gegevensreplicatie tussen een SQL-Database en de database van de opgegeven secundaire beëindigen.|
|[sys.geo_replication_links (SQL-Database Azure)](https://msdn.microsoft.com/library/mt575501.aspx)|Retourneert informatie over alle replicatiekoppelingen voor elke database op de logische server Azure SQL-Database.|
|[sys.dm_geo_replication_link_status (SQL-Database Azure)](https://msdn.microsoft.com/library/mt575504.aspx)|De laatste keer dat replicatie, laatste replicatie lag en andere informatie over de replicatiekoppeling haalt voor een bepaalde SQL database.|
|[sys.dm_operation_status (SQL-Database Azure)](https://msdn.microsoft.com/library/dn270022.aspx)|Geeft de status voor alle databasebewerkingen, waaronder de status van de replicatiekoppelingen.|
|[sp_wait_for_database_copy_sync (SQL-Database Azure)](https://msdn.microsoft.com/library/dn467644.aspx)|zorgt ervoor dat de toepassing moet worden gewacht totdat alle doorgevoerde transacties zijn gerepliceerd en bevestigd door de actieve secundaire database.|
||||

### <a name="powershell"></a>PowerShell

|Cmdlet|Beschrijving|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Met deze eigenschap haalt een of meer databases.|
|[Nieuwe AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx)|Hiermee maakt u een secundaire database voor een bestaande database en gegevensreplicatie wordt gestart.|
|[Set AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt619393.aspx)|Een secundaire database om het primaire initiëren van failover worden geschakeld.|
|[Verwijderen AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt603457.aspx)|Gegevensreplicatie tussen een SQL-Database en de database van de opgegeven secundaire beëindigt.|
|[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx)|Met deze eigenschap wordt de geo-replicatie koppelingen tussen een Azure SQL-Database en een resourcegroep of SQL Server.|
||||

### <a name="rest-api"></a>REST-API

|API|Beschrijving|
|---|-----------|
|[Maken of de Database bijwerken (createMode = terugzetten)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Maken, bijwerken of een primaire of secundaire database herstelt.|
|[Get maken of bijwerken van de databasestatus](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Geeft als resultaat de status tijdens een bewerking maken.|
|[Secundaire Database instellen als primaire (geplande Failover)](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)|Bevordering van een tweede database in een partnerschap Geo-replicatie worden de nieuwe primaire database.|
|[Secundaire Database instellen als primaire (niet-geplande Failover)](https://msdn.microsoft.com/library/azure/mt582027.aspx)|Een failover-bewerking voor de secundaire database en de secundaire instellen als de primaire.|
|[Koppelingen voor replicatie](https://msdn.microsoft.com/library/azure/mt600929.aspx)|Haalt alle replicatiekoppelingen voor een opgegeven SQL-database in een partnerschap geo-replicatie. De gegevens in de weergave van de catalogus sys.geo_replication_links zichtbaar worden.|
|[Koppeling voor replicatie](https://msdn.microsoft.com/library/azure/mt600778.aspx)|Haalt een replicatiekoppeling specifiek voor een bepaalde SQL-database in een partnerschap geo-replicatie. De gegevens in de weergave van de catalogus sys.geo_replication_links zichtbaar worden.|
||||



## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van business continuity en scenario's [Business continuity-overzicht](sql-database-business-continuity.md)
- Zie informatie over Azure SQL-Database automatisch back-ups, [geautomatiseerde back-ups van SQL-Database](sql-database-automated-backups.md).
- Zie informatie over het gebruik van geautomatiseerde back-ups voor herstel [herstellen van een database van back-ups service gestart](sql-database-recovery-using-backups.md).
- Zie voor meer informatie over het gebruik van geautomatiseerde back-ups voor archivering, [databasekopie](sql-database-copy.md).
- Zie meer informatie over verificatie eisen voor een nieuwe primaire server en database, [SQL-Database beveiliging na noodherstel](sql-database-geo-replication-security-config.md).

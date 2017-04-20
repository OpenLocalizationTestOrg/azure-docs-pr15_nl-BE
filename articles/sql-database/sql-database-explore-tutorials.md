<properties
   pageTitle="Zelfstudies voor Azure SQL Database verkennen | Microsoft Azure"
   description="Meer informatie over de mogelijkheden en functies van SQL-Database"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="08/24/2016"
   ms.author="carlrab"/>
   
# <a name="explore-azure-sql-database-tutorials"></a>Zelfstudies voor Azure SQL Database verkennen

De onderstaande koppelingen gaat u naar een overzicht van elk functiegebied weergegeven en een eenvoudige stap door start zelfstudie voor elk gebied. Zie voor binnen het bereik van oplossing snel begint die aantonen het gebruik van SQL-Database in een complete oplossing op basis van praktijksituaties dat, [Azure SQL Database oplossing snel begint](sql-database-solution-quick-starts.md).

## <a name="using-sql-server-management-studio"></a>Met behulp van SQL Server Management Studio

In de volgende cursussen leert u over Azure SQL-Database opvragen en beheren met behulp van SQL Server Management Studio.


> [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de meest recente versie van het Management Studio gebruiken om met updates voor Microsoft Azure en SQL-Database gesynchroniseerd blijven. [Bijwerken van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Verbinding maken met Azure SQL-Database met behulp van een UPN-niveau van de server-aanmelding](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| In deze zelfstudie leert u hoe verbinding maken met Azure SQL-Database met behulp van een UPN-niveau van de server-aanmelding.|
| [Als een gebruiker verbinding maken met Azure SQL-database](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | In deze zelfstudie leert u hoe u verbinding maken met een gebruikersaccount op databaseniveau Azure SQL-database.|
||||

## <a name="elastic-pools"></a>Elastische pools

In de volgende cursussen leert u over het gebruik van [elastische van toepassingen](sql-database-elastic-pool.md) voor het beheer van de prestatiedoelstellingen voor meerdere databases met veel verschillende en onvoorspelbare gebruikspatronen.

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Maak een elastische toepassingen](sql-database-elastic-pool-create-portal.md) | In deze zelfstudie leert u hoe een schaalbare toepassingen van Azure SQL-databases maken. |
| [Een elastische database controleren](sql-database-elastic-pool-manage-portal.md#elastic-database-monitoring) | In deze zelfstudie leert u hoe een afzonderlijke elastische database voor mogelijke problemen te controleren. |
| [Een waarschuwing toevoegen aan een resourcegroep resource](sql-database-elastic-pool-manage-portal.md#add-an-alert-to-a-pool-resource) | In deze zelfstudie leert u hoe u regels toevoegt aan resources die per e-mail aan personen of waarschuwing tekenreeksen eindpunten URL als de bron een drempel die u hebt ingesteld voor gebruik raakt. |
| [Een database verplaatsen naar een elastische toepassingen](sql-database-elastic-pool-manage-portal.md#move-a-database-into-an-elastic-pool) | In deze zelfstudie leert u hoe een database verplaatsen naar een elastische toepassingen. |
| [Een database uit een elastisch toepassingen verplaatsen](sql-database-elastic-pool-manage-portal.md#move-a-database-out-of-an-elastic-pool) | In deze zelfstudie leert u hoe een database verplaatsen van een elastische toepassingen. |
| [Prestatie-instellingen van een groep wijzigen](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) | In deze zelfstudie leert u hoe de grenzen voor prestaties en opslag voor een groep van toepassingen aanpassen. |
||||

## <a name="elastic-database-jobs"></a>Elastische database taken

In de volgende cursussen leert u met behulp van [elastische database taken](sql-database-elastic-jobs-overview.md).

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Aan de slag met elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md) | In deze zelfstudie leert u hoe u de mogelijkheden van elastische Databasehulpmiddelen met behulp van een eenvoudige toepassing een laptopgeheugen. |
| [Aan de slag met Azure SQL Database elastische taken](sql-database-elastic-jobs-getting-started.md)  | In deze zelfstudie leert u hoe te maken en beheren van taken die een groep van gerelateerde databases beheren.  | 
||||

## <a name="elastic-queries"></a>Elastische query 's

In de volgende cursussen leert u over [elastische query's](sql-database-elastic-query-overview.md)uitvoeren. 

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Via een horizontaal gepartitioneerde (een laptopgeheugen) database opvragen)](sql-database-elastic-query-getting-started.md) | In deze zelfstudie leert u hoe om rapporten te maken van alle databases in een horizontaal gepartitioneerde (een laptopgeheugen) database met [elastische query](sql-database-elastic-query-overview.md) |
| [Uitvoeren van een query in een verticaal gepartitioneerde database)](sql-database-elastic-query-getting-started-vertical.md#create-database-objects) | In deze zelfstudie leert u hoe om rapporten te maken van alle databases in een verticaal database met [elastische query](sql-database-elastic-query-overview.md) |
| [Migreren van een bestaande database te schalen](sql-database-elastic-convert-to-use-elastic-tools.md)| In deze zelfstudie leert u voor het horizontaal schalen (shard) een Azure SQL-database. |
||||

## <a name="performance-optimization"></a>Optimalisatie van prestaties

In de volgende cursussen leert u over het optimaliseren van de [prestaties van afzonderlijke databases](sql-database-performance-guidance.md). Zie voor het optimaliseren van de prestaties van meerdere databases [elastische pools](#elastic-pools).

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Het serviceniveau voor laag en de prestaties van uw database wijzigen](sql-database-scale-up.md#change-the-service-tier-and-performance-level-of-your-database) | In deze zelfstudie leert u hoe te vergroten of verkleinen van de prestaties van de service tiers met Azure SQL-database. |
| [SQL Database Advisor Query prestaties inzicht](sql-database-performance.md#performance-overview) | In deze zelfstudie leert u hoe SQL Database Advisor Query prestaties inzicht te openen.|
| [SQL Database Advisor prestaties aanbevelingen](sql-database-advisor.md#viewing-recommendations) | In deze zelfstudie leert u hoe bekijken en toepassen van SQL-Database Advisor prestaties aanbevelingen. |
| [Bovenste CPU verbruikt query's bekijken](sql-database-query-performance.md#review-top-cpu-consuming-queries)| In deze zelfstudie leert u hoe met behulp van SQL-Database Advisor Query prestaties inzicht bovenste CPU verbruikt query's bekijken.|
| [Details van de afzonderlijke query's weergeven](sql-database-query-performance.md#viewing-individual-query-details)| In deze zelfstudie leert u SQL Database Advisor Query prestaties inzicht gebruiken om de afzonderlijke query's Prestatiedetails te bekijken.|
||||

## <a name="sql-database-migration-and-archive"></a>SQL-Database migreren en archief 

In de volgende cursussen leert u over het [migreren van een bestaande SQL Server-database met Azure SQL-Database](sql-database-cloud-migrate.md).

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Opsporen van problemen met de compatibiliteit met SQL Server Data Tools voor Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md#detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | In deze zelfstudie leert u hoe u met behulp van SQL Server Data Tools voor Visual Studio compatibiliteit Azure SQL-Database. |
| [Oplossen van problemen met de compatibiliteit met SQL Server Data Tools voor Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt#fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | In deze zelfstudie leert u SQL Server Data Tools voor Visual Studio gebruiken voor het oplossen van compatibiliteitsproblemen Azure SQL-Database. |
| [SQL-Database compatibiliteit met SqlPackage.exe bepalen](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md#using-sqlpackageexe) | In deze zelfstudie leert u hoe u met het opdrachtregelprogramma SQLPackage.exe compatibiliteit Azure SQL-Database bepaalt.|
| [Compatibiliteit van SQL-Database met behulp van SSMS bepalen](sql-database-cloud-migrate-determine-compatibility-ssms.md#using-sql-server-management-studio) |In deze zelfstudie leert u hoe u met behulp van SQL Server Management Studio compatibiliteit Azure SQL-Database.|
| [SQL Server-database naar Microsoft Azure Database Wizard Database implementeren met SQL-Database migreren](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md#use-the-deploy-database-to-microsoft-azure-database-wizard) | In deze zelfstudie leert u een compatibele SQL Server-database migreren naar Azure SQL-Database de Database distribueren Microsoft Azure Database wizard gebruikt in SQL Server Management Studio.
| [Een SQL Server-database exporteren naar een Bacpac-bestand met behulp van SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | In deze zelfstudie leert u hoe u een compatibele SQL Server-database exporteren naar een Bacpac-bestand met de Wizard exporteren Tier toepassing in SQL Server Management Studio.|
| [Een SQL Server-database exporteren naar een Bacpac-bestand met behulp van SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | In deze zelfstudie leert u een compatibele SQL Server-database exporteren naar een Bacpac-bestand met het opdrachtregelprogramma SQLPackage.exe.|
| [Een Bacpac-bestand importeren in Azure SQL-Database met behulp van SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | In deze zelfstudie leert u hoe u een database importeert in Azure SQL-Database vanuit een Bacpac-bestand met de Wizard exporteren Tier toepassing in SQL Server Management Studio. |
| [Een Bacpac-bestand importeren in Azure SQL-Database met behulp van SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md#import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage) | In deze zelfstudie leert u hoe u een database importeert in Azure SQL-Database vanuit een Bacpac-bestand met het opdrachtregelprogramma SQLPackage. |
| [Een Bacpac-bestand importeren in Azure SQL-Database via de portal Azure](sql-database-import.md) | In deze zelfstudie leert u hoe u een database importeert in Azure SQL-Database vanuit een Bacpac-bestand dat is opgeslagen in een Azure blob met de Portal Azure.|
| [Een Bacpac-bestand importeren in Azure PowerShell met SQL-Database](sql-database-import-powershell.md) | In deze zelfstudie leert u hoe u een database importeert in Azure SQL-Database vanuit een Bacpac-bestand met PowerShell.|
| [Archief met behulp van de portal Azure Azure SQL-database](sql-database-export.md#export-your-database) | In deze zelfstudie leert u hoe een Azure SQL-database naar een Bacpac-bestand met behulp van de portal Azure archiveren. |
| [Archiveren met PowerShell Azure SQL-database](sql-database-export-powershell.md) | In deze zelfstudie leert u hoe een Azure SQL-database naar een Bacpac-bestand met PowerShell archiveren. |
| [Kopiëren met behulp van de portal Azure Azure SQL-database](sql-database-copy.md#copy-your-sql-database) | In deze zelfstudie leert u hoe met behulp van de portal Azure Azure SQL-database kopiëren. |
| [Met PowerShell Azure SQL-database kopiëren](sql-database-copy-powershell#copy-your-sql-database) | In deze zelfstudie leert u hoe om te kopiëren met PowerShell Azure SQL-database. |
| [Kopiëren met behulp van Transact-SQL Azure SQL-database](sql-database-copy-transact-sql.md#copy-your-sql-database) | In deze zelfstudie leert u hoe met behulp van Transact-SQL Azure SQL-database kopiëren. |
||||

##<a name="develop"></a>Ontwikkelen

In de volgende cursussen leert u over [SQL databaseontwikkeling](sql-database-develop-overview.md) en het gebruik van [bibliotheken voor databaseconnectiviteit](sql-database-libraries.md).

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Verbinding maken met de SQL-Database via .NET (C#)](sql-database-develop-dotnet-simple.md) | In deze zelfstudie leert u hoe verbinding maken met een Azure SQL-database met behulp van C#. |
| [Verbinding maken met de SQL-Database met behulp van Java](sql-database-develop-java-simple.md) | In deze zelfstudie leert u hoe verbinding maken met een Azure SQL-database met behulp van Java. |
| [Verbinding maken met de SQL-Database via Node.js](sql-database-develop-nodejs-simple.md) | In deze zelfstudie leert u hoe verbinding maken met een Node.js met Azure SQL-database. |
| [Verbinding maken met de SQL-Database met behulp van PHP](sql-database-develop-php-simple.md) | In deze zelfstudie leert u hoe verbinding maken met een Azure SQL-database met PHP. |
| [Verbinding maken met de SQL-Database met behulp van Python](sql-database-develop-python-simple.md) | In deze zelfstudie leert u hoe verbinding maken met een Azure SQL-database met behulp van Python. |
| [Verbinding maken met de SQL-Database met behulp van Ruby](sql-database-develop-ruby-simple.md) | In deze zelfstudie leert u hoe verbinding maken met een Ruby met Azure SQL-database. |
||||
 
## <a name="database-access"></a>Toegang tot de database

In de volgende cursussen leert u over het [maken en beheren van aanmeldingen van gebruikers](sql-database-manage-logins.md).

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Een Azure SQL Database server niveau firewallregel maken met behulp van de portal Azure](sql-database-configure-firewall-settings.md)  | Informatie over het configureren van een SQL-Database server niveau firewall met behulp van de portal Azure in deze zelfstudie.  |
| [Met behulp van Transact-SQL database niveau firewall regels maken](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules) | In deze zelfstudie leert u hoe u een database niveau firewallregel maken met Transact-SQL.|
| [Beheren met behulp van Transact-SQL server niveau firewallregels](sql-database-configure-firewall-settings-tsql.md#manage-server-level-firewall-rules-through-transact-sql) | In deze zelfstudie leert u het beheer van een firewall voor Azure SQL-Database-niveau van de server met behulp van Transact-SQL.|
| [Niveau van de server firewallregels met PowerShell beheren](sql-database-configure-firewall-settings-powershell.md#manage-firewall-rules-using-powershell) | In deze zelfstudie leert u hoe een Azure SQL Database server niveau firewall met PowerShell beheert.|
| [Niveau van de server firewallregels met de REST API beheren](sql-database-configure-firewall-settings-rest.md#manage-firewall-rules-using-the-service-management-rest-api) | In deze zelfstudie leert u het beheer van een firewall voor Azure SQL-Database-niveau van de server met de API voor opnieuw instellen.|
| [Verbinding maken met Azure SQL-Database met behulp van een UPN-niveau van de server-aanmelding](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| In deze zelfstudie leert u hoe verbinding maken met Azure SQL-Database met behulp van een UPN-niveau van de server-aanmelding.|
| [Databasetoegang verlenen tot een aanmelding] (sql-database-manage-logins.md#granting-database-access-to-a-login() | In deze zelfstudie leert u hoe databasetoegang verlenen tot een niveau van de server aanmelding.|
| [Nieuwe databasegebruiker met behulp van SSMS maken](sql-database-get-started-security.md#create-new-database-user-using-ssms) | In deze zelfstudie leert u hoe u een nieuwe databasegebruiker maken in een bestaande database met behulp van SSMS.|
| [Nieuwe database db_owner gebruikersmachtigingen verlenen](sql-database-get-started-security.md#grant-new-database-user-dbowner-permissions) | In deze zelfstudie leert u hoe een bestaande database gebruiker db_owner om machtigingen te verlenen.|
| [Als een gebruiker verbinding maken met Azure SQL-database](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | In deze zelfstudie leert u hoe verbinding maken met een gebruikersaccount op databaseniveau Azure SQL-database.|
||||


## <a name="data-security"></a>Beveiliging van gegevens

In de volgende cursussen leert u over het [beveiligen van Azure SQL-Database](sql-database-security.md). 

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [De database met behulp van de portal Azure bedreiging-detectie inschakelen](sql-database-threat-detection-get-started.md#set-up-threat-detection-for-your-database) | In deze zelfstudie leert u hoe threat detection in Azure portal voor uw database instellen.|
| [Beveiliging van vertrouwelijke gegevens uisng altijd gecodeerd](sql-database-always-encrypted-azure-key-vault.md) | In deze zelfstudie gebruikt u de wizard altijd gecodeerd voor het beveiligen van vertrouwelijke gegevens in een database SQL Azure.|
| [Veilig laten gegevens transparant codering](https://msdn.microsoft.com/library/dn948096.aspx)| In deze zelfstudie leert u hoe transparante gegevenscodering gebruiken om u te laten gegevens zijn beveiligd.|
| [Een kolom met gegevens coderen](https://msdn.microsoft.com/library/ms179331.aspx)| In deze zelfstudie leert u het coderen van een kolom met gegevens met behulp van Transact-SQL.|
| [Maskeren voor dynamische gegevens instellen](sql-database-dynamic-data-masking-get-started.md#set-up-dynamic-data-masking-for-your-database-using-the-azure-portal)  | In deze zelfstudie leert u hoe om dynamische gegevens maskeren voor uw Azure SQL-database in te stellen. |
||||

## <a name="business-continuity-and-query-scale-out"></a>Bedrijfscontinuïteit en Query Scale-Out

In de volgende cursussen leert u over het gebruik van [Geo-terugzetten en actieve Geo-replicatie](sql-database-business-continuity.md) te reccover tegen fouten voor bedrijfscontinuïteit en schalen van query.

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Azure SQL-Database herstellen naar een vorig punt in de tijd met Azure Portal](sql-database-point-in-time-restore-portal.md)| In deze zelfstudie leert u hoe u de database te herstellen naar een eerder punt in de tijd met behulp van de portal Azure.|
| [Azure SQL-Database herstellen naar een vorig punt in de tijd met PowerShell](sql-database-point-in-time-restore-powershell.md) | In deze zelfstudie leert u hoe de database te herstellen op een eerder tijdstip met PowerShell|
| [Een verwijderde Azure SQL-database via de Portal Azure terugzetten](sql-database-restore-deleted-database-portal.md) | In deze zelfstudie leert u hoe met behulp van de portal Azure-een verwijderde database terugzetten.|
| [Een verwijderde Azure SQL-database met de PowerShell terugzetten](sql-database-restore-deleted-database-powershell.md) | In deze zelfstudie leert u hoe om te herstellen van een verwijderde database met PowerShell.|
| [Geo-replicatie configureren voor Azure SQL-Database via de portal Azure](sql-database-geo-replication-portal.md)| In deze zelfstudie leert u hoe Active Geo-replicatie met behulp van de portal Azure configureren.|
| [Geo-replicatie configureren voor Azure PowerShell met SQL-Database](sql-database-geo-replication-powershell.md)| In deze zelfstudie leert u hoe Active Geo-replicatie configureren met PowerShell.|
| [Geo-replicatie configureren voor Azure SQL-Database met behulp van Transact-SQL](sql-database-geo-replication-transact-sql.md)| In deze zelfstudie leert u hoe Active Geo-replicatie configureren met behulp van Transact-SQL.|
| [Het initiëren van failover of niet gepland voor Azure SQL-Database via de portal Azure](sql-database-geo-replication-failover-portal.md) | In deze zelfstudie leert u hoe u failover naar een geo gerepliceerd secundaire replica met de Azure portal.|
| [Het initiëren van failover of niet gepland voor Azure PowerShell met SQL-Database](sql-database-geo-replication-failover-powershell.md) | In deze zelfstudie leert u hoe u failover naar een geo gerepliceerd secundaire replica met PowerShell.|
| [Het initiëren van failover of niet gepland voor Azure SQL-Database met behulp van Transact-SQL](sql-database-geo-replication-failover-transact-sql.md) | In deze zelfstudie leert u hoe u failover naar een geo gerepliceerd secundaire replica met behulp van Transact-SQL.|
||||

## <a name="data-sync"></a>Gegevens synchroniseren

In deze zelfstudie leert u over het [Synchroniseren van gegevens](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

| Zelfstudie  | Beschrijving  |
|---|---|---| 
| [Aan de slag met Azure SQL gegevens synchroniseren (voorbeeld)](sql-database-get-started-sql-data-sync.md)  | In deze zelfstudie leert u de grondbeginselen van Azure SQL gegevens synchroniseren met de klassieke Azure-Portal. |
||||

## <a name="next-steps"></a>Volgende stappen

[Ontdek de Azure SQL Database oplossing snel aan de slag](sql-database-solution-quick-starts.md)

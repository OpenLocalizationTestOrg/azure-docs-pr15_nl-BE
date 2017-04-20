<properties
    pageTitle="Alle onderwerpen voor de service SQL-Database | Microsoft Azure"
    description="Tabel van alle onderwerpen voor de Azure service met de naam SQL-Database die zich op http://azure.microsoft.com/documentation/articles/, titel en beschrijving."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="genemi"/>


# <a name="all-topics-for-azure-sql-database-service"></a>Alle onderwerpen voor service Azure SQL-Database

In dit onderwerp worden elk onderwerp dat van toepassing op de service **SQL-Database** van Azure rechtstreeks is. U kunt deze webpagina voor trefwoorden zoeken met behulp van **Ctrl + F**, te vinden in de onderwerpen van de huidige rente.




## <a name="new"></a>Nieuw

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 1 | [Daxko/CSI Azure gebruikt de ontwikkelingscyclus en en de klantenservice en de prestaties verbeteren](sql-database-implementation-daxko.md) | Meer informatie over hoe Daxko/CSI SQL-Database gebruikt de ontwikkelingscyclus en en de klantenservice en de prestaties verbeteren |
| 2 | [Azure biedt GEP wereldwijd bereik en een verbeterde efficiëntie](sql-database-implementation-gep.md) | Meer informatie over hoe GEP SQL-Database gebruikt om meer klanten bereiken en extra efficiëntie |
| 3 | [Met Azure, is SnelStart snel haar zakelijke diensten met een snelheid van 1000 nieuwe Azure SQL-Databases per maand uitgebreid](sql-database-implementation-snelstart.md) | Meer informatie over hoe SnelStart SQL-Database gebruikt om snel uitgebreid haar zakelijke diensten met een snelheid van 1000 nieuwe Azure SQL-Databases per maand |
| 4 | [Umbraco Azure SQL-Database gebruikt snel levering en schaal diensten voor duizenden huurders in de cloud](sql-database-implementation-umbraco.md) | Meer informatie over het gebruik van SQL-Database snel inrichten in Umbraco en schaal diensten voor duizenden huurders in de cloud |
| 5 | [Azure SQL-Database met PowerShell beheren](sql-database-manage-powershell.md) | Azure SQL-Database beheren met PowerShell. |
| 6 | [SSMS-ondersteuning voor Azure AD MVR gesloten met de SQL-Database en SQL Data Warehouse](sql-database-ssms-mfa-authentication.md) | Rekening gehouden met Multi verificatie gebruiken met SSMS voor SQL-Database en SQL datawarehouse. |
| 7 | [Uitleg over Database-transactie-eenheden (DTUs) en de elastische Database transactie-eenheden (eDTUs)](sql-database-what-is-a-dtu.md) | Inzicht in wat een Azure SQL-Database is transactie-eenheid. |


## <a name="updated-articles-sql-database"></a>Bijgewerkte artikelen, SQL-Database

Deze sectie bevat een lijst van artikelen die onlangs zijn bijgewerkt waarin de update groot of significant is. Voor elk artikel bijgewerkt, wordt een ruwe fragment van de prijsverlaging toegevoegde tekst weergegeven. De artikelen zijn bijgewerkt binnen het datumbereik van **2016-08-22** tot en met **2016-10-05**.

| &nbsp; | Artikel | Bijgewerkte tekst, fragment | Bijgewerkt toen |
| --: | :-- | :-- | :-- |
| 8 | [Azure SQL-Database met PowerShell beheren](sql-database-command-line-tools.md) | Een resourcegroep maken voor onze SQL-Database en de bijbehorende Azure resources met de cmdlet New-AzureRmResourceGroup (https://msdn.microsoft.com/library/azure/mt759837.aspx). *$resourceGroupName = "resourcegroup1" $resourceGroupLocation = "northcentralus" nieuw-AzureRmResourceGroup-naam $resourceGroupName-locatie $resourceGroupLocation* Zie voor meer informatie, met behulp van Azure PowerShell Azure Resource Manager (.. / powershell azure-resource manager.md.). Zie voor een voorbeeld van een script, maken een SQL-database (sql-database-get-gestart-powershell.md create-a-sql-database-powershell-script)-PowerShell-script. **Maken van een Database van SQL server** Maak een Database van SQL server met de cmdlet New-AzureRmSqlServer (https://msdn.microsoft.com/library/azure/mt603715.aspx). Vervangt *server1* door de naam van uw server. Namen moeten uniek zijn voor alle servers met Azure SQL-Database. Als de naam van de server, al gebruikt wordt, krijgt u een fout. Deze opdracht kan enkele minuten in beslag nemen. De resou | 2016-09-14 |
| 9 | [Met PowerShell Azure SQL-database kopiëren](sql-database-copy-powershell.md) |   SQL databasebron (de bestaande database wilt kopiëren)--$sourceDbName = 'boekwaarde 1' $sourceDbServerName = 'server1' $sourceDbResourceGroupName = "rg1" SQL-database kopiëren (de nieuwe database moet worden gemaakt)--$copyDbName = "db1_copy" $copyDbServerName = "server2" $copyDbResourceGroupName = "rg2" kopie van een database op dezelfde server--nieuw AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - servernaam $sourceDbServerName - DatabaseName $sourceDbName - CopyDatabaseName $copyDbName een database kopiëren naar een andere server--nieuw AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - servernaam $sourceDbServerName - DatabaseName $sourceDbName - CopyResourceGroupName $copyDbResourceGroupName - CopyServerName $copyDbServerName - CopyDatabaseName $copyDbName een database kopiëren naar een database elastische toepassingen--$poolName = "pool1" nieuw-AzureRmSqlDatabaseCopy - ResourceGroupName $ sourceDbResourceGroupName - servernaam $sourceDbServerName - DatabaseName $sourceDbName - CopyReso | 2016-09-08 |
| 10 | [Maak een elastische database toepassingen met C#](sql-database-elastic-pool-create-csharp.md) |   Console.WriteLine ("een firewall...');  FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule (_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);  Console.WriteLine ("een firewall:" + fwr. FirewallRule.Id);  Console.WriteLine ("elastische pool...");  ElasticPoolCreateOrUpdateResponse epr (eindpuntreferentie) = CreateOrUpdateElasticDatabasePool (_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);  Console.WriteLine ("elastische pool:" + epr (eindpuntreferentie). ElasticPool.Id);  Console.WriteLine("Database...");  DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase (_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);  Console.WriteLine ("Database:" + dbr. Database.Id);  Console.WriteLine ("Druk op een toets om door te gaan...');  Console.ReadKey();  } statische ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupNa | 2016-09-14 |
| 11 | [PowerShell script voor het identificeren van databases die geschikt zijn voor een elastische database toepassingen](sql-database-elastic-pool-database-assessment-powershell.md) | **Voor elastische database groep kandidaten, we geen master en alle databases die in een groep. U mag toevoegen meer databases in de onderstaande lijst uitgesloten indien nodig** $ListOfDBs = $servername, Get-AzureRmSqlDatabase - servernaam. Split('.') 0 - ResourceGroupName-$ResourceGroupName / waar-Object {$_. DatabaseName - notin ("master") - en $_. CurrentServiceLevelObjectiveName - notin ("ElasticPool")- en $_. Notin - CurrentServiceObjectiveName ("ElasticPool")} $outputConnectionString = "Data Source = $outputServerName; Integrated Security = false; eerste catalogus = $outputdatabaseName; Gebruikers-Id = $outputDBUsername; Wachtwoord = $outputDBpassword ' $destinationTableName = "resource_stats_output" **maken van een tabel in de database output voor de parameters-collectie** $sql = "als NOT EXISTS (selecteren * van sys.objects WHERE object_id = OBJECT_ID(N'$($destinationTableName)') en typ in (N'U')) BEGIN maken tabel $($destinationTableName) (databasenaam varchar(128), slo-varchar(20), end_time, datetime, avg_cpu float, avg_ | 2016-09-29 |
| 12 | [Zelfstudie voor SQL-Database: een SQL-database maken in minuten met behulp van de portal voor Azure](sql-database-get-started.md) |   ! Nieuwe sql-database 1 (. / media/sql-database-get-started/sql-database-new-database-1.png) 3. Klik op **SQL-Database** voor het openen van de SQL-Database-blade. De inhoud van deze blade is afhankelijk van het aantal abonnementen en uw bestaande objecten (zoals bestaande servers).  ! Nieuwe sql-database 2 (. / media/sql-database-get-started/sql-database-new-database-2.png) 4. Geef een naam voor uw eerste database 'mijn database' in het tekstvak **naam van de Database** . Een groen vinkje geeft aan dat u een geldige naam hebt opgegeven.  ! Nieuwe sql-database 3 (. / media/sql-database-get-started/sql-database-new-database-3.png) 5. Selecteer een abonnement hebt u meerdere abonnementen. 6. Klik op **Nieuw** onder **resourcegroep**, en geef een naam op voor de eerste resourcegroep - zoals "mijn-resource-groep". Een groen vinkje geeft aan dat u een geldige naam hebt opgegeven.  ! Nieuwe sql-database 4 (. / media/sql-database-get-started/sql-database-new-database-4.png) 7. Onder **bron selecteren* | 2016-09-08 |
| 13 | [Voer een SQL-Database: Gebruik C# een SQL-database maken met de SQL-Database bibliotheek voor .NET](sql-database-get-started-csharp.md) | **Een service principal toegang krijgen tot bronnen maken** De volgende PowerShell script maakt de toepassing van Active Directory (AD) en de service principal die we nodig hebben voor het verifiëren van onze C-app. Het script levert waarden die we nodig hebben om de voorgaande C monster. Zie voor gedetailleerde informatie gebruik Azure PowerShell voor het maken van een service principal voor toegang tot bronnen (.. / resource-groep-verificatie-service-principal.md.).  Aanmelden bij Azure.  Toevoegen-AzureRmAccount als er meerdere abonnementen, behandelen en ingesteld op het abonnement dat u wilt werken.  $subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}" Set AzureRmContext - SubscriptionId $subscriptionId bieden deze waarden voor uw nieuwe AAD app.  $appName is de weergavenaam voor uw app, moet uniek zijn in uw directory.  $uri hoeft niet te worden van een echte uri.  $secret is een wachtwoord dat u maakt.  $appName = "{app-naam}" $uri = "http://{app-name"} $secret = '{app-wachtwoord}' maken een app AAD $azureAdApplication = New AzureRmADApp | 2016-09-23 |
| 14 | [Azure SQL-Databases via de Azure portal beheren](sql-database-manage-portal.md) | Als u wilt weergeven of bijwerken van de database-instellingen, klikt u op de gewenste instelling op de SQL-database blade:! SQL-database-instellingen (. / media/sql-database-manage-portal/settings.png) **Hoe vind ik een SQL-databases volledig gekwalificeerde naam?** Als u de naam van uw databases, klikt u op **Overzicht** in de **SQL-database** -blade en noteer de servernaam van de:! SQL-database-instellingen (. / media/sql-database-manage-portal/server-name.png) **Hoe beheer toegang tot mijn database en SQL server firewall-regels?** Als u wilt weergeven, maken of bijwerken van de firewall-regels, klikt u op **een firewall** op de **SQL-database** -blade. Zie configureren een Azure SQL Database server niveau firewallregel met de Azure portal (sql-database-configureren-firewall-settings.md) voor meer informatie. ! firewall-regels (. / media/sql-database-manage-portal/sql-database-firewall.png) **hoe wijzig ik mijn SQL database laag of prestaties serviceniveau?** Klik op om het serviceniveau voor laag of de prestaties van een SQL-database ** prijzen laag (s | 2016-09-20 |





## <a name="get-started"></a>Aan de slag

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 15 | [Maakt de huurder met meerdere Apps met Azure SQL-Database met isolatie en efficiëntie](sql-database-build-multi-tenant-apps.md) | Meer informatie over hoe SQL-Database wordt gemaakt met meerdere huurder apps |
| 16 | [Verbinding maken met de SQL-Database met SQL Server Management Studio en een steekproef T-SQL-query uitvoeren](sql-database-connect-query-ssms.md) | Informatie over hoe u verbinding maakt met SQL-Database op Azure met behulp van SQL Server Management Studio (SSMS). Voer een voorbeeldquery die met behulp van Transact-SQL (T-SQL). |
| 17 | [Verbinding maken met de SQL-Database via .NET (C#)](sql-database-develop-dotnet-simple.md) | Gebruik de voorbeeldcode in deze snel beginnen met het opbouwen van een moderne toepassing die met C# en ondersteund door een krachtige relationele database in de cloud met Azure SQL-Database. |
| 18 | [Een nieuwe database elastische toepassingen maken met de Azure portal](sql-database-elastic-pool-create-portal.md) | Hoe een schaalbare elastische database toepassingen toevoegen aan uw SQL-databaseconfiguratie voor eenvoudiger beheer en de resource binnen veel databases te delen. |
| 19 | [Zelfstudies voor Azure SQL Database verkennen](sql-database-explore-tutorials.md) | Meer informatie over de mogelijkheden en functies van SQL-Database |
| 20 | [Zelfstudie voor SQL-Database: een SQL-database maken in minuten met behulp van de portal voor Azure](sql-database-get-started.md) | Informatie over het instellen van een logische server SQL-Database, de firewallregel server, SQL-database en voorbeeldgegevens. Ook informatie over het verbinden met clientprogramma's, gebruikers configureren en instellen van een firewallregel database. |
| 21 | [Azure SQL-Database wordt beveiligd en beschermt](sql-database-helps-secures-and-protects.md) | Informatie over hoe SQL-Database helpt beveiligen en beschermen |
| 22 | [Azure SQL-Database wordt &amp; aangepast](sql-database-learn-and-adapt.md) | Meer informatie over hoe SQL-Database wordt, en past zich |
| 23 | [Kies een wolk van SQL Server option: Azure SQL (PaaS)-Database of SQL Server op Azure VMs (IaaS)](sql-database-paas-vs-sql-server-iaas.md) | Informatie over welke optie u SQL Server cloud past bij uw toepassing: Azure SQL (PaaS)-Database of SQL Server in de cloud Azure virtuele Machines. |
| 24 | [Azure SQL Database schalen onderweg](sql-database-scale-on-the-fly.md) | Meer informatie over hoe SQL-Database wordt aangepast tijdens het typen |
| 25 | [Ontdek de Azure SQL Database oplossing snel aan de slag](sql-database-solution-quick-starts.md) | Meer informatie over Azure SQL Database-oplossingen |
| 26 | [Azure SQL-Database werkt in uw omgeving](sql-database-works-in-your-environment.md) | Informatie over hoe SQL-Database helpt, worden beveiligd en beschermt |



## <a name="build-an-app"></a>Bouwen van een app

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 27 | [Problemen oplossen, diagnose en voorkomen van fouten in SQL-verbinding en tijdelijke voor SQL-Database](sql-database-connectivity-issues.md) | Informatie over het oplossen van problemen, opsporen en voorkomen dat een tijdelijke fout in Azure SQL-Database of een SQL-verbindingsfout.  |
| 28 | [Verbinding maken met een SQL-Database met Visual Studio](sql-database-connect-query.md) | Een programma geschreven in C# en verbinding maken met een SQL-database. Info over IP-adressen, verbindingsreeksen beveiligde aanmelding en vrije Visual Studio. |
| 29 | [Behalve 1433 voor ADO.NET 4.5 en SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md) | Verbindingen van ADO.NET met Azure SQL Database V12 soms de proxyserver wordt overgeslagen en rechtstreeks communiceren met de database. Poorten dan 1433 steeds belangrijker. |
| 30 | [SQL-foutcodes voor SQL-Database-clienttoepassingen: verbindingsfout en andere problemen](sql-database-develop-error-messages.md) | Meer informatie over de foutcodes van de SQL voor SQL-Database-clienttoepassingen, zoals algemene database verbindingsfouten, problemen met de database kopiëren en algemene fouten.  |
| 31 | [Verbinding maken met de SQL-Database met behulp van PHP op Windows](sql-database-develop-php-simple.md) | Geeft een voorbeeld PHP-programma dat verbinding maakt met Azure SQL-Database van een Windows-client en koppelingen naar de benodigde software-onderdelen die nodig zijn voor de client. |
| 32 | [Voer een SQL-Database: Gebruik C# een SQL-database maken met de SQL-Database bibliotheek voor .NET](sql-database-get-started-csharp.md) | Probeer SQL-Database voor het ontwikkelen van apps in SQL en C# en Azure SQL-Database maken met C# met behulp van de SQL-Database bibliotheek voor .NET. |
| 33 | [Aan de slag met de functies van JSON in Azure SQL-Database](sql-database-json-features.md) | Azure SQL-Database kan parseren, een query of gegevens opmaken in de notatie JavaScript Object Notation (JSON). |
| 34 | [Problemen met verbinding met Azure SQL-Database](sql-database-troubleshoot-common-connection-issues.md) | Stappen voor het identificeren en oplossen van veelvoorkomende fouten voor Azure SQL-Database. |
| 35 | [Fout 'De Database op de server is niet beschikbaar' wanneer u verbinding maakt met sql-database](sql-database-troubleshoot-connection.md) | Problemen oplossen met de database op server is niet beschikbaar fout wanneer een toepassing een verbinding met de SQL-Database. |



## <a name="develop"></a>Ontwikkelen

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 36 | [De vereiste waarden voor het verifiëren van een toepassing naar een SQL-Database openen vanuit code ophalen](sql-database-client-id-keys.md) | Een service principal voor SQL-Database openen vanuit code maken. |
| 37 | [Verbinding maken met de SQL-Database met behulp van Java met JDBC in Windows](sql-database-develop-java-simple.md) | Geeft een voorbeeld van Java-code die kunt u verbinding maken met Azure SQL-Database. In dit voorbeeld worden de JDBC en wordt uitgevoerd op een Windows-clientcomputer. |
| 38 | [Verbinding maken met de SQL-Database via Node.js](sql-database-develop-nodejs-simple.md) | Bevat een codevoorbeeld van Node.js die kunt u verbinding maken met Azure SQL-Database. |
| 39 | [Overzicht SQL Database-ontwikkeling](sql-database-develop-overview.md) | Meer informatie over de beschikbare verbindingen bibliotheken en aanbevolen procedures voor toepassingen die verbinding maken met een SQL-Database. |
| 40 | [Verbinding maken met de SQL-Database met behulp van Python](sql-database-develop-python-simple.md) | Geeft een voorbeeld van Python code die kunt u verbinding maken met Azure SQL-Database. |
| 41 | [Verbinding maken met de SQL-Database met behulp van Ruby](sql-database-develop-ruby-simple.md) | Geef een Ruby voorbeeldcode die u uitvoeren kunt om verbinding te maken met Azure SQL-Database. |
| 42 | [Aan de slag met tijdelijke tabellen in Azure SQL-Database](sql-database-temporal-tables.md) | Informatie over hoe te beginnen met het gebruik van tijdelijke tabellen in Azure SQL-Database. |



## <a name="performance-and-scale"></a>Prestaties en schaal

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 43 | [SQL-Database-adviseur](sql-database-advisor.md) | De Advisor Azure SQL Database bevat aanbevelingen voor uw bestaande SQL-Databases die de huidige prestaties van query's kunt verbeteren. |
| 44 | [SQL Database-adviseur met de Azure portal](sql-database-advisor-portal.md) | Kunt u de Advisor Azure SQL-Database in de Azure portal te controleren en de implementatie van de aanbevelingen voor uw bestaande SQL-Databases die de huidige prestaties van query's kunt verbeteren. |
| 45 | [Overzicht van de benchmark Azure SQL-Database](sql-database-benchmark-overview.md) | Dit onderwerp beschrijft de Benchmark Azure SQL-Database die wordt gebruikt bij de waardering van de prestaties van Azure SQL-Database. |
| 46 | [Wanneer moet een elastische database-toepassingen worden gebruikt?](sql-database-elastic-pool-guidance.md) | Een groep elastische database is een verzameling van beschikbare bronnen die worden gedeeld door een groep van elastische databases. Dit document bevat richtlijnen voor het beoordelen van de geschiktheid van het gebruik van een elastische database toepassingen voor een groep van databases. |
| 47 | [Aan de slag met elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md) | Eenvoudige uitleg van elastische database extra functie van Azure SQL-Database, inclusief eenvoudig voorbeeld app uitvoeren. |
| 48 | [Veelgestelde vragen over SQL-Database](sql-database-faq.md) | Antwoorden op algemene vragen klanten vragen over cloud databases en Azure SQL-Database, Microsoft relationeel databasebeheersysteem (RDBMS) en de database als een service in de cloud. |
| 49 | [Aan de slag met In het geheugen (voorbeeld) in de SQL-Database](sql-database-in-memory.md) | Technologieën voor SQL In het geheugen verbeteren aanzienlijk de prestaties van transactionele en analytics werkbelasting. Informatie over het gebruik van deze technologieën. |
| 50 | [Gebruik In Memory OLTP (voorbeeld) voor het verbeteren van de toepassingsprestaties van uw in SQL-Database](sql-database-in-memory-oltp-migration.md) | Hanteer In Memory OLTP transactionele prestaties in een bestaande SQL-database te verbeteren. |
| 51 | [Monitor In Memory OLTP-opslag](sql-database-in-memory-oltp-monitoring.md) | Raming en XTP geheugenopslag monitor gebruikt, capaciteit; capaciteit fout 41823 oplossen |
| 52 | [De Query-winkel in Azure SQL-Database](sql-database-operate-query-store.md) | Informatie over het werken in Azure SQL-Database het Query-archief |
| 53 | [SQL Database prestaties inzicht](sql-database-performance.md) | De Azure SQL-Database biedt hulpmiddelen waarmee u gebieden herkennen die in de huidige queryprestaties kunnen verbeteren. |
| 54 | [Azure SQL-Database en prestaties voor enkele databases](sql-database-performance-guidance.md) | In dit artikel kunt u vaststellen welke servicelaag om te kiezen voor uw toepassing. Het adviseert ook manieren om uw toepassing om optimaal te profiteren van Azure SQL-Database optimaliseren. |
| 55 | [Azure SQL Database Query prestaties inzicht](sql-database-query-performance.md) | Prestaties controleren in query geeft u de meeste CPU-gebruik van query's voor Azure SQL-Database. |
| 56 | [De laag en prestaties serviceniveau (prijzen laag) van een SQL-database wijzigen](sql-database-scale-up.md) | Wijzig de servicelaag en prestatieniveau van Azure SQL-database ziet u hoe u de SQL-database geschaald omhoog of omlaag. Het wijzigen van de prijzen laag van een Azure SQL-database. |
| 57 | [Controleren van de databaseprestaties in Azure SQL-Database](sql-database-single-database-monitor.md) | Meer informatie over de opties voor het controleren van uw database met Azure's en het beheer van dynamische weergaven. |
| 58 | [SQL-Database performance tuning tips](sql-database-troubleshoot-performance.md) | Tips voor het afstemmen in Azure SQL-Database via de evaluatie en verbetering van prestaties. |
| 59 | [Batchverwerking gebruiken voor het verbeteren van de prestaties van de SQL-Database](sql-database-use-batching-to-improve-performance.md) | Het onderwerp het bewijs levert dat batchen databasebewerkingen aanzienlijk imroves de snelheid en de schaalbaarheid van uw toepassingen Azure SQL-Database. Hoewel deze batchen technieken voor een SQL Server-database, wordt de focus van het artikel op Azure. |



## <a name="tools-for-scaling-out"></a>Hulpprogramma's voor het horizontaal schalen

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 60 | [Patronen voor multitenant SaaS-toepassingen en Azure SQL Database ontwerpen](sql-database-design-patterns-multi-tenancy-saas-applications.md) | Dit artikel worden de vereisten en algemene gegevens architectuur patronen van multitenant database moeten rekening houden met toepassingen die worden uitgevoerd in een cloud-omgeving en de verschillende optimalisatie die zijn gekoppeld aan deze patronen. Ook wordt uitgelegd hoe Azure SQL-Database, met een elastische van toepassingen en hulpmiddelen voor elastische helpen deze eisen op een manier zonder adres. |
| 61 | [Migreren van bestaande databases te schalen](sql-database-elastic-convert-to-use-elastic-tools.md) | Een laptopgeheugen databases een shard kaart manager maken gebruik van elastische Databasehulpprogramma's converteren |
| 62 | [Bouwen van schaalbare cloud-databases](sql-database-elastic-database-client-library.md) | Bouwen van schaalbare .NET database apps met de elastische database client library |
| 63 | [Prestatiemeteritems voor shard manager toewijzen](sql-database-elastic-database-perf-counters.md) | ShardMapManager klasse- en afhankelijke routering prestatiemeteritems |
| 64 | [Een shard met elastische Databasehulpprogramma's toe te voegen](sql-database-elastic-scale-add-a-shard.md) | Het gebruik van de elastische schaal API's nieuwe shards toevoegen aan een shard instellen. |
| 65 | [Een gesplitste samenvoegen service implementeren](sql-database-elastic-scale-configure-deploy-split-and-merge.md) | Splitsen en samenvoegen met elastische Databasehulpmiddelen |
| 66 | [Gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) | Het gebruik van de klasse ShardMapManager in .NET-toepassingen voor het gegevensafhankelijke routering, een functie van elastische databases voor Azure SQL-Database |
| 67 | [Elastische Databasehulpmiddelen Veelgestelde vragen](sql-database-elastic-scale-faq.md) | Veelgestelde vragen over Azure SQL Database elastische schaal. |
| 68 | [Elastische Database tools-verklarende woordenlijst](sql-database-elastic-scale-glossary.md) | Uitleg van termen die worden gebruikt voor elastische Databasehulpmiddelen |
| 69 | [Geschaald uitbreiden met Azure SQL-Database](sql-database-elastic-scale-introduction.md) | Software als een Service (SaaS)-ontwikkelaars gemakkelijk elastisch, schaalbare databases kunt maken in de cloud met deze hulpmiddelen |
| 70 | [Referenties die worden gebruikt voor toegang tot de elastische Database client library](sql-database-elastic-scale-manage-credentials.md) | Het vereiste niveau van de referenties van een beheerder voor apps elastische database alleen-lezen instellen |
| 71 | [Bij het controleren van meerdere shard](sql-database-elastic-scale-multishard-querying.md) | Query's uitvoeren in shards met behulp van de elastische database client library. |
| 72 | [Verplaatsen van gegevens tussen databases schaalvergroting cloud](sql-database-elastic-scale-overview-split-and-merge.md) | Hoe shards bewerken en verplaatsen van gegevens via een zelf gehoste service elastische database API's gebruikt. |
| 73 | [Schaal van databases met de shard kaart manager](sql-database-elastic-scale-shard-map-management.md) | Het gebruik van de ShardMapManager, elastische database client library |
| 74 | [Gesplitste samenvoegen Beveiligingsconfiguratie](sql-database-elastic-scale-split-merge-security-configuration.md) | Instellen van x409 certificaten voor codering |
| 75 | [Een app voor het gebruik van de meest recente client-bibliotheek van elastische database bijwerken](sql-database-elastic-scale-upgrade-client-library.md) | Apps en bibliotheken met Nuget upgraden |
| 76 | [Elastische Database client-bibliotheek met entiteit-Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) | Elastische Database client-bibliotheek en entiteit Framework gebruiken voor het coderen van databases |
| 77 | [Elastische database client library gebruiken met Dapper](sql-database-elastic-scale-working-with-dapper.md) | Met behulp van elastische database client-bibliotheek met Dapper. |
| 78 | [Toepassingen met meerdere huurder met elastische Databasehulpmiddelen en beveiliging](sql-database-elastic-tools-multi-tenant-row-level-security.md) | Informatie over het gebruik van extra elastische database met beveiliging op een toepassing met een zeer schaalbare gegevenslaag maken op Azure SQL databases die met meerdere huurder shards. |



## <a name="elastic-jobs"></a>Elastische taken

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 79 | [Maken en beheren van geschaalde van Azure SQL-Databases met behulp van elastische taken (voorbeeld)](sql-database-elastic-jobs-create-and-manage.md) | Maken en beheren van een taak elastische database doorlopen. |
| 80 | [Aan de slag met elastische Database taken](sql-database-elastic-jobs-getting-started.md) | het gebruik van de elastische database taken |
| 81 | [Schaalvergroting wolk databases beheren](sql-database-elastic-jobs-overview.md) | De elastische databaseservice taak ziet u |
| 82 | [Maken en beheren van een SQL-Database database elastische taken met PowerShell (voorbeeld)](sql-database-elastic-jobs-powershell.md) | PowerShell is gebruikt voor het beheren van toepassingen Azure SQL-Database |
| 83 | [Overzicht van elastische Database installeren](sql-database-elastic-jobs-service-installation.md) | Installatie van de functie elastische project doorlopen. |
| 84 | [Elastische databaseonderdelen van taken verwijderen](sql-database-elastic-jobs-uninstall.md) | Hoe u het Databasehulpprogramma elastische taken verwijderen |



## <a name="elastic-pools"></a>Elastische pools

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 85 | [Wat is een elastische Azure toepassingen?](sql-database-elastic-pool.md) | Honderden of duizenden databases met behulp van een groep beheren. Een prijs voor een aantal prestatie-eenheden kan worden verdeeld over de groep. Verplaats de databases in- of uitzoomen op wordt. |
| 86 | [Maak een elastische database toepassingen met C#](sql-database-elastic-pool-create-csharp.md) | Technieken van C#-database gebruik te maken van een schaalbare elastische database toepassingen in Azure SQL-Database, zodat u resources veel databases delen kunt. |
| 87 | [Maak een nieuwe database elastische toepassingen met PowerShell](sql-database-elastic-pool-create-powershell.md) | Informatie over het gebruik van PowerShell naar Azure SQL-Database geschaald uitbreiden bronnen op een schaalbare elastische database-toepassingen voor het beheren van meerdere databases maken. |
| 88 | [PowerShell script voor het identificeren van databases die geschikt zijn voor een elastische database toepassingen](sql-database-elastic-pool-database-assessment-powershell.md) | Een groep elastische database is een verzameling van beschikbare bronnen die worden gedeeld door een groep van elastische databases. Dit document bevat een Powershell script voor het beoordelen van de geschiktheid van het gebruik van een elastische database toepassingen voor een groep van databases. |
| 89 | [Bewaken en beheren van een elastische database toepassingen met C#](sql-database-elastic-pool-manage-csharp.md) | C#-database ontwikkelingstechnieken voor het beheer van een SQL-Database Azure elastische database toepassingen gebruiken. |
| 90 | [Bewaken en beheren van een elastische database toepassingen met Azure portal](sql-database-elastic-pool-manage-portal.md) | Informatie over het gebruik van de Azure portal en SQL-Database ingebouwde intelligentie te beheren, bewaken en breedte een schaalbare elastische database toepassingen om databaseprestaties te optimaliseren en kosten te beheren. |
| 91 | [Bewaken en beheren van een elastische database toepassingen met PowerShell](sql-database-elastic-pool-manage-powershell.md) | Leer een elastische database toepassingen beheren met PowerShell. |
| 92 | [Bewaken en beheren van een elastische database toepassingen met Transact-SQL](sql-database-elastic-pool-manage-tsql.md) | T-SQL Azure SQL-database maken in een elastische toepassingen gebruiken. Of T-SQL in de datbase en toepassingen uit. |
| 93 | [Elastische database toepassingen facturerings- en prijsinformatie](sql-database-elastic-pool-price.md) | Prijsinformatie voor elastische database van toepassingen. |



## <a name="elastic-query"></a>Elastische query

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 94 | [Rapport over schaalvergroting wolk databases (voorbeeld)](sql-database-elastic-query-getting-started.md) | cross-database databasequery's gebruiken |
| 95 | [Aan de slag met cross-databasequery's (verticale partities) (voorbeeld)](sql-database-elastic-query-getting-started-vertical.md) | het gebruik van de elastische databasequery met verticaal gepartitioneerde databases |
| 96 | [Rapportage over schaalvergroting wolk databases (voorbeeld)](sql-database-elastic-query-horizontal-partitioning.md) | elastische query's via horizontale partities instellen |
| 97 | [Azure SQL-Database elastische database query overzicht (voorbeeld)](sql-database-elastic-query-overview.md) | Overzicht van de functie query elastisch |
| 98 | [Vragen over cloud-databases met verschillende schema's (voorbeeld)](sql-database-elastic-query-vertical-partitioning.md) | het instellen van cross-databasequery's via verticale partities |



## <a name="elastic-transaction"></a>Elastische transactie

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 99 | [Gedistribueerde transacties via cloud-databases](sql-database-elastic-transactions-overview.md) | Overzicht van de elastische databasetransacties met Azure SQL-Database |



## <a name="backup-and-recovery"></a>Back-up en herstel

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 100 | [Back-ups van SQL-Database](sql-database-automated-backups.md) | Meer informatie over SQL-Database ingebouwde back-ups die u wilt samenvouwen Azure SQL-Database terug naar een vorig punt in de tijd of een database kopiëren naar een nieuwe database in een geografische regio (maximaal 35 dagen). |
| 101 | [Overzicht van de bedrijfscontinuïteit met Azure SQL-Database](sql-database-business-continuity.md) | Ontdek hoe Azure SQL-Database ondersteunt wolk bedrijfscontinuïteit en herstellen van databases en kunt u belangrijke cloud-toepassingen die worden uitgevoerd. |
| 102 | [Een enkele tabel uit een Azure SQL Database back-up terugzetten](sql-database-cloud-migrate-restore-single-table-azure-backup.md) | Informatie over één tabel Azure SQL Database back-up terugzetten. |
| 103 | [Een toepassing voor noodherstel wolk met actieve Geo-replicatie in SQL-Database](sql-database-designing-cloud-solutions-for-disaster-recovery.md) | Informatie over het ontwerpen van de wolk disaster recovery oplossingen voor business continuity planning met behulp van geografische replicatie voor app gegevens back-up met Azure SQL-Database. |
| 104 | [Een Azure SQL-Database of een failover herstellen naar een secundair](sql-database-disaster-recovery.md) | Informatie over het herstellen van een database van een storing in regionale datacenter of storing Geo-herstel-mogelijkheden en de Azure SQL-Database actief Geo-replicatie. |
| 105 | [Disaster Recovery analyse uitvoeren](sql-database-disaster-recovery-drills.md) | Informatie over richtlijnen en aanbevolen procedures voor het uitvoeren van disaster recovery oefeningen die met Azure SQL Database houden uw missie essentiële bedrijfstoepassingen veerkrachtig storingen en storingen. |
| 106 | [Disaster recovery strategieën voor toepassingen met behulp van SQL Database elastische Pool](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) | Informatie over het kiezen van de juiste failover-patroon voor het ontwerpen van uw cloud oplossing voor noodherstel. |
| 107 | [Met behulp van de klasse RecoveryManager shard kaart problemen op te lossen](sql-database-elastic-database-recovery-manager.md) | De klasse RecoveryManager gebruiken bij het oplossen van problemen met shard kaarten |
| 108 | [Een Bacpac-bestand om een Azure SQL-database importeren](sql-database-import.md) | Azure SQL-database maken door een bestaande Bacpac-bestand te importeren. |
| 109 | [Azure SQL-Database herstellen naar een vorig punt in de tijd met Azure Portal](sql-database-point-in-time-restore-portal.md) | Azure SQL-Database herstellen naar een vorig punt in de tijd. |
| 110 | [Azure SQL-Database herstellen naar een vorig punt in de tijd met PowerShell](sql-database-point-in-time-restore-powershell.md) | Azure SQL-Database herstellen naar een vorig punt in de tijd |
| 112 | [Azure SQL-database met behulp van geautomatiseerde back-ups herstellen](sql-database-recovery-using-backups.md) | Informatie over het terugzetten van Point-in-Time, waarmee u kunt terugdraaien Azure SQL-Database naar een vorig punt in tijd (maximaal 35 dagen). |
| 113 | [Een verwijderde Azure SQL-database via de Portal Azure terugzetten](sql-database-restore-deleted-database-portal.md) | Herstel verwijderde Azure SQL-database (Azure Portal). |
| 114 | [Een verwijderde Azure SQL-Database herstellen via een PowerShell](sql-database-restore-deleted-database-powershell.md) | Een verwijderde Azure SQL Database terugzetten (PowerShell). |
| 115 | [Een database herstellen naar een vorig punt in de tijd, een verwijderde database terugzetten of herstellen na een stroomstoring gegevens-center](sql-database-troubleshoot-backup-and-restore.md) | Informatie over het herstellen van een database wolk van fouten en storingen met behulp van back-ups en replica's in Azure SQL-Database. |



## <a name="migrate"></a>Migreren

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 116 | [Een SQL Server-database exporteren naar een Bacpac-bestand met behulp van SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | Microsoft Azure SQL-Database, databasemigratie, database exporteren, sqlpackage-Bacpac-bestand exporteren |
| 117 | [Een SQL Server-database exporteren naar een Bacpac-bestand met behulp van SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | Microsoft Azure SQL-Database, databasemigratie, database exporteren, wizard exporteren gegevens Tier toepassing Bacpac-bestand exporteren |
| 118 | [In de SQL-Database importeren vanuit een Bacpac-bestand met behulp van SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md) | Microsoft Azure SQL-Database, databasemigratie, database importeren, Bacpac-bestand importeert, sqlpackage |
| 119 | [SQL-database met behulp van SSMS van BACPAC importeren](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | Microsoft Azure SQL Database database implementeren, databasemigratie, database importeren, wizard migreren-database exporteren |
| 120 | [SQL Server-database naar Microsoft Azure Database Wizard Database implementeren met SQL-Database migreren](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) | Microsoft Azure SQL-Database, databasemigratie, Microsoft Azure Wizard Database |
| 121 | [SQL Server-database migreren naar Azure SQL-Database met behulp van transactionele replicatie](sql-database-cloud-migrate-compatible-using-transactional-replication.md) | Microsoft Azure SQL-Database, databasemigratie, database, importeert transactionele replicatie |
| 122 | [SQL-Database compatibiliteit met SqlPackage.exe bepalen](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md) | Microsoft Azure SQL-Database, databasemigratie, compatibiliteit met SQL-Database SqlPackage |
| 123 | [Gebruik SQL Server Management Studio compatibiliteit voor de migratie naar Azure SQL-Database SQL-Database bepalen](sql-database-cloud-migrate-determine-compatibility-ssms.md) | Microsoft Azure SQL-Database, databasemigratie, compatibiliteit met SQL-Database gegevens Tier toepassing Wizard exporteren |
| 124 | [Gebruik SQL Azure Migration Wizard voor herstellen SQL Server-database compatibiliteitsproblemen voor de migratie naar Azure SQL-Database](sql-database-cloud-migrate-fix-compatibility-issues.md) | Microsoft Azure SQL-Database, databasemigratie, compatibiliteit met SQL Azure Migration Wizard |
| 125 | [Een SQL Server-Database migreren naar Azure SQL-Database met behulp van SQL Server Data Tools voor Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) | Microsoft Azure SQL-Database, databasemigratie, compatibiliteit, SQL Azure Migration Wizard, SSDT |
| 126 | [Compatibiliteitsproblemen voor SQL Server-database met behulp van SQL Server Management Studio voor de migratie naar een SQL-Database oplossen](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) | Microsoft Azure SQL-Database, databasemigratie, compatibiliteit met SQL Azure Migration Wizard |
| 127 | [Azure SQL-database naar een bestand Bacpac-archiveren met PowerShell](sql-database-export-powershell.md) | Azure SQL-database naar een bestand Bacpac-archiveren met PowerShell |
| 128 | [Een Azure SQL-database maken met behulp van PowerShell Bacpac-bestanden importeren](sql-database-import-powershell.md) | Een Azure SQL-database maken met behulp van PowerShell Bacpac-bestanden importeren |
| 129 | [Gegevens laden vanuit CSV in Azure SQL Data Warehouse (bestanden)](sql-database-load-from-csv-with-bcp.md) | Gebruikt voor een kleine gegevensgrootte bcp om gegevens te importeren in Azure SQL-Database. |
| 130 | [Databases verplaatsen tussen servers, tussen abonnementen en in Azure](sql-database-troubleshoot-moving-data.md) | Snelle stappen te kopiëren, verplaatsen en gegevens en databases in Azure SQL-Database migreren. |



## <a name="move-data-in-and-out"></a>Gegevens verplaatsen in en uit

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 131 | [SQL Server-databasemigratie naar de SQL-Database in de cloud](sql-database-cloud-migrate.md) | Informatie over hoe op ruimten SQL Server-databasemigratie naar Azure SQL-Database in de cloud. Hulpprogramma's voor migratie gebruiken voor het testen van compatibiliteit voorafgaand aan de databasemigratie. |
| 132 | [Azure SQL-Database kopiëren](sql-database-copy.md) | Maak een kopie van een Azure SQL-database |
| 133 | [Azure SQL-database naar een Bacpac-bestand met behulp van de Portal Azure archiveren](sql-database-export.md) | Azure SQL-database naar een Bacpac-bestand met behulp van de Portal Azure archiveren |



## <a name="security"></a>Beveiliging

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 134 | [Verbinding maken met de SQL-Database of SQL datawarehouse met Azure Active Directory-verificatie](sql-database-aad-authentication.md) | Informatie over hoe u verbinding maakt met de SQL-Database via Azure Active Directory-verificatie. |
| 135 | [Altijd gecodeerd: Bescherming van gevoelige gegevens in een SQL-Database en uw coderingssleutels worden opgeslagen in het Windows-certificaatarchief](sql-database-always-encrypted.md) | Beschermen van vertrouwelijke gegevens in uw SQL-database in minuten. |
| 136 | [Altijd gecodeerd: Bescherming van gevoelige gegevens in een SQL-Database en opslaan van uw coderingssleutels in Azure sleutel kluis](sql-database-always-encrypted-azure-key-vault.md) | Beschermen van vertrouwelijke gegevens in uw SQL-database in minuten. |
| 137 | [SQL-Database - ondersteuning voor Downlevel-clients en IP-eindpunt wijzigingen voor controle](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md) | Informatie over ondersteuning voor SQL-Database voor downlevel-clients en IP-eindpunt wijzigingen voor controle. |
| 138 | [Azure SQL Database server niveau firewallregels configureren met behulp van PowerShell](sql-database-configure-firewall-settings-powershell.md) | Informatie over het configureren van de firewall voor IP-adressen die toegang hebben tot Azure SQL-databases. |
| 139 | [Azure SQL Database server niveau firewall-regels met de REST API configureren](sql-database-configure-firewall-settings-rest.md) | Informatie over het configureren van de firewall voor IP-adressen die toegang hebben tot Azure SQL-databases. |
| 140 | [Azure SQL Database server- en database-niveau firewallregels met T-SQL configureren](sql-database-configure-firewall-settings-tsql.md) | Informatie over het configureren van de firewall voor IP-adressen die toegang hebben tot Azure SQL-databases. |
| 141 | [Aan de slag met SQL Database dynamische gegevens maskeren (Azure Portal)](sql-database-dynamic-data-masking-get-started.md) | Om te beginnen met de SQL-Database dynamische gegevens maskeren in Azure Portal |
| 142 | [Aan de slag met SQL Database dynamische gegevens maskeren (Azure klassieke Portal)](sql-database-dynamic-data-masking-get-started-portal.md) | Om te beginnen met de SQL-Database dynamische gegevens maskeren in de klassieke Azure-Portal |
| 143 | [Azure SQL Database firewallregels configureren \- overzicht](sql-database-firewall-configure.md) | Een SQL-database-firewall configureren met server- en database-niveau firewall-regels voor het beheren van toegang tot informatie. |
| 144 | [Zelfstudie voor SQL-Database: gebruikersaccounts te openen en te beheren, een database van SQL-database maken](sql-database-get-started-security.md) | Informatie over het maken van gebruikersaccounts te openen en om een database te beheren. |
| 145 | [SQL-Database-verificatie en autorisatie: toegang verlenen](sql-database-manage-logins.md) | Meer informatie over het beheer van SQL-Database beveiliging, specifiek het beheren van toegang en aanmelding beveiliging van de database via de principal-niveau van de server-account. |
| 146 | [Richtlijnen voor beveiliging van Azure SQL-Database en beperkingen](sql-database-security-guidelines.md) | Meer informatie over Microsoft Azure SQL-Database-richtlijnen en beperkingen die betrekking hebben op beveiliging. |
| 147 | [Aan de slag met SQL Database dreiging detectie](sql-database-threat-detection-get-started.md) | Hoe aan de slag met SQL Database dreiging detectie in Azure Portal |
| 148 | [Het uitvoeren van veelvoorkomende beheertaken, zoals het opnieuw instellen van wachtwoord admin in Azure SQL-Database](sql-database-troubleshoot-permissions.md) | Wordt beschreven hoe u algemene beheertaken uitvoert in een SQL-Database. Bijvoorbeeld: admin-wachtwoord opnieuw instellen, verlenen en toegang verwijderen. |



## <a name="manage-your-database"></a>Beheer van uw database

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 149 | [Aan de slag met SQL-database controleren](sql-database-auditing-get-started.md) | Aan de slag met SQL-database controleren |
| 150 | [Een Azure SQL Database server niveau firewallregel met behulp van de Portal Azure configureren](sql-database-configure-firewall-settings.md) | Informatie over het configureren van de firewall voor IP-adressen die toegang hebben tot Azure SQL server. |
| 151 | [Azure SQL-Database via de portal Azure kopiëren](sql-database-copy-portal.md) | Maak een kopie van een Azure SQL-database |
| 152 | [Azure Azure automatisering met SQL-Databases beheren](sql-database-manage-automation.md) | Meer informatie over hoe de automatisering Azure-service kan worden gebruikt voor het beheren van Azure SQL-databases op een schaal. |
| 153 | [Overzicht: hulpprogramma's voor SQL-Database](sql-database-manage-overview.md) | Vergelijkt de hulpprogramma's en opties voor het beheren van Azure SQL-Database |
| 154 | [Azure SQL-Database met behulp van beheer van dynamische weergaven controleren](sql-database-monitoring-with-dmvs.md) | Informatie over het opsporen en onderzoeken van algemene prestatieproblemen met behulp van beheer van dynamische weergaven voor het controleren van Microsoft Azure SQL-Database. |
| 155 | [De SQL-Database beveiligen](sql-database-security.md) | Informatie over Azure SQL-Database en SQL Server-beveiliging, met inbegrip van de verschillen tussen de wolk en SQL Server op-ruimten als het gaat om verificatie, machtiging, beveiliging van verbindingen, codering en conformiteit. |



## <a name="extended-events"></a>Uitgebreide gebeurtenissen

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 156 | [Bestand doel gebeurteniscode voor uitgebreide gebeurtenissen in de SQL-Database](sql-database-xevent-code-event-file.md) | PowerShell en Transact-SQL biedt voor een in twee fasen codevoorbeeld dat het doel van de gebeurtenis-bestand in een uitgebreide gebeurtenis op Azure SQL-Database toont. Azure opslag is een noodzakelijk onderdeel van dit scenario. |
| 157 | [Ring Buffer doel code voor uitgebreide gebeurtenissen in de SQL-Database](sql-database-xevent-code-ring-buffer.md) | Bevat een codevoorbeeld Transact-SQL die eenvoudig en snel door middel van het doel Ring Buffer in Azure SQL-Database is gemaakt. |
| 158 | [Uitgebreide gebeurtenissen in de SQL-Database](sql-database-xevent-db-diff-from-svr.md) | Beschrijving van uitgebreide gebeurtenissen (XEvents) in Azure SQL-Database en hoe gebeurtenis sessies enigszins verschillen van de gebeurtenis sessies in Microsoft SQL Server. |



## <a name="geo-replication"></a>Geo-replicatie

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 159 | [Een failover of niet gepland voor Azure SQL-Database tot stand brengen met de Azure portal](sql-database-geo-replication-failover-portal.md) | Het initiëren van failover of niet gepland voor Azure SQL-Database via de portal Azure |
| 160 | [Het initiëren van failover of niet gepland voor Azure SQL-Database met PowerShell](sql-database-geo-replication-failover-powershell.md) | Het initiëren van failover of niet gepland voor Azure PowerShell met SQL-Database |
| 161 | [Het initiëren van failover of niet gepland voor Azure SQL-Database met Transact-SQL](sql-database-geo-replication-failover-transact-sql.md) | Het initiëren van failover of niet gepland voor Azure SQL-Database met behulp van Transact-SQL |
| 162 | [Overzicht: SQL-Database actief Geo-replicatie](sql-database-geo-replication-overview.md) | Actieve Geo-replicatie kunt u setup-4 replica's van de database in een van de Azure datacenters. |
| 163 | [Geo-replicatie configureren voor Azure SQL-Database met Azure portal](sql-database-geo-replication-portal.md) | Geo-replicatie configureren voor Azure SQL-Database via de portal Azure |
| 164 | [Geo-replicatie configureren voor Azure SQL-Database met PowerShell](sql-database-geo-replication-powershell.md) | Actieve Geo-replicatie configureren voor Azure PowerShell met SQL-Database |
| 165 | [Het beheren van beveiliging Azure SQL-Database na noodherstel](sql-database-geo-replication-security-config.md) | Dit onderwerp worden beveiligingsoverwegingen voor het beheren van beveiliging na het terugzetten van een database of een failover. |
| 166 | [Geo-replicatie configureren voor Azure SQL-Database met Transact-SQL](sql-database-geo-replication-transact-sql.md) | Geo-replicatie configureren voor Azure SQL-Database met behulp van Transact-SQL |
| 167 | [Geo-Restore Azure SQL-Database van een geo-redundante back-up met behulp van de Portal Azure](sql-database-geo-restore-portal.md) | Geo-Restore Azure SQL-Database van een geo-redundante back-up (Azure Portal). |
| 168 | [Azure SQL-Database van een geo-redundante back-up terugzetten met PowerShell](sql-database-geo-restore-powershell.md) | Azure SQL-Database naar een nieuwe server van een geo-redundante back-up terugzetten |



## <a name="upgrade"></a>Upgrade

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 169 | [Verbeterde prestaties van query's en compatibiliteit niveau 130 in Azure SQL-Database](sql-database-compatibility-level-query-performance-130.md) | Stappen en hulpmiddelen om te bepalen welk compatibiliteitsniveau is het meest geschikt voor uw op Azure SQL-Database of Microsoft SQL Server-database |
| 170 | [Rolling upgrades van cloud-toepassingen met behulp van SQL Database actief Geo-replicatie beheren](sql-database-manage-application-rolling-upgrade.md) | Informatie over het geo Azure SQL-Database-replicatie gebruiken ter ondersteuning van online upgrades van de wolk toepassing. |
| 171 | [Upgrade naar Azure SQL Database V12 met de Azure portal](sql-database-upgrade-server-portal.md) | Wordt uitgelegd hoe u een upgrade uitvoeren naar Azure SQL Database V12 met inbegrip van Web- en databases bijwerken en het upgraden van een V11: server migreren van de databases rechtstreeks in een elastische database-toepassingen met behulp van de portal Azure. |
| 172 | [Upgrade naar Azure SQL Database V12 met PowerShell](sql-database-upgrade-server-powershell.md) | Wordt uitgelegd hoe u een upgrade uitvoeren naar Azure SQL Database V12 met inbegrip van het bijwerken van Web- en databases en het upgraden van een V11: server migreren van de databases rechtstreeks in een elastische database toepassingen met PowerShell. |
| 173 | [Plannen en voorbereiden voor een upgrade naar SQL Database V12](sql-database-v12-plan-prepare-upgrade.md) | Beschrijving van de voorbereidingen en de beperkingen die zijn betrokken bij een upgrade naar versie V12 van Azure SQL-Database. |
| 174 | [Nieuwe functies in een SQL-Database V12](sql-database-v12-whats-new.md) | Hierin wordt beschreven waarom bedrijfssystemen Azure SQL-Database gebruikt in de cloud door te upgraden naar versie V12 nu profiteren. |
| 175 | [Web- en Business Edition zonsondergang Veelgestelde vragen](sql-database-web-business-sunset-faq.md) | Meer informatie over wanneer de Azure SQL Web- en databases wordt ingetrokken en meer informatie over de functies en functionaliteit van de nieuwe service niveaus. |



## <a name="tools"></a>Hulpprogramma 's

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 176 | [Azure SQL-Database met PowerShell beheren](sql-database-command-line-tools.md) | Azure SQL-Database beheren met PowerShell. |
| 177 | [Zelfstudie voor SQL-Database: Excel verbinding met een Azure SQL-database en een rapport maken](sql-database-connect-excel.md) | Informatie over Microsoft Excel verbinding met Azure SQL-database in de cloud. Gegevens importeren in Excel voor rapportage- en exploratie. |
| 178 | [Een SQL-database maken en uitvoeren van gemeenschappelijke databasetaken met PowerShell-cmdlets](sql-database-get-started-powershell.md) | Leer nu een SQL-database maken met PowerShell. Setup taken kunnen worden beheerd via PowerShell-cmdlets. |
| 179 | [Aan de slag met Azure SQL gegevens synchroniseren (voorbeeld)](sql-database-get-started-sql-data-sync.md) | Deze zelfstudie kunt u aan de slag met de Azure SQL Data synchronisatie (voorbeeld). |
| 180 | [Azure SQL-Database met behulp van SQL Server Management Studio beheren](sql-database-manage-azure-ssms.md) | Informatie over het beheren van de SQL-Database servers en databases met SQL Server Management Studio. |
| 181 | [Azure SQL-Databases via de Azure portal beheren](sql-database-manage-portal.md) | Informatie over de Azure Portal gebruiken voor het beheren van een relationele database in de cloud via de Portal Azure. |
| 182 | [De laag en prestaties serviceniveau (prijzen laag) van een SQL-database met PowerShell wijzigen](sql-database-scale-up-powershell.md) | Wijzig de servicelaag en prestatieniveau van Azure SQL-database ziet u hoe u de SQL-database geschaald omhoog of omlaag met PowerShell. Het wijzigen van de prijzen laag van Azure SQL-database met PowerShell. |
| 183 | [Verbinding maken met de SQL-Database via SQL Server Management Studio in Azure RemoteApp-](sql-database-ssms-remoteapp.md) | Deze zelfstudie gebruiken voor informatie over het gebruik van SQL Server Management Studio in Azure RemoteApp voor beveiliging en prestaties wanneer u verbinding maakt met SQL-Database |



## <a name="reference"></a>Referentie

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 184 | [Gegevensverbindingsbibliotheek voor SQL-Database en SQL Server](sql-database-libraries.md) | Geeft het minimale versienummer voor elk stuurprogramma waarmee clientprogramma's kunt verbinding maken met Azure SQL-Database of Microsoft SQL Server. Er wordt een koppeling gegeven voor versie-informatie over stuurprogramma's die door de Gemeenschap en niet door Microsoft zijn uitgebracht. |
| 185 | [Azure SQL-Database resource limieten](sql-database-resource-limits.md) | Deze pagina beschrijft enkele algemene resource limieten voor Azure SQL-Database. |
| 186 | [Verschillen met Azure SQL Database Transact-SQL](sql-database-transact-sql-information.md) | Transact-SQL-instructies die minder dan volledig worden ondersteund in Azure SQL-Database |



## <a name="miscellaneous"></a>Diverse

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 187 | [Met PowerShell Azure SQL-database kopiëren](sql-database-copy-powershell.md) | Kopie van een met PowerShell Azure SQL-database maken |
| 188 | [Kopiëren met behulp van Transact-SQL Azure SQL-database](sql-database-copy-transact-sql.md) | Kopie maken van een Azure SQL-database met behulp van Transact-SQL |
| 189 | [Richtlijnen en beperkingen voor Azure SQL Database-algemeen](sql-database-general-limitations.md) | Deze pagina beschrijft enkele algemene beperkingen voor Azure SQL-Database, alsmede de gebieden van samenwerking en ondersteuning. |
| 190 | [SQL-Database prijzen laag aanbevelingen](sql-database-service-tier-advisor.md) | Bij het wijzigen van prijzen zijn lagen in de prijzen laag aanbevelingen Azure portal mits aanbevelen de laag die het best geschikt is voor het uitvoeren van een bestaande Azure SQL-Database van werkbelasting. Prijzen lagen beschrijven het serviceniveau voor laag en de prestaties van een SQL-database. |


&nbsp;

<hr/>

<a name="extras_append"></a>

## <a name="extras"></a>Extra 's

<a name="extra_related_articles"></a>

### <a name="related-articles-from-other-azure-services"></a>Verwante artikelen van andere Azure services

- [Back-up van uw app Azure App Services in Azure](../app-service-web/web-sites-backup.md)

<a name="extra_documentation_tools"></a>

### <a name="documentation-tools"></a>Extra documentatie

- [Updates aangekondigd voor Azure SQL-Database](http://azure.microsoft.com/updates/?service=sql-database)

- [Alle soorten documentatie van Microsoft Azure, zoeken met filters](http://azure.microsoft.com/search/documentation/)

<a name="extra_learning_path_graphics"></a>

### <a name="learning-path-graphics"></a>Leren werken met grafische pad

- [Leren werken met afbeeldingen voor alle services van Microsoft Azure pad](http://azure.microsoft.com/documentation/learning-paths/)

- [Leren pad: Informatie over Azure SQL-Database](http://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/)

- [Pad leren: Elastisch SQL-Database-schaal](http://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/)


<!--
AzIxAA.ExtraAppend.sql-database.txt
C:\_MainW\VS15\AzureIndexAllArticles\AzureIndexAllArticles\In-Out\In\

This bullet link is improperly disallowed by publishing automation due to presence of string 'docuXXmentation/arXXticles':
- [Search SQL Database documentation, with filters](http://azure.microsoft.com/docuXXmentation/arXXticles/?service=sql-database)
-->



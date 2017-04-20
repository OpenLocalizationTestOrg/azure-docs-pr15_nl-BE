<properties
   pageTitle="Aan de slag met tijdelijke tabellen in Azure SQL-Database | Microsoft Azure"
   description="Informatie over hoe te beginnen met het gebruik van tijdelijke tabellen in Azure SQL-Database."
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
   ms.workload="sql-database"
   ms.date="08/29/2016"
   ms.author="carlrab"/>

#<a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Aan de slag met tijdelijke tabellen in Azure SQL-Database

Tijdelijke tabellen zijn een nieuwe functie van het programmeren van Azure SQL-Database waarmee u kunt bijhouden en analyseren van de volledige historie van wijzigingen in uw gegevens, zonder de noodzaak van aangepaste code. Tijdelijke tabellen bewaren gegevens die nauw verband houden met de context van de tijd dat opgeslagen gegevens kunnen worden geïnterpreteerd als geldig alleen binnen een bepaalde periode. Deze eigenschap van tijdelijke tabellen kan efficiënt analyse op basis van tijd en inzichten die ontwikkeling gegevens ophalen.

##<a name="temporal-scenario"></a>Tijdelijke Scenario

In dit artikel ziet u de stappen voor het gebruik van tijdelijke tabellen in een scenario van toepassing. Stel dat u wilt bijhouden van gebruikersactiviteiten op een nieuwe website die volledig wordt ontwikkeld of een bestaande website die u wilt uitbreiden met gebruiker activiteit analytics. In dit eenvoudige voorbeeld nemen we aan dat het aantal bezochte webpagina's in een periode is een indicator die moet worden vastgelegd en bewaakt in de database website die wordt gehost op Azure SQL-Database. Het doel van de historische analyse van de gebruikersactiviteit wordt om invoer voor website ontwerpen en betere ervaring voor de bezoekers.

Het databasemodel in dit scenario is erg eenvoudig: gebruiker activiteit metric wordt voorgesteld met een veld één geheel, **PageVisited**, en samen met basisinformatie over het profiel is opgenomen. Ook voor op basis van de analyse, zou u een reeks rijen voor elke gebruiker, waarbij elke rij vertegenwoordigt het aantal pagina's een bepaalde gebruiker heeft bezocht binnen een bepaalde periode.

![Schema](./media/sql-database-temporal-tables/AzureTemporal1.png)

Gelukkig hoeft u niet alle moeite om in te zetten de app om de informatie van deze activiteit te onderhouden. Met tijdelijke tabellen, wordt dit proces wordt uitgevoerd - waardoor u de volledige flexibiliteit tijdens het websiteontwerp en meer tijd te concentreren op de analyse van de gegevens zelf. Het enige wat u hoeft te doen is ervoor te zorgen dat de tabel **WebSiteInfo** is geconfigureerd als [temporal systeem versienummer](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). De exacte procedure voor het gebruik van tijdelijke tabellen in dit scenario worden hieronder beschreven.

##<a name="step-1-configure-tables-as-temporal"></a>Stap 1: Tabellen als tijdelijke configureren

Afhankelijk van of u het starten van de ontwikkeling van nieuwe of bestaande toepassing upgraden, wordt u tijdelijke tabellen maken of bestaande wijzigen door tijdelijke kenmerken toe te voegen. In het geval uw scenario is een combinatie van deze twee opties. Deze actie uitvoeren met gebruik [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) of enig ander ontwikkelprogramma van Transact-SQL.


> [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de meest recente versie van het Management Studio gebruiken om met updates voor Microsoft Azure en SQL-Database gesynchroniseerd blijven. [Bijwerken van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


###<a name="create-new-table"></a>Nieuwe tabel maken

Gebruik context menu 'Nieuwe systeem Versioned Table' in SSMS Object Explorer query editor met een tijdelijke tabel sjabloonscript openen en vervolgens met "Geef waarden voor Parameters sjabloon" (Ctrl + Shift + M) voor het vullen van de sjabloon:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

In SSDT, gekozen voor "(systeem-versienummer) tijdelijke tabel" sjabloon wanneer nieuwe items worden toegevoegd aan het databaseproject. Die opent de ontwerpfunctie voor tabellen en kunt u gemakkelijk de tabelindeling opgeven:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

U kunt ook een tijdelijke tabel maken door te geven van de Transact-SQL-instructies rechtstreeks, zoals in het volgende voorbeeld wordt getoond. Houd er rekening mee dat de verplichte elementen van elke tijdelijke tabel de definitie en de SYSTEM_VERSIONING-component met een verwijzing naar een andere gebruikerstabel waarin historische rij versies zijn:

````
CREATE TABLE WebsiteUserInfo 
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED 
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL 
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
````

Wanneer u systeem versioned tijdelijke tabel maakt, wordt automatisch de bijbehorende historietabel met de standaardconfiguratie gemaakt. De standaardtabel geschiedenis bevat een gegroepeerde index B structuur van de kolommen (einde begin) met gebruik van compressie pagina. Deze configuratie is optimaal voor het merendeel van de scenario's waarin tijdelijke tabellen worden gebruikt, met name voor de [controle van de gegevens](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0). 

In dit bijzondere geval spijt van trendanalyses uit te voeren op basis van tijd over een langere gegevensgeschiedenis en met grotere gegevensverzamelingen, dus de keuze opslag voor de historietabel een columnstore voor geclusterde index is. Een geclusterde columnstore zeer goede compressie en prestaties biedt voor analytische query's. Tijdelijke tabellen geven u de flexibiliteit om de indexen op de huidige en tijdelijke tabellen volledig onafhankelijk van elkaar configureren. 

**Opmerking**: Columnstore indexen zijn alleen beschikbaar in de laag premium service.

Het volgende script ziet u hoe de standaardindex voor geschiedenistabel met kan worden gewijzigd in de geclusterde columnstore:

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

Tijdelijke tabellen worden weergegeven in het Explorer-Object met het specifieke pictogram voor gemakkelijk kan worden herkend, terwijl de geschiedenistabel wordt weergegeven als een onderliggend knooppunt.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

###<a name="alter-existing-table-to-temporal"></a>Bestaande tijdelijke tabel wijzigen

We dekken het alternatieve scenario waarin de tabel WebsiteUserInfo bestaat al, maar is niet ontworpen voor het bijhouden van wijzigingen. In dit geval kunt u de bestaande tabel worden tijdelijke, zoals in het volgende voorbeeld eenvoudig uitbreiden:

````
ALTER TABLE WebsiteUserInfo 
ADD 
    ValidFrom datetime2 (0) GENERATED ALWAYS AS ROW START HIDDEN  
        constraint DF_ValidFrom DEFAULT DATEADD(SECOND, -1, SYSUTCDATETIME())
    , ValidTo datetime2 (0)  GENERATED ALWAYS AS ROW END HIDDEN   
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo); 

ALTER TABLE WebsiteUserInfo  
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

##<a name="step-2-run-your-workload-regularly"></a>Stap 2: Uw werkbelasting regelmatig wordt uitgevoerd

Het belangrijkste voordeel van tijdelijke tabellen is dat u wilt wijzigen of aanpassen van uw website op geen enkele manier voor het bijhouden van wijzigingen uitvoeren. Eenmaal gemaakt, behouden tijdelijke tabellen transparant vorige rij versies telkens wanneer u wijzigingen kunt op uw gegevens uitvoeren. 

Om te kunnen gebruikmaken van automatische wijzigingen bijhouden voor dit scenario gaan we net update **PagesVisited** kolom telkens wanneer de gebruiker zijn geïnstaleerd sessie op de website eindigt:

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

Het is belangrijk dat u ziet dat de bijwerkquery niet hoeft te weten het exacte tijdstip wanneer de huidige bewerking is opgetreden en hoe de historische gegevens blijven behouden voor toekomstige analyses. Beide aspecten worden automatisch verwerkt door de Azure SQL-Database. In het volgende diagram ziet u hoe gegevens op elke update wordt gegenereerd.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

##<a name="step-3-perform-historical-data-analysis"></a>Stap 3: Analyse van historische gegevens uitvoeren

Als tijdelijke systeem-versiebeheer is ingeschakeld, is analyse van historische gegevens slechts één query u af. In dit artikel bieden we een paar voorbeelden die adres algemene analyse scenario's - voor meer details van alle verschillende opties geïntroduceerd met de component [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) verkennen.

Overzicht van de top 10 gebruikers, het aantal bezochte webpagina's op een uur geleden heeft besteld, kunt u deze query uitvoeren:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

U kunt deze query voor het analyseren van de site bezoeken op een dag geleden, een maand geleden eenvoudig wijzigen of op elk punt in het verleden die u wenst.

Gebruik eenvoudige statistische analyse uitvoeren voor de vorige dag, in het volgende voorbeeld:

````
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
````

Gebruik de component opgenomen om te zoeken naar activiteiten van een specifieke gebruiker binnen een termijn:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

Grafische visualisatie is vooral handig voor tijdelijke query's kunt u weergeven trends en gebruikspatronen in een intuïtieve manier heel eenvoudig:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

##<a name="evolving-table-schema"></a>Tabelschema in ontwikkeling

Meestal moet u het tabelschema van de tijdelijke wijzigen terwijl u ontwikkeling van apps. Voor dat, wordt uit te voeren regelmatig ALTER TABLE instructies en Azure SQL-Database op de juiste wijze doorgeven van wijzigingen in de historietabel. Het volgende script ziet u hoe u extra kenmerk voor tracering kunt toevoegen:

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

Op dezelfde manier kunt u de definitie van kolom terwijl uw werkbelasting actief is:

````
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

Ten slotte kunt u een kolom die u niet meer nodig hebt verwijderen.

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````
    
Laatste [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) ook gebruiken tabelschema tijdelijke wijzigen terwijl u met de database (on line modus) of als onderdeel van het databaseproject (offline-modus verbonden bent).

##<a name="controlling-retention-of-historical-data"></a>Behoud van de historische gegevens beheren

Met systeem versioned tijdelijke tabellen, kan de historietabel verhogen de grootte van de database van meer dan gewone tabellen. Een grote en groeiende geschiedenistabel kan resulteren in een probleem dat zowel door zuivere opslagkosten, alsmede het opleggen van een belasting op het temporal opvragen. Ontwikkelen van een bewaarbeleid gegevens voor het beheren van gegevens in de historietabel is daarom een belangrijk aspect van het plannen en beheren van de levenscyclus van elke tijdelijke tabel. Met Azure SQL-Database hebt u de volgende methoden voor het beheer van historische gegevens in de tijdelijke tabel:

- [Partitioneren van tabel](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
- [Script voor aangepaste opruimen](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

##<a name="next-steps"></a>Volgende stappen

Uitchecken [MSDN-documentatie](https://msdn.microsoft.com/library/dn935015.aspx)voor gedetailleerde informatie over tijdelijke tabellen.
Ga naar kanaal 9 om te horen van een [Succesverhaal van echte klant temporal-implementatie](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) en bekijk een [live demonstratie temporal](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

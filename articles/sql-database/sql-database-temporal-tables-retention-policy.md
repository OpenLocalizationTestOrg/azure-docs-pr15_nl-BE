<properties
   pageTitle="Beheer van historische gegevens in tijdelijke tabellen met bewaarbeleid | Microsoft Azure"
   description="Leren werken met tijdelijke bewaarbeleid historische gegevens onder controle te houden."
   services="sql-database"
   documentationCenter=""
   authors="bonova"
   manager="drasumic"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sql-database"
   ms.date="10/12/2016"
   ms.author="bonova"/>

#<a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Historische gegevens in tijdelijke tabellen met bewaarbeleid beheren

Tijdelijke tabellen kunnen verhogen grootte van de database van meer dan gewone tabellen, met name als u historische gegevens voor een langere periode bewaren. Het bewaarbeleid voor historische gegevens is daarom een belangrijk aspect van het plannen en beheren van de levenscyclus van elke tijdelijke tabel. Tijdelijke tabellen in Azure SQL-Database wordt geleverd met eenvoudig gebruiken bewaren mechanisme waarmee u deze taak kunt uitvoeren.

Tijdelijke Geschiedenis vasthouden kan worden geconfigureerd op het niveau van afzonderlijke tabel, waardoor gebruikers maken flexibele veroudering beleid. Toepassen van tijdelijke inhouding is eenvoudig: slechts één parameter worden ingesteld tijdens het maken of schema wijziging is vereist.

Na het bewaarbeleid definiëren, start Azure SQL-Database regelmatig controleren of er zijn historische rijen die in aanmerking voor automatische gegevens wissen komen. Identificatie van de overeenkomende rijen en de uitslag uit de historietabel optreden transparant, de achtergrondtaak die is gepland en uitgevoerd door het systeem. Leeftijdsvoorwaarde voor de geschiedenis van tabelrijen wordt gecontroleerd op basis van de kolom die het einde van de periode SYSTEM_TIME. Als de bewaarperiode, bijvoorbeeld is ingesteld op zes maanden, heeft tabelrijen in aanmerking voor opschonen voldoen aan de volgende voorwaarde:

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

In het bovenstaande voorbeeld we aangenomen dat **ValidTo** kolom komt met het einde van de periode SYSTEM_TIME overeen.

##<a name="how-to-configure-retention-policy"></a>Het bewaarbeleid configureren?

Voordat u het bewaarbeleid voor een tijdelijke tabel configureren, Controleer eerst of tijdelijke inhouding van historische ingeschakeld *op het databaseniveau van de is*.

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

Database vlag **is_temporal_history_retention_enabled** is standaard ingesteld op ingeschakeld, maar gebruikers kunnen worden gewijzigd met de instructie ALTER DATABASE. Deze ook automatisch ingesteld op OFF na bewerking [in een keer herstellen](sql-database-point-in-time-restore-portal.md) . Voer de volgende instructie zodat de geschiedenis van tijdelijke inhouding cleanup voor uw database:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [AZURE.IMPORTANT] Kunt u bewaren voor tijdelijke tabellen ook als **is_temporal_history_retention_enabled** uitgeschakeld is, maar automatisch opschonen van verouderde rijen wordt in dat geval niet geactiveerd.


Bewaarbeleid wordt geconfigureerd tijdens het maken van de tabel door te geven voor de parameter HISTORY_RETENTION_PERIOD:

````
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
````

Azure SQL-Database, kunt u de bewaarperiode met behulp van verschillende tijdseenheden opgeven: dagen, weken, maanden en jaren. Als HISTORY_RETENTION_PERIOD wordt weggelaten, wordt uitgegaan van oneindige bewaren. U kunt ook ONEINDIG trefwoord expliciet.

In sommige gevallen kunt u configureren bewaren na het maken van een tabel of als u wilt wijzigen, eerder geconfigureerde waarde. In dat geval gebruikt u ALTER TABLE-instructie:

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [AZURE.IMPORTANT]  Als u SYSTEM_VERSIONING uit de periode waarde *blijven niet behouden* . SYSTEM_VERSIONING instellen op ON zonder HISTORY_RETENTION_PERIOD expliciet resultaten in de oneindige bewaartermijn opgegeven.

Om de huidige status van het bewaarbeleid bekijken, gebruikt u de volgende query waardoor tijdelijke inhouding medewerkers vlag op het databaseniveau met de bewaarperiode voor afzonderlijke tabellen:

````
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
````


##<a name="how-sql-database-deletes-aged-rows"></a>Hoe SQL-Database verwijdert rijen veroudering?

Het opruimingsproces is afhankelijk van de opmaak voor de index van de historietabel. Het is belangrijk dat u ziet dat *alleen de geschiedenis van tabellen met een gegroepeerde index (B-tree of columnstore) eindige bewaarbeleid geconfigureerd kunnen hebben*. Een achtergrondtaak wordt verouderde gegevens opschonen uitvoeren voor alle tijdelijke tabellen met beperkte bewaartermijn gemaakt.
CleanUp logica voor de geclusterde index voor rowstore (B-tree) verwijdert u verouderde rij in kleinere stukken (maximaal 10 K) druk op de i/o-subsysteem van het logboek te minimaliseren. Hoewel het opschonen logica gebruikt een vereiste index van B-structuur, volgorde van verwijderingen voor de rijen die ouder zijn dan de bewaarperiode stevig kan niet worden gegarandeerd. Daarom *nemen niet alle afhankelijkheden van de order opruimen in uw toepassingen*.

De taak voor de geclusterde columnstore verwijdert u hele [Rijgroepen](https://msdn.microsoft.com/library/gg492088.aspx) tegelijk (bevatten doorgaans 1 miljoen rijen elk), die is zeer efficiënt, met name wanneer de historische gegevens wordt gegenereerd in een hoog tempo.

![Geclusterde columnstore bewaren](./media/sql-database-temporal-tables-retention-policy/cciretention.png)


Uitstekende gegevenscompressie en efficiënte handhaving opruimen wordt geclusterde index columnstore een perfecte keuze voor scenario's wanneer uw werkbelasting snel hoog bedrag van historische gegevens genereert. Dat patroon is typisch voor intensieve [transactionele processorbelasting die tijdelijke tabellen gebruikt](https://msdn.microsoft.com/library/mt631669.aspx) voor het bijhouden en controleren, trendanalyse of ingestie van IoT-gegevens.

##<a name="index-considerations"></a>Index-overwegingen

De opruimactie voor tabellen met rowstore geclusterde index moet index beginnen met de kolom die het einde van de periode SYSTEM_TIME. Als de index niet bestaat, niet mogelijk beperkte bewaartermijn configureren:

*Msg 13765 16 niveau 1 staat <br> </br> op systeem versioned tijdelijke tabel 'temporalstagetestdb.dbo.WebsiteUserInfo' beperkte bewaartermijn instellen is mislukt omdat de historietabel 'temporalstagetestdb.dbo.WebsiteUserInfoHistory' bevat geen geclusterde index vereist. Rekening houden met het maken van een geclusterde columnstore of B-tree index beginnen met de kolom die overeenkomt met het einde van de SYSTEM_TIME periode, in de historietabel.*

Het is belangrijk dat u ziet dat de standaardtabel geschiedenis al gemaakt door Azure SQL Database index, die voldoet aan de standaard voor het bewaarbeleid is geclusterd. Als u probeert te verwijderen die index op een tabel met beperkte bewaartermijn, mislukt de bewerking met de volgende fout:

*Msg 13766 16 niveau 1 staat <br> </br> de geclusterde index 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' kan niet verwijderen omdat deze wordt gebruikt voor het automatisch opschonen van verouderde gegevens. U kunt HISTORY_RETENTION_PERIOD infinite op de overeenkomstige systeem versioned tijdelijke tabel instellen als u nodig hebt om deze index neer te zetten.*

Opruimen van de columnstore van geclusterde index werkt optimaal als historische rijen worden ingevoegd in de oplopende volgorde (gesorteerd op het einde van de kolom periode), dat altijd het geval is wanneer de historietabel uitsluitend door het mechanisme van de SYSTEM_VERSIONIOING is gevuld. Als de rijen in de historietabel niet besteld door het einde van de kolom periode (dit kan het geval als u bestaande historische gegevens gemigreerd), moet u opnieuw columnstore geclusterde index op B-tree rowstore index, die goed zijn besteld, voor een optimale prestatie te maken.

Vermijd opnieuw samenstellen columnstore geclusterde index op de historietabel met de beperkte bewaartermijn omdat deze volgorde in de Rijgroepen natuurlijk opgelegd door de werking van het systeem voor het bijhouden van versies kan veranderen. Als u nodig hebt om op te bouwen columnstore geclusterde index voor een geschiedenistabel met de, door opnieuw op te maken op een compatibele B tree-index, behouden bestellen in de rowgroups die nodig zijn voor gewone gegevens opschonen doen. Dezelfde aanpak dient te worden gehouden als u tijdelijke tabel maken met de bestaande geschiedenistabel die is geclusterde index kolommen zonder gegarandeerde gegevensvolgorde:

````
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

Als beperkte bewaartermijn voor de historietabel met de columnstore van geclusterde index is geconfigureerd, kunt u geen B-structuur voor extra niet-geclusterde indexen maken op die tabel:

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

Een poging tot het uitvoeren van bovenstaande instructie mislukt met de volgende fout:

*Msg 13772 16 niveau 1 staat <br> </br> kan maken niet-geclusterde index voor een geschiedenistabel tijdelijk 'WebsiteUserInfoHistory' omdat er beperkte bewaartermijn en columnstore geclusterde index die wordt gedefinieerd.*

##<a name="querying-tables-with-retention-policy"></a>Tabellen met bewaarbeleid opvragen

Alle query's in de tijdelijke tabel filter automatisch historische rijen die voldoen aan eindige bewaarbeleid onvoorspelbaar en inconsistente resultaten vermijden omdat verouderde rijen kunnen worden verwijderd door de opruimactie *op elk punt in de tijd en in willekeurige volgorde*.

De volgende afbeelding ziet het queryplan voor een eenvoudige query:

````
SELECT * FROM dbo.WebsiteUserInfo FROM SYSTEM_TIME ALL;
````

Het queryplan bevat extra filter toegepast op het einde van de kolom periode (ValidTo) in de operator "Geclusterde Index controleren" in de historietabel (gemarkeerd). In dit voorbeeld wordt ervan uitgegaan dat 1 maand bewaarperiode is ingesteld in de tabel WebsiteUserInfo.

![Behoud queryfilter](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Als u een query geschiedenistabel rechtstreeks uitvoert, kan u rijen die ouder dan behoud opgegeven periode, maar zonder enige garantie voor herhaalbare resultaten zijn zien. De volgende afbeelding ziet query uitvoeringsplan voor de query op de historietabel zonder aanvullende filters toegepast:

![Bij het controleren van geschiedenis zonder filter bewaren](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Niet te vertrouwen uw zakelijke logica op geschiedenistabel buiten de bewaarperiode lezen als u inconsistente of onverwachte resultaten. Het verdient tijdelijke query's gebruiken met de component voor SYSTEM_TIME voor het analyseren van gegevens in de tijdelijke tabellen.

##<a name="point-in-time-restore-considerations"></a>Wijs in de overwegingen bij het terugzetten van tijd

Wanneer u een nieuwe database met het [terugzetten bestaande database naar een bepaald punt in de tijd maakt](sql-database-point-in-time-restore-portal.md), heeft tijdelijke inhouding op het databaseniveau van de uitgeschakeld. (de vlag**is_temporal_history_retention_enabled** is ingesteld op OFF). Met deze functie kunt u alle historische rijen bij het herstellen, zonder zorgen dat verouderde rijen worden verwijderd voordat u deze query bekijken. U kunt het *controleren van historische gegevens buiten de geconfigureerde periode*.

Stel dat een tijdelijke tabel een maand bewaren opgegeven periode heeft. Als uw database is gemaakt in laag Premium Service, zou u kunnen maken van databasekopie met de status van de database van 35 dagen terug in het verleden zijn. Die effectief kunt u historische rijen die door het rechtstreeks opvragen van de historietabel tot 65 dagen te analyseren.

Als u opruimen van tijdelijke inhouding activeren wilt, uitvoeren met de volgende Transact-SQL-instructie na punt in tijd herstellen:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

##<a name="next-steps"></a>Volgende stappen

Meer informatie over het gebruik van uitchecken tijdelijke tabellen in uw toepassingen [Aan de slag met tijdelijke tabellen in Azure SQL-Database](sql-database-temporal-tables.md).

Ga naar kanaal 9 om te horen van een [Succesverhaal van echte klant temporal uitvoering](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) en bekijk een [live demonstratie temporal](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

Bekijk de [MSDN-documentatie](https://msdn.microsoft.com/library/dn935015.aspx)voor gedetailleerde informatie over tijdelijke tabellen.

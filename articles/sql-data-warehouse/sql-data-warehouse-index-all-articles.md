<properties
    pageTitle="Alle onderwerpen voor de service SQL Data Warehouse | Microsoft Azure"
    description="Tabel van alle onderwerpen voor de Azure service met de naam SQL Data Warehouse die aanwezig zijn op http://azure.microsoft.com/documentation/articles/, titel en beschrijving."
    services="sql-data-warehouse"
    documentationCenter=""
    authors="barbkess"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-data-warehouse"
    ms.workload="sql-data-warehouse"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="barbkess"/>


# <a name="all-topics-for-azure-sql-data-warehouse-service"></a>Alle onderwerpen voor Azure SQL Data Warehouse-service

In dit onderwerp worden elk onderwerp dat van toepassing op de service **SQL Data Warehouse** van Azure rechtstreeks is. U kunt deze webpagina voor trefwoorden zoeken met behulp van **Ctrl + F**, te vinden in de onderwerpen van de huidige rente.




## <a name="new"></a>Nieuw

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 1 | [Back-ups van SQL Data Warehouse](sql-data-warehouse-backups.md) | Meer informatie over SQL Data Warehouse ingebouwde back-ups voor een magazijn Azure SQL-gegevens terugzetten naar een herstelpunt of een andere geografische regio. |


## <a name="updated-articles-sql-data-warehouse"></a>Bijgewerkte artikelen, SQL Data Warehouse

Deze sectie bevat een lijst van artikelen die onlangs zijn bijgewerkt waarin de update groot of significant is. Voor elk artikel bijgewerkt, wordt een ruwe fragment van de prijsverlaging toegevoegde tekst weergegeven. De artikelen zijn bijgewerkt binnen het datumbereik van **2016-08-22** tot en met **2016-10-05**.

| &nbsp; | Artikel | Bijgewerkte tekst, fragment | Bijgewerkt toen |
| --: | :-- | :-- | :-- |
| 2 | [Gegevens laden van Azure blob-opslag in SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | /-Voor het bijhouden van bytes en bestanden selecteren r.command, s.request_id, r.status, count (distinct input_name) als nbr_files, som (s.bytes_processed) / 1024/1024 als gb_processed van sys.dm_pdw_exec_requests r inner join sys.dm_pdw_dms_external_work s op r.request_id s.request_id = r waar. label = ' CTAS: cso laden. DimProduct ' OR r. label = ' CTAS: cso laden. FactOnlineSales GROEPEREN op r.command, s.request_id, r.status ORDER BY nbr_files desc, gb_processed desc;  | 2016-09-07 |
| 3 | [Magazijn van SQL-gegevens terugzetten](sql-data-warehouse-restore-database-overview.md) | **Kan ik een onderbroken datawarehouse terugzetten?** Als u met het herstellen van een datawarehouse die is onderbroken, moet u eerst weer on line brengen. Nadat het datawarehouse weer on line is, hebt u zeven dagen van herstelpunten om uit te kiezen. **Terugzetten naar een geo-redundante regio** Als u de geo-redundante opslag gebruikt, kunt u het datawarehouse terugzetten naar uw center gekoppelde gegevens in een andere geografische regio. Het datawarehouse wordt van de laatste dagelijkse back-up teruggezet. **Tijdlijn herstellen** In de afgelopen zeven dagen kunt u een database naar elk gewenst herstelpunt herstellen. Momentopnamen start elke vier tot acht uur en zijn beschikbaar voor zeven dagen. Wanneer u een momentopname is ouder dan zeven dagen, deze verloopt en het herstelpunt is niet langer beschikbaar. **Kosten herstellen** De kosten van opslag voor het magazijn van de teruggezette gegevens wordt gefactureerd tegen een tarief Azure Premium opslag. Als u een teruggezette gegevensmagazijn onderbreekt, worden in rekening gebracht voor opslag Azure Premium opslag snelheid. Het voordeel van onderbreken is dat u bent geen kosten | 2016-09-29 |





## <a name="get-started"></a>Aan de slag

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 4 | [Datawarehouse Azure SQL-verificatie](sql-data-warehouse-authentication.md) | Azure Active Directory (AAD) en SQL Server-verificatie met Azure SQL Data Warehouse. |
| 5 | [Aanbevolen procedures voor Azure SQL Data Warehouse](sql-data-warehouse-best-practices.md) | Aanbevelingen en beste praktijken die u als u weten moet oplossingen ontwikkelen voor Azure SQL Data Warehouse. Deze helpen u slagen. |
| 6 | [Stuurprogramma's voor Azure SQL datawarehouse](sql-data-warehouse-connection-strings.md) | Verbindingsreeksen en stuurprogramma's voor SQL Data Warehouse |
| 7 | [Verbinding maken met Azure SQL datawarehouse](sql-data-warehouse-connect-overview.md) | Het zoeken naar de server en de verbinding string voor uw naar Azure SQL Data Warehouse |
| 8 | [Gegevens analyseren met Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md) | Gebruik de Azure Machine leren een voorspellende machine leren model op basis van gegevens die zijn opgeslagen in Azure SQL Data Warehouse bouwen. |
| 9 | [Query Azure SQL Data Warehouse (sqlcmd)](sql-data-warehouse-get-started-connect-sqlcmd.md) | Azure SQL Data Warehouse opvragen met de Command-line hulpprogramma sqlcmd. |
| 10 | [Een datawarehouse SQL-database maken met behulp van Transact-SQL (TSQL)](sql-data-warehouse-get-started-create-database-tsql.md) | Informatie over het maken van een datawarehouse Azure SQL met TSQL |
| 11 | [Het maken van een ticket voor de ondersteuning voor SQL Data Warehouse](sql-data-warehouse-get-started-create-support-ticket.md) | Het maken van een support ticket in Azure SQL Data Warehouse. |
| 12 | [Laden van gegevens met Azure Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md) | Meer informatie over laden van gegevens met Azure Data Factory |
| 13 | [Gegevens laden met PolyBase in SQL Data Warehouse](sql-data-warehouse-get-started-load-with-polybase.md) | Informatie over PolyBase en hoe u deze gebruikt voor scenario's voor gegevensopslag. |
| 14 | [Een datawarehouse Azure SQL maken](sql-data-warehouse-get-started-provision.md) | Informatie over het maken van een datawarehouse Azure SQL in Azure portal |
| 15 | [SQL Data Warehouse met PowerShell maken](sql-data-warehouse-get-started-provision-powershell.md) | SQL Data Warehouse met PowerShell maken |
| 16 | [Gegevens met Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md) | Gegevens met Power BI SQL Data Warehouse |
| 17 | [Query Azure SQL datawarehouse (Visual Studio)](sql-data-warehouse-query-visual-studio.md) | Query SQL datawarehouse met Visual Studio. |



## <a name="develop"></a>Ontwikkelen

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 18 | [Optimaliseren voor SQL Data Warehouse](sql-data-warehouse-develop-best-practices-transactions.md) | Best Practice richtlijnen voor efficiënte transactie updates schrijven in Azure SQL Data Warehouse |
| 19 | [Gelijktijdigheid en werkbelasting beheer in SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md) | Beheer van gelijktijdigheid en werkbelasting in Azure SQL Data Warehouse begrijpen voor het ontwikkelen van oplossingen. |
| 20 | [Tabel maken als selecteren (CTAS) in SQL datawarehouse](sql-data-warehouse-develop-ctas.md) | Tips voor het coderen met de tabel maken als de select-instructie (CTAS) in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 21 | [Dynamische SQL in SQL datawarehouse](sql-data-warehouse-develop-dynamic-sql.md) | Tips voor het gebruik van dynamische SQL in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 22 | [Groeperen op opties in het datawarehouse SQL](sql-data-warehouse-develop-group-by-options.md) | Tips voor uitvoering van de groep met opties in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 23 | [Labels voor instrument van query's in SQL Data Warehouse gebruiken](sql-data-warehouse-develop-label.md) | Tips voor het gebruik van labels aan query's instrument in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 24 | [Lussen in SQL datawarehouse](sql-data-warehouse-develop-loops.md) | Tips voor Transact-SQL-lussen en vervangt cursors in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 25 | [Opgeslagen procedures in SQL Data Warehouse](sql-data-warehouse-develop-stored-procedures.md) | Tips voor de uitvoering van opgeslagen procedures in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 26 | [Transacties in SQL datawarehouse](sql-data-warehouse-develop-transactions.md) | Tips voor het implementeren van transacties in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 27 | [Door de gebruiker gedefinieerde schema's in SQL Data Warehouse](sql-data-warehouse-develop-user-defined-schemas.md) | Tips voor het gebruik van de Transact-SQL-schema's in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 28 | [Toewijzen van variabelen in SQL Data Warehouse](sql-data-warehouse-develop-variable-assignment.md) | Tips voor het toewijzen van variabelen in Azure SQL Data Warehouse Transact-SQL voor het ontwikkelen van oplossingen. |
| 29 | [Weergaven in SQL datawarehouse](sql-data-warehouse-develop-views.md) | Tips voor het gebruik van de Transact-SQL-weergaven in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 30 | [Ontwerp en code technieken voor SQL Data Warehouse](sql-data-warehouse-overview-develop.md) | Concepten van ontwikkeling, ontwerpbesluiten, aanbevelingen en coding technieken voor SQL Data Warehouse. |



## <a name="manage"></a>Beheren

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 31 | [Beheren van rekenkracht in Azure SQL Data Warehouse (overzicht)](sql-data-warehouse-manage-compute-overview.md) | Schaal van de prestaties van mogelijkheden in Azure SQL Data Warehouse. Geschaald uit door DWUs aan te passen of onderbreken en hervatten compute resources om de kosten besparen. |
| 32 | [Beheren van rekenkracht in Azure SQL Data Warehouse (Azure portal)](sql-data-warehouse-manage-compute-portal.md) | Azure portal taken voor het beheren van rekenkracht. Schaal berekenen bronnen door DWUs aan te passen. Of onderbreken en hervatten middelen om op te slaan van de kosten berekenen. |
| 33 | [Rekenkracht in Azure SQL Data Warehouse (PowerShell) beheren](sql-data-warehouse-manage-compute-powershell.md) | Taken voor het beheren van PowerShell rekenkracht. Schaal berekenen bronnen door DWUs aan te passen. Of onderbreken en hervatten middelen om op te slaan van de kosten berekenen. |
| 34 | [Beheren van rekenkracht in Azure SQL gegevens magazijn (REST)](sql-data-warehouse-manage-compute-rest-api.md) | Taken voor het beheren van PowerShell rekenkracht. Schaal berekenen bronnen door DWUs aan te passen. Of onderbreken en hervatten middelen om op te slaan van de kosten berekenen. |
| 35 | [Beheren van rekenkracht in Azure SQL Data Warehouse (T-SQL)](sql-data-warehouse-manage-compute-tsql.md) | Transact-SQL (T-SQL) taken schalingsprestaties door DWUs aan te passen. Kosten besparen door de schaal terug tijdens niet-piekuren. |
| 36 | [Controleer uw werkbelasting met DMVs](sql-data-warehouse-manage-monitor.md) | Informatie over het controleren van uw werkbelasting met DMVs. |
| 37 | [Beheren van databases in Azure SQL Data Warehouse](sql-data-warehouse-overview-manage.md) | Overzicht van het beheren van databases SQL Data Warehouse. Bevat hulpprogramma's, DWUs en schaalbare prestaties, problemen met prestaties van query's, tot stand brengen van goed beleid en een database herstellen van beschadigde gegevens of van een regionale uitvalt. |
| 38 | [Monitor gebruikersquery in Azure SQL Data Warehouse](sql-data-warehouse-overview-manage-user-queries.md) | Overzicht van de overwegingen, best practices en taken voor het controleren van vragen van gebruikers in Azure SQL Data Warehouse |
| 39 | [Magazijn van SQL-gegevens terugzetten](sql-data-warehouse-restore-database-overview.md) | Overzicht van de opties voor database terugzetten voor het terugzetten van een database in Azure SQL Data Warehouse. |
| 40 | [Herstellen van een datawarehouse Azure SQL (Portal)](sql-data-warehouse-restore-database-portal.md) | Portal taken voor het terugzetten van een datawarehouse Azure SQL Azure. |
| 41 | [Een datawarehouse Azure SQL (PowerShell) herstellen](sql-data-warehouse-restore-database-powershell.md) | PowerShell-taken voor het terugzetten van een Azure SQL Data Warehouse. |
| 42 | [Herstellen van een datawarehouse Azure SQL (REST API)](sql-data-warehouse-restore-database-rest-api.md) | Taken in de REST API voor het terugzetten van een Azure SQL Data Warehouse. |



## <a name="tables-and-indexes"></a>Tabellen en indexen

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 43 | [Gegevenstypen voor tabellen in SQL Data Warehouse](sql-data-warehouse-tables-data-types.md) | Aan de slag met Azure SQL Data Warehouse tabellen-gegevenstypen. |
| 44 | [Distributie van tabellen in SQL Data Warehouse](sql-data-warehouse-tables-distribute.md) | Aan de slag met de distributie van tabellen in Azure SQL Data Warehouse. |
| 45 | [Tabellen in SQL Data Warehouse indexeren](sql-data-warehouse-tables-index.md) | Aan de slag met tabel indexeren in Azure SQL Data Warehouse. |
| 46 | [Overzicht van de tabellen in SQL Data Warehouse](sql-data-warehouse-tables-overview.md) | Aan de slag met Azure SQL-gegevenstabellen in het magazijn. |
| 47 | [Partitioneren van tabellen in SQL Data Warehouse](sql-data-warehouse-tables-partition.md) | Aan de slag met de tabel partitioneren in Azure SQL Data Warehouse. |
| 48 | [Statistieken over tabellen in SQL Data Warehouse beheren](sql-data-warehouse-tables-statistics.md) | Aan de slag met statistieken over tabellen in Azure SQL Data Warehouse. |
| 49 | [Tijdelijke tabellen in SQL Data Warehouse](sql-data-warehouse-tables-temporary.md) | Aan de slag met tijdelijke tabellen in Azure SQL Data Warehouse. |



## <a name="integrate"></a>Integreren

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 50 | [Azure Data Factory met SQL datawarehouse gebruiken](sql-data-warehouse-integrate-azure-data-factory.md) | Tips voor het gebruik van Azure Factory (ADF) met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 51 | [Azure Machine Learning met SQL datawarehouse gebruiken](sql-data-warehouse-integrate-azure-machine-learning.md) | Zelfstudie voor het gebruik van Azure Machine Learning met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 52 | [Azure Stream Analytics met SQL datawarehouse gebruiken](sql-data-warehouse-integrate-azure-stream-analytics.md) | Tips voor het gebruik van Azure Stream Analytics met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 53 | [Power BI met SQL datawarehouse gebruiken](sql-data-warehouse-integrate-power-bi.md) | Tips voor het gebruik van Power BI met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 54 | [Andere services van SQL Data Warehouse](sql-data-warehouse-overview-integrate.md) | Hulpprogramma's en oplossingen voor integratie met SQL Data Warehouse-partners.  |



## <a name="load"></a>Belasting

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 55 | [Gegevens laden van Azure blob-opslag in Azure SQL Data Warehouse (Azure Data Factory genoemd)](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md) | Meer informatie over laden van gegevens met Azure Data Factory |
| 56 | [Gegevens laden van Azure blob-opslag in SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | Informatie over het gebruik van PolyBase om gegevens te laden uit de Azure blobopslag in SQL Data Warehouse. Enkele tabellen uit openbare gegevens in het schema van Contoso Retail Data Warehouse worden geladen. |
| 57 | [Laden van gegevens van SQL Server in Azure SQL Data Warehouse (AZCopy)](sql-data-warehouse-load-from-sql-server-with-azcopy.md) | Bcp gebruikt om gegevens te exporteren van SQL Server met platte bestanden, AZCopy voor het importeren van gegevens naar Azure blob-opslag en PolyBase te nemen van de gegevens in Azure SQL Data Warehouse. |
| 58 | [Laden van gegevens van SQL Server in Azure SQL Data Warehouse (bestanden)](sql-data-warehouse-load-from-sql-server-with-bcp.md) | Voor een kleine gegevensgrootte wordt bcp gegevens uit SQL Server om bestanden te exporteren en de gegevens rechtstreeks importeren in Azure SQL Data Warehouse. |
| 59 | [Laden van gegevens uit SQL Server in Azure SQL gegevens magazijn (SSIS)](sql-data-warehouse-load-from-sql-server-with-integration-services.md) | Laat zien hoe u een pakket van SQL Server Integration Services (SSIS) gegevens uit uiteenlopende gegevensbronnen naar SQL Data Warehouse maken. |
| 60 | [Gegevens laden met PolyBase in SQL Data Warehouse](sql-data-warehouse-load-from-sql-server-with-polybase.md) | Bcp gebruikt om gegevens te exporteren van SQL Server met platte bestanden, AZCopy voor het importeren van gegevens naar Azure blob-opslag en PolyBase te nemen van de gegevens in Azure SQL Data Warehouse. |
| 61 | [Handleiding voor het gebruik van PolyBase in SQL Data Warehouse](sql-data-warehouse-load-polybase-guide.md) | Richtsnoeren en aanbevelingen voor het gebruik van PolyBase in SQL Data Warehouse-scenario's. |
| 62 | [Voorbeeldgegevens in SQL Data Warehouse laden](sql-data-warehouse-load-sample-databases.md) | Voorbeeldgegevens in SQL Data Warehouse laden |
| 63 | [Laden van gegevens met bcp](sql-data-warehouse-load-with-bcp.md) | Informatie over welke bcp is en hoe u deze gebruikt voor scenario's voor gegevensopslag. |
| 64 | [Gegevens laden in Azure SQL Data Warehouse](sql-data-warehouse-overview-load.md) | Informatie over de algemene scenario's voor gegevens in SQL Data Warehouse worden geladen. Deze omvatten PolyBase, Azure blob-opslag, platte bestanden en verzending van de schijf. U kunt ook hulpprogramma's van derden. |



## <a name="migrate"></a>Migreren

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 65 | [De SQL-code migreren naar SQL Data Warehouse](sql-data-warehouse-migrate-code.md) | Tips voor het migreren van uw SQL-code naar Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 66 | [Uw gegevens migreren](sql-data-warehouse-migrate-data.md) | Tips voor uw gegevens migreren naar het datawarehouse van Azure SQL voor het ontwikkelen van oplossingen. |
| 67 | [Het migratiehulpprogramma magazijn van gegevens (voorbeeld)](sql-data-warehouse-migrate-migration-utility.md) | Migreren naar SQL datawarehouse. |
| 68 | [Uw schema migreren naar SQL Data Warehouse](sql-data-warehouse-migrate-schema.md) | Tips voor het migreren van uw schema naar Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 69 | [Uw oplossing migreren naar SQL Data Warehouse](sql-data-warehouse-overview-migrate.md) | Instructies voor migratie van uw oplossing te brengen Azure SQL Data Warehouse-platform. |



## <a name="partners"></a>Partners

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 70 | [SQL Data Warehouse business intelligence-partners](sql-data-warehouse-partner-business-intelligence.md) | Lijsten van derde business intelligence partners met oplossingen voor SQL Data Warehouse. |
| 71 | [SQL Data Warehouse data integratiepartners](sql-data-warehouse-partner-data-integration.md) | Lijsten met derde partners met oplossingen voor integratie die ondersteuning bieden voor Azure SQL Data Warehouse. |
| 72 | [SQL Data Warehouse data management partners](sql-data-warehouse-partner-data-management.md) | Lijsten met gegevens van andere leveranciers management partners met oplossingen voor SQL Data Warehouse. |



## <a name="reference"></a>Referentie

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 73 | [Naslaginformatie voor SQL Data Warehouse](sql-data-warehouse-overview-reference.md) | Verwijzing naar koppelingen naar inhoud die voor SQL Data Warehouse. |
| 74 | [PowerShell-cmdlets en REST API's voor SQL Data Warehouse](sql-data-warehouse-reference-powershell-cmdlets.md) | De bovenste PowerShell-cmdlets voor Azure SQL Data Warehouse, met inbegrip van het onderbreken en hervatten van een database niet vinden. |
| 75 | [Taalelementen](sql-data-warehouse-reference-tsql-language-elements.md) | Lijst met koppelingen naar de inhoud van de taalelementen van Transact-SQL voor SQL Data Warehouse gebruikt. |
| 76 | [Onderwerpen van Transact-SQL](sql-data-warehouse-reference-tsql-statements.md) | Koppelingen naar naslaginformatie voor de Transact-SQL-onderwerpen die worden gebruikt door SQL Data Warehouse. |
| 77 | [Systeemweergaven](sql-data-warehouse-reference-tsql-system-views.md) | Koppelingen naar systeem analyseert de inhoud voor SQL Data Warehouse. |



## <a name="security"></a>Beveiliging

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 78 | [SQL datawarehouse - Downlevel-clients ondersteunen voor controle en dynamische gegevens maskeren](sql-data-warehouse-auditing-downlevel-clients.md) | Meer informatie over SQL Data Warehouse-ondersteuning voor downlevel clients voor de controle van gegevens |
| 79 | [Controle in Azure SQL datawarehouse](sql-data-warehouse-auditing-overview.md) | Aan de slag met Azure SQL Data Warehouse controle |
| 80 | [Aan de slag met transparante gegevens codering (TDE) in SQL Data Warehouse](sql-data-warehouse-encryption-tde.md) | Transparante gegevenscodering (TDE) in SQL datawarehouse |
| 81 | [Aan de slag met transparante gegevens codering (TDE)](sql-data-warehouse-encryption-tde-tsql.md) | Transparante gegevenscodering (TDE) in SQL datawarehouse (T-SQL) |
| 82 | [Een SQL Data Warehouse-database beveiligen](sql-data-warehouse-overview-manage-security.md) | Tips voor het beveiligen van een Azure SQL Data Warehouse-database voor het ontwikkelen van oplossingen. |



## <a name="miscellaneous"></a>Diverse

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 83 | [Installeer Visual Studio 2015 en SSDT voor SQL datawarehouse](sql-data-warehouse-install-visual-studio.md) | Installeer Visual Studio en SQL Server Development Tools (SSDT) voor Azure SQL datawarehouse |
| 84 | [Migratie naar Premium Storage Details](sql-data-warehouse-migrate-to-premium-storage.md) | Instructies voor het migreren van een bestaande SQL-datawarehouse premium opslag |
| 85 | [Aan de slag met bedreiging detectie](sql-data-warehouse-security-threat-detection.md) | Om te beginnen met bedreiging detectie |
| 86 | [Beperkingen van het datawarehouse SQL](sql-data-warehouse-service-capacity-limits.md) | Maximumwaarden voor verbindingen, databases, tabellen en query's voor SQL Data Warehouse. |
| 87 | [Het datawarehouse Azure SQL oplossen](sql-data-warehouse-troubleshoot.md) | Problemen met Azure SQL datawarehouse. |


<properties
   pageTitle="Problemen met Azure SQL datawarehouse | Microsoft Azure"
   description="Problemen met Azure SQL datawarehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="sonyama;barbkess"/>

# <a name="troubleshooting-azure-sql-data-warehouse"></a>Het datawarehouse Azure SQL oplossen

In dit onderwerp worden enkele van de meest voorkomende vragen die we van onze klanten horen.

## <a name="connecting"></a>Verbinding maken

| Probleem                              | Resolutie                                      |
| :----------------------------------| :---------------------------------------------- |
| Aanmelding mislukt voor gebruiker 'NT AUTHORITY\ANONYMOUS LOGON'. (Microsoft SQL Server, fout: 18456) | Deze fout treedt op wanneer een gebruiker AAD probeert verbinding te maken met de master-database, maar een gebruiker niet in het model laat.  U kunt dit probleem op SQL Data Warehouse u verbinding maken met tijdens de verbinding of de gebruiker toevoegen aan de master-database wilt opgeven.  Zie [Beveiliging-overzicht][] -artikel voor meer informatie.|
|De server principal "" MijnGebruikersnaam geen toegang tot de database 'master' onder de huidige beveiligingscontext. Kan de gebruiker standaarddatabase niet openen. Aanmelden is mislukt. Aanmelding mislukt voor gebruiker 'gebruik ' met Windows. (Microsoft SQL Server, fout: 916) | Deze fout treedt op wanneer een gebruiker AAD probeert verbinding te maken met de master-database, maar een gebruiker niet in het model laat.  U kunt dit probleem op SQL Data Warehouse u verbinding maken met tijdens de verbinding of de gebruiker toevoegen aan de master-database wilt opgeven.  Zie [Beveiliging-overzicht][] -artikel voor meer informatie.|
| CTAIP fout                        | Deze fout kan optreden wanneer een aanmelding op de master SQL server-database, maar niet in de SQL Data Warehouse-database is gemaakt.  Als deze fout optreedt, een kijkje nemen op het artikel [overzicht van de beveiliging][] .  In dit artikel wordt beschreven hoe u een aanmeldings-id en de gebruiker op de master- en vervolgens een gebruiker maken in de SQL Data Warehouse-database maken.|
| Geblokkeerd door Firewall                |Azure SQL-databases worden beschermd door de server en de database niveau firewalls om ervoor te zorgen alleen bekende IP-adressen toegang krijgen tot een database. De firewalls zijn beveiligd door standaard, hetgeen betekent dat u expliciet moet inschakelen en IP-adres of adresbereik voordat u verbinding kunt maken.  Volg de stappen in de [toegang tot de firewall configureren server voor de client-IP-][] in de [instructies van de Provisioning][]configureren van uw firewall om toegang te krijgen.|
| Kan geen verbinding maken met het programma of stuurprogramma | SQL Data Warehouse raadt het gebruik van [SSMS][], [SSDT voor Visual Studio 2015][]of [sqlcmd][] uw gegevens opvragen. Zie [stuurprogramma's voor Azure SQL Data Warehouse][] en [verbinding maken met Azure SQL Data Warehouse][] -artikelen voor meer informatie over stuurprogramma's en een verbinding met SQL Data Warehouse.|


## <a name="tools"></a>Hulpprogramma 's

| Probleem                              | Resolutie                                      |
| :----------------------------------| :---------------------------------------------- |
| Visual Studio object explorer ontbreekt AAD gebruikers | Dit is een bekend probleem.  Als tijdelijke oplossing de gebruikers in [sys.database_principals][]te bekijken.  Zie [verificatie voor Azure SQL Data Warehouse][] voor meer informatie over het gebruik van Azure Active Directory met SQL Data Warehouse.|

## <a name="performance"></a>Prestaties

|  Probleem                             | Resolutie                                      |
| :----------------------------------| :---------------------------------------------- |
| Problemen met query prestaties  | Als u probeert een bepaalde query oplossen, kunt u beginnen met het [leren werken met het controleren van uw query's][].|
| Slechte prestaties en plannen vaak het gevolg is van ontbrekende statistieken   | De meest voorkomende oorzaak van slechte prestaties is gebrek aan statistieken over uw tabellen.  [De tabelstatistieken Maintaining] Zie[ Statistics] voor meer informatie over het maken van statistieken en waarom ze zijn essentieel voor de prestaties.|
| Lage gelijktijdigheid / query's in de wachtrij geplaatst   | Informatie over [werkbelasting management][] is belangrijk om te begrijpen hoe de geheugentoewijzing in evenwicht met gelijktijdigheid.|
| Het implementeren van beste praktijken    | De beste plaats om te starten voor meer informatie over het verbeteren van prestaties van query's is artikel [SQL Data Warehouse aanbevolen procedures][] .|
| Het verbeteren van prestaties en schaalbaarheid  | Soms is de oplossing voor het verbeteren van de prestaties toe te voegen dat meer rekenkracht van uw query's door te [schalen van uw SQL-datawarehouse][].|
| Slechte prestaties als gevolg van de index van slechte kwaliteit | Sommige momenten query's kunnen vertraging vanwege [slechte columnstore index kwaliteit][].  Zie dit artikel voor meer informatie en het [opnieuw opbouwen van indexen voor het verbeteren van kwaliteit segment][].|

## <a name="system-management"></a>Systeembeheer

|  Probleem                             | Resolutie                                      |
| :----------------------------------| :---------------------------------------------- |
| Msg 40847: Kan de bewerking niet uitvoeren omdat de server meer dan het toegestane quotum Database transactie-eenheid van 45000. | Verlaagt de [DWU][] van de database die u probeert te maken of het [aanvragen van een verhoging van de quota][].|
| Onderzoek naar gebruik van de ruimte    | Zie de [grootte van de tabel][] te begrijpen van het gebruik van de ruimte van uw systeem.|
| Hulp bij het beheren van tabellen          | Zie de [tabel overzicht] [ Overview] artikel voor hulp bij het beheren van uw tabellen.  Dit artikel bevat tevens koppelingen naar meer gedetailleerde onderwerpen zoals [tabel-gegevenstypen][Data types], [distributie van een tabel][Distribute], [een tabel Indexing][Index], [partitioneren van een tabel][Partition], [Maintaining tabelstatistieken] [ Statistics] en [tijdelijke tabellen][Temporary].|

## <a name="polybase"></a>Polybase

|  Probleem                             | Resolutie                                      |
| :----------------------------------| :---------------------------------------------- |
| Fout in UTF-8                        |  PolyBase ondersteunt momenteel alleen gegevensbestanden die UTF-8 gecodeerd zijn geladen.  Zie [werken rond de vereiste PolyBase UTF-8][] voor instructies over het omzeilen van deze beperking.|
| Laden mislukt als gevolg van grote rijen   | Ondersteuning voor grote rij is momenteel niet beschikbaar voor Polybase.  Dit betekent dat als uw tabel VARCHAR(MAX), NVARCHAR(MAX) of VARBINARY(MAX) bevat, externe tabellen kunnen niet worden gebruikt om uw gegevens te laden.  Belasting voor grote rijen wordt momenteel alleen ondersteund via Azure Data Factory (met BCP), Azure Stream Analytics, SSIS, BCP of de klasse SQLBulkCopy .NET. PolyBase ondersteuning voor grote rijen wordt toegevoegd in een toekomstige versie.|
| laden van de tabel met het gegevenstype MAX BCP is mislukt. | Er is een bekend probleem waarvoor dat VARCHAR(MAX), NVARCHAR(MAX) of VARBINARY(MAX) worden geplaatst aan het einde van de tabel in bepaalde scenario's.  Verplaats het maximum aantal kolommen aan het einde van de tabel.|

## <a name="differences-from-sql-database"></a>Verschillen met de SQL-Database

|  Probleem                             | Resolutie                                      |
| :----------------------------------| :---------------------------------------------- |
| Niet-ondersteunde functies van SQL-Database  | Zie de [met niet-ondersteunde tabelfuncties][].|
| Niet-ondersteunde gegevenstypen van SQL-Database  | Zie [niet-ondersteunde gegevenstypen][].|
| DELETE en UPDATE-beperkingen      | Zie [UPDATE oplossingen][], [oplossingen verwijderen][] en [CTAS gebruiken om niet-ondersteunde UPDATE en DELETE syntaxis te omzeilen][].  |
| SAMENVOEGEN-instructie wordt niet ondersteund.   | Zie het [samenvoegen van tijdelijke oplossingen][].|
| Beperkingen van de opgeslagen procedure       | Zie [beperkingen voor opgeslagen procedure][] te begrijpen sommige van de beperkingen van opgeslagen procedures.|
| SELECT-instructies bieden geen ondersteuning voor UDF 's | Dit is een beperking van onze UDF's.  Zie [Functie maken][] voor de syntaxis van de die door ons ondersteund.   |

## <a name="next-steps"></a>Volgende stappen

Als u zijn kan niet zoeken naar een oplossing voor uw probleem hierboven, hier zijn enkele andere bronnen die u kunt proberen.

- [Blogs]
- [Functie-aanvragen]
- [Video 's]
- [Blogs van het productteam kat]
- [Support ticket maken]
- [MSDN-forum]
- [Stack Overflow-forum]
- [Twitter]

<!--Image references-->

<!--Article references-->
[Beveiliging-overzicht]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[SSDT voor Visual Studio 2015]: ./sql-data-warehouse-install-visual-studio.md
[Stuurprogramma's voor Azure SQL datawarehouse]: ./sql-data-warehouse-connection-strings.md
[Verbinding maken met Azure SQL datawarehouse]: ./sql-data-warehouse-connect-overview.md
[Support ticket maken]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Schalen van uw SQL-datawarehouse]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[aanvragen van een verhoging van de quota]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change 
[Leren hoe u kunt uw query's controleren]: ./sql-data-warehouse-manage-monitor.md
[Inrichten van instructies]: ./sql-data-warehouse-get-started-provision.md
[Toegang tot server-firewall configureren voor de client-IP-]: ./sql-data-warehouse-get-started-provision.md#create-a-new-azure-sql-server-level-firewall
[Aanbevolen procedures voor SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Maten tabel]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Met niet-ondersteunde tabelfuncties]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Niet-ondersteunde gegevenstypen]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Slechte columnstore index kwaliteit]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Indexen verbeteren kwaliteit segment opnieuw opbouwen]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Beheer van de werkbelasting]: ./sql-data-warehouse-develop-concurrency.md
[CTAS gebruiken om niet-ondersteunde UPDATE en DELETE syntaxis te omzeilen]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[Tijdelijke oplossingen voor UPDATE]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[VERWIJDEREN van tijdelijke oplossingen]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[Tijdelijke oplossingen samenvoegen]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Beperkingen van de opgeslagen procedure]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Datawarehouse Azure SQL-verificatie]: ./sql-data-warehouse-authentication.md
[Door de vereiste PolyBase UTF-8]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[sys.database_principals]: https://msdn.microsoft.com/library/ms187328.aspx
[FUNCTIE MAKEN]: https://msdn.microsoft.com/library/mt203952.aspx
[Sqlcmd]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogs van het productteam kat]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Functie-aanvragen]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video 's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse


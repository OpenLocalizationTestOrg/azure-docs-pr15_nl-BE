<properties
   pageTitle="Beperkingen van het datawarehouse SQL | Microsoft Azure"
   description="Maximumwaarden voor verbindingen, databases, tabellen en query's voor SQL Data Warehouse."
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
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="sql-data-warehouse-capacity-limits"></a>Beperkingen van het datawarehouse SQL

De volgende tabellen bevatten de maximale waarden toegestaan voor verschillende onderdelen van Azure SQL Data Warehouse.


## <a name="workload-management"></a>Beheer van de werkbelasting

| Categorie            | Beschrijving                                       | Maximum            |
| :------------------ | :------------------------------------------------ | :----------------- |
| [Magazijn eenheden (DWU)][]| Max DWU voor een enkele SQL Data Warehouse | 6000               |
| [Magazijn eenheden (DWU)][]| Max DWU voor één SQL server         | 6000 standaard<br/><br/> Elke SQL server (bijv. myserver.database.windows.net) heeft een DTU Quota 45,000 waarmee tot 6000 DWU. Dit quotum is gewoon een limiet van de veiligheid. U kunt uw quota door te [maken van een support ticket][] en *Quota* te selecteren als het aanvraagtype verhogen.  Voor het berekenen van de DTU nodig heeft, de 7.5 te vermenigvuldigen met het totale dat DWU nodig. U kunt uw huidige DTU verbruik van de SQL server-blade weergeven in de portal. Onderbroken en voortgezet databases tellen naar de DTU-quota. |
| Databaseverbinding | Gelijktijdige sessies actief                          | 1024<br/><br/>Ondersteuning van maximaal 1024 actieve verbindingen, die elk aanvragen om te een SQL Data Warehouse-database op hetzelfde moment indienen kan. Let op: Er zijn beperkingen op het aantal query's dat feitelijk gelijktijdig kan worden uitgevoerd. Wanneer de gelijktijdigheid limiet wordt overschreden, wordt de aanvraag gaat in een interne wachtrij waar wacht op verwerking.|
| Databaseverbinding | Maximale hoeveelheid geheugen voor voorbereide instructies            | 20 MB              |
| [Beheer van de werkbelasting][] | Maximumaantal gelijktijdige query 's                    | 32<br/><br/> Standaard kunnen SQL Data Warehouse maximaal 32 gelijktijdige query's en query's resterende wachtrijen uitvoeren.<br/><br/>Het niveau van de gelijktijdigheid afnemen wanneer gebruikers zijn toegewezen aan een hogere klasse van de resource of het datawarehouse SQL is geconfigureerd met een lage DWU. Sommige query's, zoals query's DMV, altijd mogen worden uitgevoerd.|
| [TempDB][]          | Maximale omvang van Tempdb                                | 399 GB per DW100. Daarom op Tempdb DWU1000 formaat 3,99 TB |


## <a name="database-objects"></a>Database-objecten

| Categorie          | Beschrijving                                  | Maximum            |
| :---------------- | :------------------------------------------- | :----------------- |
| Database          | Maximale grootte                                     | 240 TB gecomprimeerd op schijf<br/><br/>Deze ruimte is onafhankelijk van de ruimte in tempdb of logboek en daarom deze ruimte is gewijd aan het permanente tabellen.  Geclusterde columnstore compressie wordt geschat op 5 X.  Deze compressie kan de database te laten groeien tot ongeveer 1 PB wanneer alle tabellen gegroepeerde columnstore (het tabeltype zijn).|
| Tabel             | Maximale grootte                                     | 60 TB gecomprimeerd op schijf   |
| Tabel             | Tabellen per database                          | 2 miljard          |
| Tabel             | Kolommen per tabel                            | 1024 kolommen       |
| Tabel             | Bytes per kolom                             | Afhankelijk van [het gegevenstype][]van de kolom.  Limiet is 8000 voor de gegevenstypen char, 4000 voor nvarchar of 2 GB voor MAX.|
| Tabel             | Bytes per rij, gedefinieerde grootte                  | 8060 bytes<br/><br/>Het aantal bytes per rij wordt berekend op dezelfde manier als voor de SQL Server met compressie van de pagina. Als SQL Server ondersteunt SQL Data Warehouse overloop rij opslag waarmee de **kolommen met variabele lengte** moet uit de rij worden geplaatst. Wanneer rijen van variabele lengte zijn buiten een rij wordt geduwd, wordt alleen de hoofdmap 24 bytes in het hoofdformulier voor de record opgeslagen. Zie voor meer informatie het MSDN-artikel [overloop rij gegevens van meer dan 8 KB][] .|
| Tabel             | Partities per tabel                    | 15.000<br/><br/>Voor hoge prestaties, raden wij aan tot een minimum beperken het aantal partities u nodig hebt tijdens het nog steeds ondersteuning van uw zakelijke behoeften. Het aantal partities groeit, groeit en trager zorgt ervoor dat de overhead voor de Data Definition Language (DDL) en Data Manipulation Language (DML).|
| Tabel             | Tekens per partitie grenswaarde.| 4000 |
| Index             | Niet-geclusterde indexen per tabel.        | 999<br/><br/>Van toepassing op rowstore alleen tabellen.|
| Index             | Gegroepeerde indexen per tabel.            | 1<br><br/>Van toepassing op zowel rowstore als columnstore.|
| Index             | Sleutelgrootte index.                          | 900 bytes.<br/><br/>Geldt voor alleen indexen rowstore.<br/><br/>Indexen voor varchar kolommen met een maximale grootte van meer dan 900 bytes kunnen worden gemaakt als de bestaande gegevens in de kolommen niet groter is dan 900 bytes wanneer de index wordt gemaakt. Echter later invoegen of UPDATE-acties voor de kolommen die ervoor zorgen dat de totale grootte groter zijn dan 900 bytes, mislukt.|
| Index             | Belangrijke kolommen per index.                   | 16<br/><br/>Geldt voor alleen indexen rowstore. Columnstore geclusterde indexen bevatten alle kolommen.|
| Statistieken        | De grootte van de gecombineerde kolomwaarden.      | 900 bytes.         |
| Statistieken        | Kolommen per object statistieken.           | 32                 |
| Statistieken        | Statistieken op kolommen per tabel gemaakt. | 30.000            |
| Opgeslagen Procedures | Maximum aantal geneste niveaus.               | 8                 |
| Weergave              | Kolommen per weergave                         | 1024             |


## <a name="loads"></a>Belastingen

| Categorie          | Beschrijving                                  | Maximum            |
| :---------------- | :------------------------------------------- | :----------------- |
| Polybase belastingen    | Bytes per rij                                | 32.768<br/><br/>Polybase belastingen zijn beperkt tot het laden van rijen, beide kleiner dan 32K en VARCHR(MAX), NVARCHAR(MAX) of VARBINARY(MAX) kunnen niet worden geladen.  Terwijl deze limiet vandaag bestaat, wordt deze verwijderd vrij snel.<br/><br/>


## <a name="queries"></a>Query 's

| Categorie          | Beschrijving                                  | Maximum            |
| :---------------- | :------------------------------------------- | :----------------- |
| Query             | In de wachtrij query's over gebruikerstabellen.               | 1000               |
| Query             | Gelijktijdige query's over systeemweergaven.          | 100                |
| Query             | In de wachtrij query's over systeemweergaven               | 1000               |
| Query             | Maximale parameters                           | 2098               |
| Batch             | Maximale grootte                                 | 65, 536 4096 *        |
| Selecteer resultaten    | Kolommen per rij                              | 4096<br/><br/>U kunt nooit meer dan 4096 kolommen per rij in het geselecteerde resultaat hebben. Er is geen garantie dat het altijd mogelijk is 4096. Als het queryplan is een tijdelijke tabel vereist, kunnen de 1024 kolommen per tabel maximale toepassen.|
| SELECTEER            | Geneste subquery 's                            | 32<br/><br/>U kunt nooit meer dan 32 geneste subquery's hebben in een SELECT-instructie. Er is geen garantie dat u altijd 32 kan hebben. Een JOIN kan bijvoorbeeld een subquery in het queryplan introduceren. Het aantal van subquery's kan ook worden beperkt door het beschikbare geheugen.|
| SELECTEER            | Kolommen per JOIN                             | 1024 kolommen<br/><br/>U kunt nooit meer dan 1024 kolommen in de JOIN hebt. Er is geen garantie dat het altijd mogelijk is 1024. Als de JOIN plan een tijdelijke tabel met meer dan het resultaat van de JOIN-kolommen vereist, wordt de limiet van 1024 is van toepassing op de tijdelijke tabel. |
| SELECTEER            | Aantal bytes per groep gerangschikt.                  | 8060<br/><br/>De kolommen in de GROUP BY-component kunnen maximaal 8060 bytes hebben.|
| SELECTEER            | Bytes per ORDER BY kolommen                   | 8060 bytes.<br/><br/>De kolommen in de ORDER BY-component kunnen maximaal 8060 bytes hebben.|
| Id's en constanten per stelling | Aantal waarnaar wordt verwezen-id's en -constanten zijn. | 65.535<br/><br/>SQL Data Warehouse beperkt het aantal id's en constanten die kunnen worden opgenomen in een enkele query-expressie. Deze limiet is 65.535. Meer dan dit aantal resultaten in SQL Server-fout 8632. Zie voor meer informatie [interne fout: een expressie services limiet is bereikt][].|


## <a name="metadata"></a>Metagegevens

| Weergeven systeem                        | Maximum aantal rijen |
| :--------------------------------- | :------------|
| sys.dm_pdw_component_health_alerts | 10.000       |
| sys.dm_pdw_dms_cores               | 100          |
| sys.dm_pdw_dms_workers             | Totaal aantal werknemers voor de meest recente 1000 DMS aanvragen voor SQL. |
| sys.dm_pdw_errors                  | 10.000       |
| sys.dm_pdw_exec_requests           | 10.000       |
| sys.dm_pdw_exec_sessions           | 10.000       |
| sys.dm_pdw_request_steps           | Totaal aantal stappen voor de meest recente 1000 SQL aanvragen die zijn opgeslagen in sys.dm_pdw_exec_requests. |
| sys.dm_pdw_os_event_logs           | 10.000       |
| sys.dm_pdw_sql_requests            | De meest recente 1000 SQL-aanvragen die zijn opgeslagen in sys.dm_pdw_exec_requests. |


## <a name="next-steps"></a>Volgende stappen
Voor meer informatie, Zie [: overzicht naslaginformatie SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Magazijn eenheden (DWU)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Datawarehouse SQL: overzicht naslaginformatie]: ./sql-data-warehouse-overview-reference.md
[Beheer van de werkbelasting]: ./sql-data-warehouse-develop-concurrency.md
[TempDB]: ./sql-data-warehouse-tables-temporary.md
[gegevenstype]: ./sql-data-warehouse-tables-data-types.md
[maken van een support ticket]: /sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->
[Overloop rij gegevens van meer dan 8 KB]: https://msdn.microsoft.com/library/ms186981.aspx
[Interne fout: een expressie services limiet is bereikt.]: https://support.microsoft.com/kb/913050

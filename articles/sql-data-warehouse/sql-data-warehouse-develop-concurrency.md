<properties
   pageTitle="Gelijktijdigheid en werkbelasting beheer in SQL Data Warehouse | Microsoft Azure"
   description="Beheer van gelijktijdigheid en werkbelasting in Azure SQL Data Warehouse begrijpen voor het ontwikkelen van oplossingen."
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
   ms.date="09/27/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>Gelijktijdigheid en werkbelasting beheer in SQL Data Warehouse

Voorspelbare om prestaties te bieden op schaal, Microsoft Azure SQL Data Warehouse helpt bepalen u gelijktijdigheid niveaus en resourcetoewijzingen zoals geheugen en CPU-prioriteiten. Dit artikel maakt u kennis met de concepten van gelijktijdigheid en werkbelasting, waarin wordt uitgelegd hoe u beide functies geïmplementeerd en hoe u ze in uw gegevensmagazijn kunt bepalen. SQL werkbelasting Magazijnbeheer is bedoeld om u ondersteuning voor omgevingen met meerdere gebruikers. Het is niet bedoeld voor huurder van meerdere werklasten.

## <a name="concurrency-limits"></a>Gelijktijdigheid limieten

SQL Data Warehouse kunnen maximaal 1.024 gelijktijdige verbindingen. Alle verbindingen van 1024 kunnen gelijktijdig worden query's indienen. SQL Data Warehouse kan echter optimale doorvoersnelheid, wachtrij sommige query's om ervoor te zorgen dat elke query een minimale geheugen subsidie ontvangt. Queuing vindt plaats tijdens uitvoeringstijd voor query. Door queuing query's als gelijktijdigheid limieten zijn bereikt, kunt SQL Data Warehouse verhogen totale doorvoer door ervoor te zorgen dat actieve query's toegang tot ernstige benodigde geheugenbronnen krijgen.  

Gelijktijdigheid grenzen worden bepaald door twee concepten: *gelijktijdige query's* en *gelijktijdigheid sleuven*. Voor een query uit te voeren, moet het uit te voeren in zowel de gelijktijdige query beperken en de toewijzing van de sleuf gelijktijdigheid.

- Gelijktijdige query's worden de query's tegelijk uitvoeren. SQL Data Warehouse ondersteunt maximaal 32 gelijktijdige query's op de grotere DWU.
- Gelijktijdigheid van "slots" worden toegewezen op basis van DWU. Elke 100 DWU biedt 4 sleuven van gelijktijdigheid. Bijvoorbeeld een DW100 toegewezen 4 sleuven van gelijktijdigheid en 40 DW1000 worden toegewezen. Elke query neemt een of meer gelijktijdigheid sleuven, afhankelijk van de [bronklasse](#resource-classes) van de query. Query uitvoeren in de klasse smallrc resource verbruiken een gelijktijdigheid sleuf. Query uitvoeren in een hogere klasse van de resource verbruiken meer gelijktijdigheid slots.

De volgende tabel beschrijft de beperkingen voor gelijktijdige query's en gelijktijdigheid van "slots" op de DWU met verschillende formaten.

### <a name="concurrency-limits"></a>Gelijktijdigheid limieten

|  DWU   | Maximum aantal gelijktijdige query 's  | Gelijktijdigheid sleuven toegewezen |
| :----  | :---------------------: | :-------------------------: |
| DW100  |            4            |                4            |
| DW200  |            8            |                8            |
| DW300  |           12            |               12            |
| DW400  |           16            |               16            |
| DW500  |           20            |               20            |
| DW600  |           24            |               24            |
| DW1000 |           32            |               40            |
| DW1200 |           32            |               48            |
| DW1500 |           32            |               60            |
| DW2000 |           32            |               80            |
| DW3000 |           32            |              120            |
| DW6000 |           32            |              240            |

Wanneer een van deze drempels wordt bereikt, worden nieuwe query's in de wachtrij geplaatst en uitgevoerd op een first-in, first-out basis.  Als een query is voltooid en het aantal query's en -sleuven, onder de valt, worden in de wachtrij query's uitgebracht. 

> [AZURE.NOTE]  *Selectiequery uitvoeren van uitsluitend op het beheer van dynamische weergaven (DMVs) of catalogus* niet worden geregeld door een van de grenzen van gelijktijdigheid. U kunt het-ongeacht het aantal query's uitvoeren op het systeem controleren.

## <a name="resource-classes"></a>Resource-klassen

Resource klassen help die u de toewijzing van geheugen en CPU-cycli gegeven om een query te bepalen. U kunt vier klassen van de resource toewijzen aan een gebruiker in de vorm van *databaserollen*. De resource vier klassen zijn **smallrc**, **mediumrc**, **largerc**en **xlargerc**. Gebruikers in de smallrc krijgen een kleinere hoeveelheid geheugen en kunnen profiteren van de hogere gelijktijdigheid. Daarentegen krijgen gebruikers toegewezen aan xlargerc grote hoeveelheden geheugen en dus minder van hun query's kunnen gelijktijdig worden uitgevoerd.

Elke gebruiker is een lid van de kleine bronklasse, smallrc. De procedure `sp_addrolemember` wordt gebruikt voor het verhogen van de resource-klasse, en `sp_droprolemember` wordt gebruikt voor het verlagen van de klasse van de resource. Met deze opdracht zou bijvoorbeeld de bronklasse van loaduser naar largerc te verhogen:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```

Het wordt aangeraden gebruikers permanent toewijzen aan een resource-klasse in plaats van hun klassen resource te wijzigen. Belasting op geclusterde columnstore tabellen maken bijvoorbeeld hogere kwaliteit indexen wanneer meer geheugen toegewezen. Om ervoor te zorgen dat belasting toegang tot hogere geheugen hebben, een gebruiker specifiek voor het laden van gegevens maken en deze permanent toewijst aan een hogere klasse van de resource.

Er zijn een aantal typen query's die niet van een grotere geheugentoewijzing profiteren. Het systeem zal negeren hun klasse brontoewijzing en altijd deze query's in plaats daarvan in de bronklasse small uitvoeren. Als deze query's worden altijd uitgevoerd in de bronklasse van de kleine, kunnen ze uitgevoerd als gelijktijdigheid sleuven onder druk zijn en ze meer sleuven verbruiken Won't dan nodig is. Zie [Resource class uitzonderingen](#query-exceptions-to-concurrency-limits) voor meer informatie.

Een paar meer details over resources, klasse:

- *Rol wijzigen* machtiging is vereist om de bronklasse van een gebruiker wijzigen.  
- Hoewel u een gebruiker aan een of meer van de hogere klassen van de resource toevoegen kunt, wordt gebruikers uitvoeren op de kenmerken van de hoogste bronklasse waaraan ze zijn toegewezen. Als een gebruiker wordt toegewezen aan mediumrc en largerc, is de hogere bronklasse (largerc) de resource-klasse die wordt ingewilligd.  
- De bronklasse van de beheerder van het systeem kan niet worden gewijzigd.

Zie voor een gedetailleerd voorbeeld [wijzigen gebruiker resource class voorbeeld](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>De toewijzing van geheugen

Er zijn voor- en nadelen voor het verbeteren van de bron van een gebruikersklasse. Verhoging van de klasse van een resource voor een gebruiker geeft hun toegang query's naar het meer geheugen kan betekenen query's sneller uitvoeren.  Echter minder hogere klassen van de resource ook gelijktijdige query's die kunnen worden uitgevoerd. Dit is de verhouding tussen het toewijzen van grote hoeveelheden geheugen aan een enkele query of om andere query's die u moeten ook de geheugentoewijzingen gelijktijdig worden uitgevoerd. Als een gebruiker hoge toewijzing van geheugen voor een query krijgt, hebben andere gebruikers geen toegang tot die hetzelfde geheugen een query uit te voeren.

In de volgende tabel wordt het geheugen dat is toegewezen aan elke distributie door de klasse DWU en resource toegewezen.

### <a name="memory-allocations-per-distribution-mb"></a>Geheugentoewijzingen per distributie (MB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |   100   |    100   |    200  |    400   |
| DW200  |   100   |    200   |    400  |    800   |
| DW300  |   100   |    200   |    400  |    800   |
| DW400  |   100   |    400   |    800  |  1600 euro   |
| DW500  |   100   |    400   |    800  |  1600 euro   |
| DW600  |   100   |    400   |    800  |  1600 euro   |
| DW1000 |   100   |    800   |  1600 euro  |  3200   |
| DW1200 |   100   |    800   |  1600 euro  |  3200   |
| DW1500 |   100   |    800   |  1600 euro  |  3200   |
| DW2000 |   100   |  1600 euro   |  3200  |  6,400   |
| DW3000 |   100   |  1600 euro   |  3200  |  6,400   |
| DW6000 |   100   |  3200   |  6,400  |  12,800  |

Uit de voorgaande tabel ziet u dat er een query uitgevoerd op een DW2000 in de klasse xlargerc resource toegang tot 6400 MB geheugen in elk van de 60 gedistribueerde databases.  In SQL Data Warehouse zijn er 60 verdelingen. Daarom voor het berekenen van de totale hoeveelheid geheugen voor de toewijzing van een query in een bepaalde bronklasse moeten de bovenstaande waarden worden vermenigvuldigd met 60.

### <a name="memory-allocations-system-wide-gb"></a>Geheugen toewijzingen systeemomvattende (GB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |    6    |    6     |    12   |    23    |
| DW200  |    6    |    12    |    23   |    47    |
| DW300  |    6    |    12    |    23   |    47    |
| DW400  |    6    |    23    |    47   |    94    |
| DW500  |    6    |    23    |    47   |    94    |
| DW600  |    6    |    23    |    47   |    94    |
| DW1000 |    6    |    47    |    94   |   188    |
| DW1200 |    6    |    47    |    94   |   188    |
| DW1500 |    6    |    47    |    94   |   188    |
| DW2000 |    6    |    94    |   188   |   375    |
| DW3000 |    6    |    94    |   188   |   375    |
| DW6000 |    6    |   188    |   375   |   750    |

Uit deze tabel van geheugentoewijzingen van het hele systeem, kunt u zien dat een query uitgevoerd op een DW2000 in de klasse xlargerc resource een totaal van 375 GB geheugen wordt toegewezen (verdelingen 6400 MB * 60 * 1024 converteren naar GB) over het geheel van uw SQL-datawarehouse.

## <a name="concurrency-slot-consumption"></a>Gelijktijdigheid sleuf verbruik

SQL Data Warehouse verleent meer geheugen aan query's uitgevoerd in de hogere klassen van de resource. Geheugen is een vaste bron.  Dus hoe meer geheugen per query toegewezen, de minder gelijktijdige query's kunnen uitvoeren. De volgende tabel herhaalt alle van de eerdere concepten in één weergave ziet u het aantal gelijktijdige sleuven beschikbaar door DWU en de sleuven worden gebruikt door de klasse voor elke resource.

### <a name="allocation-and-consumption-of-concurrency-slots"></a>Toewijzing en het verbruik van "slots" gelijktijdigheid

|  DWU   | Maximumaantal gelijktijdige query 's  | Gelijktijdigheid sleuven toegewezen | "Slots" die worden gebruikt door smallrc |  "Slots" die worden gebruikt door mediumrc |  "Slots" die worden gebruikt door largerc |  "Slots" die worden gebruikt door xlargerc |
| :----  | :---------------------: | :-------------------------: | :-----: | :------: | :-----: | :------: |
| DW100  |            4            |                4            |    1    |     1    |    2    |    4     |
| DW200  |            8            |                8            |    1    |     2    |    4    |    8     |
| DW300  |           12            |               12            |    1    |     2    |    4    |    8     |
| DW400  |           16            |               16            |    1    |     4    |    8    |   16     |
| DW500  |           20            |               20            |    1    |     4    |    8    |   16     |
| DW600  |           24            |               24            |    1    |     4    |    8    |   16     |
| DW1000 |           32            |               40            |    1    |     8    |   16    |   32     |
| DW1200 |           32            |               48            |    1    |     8    |   16    |   32     |
| DW1500 |           32            |               60            |    1    |     8    |   16    |   32     |
| DW2000 |           32            |               80            |    1    |    16    |   32    |   64     |
| DW3000 |           32            |              120            |    1    |    16    |   32    |   64     |
| DW6000 |           32            |              240            |    1    |    32    |   64    |  128     |


Uit deze tabel ziet u dat SQL Data Warehouse uitgevoerd als DW1000 maximaal 32 gelijktijdige query's en een totaal van 40 gelijktijdigheid van "slots" worden toegewezen. Als alle gebruikers worden uitgevoerd in smallrc, zou 32 gelijktijdige query's worden toegestaan, omdat elke query 1 sleuf voor gelijktijdigheid zou verbruiken. Als alle gebruikers op een DW1000 in de mediumrc werden uitgevoerd, elke query zou 800 MB worden toegewezen per distributie voor een totale geheugentoewijzing van 47 GB per query gelijktijdigheid zou worden beperkt tot 5 gebruikers (40 gelijktijdigheid sleuven / 8-sleuven per gebruiker mediumrc).

## <a name="query-importance"></a>Belang van de query

SQL Data Warehouse implementeert klassen van de resource met behulp van groepen van de werkbelasting. Er zijn een totaal van acht groepen van belasting die het gedrag van de resource-klassen via het DWU met verschillende formaten beheren. SQL Data Warehouse gebruikt voor alle DWU, slechts vier van de werkbelasting van acht groepen. Dit is logisch omdat elke groep werkbelasting is toegewezen aan een van de vier Bronklassen: smallrc, mediumrc, largerc, of xlargerc. Het belang van inzicht in de werkbelasting van groepen is dat sommige van deze groepen werkbelasting hoger *belang*zijn ingesteld. Belang wordt gebruikt voor CPU plannen. Query's uitvoeren met hoge prioriteit krijgt drie keer meer CPU-cycli dan die met de urgentie Normaal. Daarom bepalen gelijktijdigheid sleuf toewijzingen ook CPU prioriteit. Wanneer een query 16 of meer sleuven verbruikt, wordt MMC uitgevoerd als hoge urgentie.

De volgende tabel toont de toewijzingen belang voor elke groep van de werkbelasting.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Groepstoewijzingen werkbelasting gelijktijdigheid sleuven en belang

| Werkbelasting van groepen | Toewijzing van gelijktijdigheid sleuf | MB / distributie | Toewijzing van belang |
| :-------------- | :----------------------: | :---------------: | :----------------- |
| SloDWGroupC00   |            1             |         100       |       Gemiddeld       |
| SloDWGroupC01   |            2             |         200       |       Gemiddeld       |
| SloDWGroupC02   |            4             |         400       |       Gemiddeld       |
| SloDWGroupC03   |            8             |         800       |       Gemiddeld       |
| SloDWGroupC04   |           16             |       1600 euro       |       Hoog         |
| SloDWGroupC05   |           32             |       3200       |       Hoog         |
| SloDWGroupC06   |           64             |       6,400       |       Hoog         |
| SloDWGroupC07   |          128             |      12,800       |       Hoog         |

U kunt zien dat een DW500 worden gebruikt 1, 4, 8 of 16 gelijktijdigheid sleuven voor smallrc, mediumrc, largerc en xlargerc, respectievelijk uit het diagram de **toewijzing en het verbruik van "slots" gelijktijdigheid** . U kunt deze waarden opzoeken in de voorgaande vinden het belang voor elke resource-klasse.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>Toewijzing van resource klassen op belang DW500

| Resource-klasse | Werkbelasting groep | Gelijktijdigheid sleuven gebruikt | MB / distributie | Belang |
| :------------- | :------------- | :--------------------: | :---------------: | :--------- |
| smallrc        | SloDWGroupC00  |           1            |         100       |   Gemiddeld   |
| mediumrc       | SloDWGroupC02  |           4            |         400       |   Gemiddeld   |
| largerc        | SloDWGroupC03  |           8            |         800       |   Gemiddeld   |
| xlargerc       | SloDWGroupC04  |          16            |        1600 euro      |   Hoog     |


U kunt de volgende DMV query om de verschillen in geheugen Resourcetoewijzing gedetailleerde vanuit het perspectief van de gouverneur van de resource te bekijken of voor het analyseren van actieve en historische gebruik van de werkbelasting groepen bij het oplossen van problemen.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                      AS node_type
    ,pn.pdw_node_id                 AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024              AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                  AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                     AS group_max_dop
    ,wg.effective_max_dop               AS group_effective_max_dop
    ,wg.total_request_count             AS group_total_request_count
    ,wg.total_queued_request_count          AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON  wg.pdw_node_id  = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT  pool_name
,       pool_max_mem_MB
,       group_name
,       group_importance
,       (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,       node_name
,       node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,   group_request_max_memory_grant_pcnt
,   group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Query's die voldoen aan de limieten van gelijktijdigheid

De meeste query's worden beheerst door de klassen van de resource. Deze query's moeten in zowel de gelijktijdige query en gelijktijdigheid sleuf drempels passen. Een gebruiker kan niet uit te sluiten van een query van de sleuf gelijktijdigheidsmodel kiezen.

De volgende instructies inwilligen om vast, resource-klassen:

- INVOEGEN SELECTEREN
- UPDATE
- VERWIJDEREN
- SELECTEREN (als de gebruikerstabellen opvragen)
- ALTER INDEX OPNIEUW MAKEN
- ALTER INDEX HERORGANISEREN
- ALTER TABEL OPNIEUW MAKEN
- INDEX MAKEN
- COLUMNSTORE GECLUSTERDE INDEX MAKEN
- TABEL AS SELECT (CTAS) MAKEN
- Gegevens laden
- Data verkeer bewerkingen uitgevoerd door Data verkeer Service (DMS)

## <a name="query-exceptions-to-concurrency-limits"></a>Uitzonderingen maximumaantal gelijktijdige query

Sommige query's doen niet voldoen aan de klasse van de resource waaraan de gebruiker is toegewezen. Deze uitzonderingen op de gelijktijdigheid grenzen worden aangebracht als de geheugenruimte die nodig zijn voor een bepaalde opdracht laag, vaak zijn omdat de opdracht een Metagegevensbewerking is. Het doel van deze uitzonderingen is groter geheugentoewijzingen voor query's die ze nooit moet voorkomen. In deze gevallen wordt de standaardklasse voor kleine bron (smallrc) altijd gebruikt de klasse werkelijke resource is toegewezen aan de gebruiker. Bijvoorbeeld, `CREATE LOGIN` altijd wordt uitgevoerd in de smallrc. De middelen die nodig zijn om te voldoen aan deze bewerking zijn zeer laag zodat het niet logisch om op te nemen van de query in het model van de sleuf gelijktijdigheid.  Deze query's zijn ook niet beperkt door de limiet van 32-gebruiker gelijktijdigheid, een onbeperkt aantal deze query's kunt uitvoeren om aan de sessielimiet van 1024-sessies.

Resource klassen doen niet ingaan op de volgende instructies:

- CREATE of DROP TABLE
- ALTER TABLE... SWITCH, splitsen of samenvoegen van partitie
- UITSCHAKELEN VAN INDEX WIJZIGEN
- DROP INDEX
- MAKEN, bijwerken of DROP STATISTICS
- TABEL AFKAPPEN
- MACHTIGING WIJZIGEN
- AANMELDING MAKEN
- MAKEN, wijzigen of DROP USER
- MAKEN, wijzigen of DROP PROCEDURE
- CREATE of DROP VIEW
- WAARDEN INVOEGEN
- Selecteer in de systeemweergaven- en DMVs
- LEG UIT
- DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="change-a-user-resource-class-example"></a>Voorbeeld van een gebruiker resources klasse wijzigen

1. **-Aanmelding maken:** Een verbinding met **de database op de SQL-server die als host fungeert voor de SQL Data Warehouse-database** openen en de volgende opdrachten uit te voeren.

    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```

    > [AZURE.NOTE] Het is een goed idee om een gebruiker in de hoofddatabase Azure SQL Data Warehouse-gebruikers maken. Maken van een gebruiker in het model kan een gebruiker aan te melden met behulp van hulpprogramma's zoals SSMS zonder de naam van een database.  Ook kunnen ze het object explorer gebruiken om weer te geven van alle databases op een SQL server.  Zie [een SQL Data Warehouse-database beveiligen][]voor meer informatie over het maken en beheren van gebruikers.

2. **Gebruiker SQL Data Warehouse maken:** Verbinding maken met de **SQL Data Warehouse** -database en voer de volgende opdracht uit.

    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```

3. **Machtigingen:** In het volgende voorbeeld verleent `CONTROL` op de **SQL Data Warehouse** -database. `CONTROL`op de database is niveau het equivalent van de rol db_owner in SQL Server.

    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```

4. **Vergroten resource class:** Gebruik de volgende query op een gebruiker toevoegen aan een hogere werklast rol.

    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```

5. **Verkleinen resource class:** Gebruik de volgende query een gebruiker verwijderen uit een rol werkbelasting.

    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```

    > [AZURE.NOTE] Het is niet mogelijk een gebruiker verwijderen uit de smallrc.

## <a name="queued-query-detection-and-other-dmvs"></a>Detectie van de query in de wachtrij en andere DMVs

U kunt de `sys.dm_pdw_exec_requests` DMV voor query's die in een wachtrij gelijktijdigheid wachten. Wachten op een sleuf gelijktijdige query's krijgen de status **geschorst**.

```sql
SELECT   r.[request_id]              AS Request_ID
        ,r.[status]              AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]              AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Werkbelasting management rollen kunnen worden bekeken met `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

De volgende query ziet u welke rol elke gebruiker is toegewezen.

```sql
SELECT   r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id     = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id   = m.principal_id
WHERE   r.name IN ('mediumrc','largerc', 'xlargerc');
```

SQL Data Warehouse heeft de volgende typen wachten:

- **LocalQueriesConcurrencyResourceType**: query's die buiten het kader van gelijktijdigheid sleuf zit. DMV-query's en het systeem functioneert, zoals `SELECT @@VERSION` zijn voorbeelden van lokale query's.
- **UserConcurrencyResourceType**: query's die binnen het kader van gelijktijdigheid sleuf zit. Query's voor de eindgebruiker tabellen staan voorbeelden van dit brontype wilt gebruiken.
- **DmsConcurrencyResourceType**: wacht ten gevolge van de beweging van gegevensbewerkingen.
- **BackupConcurrencyResourceType**: deze wachttijd geeft aan dat een database is back-up gemaakt. De maximale waarde voor dit brontype is 1. Als u tegelijkertijd meerdere back-ups zijn aangevraagd, de andere wachtrij wordt geplaatst.

De `sys.dm_pdw_waits` DMV kan worden gebruikt om te zien welke bronnen een aanvraag is in behandeling.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,   SESSION_ID()            AS Current_session
,   s.[status]          AS Session_status
,   s.[login_name]
,   s.[query_count]
,   s.[client_id]
,   s.[sql_spid]
,   r.[command]         AS Request_command
,   r.[label]
,   r.[status]          AS Request_status
,   r.[submit_time]
,   r.[start_time]
,   r.[end_compile_time]
,   r.[end_time]
,   DATEDIFF(ms,r.[submit_time],r.[start_time])     AS Request_queue_time_ms
,   DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,   DATEDIFF(ms,r.[end_compile_time],r.[end_time])      AS Request_execution_time_ms
,   r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE   w.[session_id] <> SESSION_ID();
```

De `sys.dm_pdw_resource_waits` DMV ziet u alleen de bron wacht die door een bepaalde query. Wachttijd resource meet alleen de tijd wachten op resources te verstrekken, in plaats van signaal wachttijd is de tijd die nodig is om de onderliggende SQL-servers voor het plannen van de query naar de CPU.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE   [session_id] <> SESSION_ID();
```

De `sys.dm_pdw_wait_stats` DMV kan worden gebruikt voor de historische trendanalyse van wacht.

```sql
SELECT  w.[pdw_node_id]
,       w.[wait_name]
,       w.[max_wait_time]
,       w.[request_count]
,       w.[signal_time]
,       w.[completed_count]
,       w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Volgende stappen

Zie [een SQL Data Warehouse-database beveiligen][]voor meer informatie over het beheren van databasegebruikers en beveiliging. Voor meer informatie over de manier waarop grotere klassen columnstore geclusterde index kwaliteit verbeteren, Zie [Rebuilding indexen om segment kwaliteit te verbeteren].

<!--Image references-->

<!--Article references-->
[Een SQL Data Warehouse-database beveiligen]: ./sql-data-warehouse-overview-manage-security.md
[Opnieuw opbouwen van indexen voor het verbeteren van kwaliteit van segment]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Een SQL Data Warehouse-database beveiligen]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->

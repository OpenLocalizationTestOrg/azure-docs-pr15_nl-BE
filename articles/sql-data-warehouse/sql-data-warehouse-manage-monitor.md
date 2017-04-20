<properties
   pageTitle="Controleren van uw werkbelasting met DMVs | Microsoft Azure"
   description="Informatie over het controleren van uw werkbelasting met DMVs."
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
   ms.date="10/08/2016"
   ms.author="sonyama;barbkess"/>

# <a name="monitor-your-workload-using-dmvs"></a>Controleer uw werkbelasting met DMVs

In dit artikel wordt beschreven hoe dynamische weergaven van beheer (DMVs) gebruiken om te controleren uw werkbelasting en onderzoeken van de uitvoering van de query in Azure SQL Data Warehouse.

## <a name="permissions"></a>Machtigingen

Om de DMVs in dit artikel een query uitvoert, moet u de WEERGAVESTATUS van de DATABASE of BESTURINGSELEMENTEN machtiging. STATUS verlenen DATABASE is meestal de voorkeur machtiging veel restrictiever is.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Verbindingen controleren

Alle aanmeldingen bij de SQL Data Warehouse worden geregistreerd in de [sys.dm_pdw_exec_sessions][].  Deze DMV bevat de laatste 10.000 aanmeldingen.  De session_id is de primaire sleutel en opeenvolgend toegewezen voor elke nieuwe aanmelding.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Uitvoering van de query controleren

Alle query's uitgevoerd op SQL Data Warehouse worden geregistreerd in de [sys.dm_pdw_exec_requests][].  Deze DMV bevat de laatste 10.000 query's uitgevoerd.  De request_id geeft elke query een unieke en is de primaire sleutel voor deze DMV.  De request_id wordt opeenvolgend toegewezen voor elke nieuwe query en wordt voorafgegaan door een QID staat voor query-ID.  Bij het controleren van deze DMV voor een bepaalde session_id geeft alle query's voor een bepaalde aanmelden.

>[AZURE.NOTE] Opgeslagen procedures gebruiken meerdere id's aanvragen.  Aanvraag-id's zijn toegewezen in de juiste volgorde. 

Hier vindt u de stappen volgen om te onderzoeken uitvoeren van queryplannen en tijden voor een bepaalde query.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>STAP 1: Bepaal de query die u wilt onderzoeken

```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

Uit de voorgaande queryresultaten **Opmerking de aanvraag-ID** van de query die u wilt onderzoeken.

Query's in de **onderbroken** toestand wordt in de wachtrij staan vanwege de beperkte vergrendeling. Deze query's worden ook weergegeven in de sys.dm_pdw_waits wachten op query met een soort UserConcurrencyResourceType. Zie [beheer van gelijktijdigheid en werkbelasting][] voor meer details over de grenzen van gelijktijdigheid. Query's kunnen ook voor andere doeleinden, zoals voor object vergrendelingen wachten.  Als uw query een bron wacht is, Zie [Investigating query wacht op bronnen][] verderop in dit artikel.

Ter vereenvoudiging van het opzoeken van een query in de tabel sys.dm_pdw_exec_requests [LABEL][] te gebruiken voor het toewijzen van een opmerking aan de query die u in de weergave van de sys.dm_pdw_exec_requests kan worden opgezocht.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>STAP 2: Het queryplan onderzoeken

Met de aanvraag-ID uit te halen plan voor gedistribueerde SQL (DSQL) van de query [sys.dm_pdw_request_steps][].

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Wanneer een DSQL plan langer duurt dan verwacht, de oorzaak kan liggen in een complex plan met veel DSQL stappen of slechts één stap duurt erg lang.  Als het plan veel stappen met verschillende bewerkingen voor het verplaatsen is, kunt u uw tabel verdelingen om verplaatsing van gegevens optimaliseren. De [verdeling van de tabel][] artikel wordt uitgelegd waarom gegevens moet worden verplaatst naar het oplossen van een query en sommige distributiestrategieën om verplaatsing van gegevens wordt uitgelegd.

Details over één stap, de kolom *operation_type* van de stap van langdurige query verder onderzoeken en Let op de **Index stap**:

- Wilt u doorgaan met stap 3a voor **SQL-bewerkingen**: OnOperation, RemoteOperation, ReturnOperation.
- Wilt u doorgaan met stap 3b voor **bewerkingen verplaatsen van gegevens**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>STAP 3a: SQL onderzoeken op de gedistribueerde databases

De aanvraag-ID en de Index stap voor het ophalen van gegevens van [sys.dm_pdw_sql_requests][], met informatie van de uitvoering van de query-stap op alle databases gedistribueerde gebruiken.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Wanneer de stap van de query wordt uitgevoerd, kan [DBCC PDW_SHOWEXECUTIONPLAN][] worden gebruikt voor het ophalen van het geschatte plan van SQL Server uit de cache voor het plan van SQL Server voor de stap die wordt uitgevoerd op een bepaalde verdeling.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>STAP 3b: verplaatsing van gegevens op de gedistribueerde databases onderzoeken

De aanvraag-ID en de Index stap gebruiken voor het ophalen van informatie over een gegevensverplaatsing stap uitgevoerd op elke verdeling van [sys.dm_pdw_dms_workers][].

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

- Controleer de kolom *total_elapsed_time* om te zien als een bepaalde verdeling aanzienlijk langer duurt dan anderen voor verplaatsing van gegevens.
- Voor de distributie van langdurige, Controleer de kolom *rows_processed* of het aantal rijen dat wordt verplaatst van deze verdeling is aanzienlijk groter dan de andere. Als dit het geval is, betekent dit waarschijnlijk scheeftrekken van de onderliggende gegevens.

Als de query wordt uitgevoerd, kan [DBCC PDW_SHOWEXECUTIONPLAN][] worden gebruikt voor het ophalen van het geschatte plan van SQL Server uit de Server SQL plan cache voor de actieve SQL-stap binnen een bepaalde verdeling.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>
## <a name="monitor-waiting-queries"></a>Wachten op query's controleren

Als u ontdekt dat uw query niet voortgang boekt omdat deze voor een resource wacht, is dit een query waarmee u alle resources wacht op een query.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Als de query nog actief voor resources uit een andere query, worden de staat **AcquireResources**.  Als de query de vereiste bronnen heeft, worden de staat **verleend**.

## <a name="next-steps"></a>Volgende stappen
Zie [systeem-weergaven][] voor meer informatie over DMVs.
Zie [Aanbevolen procedures voor SQL Data Warehouse][] voor meer informatie over aanbevolen procedures

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[Aanbevolen procedures voor SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Systeemweergaven]: ./sql-data-warehouse-reference-tsql-system-views.md
[Verdeling van de tabel]: ./sql-data-warehouse-tables-distribute.md
[Gelijktijdigheid en werkbelasting]: ./sql-data-warehouse-develop-concurrency.md
[Onderzoek naar query's wachten op resources]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[LABEL]: https://msdn.microsoft.com/library/ms190322.aspx

<properties
   pageTitle="Azure SQL-Database met behulp van beheer van dynamische weergaven Monitoring | Microsoft Azure"
   description="Informatie over het opsporen en onderzoeken van algemene prestatieproblemen met behulp van beheer van dynamische weergaven voor het controleren van Microsoft Azure SQL-Database."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/20/2016"
   ms.author="carlrab"/>

# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Azure SQL-Database met behulp van beheer van dynamische weergaven controleren

Microsoft Azure SQL-Database kunt een subset van het beheer van dynamische weergaven voor het onderzoeken van prestatieproblemen, door een geblokkeerde of langdurige query's, bron knelpunten en slechte queryplannen veroorzaakt mogelijk. Dit onderwerp bevat informatie over hoe u veelvoorkomende problemen met de prestaties te detecteren met behulp van beheer van dynamische weergaven.

SQL-Database wordt gedeeltelijk ondersteunt drie soorten weergaven van dynamische management:

- Beheer van dynamische database gerelateerde weergaven.
- Dynamische management uitvoering gerelateerde weergaven.
- Dynamische management transactie gerelateerde weergaven.

Zie voor gedetailleerde informatie over het beheer van dynamische weergaven, [dynamische weergaven van Management en functies (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) in SQL Server Books Online.

## <a name="permissions"></a>Machtigingen

In een SQL-Database bij het controleren van een weergave van het beheer van dynamische **DATABASE WEERGAVESTATUS** zijn machtigingen nodig. De machtiging **DATABASE status** geeft informatie over alle objecten in de huidige database.
Uitvoeren om de **DATABASE WEERGAVESTATUS** machtiging aan een bepaalde database-gebruiker, met de volgende query:

```GRANT VIEW DATABASE STATE TO database_user; ```

Beheer van dynamische weergaven retourneren in een exemplaar van de SQL-Server voor gebouwen, informatie over de status van de server. In SQL-Database retourneert ze informatie met betrekking tot de huidige logische-database alleen.

## <a name="calculating-database-size"></a>Berekening van de grootte van de database

De volgende query geeft als resultaat de grootte van de database (in MB):

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

De volgende query geeft als resultaat de grootte van afzonderlijke objecten (in MB) in de database:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Verbindingen controleren

De weergave van de [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) kunt u informatie opvragen over de verbindingen naar een specifieke Azure SQL-databaseserver en de details van elke verbinding. Bovendien worden de [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) weergave is handig bij het ophalen van informatie over alle actieve gebruikersverbindingen en interne taken.
Informatie over de huidige verbinding met de volgende query opgehaald:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [AZURE.NOTE] Wanneer de **sys.dm_exec_requests** en **sys.dm_exec_sessions-weergaven**, als u gemachtigd **WEERGAVESTATUS DATABASE** op de database wordt uitgevoerd, ziet u alle uitvoerende sessies op de database. anders, ziet u alleen de huidige sessie.

## <a name="monitoring-query-performance"></a>Prestaties van query's controleren

Trage of lange query's uitvoeren, kan veel systeembronnen vergen. In deze sectie wordt beschreven hoe beheer van dynamische weergaven gebruiken voor het detecteren van een aantal algemene query prestatieproblemen. De verwijzing naar een oudere maar nog steeds nuttig voor het oplossen van problemen is het artikel voor [Het oplossen van prestatieproblemen in SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) van Microsoft TechNet.

### <a name="finding-top-n-queries"></a>Top N query's zoeken

In het volgende voorbeeld geeft als resultaat informatie over de bovenste vijf query's gerangschikt door de gemiddelde CPU-tijd. In dit voorbeeld aggregeert de query's de query-hash, dat logisch gezien gelijk query's worden gegroepeerd door hun cumulatieve resourceverbruik.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Geblokkeerde query's controleren

Langzame of langdurige query's kunnen bijdragen aan overmatig verbruik en het gevolg was van geblokkeerde query's. De oorzaak van de blokkering is slecht ontwerp, slechte queryplannen, het gebrek aan nuttige indexen, enzovoort. De weergave van de sys.dm_tran_locks kunt u informatie opvragen over de huidige activiteit van de vergrendeling in de Azure SQL-Database. Bijvoorbeeld de code, Zie [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) in SQL Server Books Online.

### <a name="monitoring-query-plans"></a>Bewakingsprogramma's query

Een inefficiënte queryplan kan ook CPU-verbruik verhogen. Het volgende voorbeeld wordt de weergave van de [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) om te bepalen welke query het meest cumulatieve CPU gebruikt.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Zie ook

[Inleiding tot SQL-Database](sql-database-technical-overview.md)

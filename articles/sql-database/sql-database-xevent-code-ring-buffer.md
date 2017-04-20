<properties 
    pageTitle="XEvent Ring Buffer code voor SQL-Database | Microsoft Azure" 
    description="Bevat een codevoorbeeld Transact-SQL die eenvoudig en snel door middel van het doel Ring Buffer in Azure SQL-Database is gemaakt." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Ring Buffer doel code voor uitgebreide gebeurtenissen in de SQL-Database

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Wilt u een volledige voorbeeldcode voor het snelle eenvoudigste informatie vastleggen en rapporteren voor een uitgebreide gebeurtenis tijdens een test. De gemakkelijkste doelwit voor uitgebreide gegevens is het [doel van de Buffer van de Ring](http://msdn.microsoft.com/library/ff878182.aspx).


Dit onderwerp bevat een codevoorbeeld Transact-SQL die:


1. Een tabel met gegevens om aan te tonen met gemaakt.

2. Een sessie gemaakt voor een bestaande uitgebreide gebeurtenis, namelijk **sqlserver.sql_statement_starting**.
    - De gebeurtenis is beperkt tot SQL-instructies die een bepaalde Update tekenreeks bevatten: **instructie als 'UPDATE tabEmployee %'**.
    - Kiest voor het verzenden van de uitvoer van de gebeurtenis met een doel van het type Ring Buffer, namelijk **package0.ring_buffer**.

3. De gebeurtenis-sessie start.

4. Problemen met een paar eenvoudige SQL UPDATE-instructies.

5. Problemen met een SQL SELECT output gebeurtenis ophalen uit de Buffer van de Ring.
    - **sys.dm_xe_database_session_targets** en andere dynamische management (DMVs) zijn gekoppeld.

6. Hiermee beëindigt u de sessie van de gebeurtenis.

7. Het doel van de Ring-Buffer, om bronnen vrij te vallen.

8. De sessie van de gebeurtenis en de tabel demo daalt.


## <a name="prerequisites"></a>Vereisten


- Azure-account en abonnement. U kunt zich aanmelden voor een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).


- De database die kunt u een tabel in.
 - Desgewenst kunt u [een demonstratie **AdventureWorksLT** database maken](sql-database-get-started.md) in minuten.


- SQL Server Management Studio (ssms.exe), in het ideale geval meest recente maandelijkse update versie. U kunt de meest recente ssms.exe van downloaden:
 - [Download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx)-onderwerp.
 - [Een directe link naar de download.](http://go.microsoft.com/fwlink/?linkid=616025)


## <a name="code-sample"></a>Voorbeeld van code


Met zeer kleine wijziging, kan het volgende codevoorbeeld van Ring Buffer op Azure SQL-Database of Microsoft SQL Server worden uitgevoerd. Het verschil is de aanwezigheid van het knooppunt '_database' in sommige weergaven dynamic management (DMVs), die worden gebruikt in de FROM-component in stap 5. Bijvoorbeeld:

- sys.dm_xe**_database**_session_targets
- sys.dm_xe_session_targets


&nbsp;


```
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;


## <a name="ring-buffer-contents"></a>Ring Buffer inhoud


We ssms.exe uit te voeren in het codevoorbeeld gebruikt.


U kunt de resultaten, we hebt geklikt op de cel onder de kop van kolom **target_data_XML**.

Klik in het resultatendeelvenster we hebt geklikt op de cel onder de kop van kolom **target_data_XML**. Klikt u op een ander tabblad van het bestand in waarin de inhoud van de resultaatcel werd weergegeven als XML-ssms.exe gemaakt.


De uitvoer wordt weergegeven in het volgende blok. Het lijkt lang, maar het is slechts twee **<event>** elementen.


&nbsp;


```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a>Laat de bronnen die door de Buffer van de Ring


Wanneer u klaar bent met uw ringbuffer, kunt u verwijderen en laat de afgifte van een **ALTER** als de volgende bronnen:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


De definitie van de gebeurtenis sessie is bijgewerkt, maar niet verwijderd. Later kunt u een ander exemplaar van de Buffer Ring toevoegen aan de gebeurtenis sessie:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>Meer informatie


Het primaire onderwerp voor uitgebreide gebeurtenissen op Azure SQL-Database is:


- [Uitgebreide overwegingen voor gebeurtenissen in de SQL-Database](sql-database-xevent-db-diff-from-svr.md), die verschilt van sommige aspecten van de uitgebreide gebeurtenissen die tussen Azure SQL-Database versus een Microsoft SQL Server verschillen.


Andere code monster onderwerpen voor uitgebreide gebeurtenissen zijn beschikbaar op de volgende koppelingen. Echter, moet u regelmatig een voorbeeld om te zien of het monster is bedoeld voor Microsoft SQL Server versus Azure SQL-Database controleren. Vervolgens kunt u beslissen of kleine wijzigingen nodig zijn voor het uitvoeren van de steekproef.


- Voorbeeldcode voor Azure SQL-Database: [code van de gebeurtenisbestand doel voor uitgebreide gebeurtenissen in de SQL-Database](sql-database-xevent-code-event-file.md)


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->

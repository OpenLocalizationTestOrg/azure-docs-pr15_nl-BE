<properties
    pageTitle="Query over cloud databases met verschillende schema | Microsoft Azure"
    description="het instellen van cross-databasequery's via verticale partities"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/27/2016"
    ms.author="torsteng" />

# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Vragen over cloud-databases met verschillende schema's (voorbeeld)

![Vragen over tabellen in andere databases][1]

Verticaal gepartitioneerd databases gebruiken verschillende sets van tabellen in verschillende databases. Dit betekent dat het schema op verschillende databases verschilt. Bijvoorbeeld zijn alle tabellen voor de voorraad op de ene database alle accounting-gerelateerde tabellen zijn op een tweede database. 

## <a name="prerequisites"></a>Vereisten

* De gebruiker moet beschikken over de machtiging van een externe gegevensbron VERANDEREN. Deze machtiging is opgenomen in de machtiging DATABASE wijzigen.
* EEN externe gegevensbron VERANDEREN machtigingen nodig zijn om te verwijzen naar de onderliggende gegevensbron.

## <a name="overview"></a>Overzicht

**Opmerking**: in tegenstelling tot met horizontaal partitioneren deze DDL-instructies niet afhankelijk van een gegevenslaag met een shard toewijzing via de clientbibliotheek elastische database definiëren.

1. [HOOFDSLEUTEL MAKEN](https://msdn.microsoft.com/library/ms174382.aspx)
2. [MAKEN VAN DE DATABASE BINNEN HET BEREIK VAN REFERENTIES](https://msdn.microsoft.com/library/mt270260.aspx)
3. [EXTERNE GEGEVENSBRON MAKEN](https://msdn.microsoft.com/library/dn935022.aspx)
4. [EXTERNE TABEL MAKEN](https://msdn.microsoft.com/library/dn935021.aspx) 


## <a name="create-database-scoped-master-key-and-credentials"></a>Maken binnen het bereik van databasehoofdsleutel en referenties 

De referentie wordt gebruikt door de elastische query verbinding maken met uw externe databases.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
**Opmerking**    Ervoor te zorgen dat de *<username>* niet *"@servername"* achtervoegsel. 

## <a name="create-external-data-sources"></a>Externe gegevensbronnen maken

Syntaxis:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

**Belangrijk**   De parameter TYPE moet worden ingesteld op **RDBMS**. 

### <a name="example"></a>Voorbeeld 

In het volgende voorbeeld illustreert het gebruik van de CREATE-instructie voor externe gegevensbronnen. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 
 
De lijst met huidige externe gegevensbronnen ophalen: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Externe tabellen 

Syntaxis:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 
    
    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Voorbeeld  

    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

In het volgende voorbeeld ziet u hoe de lijst met externe tabellen ophalen uit de huidige database: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Opmerkingen

Elastische query breidt de syntaxis van de bestaande externe tabel als externe tabellen die gebruikmaken van externe bronnen van het type RDBMS definiëren. De definitie van een externe tabel voor het partitioneren van verticale heeft betrekking op de volgende aspecten: 

* **Schema**: de externe tabel DDL definieert een schema dat uw query's kunnen gebruiken. Het schema dat is opgegeven in de definitie van de externe tabel moet overeenkomen met het schema van de tabellen in de externe database waar de werkelijke gegevens worden opgeslagen. 

* **Verwijzing naar externe database**: de externe tabel DDL naar een externe gegevensbron verwijst. De externe gegevensbron geeft de servernaam van de logische en de naam van de externe database waar de werkelijke tabelgegevens worden opgeslagen. 

Met een externe gegevensbron, zoals wordt beschreven in de vorige sectie, is de syntaxis voor het maken van externe tabellen als volgt: 

De DATA_SOURCE-component definieert de externe gegevensbron (dat wil zeggen de externe database in het geval van verticale partities) die wordt gebruikt voor de externe tabel.  

De SCHEMA_NAME en objectnaam clausules bieden de mogelijkheid de definitie van de externe tabel aan een tabel in een ander schema op de externe database, of een tabel met een andere naam, respectievelijk toewijzen. Dit is handig als u een externe tabel aan een catalogus of DMV definiëren op de externe database – of een andere situatie waarin de naam van de externe tabel is al lokaal.  

De volgende DDL-instructie daalt de definitie van een bestaande externe tabel uit de lokale catalogus. Dit heeft geen gevolgen voor de externe database. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Machtigingen voor externe tabel maken en neerzetten**: machtigingen voor elke externe gegevensbron wijzigen voor een externe tabel DDL die ook nodig is om te verwijzen naar de onderliggende gegevensbron nodig zijn.  

## <a name="security-considerations"></a>Beveiligingsoverwegingen
Gebruikers met toegang tot de externe tabel toegang automatisch tot de onderliggende externe tabellen onder de referentie vermeld in de definitie van de externe gegevensbron. U moet toegang tot de externe tabel zorgvuldig beheren om te voorkomen dat ongewenste misbruik van bevoegdheden door de referentie van de externe gegevensbron. Gewone SQL machtigingen kunnen worden gebruikt om toegang te verlenen of INTREKKEN van toegang tot een externe tabel net alsof het een normale tabel.  


## <a name="example-querying-vertically-partitioned-databases"></a>Voorbeeld: verticaal zoeken databases gepartitioneerd 

De volgende query wordt uitgevoerd een drieweg-join tussen de twee lokale tabellen voor orders en orderregels en de externe tabel voor klanten. Dit is een voorbeeld van de verwijzing use-case voor elastische query gegevens: 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Opgeslagen procedure voor de uitvoering van externe T-SQL: sp\_execute_remote

Elastische query ook een opgeslagen procedure die rechtstreeks toegang tot de shards biedt geïntroduceerd. De naam van de opgeslagen procedure [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) en kan worden gebruikt voor extern opgeslagen procedures of T-SQL-code uit te voeren op externe databases. De volgende parameters nodig: 

* Naam van de gegevensbron (nvarchar): de naam van de externe gegevensbron van het type RDBMS. 
* Query (nvarchar): de T-SQL-query moet worden uitgevoerd op elke shard. 
* Parameterdeclaratie (nvarchar) - optioneel: tekenreeks van het typedefinities voor de parameters die worden gebruikt in de Query-parameter (zoals sp_executesql). 
* Waarde parameterlijst - optioneel: door komma's gescheiden lijst met waarden van de parameters (zoals sp_executesql).

De sp\_uitvoeren\_afstandsbediening gebruikt de externe gegevensbron die is opgegeven in de Aanroepparameters de gegeven T-SQL-instructie uitvoeren op externe databases. De referentie van de externe gegevensbron wordt gebruikt voor verbinding met de shardmap manager-database en de externe databases.  

Voorbeeld: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 


  
## <a name="connectivity-for-tools"></a>Connectiviteit voor hulpprogramma 's

Gewone tekenreeksen voor SQL Server kunt u verbinding maken met uw gegevens en BI-integratiefuncties databases op de server SQL DB met elastische query is ingeschakeld en de externe tabellen zijn gedefinieerd. Zorg ervoor dat de SQL Server als gegevensbron voor het hulpprogramma wordt ondersteund. Raadpleeg vervolgens de querydatabase elastisch en de externe tabellen net als elke andere SQL Server-database waarmee u verbinding met het gereedschap maken wilt. 

## <a name="best-practices"></a>Aanbevolen procedures 
 
* Zorg ervoor dat de querydatabase elastische eindpunt toegang heeft gekregen tot de externe database door access voor Azure Services inschakelen in de firewallconfiguratie SQL DB. Ook voor zorgen dat de referenties die in de definitie van de externe gegevensbron bij de externe database aanmelden kunt en de machtigingen voor toegang tot de externe tabel heeft.  

* Elastische query werkt het beste voor query's waarin de meeste van de berekening kan worden uitgevoerd op de externe databases. Meestal krijgt u de beste prestaties van query's met selectieve filter predikaten die op de externe databases of joins die volledig kunnen worden uitgevoerd op de externe database kan worden geëvalueerd. Andere query patronen mogelijk grote hoeveelheden gegevens uit een externe database laden en goed. 


## <a name="next-steps"></a>Volgende stappen

Zie horizontaal gepartitioneerde databases (ook bekend als een laptopgeheugen databases) opvraagt, [query's via een laptopgeheugen wolk databases (horizontaal partitioneren)](sql-database-elastic-query-horizontal-partitioning.md).

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->

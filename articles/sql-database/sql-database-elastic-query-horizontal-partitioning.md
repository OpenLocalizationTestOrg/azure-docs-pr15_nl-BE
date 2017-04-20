<properties
    pageTitle="Rapportage over schaalvergroting wolk databases | Microsoft Azure"
    description="elastische query's via horizontale partities instellen"    
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

# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Rapportage over schaalvergroting wolk databases (voorbeeld)

![Vragen over shards][1]

Een laptopgeheugen databases rijen verdelen over de gegevens van een schaal van laag. Het schema is identiek voor alle deelnemende databases, ook wel bekend als horizontaal partitioneren. Met behulp van een elastische query, kunt u rapporten die gelden voor alle databases in een database een laptopgeheugen.

Zie [rapportage over databases schaalvergroting cloud](sql-database-elastic-query-getting-started.md)voor een snelle start.

Zie voor een niet-laptopgeheugen databases, [Query via cloud-databases met verschillende schema's](sql-database-elastic-query-vertical-partitioning.md). 

 
## <a name="prerequisites"></a>Vereisten

* Een shard kaart met de elastische database client-bibliotheek maken. Zie [Shard beheer toewijzen](sql-database-elastic-scale-shard-map-management.md). Of gebruik het monster app in [aan de slag met elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md).
* Zie ook [de bestaande databases migreren naar schaalvergroting databases](sql-database-elastic-convert-to-use-elastic-tools.md).
* De gebruiker moet beschikken over de machtiging van een externe gegevensbron VERANDEREN. Deze machtiging is opgenomen in de machtiging DATABASE wijzigen.
* EEN externe gegevensbron VERANDEREN machtigingen nodig zijn om te verwijzen naar de onderliggende gegevensbron.

## <a name="overview"></a>Overzicht

De weergave van de metagegevens van de gegevenslaag een laptopgeheugen maken deze instructies in de querydatabase elastisch. 


1. [HOOFDSLEUTEL MAKEN](https://msdn.microsoft.com/library/ms174382.aspx)
2. [MAKEN VAN DE DATABASE BINNEN HET BEREIK VAN REFERENTIES](https://msdn.microsoft.com/library/mt270260.aspx)
3. [EXTERNE GEGEVENSBRON MAKEN](https://msdn.microsoft.com/library/dn935022.aspx)
4. [EXTERNE TABEL MAKEN](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 maken binnen het bereik van databasehoofdsleutel en referenties 

De referentie wordt gebruikt door de elastische query verbinding maken met uw externe databases.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
>[AZURE.NOTE] Zorg ervoor dat de *'\<gebruikersnaam\>"* omvat geen *"@servername"* achtervoegsel. 

## <a name="12-create-external-data-sources"></a>1.2 externe gegevensbronnen maken

Syntaxis:

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                            
            (TYPE = SHARD_MAP_MANAGER,
                    LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a>Voorbeeld 

    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );
 
De lijst met huidige externe gegevensbronnen ophalen: 

    select * from sys.external_data_sources; 

De kaart shard wordt verwezen naar de externe gegevensbron. Een elastische query wordt de gegevensbron en de onderliggende shard kaart inventariseren van de databases die deelnemen aan de gegevenslaag. Dezelfde referenties worden gebruikt voor het lezen van de plattegrond shard en toegang tot de gegevens op de shards tijdens de verwerking van een elastische query. 

## <a name="13-create-external-tables"></a>1.3 de externe tabellen maken 
 
Syntaxis:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  
    
    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Voorbeeld**

    CREATE EXTERNAL TABLE [dbo].[order_line]( 
         [ol_o_id] int NOT NULL, 
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL, 
         [ol_number] tinyint NOT NULL, 
         [ol_i_id] int NOT NULL, 
         [ol_delivery_d] datetime NOT NULL, 
         [ol_amount] smallmoney NOT NULL, 
         [ol_supply_w_id] int NOT NULL, 
         [ol_quantity] smallint NOT NULL, 
         [ol_dist_info] char(24) NOT NULL 
    ) 
    
    WITH 
    ( 
        DATA_SOURCE = MyExtSrc, 
        SCHEMA_NAME = 'orders', 
        OBJECT_NAME = 'order_details', 
        DISTRIBUTION=SHARDED(ol_w_id)
    ); 

De lijst met externe tabellen ophalen uit de huidige database: 

    SELECT * from sys.external_tables; 

Externe tabellen verwijderen:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Opmerkingen

De gegevens\_SOURCE component definieert de externe gegevensbron (een kaart met de shard) die wordt gebruikt voor de externe tabel.  

Het SCHEMA\_naam en het OBJECT\_naam clausules definitie van de externe tabel koppelen aan een tabel in een ander schema. Als dit argument wordt weggelaten, het schema van een extern object wordt aangenomen dat 'dbo' en de naam ervan wordt aangenomen dat identiek is aan de naam van de externe tabel wordt gedefinieerd. Dit is handig als u de naam van de externe tabel is al in de database waar u de externe tabel maken. Bijvoorbeeld, u wilt een externe tabel als u een samengevoegde weergave van de catalogusweergaven definiëren of trapsgewijs DMVs op de schaal van gegevens. Sinds de catalogusweergaven en DMVs lokaal bestaat, kunt u hun namen voor de definitie van de externe tabel niet gebruiken. In plaats daarvan gebruikt u een andere naam en gebruik van de catalogusweergave of van de DMV naam in het SCHEMA\_naam en/of het OBJECT\_naam componenten. (Zie het voorbeeld hieronder). 

De distributie-component bepaalt hoe de gegevens voor deze tabel gebruikt. De queryprocessor maakt gebruik van de informatie in de distributie-component te maken van de meest efficiënte queryplannen.  

1. **SHARDED** betekent dat gegevens horizontaal over de databases is gepartitioneerd. De partitionering sleutel voor de verdeling van de gegevens is de parameter **< sharding_column_name >** .
2. **GEREPLICEERDE** betekent dat identieke kopieën van de tabel op elke database aanwezig zijn. Het is uw verantwoordelijkheid om ervoor te zorgen dat de replica's identiek op de databases zijn.
3. **Ronde\_ROBIN** betekent dat de tabel horizontaal is gepartitioneerd met behulp van een toepassing afhankelijk van de distributiemethode. 

**Gegevens trapsgewijs verwijzing**: de externe tabel DDL naar een externe gegevensbron verwijst. De externe gegevensbron geeft een overzicht van shard waarmee de externe tabel met de gegevens die nodig zijn om te zoeken naar alle databases in de gegevenslaag. 


### <a name="security-considerations"></a>Beveiligingsoverwegingen 

Gebruikers met toegang tot de externe tabel toegang automatisch tot de onderliggende externe tabellen onder de referentie vermeld in de definitie van de externe gegevensbron. Voorkom ongewenste misbruik van bevoegdheden door de referentie van de externe gegevensbron. Gebruik GRANT of REVOKE voor een externe tabel net alsof het een normale tabel.  

Als u de externe gegevensbron en de externe tabellen hebt gedefinieerd, kunt u nu volledige T-SQL via de externe tabellen gebruiken.

## <a name="example-querying-horizontal-partitioned-databases"></a>Voorbeeld: bij het controleren van horizontale gepartitioneerde databases 

De volgende query wordt uitgevoerd een drieweg-join tussen magazijnen, orders en orderregels en verschillende aggregaten en een selectieve filter gebruikt. Er wordt aangenomen (1) horizontaal partitionering (sharding) en (2) dat magazijnen, orders en regels een laptopgeheugen van de id-kolom voor magazijn zijn en dat de elastische query kan samen de verbindingen in de shards vinden en de dure deel uit van de query op de shards parallel verwerken. 

    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount, 
         min(ol_delivery_d) as min_deliv_date
    from warehouse 
    join orders 
    on w_id = o_w_id
    join order_line 
    on o_id = ol_o_id and o_w_id = ol_w_id 
    where w_id > 100 and w_id < 200 
    group by w_id, o_c_id 
 
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

Gebruik gewone tekenreeksen voor SQL Server verbinding maken met uw toepassing, de integratieprogramma's voor BI en gegevens naar de database met de definities van de externe tabel. Zorg ervoor dat de SQL Server als gegevensbron voor het hulpprogramma wordt ondersteund. Vervolgens verwijzing naar de querydatabase elastisch net als elke andere SQL Server-database verbonden met het hulpprogramma en de externe tabellen gebruik van het hulpprogramma of toepassing als lokale tabellen. 

## <a name="best-practices"></a>Aanbevolen procedures 

* Zorg ervoor dat de querydatabase elastische eindpunt toegang heeft gekregen tot de database shardmap en alle shards via de firewalls SQL DB.  

* Valideren of af te dwingen de verdeling van de gegevens die door de externe tabel zijn gedefinieerd. Als u de verdeling van de werkelijke gegevens verschilt van de verdeling die is opgegeven in de definitie van de tabel, kunnen de query's onverwachte resultaten opleveren. 

* Elastische query voert momenteel geen shard schrapping als predikaten via de sleutel sharding zou toestaan dat bepaalde shards veilig uitsluiten van de verwerking.

* Elastische query werkt het beste voor query's waarin de meeste van de berekening kan worden uitgevoerd op de shards. Meestal krijgt de beste prestaties van query's met selectieve filter predikaten die kan worden geëvalueerd op de shards of joins via de partitionering toetsen die kunnen worden uitgevoerd op een manier partitie uitgelijnd op alle shards. Andere patronen query mogelijk grote hoeveelheden gegevens laden vanuit de shards naar het knooppunt head en goed

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->

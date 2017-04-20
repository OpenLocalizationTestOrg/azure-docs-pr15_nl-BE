<properties
    pageTitle="Geo-replicatie configureren voor Azure SQL-Database met Transact-SQL | Microsoft Azure"
    description="Geo-replicatie configureren voor Azure SQL-Database met behulp van Transact-SQL"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="NA"
    ms.date="10/13/2016"
    ms.author="carlrab"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-transact-sql"></a>Geo-replicatie configureren voor Azure SQL-Database met Transact-SQL

> [AZURE.SELECTOR]
- [Overzicht](sql-database-geo-replication-overview.md)
- [Azure Portal](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

In dit artikel wordt beschreven hoe u actieve Geo-replicatie configureren voor een Azure SQL-Database met Transact-SQL.

Als u wilt tot stand brengen met behulp van Transact-SQL failover, Zie [een failover- of niet gepland voor Azure SQL-Database met Transact-SQL](sql-database-geo-replication-failover-transact-sql.md).

>[AZURE.NOTE] Actieve Geo-replicatie (secundaire servers kan worden gelezen) is nu beschikbaar voor alle databases in alle Servicelagen. In April 2017 het secundaire niet-leesbare type wordt ingetrokken en de bestaande niet-leesbare databases automatisch worden bijgewerkt naar secundaire servers voor kan worden gelezen.

Actieve Geo-replicatie configureren met behulp van Transact-SQL, moet u het volgende:

- Een abonnement op Azure.
- Een logische Azure SQL-Database-server <MyLocalServer> en een SQL-database <MyDB> -de primaire database die u wilt repliceren.
- Een of meer logische Azure SQL Database servers < MySecondaryServer(n) > - het logische servers die de partnerservers waarin u secundaire databases maakt.
- Een aanmelding die in de primaire, DBManager hebben db_ownership van de lokale database dat u geo-repliceren zult en DBManager op de partner (s) waarop u Geo-replicatie configureren.
- SQL Server Management Studio (SSMS)

> [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de meest recente versie van het Management Studio gebruiken om met updates voor Microsoft Azure en SQL-Database gesynchroniseerd blijven. [Bijwerken van SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="add-secondary-database"></a>Secundaire database toevoegen

U kunt de instructie **ALTER DATABASE** een secundaire geo gerepliceerd-database maakt op een partnerserver. U uitvoert deze instructie op de master-database van de server waarop de database wordt gerepliceerd. De geo-gerepliceerde database (de "primaire database") heeft dezelfde naam als de database wordt gerepliceerd en, standaard, hebben hetzelfde serviceniveau als de primaire database. De secundaire database kan kan worden gelezen of niet-leesbare, en één database of een elastische databbase. Voor meer informatie Zie [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) en [Service Tiers](sql-database-service-tiers.md).
Nadat u de secundaire database gemaakt en overgeënt begint gegevens asynchroon uit de primaire database te repliceren. De volgende stappen wordt beschreven hoe Geo-replicatie configureren met Management Studio. Stappen voor het maken van niet-leesbare en leesbaar secundaire servers, met een enkele database of een elastische database worden geleverd.

> [AZURE.NOTE] Als een database op de opgegeven server met dezelfde naam als de primaire database, de opdracht mislukt.


### <a name="add-non-readable-secondary-single-database"></a>Niet-leesbare secundaire (één database) toevoegen

Gebruik de volgende stappen voor het maken van een niet-leesbare secundair als één enkele database.

1. Met versie 13.0.600.65 of hoger van SQL Server Management Studio.

     > [AZURE.IMPORTANT] Download de [nieuwste](https://msdn.microsoft.com/library/mt238290.aspx) versie van SQL Server Management Studio. Het wordt aanbevolen dat u altijd de meest recente versie van het Management Studio synchroon met updates voor de Azure portal blijven gebruiken.


2. Open de map Databases, vouw de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik op **Nieuwe Query**.

3. Met de volgende instructie **ALTER DATABASE** kunt u een lokale database in een Geo-replicatie met een niet-leesbare secundaire database op MySecondaryServer1 waar MySecondaryServer1 de naam van uw aangepaste is primaire.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer1> WITH (ALLOW_CONNECTIONS = NO);

4. Klik op **uitvoeren** als de query wilt uitvoeren.


### <a name="add-readable-secondary-single-database"></a>Leesbare secundaire (één database) toevoegen
Gebruik de volgende stappen voor het maken van een secundair kan worden gelezen als een enkele database.

1. Verbinding maken met de logische server Azure SQL-Database in Management Studio.

2. Open de map Databases, vouw de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik op **Nieuwe Query**.

3. Met de volgende instructie **ALTER DATABASE** kunt u een lokale database in een Geo-replicatie primaire met een secundaire leesbare-database op een secundaire server.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);

4. Klik op **uitvoeren** als de query wilt uitvoeren.



### <a name="add-non-readable-secondary-elastic-database"></a>Niet-leesbare secundaire (elastische database) toevoegen

Gebruik de volgende stappen voor het maken van een niet-leesbare secundair als een elastische database.

1. Verbinding maken met de logische server Azure SQL-Database in Management Studio.

2. Open de map Databases, vouw de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik op **Nieuwe Query**.

3. Met de volgende instructie **ALTER DATABASE** kunt u een lokale database in een Geo-replicatie primaire met een secundaire niet-leesbare-database op een secundaire server in een elastische toepassingen.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer3> WITH (ALLOW_CONNECTIONS = NO
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool1));

4. Klik op **uitvoeren** als de query wilt uitvoeren.



### <a name="add-readable-secondary-elastic-database"></a>Leesbare secundaire (elastische database) toevoegen
Gebruik de volgende stappen voor het maken van een secundair leesbaar is als een elastische database.

1. Verbinding maken met de logische server Azure SQL-Database in Management Studio.

2. Open de map Databases, vouw de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik op **Nieuwe Query**.

3. Met de volgende instructie **ALTER DATABASE** kunt u een lokale database in een Geo-replicatie primaire met een secundaire leesbare-database op een secundaire server in een elastische toepassingen.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));

4. Klik op **uitvoeren** als de query wilt uitvoeren.



## <a name="remove-secondary-database"></a>Secundaire database verwijderen

De instructie **ALTER DATABASE** kunt u de replicatie partnerschap tussen een secundaire database en de bijbehorende primaire definitief te beëindigen. Deze instructie is uitgevoerd op de master-database waarin de primaire database zich bevindt. Na de beëindiging van de relatie wordt de tweede database een normale database voor lezen en schrijven. De opdracht is uitgevoerd als de secundaire database verbinding verbroken wordt, maar de secundaire worden alleen-lezen nadat de verbinding is hersteld. Voor meer informatie Zie [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) en [Service Tiers](sql-database-service-tiers.md).

Gebruik de volgende stappen uit secundaire geo gerepliceerd verwijderen uit een partnerschap Geo-replicatie.

1. Verbinding maken met de logische server Azure SQL-Database in Management Studio.

2. Open de map Databases, vouw de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik op **Nieuwe Query**.

3. Met de volgende instructie **ALTER DATABASE** te verwijderen van een secundaire geo gerepliceerd.

        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;

4. Klik op **uitvoeren** als de query wilt uitvoeren.

## <a name="monitor-geo-replication-configuration-and-health"></a>Configuratie van Geo-Replication Monitor en de gezondheid

Controle taken omvatten het toezicht op de configuratie van de Geo-replicatie en de controle van de gezondheid voor replicatie van gegevens.  Kunt u de weergave van de dynamische management **sys.dm_geo_replication_links** in de hoofddatabase om informatie te retourneren over alle bestaande replicatiekoppelingen voor elke database op de logische server Azure SQL-Database. Deze weergave bevat een rij voor elk van de replicatiekoppeling tussen primaire en secundaire databases. De **sys.dm_replication_link_status** beheer van dynamische weergave kunt u een rij voor elke Azure SQL-Database die op dat moment een replicatiekoppeling replicatie te retourneren. Dit omvat zowel primaire als secundaire databases. Als er meer dan één doorlopende replicatiekoppeling bestaat voor een bepaalde primaire database, bevat deze tabel een rij voor elk van de relaties. De weergave is in alle databases, met inbegrip van het logische model gemaakt. Bij het controleren van deze weergave in het logische model retourneert echter een lege set. De **sys.dm_operation_status** beheer van dynamische weergave kunt u de status voor alle databasebewerkingen, waaronder de status van de replicatiekoppelingen weergeven. Voor meer informatie, Zie [sys.geo_replication_links (Azure SQL-Database)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (Azure SQL Database)](https://msdn.microsoft.com/library/mt575504.aspx)en [sys.dm_operation_status (Azure SQL-Database)](https://msdn.microsoft.com/library/dn270022.aspx).

Gebruik de volgende stappen voor het controleren van een partnerschap Geo-replicatie.

1. Verbinding maken met de logische server Azure SQL-Database in Management Studio.

2. Open de map Databases, vouw de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik op **Nieuwe Query**.

3. Gebruik de volgende instructie worden alle databases met Geo replicatiekoppelingen weergegeven.

        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM [sys].geo_replication_links;

4. Klik op **uitvoeren** als de query wilt uitvoeren.
5. Open de map Databases, vouw de map **System Databases** , met de rechtermuisknop op **mijndb**en klikt u op **Nieuwe Query**.
6. De volgende instructie gebruiken zodat de laatste replicatie en replicatie lag tijd van mijn secundaire mijndb-databases.

        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status

7. Klik op **uitvoeren** als de query wilt uitvoeren.
8. De volgende instructie gebruiken zodat de meest recente geo-replicatiebewerkingen database mijndb gekoppeld.

        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC

9. Klik op **uitvoeren** als de query wilt uitvoeren.

## <a name="upgrade-a-non-readable-secondary-to-readable"></a>Een secundair niet-leesbare upgraden naar leesbare

In April 2017 het secundaire niet-leesbare type wordt ingetrokken en de bestaande niet-leesbare databases automatisch worden bijgewerkt naar secundaire servers voor kan worden gelezen. Als u vandaag niet-leesbare secundaire servers worden gebruikt en u wilt upgraden, zodat ze leesbaar zijn, kunt u de volgende eenvoudige stappen voor elke secundaire.

> [AZURE.IMPORTANT] Er is geen methode zelf ter plekke een upgrade van een niet-leesbare secundair aan kan worden gelezen. Als u alleen uw secundaire neerzet, blijft de primaire database onbeveiligde totdat de nieuwe secundaire is volledig gesynchroniseerd. Als de SERVICEOVEREENKOMST van uw toepassing vereist dat de primaire altijd beveiligd is, moet u overwegen een parallelle secundair maken in een andere server voordat u de upgrade hiervoor. Opmerking elke primaire kan maximaal 4 secundaire databases.


1. Eerst verbinding maken met de *secundaire* server en verwijder de niet-leesbare secundaire database:  
        
        DROP DATABASE <MyNonReadableSecondaryDB>;

2. Nu verbinding maken met de *primaire* server en een nieuwe leesbaar secundair

        ALTER DATABASE <MyDB>
            ADD SECONDARY ON SERVER <MySecondaryServer> WITH (ALLOW_CONNECTIONS = ALL);




## <a name="next-steps"></a>Volgende stappen

- Zie meer informatie over actieve Geo-replicatie, - [Actieve Geo-replicatie](sql-database-geo-replication-overview.md)
- Zie voor een overzicht van business continuity en scenario's [Business continuity-overzicht](sql-database-business-continuity.md)

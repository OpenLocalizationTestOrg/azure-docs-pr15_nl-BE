<properties 
    pageTitle="Maken of een Azure SQL-database verplaatsen naar een elastische toepassingen met T-SQL | Microsoft Azure" 
    description="T-SQL Azure SQL-database maken in een elastische toepassingen gebruiken. Of T-SQL in de datbase en toepassingen uit." 
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-transact-sql"></a>Bewaken en beheren van een elastische database toepassingen met Transact-SQL  

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Gebruik de opdrachten [Create Database (SQL-Database Azure)](https://msdn.microsoft.com/library/dn268335.aspx) en [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) te maken en de databases verplaatsen naar en van de elastische pools. De elastische toepassingen moet bestaan voordat u deze opdrachten kunt gebruiken. Deze opdrachten beïnvloeden alleen databases. Het maken van nieuwe groepen en de instelling van de eigenschappen van toepassingen (zoals eDTUs, min en max) kunnen niet worden gewijzigd met T-SQL-opdrachten.

## <a name="create-a-new-database-in-an-elastic-pool"></a>Een nieuwe database maken in een elastische toepassingen
Gebruik de opdracht CREATE DATABASE met de optie SERVICE_OBJECTIVE.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in a pool named S3M100.

Alle databases in een elastische toepassingen nemen over de servicelaag van de elastische pool (Basic, Standard en Premium). 


## <a name="move-a-database-between-elastic-pools"></a>Een database verplaatsen tussen groepen van elastisch
Gebruik de opdracht ALTER DATABASE met het wijzigen en instellen van de SERVICE\_OBJECTIEVE optie als ELASTISCHE\_POOL; Stel name in op de naam van de doel-toepassingen.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move the database named db1 to a pool named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Een database verplaatsen naar een elastische toepassingen 
Gebruik de opdracht ALTER DATABASE met het wijzigen en instellen van de SERVICE\_OBJECTIEVE optie als ELASTIC_POOL; Stel name in op de naam van de doel-toepassingen.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move the database named db1 to a pool named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Een database uit een elastisch toepassingen verplaatsen
Gebruik de opdracht ALTER DATABASE en de SERVICE_OBJECTIVE ingesteld op een van de prestaties (S0 S1, enz).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes the database into a stand-alone database with the service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Lijst van databases in een elastische toepassingen
Gebruik de [sys.database\_service \_doelstellingen weergave](https://msdn.microsoft.com/library/mt712619) om alle databases in een elastische toepassingen. Aanmelden bij de master database naar de queryweergave.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-a-pool"></a>Gegevens over brongebruik ophalen voor een groep

Gebruik de [sys.elastic\_groep \_resource \_weergave stats](https://msdn.microsoft.com/library/mt280062.aspx) bekijken van gebruiksstatistieken voor de bron van een elastische toepassingen op een logische server. Aanmelden bij de master database naar de queryweergave.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-an-elastic-database"></a>Resourcegebruik voor een elastische database ophalen

Gebruik de [sys.dm\_ db\_ resource\_weergave stats](https://msdn.microsoft.com/library/dn800981.aspx) of [sys.resource \_weergave stats](https://msdn.microsoft.com/library/dn269979.aspx) bekijken van gebruiksstatistieken voor de bron van een database in een elastische toepassingen. Dit proces is vergelijkbaar met het gebruik van bronnen voor een enkele database opvragen.

## <a name="next-steps"></a>Volgende stappen

U kunt na het maken van een elastische database toepassingen elastische databases in de groep beheren door elastische taken te maken. Elastische taken vergemakkelijken T-SQL-scripts uitgevoerd tegen een onbeperkt aantal databases in de groep. Zie [overzicht van elastische database](sql-database-elastic-jobs-overview.md)voor meer informatie. 

[Scaling out met Azure SQL-Database](sql-database-elastic-scale-introduction.md)Zie: elastische Databasehulpprogramma's gebruiken om te schalen, verplaatsen van gegevens, te zoeken of te maken.

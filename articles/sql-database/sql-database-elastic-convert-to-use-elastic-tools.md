<properties
   pageTitle="Migreren van bestaande databases te schalen | Microsoft Azure"
   description="Een laptopgeheugen databases een shard kaart manager maken gebruik van elastische Databasehulpprogramma's converteren"
   services="sql-database"
   documentationCenter=""
   authors="ddove"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/24/2016"
   ms.author="ddove"/>

# <a name="migrate-existing-databases-to-scale-out"></a>Migreren van bestaande databases te schalen

Eenvoudig beheren van uw bestaande schaalvergroting een laptopgeheugen databases die gebruikmaken van databasehulpmiddelen Azure SQL-Database (bijvoorbeeld [elastische Database client library](sql-database-elastic-database-client-library.md)). Een bestaande set van databases gebruiken de [shard manager toewijzen](sql-database-elastic-scale-shard-map-management.md), moet u eerst converteren. 

## <a name="overview"></a>Overzicht
Een bestaande database een laptopgeheugen migreren: 

1. Bereid de [shard kaart manager-database](sql-database-elastic-scale-shard-map-management.md).
2. Maak de map shard.
3. De afzonderlijke shards voorbereiden.  
2. Toewijzingen toevoegen aan de kaart shard.

Deze technieken kunnen worden geïmplementeerd met behulp van de [client-bibliotheek van .NET Framework](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)of de PowerShell-scripts vinden op [Azure SQL DB - hulpmiddelen-elastische databasescripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db). Hier de voorbeelden gebruiken de PowerShell-scripts.

Zie voor meer informatie over de ShardMapManager, [Shard beheer toewijzen](sql-database-elastic-scale-shard-map-management.md). Voor een overzicht van de elastische database tools overzicht [elastische Database functies](sql-database-elastic-scale-introduction.md).

## <a name="prepare-the-shard-map-manager-database"></a>De database manager shard voorbereiden

De map shard manager is een speciale database met de gegevens voor het beheren van databases schaalvergroting. U kunt een bestaande database gebruiken of een nieuwe database maken. Houd er rekening mee dat een database die fungeert als shard kaart manager niet dezelfde database als een shard moet worden. Vergeet niet dat het PowerShell-script niet de database voor u maken. 

## <a name="step-1-create-a-shard-map-manager"></a>Stap 1: Maak een shard kaart manager

    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>Voor het ophalen van de kaart shard manager

Nadat u hebt gemaakt, kunt u de manager shard kaart met deze cmdlet ophalen. Deze stap is nodig voor elke keer dat u nodig hebt om met het ShardMapManager-object.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 

  
## <a name="step-2-create-the-shard-map"></a>Stap 2: Maak de map shard

U moet het type shard kaart te maken. De keuze is afhankelijk van de database-architectuur: 

1. Één huurder per database (Zie de [verklarende woordenlijst](sql-database-elastic-scale-glossary.md)van termen,.) 
2. Meerdere huurders per database (twee typen):
    3. Lijst toewijzen
    4. Bereik toewijzing
 

Maak een **lijst toewijzing** shard kaart voor een model met één huurder. Het model met één huurder wijst één database per huurder. Dit is een effectieve model voor SaaS-ontwikkelaars vereenvoudigt het beheer.

![Lijst toewijzen][1]

Het model met meerdere huurder verschillende huurders toegewezen aan één database (en u kunt groepen van huurders verdelen over meerdere databases). Dit model gebruiken wanneer u verwacht de huurder dat aan de gegevensbehoeften van kleine hebben. In dit model wijzen wij tal van huurders met **bereik koppelen**aan een database. 
 

![Bereik toewijzing][2]

Of u een met meerdere huurder databasemodel meerdere huurders toewijzen aan een enkele database met behulp van een *lijst met toewijzingen* kunt implementeren. Bijvoorbeeld BW1 wordt gebruikt voor het opslaan van informatie over huurder-id 1 en 5 en DB2 opgeslagen gegevens voor huurder 7 en 10 huurder. 

![Huurders op één DB Muliple][3] 

**Op basis van uw keuze, kies een van de volgende opties:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Optie 1: een map shard voor het omzetten van een lijst maken
Maak een shard toewijzen met behulp van het ShardMapManager-object. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 
 
 
### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Optie 2: een map shard voor de toewijzing van een bereik maken

Houd er rekening mee dat voor het gebruik van deze toewijzing patroon, de huurder-id waarden moet worden doorlopend bereiken en is het aanvaardbaar een tussenruimte in de bereiken door simpelweg het bereik wordt overgeslagen bij het maken van de databases hebt.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Optie 3: Lijst met toewijzingen op één enkele database
Het instellen van dit patroon vereist ook een lijst met hyperlinks te maken zoals in stap 2, optie 1.

## <a name="step-3-prepare-individual-shards"></a>Stap 3: Afzonderlijke shards voorbereiden

Elke shard (database) toevoegen aan de map shard manager. Dit zorgt ervoor dat afzonderlijke databases voor het opslaan van informatie over lettertypetoewijzing. Deze methode wordt uitgevoerd op elke shard.
     
    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.
 

## <a name="step-4-add-mappings"></a>Stap 4: Toewijzingen toevoegen

De toevoeging van toewijzingen is afhankelijk van het soort shard plattegrond die u hebt gemaakt. Als u een lijst met hyperlinks hebt gemaakt, kunt u toewijzingen van de lijst toevoegen. Als u een bereik-kaart hebt gemaakt, kunt u toewijzingen bereik toevoegen.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Optie 1: de gegevens voor het omzetten van een lijst toewijzen

De gegevens toewijzen door de toewijzing van een lijst voor elke huurder toe te voegen.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>Optie 2: de gegevens voor een bereik toewijzing toewijzen

De toewijzingen bereik voor alle huurder bereik – database koppelingen toevoegen:

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>Stap 4-optie 3: koppelt u de gegevens voor meerdere huurders op een enkele database

Voor elke huurder, voert u de Add-ListMapping (optie 1, hierboven). 


## <a name="checking-the-mappings"></a>De toewijzingen controleren

Informatie over de bestaande shards en de bijbehorende toewijzingen kan worden opgezocht met behulp van volgende opdrachten:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Samenvatting

Als u de installatie hebt voltooid, kunt u beginnen met het gebruik van de elastische Database client library. U kunt ook de [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) en [meerdere shard query](sql-database-elastic-scale-multishard-querying.md)gebruiken.

## <a name="next-steps"></a>Volgende stappen


De PowerShell-scripts ophalen van [Extra-sripts DB elastische Azure SQL-Database](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

De hulpprogramma's zijn ook op GitHub: [Azure/elastische-db-hulpmiddelen](https://github.com/Azure/elastic-db-tools).

De gesplitste samenvoegen gebruiken om gegevens te verplaatsen of naar een model met meerdere huurder naar een model met één huurder. Zie [hulpprogramma samenvoegen voor splitsen](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Aanvullende bronnen

Zie voor informatie over algemene gegevens architectuur patronen van databasetoepassingen voor meerdere huurder software as a service (SaaS), [Ontwerppatronen voor SaaS-toepassingen met meerdere huurder met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Vragen en functie-aanvragen

Voor vragen neem bereiken ons op het [forum van de SQL-Database](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) en de functie-aanvragen, Voeg aan het [Feedbackforum voor SQL-Database](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png
 

<properties
    pageTitle="Prestatiemeteritems voor shard manager toewijzen"
    description="ShardMapManager klasse- en afhankelijke routering prestatiemeteritems"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="SilviaDoomra"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="SilviaDoomra"/>

# <a name="performance-counters-for-shard-map-manager"></a>Prestatiemeteritems voor shard manager toewijzen

Met name wanneer u [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md)kunt u de prestaties van een [shard kaart manager](sql-database-elastic-scale-shard-map-management.md)vastleggen. Items worden gemaakt met de methoden van de klasse Microsoft.Azure.SqlDatabase.ElasticScale.Client.  

Items worden gebruikt voor het bijhouden van de prestaties van de verrichtingen [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) . Deze items zijn toegankelijk in de Prestatiemeter, onder de categorie "Elastische Database: Shard Management".

**Voor de meest recente versie:** Ga naar [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Zie ook [een app voor het gebruik van de meest recente client-bibliotheek van elastische database bijwerken](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Vereisten

* Als de categorie prestaties en items maken, moet de gebruiker een deel van de lokale groep **Administrators** op de computer die als host fungeert voor de toepassing.  

* Om een exemplaar van het prestatiemeteritem maken en bijwerken van de items, moet de gebruiker een lid van de **groep Administrators** of de **Prestatiemetergebruikers** . 

## <a name="create-performance-category-and-counters"></a>De categorie prestaties en items maken 

Als u wilt de items maken, roept u de methode CreatePeformanceCategoryAndCounters van de [klasse ShardMapManagmentFactory](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx). Alleen een beheerder kan de methode worden uitgevoerd: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

Ook kunt u [deze](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) PowerShell-script met de methode execute. De methode maakt u de volgende prestatiemeteritems:  

* **Toewijzingen in cache**: aantal toewijzingen in de cache opgeslagen voor de kaart shard.
*  **DDR-bewerkingen per seconde**: snelheid van gegevens afhankelijke bewerkingen in het bewerkingsplan voor de kaart shard. Dit item wordt bijgewerkt wanneer een aanroep van [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) leidt tot een succesvolle verbinding met de shard bestemming. 
*  **Toewijzing lookup treffers per seconde**: aantal geslaagde cache opzoeken bewerkingen voor toewijzingen in de map shard. 
*  **Opzoeken/sec. toewijzing**: het aantal mislukte cache opzoeken bewerkingen voor toewijzingen in de map shard.
*  **Toewijzingen worden toegevoegd of bijgewerkt in cache per seconde**: snelheid instellen welke toewijzingen worden toegevoegd of bijgewerkt in cache voor de toewijzing van shard. 
*  **Toewijzingen die zijn verwijderd uit de cache per seconde**: snelheid waarmee de toewijzingen worden verwijderd uit de cache van de kaart shard. 

Prestatiemeteritems worden gemaakt voor elke map in de cache shard per proces.  


## <a name="notes"></a>Notities
Het maken van de prestatiemeteritems worden geactiveerd door de volgende gebeurtenissen:  

* Initialisatie van de [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) met [enthousiaste laden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), als de ShardMapManager alle plattegronden shard bevat. Deze omvatten de [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) en de methoden [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) .
* Geslaagde opzoeken van een shard-kaart (met [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) of [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 

* Geslaagde maken van shard toewijzen met behulp van CreateShardMap().

De prestatiemeteritems worden bijgewerkt door alle cache bewerkingen die worden uitgevoerd op de shard toewijzen en toewijzingen. Geslaagde verwijdering van de shard kaart met DeleteShardMap () reults het exemplaar prestatie-items worden verwijderd.  

## <a name="best-practices"></a>Aanbevolen procedures

* Het maken van de categorie prestaties en tellers moet slechts eenmaal worden uitgevoerd vóór het maken van een ShardMapManager-object. Elke uitvoering van de opdracht CreatePerformanceCategoryAndCounters() wist de vorige items (gegevensverlies gerapporteerd door alle exemplaren) en nieuwe regels worden gemaakt.  

* Performance counter exemplaren worden gemaakt per proces. De toepassing is vastgelopen of het verwijderen van een shard kaart uit de cache leidt tot verwijdering van de prestaties tellers exemplaren.  

### <a name="see-also"></a>Zie ook

[Elastische Database functies-overzicht](sql-database-elastic-scale-introduction.md)  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->


<properties 
    pageTitle="Een shard met elastische Databasehulpmiddelen toevoegen | Microsoft Azure" 
    description="Het gebruik van de elastische schaal API's nieuwe shards toevoegen aan een shard instellen." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="adding-a-shard-using-elastic-database-tools"></a>Een shard met elastische Databasehulpprogramma's toe te voegen

## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Een shard voor een nieuw bereik of nieuwe sleutel toevoegen  

Toepassingen moeten vaak toe te voegen nieuwe shards voor het verwerken van gegevens die wordt verwacht van nieuwe sleutels of sleutels bereiken, voor de toewijzing van een shard die al bestaat. Bijvoorbeeld, een toepassing die door de huurder-ID een laptopgeheugen moet mogelijk een nieuwe shard te creëren voor een nieuwe huurder of een laptopgeheugen maandelijkse gegevens moet mogelijk een nieuwe shard vóór het begin van elke nieuwe maand ingericht. 

Als het nieuwe bereik van waarden niet deel uitmaken van een bestaande toewijzing is, is het zeer eenvoudig om de nieuwe shard toevoegen en koppelen van de nieuwe sleutel of het bereik op dat shard. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Voorbeeld: een shard en het bereik toevoegen aan een bestaande shard kaart
In dit voorbeeld wordt de [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) de [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx) [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0})) -methoden en maakt een instantie van de klasse [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) . In het onderstaande voorbeeld een database met de naam **sample_shard_2** en de nodige schemaobjecten erin zijn gemaakt voor het bereik [300, 400).  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Als alternatief kunt u Powershell Manager Shard kaart maken. Een voorbeeld vindt u [hier](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).
## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Een shard voor een leeg gedeelte van een bestaand bereik toevoegen  

In sommige gevallen al een bereik toegewezen aan een shard en het gedeeltelijk worden gevuld met gegevens, maar u wilt nu aanstaande gegevens worden doorgestuurd naar een andere shard. Bijvoorbeeld: u shard per dagenbereik en 50 dagen al toegewezen aan een shard hebben, maar op een dag 24, toekomstige gegevens aan te voeren in een andere shard. De elastische database [splitsen samenvoegen tool](sql-database-elastic-scale-overview-split-and-merge.md) kan deze bewerking uitvoeren, maar als de verplaatsing van gegevens niet nodig is (bijvoorbeeld gegevens voor het bereik met dagen [25, 50), dat wil zeggen, 25 tot 50 exclusieve, inclusief dag nog niet bestaat) kunt u uitvoeren dit geheel rechtstreeks met de Shard kaart beheer-API's.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Voorbeeld: een bereik te splitsen en het lege gedeelte toewijzen aan een toegevoegde shard

Een database met de naam 'sample_shard_2' en alle nodige schemaobjecten erin zijn gemaakt.  

 
    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
    
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Belangrijk**: Gebruik deze techniek alleen als u zeker weet dat het bereik voor de bijgewerkte toewijzing leeg is.  De bovenstaande methoden controleren niet voor het bereik wordt verplaatst, is het verstandig om de controles in de code opnemen.  Als rijen in het bereik wordt verplaatst, wordt de werkelijke gegevensdistributie niet overeen met de bijgewerkte shard kaart. Het [hulpprogramma gesplitste samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md) gebruiken om de bewerking in plaats daarvan in deze gevallen.  


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 

<properties 
    pageTitle="Bij het controleren van meerdere shard | Microsoft Azure" 
    description="Query's uitvoeren in shards met behulp van de elastische database client library." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/12/2016" 
    ms.author="torsteng"/>

# <a name="multi-shard-querying"></a>Bij het controleren van meerdere shard

## <a name="overview"></a>Overzicht

Met de [elastische Database tools](sql-database-elastic-scale-introduction.md)kunt u een laptopgeheugen databaseoplossingen maken. **Bij het controleren van meerdere shard** wordt gebruikt voor taken als verzameling gegevens/rapportage die is vereist voor het uitvoeren van een query die zich uitstrekt over verschillende shards. (Contrast dit [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md), waarmee alle werkzaamheden op een enkele shard.) 

## <a name="overview"></a>Overzicht

1. Een [**RangeShardMap**](https://msdn.microsoft.com/library/azure/dn807318.aspx) of [**ListShardMap**](https://msdn.microsoft.com/library/azure/dn807370.aspx) met de [**TryGetRangeShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), de [**TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)of de methode [**GetShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) worden opgehaald. Zie [**een ShardMapManager maken**](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) en [**u een RangeShardMap of ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Maak een **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)** -object.
2. Een **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**maken. 
3. Stel de **[eigenschap CommandText](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)** op een T-SQL-opdracht.
3. De opdracht uitvoeren door de **[methode ExecuteReader](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**wordt aangeroepen.
4. Bekijk de resultaten met behulp van de **[klasse MultiShardDataReader](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**. 

## <a name="example"></a>Voorbeeld

De volgende code ziet u het gebruik van meerdere shard opvragen met behulp van een bepaalde **ShardMap** met de naam *myShardMap*. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                } 
           } 
    } 

 
Een belangrijk verschil is het maken van verbindingen met meerdere shard. Indien **SqlConnection** op één database werkt, neemt het **MultiShardConnection** een ***verzameling shards*** als invoer. De collectie van shards uit een toewijzing shard vullen. De query wordt vervolgens uitgevoerd in de collectie van shards met **UNION ALL** semantiek voor het samenstellen van een enkel het uiteindelijke resultaat. De naam van de shard waar de rij afkomstig is uit kan eventueel worden toegevoegd aan de uitvoer met behulp van de eigenschap **ExecutionOptions** op de opdracht. 

Opmerking het aanroepen van **myShardMap.GetShards()**. Deze methode haalt alle shards uit de toewijzing shard en biedt een eenvoudige manier een query uitvoeren op alle relevante databases. De collectie van shards voor een query met meerdere shard verfijnd worden kan verder door een LINQ query uitvoeren via de collectie geretourneerd door het aanroepen van **myShardMap.GetShards()**. In combinatie met het beleid van de gedeeltelijke resultaten, is de huidige capaciteit in het uitvoeren van een query met meerdere shard ontworpen om te werken voor tientallen tot honderden shards.

Een beperking voor het uitvoeren van een query met meerdere shard is momenteel het ontbreken van validatie voor shards en shardlets die worden doorzocht. Terwijl de gegevensafhankelijke routering wordt gecontroleerd of een bepaalde shard deel van de kaart shard ten tijde van het uitvoeren van query's, wordt deze controle niet uitvoert door query's met meerdere shard. Dit kan leiden tot meerdere shard-query uitvoeren op databases die zijn verwijderd uit de map shard.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Query's met meerdere shard en samenvoegen van gesplitste bewerkingen

Query's met meerdere shard controleren niet of shardlets op de querydatabase aan doorlopende samenvoegen gesplitste bewerkingen deelnemen. (Zie [schaal hulpprogramma voor het elastische Database splitsen samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md)). Dit kan leiden tot inconsistenties wanneer rijen uit de dezelfde shardlet voor meerdere databases in dezelfde query met meerdere shard weergeven. Zich bewust zijn van deze beperkingen en eventueel verwerkingsstop doorlopende samenvoegen gesplitste bewerkingen en wijzigingen in de toewijzing shard tijdens het uitvoeren van query's met meerdere shard.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Zie ook
**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)** klassen en methoden.


Shards gebruikt de [clientbibliotheek elastische Database](sql-database-elastic-database-client-library.md)beheren. Bevat de naamruimte [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) waarmee u de mogelijkheid om een query meerdere shards met een enkele query en het resultaat. Het biedt een abstractie opvragen op een verzameling shards. Het biedt ook alternatieve uitvoering van beleid, met name gedeeltelijke resultaten, omgaan met fouten bij het opvragen via veel shards.  

 
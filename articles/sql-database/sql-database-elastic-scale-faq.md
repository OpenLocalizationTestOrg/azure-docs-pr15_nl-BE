<properties 
    pageTitle="Azure SQL elastische schaal Veelgestelde vragen | Microsoft Azure" 
    description="Veelgestelde vragen over Azure SQL Database elastische schaal." 
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
    ms.date="05/03/2016" 
    ms.author="ddove"/>

# <a name="elastic-database-tools-faq"></a>Elastische Databasehulpmiddelen Veelgestelde vragen 

#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Als er een één-huurder per shard en geen sleutel sharding, hoe ik vullen de sleutel sharding voor de schema-info?

Het schemaobject info wordt alleen gebruikt voor het splitsen van scenario's samenvoegen. Als een toepassing per definitie één huurder is, hoeft niet de gesplitste samenvoegen tool en is dus niet nodig om het schema info-object te vullen.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Ik heb een database ingericht en hebt al een Map Shard Manager, hoe kan ik deze nieuwe database registreren als een shard?

Zie **[een shard naar een toepassing gebruikt de clientbibliotheek elastische database toe te voegen](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Hoeveel kost het elastische Databasehulpmiddelen?

Met behulp van de elastische database client-bibliotheek heeft geen kosten een. In hun kosten alleen voor de Azure SQL-databases die u gebruikt voor shards en Shard kaart Manager, evenals de web/werknemer rollen die u voor het samenvoegen van gesplitste gereedschap inrichten.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Waarom worden mijn referenties niet werkt wanneer ik een shard van een andere server toevoegen?
Gebruik geen referenties in de vorm van ' gebruiker ID=username@servername”, in plaats daarvan gebruikt u "gebruikers-ID = gebruikersnaam '.  Ook, zorg ervoor dat de aanmelding "gebruikersnaam" machtigingen op de shard.

#### <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Moet ik een Manager Shard toewijzing maken en vullen van shards telkens wanneer ik mijn toepassingen starten?

Nee, het maken van de Shard kaart Manager (bijvoorbeeld **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) is een eenmalige bewerking.  Uw toepassing moet de aanroep **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)** gebruiken tijdens het opstarten van toepassing.  Indien er slechts één aanroep per domein.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Ik heb vragen over het gebruik van de elastische databasehulpmiddelen, hoe krijg ik ze beantwoord? 

Neem bereiken ons op het [forum Azure SQL-Database](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Als ik een databaseverbinding met behulp van een sleutel sharding ontvangt, maar ik kan nog steeds querygegevens voor andere sharding toetsen op de shard dezelfde.  Dit is inherent aan het ontwerp?

De elastische schaal API's bieden u een verbinding met de juiste database voor uw sharding sleutel, maar geen sharding sleutel filteren.  Indien nodig **waar** clausules toevoegen aan uw query om het bereik te beperken tot de sleutel opgegeven sharding.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Kan ik een andere editie van Azure Database voor elke shard in mijn set shard gebruiken?

Ja, een shard is een afzonderlijke database en een shard kan dus een Premium edition terwijl andere een Standard edition. Verder is de editie van een shard omhoog of omlaag schaalbaar meerdere keren tijdens de levensduur van de shard.

#### <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Een database tijdens het splitsen of samenvoegen is het aanbieden van de tool splitsen, samenvoegen (of verwijderen)? 

Nr. Voor **gesplitste** bewerkingen de doeldatabase met het juiste schema moet bestaan en zijn geregistreerd bij de Shard kaart Manager.  Voor **samenvoegbewerkingen,** moet u de shard verwijderen uit de map shard manager en verwijder vervolgens de database.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
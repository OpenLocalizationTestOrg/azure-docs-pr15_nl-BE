<properties 
    pageTitle="Beheren van referenties in de bibliotheek elastische database client | Microsoft Azure" 
    description="Het vereiste niveau van de referenties van een beheerder voor apps elastische database alleen-lezen instellen" 
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

# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Referenties die worden gebruikt voor toegang tot de elastische Database client library

De [elastische Database client library](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) maakt gebruik van drie verschillende soorten referenties toegang te krijgen tot de [shard kaart manager](sql-database-elastic-scale-shard-map-management.md). Afhankelijk van de noodzaak, de referentie met het laagste niveau van toegang mogelijk te gebruiken.

* **Referenties**: voor het maken of bewerken van een shard kaart manager. (Zie de [woordenlijst](sql-database-elastic-scale-glossary.md)). 
* **-Referenties**: toegang tot een bestaande shard kaart manager voor informatie over shards.
* **Verbindingsreferenties**: verbinding maken met shards. 

Zie ook [databases beheren en Azure SQL-Database-aanmeldingen](sql-database-manage-logins.md). 
 
## <a name="about-management-credentials"></a>Over referenties

Referenties worden gebruikt voor het maken van een [**ShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) -object voor toepassingen die shard-toewijzingen bewerken. (Zie bijvoorbeeld [het toevoegen van een shard met elastische Databasehulpmiddelen](sql-database-elastic-scale-add-a-shard.md) en [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md)) De gebruiker van de client-bibliotheek van elastische schaal maakt de SQL-gebruikers- en SQL-aanmeldingen en zorgt ervoor dat elke lees-en schrijfmachtigingen voor de database voor wereldwijde shard en ook alle shard-databases wordt verleend. Deze referenties worden gebruikt voor het onderhouden van de globale shard toewijzen en de toewijzingen voor lokale shard wanneer wijzigingen in de map shard worden uitgevoerd. Gebruik bijvoorbeeld de referenties voor het maken van het object shard kaart manager (met behulp van [**GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx): 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

De variabele **smmAdminConnectionString** is een verbindingsreeks met de referenties. De gebruikersnaam en het wachtwoord geeft lezen/schrijven-toegang tot database voor shard en individuele shards. De beheer-verbindingsreeks bevat ook de naam van de server en de databasenaam voor de database voor globale shard. Dit is een typische verbindingsreeks voor dat doel:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Gebruik geen waarden in de vorm van "username@server"—instead de waarde "gebruikersnaam" gewoon gebruiken.  Dit komt omdat de referenties moeten werken met de shard kaart manager-database en de afzonderlijke shards die mogelijk op verschillende servers.

## <a name="access-credentials"></a>-Referenties
  
Gebruik bij het maken van een shard kaart manager in een toepassing die wordt niet shard toewijzingen beheren referenties met alleen-lezen machtigingen voor de globale shard kaart. De informatie wordt opgehaald uit het overzicht van de globale shard onder deze referenties worden gebruikt voor het [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) en voor het vullen van de toewijzingscache shard op de client. De referenties zijn beschikbaar via hetzelfde patroon aanroep naar **GetSqlShardMapManager** zoals hierboven: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Let op het gebruik van de **smmReadOnlyConnectionString** aangepast aan het gebruik van verschillende referenties voor deze toegang voor gebruikers die **geen beheerders** : deze referenties geen schrijfmachtigingen moeten bieden op de kaart voor globale shard. 

## <a name="connection-credentials"></a>Verbindingsreferenties 

Aanvullende referenties nodig zijn wanneer de methode [**OpenConnectionForKey**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) wordt gebruikt voor toegang tot een shard die is gekoppeld aan een sleutel sharding. Deze referenties moeten machtigingen voor alleen-lezen toegang tot de lokale shard kaart-tabellen die zich op de shard. Dit is nodig voor het uitvoeren van validatie van de verbinding voor het gegevensafhankelijke routering op de shard. Dit codefragment kunt toegang tot gegevens in de context van de gegevensafhankelijke routering: 
 
    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

**SmmUserConnectionString** bevat in dit voorbeeld wordt de verbindingsreeks voor de referenties van de gebruiker. Azure SQL DB is hier een typische verbindingsreeks voor de referenties van de gebruiker: 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

Net als bij de gebruikersnaam admin en doen niet de waarden in de vorm van "username@server". In plaats daarvan gebruikt u "username".  Vergeet niet dat de verbindingsreeks geen een servernaam en een databasenaam bevat. Dat komt omdat de verbinding met de juiste shard op basis van de sleutel wordt automatisch doorverwezen door het aanroepen van **OpenConnectionForKey** . De naam en de naam van de server worden dus niet geleverd. 

## <a name="see-also"></a>Zie ook
[Beheer van databases en Azure SQL-Database-aanmeldingen](sql-database-manage-logins.md)

[De SQL-Database beveiligen](sql-database-security.md)

[Aan de slag met elastische Database taken](sql-database-elastic-jobs-getting-started.md)

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
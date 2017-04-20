<properties 
    pageTitle="Voorbeelden van Azure Cache bestand Vgx. | Microsoft Azure" 
    description="Informatie over het gebruik van de Cache voor Azure bestand Vgx." 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-samples"></a>Voorbeelden van Azure Cache bestand Vgx. 

Dit onderwerp bevat een lijst met voorbeelden van Azure bestand Vgx. Cache, die betrekking hebben op de scenario's zoals verbinding maken met een cache, lezen en schrijven van gegevens naar en uit de cache en het gebruik van de ASP.NET-Cache bestand Vgx. providers. Sommige van de monsters zijn downloadbare projecten en sommige bieden stapsgewijze richtlijnen en codefragmenten bevatten maar niet koppelt aan een project kan worden gedownload.

## <a name="hello-world-samples"></a>Hello world-voorbeelden

De voorbeelden in dit gedeelte laten de basisbeginselen van verbindingen met een exemplaar van het bestand Vgx. Azure-Cache en lezen en schrijven van gegevens naar de cache met behulp van een verscheidenheid aan talen en bestand Vgx. clients.

Het [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) -voorbeeld ziet u hoe verschillende cache bewerkingen met de [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET client uit te voeren.

In dit voorbeeld ziet u hoe:

-   Gebruik verschillende verbindingsopties
-   Lezen en schrijven van objecten naar en van de cache met synchrone en asynchrone bewerkingen
-   MGET bestand Vgx. / MSET-opdrachten gebruiken om de waarden van de opgegeven toetsen
-   Uitvoeren van transactionele bewerkingen bestand Vgx.
-   Werken met lijsten bestand Vgx. en gesorteerde sets
-   .NET-objecten met behulp van JsonConvert objectserializers
-   Bestand Vgx. sets gebruiken voor het implementeren van labels
-   Werken met het Cluster bestand Vgx.

Zie de documentatie van de [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) op github en Zie de tests [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) unit voor meer gebruiksscenario's voor meer informatie.

[Azure bestand Vgx. Cache met Python gebruiken](cache-python-get-started.md) ziet u hoe aan de slag met Azure bestand Vgx. Cache met Python en het [bestand Vgx. py](https://github.com/andymccurdy/redis-py) -client.

[Werken met .NET-objecten in de cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) laat zien hoe serialiseren .NET-objecten, zodat u kunt ze kunnen schrijven en van een exemplaar van het bestand Vgx. Azure-Cache lezen. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>Bestand Vgx. Cache gebruiken als een geschaald uitbreiden-Backplane voor ASP.NET-SignalR

Het [Bestand Vgx. Cache gebruiken als een geschaald uitbreiden-Backplane voor ASP.NET-SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) -voorbeeld wordt gedemonstreerd hoe u Azure bestand Vgx. Cache kunt gebruiken als een SignalR-backplane. Zie voor meer informatie over backplane [Scaleout met het bestand Vgx. SignalR](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="redis-cache-customer-query-sample"></a>Voorbeeld van een klant query Cache bestand Vgx.

Dit voorbeeld demonstreert prestaties vergelijkt tussen toegang tot gegevens uit een cache en toegang krijgen tot gegevens uit de opslag voor persistentie. In dit voorbeeld heeft twee projecten.

-   [Demo hoe bestand Vgx. Cache prestaties kunt verbeteren door gegevens in cache opslaan](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
-   [Seed-Database en -Cache voor de demo](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>ASP.NET-sessiestatus en uitvoercaching

Het voorbeeld [Gebruiken Azure bestand Vgx. de Cache opslaan van ASP.NET SessionState en OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) wordt gedemonstreerd hoe u ASP.NET-sessie en de uitvoer-Cache met de providers SessionState en OutputCache voor bestand Vgx. opslaan met Azure bestand Vgx. Cache.

## <a name="manage-azure-redis-cache-with-maml"></a>Beheren met MAML-Cache bestand Vgx Azure.

De [Cache Azure bestand Vgx. beheren met behulp van Azure Management bibliotheken](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) ziet u hoe u kunt Azure Management bibliotheken beheren - (maken / Update / delete) uw Cache. 

## <a name="custom-monitoring-sample"></a>Aangepaste controle monster

De [toegang tot bestand Vgx. Cache controleren gegevens](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) ziet u hoe u toegang tot controlegegevens voor uw Azure Cache bestand Vgx. buiten de Portal Azure.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>Een Twitter-achtige kloon geschreven met behulp van PHP en bestand Vgx.

De [Retwis](https://github.com/SyntaxC4-MSFT/retwis) is een voorbeeld bestand Vgx. Hallo wereld. Het is een minimale Twitter-achtige sociale netwerk kloon geschreven met bestand Vgx. en PHP met de [Predis](https://github.com/nrk/predis) -client. De broncode die is ontworpen om zeer eenvoudig en tegelijkertijd verschillende weergeven bestand Vgx. gegevensstructuren.

## <a name="bandwidth-monitor"></a>Bandbreedte monitor

Het monster [bandbreedte monitor](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) kunt u de bandbreedte die wordt gebruikt op de client te controleren. Voor het meten van de bandbreedte, de steekproef worden uitgevoerd op de clientcomputer cache en aanroepen om de cache te observeren van de bandbreedte die wordt gemeld door het monster bandbreedte monitor.

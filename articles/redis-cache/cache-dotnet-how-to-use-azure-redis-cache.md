<properties 
    pageTitle="Het gebruik van Cache Azure bestand Vgx. | Microsoft Azure" 
    description="Meer informatie over het verbeteren van de prestaties van uw toepassingen Azure met Azure-Cache bestand Vgx." 
    services="redis-cache,app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="dotnet" 
    ms.topic="hero-article" 
    ms.date="08/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache"></a>Het gebruik van Cache bestand Vgx Azure.

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Deze handleiding wordt beschreven hoe u aan de slag met **Azure bestand Vgx. Cache**. Microsoft Azure bestand Vgx. Cache is gebaseerd op de populaire open-source bestand Vgx. Cache. Deze krijgt u toegang tot een beveiligde, speciale bestand Vgx. cache, beheerd door Microsoft. Een cache is gemaakt met behulp van Azure bestand Vgx. Cache is toegankelijk vanuit elke toepassing binnen Microsoft Azure.

Microsoft Azure bestand Vgx. Cache is beschikbaar in de volgende lagen:

-   **Basic** – één knooppunt. Het formaat van meerdere 53 GB.
-   **Standaard** : twee knooppunten primaire/Replica. Het formaat van meerdere 53 GB. 99,9 SLA %.
-   **Premium** : twee knooppunten primaire/Replica met maximaal 10 shards. Verschillende formaten van 6 GB 530 GB (contact met ons op voor meer informatie). Alle standaard reeks functies en meer inclusief ondersteuning voor het [bestand Vgx. cluster](cache-how-to-premium-clustering.md) [bestand Vgx. persistentie](cache-how-to-premium-persistence.md)en [Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9 SLA %.

Elke laag verschilt in termen van functies en prijzen. Zie voor informatie over prijzen, [Prijzen Details van Cache][].

Deze handleiding wordt u met de [StackExchange.Redis][] -client met behulp van C\# code. De scenario's die zijn **maken en configureren van een cache**, **cache-clients configureren**en **toevoegen en verwijderen van objecten uit de cache**. Raadpleeg de sectie [Volgende stappen][] voor meer informatie over het gebruik van Azure bestand Vgx. Cache. Zie voor een stapsgewijze zelfstudie voor het maken van een ASP.NET-MVC web app met Cache bestand Vgx. van [het maken van een Web App met Cache bestand Vgx.](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>
## <a name="get-started-with-azure-redis-cache"></a>Aan de slag met Azure bestand Vgx. Cache

Aan de slag met Azure bestand Vgx. Cache is eenvoudig. Om te beginnen richt en configureren van een cache. Vervolgens configureert u de clients cache zodat ze toegang de cache tot. Nadat de cache-clients zijn geconfigureerd, kunt u beginnen met hen.

-   [Maak de cache][]
-   [De cache-clients configureren][]

<a name="create-cache"></a>
## <a name="create-a-cache"></a>Een cache maken

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Voor toegang tot uw cache nadat deze gemaakt

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Zie voor meer informatie over het configureren van uw cache [Azure bestand Vgx. Cache configureren](cache-configure.md).

<a name="NuGet"></a>
## <a name="configure-the-cache-clients"></a>De cache-clients configureren

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Nadat uw project van de client is geconfigureerd voor het in cache plaatsen, kunt u de technieken die worden beschreven in de volgende secties voor het werken met de cache.

<a name="working-with-caches"></a>
## <a name="working-with-caches"></a>Werken met Caches

De stappen in deze sectie wordt beschreven hoe u veelvoorkomende taken met Cache uit te voeren.

-   [Verbinding maken met de cache][]
-   [Toevoegen en objecten uit de cache ophalen][]
-   [Werken met .NET-objecten in de cache](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>
## <a name="connect-to-the-cache"></a>Verbinding maken met de cache

Via programmering met cache werkt, moet u een verwijzing naar de cache. Voeg het volgende toe aan de bovenkant van elk bestand waarvan u wilt dat de client StackExchange.Redis gebruikt om een Cache Azure bestand Vgx..

    using StackExchange.Redis;

>[AZURE.NOTE] De client StackExchange.Redis vereist .NET Framework 4 of hoger.

De verbinding met de Azure bestand Vgx. Cache wordt beheerd door de `ConnectionMultiplexer` klasse. Deze klasse is ontworpen om te worden gedeeld en hergebruikt in de clienttoepassing en hoeft niet te worden gemaakt op basis van per bewerking. 

Verbinding maken met een Azure bestand Vgx. Cache en een exemplaar van een verbinding worden gegeven `ConnectionMultiplexer`, roept u de statische `Connect` methode en wachtwoord in de cache eindpunt en de sleutel, zoals in het volgende voorbeeld. Gebruik de sleutel die is gegenereerd op basis van de Portal Azure als wachtwoordparameter.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] Waarschuwing: Nooit archief referenties in broncode. Om dit voorbeeld eenvoudig te houden, ben ik ze in de broncode weergeven. Zie [hoe tekenreeksen van toepassingen en verbinding tekenreeksen werken][] voor meer informatie over het opslaan van referenties.

Als u niet wilt dat SSL te gebruiken, ofwel stelt u `ssl=false` of niet de `ssl` parameter.

>[AZURE.NOTE] De niet-SSL-poort is standaard uitgeschakeld voor nieuwe caches. Zie voor instructies over het inschakelen van de niet-SSL-poort, de [Toegang tot poorten](cache-configure.md#access-ports)...

Een benadering voor het delen van een `ConnectionMultiplexer` exemplaar in uw toepassing is dat een statische eigenschap verbonden exemplaar, zoals in het volgende voorbeeld geeft als resultaat. Thread-veilige manier kan slechts één verbinding initialiseren `ConnectionMultiplexer` exemplaar. In deze voorbeelden `abortConnect` is ingesteld op false, wat betekent dat de aanroep slaagt zelfs als een verbinding met de Azure bestand Vgx. Cache niet tot stand gebracht. Een belangrijke functie van `ConnectionMultiplexer` is dat de verbinding automatisch naar de cache eenmaal het netwerkprobleem terugbrengen wel of andere oorzaken opgelost zijn.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

Zie voor meer informatie over de configuratie van geavanceerde verbindingsopties [configuratiemodel StackExchange.Redis][].

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Zodra de verbinding tot stand is gebracht, een verwijzing naar de cache bestand Vgx. database terug door het aanroepen van de `ConnectionMultiplexer.GetDatabase` methode. Het object dat wordt geretourneerd uit de `GetDatabase` methode is een lichtgewicht Pass Through-object en hoeft niet te worden opgeslagen.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Als u weet hoe u verbinding maakt met een exemplaar van het bestand Vgx. Azure-Cache en een verwijzing naar de cachedatabase, laten we in het werken met de cache.

<a name="add-object"></a>
## <a name="add-and-retrieve-objects-from-the-cache"></a>Toevoegen en objecten uit de cache ophalen

Items kunnen worden opgeslagen in en opgehaald uit de cache met behulp van de `StringSet` en `StringGet` methoden.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Bestand Vgx. winkels de meeste gegevens als tekenreeksen bestand Vgx., maar deze tekenreeksen veel soorten gegevens bevatten kunnen, inclusief de geserialiseerde binaire gegevens, die kan worden gebruikt voor het opslaan van .NET-objecten in de cache.

Bij het aanroepen van `StringGet`, als het object bestaat, wordt het teruggestuurd, en zo niet, `null` wordt geretourneerd. In dit geval kunt u de waarde van de gewenste gegevensbron ophalen en opslaan in de cache voor later gebruik. Dit staat bekend als de grond cache-patroon.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

De vervaldatum van een item in de cache, gebruikt u de `TimeSpan` -parameter van `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-the-cache"></a>Werken met .NET-objecten in de cache

Azure Cache bestand Vgx. .NET-objecten zoals primitieve gegevenstypen kunt cache, maar voordat u een .NET-object in de cache kan worden opgeslagen moet worden geserialiseerd. Dit is de verantwoordelijkheid van de ontwikkelaar van toepassingen en biedt de flexibiliteit van de ontwikkelaar in de keuze van de serializer.

Een eenvoudige manier objecten serialiseren is met de `JsonConvert` serialisatie methoden in [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) en het serialiseren van en naar JSON. In het volgende voorbeeld ziet u een get en set met behulp van een `Employee` exemplaar van het object.


    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    
        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>
## <a name="next-steps"></a>Volgende stappen

U hebt de basisprincipes geleerd, klik op deze koppelingen voor meer informatie over het bestand Vgx. Azure-Cache.

-   De ASP.NET-providers voor Azure bestand Vgx. Cache uitchecken.
    -   [Azure bestand Vgx. Session State-Provider](cache-aspnet-session-state-provider.md)
    -   [Azure bestand Vgx. Cache ASP.NET uitvoer-Cache Provider](cache-aspnet-output-cache-provider.md)
-   [Diagnostische gegevens van cache inschakelen](cache-how-to-monitor.md#enable-cache-diagnostics) zodat kunt u de [monitor](cache-how-to-monitor.md) van de gezondheid van de cache. U kunt de statistieken bekijken in de Portal Azure en u kunt ook [downloaden en controleer](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) ze met behulp van de hulpprogramma's van uw keuze.
-   Bekijk de [documentatie cache StackExchange.Redis][].
    -   Azure bestand Vgx. Cache kan worden geopend vanuit veel clients bestand Vgx. en ontwikkeltalen. Zie [http://redis.io/clients][]voor meer informatie.
-   Azure bestand Vgx. Cache kan ook worden gebruikt met externe services en hulpprogramma's zoals Redsmin en bestand Vgx. Desktop Manager.
    -   Zie voor meer informatie over Redsmin, [een verbindingsreeks Azure bestand Vgx. ophalen en gebruiken met Redsmin][].
    -   Toegang tot en de gegevens in het bestand Vgx. Azure-Cache met een GUI met behulp van [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager)controleren.
-   Raadpleeg de documentatie bij het [bestand Vgx.][] en meer informatie over het [bestand Vgx. gegevenstypen][] en [de invoering van een vijftien minuten om gegevens van het type bestand Vgx.][].



<!-- INTRA-TOPIC LINKS -->
[Volgende stappen]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Maak de cache]: #create-cache
[Configure the cache]: #enable-caching
[De cache-clients configureren]: #NuGet
[Working with Caches]: #working-with-caches
[Verbinding maken met de cache]: #connect-to-cache
[Toevoegen en objecten uit de cache ophalen]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session

  
<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png






   
<!-- LINKS -->
[http://redis.IO/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[Een verbindingsreeks Azure bestand Vgx. ophalen en gebruiken met Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[Configuratiemodel StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Cache prijsdetails]: http://www.windowsazure.com/pricing/details/cache/
[Azure Portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis cache clientdocumentatie]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Bestand Vgx.]: http://redis.io/documentation
[Gegevens van het type bestand Vgx.]: http://redis.io/topics/data-types
[een vijftien minuten Inleiding tot gegevenstypen bestand Vgx.]: http://redis.io/topics/data-types-intro

[Werking van tekenreeksen toepassingen en verbindingsreeksen]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/



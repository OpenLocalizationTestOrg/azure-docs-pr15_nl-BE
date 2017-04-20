<properties 
    pageTitle="Cache migreren naar het bestand Vgx. | Microsoft Azure"
    description="Meer informatie over het migreren van toepassingen naar Azure bestand Vgx. Cache Cache-Service beheerd"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="09/30/2016"
    ms.author="sdanie" />

# <a name="migrate-from-managed-cache-service-to-azure-redis-cache"></a>Migreren van Service beheerde Cache naar Cache bestand Vgx Azure.

Migreren uw toepassingen die met Azure-Service beheerd Cache bestand Vgx. Azure-Cache kan worden uitgevoerd met minimale wijzigingen in uw toepassing, afhankelijk van de Cache-Service beheerd functies gebruikt door de toepassing in het cachegeheugen. Zij vergelijkbaar zijn en veel van de bestaande code die Cache-Service beheerd voor toegang tot een cache opnieuw kan worden gebruikt met minimale wijzigingen terwijl de API's niet identiek zijn. In dit onderwerp ziet u hoe u de benodigde configuratie en wijzigingen in de toepassing voor het migreren van uw beheerde Cache servicetoepassingen Azure bestand Vgx. Cache gebruiken en ziet u hoe sommige functies van Azure bestand Vgx. Cache kan worden gebruikt voor het implementeren van de functionaliteit van een cache Cache-Service beheerd.

## <a name="migration-steps"></a>Migratiestappen

De volgende stappen zijn vereist voor het migreren van een toepassing Cache-Service beheerd bestand Vgx. Azure-Cache te gebruiken.

-   Cache-Service beheerd functies toewijzen aan Azure-Cache bestand Vgx.
-   Kies een aanbieding Cache
-   Een Cache maken
-   De Cache-Clients configureren
    -   De serviceconfiguratie beheerde Cache verwijderen
    -   Een cache-client met behulp van het pakket StackExchange.Redis NuGet configureren
-   Cache-Service Managed code migreren
    -   Verbinding maken met de cache met behulp van de klasse ConnectionMultiplexer
    -   Primitieve gegevenstypen van Access in de cache
    -   Werken met .NET-objecten in de cache
-   Migratie van ASP.NET-sessiestatus en uitvoercaching Azure-cache bestand Vgx. 

## <a name="map-managed-cache-service-features-to-azure-redis-cache"></a>Cache-Service beheerd functies toewijzen aan Azure-Cache bestand Vgx.

Azure Service Cache beheerd en Azure bestand Vgx. Cache lijken, maar sommige van hun functies op verschillende manieren implementeren. In deze sectie wordt een overzicht gegeven van de verschillen en biedt u informatie over de implementatie van de functies van Cache-Service beheerd in Azure bestand Vgx. Cache.

|Beheerde Service Cache-functie|Beheerde ondersteuning voor Cache|Ondersteuning van Azure Cache bestand Vgx.|
|---|---|---|
|Benoemde caches|Een standaardcache is geconfigureerd en in de cache Standard en Premium aanbiedingen maximaal negen aanvullende benoemde caches kunnen worden geconfigureerd als gewenst.|Azure bestand Vgx. caches hebben een configureerbare aantal databases (standaard 16) die kunnen worden gebruikt voor het implementeren van een soortgelijke functionaliteit voor benoemde caches. Zie [standaard bestand Vgx. configuratie van de server](cache-configure.md#default-redis-server-configuration)voor meer informatie.|
|Hoge beschikbaarheid|Hoge beschikbaarheid biedt voor items in de cache in het cache-Standard en Premium aanbiedingen. Als items verloren gegaan als gevolg van een storing zijn, zijn back-up van de items in de cache nog steeds beschikbaar. Aan het secundaire cachegeheugen worden geschreven synchroon.|Hoge beschikbaarheid is beschikbaar in de Standard- en Premium-cache-aanbiedingen, waarvoor een primaire Replica /-configuratie van twee knooppunten (elke shard in een cache Premium heeft een paar primaire/replica). Schrijfbewerkingen naar de replica asynchroon uitgevoerd. Zie voor meer informatie, [prijzen Azure bestand Vgx. Cache](https://azure.microsoft.com/pricing/details/cache/).|
|Meldingen|Hiermee kunnen clients asynchrone meldingen wilt ontvangen wanneer u verschillende bewerkingen van de cache op een benoemde cache plaatsvinden.|Clienttoepassingen kunnen bestand Vgx. pub/sub- of [Keyspace meldingen](cache-configure.md#keyspace-notifications-advanced-settings) gebruiken voor een vergelijkbare functionaliteit voor meldingen.|
|Lokale cache|Lokaal een kopie van de objecten in de cache opgeslagen op de client voor een extra snelle toegang.|Client-toepassingen moet deze functionaliteit met behulp van een woordenboek of een vergelijkbare structuur implementeren.|
|Beleid voor verwijdering|Geen of Recentelijk. Het standaardbeleid is Recentelijk.|Azure Cache bestand Vgx. ondersteunt de volgende beleidsregels voor verwijdering: vluchtige recentelijk, allkeys recentelijk, vluchtige willekeurige, allkeys-willekeurige vluchtige-ttl, noeviction. Het standaardbeleid voor is vluchtige recentelijk. Zie [standaard bestand Vgx. configuratie van de server](cache-configure.md#default-redis-server-configuration)voor meer informatie.|
|Verloopbeleid|Het verloopbeleid standaard is absoluut en het verlopen van interval is standaard tien minuten. Schuif-en nooit zijn ook beschikbaar.|Standaard verlopen items in de cache niet, maar een verloopbeleid kan worden geconfigureerd op basis van per schrijven met cache set overbelastingen. Zie voor meer informatie [toevoegen en het ophalen van objecten uit de cache](cache-dotnet-how-to-use-azure-redis-cache.md#add-and-retrieve-objects-from-the-cache).|
|Regio's en labels|Gebieden zijn subgroepen van items in de cache. Regio's ondersteunen ook de aantekening van items in de cache met tekenreeksen met de aanvullende beschrijvende codes. Regio's ondersteunen de mogelijkheid om te zoekbewerkingen uitvoeren op gemarkeerde items in die regio. Alle items in een gebied zich in één knooppunt van het cluster cache bevinden.|Een bestand Vgx. cache bestaat uit een enkel knooppunt (tenzij het bestand Vgx. cluster is ingeschakeld) zodat het concept van Managed Cache Service regio's niet van toepassing. Bestand Vgx. ondersteunt zoeken en jokertekens bewerkingen bij het ophalen van sleutels, zodat er beschrijvende labels kunnen worden ingesloten in de namen van sleutels en gebruikt om de items later ophalen. Zie [cache wordt gelabeld met het bestand Vgx. implementeren](http://stackify.com/implementing-cache-tagging-redis/)voor een voorbeeld van de implementatie van een oplossing voor tagging met bestand Vgx..|
|Serialisatie|Beheerde Cache ondersteunt NetDataContractSerializer, BinaryFormatter en het gebruik van aangepaste objectserializers. De standaardwaarde is NetDataContractSerializer.|Het is de verantwoordelijkheid van de clienttoepassing serialiseren .NET-objecten voordat u plaatst ze in de cache, met de keuze van de serializer tot de client application developer. Zie [werken met .NET-objecten in de cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache)voor meer informatie en voorbeeldcode.|
| Cache-emulator | Beheerde Cache levert een lokale cache-emulator. | Azure bestand Vgx. Cache hoeft niet een emulator, maar u kunt [de build MSOpenTech van bestand Vgx.-server.exe lokaal uitvoeren](cache-faq.md#cache-emulator) om een emulator-ervaring. |

## <a name="choose-a-cache-offering"></a>Kies een aanbieding Cache

Microsoft Azure bestand Vgx. Cache is beschikbaar in de volgende lagen:

-   **Basic** – één knooppunt. Het formaat van meerdere 53 GB.
-   **Standaard** : twee knooppunten primaire/Replica. Het formaat van meerdere 53 GB. 99,9 SLA %.
-   **Premium** : twee knooppunten primaire/Replica met maximaal 10 shards. Verschillende formaten van 6 GB 530 GB (contact met ons op voor meer informatie). Alle standaard reeks functies en meer inclusief ondersteuning voor het [bestand Vgx. cluster](cache-how-to-premium-clustering.md) [bestand Vgx. persistentie](cache-how-to-premium-persistence.md)en [Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9 SLA %.

Elke laag verschilt in termen van functies en prijzen. De functies worden verderop in deze handleiding en voor meer informatie over prijzen vallen, Zie [Cache prijzen voor meer informatie](https://azure.microsoft.com/pricing/details/cache/).

Een startpunt voor migratie is het afdrukformaat dat overeenkomt met de grootte van uw vorige Managed Service voor Cache-cache en vervolgens omhoog of omlaag te schalen afhankelijk van de vereisten van uw toepassing. Zie voor meer informatie over het kiezen van het juiste Azure bestand Vgx. Cache aanbod [Cache bestand Vgx. aanbod en de grootte moet ik gebruiken?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Een Cache maken

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-the-cache-clients"></a>De Cache-Clients configureren

Wanneer de cache gemaakt en geconfigureerd, wordt de volgende stap is het verwijderen van de serviceconfiguratie Cache beheerd en het toevoegen van de configuratie bestand Vgx. Azure-Cache toevoegen en verwijst naar dat cache-clients toegang de cache tot hebben.

-   De serviceconfiguratie beheerde Cache verwijderen
-   Een cache-client met behulp van het pakket StackExchange.Redis NuGet configureren

### <a name="remove-the-managed-cache-service-configuration"></a>De serviceconfiguratie beheerde Cache verwijderen

Voordat u de client-toepassingen kunnen worden geconfigureerd voor Azure bestand Vgx. Cache, de bestaande configuratie van de Cache-Service beheerd en assembly-verwijzingen worden verwijderd door het verwijderen van het pakket NuGet voor Cache-Service beheerd.

Het pakket NuGet voor Cache-Service beheerd verwijderen, het client-project in de **Solution Explorer** met de rechtermuisknop en kies **NuGet pakketten beheren**. Selecteer het knooppunt **packages Installed** en W**indowsAzure.Caching** typt in het zoekvak van de geïnstalleerde pakketten. Selecteer **Windows** **Azure-Cache** (of **Windows** **Azure Caching** afhankelijk van de versie van het pakket NuGet), klik op **verwijderen**en klik vervolgens op **sluiten**.

![Azure beheerde Service NuGet pakket verwijderen](./media/cache-migrate-to-redis/IC757666.jpg)

De beheerde Cache Service NuGet package verwijderen verwijdert de Managed Service voor Cache assembly's en de posten Cache beheerd in de app.config of web.config van de clienttoepassing. Omdat sommige aangepaste instellingen kunnen niet worden verwijderd wanneer u het pakket NuGet verwijdert, opent u web.config of app.config en ervoor te zorgen dat de volgende elementen volledig worden verwijderd.

Ervoor te zorgen dat de `dataCacheClients` vermelding is verwijderd uit de `configSections` element. Verwijder niet de volledige `configSections` -element; alleen de `dataCacheClients` als deze aanwezig is.

    <configSections>
      <!-- Existing sections omitted for clarity. -->
      <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
    </configSections>

Ervoor te zorgen dat de `dataCacheClients` sectie is verwijderd. De `dataCacheClients` sectie is vergelijkbaar met het volgende voorbeeld.

    <dataCacheClients>
      <dataCacheClientname="default">
        <!--To use the in-role flavor of Azure Cache, set identifier to be the cache cluster role name -->
        <!--To use the Azure Managed Cache Service, set identifier to be the endpoint of the cache cluster -->
        <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

        <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
        <!--Use this section to specify security settings for connecting to your cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
        <!--<securityProperties mode="Message" sslEnabled="true">
          <messageSecurity authorizationInfo="[Authentication Key]" />
        </securityProperties>-->
      </dataCacheClient>
    </dataCacheClients>

Zodra de serviceconfiguratie beheerd Cache is verwijderd, kunt u de cache-client configureren zoals wordt beschreven in de volgende sectie.

### <a name="configure-a-cache-client-using-the-stackexchangeredis-nuget-package"></a>Een cache-client met behulp van het pakket StackExchange.Redis NuGet configureren

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Cache-Service Managed code migreren

De API voor de cache StackExchange.Redis client is vergelijkbaar met de Service Managed Cache. Deze sectie bevat een overzicht van de verschillen.

### <a name="connect-to-the-cache-using-the-connectionmultiplexer-class"></a>Verbinding maken met de cache met behulp van de klasse ConnectionMultiplexer

In Cache-Service beheerd verbindingen aan de cache zijn verwerkt door de `DataCacheFactory` en `DataCache` klassen. Deze verbindingen worden in Azure bestand Vgx. Cache beheerd door de `ConnectionMultiplexer` klasse.

Voeg de volgende instructie aan de bovenkant van elk bestand van waaruit u toegang wilt tot de cache.

    using StackExchange.Redis
                                
Als deze naamruimte niet wordt opgelost, moet u het pakket StackExchange.Redis NuGet toegevoegd zoals beschreven in [de cache-clients configureren](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

>[AZURE.NOTE] Houd er rekening mee dat de client StackExchange.Redis .NET Framework 4 of hoger vereist.

Bel voor de verbinding met een exemplaar van het bestand Vgx. Azure-Cache wordt de statische `ConnectionMultiplexer.Connect` methode en pass in het eindpunt en de sleutel. Een benadering voor het delen van een `ConnectionMultiplexer` exemplaar in uw toepassing is dat een statische eigenschap verbonden exemplaar, zoals in het volgende voorbeeld geeft als resultaat. Thread-veilige manier kan slechts één verbinding initialiseren `ConnectionMultiplexer` exemplaar. In dit voorbeeld `abortConnect` is ingesteld op false, wat betekent dat de aanroep slaagt zelfs als een verbinding met de cache niet is gevestigd. Een belangrijke functie van `ConnectionMultiplexer` is dat de verbinding automatisch naar de cache eenmaal het netwerkprobleem terugbrengen wel of andere oorzaken opgelost zijn.

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

Het eindpunt van de cache, sleutels en poorten kunnen worden opgehaald uit de **Cache bestand Vgx.** blade voor uw exemplaar van de cache. Zie [Eigenschappen voor Cache bestand Vgx.](cache-configure.md#properties)voor meer informatie.

Zodra de verbinding tot stand is gebracht, een verwijzing naar de cache bestand Vgx. database terug door het aanroepen van de `ConnectionMultiplexer.GetDatabase` methode. Het object dat wordt geretourneerd uit de `GetDatabase` methode is een lichtgewicht Pass Through-object en hoeft niet te worden opgeslagen.

    IDatabase cache = Connection.GetDatabase();
    
    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);
    
    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

De StackExchange.Redis-client gebruikt de `RedisKey` en `RedisValue` voor toegang tot en opslaan van items in de cache. Deze typen wijzen naar de meest primitieve taaltypen, met inbegrip van de tekenreeks, en vaak worden niet direct gebruikt. Bestand Vgx. tekenreeksen zijn de meest eenvoudige soort bestand Vgx. waarde en veel soorten gegevens, inclusief de geserialiseerde binaire streams kunnen bevatten en u mogelijk het type niet rechtstreeks gebruiken, gebruikt u de methoden met `String` in de naam. Voor meest primitieve gegevenstypen u opslaan en ophalen van objecten uit de cache met behulp van de `StringSet` en `StringGet` methoden, tenzij u collecties of andere gegevenstypen bestand Vgx. in de cache opslaat. 

`StringSet`en `StringGet` zijn vergelijkbaar met de Service Managed Cache `Put` en `Get` methoden, met een groot verschil dat voordat u een .NET-object in de cache ophalen en u het eerst serialiseren moet. 

Bij het aanroepen van `StringGet`, als het object bestaat, wordt deze geretourneerd en als dat niet het geval is, wordt null geretourneerd. In dit geval kunt u de waarde van de gewenste gegevensbron ophalen en opslaan in de cache voor later gebruik. Dit staat bekend als de grond cache-patroon.

De vervaldatum van een item in de cache, gebruikt u de `TimeSpan` -parameter van `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

Azure bestand Vgx. Cache met .NET-objecten zoals primitieve gegevenstypen kunt werken, maar voordat u een .NET-object in de cache kan worden opgeslagen moet worden geserialiseerd. Dit is de verantwoordelijkheid van de ontwikkelaar van toepassingen. Dit biedt de ontwikkelaar flexibiliteit bij de keuze van de serializer. Zie [werken met .NET-objecten in de cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache)voor meer informatie en voorbeeldcode.

## <a name="migrate-aspnet-session-state-and-output-caching-to-azure-redis-cache"></a>Migratie van ASP.NET-sessiestatus en uitvoercaching Azure-cache bestand Vgx.

Azure Cache bestand Vgx. heeft providers voor ASP.NET-sessiestatus en opslaan in de cache voor pagina-uitvoer. De migratie van de toepassing die gebruikmaakt van de Service voor beheerde Cache versies van deze providers, eerst de bestaande secties verwijderen uit uw web.config en configureer de Azure bestand Vgx. Cache-versies van de providers. Voor instructies over het gebruik van de providers Azure bestand Vgx. Cache ASP.NET Zie [ASP.NET Session State Provider voor Azure bestand Vgx. Cache](cache-aspnet-session-state-provider.md) en [ASP.NET uitvoer-Cache Provider voor Azure bestand Vgx. Cache](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Volgende stappen

Ontdek de [Azure-Cache bestand Vgx. documentatie](https://azure.microsoft.com/documentation/services/cache/) voor zelfstudies, monsters en video's.


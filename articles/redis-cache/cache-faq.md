<properties 
    pageTitle="Veelgestelde vragen-Cache Azure bestand Vgx. | Microsoft Azure" 
    description="Meer informatie over de antwoorden op veelgestelde vragen, patronen en aanbevolen procedures voor Azure-Cache bestand Vgx." 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="sdanie"/>

# <a name="azure-redis-cache-faq"></a>Veelgestelde vragen-Cache bestand Vgx Azure.

Informatie over de antwoorden op veelgestelde vragen, patronen en aanbevolen procedures voor Azure bestand Vgx. Cache. 


## <a name="what-if-my-question-isnt-answered-here"></a>Wat gebeurt er als mijn vraag hier niet wordt beantwoord?

Als uw vraag hier niet wordt weergegeven, laat het ons weten en wij helpen u een antwoord te vinden.

-   Als u een vraag post in de [thread Disqus](#comments) aan het einde van deze Veelgestelde vragen en oefenen met het team de Cache Azure en andere leden van de community over dit artikel.
-   Voor het bereiken van een breder publiek, kunt u een vraag op de [Azure Cache MSDN Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) boeken en oefenen met het team de Cache Azure en andere leden van de Gemeenschap.
-   Als u maken van een aanvraag voor een functie wilt, kunt u uw aanvragen en ideeën op [Azure bestand Vgx. Cache gebruiker stem](https://feedback.azure.com/forums/169382-cache)indienen.
-   U kunt ook een e-mail naar ons verzenden op de [Externe Cache-Feedback Azure](mailto:azurecache@microsoft.com).

## <a name="azure-redis-cache-basics"></a>Grondbeginselen van Azure Cache bestand Vgx.

Enkele van de basisprincipes van Azure bestand Vgx. Cache betrekking hebben op de veelgestelde vragen in deze sectie.

-    [Wat is Azure bestand Vgx. Cache?](#what-is-azure-redis-cache)
-    [Hoe kan ik aan de slag met Azure bestand Vgx. Cache?](#how-can-i-get-started-with-azure-redis-cache)

De volgende veelgestelde vragen betrekking op elementaire begrippen en vragen over Azure bestand Vgx. Cache en in een van de andere secties Veelgestelde vragen worden beantwoord.

-   [Cache bestand Vgx. aanbod en de grootte moet ik gebruiken?](#what-redis-cache-offering-and-size-should-i-use)
-   [Welke clients cache bestand Vgx. kan ik gebruiken?](#what-redis-cache-clients-can-i-use)
-   [Is er een lokale emulator voor Azure bestand Vgx. Cache?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Hoe controleer ik de gezondheid en de prestaties van mijn cache?](#how-do-i-monitor-the-health-and-performance-of-my-cache)



## <a name="planning-faqs"></a>Veelgestelde vragen over planning

-   [Cache bestand Vgx. aanbod en de grootte moet ik gebruiken?](#what-redis-cache-offering-and-size-should-i-use)
-   [Prestaties van Azure Cache bestand Vgx.](#azure-redis-cache-performance)
-   [In welk gebied moet ik mijn cache vinden?](#in-what-region-should-i-locate-my-cache)
-   [Hoe ben ik gefactureerd voor Azure-Cache bestand Vgx.](#how-am-i-billed-for-azure-redis-cache)
-   [Kan ik met Azure overheid Cloud of Azure China Cloud Azure bestand Vgx. Cache gebruiken?](#can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud)



## <a name="development-faqs"></a>Veelgestelde vragen over ontwikkeling

-   [Wat doet de configuratieopties voor StackExchange.Redis](#what-do-the-stackexchangeredis-configuration-options-do)
-   [Welke clients cache bestand Vgx. kan ik gebruiken?](#what-redis-cache-clients-can-i-use)
-   [Is er een lokale emulator voor Azure bestand Vgx. Cache?](#is-there-a-local-emulator-for-azure-redis-cache)
-   [Hoe kan ik bestand Vgx. opdrachten uitvoeren?](#how-can-i-run-redis-commands)
-   [Waarom geen Azure bestand Vgx. Cache heeft een verwijzing MSDN class library als sommige van de Azure services?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
-   [Kan ik als PHP sessiecache Azure bestand Vgx. Cache gebruiken?](#can-i-use-azure-redis-cache-as-a-php-session-cache)


## <a name="security-faqs"></a>Veelgestelde vragen over Security

-   [Wanneer moet ik de niet-SSL-poort voor de verbinding met het bestand Vgx. inschakelen?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)


## <a name="production-faqs"></a>Productie-Veelgestelde vragen

-   [Wat zijn enkele aanbevelingen productie?](#what-are-some-production-best-practices)
-   [Wat zijn de aandachtspunten bij het gebruik van veelgebruikte opdrachten bestand Vgx.?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
-   [Hoe kan ik benchmark en test de prestaties van mijn cache?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Belangrijke informatie over de groei van de ThreadPool](#important-details-about-threadpool-growth)
-   [GC meer doorvoer ophalen op de client bij het gebruik van StackExchange.Redis server inschakelen](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)


## <a name="monitoring-and-troubleshooting-faqs"></a>Monitoring en veelgestelde vragen over probleemoplossing

Gemeenschappelijke controle en probleemoplossing vragen betrekking hebben op de veelgestelde vragen in deze sectie. Zie voor meer informatie over het controleren en het oplossen van uw sessies Azure bestand Vgx. Cache [Azure bestand Vgx. Cache controleren](cache-how-to-monitor.md) en [problemen met Azure bestand Vgx. Cache](cache-how-to-troubleshoot.md).

-   [Hoe controleer ik de gezondheid en de prestaties van mijn cache?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
-   [Mijn cache diagnostische gegevens opslag Accountinstellingen gewijzigd, wat is er gebeurd?](#my-cache-diagnostics-storage-account-settings-changed-what-happened)
-   [Waarom is de diagnostische gegevens voor sommige nieuwe caches, maar andere niet ingeschakeld?](#why-is-diagnostics-enabled-for-some-new-caches-but-not-others)
-   [Waarom time-out?](#why-am-i-seeing-timeouts)
-   [Waarom is mijn client losgekoppeld van de cache?](#why-was-my-client-disconnected-from-the-cache)


## <a name="prior-cache-offering-faqs"></a>Veelgestelde vragen over voorafgaande Cache aanbod

-   [Welke Cache Azure-aanbod is geschikt voor mij?](#which-azure-cache-offering-is-right-for-me)


### <a name="what-is-azure-redis-cache"></a>Wat is Azure bestand Vgx. Cache?

Azure bestand Vgx. Cache is gebaseerd op de populaire open-source [bestand Vgx. cache](http://redis.io). Deze krijgt u toegang tot een beveiligde, speciale bestand Vgx. cache, beheerd door Microsoft en toegankelijk vanuit elke toepassing in Azure. Zie de productpagina [Azure bestand Vgx. Cache](https://azure.microsoft.com/services/cache/) op Azure.com voor een gedetailleerder overzicht.


### <a name="how-can-i-get-started-with-azure-redis-cache"></a>Hoe kan ik aan de slag met Azure bestand Vgx. Cache?

Er zijn verschillende manieren waarop die u kunt aan de slag met Azure bestand Vgx. Cache.

-    U kunt een van onze cursussen beschikbaar uitchecken voor [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Python](cache-python-get-started.md)en [Node.js](cache-nodejs-get-started.md). 
-    U kunt bekijken [hoe hoge prestaties Apps met behulp van Microsoft Azure bestand Vgx. Cache opbouwen](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/).
-    U kunt de clientdocumentatie voor de clients die overeenkomt met de taal van de ontwikkeling van het project om te zien hoe u het bestand Vgx. uitchecken. Er zijn vele klanten bestand Vgx. die kunnen worden gebruikt met Azure bestand Vgx. Cache. Zie voor een lijst van clients bestand Vgx., [http://redis.io/clients](http://redis.io/clients).


Als u niet al een Azure-account hebt, kunt u:

-    [Gratis een Azure account openen](/pricing/free-trial/?WT.mc_id=redis_cache_hero). U krijgt credits die kunnen worden gebruikt om te proberen betaald Azure Services. Zelfs nadat de kredieten zijn gebruikt, kunt u de rekening houden en gratis Azure services en functies gebruiken.
-    [Visual Studio activeren abonnee vergoedingen](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Uw MSDN-abonnement geeft u credits elke maand die u voor betaalde Azure services gebruiken kunt.


<a name="cache-size"></a>
### <a name="what-redis-cache-offering-and-size-should-i-use"></a>Cache bestand Vgx. aanbod en de grootte moet ik gebruiken?
Elk bestand Vgx. Cache van Azure biedt verschillende niveaus van **grootte**, **bandbreedte**, **beschikbaarheid**en **SLA** opties.

Hieronder vindt u aandachtspunten bij het kiezen van een Cache-aanbod.

-   **Geheugen**: The Basic en Standard lagen 250 MB – 53 GB bieden. De premie laag biedt tot 530 GB met meer beschikbaar [op verzoek](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase). Zie voor meer informatie, [Prijzen van Azure bestand Vgx. Cache](https://azure.microsoft.com/pricing/details/cache/).
-   **Prestaties van het netwerk**: als er een werkbelasting waarvoor hoge doorvoer de laag Premium biedt meer bandbreedte in vergelijking met standaard of Basic. Binnen elke laag hebben caches van grotere omvang ook meer bandbreedte door de onderliggende VM die fungeert als host voor de cache. Zie de [volgende tabel](#cache-performance) voor meer informatie.
-   **Doorvoer**: de premie laag biedt de maximumdoorvoer beschikbaar. Als de cacheserver of de client de limieten van de bandbreedte wordt bereikt, ontvangt u time-outs op de client. Zie de volgende tabel voor meer informatie.
-   **Hoge beschikbaarheid en/of SLA**: Azure bestand Vgx. Cache zorgt ervoor dat een standaard Premium/cache beschikbaar ten minste 99,9% van de tijd zijn zal. Zie meer informatie over onze SLA, [Azure bestand Vgx. Cache prijzen](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). De SLA alleen voor een verbinding met de eindpunten van de Cache. De SLA geldt niet voor bescherming tegen verlies van gegevens. Wij raden aan tolerantie tegen gegevensverlies verhoogd met de functie bestand Vgx. persistentie in de Premium-laag.
-   **Bestand Vgx. gegevens persistentie**: de premie laag kunt u de cache-gegevens in een opslag Azure-account. In een Basic-/ Standard-cache alle gegevens alleen in het geheugen opgeslagen. In geval van een onderliggende infrastructuur kunnen er problemen worden potentieel gegevensverlies. Wij raden aan tolerantie tegen gegevensverlies verhoogd met de functie bestand Vgx. persistentie in de Premium-laag. Azure Cache bestand Vgx. biedt een RDB en AOF (binnenkort) opties voor bestand Vgx. persistentie. Zie voor meer informatie [het configureren van persistentie van een Premium Azure bestand Vgx. Cache](cache-how-to-premium-persistence.md).
-   **Bestand Vgx. Cluster**: caches groter zijn dan 53 GB maken, of met gegevens op meerdere knooppunten bestand Vgx. shard kunt u bestand Vgx. clustering, die beschikbaar is in de Premium-laag. Elk knooppunt bestaat uit een combinatie van een primaire replica/cache voor een hoge beschikbaarheid. Zie voor meer informatie [het configureren van clusters voor een Premium Azure bestand Vgx. Cache](cache-how-to-premium-clustering.md).
-   **Verbeterde beveiliging en netwerkisolatie**: implementatie van Azure Virtual Network (VNET) biedt verbeterde beveiliging en isolatie voor uw Azure bestand Vgx. Cache en subnetten, opstellen, en andere functies die verdere toegang beperken. Zie [ondersteuning voor een Premium Azure bestand Vgx. Cache virtueel netwerk configureren](cache-how-to-premium-vnet.md)voor meer informatie.
-   **Configureer het bestand Vgx.**: In de Standard- en Premium-lagen, kunt u configureren bestand Vgx. Keyspace meldingen.
-   **Maximumaantal verbindingen van clients**: de premie laag biedt het maximum aantal clients die verbinding met het bestand Vgx., met een hoger aantal verbindingen voor een groter formaat caches maken kan. [Moet verwijzen naar de prijzen pagina voor meer informatie](https://azure.microsoft.com/pricing/details/cache/).
-   **Speciale Core voor Server bestand Vgx.**: In de Premium-laag hebben alle de cachegrootte een specifieke kern voor bestand Vgx.. In de Basic-/ Standard lagen de C1 de grootte en hoger hebben een speciale kern bestand Vgx. server.
-   **Single thread-bestand Vgx. is** dus met meer dan twee cores biedt geen extra voordeel via met twee cores, maar grotere VM hebben doorgaans meer bandbreedte dan kleinere. Als de cacheserver of de client de limieten van de bandbreedte wordt bereikt, ontvangt u time-outs op de client.
-   **Prestatieverbeteringen**: Caches in de premie laag op hardware met snellere processors worden geïmplementeerd en biedt betere prestaties in vergelijking met de Basic- of Standard-laag. Premium-tier Caches hebben hogere doorvoer en lagere vertragingstijden.

<a name="cache-performance"></a>
### <a name="azure-redis-cache-performance"></a>Prestaties van Azure Cache bestand Vgx.

De volgende tabel ziet u de maximale bandbreedte waarden tijdens het testen van verschillende grootten van Standard en Premium caches met `redis-benchmark.exe` in een VM Iaas ten opzichte van het eindpunt Azure bestand Vgx. Cache. Houd er rekening mee dat deze waarden zijn niet gegarandeerd en er is geen SLA voor deze getallen, maar normaal moet zijn. U moet laden uw eigen toepassing bepaalt de grootte van de juiste cache voor uw toepassing te testen.

We kunnen de volgende conclusies te trekken uit deze tabel.

-   Doorvoer voor de caches die even groot zijn, is hoger in de Premium-laag in vergelijking met de standaard laag. Doorvoer van P1 is bijvoorbeeld met een 6 GB Cache, 140K RPS in vergelijking met 49 K voor C3.
-   Met het bestand Vgx. clustering, neemt doorvoer lineair verhogen van het aantal shards (knooppunten) in het cluster. Als u een cluster P4 van 10 shards maakt, vervolgens de beschikbare doorvoer is bijvoorbeeld 250K * 10 = 2,5 miljoen RPS.
-   Doorvoer voor grotere sleutelgrootten is hoger in de Premium-laag in vergelijking met de standaard laag.

| Prijzen laag             | Grootte   | CPU-kernen | Beschikbare bandbreedte                                    | Sleutelgrootte 1 KB                            |
|--------------------------|--------|-----------|--------------------------------------------------------|------------------------------------------|
| **Standaard de cachegrootte** |        |           | **Megabits per seconde (Mb/s) / Megabytes per seconde (MB/s)** | **Aantal aanvragen per seconde (RPS)**            |
| C0                       | 250 MB | Gedeeld    | 5 / 0.625                                              | 600                                      |
| C1                       | 1 GB   | 1         | 100 / 12,5                                             | 12200                                    |
| C2                       | 2.5 GB | 2         | 200 / 25                                               | 24000                                    |
| C3                       | 6 GB   | 4         | 400 / 50                                               | 49000                                    |
| C4                       | 13 GB  | 2         | 500 / 62,5                                             | 61000                                    |
| C5                       | 26 GB  | 4         | 1000 / 125                                             | 115000                                   |
| C6                       | 53 GB  | 8         | 2000 / 250                                             | 150000                                   |
| **De cachegrootte Premium**  |        | **Processor-cores per shard**  |                                         | **Aantal aanvragen per seconde (RPS), per shard** |
| P1                       | 6 GB   | 2         | 1000 / 125                                             | 140000                                   |
| P2                       | 13 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P3                       | 26 GB  | 4         | 2000 / 250                                             | 220000                                   |
| P4                       | 53 GB  | 8         | 4000 / 500                                             | 250000                                   |


Voor instructies over het downloaden van het bestand Vgx. hulpmiddelen zoals `redis-benchmark.exe`, Zie de [hoe kan ik bestand Vgx. opdrachten uitvoeren?](#cache-commands) sectie.

<a name="cache-region"></a>
### <a name="in-what-region-should-i-locate-my-cache"></a>In welk gebied moet ik mijn cache vinden?

Voor de beste prestaties en de laagste wachttijden, zoek uw Cache Azure bestand Vgx. in hetzelfde gebied, als de clienttoepassing cache.

<a name="cache-billing"></a>
### <a name="how-am-i-billed-for-azure-redis-cache"></a>Hoe ben ik gefactureerd voor Azure-Cache bestand Vgx.

Azure Cache bestand Vgx. prijzen [hier](https://azure.microsoft.com/pricing/details/cache/). De prijzen pagina worden prijzen als een uurtarief. Caches worden gefactureerd op basis van per minuut vanaf het moment dat de cache wordt gemaakt totdat de cache is verwijderd. Er is geen optie voor het stoppen of onderbreken van de facturering van een cache.


## <a name="can-i-use-azure-redis-cache-with-azure-government-cloud-or-azure-china-cloud"></a>Kan ik met Azure overheid Cloud of Azure China Cloud Azure bestand Vgx. Cache gebruiken?

Ja, Azure bestand Vgx. Cache beschikbaar is in Azure overheid wolk en Azure China Cloud. Houd er rekening mee dat de URL's voor toegang tot en beheer van Azure bestand Vgx. Cache verschillen in Azure overheid wolk en Azure China Cloud met Azure openbare Cloud vergeleken. Zie voor meer informatie over overwegingen bij het gebruik van Azure bestand Vgx. Cache met Azure overheid wolk en Azure China Cloud, [Azure overheid Databases - Azure bestand Vgx. Cache](../azure-government/documentation-government-services-database.md#azure-redis-cache) en [Azure China Cloud - Azure bestand Vgx. Cache](https://www.azure.cn/documentation/services/redis-cache/).

Zie [verbinding maken met Azure overheid Cloud of Azure China Cloud](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud)voor meer informatie over het gebruik van Azure bestand Vgx. Cache met PowerShell in Azure overheid wolk en Azure China Cloud.


<a name="cache-configuration"></a>
### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>Wat doet de configuratieopties voor StackExchange.Redis

StackExchange.Redis heeft veel opties. In deze sectie gesprekken voert over een aantal algemene instellingen. Zie voor meer informatie over opties voor StackExchange.Redis, [StackExchange.Redis-configuratie](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md).

ConfigurationOptions|Beschrijving|Aanbeveling
---|---|---
AbortOnConnectFail|Wanneer wordt ingesteld op true, wordt de verbinding niet opnieuw na een netwerkstoring in een.|Ingesteld op false en laat StackExchange.Redis automatisch opnieuw verbinding te maken.
ConnectRetry|Het aantal keren te herhalen verbindingspogingen tijdens verbinding maken.| Zie de volgende opmerkingen voor instructies. |
ConnectTimeout|Time-out in ms voor bewerkingen verbinden.| Zie de volgende opmerkingen voor instructies. |

De standaardwaarden van de client zijn in de meeste gevallen voldoende. U kunt de opties op basis van uw werkbelasting verfijnen.

-   **Nieuwe pogingen**
    -   Voor ConnectRetry en ConnectTimeout is de algemene richtlijnen snel mislukken en probeer het opnieuw. Dit is gebaseerd op de hoeveelheid werk en hoeveel tijd op gemiddelde het duurt voor de client een opdracht bestand Vgx. en een antwoord ontvangen.
    -   Laat de StackExchange.Redis automatisch opnieuw verbinden in plaats van het controleren van de verbindingsstatus en het herstellen van uzelf. **Met de eigenschap ConnectionMultiplexer.IsConnected nooit**.
    -   Snowballing - soms kunt u uitvoeren in een probleem waarbij u opnieuw proberen zijn en dit snowballs en nooit hersteld. In dit geval moet u rekening houden met behulp van een algoritme voor exponentiële backoff opnieuw zoals beschreven in de [algemene richtsnoeren opnieuw](best-practices-retry-general.md) uitgegeven door de groep Microsoft Patterns & Practices.
-   **Time-outwaarden**
    -   Overweeg uw werkbelasting en stel de waarden dienovereenkomstig. Als u grote getallen opslaat, stelt u de time-out voor op een hogere waarde.
    -   Stel `AbortOnConnectFail` op false en laat de StackExchange.Redis opnieuw voor u.
    -   Gebruik slechts één exemplaar van de ConnectionMultiplexer voor de toepassing. U kunt een LazyConnection maken van één exemplaar dat wordt geretourneerd door de verbindingseigenschap van een, zoals [verbinding maken met de cache met behulp van de klasse ConnectionMultiplexer](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
    -   Stel de `ConnectionMultiplexer.ClientName` eigenschap aan een unieke instantienaam app voor diagnostische doeleinden.
    -   Gebruik meerdere `ConnectionMultiplexer` exemplaren voor aangepaste werkbelasting.
    -   Als er verschillende laden in uw toepassing, kunt u dit model volgen. Bijvoorbeeld:
    -   U kunt een multiplexer voor het omgaan met grote toetsen hebben.
    -   U kunt een multiplexer voor het omgaan met kleine toetsen hebben.
    -   U kunt verschillende waarden voor time-outs voor verbindingen instellen en logica opnieuw voor elke ConnectionMultiplexer die u gebruikt.
    -   Stel de `ClientName` eigenschap op elke multiplexer om te helpen bij de diagnose.
    -   Dit zal leiden tot een meer gestroomlijnd latentie per `ConnectionMultiplexer`.

### <a name="what-redis-cache-clients-can-i-use"></a>Welke clients cache bestand Vgx. kan ik gebruiken?

Een van de grote dingen over een bestand Vgx. is er zijn vele klanten veel ontwikkeling van verschillende talen ondersteunt. Zie [clients bestand Vgx.](http://redis.io/clients)voor een actuele lijst van clients. Zie [How to use Azure bestand Vgx. Cache](cache-dotnet-how-to-use-azure-redis-cache.md) voor zelfstudies over diverse verschillende talen en -clients, en klikt u op de gewenste taal in de schakelbaar taal boven aan het artikel.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>
### <a name="is-there-a-local-emulator-for-azure-redis-cache"></a>Is er een lokale emulator voor Azure bestand Vgx. Cache?

Er is geen lokale emulator voor Azure bestand Vgx. Cache, maar kunt u de MSOpenTech-versie van het bestand Vgx. server.exe uit het [bestand Vgx. opdrachtregelprogramma's](https://github.com/MSOpenTech/redis/releases/) uitvoeren op uw lokale computer en verbinding maken om een vergelijkbare ervaring aan een lokale cache-emulator, zoals in het volgende voorbeeld.


    private static Lazy<ConnectionMultiplexer>
        lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect to a locally running instance of Redis to simulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


Desgewenst kunt u een [redis.conf](http://redis.io/topics/config) bestand beter overeenkomen met de [standaardinstellingen van de cache](cache-configure.md#default-redis-server-configuration) voor de Cache on line Azure bestand Vgx. Indien gewenst.

<a name="cache-commands"></a>
### <a name="how-can-i-run-redis-commands"></a>Hoe kan ik bestand Vgx. opdrachten uitvoeren?

U kunt een van de opdrachten die aan het [bestand Vgx. opdrachten](http://redis.io/commands#) worden vermeld, met uitzondering van de opdrachten op [opdrachten die niet worden ondersteund in Azure bestand Vgx. Cache bestand Vgx.](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache). U hebt verschillende opties om het bestand Vgx. opdrachten uitvoeren.

-   Als er een cache Standard of Premium, kunt u het bestand Vgx.-opdrachten in het [Bestand Vgx. Console](cache-configure.md#redis-console)uitvoeren. Dit biedt een veilige manier uit te voeren opdrachten bestand Vgx. in de portal Azure.
-   U kunt ook het bestand Vgx. opdrachtregelprogramma's gebruiken. Als u wilt gebruiken, moet u de volgende stappen uitvoeren.
-   Download het [bestand Vgx. opdrachtregelprogramma's](https://github.com/MSOpenTech/redis/releases/).
-   Verbinding maken met behulp van de cache `redis-cli.exe`. In de cache eindpunt met behulp van die de -h schakelen en de sleutel gebruiken om-zoals in het volgende voorbeeld doorgeven.
-   `redis-cli -h <your cache="" name="">
.redis.cache.windows.net -a <key>
  `
  - Opmerking dat het bestand Vgx. opdrachtregelprogramma's niet met de SSL-poort werken, maar u een hulpprogramma zoals kunt `stunnel` veilig met de hulpprogramma's voor de SSL-poort door de aanwijzingen in het weblogbericht [Aankondigen ASP.NET sessie staat Provider voor bestand Vgx. Preview-versie](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) .

<a name="cache-reference"></a>
### <a name="why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Waarom geen Azure bestand Vgx. Cache heeft een verwijzing MSDN class library als sommige van de Azure services?

Microsoft Azure bestand Vgx. Cache is gebaseerd op de populaire open-source bestand Vgx. Cache en kan worden benaderd door een groot aantal [clients bestand Vgx.](http://redis.io/clients) die beschikbaar zijn voor veel programmeertalen. Elke client heeft een eigen API aanroepen van het bestand Vgx. cache-exemplaar met behulp van [opdrachten bestand Vgx.](http://redis.io/commands).

Omdat elke client verschillend is, is er niet één gecentraliseerde klassenverwijzing op MSDN; in plaats daarvan onderhoudt elke client zijn eigen documentatie. Naast de documentatie zijn er verschillende zelfstudies waarin wordt beschreven hoe aan de slag met Azure bestand Vgx. Cache met verschillende talen en cache-clients. Toegang tot deze zelfstudies, Zie [How to use Azure bestand Vgx. Cache](cache-dotnet-how-to-use-azure-redis-cache.md) en klik op de gewenste taal in de schakelbaar taal boven aan het artikel.


### <a name="can-i-use-azure-redis-cache-as-a-php-session-cache"></a>Kan ik als PHP sessiecache Azure bestand Vgx. Cache gebruiken?

Ja, geef de verbindingstekenreeks aan uw exemplaar van het bestand Vgx. Azure-Cache in Azure bestand Vgx. Cache als PHP sessiecache gebruik, `session.save_path`.

>[AZURE.IMPORTANT] Azure bestand Vgx. Cache als PHP sessiecache gebruikt, moet u URL coderen de beveiligingssleutel gebruikt voor verbinding met de cache, zoals in het volgende voorbeeld wordt getoond.
>
>`session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
>Als de sleutel niet als URL zijn gecodeerd, wordt een uitzondering van de volgende strekking weergegeven:`Failed to parse session.save_path`

Zie voor meer informatie over het gebruik van Cache bestand Vgx. Als cache PHP sessie met de client PhpRedis [PHP Session handler](https://github.com/phpredis/phpredis#php-session-handler).



<a name="cache-ssl"></a>
### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Wanneer moet ik de niet-SSL-poort voor de verbinding met het bestand Vgx. inschakelen?

Bestand Vgx. server biedt geen ondersteuning voor SSL uit de doos, maar Azure bestand Vgx. Cache werkt. Als u verbinding met Azure bestand Vgx. Cache maakt en de client SSL, zoals StackExchange.Redis ondersteunt, moet u SSL gebruiken.

Houd er rekening mee dat de niet-SSL-poort is standaard uitgeschakeld voor nieuwe exemplaren van het bestand Vgx. Azure-Cache. Als de client geen ondersteuning biedt voor SSL, moet u de niet-SSL-poort inschakelen door de aanwijzingen in de sectie [-poorten](cache-configure.md#access-ports) van het artikel [een cache in Azure bestand Vgx. Cache configureren](cache-configure.md) .

Als extra bestand Vgx. `redis-cli` werken niet met de SSL-poort, maar u kunt een hulpprogramma zoals `stunnel` veilig met de hulpprogramma's voor de SSL-poort door de aanwijzingen in het weblogbericht [Aankondigen ASP.NET sessie staat Provider voor bestand Vgx. Preview-versie](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) .

Zie voor meer informatie over het downloaden van het bestand Vgx. Extra de [hoe kan ik bestand Vgx. opdrachten uitvoeren?](#cache-commands) sectie.



### <a name="what-are-some-production-best-practices"></a>Wat zijn enkele aanbevelingen productie?

-   [Aanbevolen procedures voor StackExchange.Redis](#stackexchangeredis-best-practices)
-   [Configuratie en concepten](#configuration-and-concepts)
-   [Prestaties testen](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>Aanbevolen procedures voor StackExchange.Redis

-   Stel `AbortConnect` op false, laat de ConnectionMultiplexer automatisch opnieuw verbinding te maken. [Zie hier voor meer informatie](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
-   De ConnectionMultiplexer opnieuw gebruiken - Maak een nieuw voor elk verzoek geen. De `Lazy<ConnectionMultiplexer>` patroon [hier weergegeven](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) wordt ten zeerste aanbevolen.
-   Works best bestand Vgx. met kleinere waarden, moet deze hakken groter gegevens in meerdere sleutels. In [Dit bestand Vgx. discussie](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ)100 kb 'groot' beschouwd. Lees [dit artikel](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) voor een voorbeeld-probleem dat kan worden veroorzaakt door grote waarden.
-   Configureer de [ThreadPool instellingen](#important-details-about-threadpool-growth) om te voorkomen dat de time-outs.
-   Gebruik ten minste de standaard connectTimeout van 5 seconden. Dit zou StackExchange.Redis voldoende tijd om te herstellen in geval van een blip netwerk de verbinding op te geven.
-   Zich bewust zijn van de kosten van de prestaties van verschillende bewerkingen die u uitvoert. Bijvoorbeeld, de `KEYS` opdracht een bewerking O(n) is en moet worden vermeden. De [site redis.io](http://redis.io/commands/) heeft details rond de complexiteit van de tijd voor elke bewerking die hierdoor worden ondersteund. Klik op elke opdracht voor een overzicht van de complexiteit voor elke bewerking.

#### <a name="configuration-and-concepts"></a>Configuratie en concepten

-   Gebruik Standard of Premium laag voor productiesystemen. De basis laag is een systeem met één knooppunt met geen gegevensreplicatie en geen SLA. Gebruik ook ten minste een C1-cache. C0-caches zijn eigenlijk bedoeld voor eenvoudige dev/Testscenario's.
-   Vergeet niet dat bestand Vgx. een opslaggebied **In het geheugen** . Lees [dit artikel](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) zodat u zich bewust bent van scenario's waarin gegevens verloren gaan kunnen.
-   Uw systeem te ontwikkelen dat verbinding blips [patchen en failover](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md)kan verwerken.

#### <a name="performance-testing"></a>Prestaties testen

-   Starten met `redis-benchmark.exe` gecontroleerd op mogelijke doorvoer een idee krijgen voordat u schrijft uw eigen perf. Houd er rekening mee dat bestand Vgx. benchmark biedt geen ondersteuning voor SSL, zodat u zal hebben op [de niet-SSL-poort via de portal Azure inschakelen](cache-configure.md#access-ports) voordat u de test uitvoert. Zie voor voorbeelden van [hoe kan ik benchmark en test de prestaties van mijn cache?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   VM gebruikt voor het testen van de client moet in hetzelfde gebied, als uw cache bestand Vgx. exemplaar.
-   Beste Dv2 VM-reeks voor de client gebruiken omdat ze betere hardware en de beste resultaten krijgt.
-   Zorg ervoor dat uw klant u VM heeft ten minste zoveel computing en bandbreedte capaciteit als de cache die u wilt testen. 
-   VRSS inschakelen op de clientcomputer als u op Windows. [Zie hier voor meer informatie](https://technet.microsoft.com/library/dn383582.aspx).
-   Premium laag bestand Vgx. exemplaren zal hebben beter network latentie en doorvoer omdat ze op betere hardware voor CPU- en netwerk worden uitgevoerd.

<a name="cache-redis-commands"></a>
### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Wat zijn de aandachtspunten bij het gebruik van veelgebruikte opdrachten bestand Vgx.?

-   U moet bepaalde opdrachten in het bestand Vgx. die lang duren om te voltooien zonder de invloed van deze opdrachten niet uitvoeren.
    -   Bijvoorbeeld, voer geen de opdracht [sleutels](http://redis.io/commands/keys) in de productie als het lang duren kan om terug te keren is afhankelijk van het aantal toetsen. Bestand Vgx. een single thread-server en één opdrachten tegelijk worden verwerkt. Als er andere opdrachten afgegeven na sleutels, worden ze niet verwerkt totdat de opdracht sleutels bestand Vgx. verwerkt. De [site redis.io](http://redis.io/commands/) heeft details rond de complexiteit van de tijd voor elke bewerking die hierdoor worden ondersteund. Klik op elke opdracht voor een overzicht van de complexiteit voor elke bewerking.
-   Sleutelgrootten - moet ik gebruiken kleine sleutelwaarden of grote sleutelwaarden In het algemeen het is afhankelijk van het scenario. Als uw scenario grotere sleutels vereist vervolgens de ConnectionTimeout aanpassen en opnieuw de waarden en de logica opnieuw aanpassen. Vanuit een oogpunt van de server bestand Vgx. worden kleinere waarden waargenomen om prestaties te verbeteren.
-   Dit betekent niet dat u grotere waarden kunt opslaan in bestand Vgx.; u moet zich bewust zijn van de volgende overwegingen. Vertragingstijden zal hoger zijn. Als er één set gegevens die groter is en een die kleiner is, kunt u meerdere exemplaren van de ConnectionMultiplexer, elk met een andere set waarden voor time-outs en opnieuw hebt geconfigureerd, zoals beschreven in de vorige sectie voor [Wat de configuratieopties StackExchange.Redis doen](#cache-configuration) .



<a name="cache-benchmarking"></a>
### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Hoe kan ik benchmark en test de prestaties van mijn cache?

-   [Diagnostische gegevens van cache inschakelen](cache-how-to-monitor.md#enable-cache-diagnostics) zodat kunt u de [monitor](cache-how-to-monitor.md) van de gezondheid van de cache. U kunt de statistieken bekijken in de portal voor Azure en u kunt ook [downloaden en controleer](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) ze met behulp van de hulpprogramma's van uw keuze.
-   U kunt bestand Vgx. benchmark.exe laden test uw server bestand Vgx..
-   Zorg ervoor dat de belasting testen-client en de cache bestand Vgx. in hetzelfde gebied.
-   Bestand Vgx. cli.exe gebruiken en controleren van de cache met de opdracht INFO.
-   Als de belasting wordt veroorzaakt door geheugenfragmentatie van de hoge vervolgens u moet worden opgewaardeerd naar cache groter.
-   Zie voor meer informatie over het downloaden van het bestand Vgx. Extra de [hoe kan ik bestand Vgx. opdrachten uitvoeren?](#cache-commands) sectie.

Hier volgt een voorbeeld van het gebruik van bestand Vgx. benchmark.exe. Voor nauwkeurige resultaten, moet u deze opdracht uitvoert vanaf een VM in hetzelfde gebied, als de cache.

-   Test Pipelined SET-aanvragen met een 1 k-nettolading

    bestand Vgx. benchmark.exe - h- **yourcache**. redis.cache.windows.net - **yourAccesskey** -t 1024 - P 50 - n 1000000 -d instellen
    
-   Test Pipelined GET-aanvragen met een 1 k-nettolading. 
    Opmerking: Voer de SET testen hierboven afgebeelde eerst om in te vullen van de cache
    
    bestand Vgx. benchmark.exe - h- **yourcache**. redis.cache.windows.net - **yourAccesskey** -t-u -n 1000000 - d 1024 - P 50

<a name="threadpool"></a>
### <a name="important-details-about-threadpool-growth"></a>Belangrijke informatie over de groei van de ThreadPool

De CLR ThreadPool kent twee typen threads - "Werknemer" en "I/o-voltooiingspoort" (aka IOCP) threads. 

-   Worker-threads worden gebruikt wanneer voor zaken als de verwerking van `Task.Run(…)` of `ThreadPool.QueueUserWorkItem(…)` methoden. Deze threads worden ook gebruikt door diverse onderdelen in de CLR wanneer het werk moet gebeuren op een thread op de achtergrond.
-   IOCP threads worden gebruikt als asynchrone i/o gebeurt (bijv. lezen van het netwerk). 

De thread-groep biedt nieuwe worker-threads of i/o-poort-threads op aanvraag (zonder enige beperking) totdat de "Minimum" instelling voor elk type thread wordt bereikt. Het minimum aantal threads is standaard ingesteld op het aantal processors op een systeem. 

Zodra het aantal bestaande (bezet) de 'minimum' aantal threads in de ThreadPool wordt treffers threads injects gashendel de snelheid waarmee is nieuwe threads een bepaalde thread per 500 milliseconden. Dit betekent dat als uw systeem een package-burst die behoefte hebben aan een thread IOCP werk ontvangt, worden verwerkt die zeer snel. Echter als de omvang van het werk meer is dan de instelling "Minimum", zal er enige vertraging optreedt bij de verwerking van het werk zoals de ThreadPool wacht op een van twee dingen te gebeuren.

1. Een bestaande thread wordt het verwerken van het werk.
1. Geen bestaande thread wordt gratis voor 500ms, zodat een nieuwe thread is gemaakt.

In principe betekent dit dat wanneer het aantal threads bezet groter dan Min threads is, u waarschijnlijk een 500ms vertraging betaalt voordat het netwerkverkeer wordt verwerkt door de toepassing. Het is bovendien belangrijk te weten dat als een bestaande thread niet langer dan 15 seconden (gebaseerd op wat ik nog weet) actief blijft, zullen worden opgeschoond en deze cyclus van groei en inkrimping kunt herhalen.

Als we een voorbeeld van de foutmelding van de StackExchange.Redis (build 1.0.450 of hoger), ziet u het nu ThreadPool statistieken (Zie IOCP en werknemer details hieronder) afgedrukt.

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive, 
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0, 
    IOCP: (Busy=6,Free=994,Min=4,Max=1000), 
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

In het bovenstaande voorbeeld ziet u dat voor de thread IOCP 6 bezette threads zijn en het systeem is geconfigureerd voor 4 minimaal threads. In dit geval de client zou hebben waarschijnlijk gezien twee 500 ms vertragingen omdat 6 > 4.

Houd er rekening mee dat StackExchange.Redis time-outs kunnen worden bereikt als met deze eigenschap wordt de snelheid van de groei van de IOCP of werknemer threads.

### <a name="recommendation"></a>Aanbeveling

Deze informatie wordt gegeven, wordt aangeraden dat klanten de waarde van de minimale configuratie voor IOCP en werknemer threads ingesteld op iets groter is dan de standaardwaarde. Wij kunnen niet alle standaardoplossing voor alle richtlijnen op deze waarde moet omdat de juiste waarde voor een bepaalde toepassing te hoog/laag voor een andere toepassing. Deze instelling kan ook invloed op de prestaties van andere delen van complexe toepassingen, zodat elke klant moet deze instelling op hun specifieke behoeften aanpassen. Een goed beginpunt is 200 of 300, test en zo nodig aanpassen.

Het configureren van deze instelling:

-   In ASP.NET, gebruikt u de [configuratie-instelling 'minIoThreads'][] onder de `<processModel>` in web.config, configuratie-element. Als u in Azure WebSites, wordt deze instelling niet beschikbaar via de configuratieopties. Echter u moet nog wel programmatisch instellen (Zie hieronder) van uw methode Application_Start in global.asax.cs.

> **Belangrijke opmerking:** de waarde die is opgegeven in deze configuratie-element is een *core -* instelling. Bijvoorbeeld als een computer met 4 en u wilt dat uw instelling minIOThreads 200 tijdens runtime, gebruikt u `<processModel minIoThreads="50"/>`.

-   Gebruik de [ThreadPool.SetMinThreads(...)](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) buiten ASP.NET, API.

<a name="server-gc"></a>
### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>GC meer doorvoer ophalen op de client bij het gebruik van StackExchange.Redis server inschakelen

Server GC inschakelt, kan de client optimaliseren en bieden betere prestaties en de doorvoer bij het gebruik van StackExchange.Redis. Zie de volgende artikelen voor meer informatie over GC-server en het inschakelen van deze.

-   [GC server inschakelen](https://msdn.microsoft.com/library/ms229357.aspx)
-   [De basisbeginselen van de Garbage collector](https://msdn.microsoft.com/library/ee787088.aspx)
-   [De garbage collector en prestaties](https://msdn.microsoft.com/library/ee851764.aspx)







<a name="cache-monitor"></a>
### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Hoe controleer ik de gezondheid en de prestaties van mijn cache?

Microsoft Azure bestand Vgx. Cache exemplaren kunnen worden gecontroleerd in de [portal Azure](https://portal.azure.com). U kunt statistieken bekijken, vastmaken metrics grafieken aan de Startboard de datum- en tijdbereik van monitoring grafieken aanpassen, toevoegen en metrische gegevens verwijderen uit de grafieken en waarschuwingen instellen wanneer aan bepaalde voorwaarden wordt voldaan. Zie [Monitor Azure bestand Vgx. Cache](cache-how-to-monitor.md)voor meer informatie.

De sectie **Support + het oplossen** van de bladeserver bestand Vgx. Cache- **Instellingen** bevat ook diverse hulpmiddelen voor controle en probleemoplossing van de caches. 

-   **Problemen oplossen** biedt informatie over algemene problemen en strategieën voor het oplossen van deze.
-   **Controlelogboeken** bevat informatie over de acties die worden uitgevoerd op het cachegeheugen. U kunt ook filteren gebruiken om deze weergave wilt opnemen, andere bronnen weer te geven.
-   **Resource gezondheid** horloges van de resource en kunt u zien als deze wordt uitgevoerd zoals verwacht. Voor meer informatie over de Resource Azure health service overzicht [Azure Resource gezondheid](../resource-health/resource-health-overview.md).
-   **Nieuwe aanvraag ondersteuning** biedt opties voor het openen van een verzoek om ondersteuning voor de cache.

Deze hulpprogramma's kunnen u de status van uw sessies Azure bestand Vgx. Cache controleren en helpen bij het beheren van uw toepassingen in het cachegeheugen. Zie voor meer informatie, [ondersteuning en probleemoplossing van de instellingen](cache-configure.md#support-amp-troubleshooting-settings).

### <a name="my-cache-diagnostics-storage-account-settings-changed-what-happened"></a>Mijn cache diagnostische gegevens opslag Accountinstellingen gewijzigd, wat is er gebeurd?

Caches in dezelfde regio en abonnement delen dezelfde instellingen voor de diagnostische gegevens opslag en als de configuratie gewijzigd (diagnostische gegevens ingeschakeld is/uitgeschakeld of het wijzigen van de account van de opslag) van toepassing op alle caches die in die regio in het abonnement. Als de diagnostische gegevens van instellingen voor de cache hebt gewijzigd, controleert u of de diagnostische instellingen voor een andere cache in dezelfde regio en abonnement hebt gewijzigd. Een manier om te controleren is om weer te geven van de controlelogboeken voor de cache voor een `Write DiagnosticSettings` gebeurtenis. Zie voor meer informatie over het gebruik van controlelogboeken [weergeven gebeurtenissen en logboeken controleren](../monitoring-and-diagnostics/insights-debugging-with-events.md) en [bewerkingen met Resource Manager controleren](../resource-group-audit.md). Zie voor meer informatie over het controleren van gebeurtenissen Azure bestand Vgx. Cache [bewerkingen en waarschuwingen](cache-how-to-monitor.md#operations-and-alerts).

### <a name="why-is-diagnostics-enabled-for-some-new-caches-but-not-others"></a>Waarom is de diagnostische gegevens voor sommige nieuwe caches, maar andere niet ingeschakeld?

Caches in dezelfde regio en abonnement delen dezelfde instellingen voor de diagnostische gegevens opslag. Als u een nieuwe cache in dezelfde regio en abonnement als een andere cache dat diagnostische gegevens ingeschakeld is gemaakt, worden diagnostische gegevens is ingeschakeld op de nieuwe cache met dezelfde instellingen.


<a name="cache-timeouts"></a>
### <a name="why-am-i-seeing-timeouts"></a>Waarom time-out?

Time-outs gebeuren in de client die u gebruikt om het bestand Vgx. te spreken. Voor het grootste deel heeft bestand Vgx. server geen time-out. Wanneer een opdracht wordt verzonden naar de server bestand Vgx., de opdracht is in de wachtrij geplaatst en bestand Vgx. server uiteindelijk de opdracht wordt opgehaald en wordt deze uitgevoerd. Echter de client kunt time-outinstellingen tijdens dit proces, en als dit het geval is een uitzondering wordt gegenereerd aan de aanroepende. Zie voor meer informatie over het oplossen van problemen met time-out [problemen met Client side](cache-how-to-troubleshoot.md#client-side-troubleshooting) en [StackExchange.Redis-out uitzonderingen](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).


<a name="cache-disconnect"></a>
### <a name="why-was-my-client-disconnected-from-the-cache"></a>Waarom is mijn client losgekoppeld van de cache?

Hier volgen enkele algemene reden voor het verbreken van een cache.

-   Oorzaken van client-side
    -   De clienttoepassing is geïmplementeerd.
    -   De clienttoepassing een schaal bewerking uitgevoerd.
        -   Cloud-Services of Web Apps, kan dit worden veroorzaakt door automatisch schalen.
    -   De netwerklaag op de client is gewijzigd.
    -   Tijdelijke fouten zijn opgetreden in de client of de netwerkknooppunten tussen de client en de server.
    -   De bandbreedte grenswaarden werden bereikt.
    -   CPU-gebonden activiteiten duurde te lang om te voltooien.
-   Oorzaken van server-side
    -   De Azure bestand Vgx. Cache-service gestart op de standaard cache bieden, een storing van het primaire knooppunt naar het tweede knooppunt.
    -   Azure is de instantie waar de cache is geïmplementeerd patches
        -   Dit kan zijn voor het bestand Vgx. serverupdates of algemeen VM onderhoud.







### <a name="which-azure-cache-offering-is-right-for-me"></a>Welke Cache Azure-aanbod is geschikt voor mij?

>[AZURE.IMPORTANT]Aan de hand van vorig jaar [aankondiging](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/), wordt Azure-Service beheerd Cache en Azure In rol Cache service ingetrokken op 30 November 2016. Onze aanbeveling is [Azure bestand Vgx. Cache](https://azure.microsoft.com/services/cache/)gebruiken. Zie voor meer informatie over het migreren van [migreren van Azure bestand Vgx. Cache-Cache Service beheerd](cache-migrate-to-redis.md).

### <a name="azure-redis-cache"></a>Cache bestand Vgx Azure.
Azure bestand Vgx. Cache is algemeen beschikbaar zijn in het formaat van 53 GB en is de beschikbaarheid van SLA van 99,9%. De nieuwe [laag premium](cache-premium-tier-intro.md) biedt formaten tot 530 GB en ondersteuning voor clustering, VNET en persistentie, met een SLA 99,9%.

Azure bestand Vgx. Cache bieden klanten de mogelijkheid voor het gebruik van een beveiligde, speciale bestand Vgx. cache, beheerd door Microsoft. Met dit voorstel krijgt u gebruikmaken van de uitgebreide functies en het ecosysteem, geleverd door het bestand Vgx., en betrouwbare hosting en het controleren van Microsoft.

In tegenstelling tot traditionele caches die alleen met sleutel / waarde-paren, is bestand Vgx. populair voor haar zeer zodat gegevenstypen. Bestand Vgx. ook ondersteunt met atomaire bewerkingen op deze typen, zoals toe te voegen aan een tekenreeks; verhogen van de waarde in een hash; duwen naar een lijst. Computing set doorsnede, de Unie en verschil; of aan het lid met de hoogste positie in een gesorteerde set. Andere functies omvatten ondersteuning voor transacties, pub/sub, scripts, sleutels met een beperkte tijd-to-live, Lua en configuratie-instellingen te maken bestand Vgx. gedragen zich meer als een traditionele cache.

Een ander belangrijk aspect voor bestand Vgx. succes is het gebouwd rond het ecosysteem van gezonde, levendige bron openen. Dit wordt weerspiegeld in de grote verscheidenheid aan clients bestand Vgx. beschikbaar in meerdere talen. Dit kan worden gebruikt door vrijwel elke werklast die in Azure zou maken. 

Zie [hoe gebruik Azure bestand Vgx. Cache](cache-dotnet-how-to-use-azure-redis-cache.md) en [Azure bestand Vgx. Cache-documentatie](https://azure.microsoft.com/documentation/services/redis-cache/)voor meer informatie over aan de slag met Azure bestand Vgx. Cache.

### <a name="managed-cache-service"></a>Beheerde service van Cache
[Service is ingesteld op 30 November 2016 worden ingetrokken Cache beheerd.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

### <a name="in-role-cache"></a>Cache in rol
[Cache in de rol wordt ingesteld op 30 November 2016 worden ingetrokken.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)





['minIoThreads' configuratie-instelling]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx

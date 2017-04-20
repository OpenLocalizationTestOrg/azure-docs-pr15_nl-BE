<properties 
    pageTitle="Problemen met Azure bestand Vgx. Cache | Microsoft Azure" 
    description="Informatie over het oplossen van veelvoorkomende problemen met Azure bestand Vgx. Cache." 
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
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-troubleshoot-azure-redis-cache"></a>Problemen met Azure-Cache bestand Vgx.

Dit artikel bevat richtlijnen voor het oplossen van de volgende categorieën van Azure bestand Vgx. Cache problemen.

-   [Problemen met client side](#client-side-troubleshooting) - deze sectie bevat richtlijnen op het opsporen en oplossen van problemen die worden veroorzaakt door de toepassing die verbinding maken met Azure bestand Vgx. Cache.
-   [Problemen met server side](#server-side-troubleshooting) - deze sectie bevat richtlijnen identificeren en oplossen van problemen aan de serverzijde Azure bestand Vgx. Cache.
-   [StackExchange.Redis-out uitzonderingen](#stackexchangeredis-timeout-exceptions) - deze sectie bevat informatie over het oplossen van problemen bij het gebruik van de StackExchange.Redis-client.


>[AZURE.NOTE] Verschillende van de stappen in deze handleiding bevatten instructies om bestand Vgx. opdrachten uitvoeren en controleren van diverse prestatiestatistieken ophalen. Zie de artikelen in de sectie [aanvullende informatie](#additional-information) voor meer informatie en instructies.

## <a name="client-side-troubleshooting"></a>Problemen met client side


Deze sectie worden beschreven voor het oplossen van problemen die worden veroorzaakt door een voorwaarde op de clienttoepassing.

-   [Geheugendruk op de client](#memory-pressure-on-the-client)
-   [Burst-verkeer](#burst-of-traffic)
-   [Hoog CPU-gebruik-client](#high-client-cpu-usage)
-   [Client-Side bandbreedte overschreden](#client-side-bandwidth-exceeded)
-   [Aanvraag/antwoord van groot formaat](#large-requestresponse-size)
-   [Wat is er gebeurd met mijn gegevens in het bestand Vgx.?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-the-client"></a>Geheugendruk op de client

#### <a name="problem"></a>Probleem

Belasting op de clientcomputer leidt tot allerlei problemen met de prestaties die de verwerking van gegevens die is verzonden door het bestand Vgx. exemplaar zonder enige vertraging kunnen worden vertraagd. Wanneer geheugenbelasting raakt, heeft het systeem meestal op de paginagegevens uit het fysieke geheugen in het virtuele geheugen op schijf. Deze *pagina met fout* zorgt ervoor dat het systeem aanzienlijk vertragen.

#### <a name="measurement"></a>Meting 

1.  Geheugengebruik op de computer om ervoor te zorgen dat het beschikbare geheugen niet wordt overschreden. 
2.  Monitor de `Page Faults/Sec` Prestatiemeter. De meeste systemen zullen sommige pagina's hebben zelfs tijdens normale werking, dus Kijk voor pieken in dit prestatiemeteritem paginafouten die met de time-outs overeenkomen.

#### <a name="resolution"></a>Resolutie

Uw klant upgraden naar een groter formaat met meer geheugen VM-client of verdiepen in de gebruikspatronen geheugen geheugen consuption te verminderen.


### <a name="burst-of-traffic"></a>Burst-verkeer

#### <a name="problem"></a>Probleem

Bursts verkeer in combinatie met slecht `ThreadPool` instellingen kunnen leiden tot vertragingen in de verwerking van gegevens, maar nog niet zijn verbruikt op de client al is verzonden door de Server bestand Vgx..

#### <a name="measurement"></a>Meting 

Controleren hoe uw `ThreadPool` statistieken over de tijd met behulp van code [als volgt](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs)wijzigen. U kunt ook zoeken op de `TimeoutException` bericht van StackExchange.Redis. Hier volgt een voorbeeld:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

In het bovenstaande bericht zijn er diverse problemen die interessant zijn:

 1. Merk op dat in de `IOCP` sectie en de `WORKER` sectie u hebt een `Busy` -waarde die groter is dan de `Min` waarde. Dit betekent dat de `ThreadPool` -instellingen moeten aanpassen.
 2. U kunt ook zien `in: 64221`. Dit geeft aan dat 64211 bytes aan de kernel socket layer zijn ontvangen maar nog niet zijn gelezen door de toepassing (bijvoorbeeld StackExchange.Redis). Dit betekent meestal dat de toepassing wordt niet lezen van gegevens uit het netwerk snel de server is naar u te verzenden.

#### <a name="resolution"></a>Resolutie

Configureer de [ThreadPool instellingen](https://gist.github.com/JonCole/e65411214030f0d823cb) om ervoor te zorgen dat de threadgroep snel onder burst-scenario's vergroten zal.


### <a name="high-client-cpu-usage"></a>Hoog CPU-gebruik-client

#### <a name="problem"></a>Probleem

Hoog CPU-gebruik op de client is een indicatie dat het systeem niet kan bijhouden met het werk dat is gevraagd om uit te voeren. Dit betekent dat de client mogelijk niet een reactie van het bestand Vgx. tijdig worden verwerkt, hoewel het antwoord bestand Vgx. zeer snel verzonden.

#### <a name="measurement"></a>Meting

Het systeem breed CPU-gebruik via de Portal Azure of de bijbehorende prestatiemeteritems controleren. Wees voorzichtig niet te controleren CPU *proces* omdat één proces beschikt over CPU-gebruik laag tegelijk met het algehele systeem-CPU kan hoog zijn. Kijk voor pieken in het CPU-gebruik die met de time-outs overeenkomen. Als gevolg van het hoge CPU, u ziet misschien ook hoge `in: XXX` waarden in `TimeoutException` zoals beschreven in de sectie [Burst verkeer](#burst-of-traffic) foutberichten.

>[AZURE.NOTE] StackExchange.Redis 1.1.603 en later de `local-cpu` in metrieke `TimeoutException` foutberichten worden weergegeven. Zorg ervoor u de meest recente versie van het [pakket StackExchange.Redis NuGet](https://www.nuget.org/packages/StackExchange.Redis/). Er zijn fouten die voortdurend wordt verholpen in de code zodat deze meer robuuste voor time-outs zo belangrijk dat de meest recente versie is.

#### <a name="resolution"></a>Resolutie

Upgraden naar een groter VM met meer CPU-capaciteit of onderzoeken wat de oorzaak van Processorbelasting vertoont pieken. 



### <a name="client-side-bandwidth-exceeded"></a>Client-side bandbreedte overschreden

#### <a name="problem"></a>Probleem

Clientcomputers met verschillende afmetingen hebben beperkingen op hoeveel bandbreedte zij beschikbaar hebben. Als de client meer dan de beschikbare bandbreedte, worden gegevens niet verwerkt op de client zo snel de server is verzonden. Dit kan leiden tot time-outs.

#### <a name="measurement"></a>Meting

Controleren hoe uw bandbreedtegebruik na verloop van tijd met behulp van code [als volgt](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs)wijzigen. Houd er rekening mee dat deze code kan niet worden uitgevoerd in bepaalde omgevingen met beperkte machtigingen (zoals Azure websites).

#### <a name="resolution"></a>Resolutie 

Client VM vergroten of verkleinen van netwerkbandbreedte.


### <a name="large-requestresponse-size"></a>Aanvraag/antwoord van groot formaat

#### <a name="problem"></a>Probleem

Time-outs voor veroorzaken een grote aanvraag/reactie. Stel bijvoorbeeld dat de time-outwaarde op de client geconfigureerd is 1 seconde. De toepassing vraagt om twee sleutels (bijvoorbeeld "A" en "B") op hetzelfde moment (met dezelfde fysieke netwerkverbinding). Ondersteunen de meeste clients 'Pipelining"van de aanvragen, die beide aanvragen"A"en"B"worden verzonden over het netwerk naar de server een na de andere zonder te wachten op de antwoorden. De server stuurt de antwoorden in dezelfde volgorde. Als antwoord "A" groot kunt is het grootste deel van de time-out voor volgende aanvragen smullen. 

In het volgende voorbeeld ziet u in dit scenario. In dit scenario aanvraag "A" en "B" snel worden verzonden, start de server snel antwoorden op "A" en "B" verzenden, maar vanwege de overdrachtstijd van gegevens, "B" hangen achter de andere aanvragen en time-out, hoewel de server snel heeft gereageerd.

  	|-------- 1 Second Timeout (A)----------|
  	|-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Meting

Dit is een moeilijk te meten. In principe hebt u uw clientcode voor het bijhouden van grote aanvragen en antwoorden instrument. 

#### <a name="resolution"></a>Resolutie

1.  Bestand Vgx. is geoptimaliseerd voor een groot aantal kleine waarden in plaats van enkele grote waarden. De beste oplossing is om uw gegevens in gerelateerde kleinere waarden. Zie de [, wat is de ideale grootte van het waardebereik voor bestand Vgx.? 100KB te groot is?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) boeken voor meer informatie over waarom kleinere waarden worden aanbevolen.
2.  Vergroten van uw VM (voor client en Server van Cache bestand Vgx.) als u hogere bandbreedte mogelijkheden, waardoor de overdrachtstijd van gegevens voor grotere antwoorden. Houd er rekening mee dat krijgen meer bandbreedte op alleen de server of alleen op de client mogelijk niet voldoende. Meet uw bandbreedtegebruik en vergelijk deze met de mogelijkheden van de grootte van de VM die u momenteel hebt.
3.  Verhoog het aantal `ConnectionMultiplexer` objecten u round-robin aanvragen en gebruiken via verschillende verbindingen.


### <a name="what-happened-to-my-data-in-redis"></a>Wat is er gebeurd met mijn gegevens in het bestand Vgx.?

#### <a name="problem"></a>Probleem

Verwacht voor bepaalde gegevens in mijn exemplaar Azure bestand Vgx. Cache, maar het niet lijkt te bestaan.

##### <a name="resolution"></a>Resolutie

Zie [Wat is er gebeurd met mijn gegevens in het bestand Vgx.?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) voor mogelijke oorzaken en oplossingen.


## <a name="server-side-troubleshooting"></a>Problemen met server side

In deze sectie wordt beschreven voor het oplossen van problemen die worden veroorzaakt door een voorwaarde op de cacheserver.

-   [Geheugendruk op de server](#memory-pressure-on-the-server)
-   [Hoog CPU-gebruik / Server laden](#high-cpu-usage-server-load)
-   [Server-Side bandbreedte overschreden](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Geheugendruk op de server

#### <a name="problem"></a>Probleem

Geheugendruk op de server leidt tot allerlei problemen met de prestaties die de verwerking van aanvragen kunnen worden vertraagd. Wanneer geheugenbelasting raakt, heeft het systeem meestal op de paginagegevens uit het fysieke geheugen in het virtuele geheugen op schijf. Deze *pagina met fout* zorgt ervoor dat het systeem aanzienlijk vertragen. Er zijn verschillende mogelijke oorzaken van deze belasting: 

1.  U kunt cache van de volledige capaciteit zijn gevuld met gegevens. 
2.  Hoge geheugenfragmentatie - meestal als gevolg van grote objecten opslaan bestand Vgx. ziet (bestand Vgx. is geoptimaliseerd voor een kleine objecten - Zie de [, wat is de ideale grootte van het waardebereik voor bestand Vgx.? 100KB te groot is?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) boeken voor meer informatie). 

#### <a name="measurement"></a>Meting

Bestand Vgx. beschrijft twee parameters waarmee u dit probleem identificeren. De eerste is `used_memory` en de andere is `used_memory_rss`. [Deze gegevens](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) zijn beschikbaar in de Portal Azure of via de opdracht [Bestand Vgx. INFO](http://redis.io/commands/info) .

#### <a name="resolution"></a>Resolutie

Er zijn verschillende mogelijke wijzigingen die u maken kunt om het geheugengebruik gezond te houden:

1. [Een geheugen-beleid configureren](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) en verlooptermijn op uw sleutels in te stellen. Houd er rekening mee dat dit mogelijk niet voldoende als er fragmentatie.
2. [Een waarde voor maxmemory gereserveerd configureren](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) die groot genoeg is voor het compenseren van de geheugenfragmentatie.
3. De grote objecten in de cache in kleinere verwante objecten opheffen.
4. [Schaal](cache-how-to-scale.md) in een groter formaat van de cache.
5. Als u een [premium cache bestand Vgx. cluster ingeschakeld](cache-how-to-premium-clustering.md) kunt u [het aantal shards verhogen](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Hoog CPU-gebruik / Server laden

#### <a name="problem"></a>Probleem

Hoog CPU-gebruik betekent dat de client kan niet een reactie van het bestand Vgx. tijdig worden verwerkt, hoewel het antwoord bestand Vgx. zeer snel verzonden.

#### <a name="measurement"></a>Meting

Het systeem breed CPU-gebruik via de Portal Azure of de bijbehorende prestatiemeteritems controleren. Wees voorzichtig niet te controleren CPU *proces* omdat één proces beschikt over CPU-gebruik laag tegelijk met het algehele systeem-CPU kan hoog zijn. Kijk voor pieken in het CPU-gebruik die met de time-outs overeenkomen.

#### <a name="resolution"></a>Resolutie

[Schalen](cache-how-to-scale.md) naar een grotere cache trapsgewijs met meer CPU-capaciteit of onderzoeken wat de oorzaak van Processorbelasting vertoont pieken. 

### <a name="server-side-bandwidth-exceeded"></a>Server-Side bandbreedte overschreden

#### <a name="problem"></a>Probleem

Exemplaren van verschillende grootte cache hebben beperkingen op hoeveel bandbreedte zij beschikbaar hebben. Als de server groter is dan de beschikbare bandbreedte, wordt er geen gegevens naar de client zo snel worden verzonden. Dit kan leiden tot time-outs.

#### <a name="measurement"></a>Meting

U kunt controleren de `Cache Read` metric wordt de hoeveelheid gegevens lezen uit de cache in Megabytes per seconde (MB/s) tijdens het opgegeven interval voor rapportage. Deze waarde komt overeen met de bandbreedte die wordt gebruikt door deze cache. Als u wilt dat meldingen van server side netwerk bandbreedte limiet wilt instellen, kunt u ze met dit `Cache Read` teller. Vergelijk de waarden met de waarden in [deze tabel](cache-faq.md#cache-performance) voor de waargenomen bandbreedte limieten voor verschillende niveaus en afmetingen prijzen-cache.

#### <a name="resolution"></a>Resolutie

Als u voortdurend in de buurt van de waargenomen maximale bandbreedte voor de grootte van uw prijzen laag en de cache, kunt u [schalen](cache-how-to-scale.md) naar een prijzen laag of grootte met een grotere bandbreedte op het netwerk met behulp van de waarden in [deze tabel](cache-faq.md#cache-performance) als richtlijn.


## <a name="stackexchangeredis-timeout-exceptions"></a>StackExchange.Redis-out uitzonderingen

StackExchange.Redis maakt gebruik van een configuratie-instelling met de naam `synctimeout` voor synchrone bewerkingen die een standaardwaarde van 1000 ms heeft. Als een synchrone oproep niet binnen de vastgestelde tijd voltooien, genereert de client StackExchange.Redis een time-outfout zoals in het volgende voorbeeld.

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Dit foutbericht bevat parameters waarmee u de oorzaak en de mogelijke oplossing van het probleem te wijzen. De volgende tabel bevat details over de fout bericht maatstaven.

| Fout bericht metric | Details                                                                                                                                                                                                                                          |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inst       | In het laatste tijdsegment: 0-opdrachten zijn verleend.                                                                                                                                                                                              |
| Mgr        | Het uitvoeren van de socket-manager `socket.select` hetgeen betekent dat vraagt het besturingssysteem om aan te geven van een socket die iets heeft te doen; in principe: de lezer is niet actief lezen van het netwerk omdat deze niet denkt er niets dat te maken |
| wachtrij      | Er zijn 73 totale lopende activiteiten                                                                                                                                                                                                        |
| Qu         | 6 van de lopende werkzaamheden in de wachtrij staan niet verzonden en nog niet zijn geschreven met de uitgaande netwerk                                                                                                                                                           |
| Qs         | 67 van he lopende bewerkingen op de server zijn verzonden, maar een reactie is nog niet beschikbaar. Het antwoord kan worden `Not yet sent by the server` of`sent by the server but not yet processed by the client.`                                                   |
| QC         | 0 van de lopende werkzaamheden antwoorden hebben gezien, maar nog niet is gemarkeerd als voltooid door wacht op de voltooiing lus                                                                                                                                      |
| wR         | Er is een actieve schrijver (wat betekent dat de 6 niet-verzonden aanvragen worden niet genegeerd) bytes/activewriters                                                                                                                                                   |
| in         | Er zijn geen actieve lezers en nul bytes kunnen worden gelezen op de NIC bytes/activereaders                                                                                                                                               |


### <a name="steps-to-investigate"></a>Stappen om te onderzoeken

1. Als beste Zorg ervoor dat u het volgende patroon gebruikt bij het gebruik van de StackExchange.Redis-client verbinding te maken.


        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


    Zie [verbinding maken met de cache met StackExchange.Redis](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache)voor meer informatie.

2. Zorg ervoor dat uw bestand Vgx. Azure-Cache en de clienttoepassing in hetzelfde gebied in Azure. Bijvoorbeeld, u kan worden ophalen time-outs wanneer de cache is in Oost-VS, maar de client in westelijk Verenigde Staten en de aanvraag niet voltooien binnen de `synctimeout` interval of u kan worden ophalen time-outs wanneer u foutopsporing van de ontwikkeling van lokale computer. 

    Het is raadzaam om de cache en in de client in hetzelfde gebied, Azure. Als u een scenario dat gesprekken tussen regio bevat, stelt u de `synctimeout` interval een waarde die hoger is dan het standaardinterval voor 1000 ms door een `synctimeout` eigenschap in de verbindingsreeks. In het volgende voorbeeld ziet u een fragment StackExchange.Redis cache connection string met een `synctimeout` van ms 2000.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...

3. Zorg ervoor u de meest recente versie van het [pakket StackExchange.Redis NuGet](https://www.nuget.org/packages/StackExchange.Redis/). Er zijn fouten die voortdurend wordt verholpen in de code zodat deze meer robuuste voor time-outs zo belangrijk dat de meest recente versie is.

4. Als er aanvragen voor het ophalen van aan bandbreedtebeperkingen op de server of de client gebonden, duurt het langer om te voltooien en zo leiden tot timeouts. Als de time-out vanwege de bandbreedte van het netwerk op de server is, raadpleegt u [Server-side bandbreedte overschreden](#server-side-bandwidth-exceeded). Als de time-out vanwege de bandbreedte van het netwerk is, Zie [Client side bandbreedte overschreden](#client-side-bandwidth-exceeded).

6. U ophalen van CPU of zijn gekoppeld op de server op de client?
    -   Controleren als u ophalen van door de CPU op de client waardoor de aanvraag niet verwerkt gebonden zijn in de `synctimeout` interval, hetgeen een time-out. Verplaatsen naar een groter formaat van de client of de distributie van de belasting kunt u dit instellen. 
    -   Controleer of uw CPU gebonden op de server controleren de `CPU` [cache prestaties metric](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Aanvragen dat bestand Vgx. is afhankelijk van CPU kunnen deze aanvragen met time-out veroorzaken. Dit kunt u de werklast verdelen over meerdere shards in een cache premium of upgraden naar een groter formaat of de prijzen laag. Zie voor meer informatie, [Server Side bandbreedte overschreden](#server-side-bandwidth-exceeded).

7. Zijn er opdrachten die lange tijd op de server worden verwerkt? Langdurige opdrachten die lange tijd op het bestand Vgx. server worden verwerkt kan leiden tot time-outs. Enkele voorbeelden van langdurige opdrachten zijn `mget` met grote aantallen sleutels, `keys *` of slecht geschreven scripts lua. U kunt verbinding maken met uw Azure bestand Vgx. Cache-exemplaar met behulp van het bestand Vgx. cli-client of de [Console bestand Vgx.](cache-configure.md#redis-console) en voert u de opdracht [SlowLog](http://redis.io/commands/slowlog) om te zien of er aanvragen duurt langer dan verwacht. Bestand Vgx. Server en StackExchange.Redis zijn geoptimaliseerd voor veel kleine aanvragen in plaats van minder omvangrijke verzoeken. De gegevens opsplitsen in kleinere stukken kan hier dingen verbeteren. 

    Zie voor meer informatie over het maken van verbinding met het bestand Vgx. cli en stunnel met Azure bestand Vgx. Cache SSL-eindpunt, het [Aankondigen van ASP.NET sessie staat Provider voor Preview-versie bestand Vgx.](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blog boeken. Zie [SlowLog](http://redis.io/commands/slowlog)voor meer informatie.

8. Hoge bestand Vgx. belasting van de server kan leiden tot time-outs. U kunt de belasting van de server controleren door te controleren de `Redis Server Load` [cache prestaties metric](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Een belasting van de server van 100 (maximale waarde) betekent dat het bestand Vgx. server is bezet, geen niet-actieve tijd aanvragen verwerkt. Als u wilt zien als bepaalde verzoeken van alle van de capaciteit van de server nemen, voert u de opdracht SlowLog zoals beschreven in de vorige alinea. Zie voor meer informatie [hoog CPU-gebruik / Server laden](#high-cpu-usage-server-load).

9. Is er een andere gebeurtenis op de client die een blip netwerk kan hebben veroorzaakt? Controleren op de client (web, de rol van de werknemer of een VM Iaas) als er een gebeurtenis, zoals het aantal exemplaren van de client schaal omhoog of omlaag of implementeren van een nieuwe versie van de client of het automatisch schalen is ingeschakeld? In onze tests die we hebben gevonden automatisch schalen of omhoog en omlaag te schalen dat zijn uitgaande netwerkconnectiviteit verloren gedurende enkele seconden. StackExchange.Redis code is tegen dergelijke evenementen en zal opnieuw. Gedurende deze tijd opnieuw verbinding kunnen een time-out aanvragen in de wachtrij.

10. Is er een grote aanvraag voorafgaande aan meerdere kleine verzoeken naar de Cache bestand Vgx., waarvoor een time-out? De parameter `qs` in de fout bericht laat u weten hoeveel aanvragen van de client naar de server zijn verzonden, maar nog niet zijn verwerkt op een antwoord. Deze waarde kunt laten groeien omdat StackExchange.Redis een TCP-verbinding wordt gebruikt en één antwoord kan alleen worden gelezen op een tijdstip. Hoewel de eerste bewerking is een time-out, stopt niet de gegevens die worden verzonden vanaf de server en andere verzoeken geblokkeerd totdat deze is voltooid, veroorzaakt door een time-out. Een oplossing is de kans op time-outs minimaliseren door ervoor te zorgen dat de cache groot genoeg voor uw werkbelasting is en grote getallen splitsen in kleinere stukken. Een andere mogelijke oplossing is het gebruik van een groep `ConnectionMultiplexer` in de client-objecten en kies de minste geladen `ConnectionMultiplexer` wanneer u een nieuw verzoek verzendt. Hierdoor moet een time-out voor enkele andere aanvragen ook time-out veroorzaakt.

11. Als u `RedisSessionStateprovider`, hebt u de time-out voor opnieuw correct ingesteld. `retrytimeoutInMilliseconds`hoger dan `operationTimeoutinMilliseonds`, anders geen pogingen zal optreden. In het volgende voorbeeld `retrytimeoutInMilliseconds` is ingesteld op 3000. Zie [ASP.NET Session State Provider voor Azure bestand Vgx. Cache](cache-aspnet-session-state-provider.md) en [de configuratieparameters van sessie staat Provider en uitvoer-Cache-Provider gebruiken](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration)voor meer informatie.


    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


12. Geheugen op de server Azure bestand Vgx. Cache controleren door het [controleren van](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` en `Used Memory`. Als u een beleid voor verwijdering gereed is, start bestand Vgx. verwijderen wanneer toetsen `Used_Memory` de grootte van de cache. In het ideale geval `Used Memory RSS` moet alleen iets hoger dan `Used memory`. Een groot verschil betekent dat er geheugenfragmentatie van het (intern of extern. Wanneer `Used Memory RSS` is minder dan `Used Memory`, betekent dit dat deel van het cachegeheugen is door het besturingssysteem zijn verwisseld. Als dit gebeurt, kunt u sommige belangrijke vertragingstijden verwachten. Omdat het bestand Vgx. heeft geen controle over hoe de toewijzingen worden toegewezen aan de pagina's in geheugen, hoge `Used Memory RSS` is vaak het resultaat van een knelpunt in het geheugengebruik. Bestand Vgx. vrij geheugen, het geheugen terug aan de allocator wordt gegeven als de allocator kan of mag niet teruggeven het geheugen op het systeem. Mogelijk is er een verschil is tussen de `Used Memory` waarde en geheugen verbruik, zoals gerapporteerd door het besturingssysteem. Het is mogelijk omdat de geheugen heeft gebruikt en uitgebracht door het bestand Vgx. maar niet gezien terug naar het systeem. U kunt de volgende stappen uitvoeren om geheugenproblemen.
    -   De cache upgraden naar een groter formaat, zodat u niet zich verhouden tot de geheugenbeperkingen worden uitgevoerd op het systeem.
    -   Verlooptermijn op de toetsen zo instellen dat waarden van oudere proactief zijn verwijderd.
    -   Monitor de de `used_memory_rss` metric in de cache. Wanneer deze waarde de grootte van de cache benadert, bent u waarschijnlijk problemen met de prestaties zien. De gegevens verdelen over meerdere shards, als u met behulp van een premium-cache, of een upgrade uitvoert naar een groter formaat van de cache.

    Zie [Geheugenbelasting op de server](#memory-pressure-on-the-server)voor meer informatie.

## <a name="additional-information"></a>Als u meer informatie

-   [Cache bestand Vgx. aanbod en de grootte moet ik gebruiken?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
-   [Hoe kan ik benchmark en test de prestaties van mijn cache?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Hoe kan ik bestand Vgx. opdrachten uitvoeren?](cache-faq.md#how-can-i-run-redis-commands)
-   [Het controleren van Azure-Cache bestand Vgx.](cache-how-to-monitor.md)
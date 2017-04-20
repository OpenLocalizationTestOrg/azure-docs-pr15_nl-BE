<properties 
    pageTitle="Het bestand Vgx. Azure-Cache controleren | Microsoft Azure" 
    description="Informatie over het controleren van de gezondheid en de prestaties van uw sessies Azure-Cache bestand Vgx." 
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
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-monitor-azure-redis-cache"></a>Het controleren van Azure-Cache bestand Vgx.

Azure Cache bestand Vgx. biedt verschillende opties voor het controleren van uw sessies van de cache. U kunt statistieken bekijken, vastmaken metrics grafieken aan de Startboard de datum- en tijdbereik van monitoring grafieken aanpassen, toevoegen en metrische gegevens verwijderen uit de grafieken en waarschuwingen instellen wanneer aan bepaalde voorwaarden wordt voldaan. Deze hulpprogramma's kunnen u de status van uw sessies Azure bestand Vgx. Cache controleren en helpen bij het beheren van uw toepassingen in het cachegeheugen.

Als cache diagnostische gegevens zijn ingeschakeld, zijn maatstaven voor Azure bestand Vgx. Cache exemplaren ongeveer elke 30 seconden verzameld en opgeslagen, zodat ze kunnen worden weergegeven in de grafieken metrics en geëvalueerd door waarschuwingsregels.

Cache statistieken worden verzameld met behulp van het bestand Vgx. de opdracht [INFO](http://redis.io/commands/info) . Voor meer informatie over de gegevens van de verschillende waarden die worden gebruikt voor elke metrische gegevens in de cache, Zie [statistieken beschikbaar en intervallen te melden](#available-metrics-and-reporting-intervals).

Cache metrics, [Blader](cache-configure.md#configure-redis-cache-settings) naar uw exemplaar van de cache in [Azure portal](https://portal.azure.com)weergeven. Maatstaven voor Azure bestand Vgx. Cache exemplaren zijn toegankelijk op het **bestand Vgx. metrics** blade.

![Bestand metrics Vgx.][redis-cache-redis-metrics-blade]

>[AZURE.IMPORTANT] Als het volgende bericht wordt weergegeven in het **bestand Vgx. metrics** blade, volg de stappen in de sectie [cache diagnoseprogramma inschakelen](#enable-cache-diagnostics) cache diagnostics inschakelen.
>
>`Monitoring may not be enabled. Click here to turn on Diagnostics.`

Het **bestand Vgx. metrics** blade heeft **Monitoring** diagrammen cache statistieken weergegeven. Elke grafiek kan worden aangepast door het toevoegen of verwijderen van gegevens en de rapportage interval wijzigen. Voor het weergeven en configureren van bewerkingen en waarschuwingen, heeft de **Cache bestand Vgx.** blade **een sectie waarin de cache **gebeurtenissen** en **waarschuwingsregels**** .

## <a name="enable-cache-diagnostics"></a>Diagnostische gegevens van cache inschakelen

Azure Cache bestand Vgx. biedt u diagnostische gegevens hebt opgeslagen op een rekening van de opslag zodat u kunt alle hulpprogramma's die u wilt openen en de gegevens direct worden verwerkt. Om cache diagnostische gegevens moeten worden verzameld, moet opgeslagen en weergegeven in de portal Azure, een opslag-account worden geconfigureerd. Caches in dezelfde regio en abonnement delen dezelfde diagnostische gegevens opslag account en wanneer de configuratie wordt gewijzigd van toepassing op alle caches die in die regio in het abonnement.

Als u wilt inschakelen en configureren van cache diagnostics, navigeer naar de **Cache bestand Vgx.** blade voor uw exemplaar van de cache. Als de diagnostische gegevens nog niet zijn ingeschakeld, wordt een bericht weergegeven in plaats van een grafiek diagnostische gegevens.

![Diagnostische gegevens van cache inschakelen][redis-cache-enable-diagnostics]

Klik op het bericht de **Metric** blade en **Diagnostische instellingen** om te schakelen en de diagnostische instellingen configureren voor het exemplaar van de cache op.

![Diagnostische instellingen][redis-cache-diagnostic-settings]

![Diagnostische gegevens configureren][redis-cache-configure-diagnostics]

Klik **op** cache diagnoseprogramma inschakelen en weergeven van de configuratie van diagnostische.

Klik op de pijl rechts van de **Opslag-Account** te selecteren, een account opslag voor diagnostische gegevens. Selecteer een opslag voor de beste prestaties in hetzelfde gebied, als de cache.

Nadat de diagnostische instellingen zijn geconfigureerd, klikt u op **Opslaan** om de configuratie op te slaan. Houd er rekening mee dat het kan even duren om de wijzigingen van kracht te laten worden.

>[AZURE.IMPORTANT] Caches in dezelfde regio en abonnement delen dezelfde instellingen voor de diagnostische gegevens opslag en wanneer de configuratie gewijzigd (diagnostische gegevens ingeschakeld is/uitgeschakeld of het wijzigen van de account van de opslag) van toepassing op alle caches die in die regio in het abonnement.

Als u wilt de opgeslagen statistieken bekijken, onderzoeken de tabellen in uw account opslag met namen die met beginnen `WADMetrics`. Zie voor meer informatie over de toegang tot de opgeslagen gegevens buiten de portal Azure de sample [data Access bestand Vgx. Cache controleren](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) .

>[AZURE.NOTE] Alleen gegevens die zijn opgeslagen in de geselecteerde opslag account worden weergegeven in de portal Azure. Als u opslag accounts wijzigt, blijven de gegevens in de opslag van eerder geconfigureerde account downloaden, maar wordt deze niet weergegeven in de portal Azure.  

## <a name="available-metrics-and-reporting-intervals"></a>Beschikbare statistieken en rapportage van intervallen

Cache statistieken worden gemeld met meerdere intervallen voor rapportage, met inbegrip van **afgelopen uur**, **vandaag** **afgelopen week**en **aangepaste**. De blade **Metric** voor elke grafiek metrics geeft de gemiddelde, minimum en maximum waarden voor elke waarde in de grafiek en sommige maatstaven een totaal weergeven voor de rapportage interval. 

Elke maateenheid bevat twee versies. Een meetmethode prestaties voor de hele cache en caches met [clustering](cache-how-to-premium-clustering.md), een tweede versie van de metrische gegevens met `(Shard 0-9)` in de prestaties van de naam van maatregelen voor een enkele shard in een cache. Bijvoorbeeld als een cache heeft 4 shards, `Cache Hits` is het totale bedrag van de treffers voor de hele cache en `Cache Hits (Shard 3)` is alleen de treffers voor dat shard van de cache.

>[AZURE.NOTE] Zelfs wanneer de cache niet actief is met geen actieve verbonden clienttoepassingen, ziet u mogelijk sommige cacheactiviteit, zoals aangesloten clients, geheugengebruik en bewerkingen worden uitgevoerd. Deze activiteit is normaal tijdens de bewerking van een exemplaar van het bestand Vgx. Azure-Cache.

| Metric            | Beschrijving                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Treffers in cache        | Het aantal geslaagde zoekacties van sleutels tijdens het opgegeven interval voor rapportage. Die wordt toegewezen aan `keyspace_hits` uit het bestand Vgx. [INFO](http://redis.io/commands/info) opdracht.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Missers in cache      | Het aantal mislukte sleutel lookups tijdens het opgegeven interval voor rapportage. Die wordt toegewezen aan `keyspace_misses` met de opdracht bestand Vgx. INFO. Missers in cache worden niet noodzakelijkerwijs is er een probleem met de cache. Bijvoorbeeld wanneer u de cache braakgelegde programming patroon, een toepassing automatisch eerst gezocht in de cache voor een artikel. Als het item niet is er (Cachemisser), wordt het item uit de database opgehaald en toegevoegd aan de cache voor volgende keer. Missers in cache zijn normale werking voor het programmeren grond cache-patroon. Als het aantal missers in cache hoger is dan verwacht, Controleer de toepassingslogica die wordt gevuld en leest uit de cache. Als items zijn worden verwijderd uit de cache door geheugendruk vervolgens mogelijk aantal missers in cache, maar een beter criterium om te controleren op geheugenbelasting wordt `Used Memory` of `Evicted Keys`. |
| Aangesloten Clients | Het aantal clientverbindingen met de cache tijdens het opgegeven interval voor rapportage. Die wordt toegewezen aan `connected_clients` met de opdracht bestand Vgx. INFO. Daaropvolgende verbindingspogingen naar de cache mislukt als het [maximale aantal verbindingen](cache-configure.md#default-redis-server-configuration) is bereikt. Houd er rekening mee dat zelfs als er geen actieve clienttoepassing, er toch nog een paar exemplaren van verbonden cliënten, als gevolg van interne processen en verbindingen misschien.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Verwijderde sleutels      | Het aantal items uit de cache verwijderd tijdens de opgegeven reporting interval verschuldigd aan de `maxmemory` beperken. Die wordt toegewezen aan `evicted_keys` met de opdracht bestand Vgx. INFO.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Verlopen sleutels      | Het aantal items verlopen uit de cache tijdens het opgegeven interval voor rapportage. Deze waarde wordt toegewezen aan `expired_keys` met de opdracht bestand Vgx. INFO.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Deze eigenschap haalt              | Het aantal bewerkingen ophalen uit de cache tijdens het opgegeven interval voor rapportage. Deze waarde is de som van de volgende waarden van het bestand Vgx. INFO alle opdracht: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, en `cmdstat_getrange`, en is gelijk aan de som van het aantal cachetreffers en missers tijdens het interval voor rapportage.                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Bestand Vgx belasting van de Server. | Het percentage waarin de server bestand Vgx. bezet verwerken en niet wachten op niet-actief voor berichten is geëindigd. Als deze teller betekent dit dat het bestand Vgx. server heeft een maximum prestaties limiet en kan niet worden verwerkt door de CPU 100 moet u een sneller werken. Als u hoge bestand Vgx. belasting van de Server ziet u time-uitzonderingen op de client. In dit geval beter verticaal schalen of partitioneren van uw gegevens in meerdere caches.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Sets              | Het nummer van de set-bewerkingen tijdens het opgegeven interval voor rapportage aan de cache. Deze waarde is de som van de volgende waarden van het bestand Vgx. INFO alle opdracht: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange`, en `cmdstat_setnx`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Totale activiteiten  | Het totale aantal opdrachten verwerkt door de cacheserver tijdens het opgegeven interval voor rapportage. Deze waarde wordt toegewezen aan `total_commands_processed` met de opdracht bestand Vgx. INFO. Houd er rekening mee dat wanneer het bestand Vgx. Azure-Cache wordt gebruikt voor pub/sub zal er geen parameters voor `Cache Hits`, `Cache Misses`, `Gets`, of `Sets`, maar is er `Total Operations` parameters die overeenkomen met het gebruik van de cache voor pub/sub-bewerkingen.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Gebruikt geheugen       | Het bedrag van het cache-geheugen voor sleutel/waarde-paren in de cache in MB gebruikt tijdens het opgegeven interval voor rapportage. Deze waarde wordt toegewezen aan `used_memory` met de opdracht bestand Vgx. INFO. Dit omvat geen metagegevens of fragmentatie.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Gebruikt geheugen RSS   | Het bedrag van het cache-geheugen in MB gebruikt tijdens de opgegeven reporting interval, met inbegrip van fragmentatie en metagegevens. Deze waarde wordt toegewezen aan `used_memory_rss` met de opdracht bestand Vgx. INFO.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| CPU               | Het CPU-gebruik van de Cache voor Azure bestand Vgx. server als een percentage tijdens het opgegeven interval voor rapportage. Deze waarde wordt toegewezen aan het besturingssysteem `\Processor(_Total)\% Processor Time` Prestatiemeter.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Cache lezen        | De hoeveelheid gegevens lezen uit de cache in Megabytes per seconde (MB/s) tijdens het opgegeven interval voor rapportage. Deze waarde is afgeleid van de netwerkinterfacekaarten die ondersteuning bieden voor de virtuele machine die fungeert als host voor de cache en niet bestand Vgx. specifieke. **Deze waarde komt overeen met de bandbreedte die wordt gebruikt door deze cache. Als u wilt dat meldingen van server side netwerk bandbreedte limiet wilt instellen, maakt u met behulp van dit `Cache Read` teller. Zie [deze tabel](cache-faq.md#cache-performance) voor de waargenomen bandbreedte limieten voor verschillende niveaus en afmetingen prijzen-cache.**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Write cache       | Melden van de hoeveelheid gegevens die worden geschreven naar de cache in Megabytes per seconde (MB/s) in het opgegeven interval. Deze waarde is afgeleid van de netwerkinterfacekaarten die ondersteuning bieden voor de virtuele machine die fungeert als host voor de cache en niet bestand Vgx. specifieke. Deze waarde komt overeen met de bandbreedte van de gegevens die worden verzonden naar de cache van de client.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |


## <a name="how-to-view-metrics-and-customize-charts"></a>Statistieken bekijken en aanpassen van grafieken

U kunt een overzicht van de parameters voor de cache op de bladeserver **metrics bestand Vgx.** weergeven. Toegang tot het **bestand Vgx. metrics** blade **alle instellingen**kiezen > **bestand Vgx. maatstaven**.

![Bestand metrics Vgx.][redis-cache-redis-metrics]


Het **bestand Vgx. metrics** blade bevat de volgende grafieken.

| Grafiek metrics bestand Vgx. | Statistieken weergeven     |
|------------------|-------------------|
| Write cache lees- en Cache | Cache lezen |
|                            | Write cache |
| Aangesloten Clients      | Aangesloten Clients |
| Aantal treffers en missers  | Treffers in cache        |
|                  | Missers in cache      |
| Totale opdrachten   | Totale activiteiten  |
| Haalt en Sets    | Deze eigenschap haalt              |
|                  | Sets              |
| CPU-gebruik         | CPU           |
| Geheugengebruik      | Gebruikt geheugen   |
|                   | Gebruikt geheugen RSS |
| Bestand Vgx belasting van de Server. | Belasting van de server   |
| Aantal sleutel | Totale aantal sleutels |
|           | Verwijderde sleutels |
|           | Verlopen sleutels |


Voor een meer gedetailleerde weergave van de statistieken op een bepaalde grafiek en aanpassen van de grafiek, klikt u op het gewenste diagram uit het **bestand Vgx. metrics** blade om de bladeserver **Metric** voor deze grafiek weer te geven.

![Metrische blade][redis-cache-metric-blade]

Waarschuwingen die zijn ingesteld voor de metrische gegevens weergegeven in een grafiek worden weergegeven onder aan het blad **Metric** voor deze grafiek.

Als u wilt toevoegen of verwijderen van gegevens of rapportage interval wijzigen, kies **Grafiek bewerken**.

Als u wilt toevoegen of verwijderen van metrische gegevens uit de grafiek, klikt u op het selectievakje naast de naam van de metric. De rapportage als interval wilt wijzigen, klikt u op het gewenste interval. Het **grafiektype**wijzigen, klikt u op de gewenste stijl. Nadat u de gewenste wijzigingen zijn aangebracht, klikt u op **Opslaan**. 

![Grafiek bewerken][redis-cache-edit-chart]

Wanneer u **Opslaan** op blijft uw wijzigingen van kracht totdat u de **Metric** blade laat. Als u later terugkomen, ziet u de oorspronkelijke metric en tijdsbereik opnieuw. Zie voor meer informatie over het aanpassen van grafieken, [maatstaven voor Monitor-service](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

Om de statistieken te bekijken voor een bepaalde periode in een grafiek, de aanwijzer op een van de specifieke balken of punten op de grafiek die overeenkomt met de gewenste tijd en de parameters voor dat interval worden weergegeven.

![Details van de grafiek weergeven][redis-cache-view-chart-details]

## <a name="how-to-monitor-a-premium-cache-with-clustering"></a>Het controleren van een premium-cache met clustering

Premium caches met [clustering](cache-how-to-premium-clustering.md) is ingeschakeld, kunnen maximaal 10 shards hebben. Elke shard heeft een eigen maatstaven en deze gegevens worden samengevoegd om statistieken voor de cache als geheel. Elke maateenheid bevat twee versies. Een meetmethode prestaties voor de hele cache en een tweede versie van de metrische gegevens met `(Shard 0-9)` in de prestaties van de naam van maatregelen voor een enkele shard in een cache. Bijvoorbeeld als een cache heeft 3 shards, `Cache Hits` is het totale bedrag van de treffers voor de hele cache en `Cache Hits (Shard 2)` is alleen de treffers voor dat shard van de cache.

Elke controle grafiek geeft het hoogste niveau maatstaven voor de cache en de parameters voor elke cache shard.

![Monitor][redis-cache-premium-monitor]

De details voor dat punt de muis boven de gegevenspunten worden weergegeven in de tijd. 

![Monitor][redis-cache-premium-point-summary]

De grotere waarden zijn meestal de statistische waarden voor de cache, terwijl de kleinere waarden de afzonderlijke cijfers voor de shard worden. Houd er rekening mee dat in dit voorbeeld drie shards worden en treffers in de cache worden gelijkmatig verdeeld over de shards.

![Monitor][redis-cache-premium-point-shard]

Meer detail te zien klikt u op de grafiek om de uitgevouwen weergave op de **Metric** blade weer te geven.

![Monitor][redis-cache-premium-chart-detail]

Elke grafiek bevat standaard de Prestatiemeter voor de cache op het hoogste niveau, alsmede de prestatiemeteritems voor de afzonderlijke shards. U kunt deze op de **Grafiek bewerken** blade.

![Monitor][redis-cache-premium-edit]

Zie voor meer informatie over de beschikbare prestatiemeteritems [beschikbaar maatstaven en intervallen te melden](#available-metrics-and-reporting-intervals).


## <a name="operations-and-alerts"></a>Bewerkingen en waarschuwingen

De sectie **bewerkingen** op het blad **Cache bestand Vgx.** bevat **gebeurtenissen** en **waarschuwingsregels** secties.

![Oeprations][redis-cache-operations-events]

Klik op de grafiek **gebeurtenissen** om het blad **gebeurtenissen** weer te geven voor een overzicht van recente cache-activiteiten. Voorbeelden van activiteiten zijn op te halen en opnieuw genereren van sleutels van access en de activering en de resolutie van waarschuwingsregels. Voor meer informatie over elke gebeurtenis, klikt u op de gebeurtenis in het blad **gebeurtenissen** .

Zie voor meer informatie over gebeurtenissen, [gebeurtenissen weergeven en controlelogboeken](../monitoring-and-diagnostics/insights-debugging-with-events.md).

De sectie **waarschuwingsregels** geeft het aantal waarschuwingen voor het exemplaar van de cache. Een waarschuwingsregel kunt u uw exemplaar van de cache controleren en een e-mailbericht ontvangen wanneer er een bepaalde waarde bereikt de drempel van de regel. 

Waarschuwingsregels ongeveer vijf minuten worden geëvalueerd, en wanneer een waarschuwingsregel is geactiveerd, worden alle geconfigureerde meldingen verzonden. Waarschuwingsregel activeringen en meldingen niet verwerkt onmiddellijk; het is mogelijk dat er een vertraging van enkele minuten voordat een waarschuwingsregel wordt geactiveerd en meldingen worden verzonden.

Waarschuwingsregels kunnen weergeven en instellen van de bladeserver **Metric** voor een bepaalde grafiek controleren of van de bladeserver **waarschuwingsregels** .

Waarschuwingsregels hebben de volgende eigenschappen.

| Waarschuwingsregel, eigenschap                 | Beschrijving                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Resource                            | De bron door de waarschuwingsregel geëvalueerd. Wanneer u een waarschuwingsregel maakt vanuit een bestand Vgx. cache, is de cache voor de resource.                                                                                                                                                                                                                                                                                                                                                  |
| Naam                                | Naam die een unieke aanduiding voor de waarschuwingsregel binnen het huidige exemplaar van de cache.                                                                                                                                                                                                                                                                                                                                                                                       |
| Beschrijving                         | Optionele beschrijving van de huidige regel.                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Metric                              | De metric door de waarschuwingsregel worden bewaakt. Zie voor een lijst met cache metrics, beschikbare statistieken en rapportage van intervallen.                                                                                                                                                                                                                                                                                                                                             |
| Voorwaarde                           | Voorwaardeoperator voor de voor de waarschuwingsregel. Mogelijke opties zijn: groter dan, groter dan of gelijk aan, kleiner dan, kleiner dan of gelijk aan                                                                                                                                                                                                                                                                                                                             |
| Drempel                           | De waarde die wordt gebruikt om te vergelijken met de metric-waarde met de operator die is opgegeven door de eigenschap condition. Afhankelijk van de metric, deze waarde is bytes per seconde, bytes, % of aantal.                                                                                                                                                                                                                                                                                     |
| Periode                              | Hiermee geeft u de periode waarover de gemiddelde waarde van de metric wordt gebruikt voor de vergelijking van de waarschuwingsregel. Als de periode gedurende het laatste uur is, wordt de gemiddelde waarde van de metrische gegevens over de vorige uur interval bijvoorbeeld gebruikt voor de vergelijking. Als u worden gewaarschuwd wilt wanneer de drempel wordt bereikt door een Prikker in activiteit, is een kortere periode nodig. Om de hoogte worden gesteld wanneer de aanhoudende activiteit boven de drempelwaarde voor een langere periode te gebruiken. |
| E-mail service en CO-beheerders | Indien true, worden de servicebeheerder en de CO-beheerder gestuurd wanneer de waarschuwing wordt geactiveerd.                                                                                                                                                                                                                                                                                                                                                                    |
| E-mailadres van de beheerder aanvullende      | Optioneel e-mailadres voor een beheerder als u meer wilt worden gewaarschuwd als de waarschuwing wordt geactiveerd.                                                                                                                                                                                                                                                                                                                                                                    |

Slechts één melding wordt per waarschuwingsregel activering verzonden. Zodra de overschrijding van de drempelwaarde voor een regel en een melding wordt verzonden, wordt de regel niet opnieuw geëvalueerd totdat de metric beneden de drempel valt. De metric wordt vervolgens de drempel overschrijdt, de waarschuwing opnieuw wordt geactiveerd als een nieuwe melding wordt verzonden.

Alle van de regels voor uw exemplaar van de cache, klikt u op het onderdeel **waarschuwingsregels** in de **Cache bestand Vgx.** blade. Om alleen de regels die een specifieke metric gebruikt, gaat u naar de blade **Metric** voor de grafiek weer te geven bevat dat die waarde.

![Waarschuwingsregels][redis-cache-alert-rules]

Een waarschuwing als regel wilt toevoegen, klikt u op **waarschuwing toevoegen** vanuit de **Metric** blade of de bladeserver **waarschuwingsregels** . 

De criteria van de gewenste in het blad van de regel **een waarschuwing toevoegen** en klik op **OK**. 

![Waarschuwingsregel toevoegen][redis-cache-add-alert]

>[AZURE.NOTE] Wanneer een waarschuwingsregel wordt gemaakt door te klikken op de **melding toevoegen** vanuit de **Metric** blade, verschijnen alleen de cijfers weergegeven in de grafiek in dat blade in de vervolgkeuzelijst **Metric** . Wanneer een waarschuwingsregel wordt gemaakt door te klikken op **toevoegen signaal** van de bladeserver **waarschuwingsregels** , zijn alle cache metrische gegevens beschikbaar in de vervolgkeuzelijst **Metric** .

Zodra een waarschuwingsregel is opgeslagen wordt weergegeven op het blad **waarschuwingsregels** en op de bladeserver **Metric** voor alle grafieken waarin de metric die wordt gebruikt in de waarschuwingsregel. Een waarschuwing als regel wilt bewerken, klikt u op de naam van de waarschuwingsregel om de bladeserver **Regel bewerken** weer te geven. Van de bladeserver **Regel bewerken** kunt u de eigenschappen van de regel bewerken, verwijderen of de waarschuwingsregel uitschakelen of de regel opnieuw in te schakelen als u eerder hebt uitgeschakeld.

>[AZURE.NOTE] Wijzigingen die u in de eigenschappen van de regel aanbrengt het duurt even voordat ze worden weergegeven op het blad **waarschuwingsregels** of de bladeserver **Metric** .

Wanneer een waarschuwingsregel is geactiveerd, wordt een e-mailbericht wordt verzonden, afhankelijk van de configuratie van de waarschuwingsregel en een waarschuwingspictogram wordt weergegeven in het gedeelte **regels** op de bladeserver **Cache bestand Vgx.** .

Wordt beschouwd als een waarschuwingsregel worden opgelost wanneer de waarschuwing niet meer voorwaarde is true. Als de voorwaarde waarschuwingsregel opgelost is, wordt het waarschuwingspictogram vervangen met een vinkje. Klik voor meer informatie over waarschuwingen activeringen en resoluties, het deel van de **gebeurtenissen** op het blad **Bestand Vgx. Cache** om de gebeurtenissen op het blad **gebeurtenissen** weer te geven.

Zie voor meer informatie over waarschuwingen in Azure, [meldingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

## <a name="metrics-on-the-redis-cache-blade"></a>Metrics op de bladeserver Cache bestand Vgx.

De blade **Cache bestand Vgx.** weergegeven de volgende categorieën parameters.

-   [Monitoring van grafieken](#monitoring-charts)
-   [Gebruik van grafieken](#usage-charts)

### <a name="monitoring-charts"></a>Monitoring van grafieken

**De controlesectie** heeft het **aantal treffers en missers**, **opgehaald en ingesteld**, **verbindingen**en grafieken van de **Totale opdrachten** .

![Monitoring van grafieken][redis-cache-monitoring-part]

De grafieken **controle** weergegeven de volgende parameters.

| Grafiek controleren | Cache statistieken     |
|------------------|-------------------|
| Aantal treffers en missers  | Treffers in cache        |
|                  | Missers in cache      |
| Haalt en Sets    | Deze eigenschap haalt              |
|                  | Sets              |
| Verbindingen      | Aangesloten Clients |
| Totale opdrachten   | Totale activiteiten  |

Zie voor informatie over de parameters bekijken en aanpassen van de afzonderlijke grafieken in deze sectie, de volgende [Statistieken bekijken en aanpassen van grafieken metrics](#how-to-view-metrics-and-customize-charts) sectie.

### <a name="usage-charts"></a>Gebruik van grafieken

De sectie **Gebruik** heeft **Bestand Vgx. belasting van de Server**, **Geheugengebruik**, **Bandbreedte**en **CPU-gebruik** grafieken en geeft ook de **prijzen laag** voor het exemplaar van de cache.

![Gebruik van grafieken][redis-cache-usage-part]

De **prijzen laag** geeft de cache prijzen trapsgewijs en kan worden gebruikt om de [schaal](cache-how-to-scale.md) van de cache naar een andere laag van de prijzen.

De grafieken van het **gebruik van** weergegeven de volgende parameters.

| Gebruiksdiagram       | Cache statistieken |
|-------------------|---------------|
| Bestand Vgx belasting van de Server. | Belasting van de server   |
| Geheugengebruik      | Gebruikt geheugen   |
| Bandbreedte van het netwerk | Write cache   |
| CPU-gebruik         | CPU           |

Zie voor informatie over de parameters bekijken en aanpassen van de afzonderlijke grafieken in deze sectie, de volgende [Statistieken bekijken en aanpassen van grafieken metrics](#how-to-view-metrics-and-customize-charts) sectie.
  
<!-- IMAGES -->

[redis-cache-enable-diagnostics]: ./media/cache-how-to-monitor/redis-cache-enable-diagnostics.png

[redis-cache-diagnostic-settings]: ./media/cache-how-to-monitor/redis-cache-diagnostic-settings.png

[redis-cache-configure-diagnostics]: ./media/cache-how-to-monitor/redis-cache-configure-diagnostics.png

[redis-cache-monitoring-part]: ./media/cache-how-to-monitor/redis-cache-monitoring-part.png

[redis-cache-usage-part]: ./media/cache-how-to-monitor/redis-cache-usage-part.png

[redis-cache-metric-blade]: ./media/cache-how-to-monitor/redis-cache-metric-blade.png

[redis-cache-edit-chart]: ./media/cache-how-to-monitor/redis-cache-edit-chart.png

[redis-cache-view-chart-details]: ./media/cache-how-to-monitor/redis-cache-view-chart-details.png

[redis-cache-operations-events]: ./media/cache-how-to-monitor/redis-cache-operations-events.png

[redis-cache-alert-rules]: ./media/cache-how-to-monitor/redis-cache-alert-rules.png

[redis-cache-add-alert]: ./media/cache-how-to-monitor/redis-cache-add-alert.png

[redis-cache-premium-monitor]: ./media/cache-how-to-monitor/redis-cache-premium-monitor.png

[redis-cache-premium-edit]: ./media/cache-how-to-monitor/redis-cache-premium-edit.png

[redis-cache-premium-chart-detail]: ./media/cache-how-to-monitor/redis-cache-premium-chart-detail.png

[redis-cache-premium-point-summary]: ./media/cache-how-to-monitor/redis-cache-premium-point-summary.png

[redis-cache-premium-point-shard]: ./media/cache-how-to-monitor/redis-cache-premium-point-shard.png

[redis-cache-redis-metrics]: ./media/cache-how-to-monitor/redis-cache-redis-metrics.png

[redis-cache-redis-metrics-blade]: ./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png




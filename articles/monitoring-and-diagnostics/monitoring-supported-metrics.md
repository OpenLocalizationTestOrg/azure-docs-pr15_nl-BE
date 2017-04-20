<properties
    pageTitle="Azure Monitor metrics - ondersteunde parameters per resourcetype | Microsoft Azure"
    description="Lijst met parameters beschikbaar voor elk resourcetype monitor Azure."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="supported-metrics-with-azure-monitor"></a>Ondersteunde parameters met Azure Monitor

Azure Monitor biedt verschillende manieren om te werken met statistieken, met inbegrip van deze grafiek in de portal, toegang hebben tot deze via de REST API of ze opvragen met PowerShell of CLI. Hieronder is een volledige lijst van alle metrische gegevens momenteel beschikbaar met metrische pijpleiding van Azure Monitor.

> [AZURE.NOTE] Andere parameters zijn in de portal of met oudere API's. Deze lijst bevat alleen de openbare preview statistieken beschikbaar met behulp van de public preview van de geconsolideerde Azure Monitor metrische pijpleiding.

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|CoreCount|Aantal Core|Aantal|Totaal|Totaal aantal kernen in de batch-account|
|TotalNodeCount|Aantal knooppunten|Aantal|Totaal|Totaal aantal knooppunten in de batch-account|
|CreatingNodeCount|Aantal knooppunten maken|Aantal|Totaal|Aantal knooppunten dat wordt gemaakt|
|StartingNodeCount|Aantal knooppunten starten|Aantal|Totaal|Aantal knooppunten starten|
|WaitingForStartTaskNodeCount|Wachten voor Start knooppunt telling van taken|Aantal|Totaal|Aantal knooppunten die wachten op de taak starten te voltooien|
|StartTaskFailedNodeCount|Begindatum taak knooppunt Count is mislukt|Aantal|Totaal|Aantal knooppunten waarin de taak starten is mislukt|
|IdleNodeCount|Het aantal niet-actieve knooppunt|Aantal|Totaal|Aantal niet-actieve knooppunten|
|OfflineNodeCount|Off line knooppunt tellen|Aantal|Totaal|Aantal knooppunten dat off line|
|RebootingNodeCount|Telling van knooppunt opnieuw op te starten|Aantal|Totaal|Aantal knooppunten opnieuw opstarten|
|ReimagingNodeCount|Aantal knooppunt reimaging|Aantal|Totaal|Aantal knooppunten op reimaging|
|RunningNodeCount|Lopende telling knooppunt|Aantal|Totaal|Aantal actieve knooppunten|
|LeavingPoolNodeCount|Aantal Pools knooppunt verlaten|Aantal|Totaal|Aantal knooppunten dat de groep verlaten|
|UnusableNodeCount|Onbruikbaar knooppunt tellen|Aantal|Totaal|Aantal knooppunten dat onbruikbaar|
|TaskStartEvent|Gebeurtenissen van taak starten|Aantal|Totaal|Totaal aantal taken die zijn begonnen|
|TaskCompleteEvent|Taak Complete-gebeurtenissen|Aantal|Totaal|Totaal aantal taken die voltooid|
|TaskFailEvent|Taak mislukt evenementen|Aantal|Totaal|Totaal aantal taken die voltooid in een mislukte staat|
|PoolCreateEvent|Gebeurtenissen van toepassingen maken|Aantal|Totaal|Totaal aantal groepen die zijn gemaakt|
|PoolResizeStartEvent|Groep formaat Start gebeurtenissen|Aantal|Totaal|Totaal aantal van het formaat van toepassingen die zijn gestart|
|PoolResizeCompleteEvent|Groep formaat Complete-gebeurtenissen|Aantal|Totaal|Totaal aantal toepassingen het formaat hebt voltooid|
|PoolDeleteStartEvent|Start gebeurtenissen van toepassingen verwijderen|Aantal|Totaal|Totaal aantal toepassingen verwijdert die zijn gestart|
|PoolDeleteCompleteEvent|Toepassingen verwijderen Complete-gebeurtenissen|Aantal|Totaal|Totaal aantal toepassingen verwijdert die voltooid|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|connectedclients|Aangesloten Clients|Aantal|Maximum||
|totalcommandsprocessed|Totale activiteiten|Aantal|Totaal||
|cachehits|Treffers in cache|Aantal|Totaal||
|cachemisses|Missers in cache|Aantal|Totaal||
|getcommands|Deze eigenschap haalt|Aantal|Totaal||
|setcommands|Sets|Aantal|Totaal||
|evictedkeys|Verwijderde sleutels|Aantal|Totaal||
|totalkeys|Totale aantal sleutels|Aantal|Maximum||
|expiredkeys|Verlopen sleutels|Aantal|Totaal||
|usedmemory|Gebruikt geheugen|Bytes|Maximum||
|usedmemoryRss|Gebruikt geheugen RSS|Bytes|Maximum||
|serverLoad|Belasting van de server|Procent|Maximum||
|cacheWrite|Write cache|BytesPerSecond|Maximum||
|cacheRead|Cache lezen|BytesPerSecond|Maximum||
|percentProcessorTime|CPU|Procent|Maximum||
|connectedclients0|Aangesloten Clients (Shard 0)|Aantal|Maximum||
|totalcommandsprocessed0|Totale activiteiten (Shard 0)|Aantal|Totaal||
|cachehits0|Treffers in cache (Shard 0)|Aantal|Totaal||
|cachemisses0|Missers in cache (Shard 0)|Aantal|Totaal||
|getcommands0|Deze eigenschap haalt (Shard 0)|Aantal|Totaal||
|setcommands0|Sets (Shard 0)|Aantal|Totaal||
|evictedkeys0|Verwijderde sleutels (Shard 0)|Aantal|Totaal||
|totalkeys0|Totale aantal sleutels (knooppunt 0)|Aantal|Maximum||
|expiredkeys0|Verlopen sleutels (Shard 0)|Aantal|Totaal||
|usedmemory0|Gebruikt geheugen (Shard 0)|Bytes|Maximum||
|usedmemoryRss0|Gebruikt geheugen RSS (Shard 0)|Bytes|Maximum||
|serverLoad0|Belasting van de server (Shard 0)|Procent|Maximum||
|cacheWrite0|Write cache (Shard 0)|BytesPerSecond|Maximum||
|cacheRead0|Cache lezen (Shard 0)|BytesPerSecond|Maximum||
|percentProcessorTime0|CPU (Shard 0)|Procent|Maximum||
|connectedclients1|Aangesloten Clients (Shard 1)|Aantal|Maximum||
|totalcommandsprocessed1|Totale activiteiten (Shard 1)|Aantal|Totaal||
|cachehits1|Treffers in cache (Shard 1)|Aantal|Totaal||
|cachemisses1|Missers in cache (Shard 1)|Aantal|Totaal||
|getcommands1|Deze eigenschap haalt (Shard 1)|Aantal|Totaal||
|setcommands1|Sets (Shard 1)|Aantal|Totaal||
|evictedkeys1|Verwijderde sleutels (Shard 1)|Aantal|Totaal||
|totalkeys1|Totale aantal sleutels (knooppunt 1)|Aantal|Maximum||
|expiredkeys1|Verlopen sleutels (Shard 1)|Aantal|Totaal||
|usedmemory1|Gebruikt geheugen (Shard 1)|Bytes|Maximum||
|usedmemoryRss1|Gebruikt geheugen RSS (Shard 1)|Bytes|Maximum||
|serverLoad1|Belasting van de server (Shard 1)|Procent|Maximum||
|cacheWrite1|Write cache (Shard 1)|BytesPerSecond|Maximum||
|cacheRead1|Cache lezen (Shard 1)|BytesPerSecond|Maximum||
|percentProcessorTime1|CPU (Shard 1)|Procent|Maximum||
|connectedclients2|Aangesloten Clients (Shard 2)|Aantal|Maximum||
|totalcommandsprocessed2|Totale activiteiten (Shard 2)|Aantal|Totaal||
|cachehits2|Treffers in cache (Shard 2)|Aantal|Totaal||
|cachemisses2|Missers in cache (Shard 2)|Aantal|Totaal||
|getcommands2|Deze eigenschap haalt (Shard 2)|Aantal|Totaal||
|setcommands2|Sets (Shard 2)|Aantal|Totaal||
|evictedkeys2|Verwijderde sleutels (Shard 2)|Aantal|Totaal||
|totalkeys2|Totale aantal sleutels (knooppunt 2)|Aantal|Maximum||
|expiredkeys2|Verlopen sleutels (Shard 2)|Aantal|Totaal||
|usedmemory2|Gebruikt geheugen (Shard 2)|Bytes|Maximum||
|usedmemoryRss2|Gebruikt geheugen RSS (Shard 2)|Bytes|Maximum||
|serverLoad2|Belasting van de server (Shard 2)|Procent|Maximum||
|cacheWrite2|Write cache (Shard 2)|BytesPerSecond|Maximum||
|cacheRead2|Cache lezen (Shard 2)|BytesPerSecond|Maximum||
|percentProcessorTime2|CPU (Shard 2)|Procent|Maximum||
|connectedclients3|Aangesloten Clients (Shard 3)|Aantal|Maximum||
|totalcommandsprocessed3|Totale activiteiten (Shard 3)|Aantal|Totaal||
|cachehits3|Treffers in cache (Shard 3)|Aantal|Totaal||
|cachemisses3|Missers in cache (Shard 3)|Aantal|Totaal||
|getcommands3|Deze eigenschap haalt (Shard 3)|Aantal|Totaal||
|setcommands3|Sets (Shard 3)|Aantal|Totaal||
|evictedkeys3|Verwijderde sleutels (Shard 3)|Aantal|Totaal||
|totalkeys3|Totale aantal sleutels (knooppunt 3)|Aantal|Maximum||
|expiredkeys3|Verlopen sleutels (Shard 3)|Aantal|Totaal||
|usedmemory3|Gebruikt geheugen (Shard 3)|Bytes|Maximum||
|usedmemoryRss3|Gebruikt geheugen RSS (Shard 3)|Bytes|Maximum||
|serverLoad3|Belasting van de server (Shard 3)|Procent|Maximum||
|cacheWrite3|Write cache (Shard 3)|BytesPerSecond|Maximum||
|cacheRead3|Cache lezen (Shard 3)|BytesPerSecond|Maximum||
|percentProcessorTime3|CPU (Shard 3)|Procent|Maximum||
|connectedclients4|Aangesloten Clients (Shard 4)|Aantal|Maximum||
|totalcommandsprocessed4|Totale activiteiten (Shard 4)|Aantal|Totaal||
|cachehits4|Treffers in cache (Shard 4)|Aantal|Totaal||
|cachemisses4|Missers in cache (Shard 4)|Aantal|Totaal||
|getcommands4|Deze eigenschap haalt (Shard 4)|Aantal|Totaal||
|setcommands4|Sets (4 Shard)|Aantal|Totaal||
|evictedkeys4|Verwijderde sleutels (Shard 4)|Aantal|Totaal||
|totalkeys4|Totale aantal sleutels (knooppunt 4)|Aantal|Maximum||
|expiredkeys4|Verlopen sleutels (Shard 4)|Aantal|Totaal||
|usedmemory4|Gebruikt geheugen (Shard 4)|Bytes|Maximum||
|usedmemoryRss4|Gebruikt geheugen RSS (Shard 4)|Bytes|Maximum||
|serverLoad4|Belasting van de server (Shard 4)|Procent|Maximum||
|cacheWrite4|Write cache (Shard 4)|BytesPerSecond|Maximum||
|cacheRead4|Cache lezen (Shard 4)|BytesPerSecond|Maximum||
|percentProcessorTime4|CPU (Shard 4)|Procent|Maximum||
|connectedclients5|Aangesloten Clients (Shard 5)|Aantal|Maximum||
|totalcommandsprocessed5|Totale activiteiten (Shard 5)|Aantal|Totaal||
|cachehits5|Treffers in cache (Shard 5)|Aantal|Totaal||
|cachemisses5|Missers in cache (Shard 5)|Aantal|Totaal||
|getcommands5|Deze eigenschap haalt (Shard 5)|Aantal|Totaal||
|setcommands5|(Shard 5) sets|Aantal|Totaal||
|evictedkeys5|Verwijderde sleutels (Shard 5)|Aantal|Totaal||
|totalkeys5|Totale aantal sleutels (knooppunt 5)|Aantal|Maximum||
|expiredkeys5|Verlopen sleutels (Shard 5)|Aantal|Totaal||
|usedmemory5|Gebruikt geheugen (Shard 5)|Bytes|Maximum||
|usedmemoryRss5|Gebruikt geheugen RSS (Shard 5)|Bytes|Maximum||
|serverLoad5|Belasting van de server (Shard 5)|Procent|Maximum||
|cacheWrite5|Write cache (Shard 5)|BytesPerSecond|Maximum||
|cacheRead5|Cache lezen (Shard 5)|BytesPerSecond|Maximum||
|percentProcessorTime5|CPU (Shard 5)|Procent|Maximum||
|connectedclients6|Aangesloten Clients (Shard 6)|Aantal|Maximum||
|totalcommandsprocessed6|Totale activiteiten (Shard 6)|Aantal|Totaal||
|cachehits6|Treffers in cache (Shard 6)|Aantal|Totaal||
|cachemisses6|Missers in cache (Shard 6)|Aantal|Totaal||
|getcommands6|Deze eigenschap haalt (Shard 6)|Aantal|Totaal||
|setcommands6|Sets (Shard 6)|Aantal|Totaal||
|evictedkeys6|Verwijderde sleutels (Shard 6)|Aantal|Totaal||
|totalkeys6|Totale aantal sleutels (knooppunt 6)|Aantal|Maximum||
|expiredkeys6|Verlopen sleutels (Shard 6)|Aantal|Totaal||
|usedmemory6|Gebruikt geheugen (Shard 6)|Bytes|Maximum||
|usedmemoryRss6|Gebruikt geheugen RSS (Shard 6)|Bytes|Maximum||
|serverLoad6|Belasting van de server (Shard 6)|Procent|Maximum||
|cacheWrite6|Write cache (Shard 6)|BytesPerSecond|Maximum||
|cacheRead6|Cache lezen (Shard 6)|BytesPerSecond|Maximum||
|percentProcessorTime6|CPU (Shard 6)|Procent|Maximum||
|connectedclients7|Aangesloten Clients (Shard 7)|Aantal|Maximum||
|totalcommandsprocessed7|Totale activiteiten (Shard 7)|Aantal|Totaal||
|cachehits7|Treffers in cache (Shard 7)|Aantal|Totaal||
|cachemisses7|Missers in cache (Shard 7)|Aantal|Totaal||
|getcommands7|Deze eigenschap haalt (Shard 7)|Aantal|Totaal||
|setcommands7|Sets (Shard 7)|Aantal|Totaal||
|evictedkeys7|Verwijderde sleutels (Shard 7)|Aantal|Totaal||
|totalkeys7|Totale aantal sleutels (knooppunt 7)|Aantal|Maximum||
|expiredkeys7|Verlopen sleutels (Shard 7)|Aantal|Totaal||
|usedmemory7|Gebruikt geheugen (Shard 7)|Bytes|Maximum||
|usedmemoryRss7|Gebruikt geheugen RSS (Shard 7)|Bytes|Maximum||
|serverLoad7|Belasting van de server (Shard 7)|Procent|Maximum||
|cacheWrite7|Write cache (Shard 7)|BytesPerSecond|Maximum||
|cacheRead7|Cache lezen (Shard 7)|BytesPerSecond|Maximum||
|percentProcessorTime7|CPU (Shard 7)|Procent|Maximum||
|connectedclients8|Aangesloten Clients (Shard 8)|Aantal|Maximum||
|totalcommandsprocessed8|Totale activiteiten (Shard 8)|Aantal|Totaal||
|cachehits8|Treffers in cache (Shard 8)|Aantal|Totaal||
|cachemisses8|Missers in cache (Shard 8)|Aantal|Totaal||
|getcommands8|Deze eigenschap haalt (Shard 8)|Aantal|Totaal||
|setcommands8|Sets (Shard 8)|Aantal|Totaal||
|evictedkeys8|Verwijderde sleutels (Shard 8)|Aantal|Totaal||
|totalkeys8|Totale aantal sleutels (knooppunt 8)|Aantal|Maximum||
|expiredkeys8|Verlopen sleutels (Shard 8)|Aantal|Totaal||
|usedmemory8|Gebruikt geheugen (Shard 8)|Bytes|Maximum||
|usedmemoryRss8|Gebruikt geheugen RSS (Shard 8)|Bytes|Maximum||
|serverLoad8|Belasting van de server (Shard 8)|Procent|Maximum||
|cacheWrite8|Write cache (Shard 8)|BytesPerSecond|Maximum||
|cacheRead8|Cache lezen (Shard 8)|BytesPerSecond|Maximum||
|percentProcessorTime8|CPU (Shard 8)|Procent|Maximum||
|connectedclients9|Aangesloten Clients (Shard 9)|Aantal|Maximum||
|totalcommandsprocessed9|Totale activiteiten (Shard 9)|Aantal|Totaal||
|cachehits9|Treffers in cache (Shard 9)|Aantal|Totaal||
|cachemisses9|Missers in cache (Shard 9)|Aantal|Totaal||
|getcommands9|Deze eigenschap haalt (Shard 9)|Aantal|Totaal||
|setcommands9|Sets (Shard 9)|Aantal|Totaal||
|evictedkeys9|Verwijderde sleutels (Shard 9)|Aantal|Totaal||
|totalkeys9|Totale aantal sleutels (knooppunt 9)|Aantal|Maximum||
|expiredkeys9|Verlopen sleutels (Shard 9)|Aantal|Totaal||
|usedmemory9|Gebruikt geheugen (Shard 9)|Bytes|Maximum||
|usedmemoryRss9|Gebruikt geheugen RSS (Shard 9)|Bytes|Maximum||
|serverLoad9|Belasting van de server (Shard 9)|Procent|Maximum||
|cacheWrite9|Write cache (Shard 9)|BytesPerSecond|Maximum||
|cacheRead9|Cache lezen (Shard 9)|BytesPerSecond|Maximum||
|percentProcessorTime9|CPU (Shard 9)|Procent|Maximum||

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|NumberOfCalls|Totaal aantal API-aanroepen|Aantal|Totaal|Totaal aantal API-aanroepen.|
|NumberOfFailedCalls|Totaal aantal mislukte API-aanroepen|Aantal|Totaal|Totaal aantal mislukte API-aanroepen.|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|Percentage CPU|Percentage CPU|Procent|Gemiddelde|Het percentage toegewezen compute eenheden die momenteel in gebruik door de Virtual Machine(s)|
|Het netwerk|Het netwerk|Bytes|Totaal|Het aantal bytes dat is ontvangen op alle netwerkinterfaces van het Virtual Machine(s) (binnenkomend verkeer)|
|Uit het netwerk|Uit het netwerk|Bytes|Totaal|Het aantal bytes op alle netwerkinterfaces van het Virtual Machine(s) (uitgaand verkeer)|
|Schijf gelezen Bytes|Schijf gelezen Bytes|Bytes|Totaal|Het totale aantal bytes dat is gelezen van schijf tijdens de periode controleren|
|Schijf: geschreven Bytes|Schijf: geschreven Bytes|Bytes|Totaal|Totaal aantal bytes dat is weggeschreven naar de schijf tijdens de periode controleren|
|Schijf lezen per seconde|Schijf lezen per seconde|CountPerSecond|Gemiddelde|Lezen van de schijf IOP 's|
|Schijf schrijven per seconde|Schijf schrijven per seconde|CountPerSecond|Gemiddelde|Schijf schrijven IOP 's|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|Percentage CPU|Percentage CPU|Procent|Gemiddelde|Het percentage toegewezen compute eenheden die momenteel in gebruik door de Virtual Machine(s)|
|Het netwerk|Het netwerk|Bytes|Totaal|Het aantal bytes dat is ontvangen op alle netwerkinterfaces van het Virtual Machine(s) (binnenkomend verkeer)|
|Uit het netwerk|Uit het netwerk|Bytes|Totaal|Het aantal bytes op alle netwerkinterfaces van het Virtual Machine(s) (uitgaand verkeer)|
|Schijf gelezen Bytes|Schijf gelezen Bytes|Bytes|Totaal|Het totale aantal bytes dat is gelezen van schijf tijdens de periode controleren|
|Schijf: geschreven Bytes|Schijf: geschreven Bytes|Bytes|Totaal|Totaal aantal bytes dat is weggeschreven naar de schijf tijdens de periode controleren|
|Schijf lezen per seconde|Schijf lezen per seconde|CountPerSecond|Gemiddelde|Lezen van de schijf IOP 's|
|Schijf schrijven per seconde|Schijf schrijven per seconde|CountPerSecond|Gemiddelde|Schijf schrijven IOP 's|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|Percentage CPU|Percentage CPU|Procent|Gemiddelde|Het percentage toegewezen compute eenheden die momenteel in gebruik door de Virtual Machine(s)|
|Het netwerk|Het netwerk|Bytes|Totaal|Het aantal bytes dat is ontvangen op alle netwerkinterfaces van het Virtual Machine(s) (binnenkomend verkeer)|
|Uit het netwerk|Uit het netwerk|Bytes|Totaal|Het aantal bytes op alle netwerkinterfaces van het Virtual Machine(s) (uitgaand verkeer)|
|Schijf gelezen Bytes|Schijf gelezen Bytes|Bytes|Totaal|Het totale aantal bytes dat is gelezen van schijf tijdens de periode controleren|
|Schijf: geschreven Bytes|Schijf: geschreven Bytes|Bytes|Totaal|Totaal aantal bytes dat is weggeschreven naar de schijf tijdens de periode controleren|
|Schijf lezen per seconde|Schijf lezen per seconde|CountPerSecond|Gemiddelde|Lezen van de schijf IOP 's|
|Schijf schrijven per seconde|Schijf schrijven per seconde|CountPerSecond|Gemiddelde|Schijf schrijven IOP 's|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Pogingen van telemetrie bericht verzenden|Aantal|Totaal|Nummer van het apparaat naar Cloud telemetrie-berichten worden verzonden naar uw hub IoT geprobeerd|
|d2c.telemetry.ingress.Success|Telemetrie-berichten|Aantal|Totaal|Aantal apparaat wolk telemetrie berichten verzonden naar uw hub IoT|
|c2d.Commands.egress.complete.Success|Opdrachten voltooid|Aantal|Totaal|Nummer van de wolk met opdrachten in het apparaat is voltooid door het apparaat|
|c2d.Commands.egress.Abandon.Success|Opdrachten afgebroken|Aantal|Totaal|Nummer van de wolk met opdrachten in het apparaat afgebroken door het apparaat|
|c2d.Commands.egress.Reject.Success|Opdrachten geweigerd|Aantal|Totaal|Aantal Cloud apparaat-opdrachten geweigerd door het apparaat|
|devices.totalDevices|Totale apparaten|Aantal|Totaal|Aantal apparaten op de hub IoT geregistreerd|
|devices.connectedDevices.allProtocol|Aangesloten apparaten|Aantal|Totaal|Aantal apparaten aangesloten op de hub IoT|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|INREQS|Inkomende aanvragen|Aantal|Totaal|Gebeurtenis Hub binnenkomende bericht doorvoer voor een naamruimte|
|SUCCREQ|Geslaagde aanvragen|Aantal|Totaal|Totaal aantal geslaagde aanvragen voor een naamruimte|
|FAILREQ|Mislukte aanvragen|Aantal|Totaal|Totaal aantal mislukte aanvragen voor een naamruimte|
|SVRBSY|Server bezet fouten|Aantal|Totaal|Totaal server bezet fouten voor een naamruimte|
|INTERR|Interne serverfouten|Aantal|Totaal|Totale interne serverfouten voor een naamruimte|
|MISCERR|Andere fouten|Aantal|Totaal|Totaal aantal mislukte aanvragen voor een naamruimte|
|INMSGS|Inkomende berichten|Aantal|Totaal|Totaal aantal binnenkomende berichten voor een naamruimte|
|OUTMSGS|Uitgaande berichten|Aantal|Totaal|Totaal uitgaande berichten voor een naamruimte|
|EHINMBS|Binnenkomende bytes per seconde|BytesPerSecond|Totaal|Gebeurtenis Hub binnenkomende bericht doorvoer voor een naamruimte|
|EHOUTMBS|Uitgaande bytes per seconde|BytesPerSecond|Totaal|Totaal uitgaande berichten voor een naamruimte|
|EHABL|Reserve-berichten archiveren|Aantal|Totaal|Hub archief gebeurtenisberichten in reserve voor een naamruimte|
|EHAMSGS|Berichten archiveren|Aantal|Totaal|Gebeurtenis Hub gearchiveerde berichten in een naamruimte|
|EHAMBS|Doorvoer van berichten afneemt archief|BytesPerSecond|Totaal|Gebeurtenis Hub gearchiveerd bericht doorvoersnelheid in een naamruimte|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|RunsStarted|Gestart wordt uitgevoerd|Aantal|Totaal|Nummer van de werkstroom wordt gestart.|
|RunsCompleted|Wordt uitgevoerd, voltooid|Aantal|Totaal|Nummer van de werkstroom wordt voltooid.|
|RunsSucceeded|Wordt uitgevoerd is voltooid|Aantal|Totaal|Nummer van de werkstroom wordt uitgevoerd is gelukt.|
|RunsFailed|Kan niet worden uitgevoerd|Aantal|Totaal|Nummer van de werkstroom wordt uitgevoerd is mislukt.|
|RunsCancelled|Geannuleerd wordt uitgevoerd|Aantal|Totaal|Nummer van de werkstroom wordt geannuleerd.|
|RunLatency|Latentie uitvoeren|Seconden|Gemiddelde|Latentie van voltooide workflow wordt uitgevoerd.|
|RunSuccessLatency|Latentie met succes uitgevoerd|Seconden|Gemiddelde|Latentie van geslaagde workflow wordt uitgevoerd.|
|RunThrottledEvents|Beperkt gebeurtenissen uitvoeren|Aantal|Totaal|Aantal werkstroomactie of trigger de snelheid van de gebeurtenissen.|
|RunFailurePercentage|Percentage mislukte uitvoeren|Procent|Totaal|Percentage van de werkstroom wordt uitgevoerd is mislukt.|
|ActionsStarted|Acties gestart |Aantal|Totaal|Het nummer van de werkstroom is gestart.|
|ActionsCompleted|Acties voltooid |Aantal|Totaal|Het nummer van de werkstroom voltooid.|
|ActionsSucceeded|Geslaagde acties |Aantal|Totaal|Aantal werkstroomacties is voltooid.|
|ActionsFailed|Acties is mislukt|Aantal|Totaal|Aantal workflowacties is mislukt.|
|ActionsSkipped|Acties overgeslagen |Aantal|Totaal|Aantal workflowacties overgeslagen.|
|ActionLatency|Latentie in actie |Seconden|Gemiddelde|Latentie van voltooide workflowacties.|
|ActionSuccessLatency|Latentie van geslaagde actie |Seconden|Gemiddelde|Latentie van geslaagde workflowacties.|
|ActionThrottledEvents|Actie de snelheid van de gebeurtenissen|Aantal|Totaal|Aantal werkstroomactie vertraagde gebeurtenissen...|
|TriggersStarted|Triggers gestart |Aantal|Totaal|Aantal triggers workflow gestart.|
|TriggersCompleted|Triggers voltooid |Aantal|Totaal|Aantal triggers van workflow voltooid.|
|TriggersSucceeded|Triggers is voltooid |Aantal|Totaal|Aantal triggers workflow is voltooid.|
|TriggersFailed|Triggers is mislukt |Aantal|Totaal|Aantal triggers werkstroom is mislukt.|
|TriggersSkipped|Triggers overgeslagen|Aantal|Totaal|Aantal triggers workflow overgeslagen.|
|TriggersFired|Triggers geactiveerd |Aantal|Totaal|Aantal triggers van de werkstroom heeft gestart.|
|TriggerLatency|Vertragingstijd trigger |Seconden|Gemiddelde|Latentie van voltooide workflow triggers.|
|TriggerFireLatency|Trigger brand latentie |Seconden|Gemiddelde|Latentie van triggers werkstroom wordt gestart.|
|TriggerSuccessLatency|Trigger succes latentie |Seconden|Gemiddelde|Latentie van triggers workflow voltooid.|
|TriggerThrottledEvents|Trigger de snelheid van de gebeurtenissen|Aantal|Totaal|De snelheid van de werkstroom trigger aantal gebeurtenissen.|
|BillableActionExecutions|Te factureren actie uitvoeringen|Aantal|Totaal|Het aantal workflow actie keer ophalen in rekening gebracht.|
|BillableTriggerExecutions|Te factureren Trigger uitvoeringen|Aantal|Totaal|Aantal uitvoeringen van workflow trigger ophalen in rekening gebracht.|
|TotalBillableExecutions|Totale te factureren uitvoeringen|Aantal|Totaal|Het aantal keer dat werkstroom ophalen in rekening gebracht.|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|Doorvoer|Doorvoer|BytesPerSecond|Gemiddelde||

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|SearchLatency|Latentie zoeken|Seconden|Gemiddelde|Zoeken op gemiddelde latentie voor de zoekservice|
|SearchQueriesPerSecond|Zoekquery's per seconde|CountPerSecond|Gemiddelde|Zoekquery's per seconde voor de zoekservice|
|ThrottledSearchQueriesPercentage|Percentage van beperkt search query 's|Procent|Gemiddelde|Percentage van de zoekopdrachten die zijn beperkt voor de zoekservice|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|CPUXNS|CPU-gebruik per naamruimte|Procent|Maximum|Service bus premium naamruimte CPU-gebruik metric|
|WSXNS|Grootte van geheugengebruik per naamruimte|Procent|Maximum|Service bus premium naamruimte geheugen gebruik metric|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|cpu_percent|CPU-percentage|Procent|Gemiddelde|CPU-percentage|
|physical_data_read_percent|Gegevens i/o-percentage|Procent|Gemiddelde|Gegevens i/o-percentage|
|log_write_percent|Logboek voor i/o-percentage|Procent|Gemiddelde|Logboek voor i/o-percentage|
|dtu_consumption_percent|Percentage DTU|Procent|Gemiddelde|Percentage DTU|
|opslag|Totale grootte|Bytes|Maximum|Totale grootte|
|connection_successful|Geslaagde verbindingen|Aantal|Totaal|Geslaagde verbindingen|
|connection_failed|Mislukte verbindingen|Aantal|Totaal|Mislukte verbindingen|
|blocked_by_firewall|Geblokkeerd door Firewall|Aantal|Totaal|Geblokkeerd door Firewall|
|deadlock|Impasses|Aantal|Totaal|Impasses|
|storage_percent|Percentage van database-formaat|Procent|Maximum|Percentage van database-formaat|
|xtp_storage_percent|In Memory OLTP opslag percent(Preview)|Procent|Gemiddelde|In Memory OLTP opslag percent(Preview)|
|workers_percent|Percentage van de werknemers|Procent|Gemiddelde|Procent van de werknemers|
|sessions_percent|Sessies %|Procent|Gemiddelde|Sessies %|
|dtu_limit|DTU-limiet|Aantal|Gemiddelde|DTU-limiet|
|dtu_used|DTU gebruikt|Aantal|Gemiddelde|DTU gebruikt|
|service_level_objective|Service level doel van de database|Aantal|Totaal|Service level doel van de database|
|dwu_limit|dwu beperken|Aantal|Maximum|dwu beperken|
|dwu_consumption_percent|Percentage van DWU|Procent|Gemiddelde|Percentage van DWU|
|dwu_used|DWU gebruikt|Aantal|Gemiddelde|DWU gebruikt|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|cpu_percent|CPU-percentage|Procent|Gemiddelde|CPU-percentage|
|physical_data_read_percent|Gegevens i/o-percentage|Procent|Gemiddelde|Gegevens i/o-percentage|
|log_write_percent|Logboek voor i/o-percentage|Procent|Gemiddelde|Logboek voor i/o-percentage|
|dtu_consumption_percent|Percentage DTU|Procent|Gemiddelde|Percentage DTU|
|storage_percent|Percentage van opslag|Procent|Gemiddelde|Percentage van opslag|
|workers_percent|Procent van de werknemers|Procent|Gemiddelde|Procent van de werknemers|
|sessions_percent|Sessies %|Procent|Gemiddelde|Sessies %|
|eDTU_limit|eDTU beperken|Aantal|Gemiddelde|eDTU beperken|
|storage_limit|Maximale opslagruimte|Bytes|Gemiddelde|Maximale opslagruimte|
|eDTU_used|eDTU gebruikt|Aantal|Gemiddelde|eDTU gebruikt|
|storage_used|Gebruikte opslagruimte|Bytes|Gemiddelde|Gebruikte opslagruimte|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|ResourceUtilization|SU percentage in gebruik|Procent|Maximum|SU percentage in gebruik|
|InputEvents|Invoer gebeurtenissen|Aantal|Totaal|Invoer gebeurtenissen|
|InputEventBytes|Bytes van invoer, gebeurtenis|Bytes|Totaal|Bytes van invoer, gebeurtenis|
|LateInputEvents|Latere invoer gebeurtenissen|Aantal|Totaal|Latere invoer gebeurtenissen|
|OutputEvents|Uitvoer-gebeurtenissen|Aantal|Totaal|Uitvoer-gebeurtenissen|
|ConversionErrors|Conversiefouten gegevens|Aantal|Totaal|Conversiefouten gegevens|
|Fouten|Runtime-fouten|Aantal|Totaal|Runtime-fouten|
|DroppedOrAdjustedEvents|Volgorde van gebeurtenissen|Aantal|Totaal|Volgorde van gebeurtenissen|
|AMLCalloutRequests|Functie aanvragen|Aantal|Totaal|Functie aanvragen|
|AMLCalloutFailedRequests|Functie van mislukte aanvragen|Aantal|Totaal|Functie van mislukte aanvragen|
|AMLCalloutInputEvents|Gebeurtenissen voor de functie|Aantal|Totaal|Gebeurtenissen voor de functie|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|CpuPercentage|CPU-Percentage|Procent|Gemiddelde|CPU-Percentage|
|MemoryPercentage|Geheugenpercentage|Procent|Gemiddelde|Geheugenpercentage|
|DiskQueueLength|Wachtrijlengte voor schijf|Aantal|Totaal|Wachtrijlengte voor schijf|
|HttpQueueLength|Lengte van wachtrij voor http|Aantal|Totaal|Lengte van wachtrij voor http|
|BytesReceived|Gegevens In|Bytes|Totaal|Gegevens In|
|BytesSent|Gegevens uit|Bytes|Totaal|Gegevens uit|

## <a name="microsoftwebsites"></a>Microsoft.Web/sites

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|CpuTime|CPU-tijd|Seconden|Totaal|CPU-tijd|
|Aanvragen|Aanvragen|Aantal|Totaal|Aanvragen|
|BytesReceived|Gegevens In|Bytes|Totaal|Gegevens In|
|BytesSent|Gegevens uit|Bytes|Totaal|Gegevens uit|
|Http2xx|HTTP-2xx|Aantal|Totaal|HTTP-2xx|
|Http3xx|HTTP-3xx|Aantal|Totaal|HTTP-3xx|
|Http401|HTTP 401|Aantal|Totaal|HTTP 401|
|Http403|HTTP-fout 403|Aantal|Totaal|HTTP-fout 403|
|Http404|HTTP-fout 404|Aantal|Totaal|HTTP-fout 404|
|Http406|HTTP 406|Aantal|Totaal|HTTP 406|
|Http4xx|HTTP-4xx|Aantal|Totaal|HTTP-4xx|
|Http5xx|De HTTP-Server-fouten|Aantal|Totaal|De HTTP-Server-fouten|
|MemoryWorkingSet|Geheugen-werkset|Bytes|Totaal|Geheugen-werkset|
|AverageMemoryWorkingSet|Gemiddelde geheugen werkset|Bytes|Gemiddelde|Gemiddelde geheugen werkset|
|AverageResponseTime|Gemiddelde reactietijd|Seconden|Gemiddelde|Gemiddelde reactietijd|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metric|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|CpuTime|CPU-tijd|Seconden|Totaal|CPU-tijd|
|Aanvragen|Aanvragen|Aantal|Totaal|Aanvragen|
|BytesReceived|Gegevens In|Bytes|Totaal|Gegevens In|
|BytesSent|Gegevens uit|Bytes|Totaal|Gegevens uit|
|Http2xx|HTTP-2xx|Aantal|Totaal|HTTP-2xx|
|Http3xx|HTTP-3xx|Aantal|Totaal|HTTP-3xx|
|Http401|HTTP 401|Aantal|Totaal|HTTP 401|
|Http403|HTTP-fout 403|Aantal|Totaal|HTTP-fout 403|
|Http404|HTTP-fout 404|Aantal|Totaal|HTTP-fout 404|
|Http406|HTTP 406|Aantal|Totaal|HTTP 406|
|Http4xx|HTTP-4xx|Aantal|Totaal|HTTP-4xx|
|Http5xx|De HTTP-Server-fouten|Aantal|Totaal|De HTTP-Server-fouten|
|MemoryWorkingSet|Geheugen-werkset|Bytes|Totaal|Geheugen-werkset|
|AverageMemoryWorkingSet|Gemiddelde geheugen werkset|Bytes|Gemiddelde|Gemiddelde geheugen werkset|
|AverageResponseTime|Gemiddelde reactietijd|Seconden|Gemiddelde|Gemiddelde reactietijd|

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over parameters in Azure Monitor](./monitoring-overview.md#monitoring-sources)
- [Waarschuwingen maken van gegevens](./insights-receive-alert-notifications.md)
- [Maatstaven voor opslag, gebeurtenis Hub of Analytics logboek exporteren](./monitoring-overview-of-diagnostic-logs.md)

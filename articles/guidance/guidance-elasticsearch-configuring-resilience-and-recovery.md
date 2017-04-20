<properties
   pageTitle="Veerkracht en herstel configureren op Elasticsearch op Azure"
   description="Overwegingen ten aanzien van tolerantie en herstel voor Elasticsearch."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>
   
# <a name="configuring-resilience-and-recovery-on-elasticsearch-on-azure"></a>Veerkracht en herstel configureren op Elasticsearch op Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dit artikel maakt [deel uit van een serie](guidance-elasticsearch.md). 

Een belangrijke functie van Elasticsearch is de ondersteuning die het biedt voor tolerantie in geval van storingen in knooppunten en/of partitie netwerkgebeurtenissen. Replicatie is de meest voor de hand liggende manier waarop u de tolerantie van een cluster verbeteren kunt, Elasticsearch om ervoor te zorgen dat meer dan één exemplaar van een item met gegevens beschikbaar is op andere knooppunten in het geval één knooppunt ontoegankelijk moet inschakelen. Als een knooppunt tijdelijk niet beschikbaar is wordt, kunnen andere knooppunten met replica's van gegevens uit het knooppunt ontbreekt de ontbrekende gegevens dienen totdat het probleem is opgelost. Bij een langere termijn probleem, het weggevallen knooppunt kan worden vervangen door een nieuw en Elasticsearch kunt terugzetten op het nieuwe knooppunt van de replica's.

We vatten hier de tolerantie en herstel opties met Elasticsearch wanneer in Azure gehost en beschrijven van enkele belangrijke aspecten van een cluster Elasticsearch waarmee u rekening houden moet aan het minimaliseren van de kans op verlies van gegevens en uitgebreide data recovery tijden.

Dit artikel illustreert ook sommige monster tests die zijn uitgevoerd om de effecten van verschillende soorten fouten op een cluster Elasticsearch en hoe het systeem reageert als ze realiseert weer te geven.

Replica's een Elasticsearch cluster gebruikt voor het onderhouden van beschikbaarheid en betere leesprestaties. Replica's moeten worden opgeslagen op verschillende VMs uit de primaire shards die ze repliceren. De bedoeling is dat als het hosten van een gegevensknooppunt VM mislukt of niet beschikbaar is, het systeem kan blijven functioneren met behulp van de VMs houden de replica's.

## <a name="using-dedicated-master-nodes"></a>Met behulp van speciale master knooppunten

Een knooppunt in een cluster Elasticsearch is gekozen als de master node. Het doel van dit knooppunt is cluster beheer van bewerkingen, zoals uit te voeren:

- Opsporen van storingen in knooppunten en op overschakelen naar een andere replica's.

- Shards naar knooppunt werklast evenwichtig te verplaatsen.

- Shards herstellen wanneer een knooppunt weer on line wordt gebracht.

U moet overwegen om specifieke basispagina knooppunten in belangrijke clusters en zorgen ervoor dat er 3 specifieke knooppunten waarvan alleen de rol van master wordt. Deze configuratie wordt minder intensief werk van de resource waarvoor deze knooppunten uit te voeren (ze geen gegevens opslaan en verwerken van query's) en helpt bij het verbeteren van de clusterstabiliteit. Slechts een van deze knooppunten worden gekozen, maar de andere bevat een kopie van de systeemstatus en kunnen via het gekozen model niet.

## <a name="controlling-high-availability-with-azure--update-domains-and-fault-domains"></a>Hoge beschikbaarheid met Azure – update domeinen en fouttolerantie domeinen beheren 

Verschillende VMs kunnen delen dezelfde fysieke hardware. In een datacenter Azure, kan één rack een aantal VMs, en al deze VMs delen een gemeenschappelijke bron- en stopcontact. Een storing niveau rack kan daarom invloed hebben een aantal VMs. Het concept van domeinen veroorzaakt Azure gebruikt om te proberen en verspreid dit risico. Een fout met betrekking tot domein komt ongeveer overeen met een groep van VMs die hetzelfde rack delen. Om ervoor te zorgen dat een storing rack-niveau een knooppunt niet vast wordt en de knooppunten met alle bijbehorende replica's tegelijk, u ervoor zorgen moet dat de VMs zijn verdeeld in verschillende domeinen veroorzaakt.

Op dezelfde manier kunnen VMs worden genomen naar beneden door de [Azure Fabric-Controller](https://azure.microsoft.com/documentation/videos/fabric-controller-internals-building-and-updating-high-availability-apps/) voor het geplande onderhoud en het besturingssysteem upgrades uitvoeren. Azure wijst VMs domeinen bijwerken. Als een gebeurtenis gepland onderhoud, worden alleen VMs in een domein één update op elk gewenst moment plaatsvinden. VMs in andere domeinen update blijven actief totdat de VMs in de update domein wordt bijgewerkt opnieuw on line worden gebracht. Daarom moet u ook om ervoor te zorgen dat VMs hosten van knooppunten en hun replica's deel uitmaken van domeinen in andere update zoveel mogelijk.

> [AZURE.NOTE] Zie voor meer informatie over de fout met betrekking tot domeinen en domeinen bijwerken, [beheren van de beschikbaarheid van virtuele machines][].

U kan een VM tot een specifieke update domein en het domein voor fouttolerantie niet expliciet toewijzen. Deze verdeling wordt bepaald door de Azure wanneer VMs worden gemaakt. U kunt opgeven dat VMs moeten worden gemaakt als onderdeel van een set van beschikbaarheid. VMs in bepaalde beschikbaarheid worden verdeeld over de update domeinen en domeinen met fouttolerantie. Als u handmatig VMs maakt, maakt de Azure elke set met twee fout met betrekking tot domeinen en domeinen met vijf update beschikbaar. VMs worden toegewezen aan deze fout met betrekking tot domeinen en domeinen, ronde functioneert als verdere VMs zijn ingericht bijwerken: 

| VM | Fout met betrekking tot domein | Domein bijwerken |
|----|--------------|---------------|
|  1 |            0 |             0 |
|  2 |            1 |             1 |
|  3 |            0 |             2 |
|  4 |            1 |             3 |
|  5 |            0 |             4 |
|  6 |            1 |             0 |
|  7 |            0 |             1 |

> [AZURE.IMPORTANT] Als u met bronbeheer Azure VMs maakt, kan elke set beschikbaarheid maximaal 3 fout met betrekking tot domeinen en domeinen 20 update worden toegewezen. Dit is een goede reden voor het gebruik van de Resource Manager.

In het algemeen plaatst alle VMs die hetzelfde doel in bepaalde beschikbaarheid dienen, maar de beschikbaarheid van de verschillende sets maken voor VMs die verschillende functies uitvoeren. Met dit betekent dat u kunt overwegen Elasticsearch ten minste afzonderlijk beschikbaar maken wordt ingesteld voor:

- VMs gegevensknooppunten die als host fungeert.
- VMs hosting client knooppunten (als u deze gebruikt).
- VMs master knooppunten die als host fungeert.

Bovendien moet u ervoor zorgen dat elk knooppunt in een cluster is zich bewust van het domein van de update en de fouttolerantie domein waartoe het behoort. Deze informatie kan helpen om ervoor te zorgen dat Elasticsearch niet in de fout met betrekking tot dezelfde shards en hun replica's maken en bijwerken van domeinen, minimaliseren de mogelijkheid van een shard en de replica's tegelijk omlaag worden gebracht. U kunt een knooppunt Elasticsearch vanuit de verdeling van de hardware van het cluster door [shard toewijzing bewustzijn](https://www.elastic.co/guide/en/elasticsearch/reference/current/allocation-awareness.html#allocation-awareness)configureren. U kunt bijvoorbeeld een paar aangepaste knooppunt kenmerken de naam *faultDomain* en *updateDomain* in het bestand elasticsearch.yml als volgt definiëren:

```yaml
node.faultDomain: \${FAULTDOMAIN}
node.updateDomain: \${UPDATEDOMAIN}
```

In dit geval, de kenmerken zijn ingesteld met behulp van de waarden die de * \${FAULTDOMAIN}* en * \${UPDATEDOMAIN}* omgevingsvariabelen wanneer Elasticsearch wordt gestart. Ook moet u de volgende vermeldingen toevoegen aan het bestand Elasticsearch.yml om aan te geven, *faultDomain* en *updateDomain* toewijzing bewustzijn kenmerken zijn en de sets van geldige waarden voor deze kenmerken opgeven:

```yaml
cluster.routing.allocation.awareness.force.updateDomain.values: 0,1,2,3,4
cluster.routing.allocation.awareness.force.faultDomain.values: 0,1
cluster.routing.allocation.awareness.attributes: updateDomain, faultDomain
```

Kunt u shard toewijzing bewustzijn in combinatie met de [shard toewijzing filter](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/shard-allocation-filtering.html#shard-allocation-filtering) expliciet opgeven welke knooppunten kunnen hosten shards voor de opgegeven index.

Als u schalen dan het nummer van de fout met betrekking tot domeinen en domeinen in een set beschikbaarheid update wilt, kunt u VMs in sets van extra beschikbaarheid. U moet echter begrijpen dat knooppunten in de beschikbaarheid van verschillende sets kunnen worden genomen omlaag voor onderhoud tegelijk. Probeer om ervoor te zorgen dat elke shard en ten minste één van de replica's in dezelfde beschikbaarheid reeks bevat.

> [AZURE.NOTE] Er is een limiet van 100 VMs per set beschikbaarheid. Zie voor meer informatie [Azure abonnement service grenzen, quota's en beperkingen](../azure-subscription-service-limits.md).

### <a name="backup-and-restore"></a>Back-up en terugzetten

Met behulp van replica's biedt geen volledige bescherming tegen een onherstelbare fout (zoals het per ongeluk verwijderen van het hele cluster). U moet ervoor zorgen dat u regelmatig een back-up van de gegevens in een cluster en u hebt een geprobeerd en getest strategie voor het terugzetten van het systeem van deze back-ups.

Gebruik Elasticsearch [momentopname maken en terugzetten van API's](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html) : elastische niet cap deze. >> back-up maken en terugzetten van indexen. Momentopnamen kunnen worden opgeslagen op een gedeelde bestandssysteem. U kunt ook Plug-ins beschikbaar zijn die momentopnamen kunt schrijven op het Hadoop distributed file system (HDFS) ( [HDFS-invoegtoepassing](https://github.com/elasticsearch/elasticsearch-hadoop/tree/master/repository-hdfs)) of op Azure opslag ( [Azure plugin](https://github.com/elasticsearch/elasticsearch-cloud-azure#azure-repository)).

Wanneer de momentopname opslagmethode te selecteren, moet u rekening houden met de volgende punten:

- [Azure opslag](https://azure.microsoft.com/services/storage/files/) kunt u een gedeelde bestandssysteem die toegankelijk is vanaf alle knooppunten te implementeren.

- De invoegtoepassing HDFS alleen gebruiken als u Elasticsearch in combinatie met Hadoop.

- De invoegtoepassing HDFS moet u uitschakelen Java Security Manager uitgevoerd binnen het Elasticsearch exemplaar van de Java virtual machine (JVM).

- De invoegtoepassing HDFS ondersteunt elk bestandssysteem dat compatibel is met HDFS op voorwaarde dat de juiste configuratie van Hadoop met Elasticsearch wordt gebruikt.

  
## <a name="handling-intermittent-connectivity-between-nodes"></a>Behandeling met tussenpozen verbinding tussen knooppunten

Netwerk af en toe storingen, VM opnieuw wordt opgestart nadat het dagelijks onderhoud op het datacenter en andere soortgelijke gebeurtenissen veroorzaken knooppunten tijdelijk ontoegankelijk worden. In deze situaties, waarbij de gebeurtenis waarschijnlijk korte levensduur is, de overhead van de shards opnieuw tweemaal voorkomt in snel achter elkaar (eenmaal als de fout is gedetecteerd en opnieuw wanneer het knooppunt worden weergegeven aan de kapitein) kan resulteren in een aanzienlijke overhead die van invloed op prestaties. Kunt u voorkomen dat inaccessibility tijdelijke knooppunt veroorzaakt door het model om het cluster opnieuw door de *vertraagd\_-time-out* -eigenschap van een index, of voor alle indexen. In het volgende voorbeeld wordt de vertraging ingesteld op 5 minuten:

```http
PUT /_all/settings
{
    "settings": {
    "index.unassigned.node_left.delayed_timeout": "5m"
    }
}
```

Zie [toewijzing uitstellen wanneer een knooppunt](https://www.elastic.co/guide/en/elasticsearch/reference/current/delayed-allocation.html)voor meer informatie.

U kunt ook de parameters die een model te detecteren wanneer een ander knooppunt niet langer toegankelijk is configureren in een netwerk die gevoelig is voor onderbrekingen, wijzigen. Deze parameters zijn onderdeel van de [zen discovery](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-discovery-zen.html#modules-discovery-zen) module Elasticsearch voorzien en kunt u deze instellen in het bestand Elasticsearch.yml. Bijvoorbeeld, is de parameter *discovery.zen.fd.ping.retries* geeft aan hoe vaak een hoofdknooppunt zal proberen te pingen van een ander knooppunt in het cluster voordat u besluit dat deze is mislukt. Deze parameter standaard ingesteld op 3, maar kunt u deze als volgt wijzigen:

```yaml
discovery.zen.fd.ping_retries: 6
```

## <a name="controlling-recovery"></a>Herstel beheren

Wanneer verbinding met een knooppunt na een storing wordt hersteld, moet elke shards op dat knooppunt worden hersteld om ze bij te werken. Standaard realiseert Elasticsearch shards in de volgende volgorde:

- Op aanmaakdatum omgekeerde index. Nieuwe indexen zijn hersteld voordat oudere indexen.

- Door de naam van de omgekeerde index. Indexen met namen die alfanumeriek groter dan anderen zijn worden eerst hersteld.

Als enkele indexen belangrijker dan anderen zijn, maar die niet overeenkomen met deze criteria kunt u de prioriteit van indexen wijzigen door de eigenschap *index.priority* . Indexen met een hogere waarde voor deze eigenschap wordt hersteld voordat u indexen die een lagere waarde hebben:

```http
PUT low_priority_index
{
    "settings": {
        "index.priority": 1
    }
}

PUT high_priority_index
{
    "settings": {
        "index.priority": 10
    }
}
```

Zie [Index herstel prioriteitstelling](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/recovery-prioritization.html#recovery-prioritization)voor meer informatie.

U kunt controleren het herstelproces voor een of meer indexen met behulp van de * \_herstel* API:

```http
GET /high_priority_index/_recovery?pretty=true
```

Zie [Indexen herstel](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-recovery.html#indices-recovery)voor meer informatie.

> [AZURE.NOTE] Een cluster met shards die moeten worden hersteld wordt de status van *geel* om aan te geven dat niet alle shards momenteel beschikbaar zijn hebben. Wanneer alle shards beschikbaar zijn, wordt de status van het cluster moet terugkeren naar *groen*. Een cluster met een *rode* status geeft aan dat een of meer shards fysiek ontbreken, kan het nodig zijn om gegevens te herstellen van een back-up zijn.

## <a name="preventing-split-brain"></a>Gesplitste hersenen voorkomen 

De hersenen van een splitsing kan optreden als de verbindingen tussen de knooppunten niet. Als een master knooppunt naar een deel van het cluster onbereikbaar wordt, een verkiezing zal plaatsvinden in het netwerksegment dat beschikbaar blijft en het model zal worden door een ander knooppunt. In een onjuist geconfigureerd cluster is het mogelijk voor elk deel van het cluster hebben verschillende modellen waardoor inconsistenties of is beschadigd. Dit verschijnsel staat bekend als een *gesplitste hersenen*.

Kunt u de kans op een splitsing hersenen verkleinen door het configureren van de *ten minste\_model\_knooppunten* eigenschap van de module discovery in het bestand elasticsearch.yml. Deze eigenschap geeft aan hoeveel knooppunten er beschikbaar moeten zijn voor de verkiezing van een model inschakelen. In het volgende voorbeeld wordt de waarde van deze eigenschap ingesteld op 2:

```yaml
discovery.zen.minimum_master_nodes: 2
```

Deze waarde moet worden ingesteld op de laagste meerderheid van het aantal knooppunten die kunnen voldoen aan de rol. Als het cluster heeft 3 master knooppunten, bijvoorbeeld *minimum\_model\_knooppunten* moet worden ingesteld op 2. Als u beschikt over 5 master knooppunten, *minimum\_model\_knooppunten* moet worden ingesteld op 3. In het ideale geval moet er een oneven aantal knooppunten master.

> [AZURE.NOTE] Het is mogelijk voor een gesplitste hersenen optreden als meerdere basispagina knooppunten in hetzelfde cluster tegelijkertijd worden gestart. Dit exemplaar is zeldzaam, kunt u voorkomen dat deze door de knooppunten serie begint met een korte vertraging (5 seconden) tussen elk. 

## <a name="handling-rolling-updates"></a>Rolling updates verwerken

Als u een software-upgrade met knooppunten zelf (zoals migreren naar een nieuwere versie of een patch uitvoert), moet u mogelijk werk uitvoeren op de afzonderlijke knooppunten waarvoor ze offline te nemen terwijl de rest van het cluster beschikbaar blijft. Overweeg de implementatie van de volgende procedure in deze situatie. 

1. Zorg ervoor dat shard Hertoewijzing genoeg wordt uitgesteld om te voorkomen dat het gekozen model shards vanaf een knooppunt ontbreekt in de rest van het cluster opnieuw. Hertoewijzing shard gedurende 1 minuut is vertraagd, maar kunt u de duur als een knooppunt is niet beschikbaar voor een langere periode kunnen verhogen. In het volgende voorbeeld wordt de vertraging op 5 minuten verhoogd:

    ```http
    PUT /_all/_settings
    {
        "settings": {
            "index.unassigned.node_left.delayed_timeout": "5m"
        }
    }
    ```

    > [AZURE.IMPORTANT] U kunt ook shard Hertoewijzing volledig uitschakelen door het *cluster.routing.allocation.enable* van het cluster in te stellen op *geen*. U moet echter niet met deze aanpak als nieuwe indexen waarschijnlijk worden gemaakt zijn, terwijl het knooppunt off line is, omdat dit leiden indextoewijzing tot kan niet resulteert in een cluster met de status is rood.

2. Stop Elasticsearch op het knooppunt dat moet worden gehandhaafd. Als Elasticsearch wordt uitgevoerd als een service, kunt u mogelijk het proces stoppen op een gecontroleerde manier met behulp van de opdracht van een besturingssysteem. In het volgende voorbeeld ziet u hoe de Elasticsearch-service op één knooppunt op Ubuntu stoppen:

    ```bash
    service elasticsearch stop
    ```

    Ook kunt u de API afsluiten rechtstreeks op het knooppunt:

    ```http
    POST /_cluster/nodes/_local/_shutdown
    ```

3. Het noodzakelijke onderhoud uitvoeren op het knooppunt

4. Start het knooppunt opnieuw en wachten op het deelnemen aan het cluster.

5. Shard toewijzing opnieuw in te schakelen:

    ```http
    PUT /_cluster/settings
    {
        "transient": {
            "cluster.routing.allocation.enable": "all"
        }
    }
    ```

> [AZURE.NOTE] Als u meer dan één knooppunt onderhouden, herhaalt u stap 2&ndash;4 op elk knooppunt voordat u shard toewijzing opnieuw inschakelt.

Als u kunt stoppen met het indexeren van nieuwe gegevens tijdens dit proces. Hierdoor kunt u hersteltijden wanneer de knooppunten opnieuw on line worden gebracht en het cluster opnieuw lid worden.

Pas op voor automatische updates om items als de JVM (in het ideale geval uitschakelen automatische updates voor deze artikelen), met name wanneer uitgevoerd Elasticsearch onder Windows. De Java update agent automatisch de meest recente versie van Java kunt downloaden, maar mogelijk Elasticsearch te worden gestart om de update te activeren. Dit kan leiden tot een ongecoördineerde tijdelijk verlies van knooppunten, afhankelijk van hoe de Java Update agent is geconfigureerd. Dit kan ook leiden tot verschillende exemplaren van Elasticsearch in hetzelfde cluster met verschillende versies van de JVM die kunnen compatibiliteitsproblemen veroorzaken.

## <a name="testing-and-analyzing-elasticsearch-resilience-and-recovery"></a>Testen en analyse van de schokbestendigheid Elasticsearch en herstel

Deze sectie beschrijft een serie tests die zijn uitgevoerd voor het evalueren van de veerkracht en het herstel van een Elasticsearch cluster met drie gegevensknooppunten en drie master knooppunten.

De volgende scenario's zijn getest: 

- Storingen in knooppunten en opnieuw starten zonder verlies van gegevens. Een gegevensknooppunt wordt gestopt en opnieuw gestart na 5 minuten. Elasticsearch is geconfigureerd niet opnieuw toewijzen van ontbrekende shards in dit interval, zodat er geen extra I/O in shards navigeren is ontstaan. Als het knooppunt opnieuw is opgestart, wordt het herstelproces de shards op dat knooppunt weer up-to-date.

- Storingen in knooppunten met verlies van kritieke gegevens. Een gegevensknooppunt is gestopt en de gegevens die deze bevat om te simuleren onherstelbare schijffout is gewist. Het knooppunt wordt vervolgens opnieuw opgestart (na 5 minuten), effectief fungeert als een vervanging voor het oorspronkelijke knooppunt. Het herstelproces moet opnieuw samenstellen van de ontbrekende gegevens voor dit knooppunt en verhuizen shards gehouden op andere knooppunten kan inhouden.

- Storingen in knooppunten en opnieuw starten zonder verlies van gegevens, maar met de shard Hertoewijzing. Een gegevensknooppunt is gestopt en de shards die deze bevat naar andere knooppunten worden herverdeeld. Het knooppunt vervolgens opnieuw wordt gestart en meer Hertoewijzing treedt op als het cluster opnieuw.

- Huidige updates. Elk knooppunt in het cluster wordt gestopt en opnieuw gestart na een korte interval voor het simuleren van machines opnieuw worden opgestart nadat een software-update. Slechts één knooppunt is tegelijk gestopt. Shards worden niet herverdeeld als een knooppunt niet actief is.

Elk scenario is onderworpen aan de dezelfde werklast een combinatie van gegevens via ingestie taken, aggregaties en filterquery's inclusief bij knooppunten werden genomen offline en herstelde. De meeste bewerkingen in de belasting van elk 1000 documenten opgeslagen en zijn uitgevoerd op één index tijdens de aggregaties invoegen en filter query's gebruikt in een afzonderlijke index met meerdere miljoenen documenten. Dit was om de prestaties van query's afzonderlijk beoordeeld worden vanuit de bulk invoegt. Elke index zich vijf shards en een replica.

In de volgende secties worden de resultaten van deze tests, een afname van prestaties opgetekend, terwijl een knooppunt off line is samengevat of wordt hersteld en eventuele fouten die werden gerapporteerd. De resultaten worden weergegeven als geïllustreerd, markeren van de punten die een of meer knooppunten zijn ontbreekt en schatting van de tijd die nodig is voor het systeem volledig herstellen en bereiken een soortgelijk niveau van prestaties die aanwezig is voor de knooppunten die off line wordt genomen.

> [AZURE.NOTE] De test tuigen voor het uitvoeren van deze tests zijn online beschikbaar. U kunt aanpassen en deze tuigen gebruiken om te controleren of de veerkracht en herstelbaarheid van uw eigen configuraties van clusters. Voor meer informatie, Zie [de testcriteria Elasticsearch tolerantie wordt uitgevoerd][].

## <a name="node-failure-and-restart-with-no-data-loss-results"></a>Storingen in knooppunten en opnieuw opstarten zonder verlies van gegevens: resultaten

<!-- TODO; reformat this pdf for display inline --> 

De resultaten van deze test worden weergegeven in het bestand [ElasticsearchRecoveryScenario1.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario1.pdf). De grafieken weergeven profiel van de prestaties van de werkbelasting en fysieke bronnen voor elk knooppunt in het cluster. Het eerste deel van de grafieken tonen het systeem normaal voor ongeveer 20 minuten, waarna knooppunt 0 is afgesloten voor 5 minuten voordat opnieuw wordt gestart. De statistieken nog 20 minuten worden geïllustreerd; het systeem duurt ongeveer 10 minuten om te herstellen en stabiliseren. Dit wordt geïllustreerd door de transactie- en responstijden voor verschillende werkbelastingen.

Houd rekening met de volgende punten:

- Tijdens de test zijn geen fouten gemeld. Er zijn geen gegevens verloren gegaan en alle bewerkingen voltooid.

- De transactie tarieven voor alle drie de typen (bulk insert samenvoegquery en filterquery) verwijderd en de gemiddelde responstijden gestegen terwijl 0 knooppunt off line is.

- Bewerkingen zijn tijdens de periode van herstel, de transactie tarieven en responstijden voor de statistische query en filterquery geleidelijk hersteld. De prestaties voor bulk insert hersteld voor een korte terwijl voordat verminderd. Echter dit komt waarschijnlijk door het volume van de gegevens die worden veroorzaakt door de index die door het massaal invoegen gebruikt om te groeien en de tarieven van de transactie voor deze bewerking ziet te verlagen voordat 0 knooppunt off line wordt genomen.

- De grafiek van de CPU-gebruik voor knooppunt 0 geeft verminderde activiteit tijdens de herstelfase dit komt door de hogere schijf en netwerkactiviteit veroorzaakt door de herstelfunctie, het knooppunt heeft is gemist offline gegevens achterhalen en bijwerken van de shards die het bevat.

- De shards voor de indexen worden niet gedistribueerd precies gelijk zijn op alle knooppunten. Er zijn twee indexen met 5 shards en 1 replica elk, dus voor een totaalbedrag van 20 shards. Twee knooppunten bevat daarom 6 shards terwijl de andere twee elke 7 houdt. Dit is duidelijk in de grafieken van CPU-gebruik tijdens de eerste 20 minuten durende periode, knooppunt 0 is minder intensief dan de andere twee. Nadat het terugzetten is voltooid, lijkt sommige wisselen op te treden als knooppunt 2 lijkt het meer licht geladen knooppunt.

    
## <a name="node-failure-with-catastrophic-data-loss-results"></a>Storingen in knooppunten met verlies van kritieke gegevens: resultaten

<!-- TODO; reformat this pdf for display inline -->

De resultaten van deze test worden weergegeven in het bestand [ElasticsearchRecoveryScenario2.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario2.pdf). Zoals het eerste deel van de grafieken met de eerste test het systeem normaal gesproken gedurende ongeveer 20 minuten zien, bij welk knooppunt punt wordt 0 afgesloten voor 5 minuten. Tijdens dit interval, de Elasticsearch gegevens op dit knooppunt verwijderd, verlies van kritieke gegevens simuleren voordat opnieuw wordt gestart. Volledig herstel wordt 12 tot 15 minuten duren voordat de prestatieniveaus gezien vóór de proef worden hersteld.

Houd rekening met de volgende punten:

- Tijdens de test zijn geen fouten gemeld. Er zijn geen gegevens verloren gegaan en alle bewerkingen voltooid.

- De transactie tarieven voor alle drie de typen (bulk insert samenvoegquery en filterquery) verwijderd en de gemiddelde responstijden gestegen terwijl 0 knooppunt off line is. Op dit moment is het profiel van de prestaties van de test vergelijkbaar met het eerste scenario. Dit is niet verrassend, op dit punt, de scenario's zijn hetzelfde.

- Tijdens de periode van herstel, zijn de transactie- en reactietijden teruggezet, hoewel gedurende deze tijd er veel meer volatiliteit in de cijfers is. Dit komt waarschijnlijk door het extra werk dat de knooppunten in het cluster uitvoeren wilt, de gegevens te herstellen van de ontbrekende shards. Dit extra werk is duidelijk bij het gebruik van CPU, schijfactiviteit en netwerk activiteit grafieken.

- De grafiek van de CPU-gebruik voor knooppunten 0 en 1 geeft verminderde activiteit tijdens de herstelfase, dat dit komt door de toegenomen schijf- en netwerkactiviteit veroorzaakt door het herstelproces. In het eerste scenario, alleen het knooppunt wordt hersteld dit probleem optrad, maar in dit geval lijkt het waarschijnlijk dat het grootste deel van de ontbrekende gegevens voor knooppunt 0 worden teruggezet vanaf knooppunt 1.

- De i/o-activiteit voor knooppunt 0 daadwerkelijk verminderd in vergelijking met het eerste scenario. Dit kan het gevolg zijn van de i/o-efficiëntie van de gegevens voor een volledige shard in plaats van de reeks kleinere i/o-aanvragen nodig is om een bestaande shard up-to-date te kopiëren.

- De activiteit op het netwerk voor alle drie knooppunten aangegeven bursts van de activiteit van gegevens is verzonden en ontvangen tussen knooppunten. In scenario 1 leek alleen knooppunt 0 optrad als veel activiteit op het netwerk, maar deze activiteit duurzame voor een langere periode. Nogmaals, dit verschil kan het gevolg zijn van de efficiëntie van de volledige gegevens voor een shard verzenden als een enkele aanvraag in plaats van de reeks kleinere aanvragen ontvangen bij het herstellen van een shard.

## <a name="node-failure-and-restart-with-shard-reallocation-results"></a>Storingen in knooppunten en opnieuw opstarten met de shard Hertoewijzing: resultaten

<!-- TODO; reformat this pdf for display inline -->

Het bestand [ElasticsearchRecoveryScenario3.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario3.pdf) ziet u de resultaten van deze test. Als het eerste deel van de grafieken weergeven met de eerste test het systeem normaal gesproken gedurende ongeveer 20 minuten, bij welk knooppunt punt wordt 0 afgesloten voor 5 minuten. Op dit moment geprobeerd het cluster Elasticsearch opnieuw maken van de ontbrekende shards en de shards vastleggen op de resterende knooppunten. Na 5 minuten knooppunt 0 weer on line wordt gebracht en weer het cluster heeft de shards opnieuw uit te. Prestaties wordt hersteld na 12-15 minuten.

Houd rekening met de volgende punten:

- Tijdens de test zijn geen fouten gemeld. Er zijn geen gegevens verloren gegaan en alle bewerkingen voltooid.

- De tarieven van de transactie voor drie soorten bewerking (bulk insert samenvoegquery en filterquery) genegeerd en wordt de gemiddelde responstijden aanzienlijk gestegen terwijl 0 knooppunt off line in vergelijking met de vorige twee proeven was. Dit komt door de activiteit verhoogd cluster opnieuw maken van de ontbrekende shards en het cluster opnieuw aangetoond door de verhoogde cijfers voor de schijf en netwerk activiteiten voor knooppunt 1 en 2 in deze periode.

- Tijdens de periode nadat 0 knooppunt weer on line, blijven de transactie- en responstijden volatiel.

- Het CPU-gebruik en de schijf activiteit grafieken voor knooppunt 0 geeft zeer beperkte eerste actie tijdens de herstelfase. Dit is omdat op dit moment knooppunt 0 geen gegevens levert. Na een periode van ongeveer 5 minuten, het knooppunt barst in actie < RBC: dit me hardop snort aangebracht. Ik ben niet afkomstig van een betere manier om te zeggen dit al.  >> zoals getoond door de plotselinge toename van netwerk, schijf en CPU-activiteit. Dit wordt waarschijnlijk veroorzaakt door het cluster shards verdelen over knooppunten. Normale activiteit wordt op knooppunt 0 weergegeven.
  
## <a name="rolling-updates-results"></a>Rolling updates: resultaten

<!-- TODO; reformat this pdf for display inline -->

De resultaten van deze test, in het bestand [ElasticsearchRecoveryScenario4.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario4.pdf), blijkt hoe elk knooppunt is off line genomen en vervolgens weer teruggebracht tot opnieuw achter elkaar. Elk knooppunt is afgesloten voor 5 minuten voordat opnieuw wordt gestart, waarna het volgende knooppunt in de reeks is gestopt.

Houd rekening met de volgende punten:

- Terwijl elk knooppunt gaat, blijft de prestaties in termen van doorvoer en reactie tijden redelijkerwijs zelfs.

- Activiteit van de schijf wordt verhoogd voor elk knooppunt voor korte tijd bij het weer on line. Dit komt waarschijnlijk door het herstelproces rolling forward alle wijzigingen die zijn opgetreden terwijl het knooppunt ingedrukt is.

- Wanneer een knooppunt is off line genomen, pieken in activiteit op het netwerk optreden in de resterende knooppunten. Pieken ook optreden wanneer een knooppunt opnieuw wordt opgestart.

- Nadat het laatste knooppunt herhaald wordt, voert het systeem een periode van aanzienlijke volatiliteit. Dit is waarschijnlijk veroorzaakt door het herstelproces met wijzigingen op elk knooppunt te synchroniseren en ervoor zorgen dat alle replica's en hun bijbehorende shards consistent zijn. Op één punt, deze inspanning wordt opeenvolgende bulk bewerkingen time-out invoegen en mislukt. De fouten gemeld in elk geval zijn:

```
Failure -- BulkDataInsertTest17(org.apache.jmeter.protocol.java.sampler.JUnitSampler$AnnotatedTestCase): java.lang.AssertionError: failure in bulk execution:
[1]: index [systwo], type [logs], id [AVEg0JwjRKxX_sVoNrte], message [UnavailableShardsException[[systwo][2] Primary shard is not active or isn't assigned to a known node. Timeout: [1m], request: org.elasticsearch.action.bulk.BulkShardRequest@787cc3cd]]

```

Latere experimenten bleek dat invoering van een vertraging van enkele minuten tussen elk knooppunt functioneert deze fout weggenomen zodat het meest waarschijnlijk is veroorzaakt door een conflict tussen het herstelproces te herstellen meerdere knooppunten tegelijkertijd en de meeste bewerkingen duizenden nieuwe documenten wilt invoegen.


## <a name="summary"></a>Samenvatting

De tests die uitgevoerd aangegeven dat:

- Elasticsearch is uiterst robuuste aan de meest voorkomende storingen die kunnen optreden in een cluster.

- Elasticsearch kunt snel herstellen als er een goed ontworpen cluster kan verlies van kritieke gegevens op een knooppunt. Dit kan gebeuren als u configureert Elasticsearch als gegevens wilt opslaan naar een tijdelijke opslag en het knooppunt wordt vervolgens door de service na een herstart. Deze resultaten blijkt dat zelfs in dat geval de risico's van het gebruik van tijdelijke opslag zijn waarschijnlijk overtroffen door de voordelen die deze klasse opslagruimte biedt.

- In de eerste drie scenario's zonder fouten opgetreden in gelijktijdige massaal invoegen, samenvoeging en filter query werklasten een knooppunt is off line en herstelde genomen.

- Alleen het laatste scenario potentieel gegevensverlies vermeld en wordt dit verlies alleen invloed op nieuwe gegevens worden toegevoegd. Het is een goede gewoonte in toepassingen uitvoeren via ingestie gegevens dit risico beperken door insert-bewerkingen die zijn mislukt als het type fout gerapporteerd is zeer waarschijnlijk tijdelijk opnieuw.

- De resultaten van de laatste test wordt ook weergegeven dat als u een gepland onderhoud van de knooppunten in een cluster uitvoert, prestaties voordelen als u enkele minuten tussen een knooppunt en de volgende functioneert. In een ongeplande situatie (zoals het datacenter knooppunten recycling na het uitvoeren van een update van het besturingssysteem), hebt u minder controle over hoe en wanneer de knooppunten zijn uitgeschakeld en opnieuw gestart. De bewering dat ontstaat als Elasticsearch probeert te herstellen van de status van het cluster na opeenvolgende knooppunt storingen kan leiden tot time-outs en fouten. 

[De beschikbaarheid van virtuele Machines te beheren]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md
[De tolerantie voor geautomatiseerde Elasticsearch Tests uitgevoerd]: guidance-elasticsearch-running-automated-resilience-tests.md

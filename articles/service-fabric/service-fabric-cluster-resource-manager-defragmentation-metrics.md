<properties
   pageTitle="Defragmentatie van statistieken in Azure Service Fabric | Microsoft Azure"
   description="Een overzicht van het gebruik van defragmentatie of verpakken als een strategie voor metrieken Service stof"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Defragmentatie van maatstaven en laden in Service Fabric
De Service Fabric Cluster Resource Manager heeft voornamelijk betrekking netwerktaakverdeling voor het verdelen van de belasting – ervoor te zorgen dat alle knooppunten in het cluster even gebruikt. Dit is meestal de veiligste en slimste lay-out van de storingen die leven blijven omdat deze ervoor zorgt dat bepaalde overtredingen wordt pas uit een groot percentage van een bepaalde werkbelasting. Service Fabric Cluster Resource Manager ondersteunt ook een andere strategie is defragmentatie. Defragmentatie in het algemeen betekent dat in plaats van het gebruik van een metriek verdelen over het cluster wilt, we daadwerkelijk proberen moet te consolideren van het. Dit is een fortunate omkering van onze normale strategie – in plaats van het optimaliseren van het cluster op basis van de gemiddelde standaardafwijking van metrische werklast voor een bepaalde metriek te minimaliseren, we beginnen te optimaliseren voor verhogingen van de afwijking. Maar waarom zou u dat deze strategie?

Goed, als u hebt de belasting gelijkmatig verdeeld tussen de knooppunten in het cluster hebt klikt u gegeten van enkele van de middelen die de knooppunten bieden. Normaal dit geen probleem, maar soms sommige werklasten maken die uitzonderlijk groot en verbruiken de overgrote meerderheid van de knooppunt een – zeg 75% tot 95% van de middelen van het knooppunt zou uiteindelijk bestemd voor een enkel exemplaar of de replica. Dit geen probleem, het Cluster Resource Manager wordt gedetecteerd bij het service maken die nodig zijn voor het indelen van het cluster om ruimte maken voor grote werklast en stel over zaken, maar in de tussentijd de werkbelasting die heeft te wachten om te worden gepland in het cluster.

Gezien het feit dat de planning van nieuwe werkbelasting meestal ten minste een beetje latentie gevoelige, is als we niet iets anders doen kunnen wij soms rechts door de SLA's bLaag als er een groot aantal services en staat om te navigeren, vooral als de werklast in het cluster zijn meestal grote (en dus het duurt langer om te navigeren in het cluster). Inderdaad, wanneer we keren in op basis van echte clustergegevens simulaties maken, gemeten hebt gezien die als services groot genoeg zijn en het cluster is tamelijk gebruikt bij het maken van deze grote diensten zou worden vertraagd, en dat we dit kunnen verbeteren door de invoering van het beleid van defragmentatie metrics.

Net als een bestand kan maken of toegang krijgen vertraagd dat als een vaste schijf is gefragmenteerd en kan worden sped up door het defragmenteren van de schijf zodat er grotere aaneengesloten blokken beschikbaar zijn, kunt u configureren defragmentatierapporten maatstaven voor het Cluster Resource Manager om proactief te versmallen het laden van de diensten in minder knooppunten dat er (bijna) altijd ruimte voor zelfs grote services hebben , zodat ze snel worden gemaakt. De meeste mensen dit niet nodig omdat services moet meestal klein en dus het is niet moeilijk te vinden lokaal voor hen, maar als u Services is groot en moet ze snel gemaakt (en bereid zijn te accepteren van de andere wisselwerking zoals toegenomen impactfulness van storingen en bronnen blijft wachten op de werklast worden gepland unutilized) en vervolgens de defragmentatie strategie voor u is.

Het onderstaande diagram geeft een visuele weergave van twee verschillende clusters, één die wordt gedefragmenteerd en één die niet. In het geval van evenwichtige rekening houden met de bewegingen die nodig zijn om te plaatsen in een van de grootste-objecten als een nieuw worden gemaakt, in vergelijking met de gedefragmenteerde cluster, waar onmiddellijk op knooppunten 4 of 5 kan worden geplaatst.

![Vergelijken en op evenwichtige gedefragmenteerd Clusters][Image1]

## <a name="defragmentation-pros-and-cons"></a>Defragmentatie voor- en nadelen
Dus wat zijn de conceptuele optimalisatie? Het is raadzaam grondig meting van de werklast voor het inschakelen van defragmentatie metrics. Hier is een tabel snel dingen te denken:

| Defragmentatie-professionals  | Nadelen van defragmentatie |
|----------------------|----------------------|
|Sneller maken van grote services | Concentraat op minder knooppunten, toenemende bronconflicten laden
|Hiermee verlaagt de verplaatsing van gegevens tijdens het maken van    | Storingen kunnen invloed hebben op meer services en dat meer lospeuteren
|Uitgebreide beschrijving van de eisen en terugwinning van ruimte | Meer complexe configuratie van algemene Resource Management

Kunt u de gedefragmenteerde en normale metrics in hetzelfde cluster mengen en de bronnenbeheerder van best zal doen om ervoor te zorgen dat u krijgt een lay-out die zo veel mogelijk van de defragmentatie metrics consolideert het kunt wanneer probeert de rest verspreid. Het exacte resultaat u krijgt afhankelijk van het aantal balancing metrics vergeleken met het aantal defragmentatie metrics, het gewicht en hun huidige belastingen, enz.

## <a name="configuring-defragmentation-metrics"></a>Configureren defragmentatierapporten metrics
Een globale besluit in het cluster configureren defragmentatierapporten metrics is en afzonderlijke statistieken kunnen worden geselecteerd voor defragmentatie:

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Disk" Value="true" />
    <Parameter Name="CPU" Value="false" />
</Section>
```

## <a name="next-steps"></a>Volgende stappen
- De Cluster Resource Manager heeft een groot aantal opties voor het beschrijven van het cluster. Meer informatie over deze Kijk uit dit artikel bij het [beschrijven van een cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)
- Meetgegevens worden hoe de Service Fabric Cluster Resource Manager beheert, verbruik en capaciteit in het cluster. Voor meer Kijk informatie over deze en hoe u deze configureert dan eens [in dit artikel](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png

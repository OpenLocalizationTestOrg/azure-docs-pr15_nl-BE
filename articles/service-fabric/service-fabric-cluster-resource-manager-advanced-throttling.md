<properties
   pageTitle="Beperken in de Service Fabric cluster resourcemanager | Microsoft Azure"
   description="Informatie over de throttles die door de Service Fabric Cluster Resource Manager configureren."
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


# <a name="throttling-the-behavior-of-the-service-fabric-cluster-resource-manager"></a>Beperking van de werking van de Service Cluster Resource Manager Fabric
Zelfs als de bronnenbeheerder Cluster hebt geconfigureerd, kan het cluster ophalen verstoord. Zo kan er gelijktijdige knooppunt veroorzaakt domein fouten of -wat er zou gebeuren als die zijn opgetreden tijdens een upgrade? De bronnenbeheerder zijn best zal proberen alles te herstellen, maar in tijden als volgt kunt u overwegen een backstop zodat het cluster zelf heeft een kans om te stabiliseren (de knooppunten die terug doen, de netwerkomstandigheden Retoucheren zelf, gecorrigeerde bits krijgen geïmplementeerd). De Service Fabric Cluster Resource Manager bevat om te helpen met dit soort situaties, verschillende throttles. Deze throttles zijn redelijk storend en over het algemeen beter niet worden gebruikt, tenzij er sprake is van een zorgvuldige gedaan om het bedrag van de parallelle werk dat daadwerkelijk kan worden uitgevoerd in het cluster, evenals een regelmatige wiskunde moet reageren op deze gesorteerd van (ahem) niet-geplande macroscopische herconfiguratie gebeurtenissen (BTW: "Zeer slechte dagen").

In het algemeen raadzaam zeer slechte dagen via andere opties (zoals code regelmatig updates en voorkomen eerst de cluster overscheduling) in plaats van een beperking van het cluster om te voorkomen dat deze bronnen wanneer deze probeert op te lossen zelf voorkomen). De throttles beschikt over de standaardwaarden die we hebben gevonden via ervaring ok standaardinstellingen, maar u moet waarschijnlijk een kijkje nemen en ze afstemmen met de verwachte werkelijke belasting. Terwijl niet overmatig wordt beperkt of laden die het cluster aanbevolen die de conclusie wordt dat er zijn gevallen waarin (totdat u deze verhelpen kunt) duurt waarin u moet beschikken over een aantal throttles, zelfs als het cluster is het langer om te stabiliseren.

##<a name="configuring-the-throttles"></a>De throttles configureren
De throttles die standaard zijn opgenomen, zijn:

-   GlobalMovementThrottleThreshold: Hiermee bepaalt u het totale aantal verplaatsingen in het cluster via een bepaalde tijd (gedefinieerd als de GlobalMovementThrottleCountingInterval, de waarde in seconden)
-   MovementPerPartitionThrottleThreshold: Hiermee bepaalt u het totale aantal verplaatsingen voor elke partitie service gedurende enige tijd (de MovementPerPartitionThrottleCountingInterval, de waarde in seconden)

``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThreshold" Value="1000" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
     <Parameter Name="MovementPerPartitionThrottleThreshold" Value="50" />
     <Parameter Name="MovementPerPartitionThrottleCountingInterval" Value="600" />
</Section>
```

Vergeet niet dat die we zag klanten gebruik van deze throttles is meestal omdat zij reeds in een omgeving met beperkte resources (zoals beperkte netwerkbandbreedte in afzonderlijke knooppunten of schijven die niet tot aan de eisen van de parallelle replica waren maakt die zijn erop wordt geplaatst) die betekende dat de bewerkingen slagen wouldn't of toch langzaam zou zijn.  In deze gevallen zijn klanten vertrouwd weet dat ze mogelijk van de hoeveelheid tijd die nodig zou hebben om het cluster uitbreiden zijn te bereiken, een stabiel is, met inbegrip van weet dat ze kunnen uiteindelijk op lagere algemene betrouwbaarheid worden uitgevoerd terwijl ze zijn beperkt.

## <a name="next-steps"></a>Volgende stappen
- Bekijk voor meer informatie over hoe het Cluster Resource Manager beheert en saldi laden in het cluster, het artikel op [load balancing](service-fabric-cluster-resource-manager-balancing.md)
- De Cluster Resource Manager heeft een groot aantal opties voor het beschrijven van het cluster. Meer informatie over deze Kijk uit dit artikel bij het [beschrijven van een cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)

<properties
   pageTitle="Service Fabric Cluster Resource Manager: verplaatsing kosten | Microsoft Azure"
   description="Overzicht van de kosten van de verplaatsing voor Service configuratieservices"
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

# <a name="service-movement-cost-for-influencing-cluster-resource-manager-choices"></a>Verplaatsing van servicekosten voor Cluster Resource Manager keuzen beïnvloeden
Een belangrijke factor te overwegen wanneer u probeert om te bepalen welke wijzigingen aanbrengen in een cluster en de score van een oplossing is de totale kosten van deze oplossing te bereiken.

Service-exemplaren of replica's kosten CPU tijd en netwerk bandbreedte ten minste verplaatsen. Voor stateful services kost het ook de hoeveelheid ruimte op de schijf die u nodig hebt voor het maken van een kopie van de staat voordat u afsluit oude replica's. Duidelijk wilt minimaliseren van de kosten van de oplossingen die Azure Service Fabric Cluster Resource Manager wordt. Maar u ook niet wilt negeren, oplossingen die de toewijzing van bronnen in het cluster aanzienlijk zou verbeteren.

Cluster Resource Manager heeft twee manieren om de kosten te berekenen en te beperken, terwijl wordt geprobeerd het cluster volgens de andere doelen beheren. De eerste is dat bij de planning van het Cluster Resource Manager is een nieuwe indeling voor het cluster, het telt elke verplaatsing die zou. In eenvoudige gevallen, als er twee oplossingen met ongeveer hetzelfde Financieringssaldo (score) aan het einde en klik vervolgens met de laagste kosten (totale aantal zetten) nemen.

Dit werkt heel goed. Maar met standaard of statische belasting, is het niet waarschijnlijk in een complex systeem dat alle Hiermee gelijk zijn. Sommige zijn waarschijnlijk veel duurder.

## <a name="changing-a-replicas-move-cost-and-factors-to-consider"></a>Wijzigen van een replica kosten verplaatsen en factoren
Als rapportering laden (een ander onderdeel van het Cluster Resource Manager), kunt u de service zelf te melden hoe kostbaar de dienst wordt op een bepaald moment verplaatst.

Code:

```csharp
this.ServicePartition.ReportMoveCost(MoveCost.Medium);
```

MoveCost heeft vier niveaus: nul, laag, gemiddeld en hoog. Dit zijn ten opzichte van elkaar, met uitzondering van nul. Nul betekent dat het verplaatsen van een replica is gratis en mag niet meegeteld in de score van de oplossing. Uw kosten verplaatsen op Hoog instellen is *niet* de garantie dat de replica niet verplaatst, net dat het niet verplaatst tenzij er een goede reden voor.

![Kosten als een factor bij de keuze van de replica's voor verplaatsing verplaatsen][Image1]

MoveCost helpt u bij het vinden van de oplossingen die ervoor zorgen dat de totale minimale verstoring en zijn het gemakkelijkst te bereiken nog steeds bij gelijkwaardige saldo binnenkomen. Begrip van de kosten van een service kan worden ten opzichte van veel dingen. De meest voorkomende factoren bij de berekening van de kosten van de verplaatsing zijn:

- Het bedrag van de staat of de gegevens die de service te gaan.
- De kosten van het verbreken van de verbinding van clients. De kosten van het verplaatsen van een primaire replica is meestal hoger dan de kosten van het verplaatsen van een secundaire replica.
- De kosten van een tijdens een vlucht bewerking onderbreken. Niveau bepaalde bewerkingen op de gegevens worden opgeslagen of bewerkingen die worden uitgevoerd in reactie op een kostbaar zijn. Na een bepaald punt wilt u niet stoppen ze als u niet wilt. Dus voor de duur van de bewerking vergroten u de kosten voor de kans die de replica van de service of het exemplaar wordt verplaatst. Wanneer de bewerking is voltooid, kunt u deze weer in de normale plaatsen.

## <a name="next-steps"></a>Volgende stappen
- Metrics service Fabric Cluster Resource Manager gebruikt om verbruik en capaciteit in het cluster te beheren. Bekijk voor meer informatie over parameters en hoe u deze configureert, [verbruik beheren en laden in Service-structuur met de parameters](service-fabric-cluster-resource-manager-metrics.md).
- Meer informatie over hoe het Cluster Resource Manager beheert en saldi laden in het cluster, uitchecken [Balancing cluster Service Fabric](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png

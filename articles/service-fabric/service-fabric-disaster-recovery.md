<properties
   pageTitle="Noodherstel van Azure Service Fabric | Microsoft Azure"
   description="Azure Service Fabric biedt de mogelijkheden die nodig zijn voor de behandeling van alle soorten rampen. In dit artikel beschrijft de soorten rampen die zich kunnen voordoen en hoe u moet handelen."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="seanmck"/>

# <a name="disaster-recovery-in-azure-service-fabric"></a>Noodherstel in Azure Service Fabric

Een essentieel onderdeel van het leveren van een hoge beschikbaarheid wolk toepassing is ervoor te zorgen dat deze verschillende soorten fouten, ook als deze buiten het besturingselement kan overleven. Dit artikel beschrijft de fysieke indeling van een Azure Service Fabric '-cluster in het kader van mogelijke rampen en biedt u informatie over het omgaan met dergelijke rampen te beperken of te elimineren van het risico van verlies van gegevens of uitval.

## <a name="physical-layout-of-service-fabric-clusters-in-azure"></a>Fysieke lay-out van de Service Fabric clusters in Azure

Om het risico van verschillende soorten fouten te begrijpen, is het handig om te weten hoe clusters zijn fysiek ingedeeld in Azure.

Wanneer u een Service Fabric '-cluster in Azure maakt, bent u verplicht om te kiezen van een regio waar deze worden gehost. De infrastructuur van Azure bepalingen vervolgens de bronnen voor dat cluster in het gebied, met name het aantal virtuele machines (VMs) aangevraagd. We nauwer kijken naar hoe en waar deze VMs worden ingericht.

### <a name="fault-domains"></a>Fout met betrekking tot domeinen

Standaard worden de VMs in het cluster gelijkmatig verdeeld over logische groepen bekend als de fout met betrekking tot domeinen (FDs), die de machines die op basis van de mogelijke storingen in de hardware van de host in segmenten. Met name als twee VMs in twee afzonderlijke FDs, kunt u zeker zijn dat ze het dezelfde bron- of stopcontact niet delen. Als gevolg hiervan een lokaal netwerk of een stroomstoring op het gebied van een VM heeft geen invloed op de andere, zodat de stof aan de taakverdeling van het werk van de computer niet reageert binnen het cluster-Service.

U kunt de lay-out van het cluster in fout met betrekking tot domeinen met de cluster-kaart die in de [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)visualiseren:

![Knooppunten verspreid over een fout met betrekking tot domeinen in Service Fabric Explorer][sfx-cluster-map]

>[AZURE.NOTE] De andere as in de overzichtsweergave van het cluster bevat upgraden domeinen die logisch op basis van de geplande onderhoud knooppunten groeperen. Clusters in Azure Service Fabric worden altijd ingedeeld in vijf domeinen voor upgrade.

### <a name="geographic-distribution"></a>Geografische spreiding

Er zijn momenteel [26 Azure regio's in de hele wereld][azure-regions], met verschillende meer aangekondigd. Een afzonderlijke regio kan bevatten een of meer fysieke datacenters afhankelijk van de vraag en de beschikbaarheid van geschikte locaties, onder andere factoren. Bedenk echter dat zelfs in regio's met meerdere fysieke datacenters, er geen garantie is dat het cluster van VMs gelijkmatig worden verdeeld over de fysieke locaties. Inderdaad, op dit moment alle VMs voor een bepaald cluster zijn ingericht in één fysieke site.

## <a name="dealing-with-failures"></a>Omgaan met fouten

Er zijn verschillende typen fouten die van invloed kunnen uw cluster, elk met een eigen beperking. Gaan we ze in volgorde van waarschijnlijkheid optreden.

### <a name="individual-machine-failures"></a>Storingen in afzonderlijke bewerkingsplaatsen

Zoals gezegd, vormen afzonderlijke bewerkingsplaatsen storingen, binnen de VM of in de hardware of software waarop deze binnen een domein veroorzaakt geen risico op hun eigen. Fabric-service wordt meestal storing binnen enkele seconden te detecteren en reageren dienovereenkomstig op basis van de status van het cluster. Als het knooppunt de primaire replica's van een partitie host is, wordt een nieuwe primaire in secundaire replica van de partitie gekozen. Als Azure ervoor zorgt dat de machine mislukte back-up, worden automatisch opnieuw deelnemen aan het cluster en zijn aandeel van de werklast opnieuw uitvoeren.

### <a name="multiple-concurrent-machine-failures"></a>Meerdere gelijktijdige machine-storingen

Fout met betrekking tot domeinen het risico van fouten bij gelijktijdige machine aanzienlijk wordt verkleind, maar er is altijd de mogelijkheden voor meerdere willekeurig mislukken brengen tegelijkertijd naar verschillende machines in een cluster.

In het algemeen als een meerderheid van de knooppunten beschikbaar blijven, blijft het cluster werken, zij het op kleinere capaciteit zoals stateful replica's in een kleiner aantal machines verpakt krijgen en minder stateless exemplaren beschikbaar zijn om te laden worden verspreid.

#### <a name="quorum-loss"></a>Quorum verlies

Als een meerderheid van de replica's van een stateful service partitie omlaag gaan, komt die partitie er een staat bekend als 'quorum verlies'. Fabric-Service stopt op dit punt waardoor schrijfbewerkingen op die partitie om ervoor te zorgen dat de toestand consistent en betrouwbaar blijft. We kiest van kracht een periode van niet beschikbaar om ervoor te zorgen dat clients niet worden verteld dat hun gegevens is opgeslagen, het is niet te accepteren. Houd er rekening mee dat als u ervoor hebt gekozen om te lezen zodat van secundaire replica's van stateful service, u blijven kunt voor het uitvoeren van de bewerkingen in deze staat. Een partitie quorum verlies blijft totdat een voldoende aantal replica's terugkomen of Clusterbeheer zorgt ervoor dat het systeem op te verplaatsen met behulp van de [API voor reparatie ServiceFabricPartition][repair-partition-ps].

>[AZURE.WARNING] Een herstelbewerking uitvoeren terwijl de primaire replica beneden is leidt tot verlies van gegevens.

Systeemservices kunnen quorum verloren gaan, ook met de gevolgen voor de betrokken dienst wordt lijden. Bijvoorbeeld, quorum verlies in de naamgevingsservice is van invloed op naamomzetting dat quorum verlies in de failover-manager-service nieuwe service maken en failover blokkeert. In tegenstelling tot de voor uw eigen diensten, het repareren van het systeem wordt *niet* aanbevolen. In plaats daarvan is het raadzaam om gewoon wachten totdat de omlaag replica's terug.

#### <a name="minimizing-the-risk-of-quorum-loss"></a>Minimaliseert het risico van verlies quorum

U kunt het risico van verlies quorum minimaliseren door vergroten doel replica instellen voor uw service. Het is handig om na te denken van het aantal replica's moet u in termen van het aantal niet-beschikbare knooppunten die u op tolereren kunt zodra terwijl beschikbaar voor schrijfbewerkingen, die toepassing te houden rekening of cluster upgrades kunnen knooppunten tijdelijk niet beschikbaar is, naast problemen met de hardware.

Bekijk de volgende voorbeelden ervan uitgaande dat u uw services als u wilt dat een MinReplicaSetSize van drie, het kleinste getal aanbevolen voor productie-services hebt geconfigureerd. Met een TargetReplicaSetSize van drie (een primaire en secundaire servers voor twee), een hardwarefout opgetreden tijdens een upgrade (twee replica's omlaag) kan resulteren in verlies van quorum en uw service zal alleen-lezen. Ook als er vijf replica's, zou u kunnen weerstaan twee fouten tijdens de upgrade (drie replica's omlaag) als de resterende twee replica's kunnen nog steeds een quorum in de replicaset minimale vormen.

### <a name="data-center-outages-or-destruction"></a>Data center storingen of vernietiging

In zeldzame gevallen kunnen fysieke datacenters worden tijdelijk niet beschikbaar vanwege verlies van energie-of netwerkverbindingen. In deze gevallen clusters Fabric-Service en toepassingen ook niet beschikbaar, maar de gegevens blijven behouden. Voor clusters die in Azure is uitgevoerd, kunt u updates weergeven op storingen op de [statuspagina van Azure][azure-status-dashboard].

In het zeer onwaarschijnlijke geval dat een hele fysieke Datacenter wordt vernietigd, worden alle Service Fabric clusters gehost er verloren, samen met hun status.

Ter bescherming tegen deze mogelijkheid, het is zeer belangrijk regelmatig aan een winkel voor geo-redundante [back-up van uw provincie](service-fabric-reliable-services-backup-restore.md) en ervoor te zorgen dat u de mogelijkheid om deze te herstellen hebt gevalideerd. Hoe vaak u een back-up uitvoert is afhankelijk van uw herstel punt doelstelling (vrijgegeven Productieorder). Zelfs als u back-up en terugzetten nog volledig niet hebt geïmplementeerd, moet u implementeert een handler voor de `OnDataLoss` gebeurtenis, zodat u zich kunt aanmelden als het voorkomt als volgt:

```c#
protected virtual Task<bool> OnDataLoss(CancellationToken cancellationToken)
{
  ServiceEventSource.Current.ServiceMessage(this, "OnDataLoss event received.");
  return Task.FromResult(true);
}
```


### <a name="software-failures-and-other-sources-of-data-loss"></a>Softwarefouten en andere bronnen van verlies van gegevens

Als een oorzaak van het verlies van gegevens zijn code fouten in services, menselijke operationele fouten en inbreuken op de beveiliging meer dan de wijdverbreide data center fouten. In alle gevallen de herstelstrategie is echter hetzelfde: regelmatig back-ups van alle stateful-services nemen en de mogelijkheid om te herstellen van die staat uitoefenen.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het simuleren van verschillende storingen met [testbaarheid framework](service-fabric-testability-overview.md)
- Lees andere bronnen voor herstel na storingen en hoge beschikbaarheid. Microsoft heeft een grote hoeveelheid informatie over deze onderwerpen worden gepubliceerd. Terwijl sommige van deze documenten naar specifieke technieken voor gebruik in andere producten verwijzen, bevatten ze veel algemene aanbevolen procedures die u in de Service Fabric-context toepassen kunt:
 - [Controlelijst voor beschikbaarheid](../best-practices-availability-checklist.md)
 - [Uitvoeren van een detailanalyse disaster recovery](../sql-database/sql-database-disaster-recovery-drills.md)
 - [Noodherstel en hoge beschikbaarheid voor Azure toepassingen][dr-ha-guide]


<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png

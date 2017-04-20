<properties
   pageTitle="Problemen oplossen met statusrapporten systeem | Microsoft Azure"
   description="Beschrijving van de statusrapporten verzonden door Azure Service Fabric onderdelen en hun gebruik voor het cluster voor het oplossen van problemen of problemen met de toepassing."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="use-system-health-reports-to-troubleshoot"></a>Gebruik system health rapporten oplossen

Azure Service Fabric lijst onderdelen uit de doos op alle entiteiten in het cluster. Het [opslaan van de gezondheid](service-fabric-health-introduction.md#health-store) wordt gemaakt en entiteiten op basis van de verslagen van het systeem worden verwijderd. Ook worden ze in een hiërarchie die entiteit interacties worden vastgelegd.

> [AZURE.NOTE] Als de basisbegrippen met betrekking tot de gezondheid, lees meer op [Service Fabric gezondheid model](service-fabric-health-introduction.md).

Systeem health rapporten bieden inzicht in de cluster en functionaliteit van de toepassing en de vlag problemen via gezondheid. Statusrapporten systeem of voor toepassingen en services, entiteiten worden geïmplementeerd en vanuit het perspectief van het weefsel van de Service zijn goed. De rapporten bieden de statuscontrole van de bedrijfslogica van de service of het opsporen van vastgelopen processen. Services voor kunnen verrijken de gezondheidsgegevens met informatie over de logica.

> [AZURE.NOTE] Watchdogs statusrapporten zijn alleen *nadat* de systeemonderdelen maken van een entiteit zichtbaar. Wanneer een entiteit wordt verwijderd, wordt het archief van de gezondheid van alle statusrapporten gekoppeld automatisch verwijderd. Hetzelfde is het geval wanneer een nieuw exemplaar van de entiteit is gemaakt (bijvoorbeeld een nieuw exemplaar van de service-replica is gemaakt). Alle rapporten die zijn gekoppeld aan het oude exemplaar worden verwijderd en opgeruimd uit de winkel.

Het systeemonderdeel rapporten zijn te herkennen aan de bron, die met begint de "**System.**" het voorvoegsel. Watchdogs niet hetzelfde voorvoegsel gebruiken voor hun bronnen, zoals rapporten met ongeldige parameters worden geweigerd.
We bekijken enkele systeemrapporten om te begrijpen wat de gebeurtenis en het corrigeren van de mogelijke problemen die zij vertegenwoordigen.

> [AZURE.NOTE] Fabric-service blijft rapporten toevoegen op de voorwaarden van belang dat u inzicht in wat in het cluster en de toepassing gebeurt er verbeteren.

## <a name="cluster-system-health-reports"></a>Cluster systeem statusrapporten
De cluster gezondheid entiteit wordt automatisch gemaakt in de winkel voor de gezondheid. Als alles goed werkt, heeft geen systeem-rapport.

### <a name="neighborhood-loss"></a>Verlies van wijk
**System.Federation** meldt een fout wanneer een groep verlies wordt gedetecteerd. Het rapport is van de afzonderlijke knooppunten en de knooppunt-ID is opgenomen in de naam van de eigenschap. Als een groep in de gehele Service Fabric-ring verloren gaat, kunt u meestal twee gebeurtenissen (beide zijden van het rapport tussenruimte) verwachten. Als er meer groepen verloren gaan, zijn er meer gebeurtenissen.

Het rapport geeft de globale lease-out als de levensduur aan. Het rapport opnieuw elke helft van de duur van de TTL verzonden als de voorwaarde actief blijft. De gebeurtenis wordt automatisch verwijderd als het is verlopen. Verwijderen wanneer verlopen gedrag zorgt ervoor dat het rapport wordt opgeruimd uit het archief van de gezondheid goed, zelfs als het knooppunt rapportage is niet beschikbaar.

- **Bron-id**: System.Federation
- **Eigenschap**: begint met de **groep** en bevat knooppuntgegevens
- **Volgende stappen**: onderzoeken waarom de wijk is verbroken (bijvoorbeeld de communicatie tussen de knooppunten van het cluster controleren).

## <a name="node-system-health-reports"></a>Knooppunt systeem statusrapporten
**System.FM**, waarmee de failover-Manager-service is de instantie die de informatie over de knooppunten van het cluster wordt beheerd. Elk knooppunt moet beschikken over een verslag van de staat met System.FM. De diensten van het knooppunt worden verwijderd wanneer de status van het knooppunt wordt verwijderd (Zie [RemoveNodeStateAsync](https://msdn.microsoft.com/library/azure/mt161348.aspx)).

### <a name="node-updown"></a>Knooppunt omhoog/omlaag
System.FM rapporteert als OK wanneer het knooppunt lid wordt van de ring (het actief is). Een fout gerapporteerd wanneer het knooppunt de ring vertrekt (is het naar beneden, hetzij voor het upgraden of gewoon omdat het niet is). De hiërarchie van gezondheid gebouwd door de winkel gezondheid neemt actie geïmplementeerde entiteiten in correlatie met System.FM knooppunt rapporten. Volgens hem het knooppunt virtuele bovenliggend element van alle geïmplementeerde entiteiten. De geïmplementeerde entiteiten op dat knooppunt worden via query's als het knooppunt als omhoog wordt gemeld door System.FM met de zelfde instantie als het exemplaar van de entiteiten. Wanneer System.FM meldt dat het knooppunt niet actief is of opnieuw gestart (een nieuw exemplaar), ruimt de gezondheid winkel de geïmplementeerde entiteiten die alleen op het knooppunt omlaag of op het vorige exemplaar van het knooppunt kunnen bestaan.

- **Bron-id**: System.FM
- **Eigenschap**: staat
- **Volgende stappen**: als het knooppunt niet actief voor een upgrade is, deze moet geactiveerd zijn nadat de upgrade is voltooid. In dit geval moet de status worden overgeschakeld op OK. Als het knooppunt niet terugkomen of mislukt, moet het probleem meer onderzoek.

Het volgende voorbeeld wordt de gebeurtenis System.FM met een status van OK voor knooppunt van:

```powershell

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 2
                        SentAt                : 4/24/2015 5:27:33 PM
                        ReceivedAt            : 4/24/2015 5:28:50 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 5:28:50 PM

```


### <a name="certificate-expiration"></a>Verlopen certificaat
**System.FabricNode** geeft een waarschuwing wanneer in de buurt van verlopen certificaten die worden gebruikt door het knooppunt. Er zijn drie certificaten per knooppunt: **Certificate_cluster**, **Certificate_server**en **Certificate_default_client**. Wanneer de vervaldatum ten minste twee weken is, wordt de status van het rapport OK. Wanneer de vervaldatum binnen twee weken is, is het rapporttype een waarschuwing. TTL-waarde van deze gebeurtenissen is oneindig en ze als een knooppunt het cluster verlaat worden verwijderd.

- **Bron-id**: System.FabricNode
- **Eigenschap**: begint met **certificaat** en bevat meer informatie over het certificaattype
- **Volgende stappen**: de certificaten bijwerken als ze verlopen in de buurt zijn.

### <a name="load-capacity-violation"></a>Schending van de capaciteit te laden
De taakverdeling Service Fabric meldt een waarschuwing als een schending van de capaciteit knooppunt wordt gedetecteerd.

 - **Bron-id**: System.PLB
 - **Eigenschap**: begint met **capaciteit**
 - **Volgende stappen**: Controleer verstrekte gegevens en de huidige capaciteit van het knooppunt weergeven.

## <a name="application-system-health-reports"></a>Systeem health rapporten
**System.CM**, dat staat voor de Manager van de Cluster-service, is de instantie die de gegevens over een toepassing beheert.

### <a name="state"></a>Staat
System.CM rapporteert als OK wanneer de toepassing is gemaakt of bijgewerkt. Deze informeert de gezondheid winkel wanneer de toepassing is verwijderd, zodat deze kan worden verwijderd uit het archief.

- **Bron-id**: System.CM
- **Eigenschap**: staat
- **Volgende stappen**: als de toepassing is gemaakt, moet het statusrapport van het Cluster Manager bevatten. Controleer de status van de toepassing door een query uitvoert (bijvoorbeeld de PowerShell-cmdlet * *Get-ServiceFabricApplication ApplicationName *applicationName***).

In het volgende voorbeeld wordt de gebeurtenis staat op de **fabric: / WordCount** toepassing:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 82
                                  SentAt                : 4/24/2015 6:12:51 PM
                                  ReceivedAt            : 4/24/2015 6:12:51 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : ->Ok = 4/24/2015 6:12:51 PM
```

## <a name="service-system-health-reports"></a>Service systeem statusrapporten
**System.FM**, waarmee de failover-Manager-service is de instantie die de informatie over services beheert.

### <a name="state"></a>Staat
System.FM rapporteert als OK wanneer de service is gemaakt. Worden de entiteit uit het archief van de gezondheid wanneer de service is verwijderd.

- **Bron-id**: System.FM
- **Eigenschap**: staat

In het volgende voorbeeld wordt de gebeurtenis staat op de service **fabric: WordCount/WordCountService**:

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService

ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Ok
PartitionHealthStates :
                        PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:01 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:01 PM
```

### <a name="unplaced-replicas-violation"></a>Schending van niet-geplaatste replica 's
**System.PLB** geeft een waarschuwing als een plaats voor een of meer replica's service kan niet worden gevonden. Het rapport wordt verwijderd wanneer deze verloopt.

- **Bron-id**: System.FM
- **Eigenschap**: staat
- **Volgende stappen**: de beperkingen van de service en de huidige status van de plaatsing.

In het volgende voorbeeld ziet u een fout voor een service is geconfigureerd met 7 doel replica's in een cluster met 5 knooppunten:

```xml
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB',
                        Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                        ConsiderWarningAsError=false.

PartitionHealthStates :
                        PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        AggregatedHealthState : Warning

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 10
                        SentAt                : 3/22/2016 7:56:53 PM
                        ReceivedAt            : 3/22/2016 7:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        HealthState           : Warning
                        SequenceNumber        : 131032232425505477
                        SentAt                : 3/23/2016 4:14:02 PM
                        ReceivedAt            : 3/23/2016 4:14:03 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        fabric:/WordCount/WordCountService Secondary Partition a1f83a35-d6bf-4d39-b90d-28d15f39599b could not be placed, possibly,
                        due to the following constraints and properties:  
                        Placement Constraint: N/A
                        Depended Service: N/A

                        Constraint Elimination Sequence:
                        ReplicaExclusionStatic eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        ReplicaExclusionDynamic eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.

                        Nodes Eliminated By Constraints:

                        ReplicaExclusionStatic:
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status:
                        None/None

                        ReplicaExclusionDynamic:
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status:
                        None/None


                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="partition-system-health-reports"></a>Partitie systeem statusrapporten
**System.FM**, waarmee de failover-Manager-service is de instantie die de informatie over partities service beheert.

### <a name="state"></a>Staat
System.FM rapporteert als OK wanneer de partitie is gemaakt en in orde is. Worden de entiteit uit het archief van de gezondheid van wanneer de partitie wordt verwijderd.

Als de partitie dan het aantal minimale replica is, rapporteert een fout. Als de partitie niet lager dan het aantal minimale replica is, maar deze lager dan het aantal doel replica is, wordt een waarschuwing weergegeven. Als de partitie verloren quorum, rapporteert System.FM een fout.

Andere belangrijke gebeurtenissen zijn een waarschuwing wanneer de nieuwe configuratie langer duurt dan verwacht en wanneer de build langer duurt dan verwacht. De verwachte tijden voor het bouwen en wijzigen van de configuratie geconfigureerd worden op basis van scenario's service. Bijvoorbeeld, als een service een terabyte van staat, zoals een SQL-Database, heeft duurt de build langer dan voor een service met een kleine hoeveelheid staat.

- **Bron-id**: System.FM
- **Eigenschap**: staat
- **Volgende stappen**: als de status niet OK is, is het mogelijk dat sommige replica's niet zijn gemaakt, geopend, of juist gepromoveerd tot primaire of secundaire. In veel gevallen is de oorzaak een service fout in de toepassing open of rol wijzigen.

In het volgende voorbeeld ziet u een gezonde partitie:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/StatelessPiApplication/StatelessPiService | Get-ServiceFabricPartitionHealth
PartitionId           : 29da484c-2c08-40c5-b5d9-03774af9a9bf
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 38
                        SentAt                : 4/24/2015 6:33:10 PM
                        ReceivedAt            : 4/24/2015 6:33:31 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:33:31 PM
```

In het volgende voorbeeld ziet u de status van een partitie die is lager dan het aantal van doel-replica. De volgende stap is om een beschrijving van de partitie waaruit blijkt hoe is geconfigureerd: **MinReplicaSetSize** is twee en **TargetReplicaSetSize** is zeven. Vervolgens krijgt u het aantal knooppunten in het cluster: vijf. Worden dus in dit geval twee replica's kunnen niet geplaatst.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 37
                        SentAt                : 4/24/2015 6:13:12 PM
                        ReceivedAt            : 4/24/2015 6:13:31 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 4/24/2015 6:13:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService

PartitionId            : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
PartitionKind          : Int64Range
PartitionLowKey        : 1
PartitionHighKey       : 26
PartitionStatus        : Ready
LastQuorumLossDuration : 00:00:00
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 7
HealthState            : Warning
DataLossNumber         : 130743727710830900
ConfigurationNumber    : 8589934592


PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a>Beperkingsfout replica
**System.PLB** geeft een waarschuwing als een replica Beperkingsfout detecteert geen replica's van de partitie plaatsen.

- **Bron-id**: System.PLB
- **Eigenschap**: begint met **ReplicaConstraintViolation**

## <a name="replica-system-health-reports"></a>Replica systeem statusrapporten
**System.RA**, dat het onderdeel herconfiguratie agent vertegenwoordigt, is de dienst voor de replica staat.

### <a name="state"></a>Staat
**System.RA** rapporteert als OK wanneer u de replica is gemaakt.

- **Bron-id**: System.RA
- **Eigenschap**: staat

In het volgende voorbeeld ziet u een gezonde replica:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth
PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
ReplicaId             : 130743727717237310
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743727718018580
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:02 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:02 PM
```

### <a name="replica-open-status"></a>Replica open status
De beschrijving van dit rapport gezondheid bevat de begintijd (Coordinated Universal Time) wanneer de API-aanroep is aangeroepen.

**System.RA** geeft een waarschuwing als de replica openen langer dan de geconfigureerde periode duurt (standaard: 30 minuten). Als de beschikbaarheid van Services heeft gevolgen voor de API, wordt het rapport uitgegeven veel sneller (een configureerbaar interval, met een standaardwaarde van 30 seconden). De tijd gemeten omvat de tijd voor de replicatie openen en de service is geopend. De eigenschap gewijzigd op OK als de open is voltooid.

- **Bron-id**: System.RA
- **Eigenschap**: **ReplicaOpenStatus**
- **Volgende stappen**: als de status niet OK is, onderzoekt u waarom de replica open duurt langer dan verwacht.

### <a name="slow-service-api-call"></a>Trage service API-aanroep
**System.RAP** en **System.Replicator** genereren een waarschuwing als een aanroep van de code van de gebruiker duurt langer dan de geconfigureerde tijd. De waarschuwing wordt gewist wanneer het gesprek is voltooid.

- **Bron-id**: System.RAP of System.Replicator
- **Eigenschap**: de naam van de langzame API. De beschrijving bevat meer details over de tijd die de API in behandeling is.
- **Volgende stappen**: onderzoeken waarom de oproep duurt langer dan verwacht.

In het volgende voorbeeld ziet u een partitie quorum verloren gaan, en het onderzoek werk gedaan om erachter te komen waarom. Een van de replica's heeft een status waarschuwing, zodat u beschikt over de gezondheid. Dat de service langer duurt dan verwacht, een gebeurtenis die wordt gemeld door System.RAP worden weergegeven. Nadat deze informatie is ontvangen, is de volgende stap de code bekijken en er onderzoeken. Voor deze aanvraag genereert de uitvoering van de **RunAsync** van de stateful-service een onverwerkte uitzondering. De replica's zijn recycling, zodat alle replica's in de waarschuwing staat mogelijk niet weergegeven. U kunt opnieuw verkrijgen van de status en zoek naar eventuele verschillen in de replica-ID. In bepaalde gevallen krijgt de pogingen u aanwijzingen.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

Wanneer u de toepassing niet juist werkt onder de debugger start, geven de diagnostische gebeurtenissen van windows de uitzondering van RunAsync:

![Diagnostische gebeurtenissen van Visual Studio 2015: fout RunAsync stof: / HelloWorldStatefulApplication.][1]

Diagnostische gebeurtenissen van Visual Studio 2015: RunAsync fout in **fabric: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Volledige replicatiewachtrij
**System.Replicator** geeft een waarschuwing als de replicatiewachtrij vol is. Op de primaire, dit gebeurt meestal omdat een of meer secundaire replica's zijn traag bewerkingen bevestigt. Op de secundaire server, dit gebeurt meestal wanneer de service wordt langzaam de bewerkingen toepassen. De waarschuwing wordt gewist wanneer de wachtrij vol is.

- **Bron-id**: System.Replicator
- **Eigenschap**: **PrimaryReplicationQueueStatus** of **SecondaryReplicationQueueStatus**, afhankelijk van de rol van replica

### <a name="slow-naming-operations"></a>Langzame Naming bewerkingen

Wanneer een bewerking Naming langer dan acceptabel duurt, **System.NamingService** gezondheid rapporten over de primaire replica. Voorbeelden van Naming bewerkingen zijn [CreateServiceAsync](https://msdn.microsoft.com/library/azure/mt124028.aspx) of [DeleteServiceAsync](https://msdn.microsoft.com/library/azure/mt124029.aspx). Meer methoden kunnen u vinden onder FabricClient, bijvoorbeeld onder [beheer van de methoden](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.aspx) of [methoden voor de eigenschap](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.propertymanagementclient.aspx).

> [AZURE.NOTE] De Naming service-namen worden omgezet in een locatie in het cluster en kan gebruikers voor het beheren van servicenamen en eigenschappen. Het is een Service-Fabric gepartitioneerd permanente service. Een van de partities geeft de eigenaar van de autoriteit, met metagegevens over alle namen Fabric-Service en services. De namen van de stof van de Service zijn toegewezen aan verschillende partities, de naam van de eigenaar-partities genoemd, zodat de service kan uitgebreid worden. Lees meer over [Naming service](service-fabric-architecture.md).

Wanneer een bewerking Naming langer duurt dan verwacht, wordt de bewerking gemarkeerd met een waarschuwing-verslag over de *primaire replica van de partitie van Naming service de bewerking fungeert*. Als de bewerking met succes is voltooid, wordt de waarschuwing uitgeschakeld. Als de bewerking is voltooid met fout, bevat het rapport health details over de fout.

- **Bron-id**: System.NamingService
- **Eigenschap**: begint met het voorvoegsel **Duration_** en identificeert de trage werking en de naam van de stof van de Service waarop de bewerking wordt toegepast. Bijvoorbeeld als service bij naam fabric maken: / MyApp/MijnService te lang duurt, is de eigenschap Duration_AOCreateService.fabric:/MyApp/MyService. AO verwijst naar de rol van de partitie Naming voor deze naam en bewerking.
- **Volgende stappen**: selectievakje waarom het Naming mislukt. Elke bewerking kan verschillende oorzaken hebben. Verwijder bijvoorbeeld service op een knooppunt vastgelopen omdat de toepassinghost op een knooppunt vanwege een fout in de code van de gebruiker vastlopen blijft.

In het volgende voorbeeld ziet u een bewerking maken. De bewerking duurde langer dan de geconfigureerde duur. AO pogingen en werk naar nr. NIET de laatste bewerking time-out voltooid. In dit geval is dezelfde replica primair voor de AO en geen rollen.

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>DeployedApplication systeem statusrapporten
**System.Hosting** is de autoriteit op gedistribueerde entiteiten.

### <a name="activation"></a>Activering
System.Hosting rapporteert als OK wanneer een toepassing is geactiveerd op het knooppunt. Anders wordt een fout.

- **Bron-id**: System.Hosting
- **Eigenschap**: activering, met inbegrip van de implementatie-versie
- **Volgende stappen**: als de toepassing beschadigd is, onderzoekt u waarom de activering mislukt.

In het volgende voorbeeld ziet u succesvolle activering:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName Node.1 -ApplicationName fabric:/WordCount

ApplicationName                    : fabric:/WordCount
NodeName                           : Node.1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : Node.1
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 130743727751144415
                                     SentAt                : 4/24/2015 6:12:55 PM
                                     ReceivedAt            : 4/24/2015 6:13:03 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Downloaden
**System.Hosting** meldt een fout als de toepassing pakket downloaden mislukt.

- **Bron-id**: System.Hosting
- **Eigenschap**: * *downloaden:*RolloutVersion***
- **Volgende stappen**: onderzoeken waarom de download op het knooppunt is mislukt.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage systeem statusrapporten
**System.Hosting** is de autoriteit op gedistribueerde entiteiten.

### <a name="service-package-activation"></a>Service pakket activeren
System.Hosting rapporten als OK als de activering van het pakket service op het knooppunt voltooid is. Anders wordt een fout.

- **Bron-id**: System.Hosting
- **Eigenschap**: activering
- **Volgende stappen**: onderzoeken waarom de activering is mislukt.

### <a name="code-package-activation"></a>Code pakket activeren
**System.Hosting** meldt OK voor elk pakket code als de activering geslaagd is. Als de activering mislukt, wordt een waarschuwing weergegeven zoals deze is geconfigureerd. Als **CodePackage** niet geactiveerd of wordt afgesloten met een fout die groter is dan de geconfigureerde **CodePackageHealthErrorThreshold**, hosting meldt een fout. Als een servicepakket meerdere code-pakketten bevat, wordt voor elk een activering rapport gegenereerd.

- **Bron-id**: System.Hosting
- **Eigenschap**: gebruikt het voorvoegsel **CodePackageActivation** en bevat de naam van het pakket van de code en het ingangspunt als * *CodePackageActivation:*CodePackageName*:*SetupEntryPoint/EntryPoint voor* ** (bijvoorbeeld, **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Registratie van type service
**System.Hosting** rapporten als OK als het servicetype is geregistreerd. Dit meldt een fout als de registratie niet is gedaan in de tijd (zoals ingesteld met behulp van **ServiceTypeRegistrationTimeout**). Als u het servicetype is niet geregistreerd in het knooppunt, namelijk de bewerkingstijd is gesloten. Host een waarschuwing gemeld.

- **Bron-id**: System.Hosting
- **Eigenschap**: het voorvoegsel **ServiceTypeRegistration** wordt gebruikt en bevat de naam van de service type (bijvoorbeeld **ServiceTypeRegistration:FileStoreServiceType**)

In het volgende voorbeeld ziet u een gezonde gedistribueerde servicepakket:

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName Node.1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 130743727751456915
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 130743727751613185
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 130743727753644473
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Downloaden
**System.Hosting** meldt een fout als de service-pakket downloaden mislukt.

- **Bron-id**: System.Hosting
- **Eigenschap**: * *downloaden:*RolloutVersion***
- **Volgende stappen**: onderzoeken waarom de download op het knooppunt is mislukt.

### <a name="upgrade-validation"></a>Validatie van de upgrade
**System.Hosting** rapporteert een fout als validatie tijdens de upgrade mislukt of als de upgrade op het knooppunt mislukt.

- **Bron-id**: System.Hosting
- **Eigenschap**: het voorvoegsel **FabricUpgradeValidation** wordt gebruikt en de upgrade-versie bevat
- **Beschrijving**: verwijst naar de fout is opgetreden

## <a name="next-steps"></a>Volgende stappen
[Service Fabric-statusrapporten weergeven](service-fabric-view-entities-aggregated-health.md)

[Het rapport en de service controleren](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Controleren en diagnosticeren lokaal services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric toepassing upgrade](service-fabric-application-upgrade.md)

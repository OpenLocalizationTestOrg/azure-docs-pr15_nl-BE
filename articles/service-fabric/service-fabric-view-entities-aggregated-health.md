<properties
   pageTitle="Het weergeven van Azure Service Fabric-entiteiten samengevoegd gezondheid | Microsoft Azure"
   description="Beschrijving van het opvragen, bekijken en evalueren van de geaggregeerde status Azure Service Fabric-entiteiten, door middel van query's voor de gezondheid en algemene query's."
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

# <a name="view-service-fabric-health-reports"></a>Service Fabric-statusrapporten weergeven
Azure Service Fabric introduceert een [gezondheid model](service-fabric-health-introduction.md) dat bestaat uit gezondheid entiteiten op welke onderdelen en watchdogs plaatselijke omstandigheden die zij controleert kunt melden. Het [opslaan van de gezondheid van](service-fabric-health-introduction.md#health-store) aggregaten alle gezondheidsgegevens om te bepalen of entiteiten gezonde.

Het cluster wordt uit de doos gevuld met statusrapporten verzonden door de onderdelen van het systeem. Lees meer bij [Gebruik system health rapporten oplossen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Fabric-service biedt verschillende manieren om de gezondheid van de geaggregeerde entiteiten:

- [Service Fabric Verkenner](service-fabric-visualizing-your-cluster.md) of andere hulpprogramma's voor visualisatie

- Query's voor de gezondheid (via PowerShell, de API- of overige)

- Algemene query's dat resultaat een lijst met entiteiten die gezondheid als een van de eigenschappen (via PowerShell, de API- of overige hebben)

U kunt deze opties zien, we gaan een lokale cluster met vijf knooppunten gebruiken. Naast de **fabric: / System** toepassing (die bestaat uit de doos), andere toepassingen worden geïmplementeerd. Een van deze toepassingen is **fabric: / WordCount**. Deze toepassing omvat een stateful geconfigureerd met zeven replica's. Omdat er slechts vijf knooppunten, geven de onderdelen van het systeem een waarschuwing weergegeven dat de partitie kleiner dan het aantal doel is.

```xml
<Service Name="WordCountService">
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Gezondheid in Service Fabric Explorer
Service Fabric Explorer levert een visuele weergave van het cluster. In de onderstaande afbeelding kunt u zien dat:

- De toepassing **fabric: / WordCount** is rood (fout) omdat er een foutgebeurtenis gemeld door de **MyWatchdog** voor de **beschikbaarheid**van de eigenschap.

- Een van haar diensten, **fabric: WordCount/WordCountService** is geel (in de waarschuwing). Sinds de service is geconfigureerd met zeven replica's, kunnen niet ze alle worden geplaatst, omdat er slechts vijf knooppunten. Hoewel deze wordt niet hier getoond, is door het rapport van de partitie service geel. De gele partitie activeert de gele service.

- Het cluster is vanwege de rode toepassing rood.

De evaluatie wordt standaardbeleid van het manifest van de cluster en het manifest van de toepassing gebruikt. Ze zijn strikte beleidsregels en doen een storing niet tolereren.

Weergave van de cluster-Service Fabric Explorer:

![Weergave van de cluster-Service Fabric Explorer.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [AZURE.NOTE] Lees meer over [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).

## <a name="health-queries"></a>Query's voor de gezondheid
Query's voor de gezondheid van stelt Fabric-service voor elk van de ondersteunde [Entiteitstypen](service-fabric-health-introduction.md#health-entities-and-hierarchy). Ze toegankelijk zijn via de API (de methoden kunnen u vinden op **FabricClient.HealthManager**), de PowerShell-cmdlets en de REST. Deze query's volledige gezondheidsinformatie ophalen over de entiteit: de geaggregeerde status, entiteit gezondheid gebeurtenissen, onderliggende statussen (indien van toepassing) en beschadigde evaluaties bij de entiteit niet in orde is.

> [AZURE.NOTE] Een entiteit gezondheid wordt geretourneerd wanneer deze volledig is gevuld, in het archief van de gezondheid. De entiteit moet actief zijn (niet verwijderd) en een rapport van het systeem. Ook moet de bovenliggende entiteiten in de keten hiërarchie systeemrapporten. Als een van deze voorwaarden niet is voldaan, retourneert de query's van de gezondheid van een uitzondering waarin wordt aangegeven waarom de entiteit niet wordt geretourneerd.

De gezondheid van query's moeten in de entiteit-id is afhankelijk van het entiteitstype doorgeven. De query accepteren parameters voor optionele gezondheid beleid. Als er geen statusbeleid worden opgegeven, wordt het [statusbeleid](service-fabric-health-introduction.md#health-policies) van het cluster of toepassing manifest worden gebruikt voor evaluatie. De query's accepteren ook filters voor het retourneren van alleen gedeeltelijke kinderen of gebeurtenissen--die met inachtneming van de opgegeven filters.

> [AZURE.NOTE] De output filters worden toegepast op de server, zodat het bericht beantwoorden wordt verkleind. Het is raadzaam dat u de uitvoerfilters gebruiken om de geretourneerde gegevens te beperken in plaats van filters toepassen op de client.

De gezondheid van de entiteit bevat:

- De samengevoegde status van de entiteit. Door de health winkel op basis van de entiteit, statusrapporten, onderliggende statussen (indien van toepassing) en statusbeleid wordt berekend. Lees meer over [entiteit gezondheid te kunnen beoordelen](service-fabric-health-introduction.md#entity-health-evaluation).  

- De gebeurtenissen van de gezondheid van de entiteit.

- De collectie van de Staten van de gezondheid van alle kinderen voor de entiteiten die kinderen kunnen hebben. De statussen bevatten entiteit-id's en de status van de geaggregeerde. Als u volledige gezondheid voor een kind, aanroepen van de gezondheid van de query voor het type van de onderliggende entiteit en in de id van het kind.

- De beschadigde evaluaties die naar het rapport dat de status van de entiteit verwijzen indien de entiteit niet in orde is.

## <a name="get-cluster-health"></a>Medische cluster
Geeft als resultaat de status van het cluster entiteit en bevat de statussen van toepassingen en knooppunten (kinderen van het cluster). Invoer:

- [Optioneel] Het statusbeleid voor cluster gebruikt voor het evalueren van de knooppunten en het cluster-servicegebeurtenissen.

- [Optioneel] De toepassing gezondheid beleid toewijzing, met het statusbeleid gebruikt voor het overschrijven van het manifest toepassingenbeleid.

- [Optioneel] Filters voor gebeurtenissen, knooppunten en toepassingen die welke posten opgeven van belang zijn en moeten worden opgenomen in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen, knooppunten en toepassingen worden gebruikt voor het evalueren van de gezondheid van de samengevoegde entiteit, onafhankelijk van het filter.

### <a name="api"></a>API
Als u cluster gezondheid, maken een `FabricClient` en de methode [GetClusterHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthasync.aspx) aangeroepen voor de **HealthManager**.

De volgende aanroep wordt de status van het cluster:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

De volgende code haalt de status van het cluster met behulp van een aangepaste cluster statusbeleid en filters voor knooppunten en toepassingen. [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthquerydescription.aspx), met de ingevoerde gegevens wordt gemaakt.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet om de gezondheid van het cluster is [Get-ServiceFabricClusterHealth](https://msdn.microsoft.com/library/mt125850.aspx). Eerst verbinding maken met het cluster via de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

De status van het cluster is vijf knooppunten de systeemtoepassing en fabric: / WordCount geconfigureerd zoals beschreven.

De volgende cmdlet haalt cluster gezondheid met behulp van standaard statusbeleid. De samengevoegde status in de waarschuwing is omdat het weefsel: / WordCount toepassing bevindt zich in de waarschuwing. U ziet hoe de beschadigde evaluaties bijzonderheden te vermelden over de voorwaarden die de gezondheid van de geaggregeerde geactiveerd.

```xml
PS C:\> Get-ServiceFabricClusterHealth

AggregatedHealthState   : Warning
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=false.


NodeHealthStates        :
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok

                          NodeName              : _Node_0
                          AggregatedHealthState : Ok

                          NodeName              : _Node_1
                          AggregatedHealthState : Ok

                          NodeName              : _Node_3
                          AggregatedHealthState : Ok

                          NodeName              : _Node_4
                          AggregatedHealthState : Ok

ApplicationHealthStates :
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok

                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning

HealthEvents            : None
```

De volgende PowerShell-cmdlet haalt de status van het cluster met behulp van een aangepaste toepassingenbeleid. Het filtert de resultaten als u alleen een fout of waarschuwing toepassingen en knooppunten. Als gevolg hiervan zijn geen knooppunten geretourneerd omdat ze allemaal in orde zijn. Alleen de fabric: / WordCount toepassing rekening wordt gehouden met het filteren van toepassingen. Omdat het aangepaste beleid rekening te houden met de waarschuwingen als fouten van het weefsel geeft: / WordCount toepassing, de toepassing wordt geëvalueerd als fout en dus is het cluster.

```powershell
PS c:\> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error"


AggregatedHealthState   : Error
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=true.


NodeHealthStates        : None
ApplicationHealthStates :
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error

HealthEvents            : None

```

### <a name="rest"></a>REST
U krijgt de cluster gezondheid met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707669.aspx) of een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707696.aspx) met statusbeleid dat wordt beschreven in de hoofdtekst.

## <a name="get-node-health"></a>Medische knooppunt
Deze eigenschap retourneert de status van een knooppunt entiteit en de gezondheid gebeurtenissen gemeld op het knooppunt bevat. Invoer:

- [Vereist] De knooppuntnaam dat het knooppunt aangeeft.

- [Optioneel] De cluster gezondheid instellingen gebruikt voor het evalueren van de gezondheid.

- [Optioneel] Filters voor gebeurtenissen die aangeven welke posten van belang zijn en moeten worden opgenomen in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen worden gebruikt voor het evalueren van de gezondheid van de samengevoegde entiteit, onafhankelijk van het filter.

### <a name="api"></a>API
Als u knooppunten gezondheid via de API, maken een `FabricClient` en de methode [GetNodeHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getnodehealthasync.aspx) aangeroepen voor de HealthManager.

De volgende code wordt de status van het knooppunt voor de naam van het opgegeven knooppunt:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

De volgende code wordt de status van het knooppunt voor de naam van het opgegeven knooppunt en passeert [NodeHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.nodehealthquerydescription.aspx)in gebeurtenissen filteren en aangepast beleid:

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet om de gezondheid van het knooppunt is een [Get-ServiceFabricNodeHealth](https://msdn.microsoft.com/library/mt125937.aspx). Eerst verbinding maken met het cluster via de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .
De volgende cmdlet wordt de status van het knooppunt met behulp van standaard statusbeleid:

```powershell
PS C:\> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 6
                        SentAt                : 3/22/2016 7:47:56 PM
                        ReceivedAt            : 3/22/2016 7:48:19 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:48:19 PM, LastWarning = 1/1/0001 12:00:00 AM
```

De volgende cmdlet wordt de status van alle knooppunten in het cluster:

```powershell
PS C:\> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_2                     Ok
_Node_0                     Ok
_Node_1                     Ok
_Node_3                     Ok
_Node_4                     Ok
```

### <a name="rest"></a>REST
U krijgt knooppunt gezondheid met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707650.aspx) of een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707665.aspx) met statusbeleid dat wordt beschreven in de hoofdtekst.

## <a name="get-application-health"></a>Toepassingsstatus ophalen
Geeft als resultaat de status van een Toepassingsentiteit. De statussen van de gedistribueerde toepassing en service kinderen bevat. Invoer:

- [Vereist] Naam van de toepassing (URI) waarmee de toepassing wordt geïdentificeerd.

- [Optioneel] Het statusbeleid voor toepassing gebruikt voor het overschrijven van het manifest toepassingenbeleid.

- [Optioneel] Filters voor gebeurtenissen, services en gedistribueerde toepassingen die welke posten aangeven van belang zijn en moeten worden opgenomen in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen, services en gedistribueerde toepassingen worden gebruikt voor het evalueren van de gezondheid van de samengevoegde entiteit, onafhankelijk van het filter.

### <a name="api"></a>API
Als u de toepassingsstatus, maken een `FabricClient` en de methode [GetApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getapplicationhealthasync.aspx) aangeroepen voor de HealthManager.

De volgende code wordt de gezondheid van de toepassing voor de opgegeven toepassingsnaam (URI):

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

De volgende code haalt de gezondheid van de toepassing voor de opgegeven toepassingsnaam (URI), met filters en een aangepast beleid opgegeven via [ApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.applicationhealthquerydescription.aspx).

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet de toepassingsstatus ophalen is [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx). Eerst verbinding maken met het cluster via de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

De volgende cmdlet retourneert de status van de **fabric: / WordCount** toepassing:

```powershell
PS c:\>
PS C:\WINDOWS\system32>  Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=false.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Ok
                                  SequenceNumber        : 131031545225930951
                                  SentAt                : 3/22/2016 9:08:42 PM
                                  ReceivedAt            : 3/22/2016 9:08:42 PM
                                  TTL                   : Infinite
                                  Description           : Availability checked successfully, latency ok
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 8:55:39 PM, LastWarning = 1/1/0001 12:00:00 AM
```

De volgende PowerShell-cmdlet wordt doorgegeven in het aangepaste beleid. Deze filters ook kinderen en gebeurtenissen.

```powershell
PS C:\> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=true.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>REST
U krijgt controle met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707681.aspx) of een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707643.aspx) met statusbeleid dat wordt beschreven in de hoofdtekst.

## <a name="get-service-health"></a>Medische service
Deze eigenschap retourneert de status van een service-entiteit. Het bevat de statussen van de partitie. Invoer:

- [Vereist] Naam van de service (URI) waarmee de service wordt geïdentificeerd.

- [Optioneel] Het statusbeleid voor toepassing gebruikt voor het overschrijven van het manifest toepassingenbeleid.

- [Optioneel] Filters voor gebeurtenissen en partities die aangeven welke posten van belang zijn en moeten worden opgenomen in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen en partities worden gebruikt voor het evalueren van de gezondheid van de samengevoegde entiteit, onafhankelijk van het filter.

### <a name="api"></a>API
Als u de status van de service via de API, maken een `FabricClient` en de methode [GetServiceHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getservicehealthasync.aspx) aangeroepen voor de HealthManager.

In het volgende voorbeeld wordt de status van een service met de naam van de opgegeven service (URI):

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

De volgende code wordt de gezondheid service voor de naam van de opgegeven service (URI), filters en aangepast beleid via [ServiceHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.servicehealthquerydescription.aspx)op te geven:

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet om de gezondheid van de service is een [Get-ServiceFabricServiceHealth](https://msdn.microsoft.com/library/mt125984.aspx). Eerst verbinding maken met het cluster via de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

De volgende cmdlet wordt de status van de service met behulp van standaard statusbeleid:

```powershell
PS C:\> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


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
                        SequenceNumber        : 131031547693687021
                        SentAt                : 3/22/2016 9:12:49 PM
                        ReceivedAt            : 3/22/2016 9:12:49 PM
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
```

### <a name="rest"></a>REST
U krijgt health service met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707609.aspx) of een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707646.aspx) met statusbeleid dat wordt beschreven in de hoofdtekst.

## <a name="get-partition-health"></a>Medische partitie
Geeft als resultaat de status van een partitie-eenheid. Het bevat de statussen van de replica. Invoer:

- [Vereist] De partitie-ID (GUID) dat de partitie wordt geïdentificeerd.

- [Optioneel] Het statusbeleid voor toepassing gebruikt voor het overschrijven van het manifest toepassingenbeleid.

- [Optioneel] Filters voor gebeurtenissen en replica's die aangeven welke posten van belang zijn en moeten worden opgenomen in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen en replica's worden gebruikt voor het evalueren van de gezondheid van de samengevoegde entiteit, onafhankelijk van het filter.

### <a name="api"></a>API
Als u de partitie gezondheid via de API, maken een `FabricClient` en de methode [GetPartitionHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getpartitionhealthasync.aspx) aangeroepen voor de HealthManager. Als u optionele parameters, [PartitionHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.partitionhealthquerydescription.aspx)te maken.

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShell
De cmdlet om de gezondheid van de partitie is [Get-ServiceFabricPartitionHealth](https://msdn.microsoft.com/library/mt125869.aspx). Eerst verbinding maken met het cluster via de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

De volgende cmdlet haalt de status voor alle partities van de **fabric: WordCount/WordCountService** service:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 131031502143040223
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844060
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844059
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844061
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844058
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 76
                        SentAt                : 3/22/2016 7:57:26 PM
                        ReceivedAt            : 3/22/2016 7:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Kun je de gezondheid van de partitie met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707683.aspx) of een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707680.aspx) met statusbeleid dat wordt beschreven in de hoofdtekst.

## <a name="get-replica-health"></a>Medische replica
Deze eigenschap retourneert de gezondheid van de replica van een stateful-service of een staatloze service-exemplaar. Invoer:

- [Vereist] De partitie-ID (GUID) en de replica-ID waarmee de replica.

- [Optioneel] De toepassing gezondheid beleidsparameters gebruikt voor het overschrijven van het manifest toepassingenbeleid.

- [Optioneel] Filters voor gebeurtenissen die aangeven welke posten van belang zijn en moeten worden opgenomen in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen worden gebruikt voor het evalueren van de gezondheid van de samengevoegde entiteit, onafhankelijk van het filter.

### <a name="api"></a>API
Als u de replica gezondheid via de API, maken een `FabricClient` en de methode [GetReplicaHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getreplicahealthasync.aspx) aangeroepen voor de HealthManager. Geavanceerde parameters, gebruikt u [ReplicaHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.replicahealthquerydescription.aspx).

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>PowerShell
De cmdlet om de gezondheid van de replica is een [Get-ServiceFabricReplicaHealth](https://msdn.microsoft.com/library/mt125808.aspx). Eerst verbinding maken met het cluster via de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

De volgende cmdlet wordt opgehaald voor de gezondheid van de primaire replica voor alle partities van de service:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
ReplicaId             : 131031502143040223
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131031502145556748
                        SentAt                : 3/22/2016 7:56:54 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
U krijgt gezondheid replica met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707673.aspx) of een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707641.aspx) met statusbeleid dat wordt beschreven in de hoofdtekst.

## <a name="get-deployed-application-health"></a>Gedistribueerde toepassing medische
Geeft als resultaat de status van een toepassing wordt geïmplementeerd op een knooppunt entiteit. De statussen van gedistribueerde service pakket bevat. Invoer:

- [Vereist] De toepassingsnaam (URI) en de knooppuntnaam (tekenreeks) die de gedistribueerde toepassing identificeren.

- [Optioneel] Het statusbeleid voor toepassing gebruikt voor het overschrijven van het manifest toepassingenbeleid.

- [Optioneel] Filters voor gebeurtenissen en servicepakketten voor gedistribueerde die aangeven welke posten van belang zijn en moeten worden opgenomen in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen en gedistribueerde servicepakketten worden gebruikt voor de evaluatie van de gezondheid van de samengevoegde entiteit, onafhankelijk van het filter.

### <a name="api"></a>API
Als u de status van een toepassing op een knooppunt via de API is geïmplementeerd, maken een `FabricClient` en de methode [GetDeployedApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync.aspx) aangeroepen voor de HealthManager. Optionele parameters, gebruikt u [DeployedApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedapplicationhealthquerydescription.aspx).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShell
Ophalen van de gezondheid van de gedistribueerde toepassing de cmdlet is [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx). Eerst verbinding maken met het cluster via de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) . Als u wilt weten wanneer een toepassing wordt geïmplementeerd, [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) uitvoeren en kijk naar de kinderen van de gedistribueerde toepassing.

De volgende cmdlet haalt de gezondheid van de **fabric: / WordCount** toepassing op **_Node_2**geïmplementeerd.

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_2


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_2
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

                                     ServiceManifestName   : WordCountWebServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131031502143710698
                                     SentAt                : 3/22/2016 7:56:54 PM
                                     ReceivedAt            : 3/22/2016 7:57:12 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Kun je de gezondheid van de gedistribueerde toepassing met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707644.aspx) of een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707688.aspx) met statusbeleid dat wordt beschreven in de hoofdtekst.

## <a name="get-deployed-service-package-health"></a>Gedistribueerde service pakket medische
Geeft als resultaat de status van een service voor gedistribueerde pakket entiteit. Invoer:

- [Vereist] De toepassingsnaam (URI), knooppuntnaam (tekenreeks) en manifest servicenaam (tekenreeks) die het servicepakket van de gedistribueerde te identificeren.

- [Optioneel] Het statusbeleid voor toepassing gebruikt voor het overschrijven van het manifest toepassingenbeleid.

- [Optioneel] Filters voor gebeurtenissen die aangeven welke posten van belang zijn en moeten worden opgenomen in het resultaat (bijvoorbeeld alleen, fouten of waarschuwingen en fouten). Alle gebeurtenissen worden gebruikt voor het evalueren van de gezondheid van de samengevoegde entiteit, onafhankelijk van het filter.

### <a name="api"></a>API
Als u de status van een pakket van de gedistribueerde service via de API, maken een `FabricClient` en de methode [GetDeployedServicePackageHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync.aspx) aangeroepen voor de HealthManager. Optionele parameters, gebruikt u [DeployedServicePackageHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedservicepackagehealthquerydescription.aspx).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShell
De cmdlet om de gezondheid van gedistribueerde service pakket is [Get-ServiceFabricDeployedServicePackageHealth](https://msdn.microsoft.com/library/mt163525.aspx). Eerst verbinding maken met het cluster via de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) . Om te zien waarin een toepassing wordt geïmplementeerd, [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) worden uitgevoerd en bekijk de gebruikte toepassingen. Bekijken om te zien welke pakketten in een toepassing zijn service, de service geïmplementeerde pakket kinderen in de uitvoer van de [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx) .

De volgende cmdlet haalt de gezondheid van de **WordCountServicePkg** -servicepakket van de **fabric: / WordCount** toepassing op **_Node_2**geïmplementeerd. De entiteit heeft **System.Hosting** rapporten voor succesvolle service-pakket en ingangspunt activering en registratie voltooid type service.

```powershell
PS C:\> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : _Node_2
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 131031502301306211
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 131031502301568982
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 131031502314788519
                        SentAt                : 3/22/2016 7:57:11 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
U krijgt service geïmplementeerde pakket gezondheid met een [GET-verzoek](https://msdn.microsoft.com/library/azure/dn707677.aspx) of een [POST-aanvraag](https://msdn.microsoft.com/library/azure/dn707689.aspx) met statusbeleid dat wordt beschreven in de hoofdtekst.

## <a name="health-chunk-queries"></a>Gezondheid chunk query 's
De gezondheid chunk query's kunnen retourneren met meerdere niveaus cluster kinderen (recursief), per invoerfilters. Biedt ondersteuning voor geavanceerde filters waardoor veel flexibiliteit Express welke specifieke kinderen moet worden geretourneerd, aangeduid met hun unieke id of andere groeps-id en/of status. Standaard zijn geen onderliggende elementen opgenomen, in plaats van gezondheid-opdrachten die eerste niveau kinderen altijd opnemen.

De [query's voor de gezondheid](service-fabric-view-entities-aggregated-health.md#health-queries) terug alleen onderliggende eerste niveau van de opgegeven entiteit per filters vereist. Als u de kinderen van de kinderen, moet u extra gezondheid API's aanroepen voor elke entiteit van belang. Op dezelfde manier als u de gezondheid van de specifieke entiteiten, moet u voor elke gewenste entiteit een gezondheid API aanroepen. Chunk kunt geavanceerde filters u de query voor het aanvragen van meerdere items in een query, de berichtgrootte en het aantal berichten beperken van belang.

De waarde van de query deel is dat u kunt status voor meer cluster entiteiten (mogelijk alle cluster entiteiten vanaf vereist root) in één aanroep. U kunt de gezondheid van complexe query, zoals express:

- Toepassingen op een fout, en voor deze toepassingen bevatten alle services op de waarschuwing terug | fout. Voor geretourneerde services, inclusief alle partities.

- Alleen de gezondheid van de 4-toepassingen, die door hun namen als resultaat.

- Alleen de gezondheid van de toepassingen van een type van de gewenste toepassing als resultaat.

- Alle gebruikte diensten terug op een knooppunt. Geeft als resultaat alle toepassingen, alle toepassingen op het opgegeven knooppunt en alle servicepakketten voor gedistribueerde op dat knooppunt geïmplementeerd.

- Alle replica's in een fout als resultaat. Toepassingen, services, partities en replica's op een fout geretourneerd.

- Retourneren van alle toepassingen. Voor een opgegeven service, inclusief alle partities.

Op dit moment is de gezondheid chunk query alleen voor de cluster-entiteit blootgesteld. Het resultaat een cluster gezondheid deel, waarin:

- De status van het cluster die worden samengevoegd.

- De gezondheid staat chunk lijst met knooppunten die invoerfilters respecteren.

- De gezondheid staat chunk lijst met toepassingen die invoerfilters respecteren. Elke toepassing gezondheid staat segment bevat een lijst segment met alle services die invoerfilters en een segment lijst met alle gebruikte toepassingen die met inachtneming van de filters voldoen. Hetzelfde voor de kinderen van services en gedistribueerde toepassingen. Op deze manier alle entiteiten in het cluster kunnen worden mogelijk geretourneerd als dit wordt gevraagd op een hiërarchische manier.

### <a name="cluster-health-chunk-query"></a>Cluster gezondheid chunk query
Geeft als resultaat de status van het cluster entiteit en de stukken hiërarchische gezondheid staat vereiste onderliggende elementen bevat. Invoer:

- [Optioneel] Het statusbeleid voor cluster gebruikt voor het evalueren van de knooppunten en het cluster-servicegebeurtenissen.

- [Optioneel] De toepassing gezondheid beleid toewijzing, met het statusbeleid gebruikt voor het overschrijven van het manifest toepassingenbeleid.

- [Optioneel] Filters voor knooppunten en toepassingen die aangeven welke posten van belang zijn en moeten worden geretourneerd in de resultaatset. De filters zijn specifiek voor een entiteit of groep van entiteiten of zijn van toepassing op alle entiteiten op dat niveau. De lijst met filters kan bevatten een algemeen filter en/of filters voor specifieke id's met fijne korrel entiteiten die door de query. Als u niets opgeeft, worden de kinderen niet standaard geretourneerd.
Lees meer over de filters op [NodeHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.nodehealthstatefilter.aspx) en [ApplicationHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthstatefilter.aspx). De toepassing filters kan recursief geeft geavanceerde filters voor kinderen.

Het resultaat van het segment omvat de kinderen die met inachtneming van de filters.

De query chunk terug op dit moment niet beschadigd evaluaties of entiteit gebeurtenissen. Deze extra informatie kan worden verkregen met behulp van de bestaande cluster gezondheid query.

### <a name="api"></a>API
Als u cluster gezondheid segment, maakt u een `FabricClient` en de methode [GetClusterHealthChunkAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync.aspx) aangeroepen voor de **HealthManager**. U kunt doorgeven in de [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthchunkquerydescription.aspx) voor het beschrijven van statusbeleid en geavanceerde filters.

De volgende code haalt cluster gezondheid segment met geavanceerde filters.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except the ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
De cmdlet om de gezondheid van het cluster is [Get-ServiceFabricClusterChunkHealth](https://msdn.microsoft.com/library/mt644772.aspx). Eerst verbinding maken met het cluster via de cmdlet [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) .

De volgende code haalt knooppunten alleen als ze in fout, met uitzondering van een bepaald knooppunt, moet altijd worden geretourneerd.

```xml
PS C:\> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in the cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters

HealthState                  : Error
NodeHealthStateChunks        :
                               TotalCount            : 1

                               NodeName              : _Node_1
                               HealthState           : Ok

ApplicationHealthStateChunks : None
```

De volgende cmdlet haalt cluster segment met toepassingsfilters.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters

HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 1

                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks :
                                   TotalCount            : 1

                                   ServiceName           : fabric:/WordCount/WordCountService
                                   HealthState           : Error
                                   PartitionHealthStateChunks :
                                       TotalCount            : 1

                                       PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                                       HealthState           : Error
                                       ReplicaHealthStateChunks :
                                           TotalCount            : 5

                                           ReplicaOrInstanceId   : 131031502143040223
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844060
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844059
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844061
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844058
                                           HealthState           : Error
```

De volgende cmdlet retourneert alle geïmplementeerde entiteiten op een knooppunt.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 2

                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 1

                                       ServiceManifestName   : FAS
                                       HealthState           : Ok



                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 2

                                       ServiceManifestName   : WordCountServicePkg
                                       HealthState           : Ok

                                       ServiceManifestName   : WordCountWebServicePkg
                                       HealthState           : Ok
```

### <a name="rest"></a>REST
U kunt cluster gezondheid segment met een [GET-verzoek](https://msdn.microsoft.com/library/azure/mt656722.aspx) of een [POST-aanvraag](https://msdn.microsoft.com/library/azure/mt656721.aspx) met statusbeleid en geavanceerde filters die worden beschreven in de hoofdtekst.

## <a name="general-queries"></a>Algemene query 's
Algemene query's als resultaat een lijst van de diensten van een bepaald type Service Fabric. Ze worden via de API (via de methoden voor **FabricClient.QueryManager**), de PowerShell-cmdlets en de REST. Subquery's samenvoegen deze query's uit meerdere onderdelen. Een van hen is de [gezondheid slaan](service-fabric-health-introduction.md#health-store), die de geaggregeerde status voor elk resultaat van de query worden ingevuld.  

> [AZURE.NOTE] Algemene query's retourneren de geaggregeerde status van de entiteit en bevatten geen opmaak gezondheidsgegevens. Als een entiteit niet in orde is, kunt u met query's voor de gezondheid van alle haar gezondheid informatie, met inbegrip van gebeurtenissen, onderliggende statussen en beschadigde evaluaties opvolgen.

Als algemene query's voor een entiteit een onbekende status retourneren, is het mogelijk dat de gezondheid winkel geen volledige gegevens over de entiteit. Het is ook mogelijk dat een subquery aan het archief van de gezondheid niet succesvol is (bijvoorbeeld, er is een communicatiefout opgetreden of de snelheid van het archief van de gezondheid). Opvolgen met een query gezondheid voor de entiteit. Als de subquery tijdelijke fouten, zoals de netwerkverbinding aangetroffen, kan deze opvolging query mislukt. Ook kan geven u meer details uit het archief van de gezondheid over waarom de entiteit niet beschikbaar is.

De query's met **HealthState** voor entiteiten zijn:

- Knooppunten: de lijst met knooppunten in het cluster (paged) als resultaat gegeven.
  - API: [FabricClient.QueryClient.GetNodeListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getnodelistasync.aspx)
  - PowerShell: Get-ServiceFabricNode
- Lijst met toepassingen: de lijst met toepassingen in het cluster (paged) als resultaat gegeven.
  - API: [FabricClient.QueryClient.GetApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getapplicationlistasync.aspx)
  - PowerShell: Get-ServiceFabricApplication
- Lijst met Services: geeft als resultaat de lijst met services in een toepassing (wisselbaar geheugen).
  - API: [FabricClient.QueryClient.GetServiceListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getservicelistasync.aspx)
  - PowerShell: Get-ServiceFabricService
- Partitielijst: de lijst met partities in een service (paged) als resultaat gegeven.
  - API: [FabricClient.QueryClient.GetPartitionListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getpartitionlistasync.aspx)
  - PowerShell: Get-ServiceFabricPartition
- Replicalijst: geeft als resultaat de lijst van replica's in een partitie (wisselbaar geheugen).
  - API: [FabricClient.QueryClient.GetReplicaListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getreplicalistasync.aspx)
  - PowerShell: Get-ServiceFabricReplica
- Lijst met toepassingen geïmplementeerd: de lijst van gedistribueerde toepassingen op een knooppunt als resultaat gegeven.
  - API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync.aspx)
  - PowerShell: Get-ServiceFabricDeployedApplication
- De pakketlijst service geïmplementeerd: de lijst met servicepakketten retourneert in een gedistribueerde toepassing.
  - API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync.aspx)
  - PowerShell: Get-ServiceFabricDeployedApplication

> [AZURE.NOTE] Sommige van de query's resultaten wisselbare. De terugkeer van deze query's is een lijst die is afgeleid van [PagedList<T>](https://msdn.microsoft.com/library/azure/mt280056.aspx). De resultaten een bericht niet passen, alleen op een pagina wordt geretourneerd als een ContinuationToken die worden bijgehouden waar de opsomming gestopt. U moet doorgaan met aanroepen van dezelfde query en doorgeven in het token van de voortzetting van de vorige query ophalen van de volgende resultaten.

### <a name="examples"></a>Voorbeelden

De volgende code wordt de beschadigde toepassingen in het cluster:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

De volgende cmdlet haalt de Toepassingdetails voor de fabric: / WordCount toepassing. Merkt op dat de status op de waarschuwing.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

De volgende cmdlet haalt de diensten met een status van waarschuwing:

```powershell
PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Warning"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Warning
```

## <a name="cluster-and-application-upgrades"></a>Cluster- en upgrades
Bij een gecontroleerde upgrade van de cluster en de toepassing controleert Fabric Service status om ervoor te zorgen dat alles in orde blijft. Als een entiteit beschadigd is als geëvalueerd met behulp van geconfigureerde statusbeleid, past de upgrade upgrade-specifiek beleid om de volgende actie vast te stellen. De upgrade kan worden onderbroken als u wilt dat de interactie van de gebruiker (zoals de vaststelling van de fout of het beleid wijzigen) of het mogelijk automatisch terugkeren naar de vorige versie van de goede.

Tijdens het upgraden van een *cluster* , kunt u de bijwerkstatus van de cluster te krijgen. De upgrade-status bevat beschadigde evaluaties die verwijzen naar wat in het cluster beschadigd is. Als de upgrade wordt hersteld door gezondheid problemen, onthoudt de upgradestatus van de laatste redenen beschadigd. Deze informatie kunt beheerders onderzoeken wat er mis is gegaan nadat de upgrade is hersteld of is gestopt.

Ook tijdens een upgrade van de *toepassing* , een beschadigde evaluaties staan in de bijwerkstatus van toepassing.

Hieronder vindt u de bijwerkstatus van toepassing voor een gewijzigde fabric: / WordCount toepassing. Een watchdog heeft een fout gemeld op een van de replica's. De upgrade is rolling omdat de controles niet worden nageleefd.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2015 5:23:26 PM
FailureTimestampUtc           : 4/21/2015 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

Lees meer over de [Service Fabric toepassing bijwerken](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-to-troubleshoot"></a>Gezondheid evaluaties gebruiken om problemen met
Wanneer er een probleem met het cluster of een toepassing wordt gebruikt, bekijkt u de cluster- of gezondheid te achterhalen wat er mis is. De beschadigde beoordelingen vindt informatie over wat de huidige status van de beschadigde geactiveerd. Als u wilt, kunt u inzoomen in beschadigde onderliggende entiteiten voor het identificeren van de oorzaak.

> [AZURE.NOTE] De beschadigde evaluaties tonen dat de eerste reden de entiteit is geëvalueerd voor de huidige status. Kunnen er meerdere andere gebeurtenissen die deze status te activeren, maar ze worden niet doorgevoerd in de evaluaties. Als u meer informatie, dieper de gezondheid entiteiten om erachter te komen wat de netwerkverbindingsmogelijkheden ervan rapporten in het cluster.

## <a name="next-steps"></a>Volgende stappen
[Gebruik system health rapporten oplossen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Aangepaste Service Fabric statusrapporten toevoegen](service-fabric-report-health.md)

[Het rapport en de service controleren](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Controleren en diagnosticeren lokaal services](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric toepassing upgrade](service-fabric-application-upgrade.md)

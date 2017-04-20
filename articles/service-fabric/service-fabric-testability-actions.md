<properties
   pageTitle="Testbaarheid actie | Microsoft Azure"
   description="In dit artikel gesprekken voert over de testbaarheid acties gevonden in Microsoft Azure Service Fabric."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="timlt"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/03/2016"
   ms.author="motanv;heeldin"/>

# <a name="testability-actions"></a>Testbaarheid acties
Te simuleren een onbetrouwbare infrastructuur, biedt Azure Service Fabric u de ontwikkelaar, manieren voor het simuleren van verschillende real-world storingen en overgangen staat. Deze zijn beschikbaar als testbaarheid acties. De acties zijn de low-level API's die ervoor zorgen een specifieke fout injectie, validatie of overgang staat dat. Door de combinatie van deze acties kunt u uitgebreide Testscenario's schrijven voor de services.

Fabric-service biedt dat enkele algemene scenario's voor test bestaat uit deze acties. We raden het gebruik van deze ingebouwde scenario's zorgvuldig gekozen zijn gemeenschappelijke staat overgangen en mislukte aanvragen te testen. Acties kunnen echter aangepaste Testscenario's maken als u wilt toevoegen van dekking voor scenario's die niet vallen onder de ingebouwde scenario's nog of die zijn aangepast op maat gemaakt voor uw toepassing worden gebruikt.

C#-implementaties van de acties worden in de assembly System.Fabric.dll gevonden. De module systeem Fabric PowerShell is gevonden in de assembly Microsoft.ServiceFabric.Powershell.dll. De ServiceFabric PowerShell-module is geïnstalleerd als onderdeel van de runtime-installatie, zodat voor gebruiksgemak.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Correcte versus geforceerde afsluiting veroorzaakt acties
Testbaarheid acties worden ingedeeld in twee grote buckets:

* Geforceerde afsluiting fouten: deze storingen simuleren fouten als de computer wordt opnieuw opgestart en crashes te verwerken. In dergelijke gevallen van storingen in de uitvoeringscontext van proces abrupt. Dit betekent dat er geen opschoning van de staat kan worden uitgevoerd voordat de toepassing opnieuw is gestart.

* Correcte fouten: deze storingen simuleren correcte acties zoals replica verplaatst en druppels geactiveerd door Netwerktaakverdeling. In dergelijke gevallen wordt de service krijgt een melding van het sluiten en de status kunt opschonen voordat u afsluit.

Voor de validatie van betere kwaliteit, uitvoeren, de werkbelasting service en business terwijl verschillende correcte en geforceerde afsluiting fouten veroorzaken. Geforceerde afsluiting fouten oefenen scenario's waar het serviceproces wordt abrupt in het midden van een werkstroom afgesloten. Hiermee test u het herstelpad zodra de service replica is hersteld door Fabric-Service. Dit helpt consistentie van de gegevens en of de status van de service goed is onderhouden na storingen testen. De andere reeks storingen (correcte fouten)-test die de service correct op replica's wordt verplaatst reageert op door de Service Fabric. Deze test behandeling van annulering in de methode RunAsync. De service moet controleren of het token van de annulering wordt ingesteld, wordt de staat de juiste manier opslaan en sluiten van de methode RunAsync.

## <a name="testability-actions-list"></a>Lijst van acties testbaarheid

| Actie | Beschrijving | Beheerde API | PowerShell-cmdlet | Correcte/geforceerde afsluiting fouten |
|---------|-------------|-------------|-------------------|------------------------------|
|CleanTestState| Verwijdert alle test-status van het cluster in het geval van een onjuist afsluiten van het stuurprogramma voor de test. | CleanTestStateAsync | Verwijderen ServiceFabricTestState | Niet van toepassing |
| InvokeDataLoss | Verlies van gegevens induceert in een partitie service. | InvokeDataLossAsync | Aanroepen van ServiceFabricPartitionDataLoss | Correcte |
| InvokeQuorumLoss | Een bepaalde dienst van de stateful-partitie wordt in quorum verloren gaan. | InvokeQuorumLossAsync | Aanroepen van ServiceFabricQuorumLoss | Correcte |
| Primaire verplaatsen | De opgegeven primaire replica van een stateful-service verplaatst naar het opgegeven knooppunt. | MovePrimaryAsync | Verplaatsen ServiceFabricPrimaryReplica | Correcte |
| Secundaire verplaatsen | De huidige secundaire replica van een stateful service naar een ander clusterknooppunt worden verplaatst. | MoveSecondaryAsync | Verplaatsen ServiceFabricSecondaryReplica | Correcte |
| RemoveReplica | Simuleert een replica storing door het verwijderen van een replica van een cluster. Dit de replica wordt afgesloten en wordt deze overgang naar de rol van 'Geen', verwijderen van alle van de status van het cluster. | RemoveReplicaAsync | Verwijderen ServiceFabricReplica | Correcte |
| RestartDeployedCodePackage | Een fout in het pakket code gesimuleerd door een pakket code is geïmplementeerd op een knooppunt in een cluster opnieuw te starten. Dit breekt het proces pakket code, die alle gebruikers service replica's die worden gehost in het proces opnieuw gestart. | RestartDeployedCodePackageAsync | Opnieuw ServiceFabricDeployedCodePackage | Geforceerde afsluiting |
| RestartNode | Een storing in het cluster Service Fabric knooppunt gesimuleerd door een knooppunt opnieuw te starten. | RestartNodeAsync | Opnieuw ServiceFabricNode | Geforceerde afsluiting |
| RestartPartition | Simuleert een datacenter onleesbaar of cluster stroomonderbreking scenario door sommige of alle replica's van een partitie opnieuw te starten. | RestartPartitionAsync | Opnieuw ServiceFabricPartition | Correcte |
| RestartReplica | Simuleert een replica storing door een permanente replica opnieuw te starten in een cluster, de replica wordt gesloten en vervolgens opnieuw te openen. | RestartReplicaAsync | Opnieuw ServiceFabricReplica | Correcte |
| Beginknooppunt | Hiermee start u een knooppunt in een cluster dat al is gestopt. | StartNodeAsync | Start ServiceFabricNode | Niet van toepassing |
| StopNode | Simuleert u een storing in het knooppunt door het stoppen van een knooppunt in een cluster. Het knooppunt blijft ingedrukt totdat het beginknooppunt wordt genoemd. | StopNodeAsync | Stop ServiceFabricNode | Geforceerde afsluiting |
| ValidateApplication | Valideert de beschikbaarheid en de gezondheid van alle configuratieservices Service binnen een toepassing, gewoonlijk na enkele storing in het systeem te induceren. | ValidateApplicationAsync | Test ServiceFabricApplication | Niet van toepassing |
| ValidateService | De beschikbaarheid en de status van een service Service Fabric valideert meestal na enkele storing in het systeem te induceren. | ValidateServiceAsync | Test ServiceFabricService | Niet van toepassing |

## <a name="running-a-testability-action-using-powershell"></a>Uitvoeren van een testbaarheid actie met PowerShell

In deze zelfstudie wordt beschreven hoe u een actie testbaarheid uitvoeren met behulp van PowerShell. Hier leert u hoe u een testbaarheid actie uitvoeren op een cluster met lokale (-in-één) of een Azure-cluster. Microsoft.Fabric.Powershell.dll--de Service Fabric PowerShell-module wordt automatisch geïnstalleerd tijdens de installatie van de Microsoft-Service Fabric MSI. De module wordt automatisch geladen bij het openen van een PowerShell-prompt.

Zelfstudie segmenten:

- [Een actie uitvoeren voor een cluster-in-één](#run-an-action-against-a-one-box-cluster)
- [Een actie uitvoeren voor een cluster Azure](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Een actie uitvoeren voor een cluster-in-één

Een testbaarheid om actie te voeren tegen een lokale cluster, eerst verbinding maken met het cluster en open de PowerShell-prompt in de administrator-modus. We bekijken de actie **Opnieuw ServiceFabricNode** .

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Hier wordt de actie **Opnieuw ServiceFabricNode** uitgevoerd op een knooppunt met de naam "Knooppunt1". De modus voltooiing geeft aan dat deze niet controleren moet of het knooppunt opnieuw actie werkelijk geslaagd. De modus is voltooid op te geven als 'Of' gaan om te controleren of de actie opnieuw daadwerkelijk met succes voltooid. In plaats van het knooppunt direct door de naam op te geven, kunt u dit aangeven via de partitiesleutel van een en de aard van de replica, als volgt:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Opnieuw ServiceFabricNode** moet worden gebruikt voor het opnieuw starten van een Service Fabric-knooppunt in een cluster. Hiermee stopt u het proces van Fabric.exe, die alle de systeem Services en service replica's die worden gehost op dat knooppunt opnieuw opstarten. Met deze API voor het testen van uw service helpt fouten langs de paden van failover-herstel aan het licht komen. Dit helpt storingen in knooppunten in het cluster te simuleren.

De volgende schermafbeelding ziet u de opdracht **Opnieuw ServiceFabricNode** testbaarheid in actie.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

De uitvoer van de eerste **Get-ServiceFabricNode** (een cmdlet uit de module Service Fabric PowerShell) laat zien dat het lokale cluster vijf knooppunten heeft: Node.1 naar Node.5. Nadat de actie testbaarheid (cmdlet) **Opnieuw ServiceFabricNode** wordt uitgevoerd op het knooppunt met de naam Node.4, zien we dat de uptime van het knooppunt opnieuw is ingesteld.

### <a name="run-an-action-against-an-azure-cluster"></a>Een actie uitvoeren voor een cluster Azure

Uitvoeren van een actie testbaarheid (via PowerShell) tegen een Azure cluster is vergelijkbaar met het uitvoeren van de actie tegen een lokale cluster. Het enige verschil is dat voordat u de actie, in plaats van een verbinding met het lokale cluster kunt uitvoeren moet u eerst verbinding maakt met het cluster Azure.

## <a name="running-a-testability-action-using-c35"></a>Uitvoeren van een actie testbaarheid is met C & #35;

Een testbaarheid om actie te voeren met behulp van C#, moet u eerst verbinding maken met het cluster via FabricClient. Moet u de parameters die nodig zijn voor het uitvoeren van de actie. Andere parameters kunnen worden gebruikt om dezelfde actie uitvoeren.
De actie RestartServiceFabricNode kijken, is een manier uit te voeren met behulp van de knooppuntgegevens (knooppuntnaam en knooppunt exemplaar-ID) in het cluster.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Parameter uitleg:

- **CompleteMode** geeft aan dat de modus niet controleren moet of de actie opnieuw daadwerkelijk met succes voltooid. De modus is voltooid op te geven als 'Of' gaan om te controleren of de actie opnieuw daadwerkelijk met succes voltooid.  
- **OperationTimeout** bepaalt de hoeveelheid tijd die voor de bewerking is voltooid voordat u een uitzondering TimeoutException wordt gegenereerd.
- **CancellationToken** kunnen een in behandeling zijnde aanroep worden geannuleerd.

In plaats van het knooppunt direct door de naam op te geven, kunt u deze via de partitiesleutel van een en de aard van de replica.

Voor meer informatie, Zie [PartitionSelector en ReplicaSelector](#partition_replica_selector).


```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>PartitionSelector en ReplicaSelector

### <a name="partitionselector"></a>PartitionSelector
PartitionSelector is een helper in testbaarheid blootgesteld en wordt gebruikt voor een specifieke partitie selecteren waarin de testbaarheid acties uitvoeren. Het kan worden gebruikt om een specifieke partitie selecteren als de partitie-ID vooraf bekend is. Of kunt u de partitiesleutel opgeven en de bewerking wordt opgelost in de partitie-ID intern. U hebt ook de mogelijkheid een willekeurige partitie te selecteren.

Het PartitionSelector-object maken en selecteert u de partitie met behulp van een van de methoden selecteren * voor het gebruik van dit hulpprogramma. Laat vervolgens in het PartitionSelector-object aan de API dat vereist is. Als er geen optie is geselecteerd, wordt standaard een willekeurige partitie.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>ReplicaSelector
ReplicaSelector is een helper in testbaarheid blootgesteld en wordt gebruikt om een replica waarvoor de testbaarheid acties selecteren. Het kan worden gebruikt voor het selecteren van een specifieke replicaset als de replica-ID vooraf bekend is. U hebt bovendien een primaire replica of een willekeurige secundair kiezen. ReplicaSelector is afgeleid van PartitionSelector, dus u moet de replica en de partitie waarop u wilt uitvoeren van de bewerking testbaarheid selecteren.

Voor het gebruik van dit hulpprogramma, een ReplicaSelector-object maken en instellen, kunt u de replica en de partitie te selecteren. U kunt deze vervolgens doorgeven in de API die vereist is. Als er geen optie is geselecteerd, wordt standaard een willekeurige replica en willekeurige partitie.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Volgende stappen

- [Testbaarheid scenario 's](service-fabric-testability-scenarios.md)
- Het testen van uw service
   - [Storingen simuleren tijdens service werkbelasting](service-fabric-testability-workload-tests.md)
   - [Fouten in Services](service-fabric-testability-scenarios-service-communication.md)

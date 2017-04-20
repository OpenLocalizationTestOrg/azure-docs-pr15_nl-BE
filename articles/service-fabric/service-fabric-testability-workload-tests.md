<properties
   pageTitle="Aangepaste Testscenario's | Microsoft Azure"
   description="Hoe de beveiliging van uw diensten tegen storingen van correcte en geforceerde afsluiting."
   services="service-fabric"
   documentationCenter=".net"
   authors="anmolah"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="anmola"/>

# <a name="simulate-failures-during-service-workloads"></a>Storingen simuleren tijdens service werkbelasting

De scenario's voor testbaarheid in Azure Service Fabric kunnen ontwikkelaars geen zorgen te maken over het omgaan met individuele fouten. Er zijn scenario's, maar waar een expliciete interleaving van client werkbelasting en storingen kan nodig zijn. De werkbelasting van de client en fouten interleaving zorgt ervoor dat de service daadwerkelijk een actie wordt uitgevoerd als fout gebeurt. Gezien de mate van controle die testbaarheid biedt, kunnen deze worden op exacte punten van de uitvoering van de werkbelasting. Deze inductie van fouten in de verschillende lidstaten in de toepassing kunt vinden fouten en de kwaliteit te verbeteren.

## <a name="sample-custom-scenario"></a>Aangepaste voorbeeldscenario
Deze test geeft een scenario waarbij de werkbelasting business met [geforceerde afsluiting-en correcte](service-fabric-testability-actions.md#graceful-vs-ungraceful-fault-actions)interleaves. In het midden van de service-activiteiten of berekenen voor de beste resultaten moeten de fouten worden geïnduceerd.

Doorlopen we een voorbeeld van een service die vier werklasten beschrijft: A, B, C en D. elke correspondeert met een set van werkstromen en compute, opslag of een combinatie. Omwille van de eenvoud zullen we de werkbelasting gelijkmatig verdeeld abstracte in ons voorbeeld. De verschillende fouten in het volgende voorbeeld wordt uitgevoerd, zijn:
  + RestartNode: Geforceerde afsluiting fouttolerantie voor het simuleren van de computer opnieuw op te starten.
  + RestartDeployedCodePackage: Geforceerde afsluiting veroorzaakt service hostproces te simuleren loopt vast.
  + RemoveReplica: Correcte storing simuleren replica verwijderen.
  + MovePrimary: Correcte fouttolerantie voor het simuleren van replica wordt geactiveerd door de Service Fabric taakverdeling.

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll.

using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        // Replace these strings with the actual version for your cluster and application.
        string clusterConnection = "localhost:19000";
        Uri applicationName = new Uri("fabric:/samples/PersistentToDoListApp");
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Workload Test...");
        try
        {
            RunTestAsync(clusterConnection, applicationName, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Workload Test failed: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Workload Test completed successfully.");
        return 0;
    }

    public enum ServiceWorkloads
    {
        A,
        B,
        C,
        D
    }

    public enum ServiceFabricFaults
    {
        RestartNode,
        RestartCodePackage,
        RemoveReplica,
        MovePrimary,
    }

    public static async Task RunTestAsync(string clusterConnection, Uri applicationName, Uri serviceName)
    {
        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);
        // Maximum time to wait for a service to stabilize.
        TimeSpan maxServiceStabilizationTime = TimeSpan.FromSeconds(120);

        // How many loops of faults you want to execute.
        uint testLoopCount = 20;
        Random random = new Random();

        for (var i = 0; i < testLoopCount; ++i)
        {
            var workload = SelectRandomValue<ServiceWorkloads>(random);
            // Start the workload.
            var workloadTask = RunWorkloadAsync(workload);

            // While the task is running, induce faults into the service. They can be ungraceful faults like
            // RestartNode and RestartDeployedCodePackage or graceful faults like RemoveReplica or MovePrimary.
            var fault = SelectRandomValue<ServiceFabricFaults>(random);

            // Create a replica selector, which will select a primary replica from the given service to test.
            var replicaSelector = ReplicaSelector.PrimaryOf(PartitionSelector.RandomOf(serviceName));
            // Run the selected random fault.
            await RunFaultAsync(applicationName, fault, replicaSelector, fabricClient);
            // Validate the health and stability of the service.
            await fabricClient.ServiceManager.ValidateServiceAsync(serviceName, maxServiceStabilizationTime);

            // Wait for the workload to finish successfully.
            await workloadTask;
        }
    }

    private static async Task RunFaultAsync(Uri applicationName, ServiceFabricFaults fault, ReplicaSelector selector, FabricClient client)
    {
        switch (fault)
        {
            case ServiceFabricFaults.RestartNode:
                await client.ClusterManager.RestartNodeAsync(selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RestartCodePackage:
                await client.ApplicationManager.RestartDeployedCodePackageAsync(applicationName, selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RemoveReplica:
                await client.ServiceManager.RemoveReplicaAsync(selector, CompletionMode.Verify, false);
                break;
            case ServiceFabricFaults.MovePrimary:
                await client.ServiceManager.MovePrimaryAsync(selector.PartitionSelector);
                break;
        }
    }

    private static Task RunWorkloadAsync(ServiceWorkloads workload)
    {
        throw new NotImplementedException();
        // This is where you trigger and complete your service workload.
        // Note that the faults induced while your service workload is running will
        // fault the primary service. Hence, you will need to reconnect to complete or check
        // the status of the workload.
    }

    private static T SelectRandomValue<T>(Random random)
    {
        Array values = Enum.GetValues(typeof(T));
        T workload = (T)values.GetValue(random.Next(values.Length));
        return workload;
    }
}
```

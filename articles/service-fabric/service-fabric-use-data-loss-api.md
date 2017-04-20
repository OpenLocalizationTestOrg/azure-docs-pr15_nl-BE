<properties
   pageTitle="Het verlies van gegevens op Service configuratieservices roepen | Microsoft Azure"
   description="Beschrijving van het gebruik van het verlies van gegevens api"
   services="service-fabric"
   documentationCenter=".net"
   authors="LMWF"
   manager="rsinha"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="lemai"/>
   
# <a name="how-to-invoke-data-loss-on-services"></a>Het verlies van gegevens op de Services aanroepen

>[AZURE.WARNING] Dit document wordt beschreven hoe u gegevensverlies veroorzaken in uw diensten en moet met zorg worden gebruikt.

## <a name="introduction"></a>Inleiding
U kunt het verlies van gegevens op een partitie van uw Fabric Service Service door de aanroepende StartPartitionDataLossAsync() aanroepen.  Deze api gebruikt de injectie van fouttolerantie en Analysis Service voor het uitvoeren van het werk als u de voorwaarden van gegevens verloren gaan.

## <a name="using-the-fault-injection-and-analysis-service"></a>De injectie van fouttolerantie en Analysis Service

De injectie van fouttolerantie en Analysis Service ondersteunt momenteel de volgende API's in de onderstaande grafiek.  Aan de rechterkant van de grafiek ziet u de bijbehorende PowerShell-cmdlet.  Raadpleeg de msdn-documentatie over elke API voor meer informatie over elke optie.

|           C#-API                    |         PowerShell-Cmdlet                      |
|-------------------------------------|-----------------------------------------------:|
|[StartPartitionDataLossAsync] [dl]   |[Start ServiceFabricPartitionDataLoss] [psdl]   |
|[StartPartitionQuorumLossAsync] [ql] |[Start ServiceFabricPartitionQuorumLoss] [psql] |
|[StartPartitionRestartAsync] [rp]    |[Start ServiceFabricPartitionRestart] [psrp]    |

## <a name="conceptual-overview-of-running-a-command"></a>Conceptueel overzicht van het uitvoeren van een opdracht

De injectie van fouttolerantie en Analysis Service maakt gebruik van een asynchrone model waar u de opdracht beginnen met een API, hierna aangeduid als de API 'Start' in dit document en vervolgens controleert de voortgang van deze opdracht met behulp van een "GetProgress" API totdat het een terminal toestand is bereikt of totdat u deze annuleren.
Een opdracht starten, roept de API 'Start' voor de bijbehorende API.  Deze API als resultaat gegeven wanneer de storing injectie en Analysis Service het verzoek heeft geaccepteerd.  Echter, het geeft niet aan hoe ver een opdracht is uitgevoerd, of zelfs als deze nog is gestart.  Bel API die overeenkomt met de API van de 'Start' voorheen 'GetProgress' om de voortgang van een opdracht.  "GetProgress" API retourneert een object die de huidige status van de opdracht in de eigenschap State.  Een opdracht wordt uitgevoerd voor onbepaalde tijd totdat:

1.  Voltooid.  Als u 'GetProgress' op het in dit geval, worden de voortgang van het object staat ingevuld.
2.  Er wordt een fatale fout aangetroffen.  Als u 'GetProgress' in dit geval op deze aanroept, wordt de voortgang van het object staat worden mislukt
3.  U deze annuleren via de [CancelTestCommandAsync]  [ cancel] API of [Stop ServiceFabricTestCommand]  [ cancelps] PowerShell-cmdlet.  Als u 'GetProgress' op het in dit geval, de voortgang van het object staat worden geannuleerd of ForceCancelled, afhankelijk van het argument voor die API.  Raadpleeg de documentatie bij de [CancelTestCommandAsync]  [ cancel] voor meer informatie.


## <a name="details-of-running-a-command"></a>Details van het uitvoeren van een opdracht

Om een opdracht starten, roept de API Start met de verwachte argumenten.  Alle API's Start hebben een Guid-argument met de naam bewerkingsnummer.  U moet bijhouden van het bewerkingsnummer argument, omdat deze wordt gebruikt voor het bijhouden van de voortgang van deze opdracht.  Dit moet worden doorgegeven in de "GetProgress" API bijhouden van de voortgang van de opdracht.  Het bewerkingsnummer moet uniek zijn.

Na met succes de Start-API aanroept, moet de API GetProgress worden aangeroepen in een lus totdat de geretourneerde voortgang staat eigenschap is voltooid.  Alle [FabricTransientException van]  [ fte] en de OperationCanceledException moet opnieuw worden uitgevoerd.
Als de opdracht een terminal staat (voltooid, Faulted of geannuleerd) bereikt is, wordt de voortgang van het geretourneerde resultaat eigenschap als u meer informatie hebben.  Als de status is voltooid, bevat Result.SelectedPartition.PartitionId de partitie-id die geselecteerd is.  Result.Exception is leeg.  Als de status is mislukt, Result.Exception de reden zal hebben veroorzaakt injectie en Analysis Service mislukt de opdracht.  Result.SelectedPartition.PartitionId moet de partitie-id die geselecteerd is.  In sommige situaties wordt de opdracht mogelijk niet verdergaat ver genoeg om een partitie te kiezen.  In dat geval wordt de id van de partitie 0 zijn.  Als de status wordt geannuleerd, worden Result.Exception niet null zijn.  Zoals het geval van Faulted, Result.SelectedPartition.PartitionId hebben de partitie-id die is gekozen, maar als de opdracht niet ver genoeg zijn heeft om dit te doen, dit is 0.  Raadpleeg tevens het onderstaande voorbeeld.

De onderstaande voorbeeldcode wordt aangegeven hoe start en schakel de voortgang van een opdracht om te leiden tot gegevensverlies op een specifieke partitie.

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // The id of the target partition inside the target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.        

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                // In a terminal state .Result.SelectedPartition.PartitionId will have the chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}'", operationId, progress.Result.Exception);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(5.0d)).ConfigureAwait(false);
        }
    }
```

Het onderstaande voorbeeld ziet u hoe u de PartitionSelector gebruiken om een willekeurige partitie van een opgegeven service te kiezen:

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have the Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }
            catch (Exception e)
            {
                Console.WriteLine("Unexpected exception '{0}'", e);
                throw;
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                Console.WriteLine("Printing progress.Result:");
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);

                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}', SelectedPartition {2}", operationId, progress.Result.Exception, progress.Result.SelectedPartition);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }
    }
```

## <a name="history-and-truncation"></a>Geschiedenis en de PST-bestanden

Nadat een opdracht een terminal staat bereikt is, blijven de metagegevens in de injectie van fouttolerantie en Analysis Service voor een bepaalde tijd voordat deze wordt verwijderd om ruimte te besparen.  Als 'GetProgress' het bewerkingsnummer van een opdracht gebruikt heet nadat het is verwijderd, wordt een FabricException met een foutcode KeyNotFound geretourneerd.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx

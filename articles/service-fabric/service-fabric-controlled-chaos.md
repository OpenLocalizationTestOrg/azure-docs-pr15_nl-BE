<properties
   pageTitle="Veroorzaken Chaos in clusters Service Fabric | Microsoft Azure"
   description="Chaos in het cluster te beheren met behulp van fouttolerantie injectie en Cluster Analysis Service-API's."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="rsinha"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="motanv"/>

# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Gecontroleerde Chaos in clusters Fabric-Service veroorzaken
Grote gedistribueerde systemen zoals cloud-infrastructuren per definitie onbetrouwbaar zijn. Azure Service Fabric kunnen ontwikkelaars schrijven betrouwbare diensten op een slechte infrastructuur. Ontwikkelaars moeten schrijven krachtige services, kunnen fouten tegen dergelijke onbetrouwbaar infrastructuur voor het testen van de stabiliteit van hun diensten veroorzaken.

De injectie van fouttolerantie en Cluster Analysis Service (ook wel bekend als de fout met betrekking tot Analysis Service) biedt ontwikkelaars de mogelijkheid om een fout met betrekking tot acties om services te testen. Echter krijgt gerichte gesimuleerde fouten u alleen tot nu toe. Als u het testen verder, kunt u de Chaos.

Continue, afwisselende fouten (correcte en geforceerde afsluiting) in het cluster simuleert chaos gedurende langere tijd. Na het configureren van Chaos met de frequentie en de aard van de fouten, kunnen starten of stoppen via API's C# of PowerShell fouten genereren in het cluster en de service.

Terwijl Chaos wordt uitgevoerd, is het resultaat van verschillende gebeurtenissen die de status van de uitvoering op het moment dat vastleggen. Een ExecutingFaultsEvent bevat bijvoorbeeld alle fouten die in deze iteratie wordt uitgevoerd. Een ValidationFailedEvent bevat de details van een fout aangetroffen tijdens het valideren van het cluster. U kunt als u het rapport van Chaos wordt uitgevoerd in de GetChaosReportAsync-API aanroepen.

## <a name="faults-induced-in-chaos"></a>Fouten die worden bewerkstelligd in de Chaos
Chaos fouten genereert over het hele Service Fabric '-cluster en gebreken die zichtbaar zijn in maanden of jaren in een paar uur gecomprimeerd. De combinatie van afwisselende fouten met de fout met hoge snelheid wordt gezocht naar hoek gevallen die anders worden gemist. In deze oefening Chaos leidt tot een aanzienlijke verbetering van de kwaliteit van de code van de service.

Chaos induceert fouten uit de volgende categorieën:

 - Een knooppunt opnieuw opstarten
 - Een pakket van de gedistribueerde code opnieuw
 - Replica's verwijderen
 - Opnieuw een replica
 - Verplaatsen van een primaire replica (configureerbaar)
 - Verplaatsen van een secundaire replica (configureerbaar)

Chaos wordt uitgevoerd in meerdere iteraties. Elke iteratie bestaat uit fouten en cluster validatie voor de opgegeven periode. U kunt de tijd die voor het cluster te stabiliseren en de validatie mislukt. Een storing in het cluster validatie wordt gevonden, Chaos wordt gegenereerd als een ValidationFailedEvent met de UTC-tijdstempel en de details van de fout zich blijft voordoen.

Neem bijvoorbeeld een instantie van de Chaos die is ingesteld voor een uur met een maximum van drie gelijktijdige fouten. Chaos induceert drie fouten en controleert vervolgens de status van het cluster. Doorlopen de vorige stap totdat deze expliciet is gestopt via de API StopChaosAsync of één uur wordt doorgegeven. Als het cluster in elke iteratie beschadigd raakt (dat wil zeggen, het is niet stabiel binnen een geconfigureerde tijdsduur), Chaos wordt een ValidationFailedEvent gegenereerd. Deze gebeurtenis geeft aan dat er iets een opgetreden fout is en verder onderzoek moet.

In zijn huidige vorm induceert Chaos alleen veilige fouten. Dit houdt in dat, bij ontstentenis van externe fouten quorum winst of verlies van gegevens nooit optreedt.

## <a name="important-configuration-options"></a>Belangrijke configuratie-opties
 - **TimeToRun**: totale tijd die Chaos wordt uitgevoerd voordat deze met succes is voltooid. U kunt de Chaos stoppen voordat het voor de periode TimeToRun heeft uitgevoerd via de API StopChaos.
 - **MaxClusterStabilizationTimeout**: de maximale hoeveelheid tijd moet worden gewacht voordat het weer in orde zijn voordat u opnieuw controleert op het cluster. Deze wachttijd is de werklast verminderen van het cluster terwijl deze worden hersteld. De controles die uitgevoerd zijn:
    - Als de status van het cluster OK is
    - Als de status van de service OK is
    - Als de doelreplica grootte instelt wordt voor de service-partitie bereikt
    - Dat er geen InBuild replica's bestaan
 - **MaxConcurrentFaults**: het maximum aantal gelijktijdige fouten die in elke iteratie worden geïnduceerd. Het hoger het getal, hoe meer agressieve Chaos is. Dit resulteert in meer complexe failover en combinaties van de overgang. Chaos zorgt ervoor dat, bij gebrek aan externe fouten, er is geen quorum verlies of verlies van gegevens, ongeacht hoe hoog een waarde deze configuratie heeft.
 - **EnableMoveReplicaFaults**: Hiermee schakelt u de fouten die ervoor zorgen de primaire of secundaire replica's dat verplaatsen of. Deze fouten zijn standaard uitgeschakeld.
 - **WaitTimeBetweenIterations**: de hoeveelheid tijd die moet worden gewacht tussen herhalingen, dat wil zeggen na een ronde van fouten en de bijbehorende validatie.
 - **WaitTimeBetweenFaults**: het bedrag van de wachttijd tussen twee opeenvolgende fouten in een herhaling.

## <a name="how-to-run-chaos"></a>Het uitvoeren van Chaos
**C#:**

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```
**PowerShell:**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```

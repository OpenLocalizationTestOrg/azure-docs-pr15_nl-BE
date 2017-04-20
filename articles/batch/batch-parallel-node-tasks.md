<properties
    pageTitle="Maximaal profiteren van Batch knooppunt met parallelle taken | Microsoft Azure"
    description="Efficiëntie en lagere kosten verhogen via minder compute nodes en lopende gelijktijdige taken op elk knooppunt in een Batch Azure-toepassingen"
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="maximize-azure-batch-compute-resource-usage-with-concurrent-node-tasks"></a>Azure Batch compute Resourcegebruik met gelijktijdige knooppunt taken maximaliseren

Door meer dan één taak tegelijk op elk computerknooppunt in uw Batch Azure-groep, kunt u brongebruik op een kleiner aantal knooppunten in de groep te maximaliseren. Voor sommige werklasten, dit kan leiden tot kortere taaktijden en lagere kosten.

Terwijl sommige scenario's alle bronnen van een knooppunt aan één taak programmathreads komen, profiteert verschillende situaties waarin meerdere taken die resources delen:

 - **Overdracht van gegevens tot een minimum beperken** wanneer taken kunnen delen van gegevens. In dit scenario kunt u data transfer kosten aanzienlijk beperken door gedeelde gegevens kopiëren naar een kleiner aantal knooppunten en het uitvoeren van taken parallel op elk knooppunt. Dit geldt met name als de gegevens worden gekopieerd naar elk knooppunt moet worden overgebracht tussen geografische gebieden.

 - **Maximizing geheugengebruik** wanneer taken een grote hoeveelheid geheugen, maar alleen gedurende korte perioden van tijd en op variabele momenten tijdens de uitvoering vereisen. U kunt gebruikmaken van minder, maar grotere, computerknooppunten met meer geheugen efficiënt verwerken dergelijke pieken. Deze knooppunten zou hebben meerdere taken gelijktijdig uitgevoerd op elk knooppunt, maar elke taak zou profiteren van een grote hoeveelheid geheugen van de knooppunten op verschillende tijdstippen.

 - **Beperkende knooppunt nummer grenzen** als communicatie tussen knooppunten binnen een groep vereist is. Groepen van toepassingen geconfigureerd voor communicatie tussen knooppunten zijn momenteel beperkt tot 50 computerknooppunten. Als elk knooppunt in een dergelijke groep kunnen taken parallel uitvoeren, kan een groter aantal taken tegelijk worden uitgevoerd.

 - **Het repliceren van een bedrijf op cluster berekenen**, zoals wanneer u eerst een compute omgeving naar Azure verplaatsen. Als uw huidige op gebouwen-oplossing wordt uitgevoerd meerdere taken per computerknooppunt, kunt u het maximum aantal taken te spiegelen nauwer die configuratie knooppunt verhogen.

## <a name="example-scenario"></a>Voorbeeldscenario

Als voorbeeld ter illustratie van de voordelen van de uitvoering van parallelle taken, Stel dat uw toepassing taak CPU en geheugen vereisten is dat [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md#general-purpose-d) knooppunten zijn voldoende. Maar om de taak in de vereiste tijd voltooien, 1000 van deze knooppunten nodig zijn.

In plaats van met behulp van standaard\_D1 knooppunten met 1 CPU core, kunt u [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) knooppunten die 16 cores hebben en tot het uitvoeren van parallelle taken. Daarom *16 maal minder knooppunten* kan worden gebruikt, in plaats van 1.000 knooppunten, alleen 63 vereist zou zijn. Bovendien als grote bestanden of referentiegegevens voor elk knooppunt, duur van de taak en efficiëntie zijn opnieuw verbeterd sinds slechts 16 knooppunten worden de gegevens gekopieerd.

## <a name="enable-parallel-task-execution"></a>Tot het uitvoeren van parallelle taken.

U configureren-computerknooppunten voor uitvoering van parallelle taken op het niveau van toepassingen. Met de bibliotheek Batch .NET stelt u de [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] eigenschap bij het maken van een groep. Als u de batchverwerking REST API gebruikt, stelt u de [maxTasksPerNode] [ rest_addpool] -element in het hoofdgedeelte van de aanvraag bij het maken van toepassingen.

Azure Batch kunt u maximale taken per knooppunt instellen tot vier keer (4 x) het aantal cores knooppunt. Bijvoorbeeld, als de groep van toepassingen is geconfigureerd met knooppunten van het formaat van 'Groot' (vier cores) vervolgens `maxTasksPerNode` kan worden ingesteld op 16. Zie voor meer informatie over het aantal kernen voor elk van de grootte van het knooppunt, [formaten voor Cloud Services](../cloud-services/cloud-services-sizes-specs.md). Zie voor meer informatie over de grenzen van de service, [quota en beperkingen voor de Batch Azure service](batch-quota-limit.md).

> [AZURE.TIP] Moet u rekening houden met de `maxTasksPerNode` als u een [formule automatisch schalen] de waarde[ enable_autoscaling] voor uw toepassingen. Bijvoorbeeld, een formule die wordt geëvalueerd als `$RunningTasks` aanzienlijk kunnen worden beïnvloed door een stijging van de taken per knooppunt. Zie [automatisch schalen computerknooppunten in een Batch Azure-toepassingen](batch-automatic-scaling.md) voor meer informatie.

## <a name="distribution-of-tasks"></a>Verdeling van taken

Wanneer de compute nodes in een groep kunnen taken gelijktijdig worden uitgevoerd, is het belangrijk om aan te geven hoe de taken verdeeld over de knooppunten in de groep.

Met behulp van de [CloudPool.TaskSchedulingPolicy] [ task_schedule] (eigenschap), kunt u opgeven dat taken gelijkmatig over alle knooppunten in de groep ("verspreiding") moeten worden toegewezen. Of u kunt opgeven dat zo zo veel mogelijk taken moeten worden toegewezen aan elk knooppunt voordat u taken zijn toegewezen aan een ander knooppunt in de groep ("verpakking").

Een voorbeeld van hoe deze functie nuttig is, kunt u in de groep [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) knooppunten (in het voorbeeld) die is geconfigureerd met een [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] waarde van 16. Als de [CloudPool.TaskSchedulingPolicy] [ task_schedule] is geconfigureerd met een [ComputeNodeFillType] [ fill_type] *Pack*, zou het gebruik van alle 16 cores van elk knooppunt te maximaliseren en toestaan dat een [groep autoscaling](batch-automatic-scaling.md) ongebruikte knooppunten uit de groep (knooppunten zonder toegewezen taken) terug te schroeven. Dit minimaliseert Resourcegebruik en bespaart u geld.

## <a name="batch-net-example"></a>Voorbeeld van de batch .NET

Deze [Batch.NET] [ api_net] API codefragment ziet u een aanvraag voor het maken van een groep met vier grote knooppunten met een maximum van vier taken per knooppunt. Deze geeft een taak plannen beleid dat elk knooppunt worden gevuld met de taken voor het toewijzen van taken naar een ander knooppunt in de groep. Zie voor meer informatie over het toevoegen van toepassingen met behulp van de Batch .NET API [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicated: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Voorbeeld van de REST batch

Deze [Batch REST] [ api_rest] API fragment toont een aanvraag voor het maken van een groep met twee grote knooppunten met een maximum van vier taken per knooppunt. Zie [een groep aan een account toevoegen]voor meer informatie over het toevoegen van toepassingen met de REST API[rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicated":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [AZURE.NOTE] U kunt instellen de `maxTasksPerNode` -element en [MaxTasksPerComputeNode] [ maxtasks_net] eigenschap die alleen bij het pool maken. Zij kunnen niet worden gewijzigd nadat een groep is gemaakt.

## <a name="code-sample"></a>Voorbeeld van code

De [ParallelNodeTasks] [ parallel_tasks_sample] project op GitHub illustreert het gebruik van de [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] eigenschap.

Deze C#-consoletoepassing gebruikt de [Batchverwerking .NET] [ api_net] bibliotheek voor het maken van een groep met een of meer compute nodes. Een configureerbaar aantal taken wordt uitgevoerd op die knooppunten voor het simuleren van variabele laden. Uitvoer van de toepassing geeft aan welke knooppunten elke taak uitgevoerd. De toepassing biedt ook een overzicht van de duur en de taakparameters. Het gedeelte overzicht van de uitvoer van twee verschillende uitvoeringen van de voorbeeldtoepassing wordt hieronder weergegeven.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

De eerste uitvoering van de voorbeeldtoepassing blijkt dat met een enkel knooppunt in de groep en de instelling van één taak per knooppunt, de duur van de taak wordt gedurende 30 minuten.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

De tweede reeks van de steekproef blijkt een aanzienlijke afname van de duur van de taak. Dit komt omdat de groep van toepassingen is geconfigureerd met de vier taken per knooppunt, waardoor uitvoering van parallelle taken voor het voltooien van de taak in bijna een kwart van de tijd.

> [AZURE.NOTE] De duur van de taak in de bovenstaande overzichten bevatten geen Aanmaaktijd van toepassingen. Elk van de bovenstaande taken is ingediend bij de eerder gemaakte groepen waarvan computerknooppunten in de *niet-actief* staat op moment van indiening zijn.

## <a name="next-steps"></a>Volgende stappen

### <a name="batch-explorer-heat-map"></a>Batch Explorer warmte kaart

De [Azure Batch Explorer][batch_explorer], een van de Batch Azure [voorbeeldtoepassingen][github_samples], bevat een *Warmte-kaart* -functie waarmee de visualisatie van de uitvoering van taken. U bent bij uitvoeren van de [ParallelTasks] [ parallel_tasks_sample] voorbeeldtoepassing, kunt u de functie warmte kaart gemakkelijk visualiseren het uitvoeren van parallelle taken op elk knooppunt.

![Batch Explorer warmte kaart][1]

*Batch Explorer warmte kaart met een groep van vier knooppunten, met elk knooppunt dat momenteel vier taken wordt uitgevoerd*

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

[1]: ./media/batch-parallel-node-tasks\heat_map.png

<properties
    pageTitle="Taakafhankelijkheden in Batch Azure | Microsoft Azure"
    description="Maken van taken die afhankelijk van de succesvolle afronding van andere taken zijn voor het verwerken van MapReduce stijl en vergelijkbare gegevens voor grote werklast in Azure Batch."
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
    ms.date="09/28/2016"
    ms.author="marsma" />

# <a name="task-dependencies-in-azure-batch"></a>Taakafhankelijkheden in Azure Batch

De functie afhankelijkheden van Azure Batch is een geschikt als u wilt verwerken:

- MapReduce-stijl werkbelasting in de cloud.
- Taken waarvan de verwerkingstaken kunnen worden uitgedrukt als een gestuurde acyclische grafiek (DAG).
- Elke andere taak waarin downstream taken afhankelijk van de uitvoer van de upstream-taken zijn.

Taakafhankelijkheden batch kunnen u taken die zijn gepland voor uitvoering op computerknooppunten pas na de succesvolle afronding van een of meer taken maken. U kunt bijvoorbeeld een taak die elk frame van een 3D-film met een aparte, parallelle taken wordt maken. De laatste taak--de "samenvoegen taak"--samenvoegingen de frames gerenderd in de volledige film nadat alle frames hebt geplaatst.

U kunt taken die afhankelijk van andere taken in een een-op- of één-op-veel-relatie zijn maken. U kunt zelfs de afhankelijkheid van een bereik waarin een taak afhankelijk van de voltooiing van een groep taken binnen een bepaald bereik van taak-id's is maken. U kunt deze drie basisscenario's als u veel-op-veel relaties wilt combineren.

## <a name="task-dependencies-with-batch-net"></a>Afhankelijkheden van taken met Batch .NET

In dit artikel wordt besproken hoe we taakafhankelijkheden configureren met behulp van de [Batch.NET] [ net_msdn] bibliotheek. We u zien [taakafhankelijkheid inschakelen](#enable-task-dependencies) voor uw projecten, en wordt vervolgens getoond hoe [u een taak met afhankelijkheden](#create-dependent-tasks). Tot slot bespreken we de [afhankelijkheid scenario's](#dependency-scenarios) waarin de Batch.

## <a name="enable-task-dependencies"></a>Taakafhankelijkheden inschakelen

Als u taakafhankelijkheden in uw toepassing Batch, moet u eerst zien de Batch-service dat de taak wordt gebruikt voor taakafhankelijkheden. In Batch .NET inschakelen op uw [CloudJob] [ net_cloudjob] door de [UsesTaskDependencies] [ net_usestaskdependencies] eigenschap `true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

In het voorgaande codefragment "batchClient" is een exemplaar van de [BatchClient] [ net_batchclient] klasse.

## <a name="create-dependent-tasks"></a>Afhankelijke taken maken

Als u een taak die afhankelijk is van de succesvolle afronding van een of meer taken maakt, wilt vertellen u Batch dat de taak "afhankelijk" de andere taken. In Batch .NET, configureert u de [CloudTask][net_cloudtask]. [DependsOn] [net_dependson] -eigenschap met een exemplaar van de [TaskDependencies] [ net_taskdependencies] klasse:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Dit codefragment maakt u een taak met de ID 'Bloemen', die wordt gepland om op een compute node worden uitgevoerd nadat de taken met de id's van de "Regen" en "Sun" hebt voltooid.

 > [AZURE.NOTE] Een taak moet worden voltooid wanneer de status **voltooid** is en de **afsluitcode** wordt beschouwd als `0`. Batch .NET, betekent dit een [CloudTask][net_cloudtask]. [Staat] [net_taskstate] van de waarde van de eigenschap `Completed` en de CloudTask van [TaskExecutionInformation][net_taskexecutioninformation]. [Afsluitcode] [net_exitcode] is van de waarde van de eigenschap `0`.

## <a name="dependency-scenarios"></a>Afhankelijkheid-scenario 's

Er zijn drie Basistaak afhankelijkheid scenario's die u in Azure Batch gebruiken kunt:-op-een, een-op-veel en taak-ID bereik afhankelijkheid. Deze kunnen worden gecombineerd om een vierde veel-op-veel-scenario.

 Scenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Voorbeeld | |
 :-------------------: | ------------------- | -------------------
 [Een-op-](#one-to-one) | *taskB* is afhankelijk van *taskA* <p/> *taskB* zal niet worden gepland voor uitvoering totdat *taskA* is voltooid | ![Diagram: de een-op-taakafhankelijkheid][1]
 [Een-op-veel](#one-to-many) | *taskC* afhankelijk van *taskA* en *taskB* <p/> *taskC* zal niet worden gepland voor uitvoering totdat zowel *taskA* en *taskB* zijn voltooid | ![Diagram: een-op-veel-taakafhankelijkheid][2]
 [Bereik van taak-ID](#task-id-range) | *taskD* is afhankelijk van een aantal taken <p/> *taskD* zal niet worden gepland voor uitvoering totdat de taken met de id's *1* tot en met *10* zijn voltooid | ![Diagram: Afhankelijkheid bereik-id][3]

>[AZURE.TIP] Kunt u **veel-op-veel** -relaties, zoals waar taken C, D, E en F elke afhankelijk van taken A en B. zijn Dit is handig, bijvoorbeeld in parallelized voorverwerking scenario's waar de downstream taken afhankelijk van de uitvoer van meerdere upstream taken zijn.

### <a name="one-to-one"></a>Een-op-

Als u een taak maken die afhankelijk van de voltooiing van een andere taak is, die u opgeeft een enkele taak-ID aan de [TaskDependencies][net_taskdependencies]. [OnId] [net_onid] statische methode wanneer u de waarden voor de [DependsOn] [ net_dependson] eigenschap van [CloudTask][net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>Een-op-veel

Een taak die afhankelijk van de succesvolle voltooiing van meerdere taken is maken, geven u een taak-id's aan de [TaskDependencies]-collectie[net_taskdependencies]. [OnIds] [net_onids] statische methode wanneer u de waarden voor de [DependsOn] [ net_dependson] eigenschap van [CloudTask][net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

### <a name="task-id-range"></a>Bereik van taak-ID

Als u een taak die afhankelijk van de voltooiing van een groep taken waarvan id's liggen binnen een bereik is, u leveren de eerste en laatste taak id's in het bereik aan de [TaskDependencies][net_taskdependencies]. [OnIdRange] [net_onidrange] statische methode wanneer u de waarden voor de [DependsOn] [ net_dependson] eigenschap van [CloudTask][net_cloudtask].

>[AZURE.IMPORTANT] Als u taak-ID bereiken voor de afhankelijkheden, worden de taak-id's in het bereik *moet* tekenreeksrepresentaties van gehele getallen. Bovendien moet alle taken in het bereik voltooid voor de afhankelijke taak te worden uitgevoerd.

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="code-sample"></a>Voorbeeld van code

De [TaskDependencies] [ github_taskdependencies] voorbeeldproject is een van de [codevoorbeelden Azure Batch] [ github_samples] op GitHub. Deze oplossing met Visual Studio 2015 wordt gedemonstreerd hoe taakafhankelijkheid op een taak inschakelen en die taken uitvoeren op een pool van computerknooppunten maken van taken die afhankelijk van andere taken zijn.

## <a name="next-steps"></a>Volgende stappen

### <a name="application-deployment"></a>Implementatie van toepassingen

De functie voor [toepassingspakketten](batch-application-packages.md) batch biedt een eenvoudige manier om beide te implementeren en toepassingen die uw taken uitvoeren op computerknooppunten versie.

### <a name="installing-applications-and-staging-data"></a>Installeren van toepassingen en gegevens in staging

Bekijk de [toepassingen installeren en tijdelijke gegevens op Batch computerknooppunten] [ forum_post] boeken in het forum Batch Azure voor een overzicht van de verschillende methoden voor het voorbereiden van de knooppunten uit te voeren taken. Geschreven door een van de teamleden Azure Batch, is dit bericht een goede primer op de verschillende manieren om bestanden (waaronder zowel toepassingen als taak invoergegevens) naar uw computerknooppunten.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagram: een-op-afhankelijkheid"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagram: een-op-veel-afhankelijkheid"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagram: afhankelijkheid bereik-id"

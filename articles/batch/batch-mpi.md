<properties
    pageTitle="MPI-toepassingen uitvoeren in Azure Batch met meerdere exemplaren taken | Microsoft Azure"
    description="Informatie over het uitvoeren van Message Passing Interface (MPI)-toepassingen met behulp van het type meerdere exemplaren in Azure Batch."
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
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-azure-batch"></a>Taken met meerdere instanties Message Passing Interface (MPI)-toepassingen uitvoeren in Azure Batch gebruiken

Meerdere exemplaren taken kunt u een batchtaak Azure gelijktijdig op meerdere computerknooppunten worden uitgevoerd. Deze taken kunnen HPC scenario's zoals Message Passing Interface (MPI)-toepassingen in Batch. In dit artikel leert u het uitvoeren van meerdere exemplaren taken met behulp van de [Batch.NET] [ api_net] bibliotheek.

>[AZURE.NOTE] De voorbeelden in dit artikel gericht op Batch .NET, MS-MPI en Windows compute nodes, zijn de meerdere exemplaren taak concepten die hier worden beschreven van toepassing op andere platforms en technologieën (Python en MPI op Linux knooppunten, bijvoorbeeld Intel).

## <a name="multi-instance-task-overview"></a>Meerdere exemplaren taak-overzicht

In een Batch wordt elke taak meestal uitgevoerd op een enkele compute node--u meerdere taken naar een project indienen en de Batch service plant elke taak voor uitvoering op een knooppunt. Echter, van een taak **meerdere instanties-instellingen**configureert, geeft u aan Batch in plaats daarvan een primaire taak en meerdere subtaken die vervolgens worden uitgevoerd op meerdere knooppunten te maken.

![Meerdere exemplaren taak-overzicht][1]

Wanneer u een taak met instellingen voor een taak meerdere instanties indient, voert Batch verschillende stappen uit unieke taken met meerdere instanties:

1. De Batch-service maakt u een **primaire** en meerdere **subtaken** op basis van de instellingen voor meerdere exemplaren. Het totale aantal taken (primaire plus alle subtaken) komt overeen met het aantal **exemplaren** (computerknooppunten) u in de instellingen van meerdere exemplaren.
1. Batch wordt aangegeven dat een van de compute nodes **master**en laat u de primaire taak uit te voeren op de master. Hiermee plant u de subtaken wilt uitvoeren op de rest van de compute nodes toegewezen aan meerdere instanties taak een subtaak per knooppunt.
1. Download **algemene resource bestanden** die u in de instellingen voor meerdere exemplaren van de primaire en alle subtaken.
1. Na de algemene resource bestanden zijn gedownload, de primaire en de **coördinatie-opdracht** die u in de instellingen voor meerdere exemplaren van subtaken worden uitgevoerd. De coördinatie-opdracht wordt meestal gebruikt voor het voorbereiden van knooppunten voor het uitvoeren van de taak. Dit zijn het starten van achtergrondservices (zoals [Microsoft MPI][msmpi_msdn]van `smpd.exe`) en controleren of de knooppunten klaar zijn voor het verwerken van berichten tussen knooppunt.
1. De belangrijkste taak voert de **toepassingsopdracht** op het hoofdknooppunt *nadat* de coördinatie-opdracht is voltooid door de primaire en alle subtaken. De toepassingsopdracht is de opdrachtregel van de taak meerdere instanties zelf, en alleen door de primaire taak wordt uitgevoerd. In een [MS MPI][msmpi_msdn]-gebaseerde oplossing, dit is waar u uw MPI-toepassingen met behulp van uitvoeren `mpiexec.exe`.

> [AZURE.NOTE] Hoewel het functioneel verschillend is, de taak"meerdere exemplaren' is geen unieke taak zoals de [starttaak] [ net_starttask] of [JobPreparationTask][net_jobprep]. De taak van meerdere exemplaren is gewoon een Batch standaardtaak ([CloudTask] [ net_task] in Batch .NET) met meerdere instanties waarvan de instellingen zijn geconfigureerd. In dit artikel noemen we dit de **taak van meerdere exemplaren**.

## <a name="requirements-for-multi-instance-tasks"></a>Vereisten voor meerdere exemplaren taken

Taken met meerdere instanties moeten een pool met **tussen knooppunt communicatie is ingeschakeld**en met de **uitvoering van gelijktijdige taken uitgeschakeld**. Als u probeert een taak meerdere exemplaren in een groep met de communicatie tussen knooppunten uitgeschakeld uitvoeren, of met een *maxTasksPerNode* -waarde groter is dan 1, wordt de taak niet gepland--blijft voor onbepaalde tijd in de 'actieve' staat. Dit codefragment bevat het maken van dergelijke een groep van toepassingen met behulp van de Batch .NET-bibliotheek.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicated: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

Taken met meerdere instanties kunnen bovendien *alleen* uitvoeren op knooppunten in **toepassingen die zijn gemaakt na 14 December 2015**.

### <a name="use-a-starttask-to-install-mpi"></a>Gebruik een starttaak MPI installeren

MPI toepassingen uitgevoerd met een taak meerdere instanties, moet u eerst een MPI-implementatie (MS MPI of Intel MPI, bijvoorbeeld) installeren op de compute nodes in de groep. Dit is een goed moment om te gebruiken een [starttaak][net_starttask], die wordt uitgevoerd wanneer een knooppunt wordt toegevoegd aan een groep of opnieuw wordt opgestart. Dit codefragment maakt een starttaak die het installatiepakket MS MPI als een [bronbestand][net_resourcefile]. Van de starttaak vanaf de opdrachtregel wordt uitgevoerd wanneer het bronbestand wordt gedownload naar het knooppunt. In dit geval in de opdrachtregel van MS MPI voor een installatie zonder toezicht wordt uitgevoerd.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    RunElevated = true,
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Externe directe geheugentoegang (RDMA)

Als u een [formaat geschikt voor RDMA](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) zoals A9 voor de compute nodes in uw Batch-groep, kunt de MPI-toepassing profiteren van Azure van hoge prestaties, lage latentie externe direct memory access (RDMA)-netwerk.

Zoek naar de grootte van de opgegeven als "RDMA staat" in de volgende artikelen:

* **CloudServiceConfiguration** van toepassingen

  * [Formaten voor Cloud Services](../cloud-services/cloud-services-sizes-specs.md) (Alleen Windows)

* **VirtualMachineConfiguration** van toepassingen

  * [Formaten voor virtuele machines in Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux)

  * [Formaten voor virtuele machines in Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows)

>[AZURE.NOTE] Als u wilt profiteren van RDMA op [Linux computerknooppunten](batch-linux-nodes.md), moet u **Intel MPI** op de knooppunten. Zie de sectie [van toepassingen](batch-api-basics.md#Pool) van de functie Batch overzicht voor meer informatie op CloudServiceConfiguration en VirtualMachineConfiguration.

## <a name="create-a-multi-instance-task-with-batch-net"></a>Een taak meerdere exemplaren met Batch .NET maken

Nu we de groep vereisten en de installatie van het pakket MPI hebben besproken, we maken de taak meerdere exemplaren. In dit fragment, maken we een standaard [CloudTask][net_task], configureert u de [MultiInstanceSettings] [ net_multiinstance_prop] eigenschap. Zoals eerder vermeld, wordt de taak meerdere exemplaren niet is een aparte soort, maar een Batch standaardtaak geconfigureerd met instellingen voor meerdere exemplaren.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Primaire taak en subtaken

Wanneer u de instellingen voor meerdere exemplaren van een taak maakt, moet u het nummer van de computerknooppunten die voor het uitvoeren van de taak opgeven. Wanneer u de taak aan een project indient, wordt de service Batch maakt één **primaire** taak en voldoende **subtaken** die samen overeenkomen met het aantal knooppunten dat u hebt opgegeven.

Deze taken worden een integer-id in het bereik van 0 toegewezen aan *numberOfInstances* - 1. De taak met id 0 is de primaire taak en andere id's worden subtaken. Als u de volgende instellingen voor meerdere exemplaren van een taak maakt, de belangrijkste taak zou hebben een id van 0 en de subtaken moet id's 1 tot en met 9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Hoofdknooppunt

Wanneer u een taak meerdere instanties indient, de Batch-service een van de compute nodes aanwijst als de 'master' node en laat u de primaire taak uit te voeren op de master node. De planning van subtaken worden uitgevoerd op de rest van de knooppunten die zijn toegewezen aan de taak meerdere exemplaren.

## <a name="coordination-command"></a>Coördinatie-opdracht

De **coördinatie-opdracht** wordt uitgevoerd door zowel de primaire en subtaken.

De aanroep van de coördinatie-opdracht blokkeert--Batch niet de toepassingsopdracht wordt uitgevoerd totdat de coördinatie-opdracht met succes is geretourneerd voor alle subtaken. De coördinatie-opdracht moet daarom start alle Achtergrondservices vereist controleren of deze gereed voor gebruik zijn en vervolgens af te sluiten. Bijvoorbeeld deze coördinatie-opdracht voor een oplossing met MS MPI versie 7 start de SMPD-service op het knooppunt vervolgens afgesloten:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Let op het gebruik van `start` in deze coördinatie-opdracht. Dit is nodig omdat de `smpd.exe` toepassing onmiddellijk na de uitvoering niet weer. Zonder het gebruik van [start] [ cmd_start] opdracht deze coördinatie-opdracht zou niet terug, en daarom aangeraden de toepassingsopdracht wordt uitgevoerd.

## <a name="application-command"></a>, Toepassingsopdracht

Als de primaire taak en alle subtaken hebt de coördinatie-opdracht wordt uitgevoerd, wordt de opdrachtregel de meerdere exemplaren van de taak uitgevoerd door de primaire taak *alleen*. We noemen dit de **toepassingsopdracht** te onderscheiden van de coördinatie-opdracht.

Voor MS MPI-toepassingen, gebruikt u de toepassingsopdracht uitvoeren MPI-toepassingen met `mpiexec.exe`. Dit is bijvoorbeeld een de toepassingsopdracht voor een oplossing met MS-MPI versie 7:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

>[AZURE.NOTE] Omdat MS-MPI `mpiexec.exe` gebruikt de `CCP_NODES` variabele door standaard (Zie [omgevingsvariabelen](#environment-variables)) in het voorbeeld hierboven toepassing vanaf de opdrachtregel worden uitgesloten van het.

## <a name="environment-variables"></a>Omgevingsvariabelen

Batch wordt gemaakt van diverse [omgevingsvariabelen] [ msdn_env_var] bepaalde taken voor meerdere exemplaren op de compute nodes toegewezen aan een taak meerdere exemplaren. Opdrachtregels voor de coördinatie en de toepassing kunt verwijzen naar deze omgevingsvariabelen kunt u scripts en programma's die zij uitvoeren.

De volgende omgevingsvariabelen worden gemaakt door de Batch-service voor gebruik door meerdere instanties taken:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Voor meer informatie over deze en de andere partij compute knooppunt omgevingsvariabelen, met inbegrip van hun inhoud en zichtbaarheid, [berekenen knooppunt omgevingsvariabelen]Zie[msdn_env_var].

>[AZURE.TIP] In het codevoorbeeld Batch Linux MPI bevat een voorbeeld van hoe meer van deze omgevingsvariabelen kunnen worden gebruikt. De [coördinatie-cmd] [ coord_cmd_example] Bash script algemene toepassing gedownload en invoer uit de opslag van Azure-bestanden, kan een Network File System (NFS)-share op de master node en configureert u de andere knooppunten die zijn toegewezen aan de taak meerdere exemplaren als NFS-clients.

## <a name="resource-files"></a>Bronbestanden

Er zijn twee sets van bronbestanden voor taken met meerdere instanties: **algemene resource bestanden** downloaden van *alle* taken (zowel primaire en subtaken), en de **bronbestanden** voor meerdere exemplaren opgegeven taak zelf welke *alleen de primaire* taak downloads.

In de instellingen voor meerdere exemplaren van een taak kunt u een of meer **gemeenschappelijke bronbestanden** . Deze algemene resource bestanden zijn gedownload van [Azure opslag](./../storage/storage-introduction.md) van elk knooppunt **taak gedeelde directory** op de primaire en alle subtaken. U toegang tot de gedeelde map van de taak van toepassing en coördinatie opdrachtregels met behulp van de `AZ_BATCH_TASK_SHARED_DIR` omgevingsvariabele. De `AZ_BATCH_TASK_SHARED_DIR` pad op elk knooppunt is toegewezen aan de taak meerdere exemplaren identiek is, dus u kunt delen met een opdracht één coördinatie tussen de primaire en alle subtaken. Batch geen 'gedeeld' de map in de zin van een RAS, maar u kunt gebruiken als een koppeling of punt zoals eerder beschreven in de tip op omgevingsvariabelen delen.

Bronbestanden die u opgeeft voor de taak meerdere instanties zelf worden gedownload naar de werkmap van de taak, `AZ_BATCH_TASK_WORKING_DIR`, standaard. Zoals gezegd, in tegenstelling tot de algemene resource bestanden downloadt alleen de primaire taak bronbestanden die zijn opgegeven voor de taak meerdere instanties zelf.

> [AZURE.IMPORTANT] Gebruik altijd de omgevingsvariabelen `AZ_BATCH_TASK_SHARED_DIR` en `AZ_BATCH_TASK_WORKING_DIR` om te verwijzen naar deze mappen in de opdrachtregels. Probeer niet om de paden handmatig samen te stellen.

## <a name="task-lifetime"></a>Levensduur van de taak

De levensduur van de primaire taak bepaalt de levensduur van de taak van de hele meerdere exemplaren. Als de primaire afsluit, worden alle subtaken beëindigd. De afsluitcode van het primaire wordt de afsluitcode van de taak en daarom wordt gebruikt om het slagen of mislukken van de taak voor de toepassing opnieuw.

Als een van de subtaken mislukt, afsluiten met een retourcode niet nul, bijvoorbeeld de hele meerdere exemplaren taak mislukt. De taak meerdere exemplaren vervolgens wordt afgesloten en opnieuw geprobeerd, tot de limiet voor opnieuw proberen.

Wanneer u een taak meerdere exemplaren verwijdert, worden de primaire en alle subtaken ook verwijderd door de Batch-service. Een subtaak alle mappen en bestanden worden verwijderd uit de compute nodes, net als voor een taak.

[TaskConstraints] [ net_taskconstraints] voor een taak meerdere instanties, zoals de [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock], en [RetentionTime] [ net_taskconstraint_retention] eigenschappen van een taak zijn, en van toepassing op de primaire en alle subtaken worden gehonoreerd. Echter, als u de [RetentionTime] [ net_taskconstraint_retention] eigenschap na de taak meerdere exemplaren toe te voegen aan de taak, wordt deze wijziging wordt alleen toegepast op de primaire taak. Alle subtaken blijven gebruiken van de oorspronkelijke [RetentionTime][net_taskconstraint_retention].

Lijst met recente taak van een compute node weerspiegelt de id van een subtaak als de taak onderdeel van een taak meerdere exemplaren is.

## <a name="obtain-information-about-subtasks"></a>Informatie verkrijgen over de subtaken

Als u informatie in de subtaken met behulp van de Batch .NET library, belt de [CloudTask.ListSubtasks] [ net_task_listsubtasks] methode. Deze methode retourneert informatie over alle subtaken en informatie over de compute node die de taken uitgevoerd. Met deze informatie kunt u bepalen de hoofdmap van elke subtaak, de pool-id, de huidige toestand, afsluitcode en meer. U kunt deze informatie gebruiken in combinatie met de [PoolOperations.GetNodeFile] [ poolops_getnodefile] methode van de subtaak bestanden te downloaden. Houd er rekening mee dat deze methode retourneert geen gegevens voor de primaire taak (id 0).

> [AZURE.NOTE] Tenzij anders vermeld, Batch .NET methoden die worden toegepast op de meerdere exemplaren [CloudTask] [ net_task] zelf zijn *alleen* van toepassing op de primaire taak. Bijvoorbeeld, als u de [CloudTask.ListNodeFiles] oproept[ net_task_listnodefiles] methode voor een taak meerdere exemplaren, alleen de primaire taak bestanden worden geretourneerd.

Het volgende stukje code laat zien hoe subtaak informatie te verkrijgen, alsmede de inhoud van aanvragen van de knooppunten waarop ze worden uitgevoerd.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == TaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Voorbeeld van code

De [MultiInstanceTasks] [ github_mpi] code op GitHub ziet u hoe u een taak meerdere instanties gebruiken voor het uitvoeren van een [MS MPI] [ msmpi_msdn] toepassing op Batch computerknooppunten. Volg de stappen in de [voorbereiding](#preparation) en [uitvoering van](#execution) het voorbeeld uitvoeren.

### <a name="preparation"></a>Voorbereiding

1. De eerste twee stappen in [het compileren en uitvoeren van een eenvoudige MS MPI programma][msmpi_howto]. Dit voldoet aan de prerequesites voor de volgende stap.
1. Bouwen van een *officiële* versie van de [MPIHelloWorld] [ helloworld_proj] monster MPI programma. Dit is het programma dat door de taak meerdere exemplaren op computerknooppunten worden uitgevoerd.
1. Maak een zip-bestand met `MPIHelloWorld.exe` (dat u kunt stap 2 gebouwd) en `MSMpiSetup.exe` (die u hebt gedownload stap 1). U zult dit zip-bestand uploaden als een toepassingspakket in de volgende stap.
1. Gebruik de [Azure portal] [ portal] maken een Batch [toepassing](batch-application-packages.md) 'MPIHelloWorld' genoemd en het zipbestand opgeven dat u hebt gemaakt in de vorige stap als "1.0" versie van het toepassingspakket. Zie [uploaden en beheren van toepassingen](batch-application-packages.md#upload-and-manage-applications) voor meer informatie.

>[AZURE.TIP] Een *Release* -versie van `MPIHelloWorld.exe` zodat u niet hoeft op te nemen als u meer afhankelijkheden (bijvoorbeeld, `msvcp140d.dll` of `vcruntime140d.dll`) in het toepassingspakket van uw.

### <a name="execution"></a>Uitvoering

1. Download de [azure-batch-monsters] [ github_samples_zip] van GitHub.
1. De MultiInstanceTasks- **oplossing** in Visual Studio 2015 geopend. De `MultiInstanceTasks.sln` solution-bestand bevindt zich in:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`

1. Voer uw referenties van Batch- en in `AccountSettings.settings` in het **Microsoft.Azure.Batch.Samples.Common** -project.
1. **Bouwen en uitvoeren van** de MultiInstanceTasks oplossing voor het uitvoeren van de MPI-voorbeeldtoepassing op computerknooppunten in een Batch-toepassingen.
1. *Optioneel*: Gebruik de [Azure portal] [ portal] of de [Explorer Batch] [ batch_explorer] te onderzoeken het monster van toepassingen, taak en taak ('MultiInstanceSamplePool', 'MultiInstanceSampleJob', 'MultiInstanceSampleTask') voordat u de bronnen verwijderen.

>[AZURE.TIP] U kunt [Visual Studio Community] downloaden[ visual_studio] gratis als u Visual Studio niet hebt.

Uitvoer van `MultiInstanceTasks.exe` is vergelijkbaar met het volgende:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Volgende stappen

- De blog Microsoft HPC & Azure Batch Team bespreekt [MPI ondersteuning voor Linux op Azure Batch][blog_mpi_linux], en bevat informatie over het gebruik van [OpenFOAM] [ openfoam] met de batchverwerking. Vindt u voorbeelden van Python code voor het [voorbeeld op GitHub OpenFOAM][github_mpi].

- Meer informatie over het [Linux-computerknooppunten groepen maken](batch-linux-nodes.md) voor gebruik in Azure Batch MPI oplossingen.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Overzicht van meerdere exemplaren"

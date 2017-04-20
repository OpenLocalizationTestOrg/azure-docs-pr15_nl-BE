<properties
    pageTitle="Voorbereiding en opruimen in Batch Job | Microsoft Azure"
    description="Gebruik voorbereiding taakniveau taken minimaliseren gegevensoverdracht naar Azure Batch computerknooppunten en laat de taken voor het opschonen van knooppunt bij Taakvoltooiing."
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
    ms.date="09/16/2016"
    ms.author="marsma" />

# <a name="run-job-preparation-and-completion-tasks-on-azure-batch-compute-nodes"></a>Voorbereiding en voltooiing van taken uitvoeren op Azure Batch-computerknooppunten

 Een vorm van setup vereist een batchverwerking Azure vaak voordat de taken worden uitgevoerd en na onderhoud van taken als de taken zijn voltooid. Mogelijk moet u veelvoorkomende taak invoergegevens downloaden naar uw computerknooppunten of taak uitvoergegevens naar Azure opslag uploaden nadat de taak is voltooid. **Voorbereiding van de taak** en **taak vrijgeven** taken kunt u deze bewerkingen uitvoeren.

## <a name="what-are-job-preparation-and-release-tasks"></a>Wat zijn taak voorbereiding en laat u taken?

Taken van een taak uitvoeren, de projecttaak voorbereiding op alle computerknooppunten gepland ten minste één taak wordt uitgevoerd. Zodra de taak is voltooid, wordt de taak release uitgevoerd op elk knooppunt in de toepassingen die ten minste één taak uitgevoerd. Net als bij normale batchtaken, kunt u een opdrachtregel moet worden aangeroepen wanneer een preparaat of een release van een taak wordt uitgevoerd.

Voorbereiding en release projecttaken bieden vertrouwde functies van Batch taak zoals het downloaden van bestand ([bronbestanden][net_job_prep_resourcefiles]), verhoogde uitvoering, aangepaste omgevingsvariabelen uitvoering maximale duur, aantal nieuwe pogingen en de retentietijd bestand.

In de volgende gedeelten leert u hoe u met de [JobPreparationTask] [ net_job_prep] en [JobReleaseTask] [ net_job_release] klassen gevonden in [Batch.NET] [ api_net] bibliotheek.

> [AZURE.TIP] Voorbereiding en release taken zijn vooral handig in omgevingen "gedeelde toepassingen", waarin een groep computerknooppunten blijft bestaan tussen de taak wordt uitgevoerd en wordt gebruikt door veel taken.

## <a name="when-to-use-job-preparation-and-release-tasks"></a>Wanneer taak voorbereiding en laat u taken

Project opstellen en vrijgeven taken zijn een geschikt voor de volgende situaties:

**Algemene taakgegevens downloaden**

Batchtaken vereisen vaak een gemeenschappelijke set gegevens als invoer voor de taken van de taak. In de dagelijkse risico-analyse berekeningen is marktgegevens bijvoorbeeld taak-specifieke, nog gemeenschappelijk zijn voor alle taken in het project. Marktgegevens van deze, vaak verscheidene gigabytes groot, moet worden gedownload naar elk computerknooppunt slechts eenmaal zodat elke taak die wordt uitgevoerd op het knooppunt kunt gebruiken. Een **projecttaak voorbereiding** gebruiken deze gegevens downloaden naar elk knooppunt voordat de uitvoering van de taak's andere taken.

**Project en uitvoer verwijderen**

In een omgeving "gedeelde toepassingen", waarin computerknooppunten een groep van toepassingen niet bedrijf tussen taken zijn, moet u taakgegevens tussen twee uitvoeringen verwijderen. Mogelijk moet u voldoen aan het beveiligingsbeleid van uw organisatie of beschikbare schijfruimte op de knooppunten. Een **projecttaak release** gebruiken om gegevens die is gedownload door een projecttaak bereiding of gegenereerd tijdens de uitvoering van taken te verwijderen.

**Logboek bewaren**

U kunt bewaren een kopie van de logboekbestanden die uw taken genereren of misschien crashdump-bestanden die kunnen worden gegenereerd door de mislukte toepassingen. Een **projecttaak release** in dergelijke gevallen te comprimeren en deze gegevens uploaden naar een [Azure opslag] gebruiken[ azure_storage] account.

>[AZURE.TIP] Een andere manier om Logboeken en andere taak en taak de uitvoer van gegevens is het gebruik van de bibliotheek [Azure Batch bestand verdragen](batch-task-output.md) .

## <a name="job-preparation-task"></a>Voorbereiding van projecttaak

Batch: de projecttaak voorbereiding vóór uitvoering van de taken van een taak uitvoeren op elk computerknooppunt dat een taak is gepland. Standaard wacht de Batch-service voor de projecttaak voorbereiding moet worden voltooid voordat de taken die worden uitgevoerd op het knooppunt worden uitgevoerd. U kunt echter de service niet te wachten. Als het knooppunt opnieuw is opgestart, voorbereiding van de taak opnieuw wordt uitgevoerd, maar u kunt dit probleem ook uitschakelen.

De taak van het preparaat wordt alleen op de knooppunten die zijn gepland voor het uitvoeren van een taak uitgevoerd. Hiermee voorkomt u onnodige uitvoering van een taak voorbereiden voor het geval een knooppunt niet aan een taak toegewezen is. Dit kan gebeuren wanneer het aantal taken voor een taak kleiner dan het aantal knooppunten in een groep is. Ook van toepassing wanneer de [uitvoering van gelijktijdige taken](batch-parallel-node-tasks.md) is ingeschakeld, waarbij sommige knooppunten niet actief is als het aantal taken lager dan het totaal aantal mogelijke gelijktijdige taken is. Door de taak van het preparaat niet op niet-actieve knooppunten wordt uitgevoerd, kunt u minder geld besteden aan data transfer kosten.

> [AZURE.NOTE] [JobPreparationTask] [ net_job_prep_cloudjob] verschilt van [CloudPool.StartTask] [ pool_starttask] in deze JobPreparationTask wordt uitgevoerd aan het begin van elk project dat starttaak wordt uitgevoerd alleen als een compute node eerst lid van een groep of opnieuw wordt opgestart.

## <a name="job-release-task"></a>Taak vrijgeven

Zodra een taak is gemarkeerd als voltooid, wordt de taak van de release uitgevoerd op elk knooppunt in de toepassingen die ten minste één taak uitgevoerd. U kunt een taak markeren als voltooid door een terminate-request. De Batch-service vervolgens stelt de taakstatus te *beëindigen*, actieve of actieve taken die zijn gekoppeld aan de taak wordt beëindigd en de release-taak wordt uitgevoerd. Vervolgens wordt de taak verplaatst naar de status *voltooid* .

> [AZURE.NOTE] Verwijdering taak voert ook de projecttaak release. Echter als er al een taak is beëindigd, is de taak vrijgeven niet uitgevoerd een tweede keer als de taak wordt later verwijderd.

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Prep van taken en taken met .NET Batch release

Toewijzen voor het gebruik van een taak voor de voorbereiding van een [JobPreparationTask] [ net_job_prep] -object van het project [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] eigenschap. Een [JobReleaseTask] op dezelfde manier initialiseren[ net_job_release] en toewijzen aan de taak van de [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] eigenschap voor het instellen van de projecttaak release.

In dit codefragment `myBatchClient` is een exemplaar van de [BatchClient][net_batch_client], en `myPool` een bestaande toepassingen binnen de Batch-account.

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Zoals eerder vermeld, wordt de taak vrijgeven wordt uitgevoerd wanneer een taak wordt beëindigd of verwijderd. Beëindigen van een project met [JobOperations.TerminateJobAsync][net_job_terminate]. Verwijderen van een project met [JobOperations.DeleteJobAsync][net_job_delete]. U meestal beëindigen of een taak verwijderen als de taken zijn voltooid of wanneer een time-out die u hebt gedefinieerd, is bereikt.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Voorbeeld van code op GitHub

Bekijk project opstellen en vrijgeven van taken in actie, de [JobPrepRelease] [ job_prep_release_sample] voorbeeldproject op GitHub. Deze consoletoepassing gebeurt het volgende:

1. Een groep van toepassingen gemaakt met twee knooppunten voor 'klein'.
2. Maakt een taak met taak voorbereiding, release en standaardtaken.
3. De voorbereiding taak, die eerst de knooppunt-ID naar een tekstbestand in 'gedeelde' directory van een knooppunt schrijft wordt uitgevoerd.
4. Een taak wordt uitgevoerd op elk knooppunt, die de taak-ID naar hetzelfde tekstbestand schrijft.
5. Zodra alle taken zijn voltooid (of de time-out is bereikt), wordt de inhoud van het tekstbestand van elk knooppunt aan de console.
6. Wanneer de taak is voltooid, voert de projecttaak vrijgeven om het bestand verwijderen van het knooppunt.
6. De afsluitcodes van de voorbereiding en release taken voor elk knooppunt waarop uitgevoerd wordt.
7. Pauzes worden uitgevoerd zodat de bevestiging van de taak en/of de groep verwijderen.

Uitvoer van de voorbeeldtoepassing is vergelijkbaar met het volgende:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

>[AZURE.NOTE] Vanwege de variabele maken en begin tijd van de knooppunten in een nieuwe groep (sommige knooppunten gereed zijn voor taken voordat anderen), ziet u verschillende uitvoer. Met name omdat de taken snel uitvoeren, mogen een van de knooppunten van de groep van toepassingen uitvoeren alle taken van het project. In dat geval zult u zien dat het project voorbereiden en release taken bestaan niet voor het knooppunt dat geen taken uitgevoerd.

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>Taak voorbereiding en taken in de portal Azure versie controleren

Wanneer u de voorbeeldtoepassing uitvoert, kunt u de [Azure portal] [ portal] de eigenschappen van de taak en de bijbehorende taken weergeven of zelfs de gedeelde tekstbestand downloaden dat door de taken van de taak wordt gewijzigd.

Het screenshot hieronder toont de **bereiding taken blade** in Azure portal na een run van de voorbeeldtoepassing. Navigeer naar de eigenschappen van de *JobPrepReleaseSampleJob* nadat de taken hebt voltooid (maar voordat u uw project en toepassingen) en klik op **Voorbereidingstaken** of **taken Release** om hun eigenschappen te bekijken.

![Voorbereiding taakeigenschappen in Azure portal][1]

## <a name="next-steps"></a>Volgende stappen

### <a name="application-packages"></a>Toepassingspakketten

Naast de projecttaak voorbereiding kun je de functie voor [toepassingspakketten](batch-application-packages.md) van Batch-computerknooppunten voorbereiden voor uitvoering van taken. Deze functie is vooral handig voor het distribueren van toepassingen die niet met een installatieprogramma, toepassingen met veel (100-+) bestanden of toepassingen waarvoor een strikt versiebeheer nodig hebben.

### <a name="installing-applications-and-staging-data"></a>Installeren van toepassingen en gegevens in staging

Dit MSDN-forum post biedt een overzicht van de verschillende methoden voor het voorbereiden van de knooppunten voor het uitvoeren van taken:

[Installeren van toepassingen en gegevens voor Batch in staging-computerknooppunten][forum_post]

Geschreven door een van de teamleden Azure Batch, deze verschillende technieken die u gebruiken kunt voor de implementatie van toepassingen en gegevens berekent u knooppunten worden besproken.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png

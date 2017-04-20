<properties
    pageTitle="Persistentie in Azure Batch-project en uitvoer | Microsoft Azure"
    description="Informatie over het Azure opslag gebruiken als een duurzame winkel voor uw Batch-taak en taak uitgevoerd en deze permanente uitvoer bekijken in het portal voor Azure inschakelen."
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
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="persist-azure-batch-job-and-task-output"></a>Azure project en batchuitvoer aanhouden

De taken die u doorgaans in Batch uitvoeren produceren output die moet worden opgeslagen en later opgehaald door andere taken in het project, de clienttoepassing die de taak en/of uitgevoerd. Deze uitvoer kan worden bestanden die zijn gemaakt door de verwerking van ingevoerde gegevens of de logboekbestanden die zijn gekoppeld aan de uitvoering van taken. Dit artikel bevat een .NET klassenbibliotheek die gebruikmaakt van een Verdragen gebaseerde techniek om deze taak uitvoeren naar Azure Blob-opslag beschikbaar te maken nadat u uw toepassingen, projecten, verwijderen en de compute nodes.

Met behulp van de methode in dit artikel, kunt u de uitvoer van de taak ook bekijken in **logboekbestanden opgeslagen** in de [portal Azure]en **uitvoerbestanden opgeslagen** [portal].

![Saved uitvoerbestanden en opgeslagen logboeken selectors in portal][1]

>[AZURE.NOTE] De [Azure Batch bestand verdragen] [ nuget_package] .NET klassenbibliotheek die worden beschreven in dit artikel is momenteel in de voorvertoning. Sommige van de hier beschreven functies kunnen voor algemene beschikbaarheid veranderen.

## <a name="task-output-considerations"></a>Overwegingen bij uitvoer van taak

Wanneer u de Batch-oplossing ontwerpt, moet u rekening houden met verschillende factoren die verband houden met het project en uitvoer.

* **Knooppunt levensduur berekenen**: berekenen knooppunten zijn vaak tijdelijk, met name in toepassingen automatisch schalen is ingeschakeld. De uitgangen van de taken die worden uitgevoerd op een knooppunt zijn beschikbaar terwijl het knooppunt bestaat en alleen binnen de retentietijd bestand die u hebt ingesteld voor de taak. Om ervoor te zorgen dat de uitvoer van de taak blijft behouden, moeten uw taken daarom hun output-bestanden uploaden naar een duurzame archief, bijvoorbeeld Azure opslag.

* **Opslag van uitvoer**: persistent maken taak uitvoergegevens naar duurzame opslag, kunt u de [SDK van Azure opslag](../storage/storage-dotnet-how-to-use-blobs.md) in de taakcode voor het uploaden van de uitvoer van de taak naar een Blob-opslag container. Als u een container en naamgeving implementeert, uw clienttoepassing of andere taken in het project vervolgens zoeken en downloaden van deze uitvoer op basis van het Verdrag.

* **Uitvoer ophalen**: vindt u uitvoer van de taak rechtstreeks vanuit de compute nodes in uw groep of vanuit Azure opslag als uw taken hun uitvoer behouden. U haalt de output van een taak rechtstreeks vanuit een compute node, moet u de bestandsnaam en de uitvoerlocatie op het knooppunt. Als u uitvoer naar Azure opslag blijft bestaan, moet downstream taken of de clienttoepassing het volledige pad naar het bestand in de opslag te downloaden met behulp van de SDK Azure opslag Azure.

* **Uitvoer weergeven**: als u navigeren naar een batchtaak in de portal Azure en **bestanden op een knooppunt**selecteert, wordt weergegeven met alle bestanden die zijn gekoppeld aan de taak, niet alleen de uitvoerbestanden waarin je geïnteresseerd bent. Nogmaals, bestanden op computerknooppunten beschikbaar zijn terwijl het knooppunt bestaat en alleen binnen de retentietijd bestand die u hebt ingesteld voor de taak. Uitvoer van de taak die u naar Azure opslag hebt doorgevoerd in de portal of een toepassing als de [Azure Opslagverkenner]bekijken[storage_explorer], moet u de locatie weten en Ga rechtstreeks naar het bestand.

## <a name="help-for-persisted-output"></a>Help voor permanente uitvoer

Om u te helpen meer eenvoudig persistent project en uitvoer, de Batch-team heeft gedefinieerd en geïmplementeerd voor een reeks naamgevingsconventies als een .NET klassenbibliotheek, de [Azure Batch bestand verdragen] [ nuget_package] -bibliotheek, kunt u in de Batch-toepassingen. Bovendien is de Azure portal op de hoogte van deze naamgevingsregels zodat u gemakkelijk de bestanden die u hebt opgeslagen met behulp van de bibliotheek kunt vinden.

## <a name="using-the-file-conventions-library"></a>Met behulp van de bibliotheek bestand verdragen

[Azure Batch bestand verdragen] [ nuget_package] is een .NET klassenbibliotheek die uw Batch .NET toepassingen gemakkelijk opslaan en ophalen van taak uitgangen en naar Azure opslag kunnen gebruiken. Het is bedoeld voor gebruik in de taak- en client code--subactiviteit persistent maken van bestanden, en in de clientcode een lijst op te halen ze. Uw taken kunnen u ook de bibliotheek gebruiken voor het ophalen van de uitgangen van de upstream-taken, zoals in een scenario met [afhankelijkheden van taken](batch-task-dependencies.md) .

De bibliotheek verdragen zorgt voor ervoor te zorgen dat de recipiënten voor opslag en taak output bestanden hebben de naam van het Verdrag en worden geüpload naar de juiste plaats als permanente opslag Azure. Bij het ophalen van resultaten, kunt u gemakkelijk de uitgangen voor een bepaalde functie of taak weergeven of ophalen van de resultaten die door de ID en het doel, in plaats van op de hoogte van de bestandsnamen of indien deze bestaat in de opslag vinden.

Bijvoorbeeld, kunt u de bibliotheek "alle tussenliggende bestanden voor taak 7 list" of "get me het miniatuurvoorbeeld voor taak *MyMovie voor linkageName*," zonder te weten de bestandsnamen of een locatie in uw account voor opslag.

### <a name="get-the-library"></a>De bibliotheek ophalen

Kunt u de bibliotheek bevat nieuwe klassen en breidt het [CloudJob] [ net_cloudjob] en [CloudTask] [ net_cloudtask] klassen met nieuwe methoden vanaf [NuGet][nuget_package]. U kunt deze toevoegen aan de Visual Studio-project met de [NuGet Library Package Manager][nuget_manager].

>[AZURE.TIP] U vindt de [broncode] [ github_file_conventions] voor de bibliotheek Azure Batch bestand verdragen op GitHub in Microsoft Azure SDK voor .NET opslagplaats.

## <a name="requirement-linked-storage-account"></a>Eis: opslag van gekoppelde account

Uitgangen naar duurzame opslag met behulp van de bibliotheek van de verdragen bestand opslaan en deze bekijken in het portal voor Azure, moet u [een opslag Azure-account koppelen](batch-application-packages.md#link-a-storage-account) aan uw account voor de Batch. Als u nog niet gedaan hebt, een opslag-account koppelen aan je account Batch via de Azure portal:

Blade **batch-account** > **Instellingen** > **Opslag Account** > **Opslag Account** (geen) > een opslag selecteren in uw abonnement

Zie [implementatie van toepassingen met Azure Batch toepassingspakketten](batch-application-packages.md)voor een meer gedetailleerde overzicht op een opslag-account koppelen.

## <a name="persist-output"></a>Uitvoer behouden

Er zijn twee primaire acties uit te voeren bij het opslaan van project en uitvoer met de bibliotheek met de verdragen: de container opslag maken en opslaan van uitvoer naar de container.

>[AZURE.WARNING] Omdat alle project en resultaten worden opgeslagen in dezelfde container, kunnen [opslag beperking beperkingen](../storage/storage-performance-checklist.md#blobs) worden toegepast als een groot aantal taken probeert om bestanden op hetzelfde moment.

### <a name="create-storage-container"></a>Container opslag maken

Voordat uw taken uitvoer naar de opslag, moet u een blob-opslag container waarmee ze hun productie gaat uploaden. Dit doen door het aanroepen van [CloudJob][net_cloudjob]. [PrepareOutputStorageAsync] [net_prepareoutputasync]. Deze uitbreiding wordt een [CloudStorageAccount] [ net_cloudstorageaccount] als een parameter-object en maakt u een container op zodanige wijze dat de inhoud ervan kunnen gevonden worden door de Azure portal en de methoden voor het ophalen is besproken verderop in het artikel genoemd.

Gewoonlijk plaatst deze code in uw clienttoepassing--de toepassing die wordt gemaakt van uw toepassingen, projecten en taken.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Uitgangen van de taak opslaan

Nu dat u een container in de blobopslag hebt voorbereid, taken uitvoer kunnen opslaan op de container met behulp van de [TaskOutputStorage] [ net_taskoutputstorage] klasse gevonden in de bibliotheek met de verdragen.

In uw taakcode maakt eerst een [TaskOutputStorage] [ net_taskoutputstorage] -object wanneer de taak voltooid zijn werk, en roep vervolgens de [TaskOutputStorage][net_taskoutputstorage]. [SaveAsync] [net_saveasync] methode voor het opslaan van de uitvoer naar Azure opslag.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

De parameter "type uitvoer" categoriseert de permanente bestanden. Er zijn vier vooraf gedefinieerde [TaskOutputKind] [ net_taskoutputkind] type: 'TaskOutput', 'TaskPreview', 'TaskLog' en 'TaskIntermediate'. Ook kunt u aangepaste typen als ze in uw werkstroom nuttig zou zijn.

Deze typen uitvoer kunnen u opgeven welk type uitgangen worden weergegeven wanneer u later een query Batch voor de permanente uitgangen van een bepaalde taak uitvoeren. Met andere woorden, wanneer je de uitgangen voor een taak, kunt u de lijst op een van de typen uitvoer filteren. Bijvoorbeeld: 'Geef me de uitvoer van het *voorbeeld* voor taak *109*." Meer weergegeven op aanbieden en het ophalen van resultaten in [uitvoer halen](#retrieve-output) later in dit artikel.

>[AZURE.TIP] Geeft het type uitvoer ook waar in de Azure portal voor een bepaald bestand wordt weergegeven: *TaskOutput*-gecategoriseerde bestanden worden weergegeven in "Taak uitvoerbestanden" en *TaskLog* -bestanden worden weergegeven in 'Taak logs'.

### <a name="store-job-outputs"></a>Uitgangen van de taak opslaan

Naast de taak uitgangen op te slaan, kunt u de resultaten die zijn gekoppeld aan een volledig project opslaan. In het samenvoegen van een film weergave taak, kunt u de film volledig kan behouden als de uitvoer van een taak. Wanneer de taak is voltooid, de clienttoepassing gewoon kunt weergeven en ophalen van de uitgangen voor het project, en hoeft niet te vragen van de afzonderlijke taken.

Uitvoer van de taak opslaan door het aanroepen van de [JobOutputStorage][net_joboutputstorage]. [SaveAsync] [net_joboutputstorage_saveasync] -methode en geeft de [JobOutputKind] [ net_joboutputkind] en de bestandsnaam:

```
CloudJob job = await batchClient.JobOperations.GetJobAsync(jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Als u met de TaskOutputKind voor de uitvoer van de taak, gebruikt u de [JobOutputKind] [ net_joboutputkind] parameter voor het categoriseren van een taak de bestanden behouden. Deze parameter kunt u later query voor (lijst) een specifiek type van uitvoer. De JobOutputKind bestaat uit zowel output- als voorbeeld-typen en ondersteunt het maken van aangepaste typen.

### <a name="store-task-logs"></a>Logboeken van de taak opslaan

Naast het persistent maken een bestand naar een duurzame opslag wanneer een taak of een taak is voltooid, vindt u het misschien nodig om bestanden die worden bijgewerkt tijdens het uitvoeren van een taak--logboekbestanden of `stdout.txt` en `stderr.txt`, bijvoorbeeld. Voor dit doel, de bibliotheek Azure Batch bestand verdragen biedt de [TaskOutputStorage][net_taskoutputstorage]. [SaveTrackedAsync] [net_savetrackedasync] methode. Met [SaveTrackedAsync][net_savetrackedasync], kunt u bijhouden van updates naar een bestand op het knooppunt (met een interval dat u opgeeft) en updates naar Azure opslag aanhouden.

In het volgende codefragment gebruiken we [SaveTrackedAsync] [ net_savetrackedasync] voor het bijwerken van `stdout.txt` in Azure opslag elke 15 seconden tijdens de uitvoering van de taak:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
    await Task.Delay(stdoutFlushDelay);
}
```

`Code to process data and produce output file(s)`is gewoon een tijdelijke aanduiding voor de code die u normaal uw taak kunt uitvoeren. Zo mogelijk code die u downloadt gegevens via Azure opslag en transformatie of berekening op deze uitvoert. Het belangrijkste gedeelte van dit fragment is om aan te tonen hoe u kunt laten lopen deze code in een `using` blok regelmatig bijwerken van een bestand met [SaveTrackedAsync][net_savetrackedasync].

De `Task.Delay` is verplicht aan het einde van dit `using` blokkeren om ervoor te zorgen dat de agent knooppunt tijd heeft legen van de inhoud van de standaard-out naar het bestand stdout.txt op het knooppunt (het knooppunt agent is een programma dat wordt uitgevoerd op elk knooppunt in de groep en de command en control interface vormt tussen het knooppunt en de Batch-service). Onverwijld is het mogelijk te missen van de laatste paar seconden van de uitvoer. Deze vertraging kan niet worden vereist voor alle bestanden.

>[AZURE.NOTE] Azure opslag blijven alleen worden *toegevoegd* aan het bestand bijgehouden wanneer u een bestand bijhouden met SaveTrackedAsync inschakelt, bewaard. Gebruik deze methode alleen voor het bijhouden van logboekbestanden niet draaien of andere bestanden die worden toegevoegd aan, dat wil, gegevens wordt alleen toegevoegd aan het einde van het bestand wordt bijgewerkt.

## <a name="retrieve-output"></a>Uitvoer ophalen

Wanneer u de blijvende uitvoer met behulp van de bibliotheek van de conventies van Azure Batch-bestand ophaalt, kunt u dat doen op een taak - en taak-georiënteerde manier. U kunt aangeven dat de uitvoer voor de gegeven taak of taak zonder te weten het pad in de blob-opslag of zelfs de naam van het bestand. U kunt gewoon zeggen, "Geef me de uitvoerbestanden voor taak *109*."

Het volgende codefragment doorloopt alle taken van een taak, wordt informatie over de uitvoerbestanden voor de taak wordt afgedrukt en downloadt vervolgens de bestanden uit de opslag.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (TaskOutputStorage output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="task-outputs-and-the-azure-portal"></a>Uitgangen van de taak en de Azure portal

De Azure portal logboeken met persistente en uitgangen van de taak wordt weergegeven in een gekoppelde opslag Azure-account met behulp van de gevonden in het [Leesmij-bestand voor Azure Batch bestand conventies]voor naamgeving[github_file_conventions_readme]. U kunt ook zelf deze conventies implementeren in een taal van uw keuze of kunt u de bibliotheek met conventies in de .NET-toepassingen.

### <a name="enable-portal-display"></a>Inschakelen van de portal weergeven

Zodat de weergave van de resultaten die in de portal moet u aan de volgende eisen voldoen:

 1. [Een opslag Azure-account koppelen](#requirement-linked-storage-account) aan uw account Batch.
 2. Voldoen aan de vooraf gedefinieerde naamgeving van bestanden en Opslagcontainers wanneer uitgangen persistent maken. Vindt u de definitie van deze verdragen in de BESTANDSBIBLIOTHEEK verdragen [Leesmij-bestand][github_file_conventions_readme]. Als u de [Azure Batch bestand verdragen] [ nuget_package] bibliotheek om de uitvoer van deze eis is voldaan.

### <a name="view-outputs-in-the-portal"></a>Uitgangen in de portal weergeven

Om Logboeken en uitgangen van de taak weergeven in de portal Azure, gaat u naar de taak waarvan de uitvoer u geïnteresseerd bent in en klik op **opgeslagen uitvoerbestanden** of **opgeslagen logboeken**. Deze afbeelding toont de **uitvoerbestanden opgeslagen** voor de taak met de ID "007":

![Taak uitgangen blade in Azure portal][2]

## <a name="code-sample"></a>Voorbeeld van code

De [PersistOutputs] [ github_persistoutputs] voorbeeldproject is een van de [codevoorbeelden Azure Batch] [ github_samples] op GitHub. Deze oplossing met Visual Studio 2015 wordt gedemonstreerd hoe de bibliotheek Azure Batch bestand conventies gebruiken om de uitvoer van de taak naar een duurzame opslag. Het voorbeeld alleen uitvoeren, als volgt te werk:

1. Open het project in **Visual Studio 2015**.
2. De Batch- en **accountreferenties** toevoegen aan **AccountSettings.settings** in het Microsoft.Azure.Batch.Samples.Common-project.
3. **Bouwen** (maar niet worden uitgevoerd) de oplossing. Alle pakketten NuGet herstellen als daarom wordt gevraagd.
4. De Azure portal gebruiken voor het uploaden van een [toepassingspakket](batch-application-packages.md) voor **PersistOutputsTask**. Omvatten de `PersistOutputsTask.exe` en de afhankelijke assembly's in het ZIP-pakket, de toepassings-ID aan 'PersistOutputsTask' en de pakket-versie van de toepassing "1.0".
5. **Start** project (uitvoeren) de **PersistOutputs** .

## <a name="next-steps"></a>Volgende stappen

### <a name="application-deployment"></a>Implementatie van toepassingen

De functie voor [toepassingspakketten](batch-application-packages.md) batch biedt een eenvoudige manier om beide te implementeren en toepassingen die uw taken uitvoeren op computerknooppunten versie.

### <a name="installing-applications-and-staging-data"></a>Installeren van toepassingen en gegevens in staging

Bekijk de [toepassingen installeren en tijdelijke gegevens op Batch computerknooppunten] [ forum_post] boeken in het forum Batch Azure voor een overzicht van de verschillende methoden voor het voorbereiden van de knooppunten voor het uitvoeren van taken. Geschreven door een van de teamleden Azure Batch, is dit bericht een goede primer op de verschillende manieren om bestanden (waaronder zowel toepassingen als taak invoergegevens) naar de compute nodes, en sommige speciale overwegingen voor elke methode.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Saved uitvoerbestanden en opgeslagen logboeken selectors in portal"
[2]: ./media/batch-task-output/task-output-02.png "Taak uitgangen blade in Azure portal"
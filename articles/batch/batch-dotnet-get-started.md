<properties
    pageTitle="Zelfstudie - aan de slag met de bibliotheek Azure Batch .NET | Microsoft Azure"
    description="Informatie over de basisbegrippen van Azure Batch en het ontwikkelen van de Batch-service met een voorbeeldscenario."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="08/15/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-library-for-net"></a>Aan de slag met de bibliotheek Azure Batch voor .NET

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Leer de grondbeginselen van [Azure Batch] [ azure_batch] en de [Batch.NET] [ net_api] library in dit artikel als u een C#-voorbeeldtoepassing stap voor stap besproken. Bekijken we hoe deze voorbeeldtoepassing maakt gebruik van de Batch-service voor het verwerken van een parallelle werkbelasting in de cloud, alsmede hoe deze met [Azure opslag](../storage/storage-introduction.md) voor het tijdelijke bestand en ophalen samenwerkt. U leert veelgebruikte Batch toepassing workflow technieken. U zult ook krijgen een basis kennis van de belangrijkste onderdelen van de partij, zoals projecten, taken, toepassingen, en de compute nodes.

![Workflow voor batch-oplossing (basic)][11]<br/>

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat er een praktische kennis van C# en Visual Studio. Ook wordt ervan uitgegaan dat u kunnen voldoen aan de account maken die bent onder zijn opgegeven voor Azure en de Batch- en services.

### <a name="accounts"></a>Rekeningen

- **Azure-account**: als u niet al een Azure-abonnement, [Maak een gratis account Azure][azure_free_account].
- **Batch-account**: zodra er een Azure-abonnement, [een Batch Azure-account maken](batch-account-create-portal.md).
- **Opslag account**: Zie [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) in [Azure over opslag rekeningen](../storage/storage-create-storage-account.md).

> [AZURE.IMPORTANT] Batch ondersteunt momenteel *alleen* **algemene** opslag accounttype, zoals beschreven in stap #5 [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) in [Azure over opslag rekeningen](../storage/storage-create-storage-account.md).

### <a name="visual-studio"></a>Visual Studio

U hebt in **Visual Studio 2015** het voorbeeldproject te maken. Gratis of op proef versies van Visual Studio vindt u in het [overzicht van de producten in Visual Studio 2015][visual_studio].

### <a name="dotnettutorial-code-sample"></a>Voorbeeld *DotNetTutorial*

De [DotNetTutorial] [ github_dotnettutorial] monster is een van de vele voorbeelden van code gevonden in de [voorbeelden van azure batch] [ github_samples] bibliotheek op GitHub. U kunt het monster downloaden door te klikken op de knop **ZIP-Download** op de introductiepagina van de opslagplaats, of door te klikken op de [azure batch-monsters master.zip] [ github_samples_zip] directe koppeling. Als u de inhoud van het ZIP-bestand hebt uitgepakt, vindt u de oplossing in de volgende map:

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Azure Batch Explorer (optioneel)

De [Azure Batch Explorer] [ github_batchexplorer] is een gratis hulpprogramma dat is opgenomen in de [voorbeelden van azure batch] [ github_samples] bibliotheek op GitHub. Hoewel niet vereist voor het voltooien van deze zelfstudie, kan het nuttig zijn bij het ontwikkelen en debuggen van uw Batch-oplossingen.

## <a name="dotnettutorial-sample-project-overview"></a>DotNetTutorial monster project-overzicht

*DotNetTutorial* in het voorbeeld is een oplossing met Visual Studio 2015 die uit twee projecten bestaat: **DotNetTutorial** en **TaskApplication**.

- **DotNetTutorial** is de clienttoepassing die samenwerkt met de Batch- en services voor het uitvoeren van een parallelle werkbelasting op computerknooppunten (virtuele machines). DotNetTutorial wordt uitgevoerd op uw lokale werkstation.

- **TaskApplication** is het programma dat wordt uitgevoerd op computerknooppunten in Azure voor het uitvoeren van de werkelijke hoeveelheid werk. In het voorbeeld `TaskApplication.exe` de tekst in een bestand wordt gedownload van Azure opslag (invoerbestand) parseert. Vervolgens is het resultaat een tekstbestand (de uitvoer) met een lijst van de bovenste drie woorden die worden weergegeven in het invoerbestand. Nadat het uitvoerbestand is gemaakt, TaskApplication het bestand geüpload naar de opslag van Azure. Dit maakt het voor de clienttoepassing voor downloaden beschikbaar. TaskApplication parallel op meerdere computerknooppunten in de Batch-service wordt uitgevoerd.

In het volgende diagram ziet u de primaire bewerkingen die worden uitgevoerd door de clienttoepassing, *DotNetTutorial*en de toepassing die wordt uitgevoerd door de taken, de *TaskApplication*. Deze eenvoudige werkstroom is typisch voor veel compute-oplossingen die zijn gemaakt met de batchverwerking. Terwijl hij alle functies die beschikbaar zijn in de Batch-service niet aantonen heeft, ook bijna elke Batch geldt voor soortgelijke processen.

![Batch voorbeeldwerkstroom][8]<br/>

[**Stap 1.**](#step-1-create-storage-containers) **Containers** in Azure Blob-opslag maken.<br/>
[**Stap 2.**](#step-2-upload-task-application-and-data-files) Taak toepassingsbestanden en input bestanden uploaden naar containers.<br/>
[**Stap 3.**](#step-3-create-batch-pool) Een Batch- **toepassingen**maken.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** De groep **starttaak** downloads de binaire bestanden (TaskApplication) voor knooppunten als zij deelnemen aan de pool.<br/>
[**Stap 4.**](#step-4-create-batch-job) Maak een Batch **job**.<br/>
[**Stap 5.**](#step-5-add-tasks-to-job) **Taken** toevoegen aan het project.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** De taken worden worden uitgevoerd op de knooppunten gepland.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Elke taak de invoergegevens van Azure opslag gedownload en vervolgens wordt uitgevoerd.<br/>
[**Stap 6.**](#step-6-monitor-tasks) Taken controleren.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Als de taken zijn voltooid, uploaden ze hun uitvoergegevens naar Azure opslag.<br/>
[**Stap 7.**](#step-7-download-task-output) Download de uitvoer van de taak van opslag.

Zoals gezegd, niet elke Batch oplossing deze exacte stappen uitvoert en eventueel ook nog veel meer, maar de *DotNetTutorial* -voorbeeldtoepassing toont gemeenschappelijke processen in een Batch-oplossing gevonden.

## <a name="build-the-dotnettutorial-sample-project"></a>Het voorbeeldproject *DotNetTutorial* bouwen

Voordat u het voorbeeld met succes uitvoeren kunt, moet u accountreferenties Batch- en opslag in de *DotNetTutorial* van het project `Program.cs` bestand. Als u dit nog niet hebt gedaan, opent u de oplossing in Visual Studio door te dubbelklikken op de `DotNetTutorial.sln` solution-bestand. Of vanuit Visual Studio openen met behulp van de **Bestand > openen > Project-oplossing** menu.

Open `Program.cs` in het *DotNetTutorial* -project. Vervolgens voegt u toe uw referenties opgegeven bij de bovenkant van het bestand:

```
// Update the Batch and Storage account credential strings below with the values
// unique to your accounts. These are used when constructing connection strings
// for the Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [AZURE.IMPORTANT] Zoals hierboven vermeld, moet u de referenties voor de account van een **algemene** opslag op dat moment in Azure opslag opgeven. Uw toepassingen Batch blob-opslag in de account van de opslag **voor algemene doeleinden** gebruiken. Geeft u de referenties voor de account van een opslag die is gemaakt door de *Blob-opslag* accounttype te selecteren.

Kunt u uw referenties van Batch- en in het blad van de account van elke service vinden in de [Azure portal][azure_portal]:

![Batch-referenties in de portal][9]
![opslag van referenties in de portal][10]<br/>

U hebt het project bijgewerkt met uw referenties, de oplossing in de Solution Explorer met de rechtermuisknop en klikt u op de **Oplossing bouwen**. Het herstel van alle pakketten NuGet bevestigen als u wordt gevraagd.

> [AZURE.TIP] Als de NuGet pakketten niet automatisch worden hersteld, of als er fouten over een fout te herstellen van de pakketten, zorg ervoor dat de [NuGet Package Manager] [ nuget_packagemgr] geïnstalleerd. Schakel het downloaden van ontbrekende pakketten. Zie [Inschakelen pakket herstellen tijdens het bouwen van] [ nuget_restore] pakket download inschakelen.

In de volgende secties we onderverdelen in de voorbeeldtoepassing in de stappen die worden uitgevoerd voor het verwerken van een werkbelasting in de Batch-service en de stappen in detail te bespreken. We raden u om te verwijzen naar de geopende oplossing in Visual Studio, terwijl u uw manier door de rest van dit artikel, werkt omdat niet elke regel code in het monster wordt besproken.

Ga naar de bovenkant van de `MainAsync` -methode in de *DotNetTutorial* van het project `Program.cs` bestand om te beginnen met stap 1. Elke stap hieronder dan ongeveer volgt de voortgang van het methodeaanroepen in `MainAsync`.

## <a name="step-1-create-storage-containers"></a>Stap 1: Maak Storage containers

![Containers in Azure opslag maken][1]
<br/>

Batch beschikt over ingebouwde ondersteuning voor interactie met de opslag van Azure. Containers in uw account opslag krijgt u de bestanden die nodig zijn voor de taken die worden uitgevoerd in uw account voor de Batch. De containers bevatten ook een plaats voor de worden uitvoergegevens opgeslagen die de taken produceren. Allereerst is de clienttoepassing *DotNetTutorial* is drie containers maken in [Azure Blob-opslag](../storage/storage-introduction.md):

- **toepassing**: deze container de toepassing die wordt uitgevoerd door de taken, evenals alle bijbehorende afhankelijkheden, zoals dll-bestanden worden opgeslagen.
- **invoer**: taken de gegevensbestanden voor het verwerken van de *invoer* container wordt gedownload.
- **output**: als invoerbestand verwerking taken hebt voltooid, zal zij de resultaten uploaden naar de container voor de *uitvoer* .

We gebruiken om te communiceren met een account voor opslag en containers maken, de [Azure opslag Client Library for .NET][net_api_storage]. We maken een verwijzing naar de rekening met [CloudStorageAccount][net_cloudstorageaccount], en maak een [CloudBlobClient]van die[net_cloudblobclient]:

```csharp
// Construct the Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve the storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create the blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

We gebruiken de `blobClient` referentie in de toepassing en als parameter doorgegeven aan de verschillende methoden. Een voorbeeld hiervan is in het codeblok die onmiddellijk volgt op het bovenstaande waar noemen we `CreateContainerIfNotExistAsync` daadwerkelijk maken van de containers.

```csharp
// Use the blob client to create the containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

Nadat de containers zijn gemaakt, kan de toepassing nu de bestanden die worden gebruikt door de taken uploaden.

> [AZURE.TIP] [Hoe u Blob-opslag van .NET](../storage/storage-dotnet-how-to-use-blobs.md) biedt een goed overzicht van het werken met Azure opslagtanks en BLOB's. Moeten worden aan de bovenkant van de lijst lezen als u begint te werken met Batch.

## <a name="step-2-upload-task-application-and-data-files"></a>Stap 2: De toepassings- en bestanden uploaden

![Toepassing van de taak en de invoer (gegevens) bestanden uploaden naar containers][2]
<br/>

In de bewerking voor het uploaden van bestanden definieert *DotNetTutorial* eerst u verzamelingen van **toepassing** en een **invoer** paden die op de lokale computer zijn. Deze vervolgens uploadt deze bestanden naar de containers die u in de vorige stap hebt gemaakt.

```
// Paths to the executable and its dependencies that will be executed by the tasks
List<string> applicationFilePaths = new List<string>
{
    // The DotNetTutorial project includes a project reference to TaskApplication,
    // allowing us to determine the path of the task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// The collection of data files that are to be processed by the tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload the application and its dependencies to Azure Storage. This is the
// application that will process the data files, and will be executed by each
// of the tasks on the compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload the data files. This is the data that will be processed by each of
// the tasks that are executed on the compute nodes within the pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

Er zijn twee methoden in `Program.cs` die betrokken zijn bij het uploadproces:

- `UploadFilesToContainerAsync`: Met deze methode geeft als resultaat een collectie van [ResourceFile] [ net_resourcefile] -objecten (Zie hieronder) en intern oproepen `UploadFileToContainerAsync` voor het uploaden van elk bestand dat wordt doorgegeven in de parameter *filePaths* .
- `UploadFileToContainerAsync`: Dit is de methode die daadwerkelijk uitvoert het bestand te uploaden en maakt de [ResourceFile] [ net_resourcefile] objecten. Na het uploaden van het bestand, het verkrijgt een gedeelde access-handtekening (SAS) voor het bestand en geeft als resultaat een ResourceFile-object dat aangeeft. Gedeelde toegang handtekeningen ook hieronder worden besproken.

```
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} to container [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath, FileMode.Open);

        // Set the expiry time and permissions for the blob shared access signature.
        // In this case, no start time is specified, so the shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct the SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles

Een [ResourceFile] [ net_resourcefile] vindt u de taken in een Batch met de URL naar een bestand in Azure opslag die is gedownload naar een compute node voordat de taak wordt uitgevoerd. De [ResourceFile.BlobSource] [ net_resourcefile_blobsource] eigenschap geeft u de volledige URL van het bestand zoals dit in Azure opslag. De URL kan ook een gedeelde access-handtekening (SAS) die veilige toegang tot het bestand biedt. De meeste taken in Batch .NET zijn een eigenschap van de *ResourceFiles* , met inbegrip van:

- [CloudTask][net_task]
- [Starttaak][net_pool_starttask]
- [JobPreparationTask][net_jobpreptask]
- [JobReleaseTask][net_jobreltask]

De voorbeeldtoepassing DotNetTutorial maakt geen gebruik van de taaktypen JobPreparationTask of JobReleaseTask, maar u kunt meer lezen over hen in [computerknooppunten uitvoeren voorbereiding en voltooiing van taken op Azure Batch](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Gedeelde toegang handtekening (SAS)

Gedeelde toegang handtekeningen zijn tekenreeksen die, als onderdeel van een URL, veilige toegang bieden tot containers en BLOB's in Azure opslag. De toepassing maakt gebruik van zowel blob en gedeelde container DotNetTutorial toegang tot URL's handtekening en laat zien hoe u deze gedeelde toegang handtekening tekenreeksen van de Storage-service verkrijgen.

- **BLOB gedeelde toegang handtekeningen**: starttaak in DotNetTutorial van de groep blob gedeelde toegang handtekeningen wordt gebruikt wanneer het downloaden van de toepassing binaire bestanden en bestanden van de ingevoerde gegevens uit de opslag (Zie stap #3 hieronder). De `UploadFileToContainerAsync` -methode in de DotNetTutorial `Program.cs` bevat de code die een handtekening voor elke blob van gedeelde toegang verkrijgt. Dit gebeurt door het aanroepen van [CloudBlob.GetSharedAccessSignature][net_sas_blob].

- **Container gedeelde toegang handtekeningen**: zoals elke taak het werk op de compute node heeft voltooid, het het output-bestand uploadt naar de container *uitvoer* in Azure opslag. TaskApplication gebruikt hiervoor een container gedeelde toegang handtekening met schrijftoegang tot de container als deel van het pad wanneer deze het bestand uploadt. Het verkrijgen van de handtekening van de gedeelde toegang container gedaan op soortgelijke wijze als bij het verkrijgen van de blob gedeeld access handtekening. In DotNetTutorial, zult u zien dat de `GetContainerSasUrl` helpmethode roept [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] te doen. U zult lezen meer informatie over het gebruik van de container in TaskApplication gedeelde toegang handtekening in ' stap 6: monitortaken. "

> [AZURE.TIP] De tweedelige serie over gedeelde toegang handtekeningen, uitchecken [deel 1: inzicht in het model van de handtekening (SAS) gedeelde toegang](../storage/storage-dotnet-shared-access-signature-part-1.md) en [deel 2: maken en gebruiken van een gedeelde access-handtekening (SAS) met Blob-opslag](../storage/storage-dotnet-shared-access-signature-part-2.md), voor meer informatie over een beveiligde toegang tot de gegevens in uw account voor opslag.

## <a name="step-3-create-batch-pool"></a>Stap 3: Maak een Batch van toepassingen

![Een Batch-toepassingen maken][3]
<br/>

Een Batch- **groep** is een verzameling van computerknooppunten (virtuele machines) op welke Batch de taken van een taak uitvoeren.

*DotNetTutorial* gestart nadat het de toepassings- en bestanden kunnen naar de rekening voor de opslag uploaden, de interactie met de Batch-service met behulp van de Batch .NET library. Om dit te doen, een [BatchClient] [ net_batchclient] wordt gemaakt:

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

Vervolgens wordt een groep computerknooppunten gemaakt in de Batch-account met een aanroep van `CreatePoolAsync`. `CreatePoolAsync`maakt gebruik van de [BatchClient.PoolOperations.CreatePool] [ net_pool_create] methode voor het maken van een groep daadwerkelijk in de Batch-service.

```csharp
private static async Task CreatePoolAsync(
    BatchClient batchClient,
    string poolId,
    IList<ResourceFile> resourceFiles)
{
    Console.WriteLine("Creating pool [{0}]...", poolId);

    // Create the unbound pool. Until we call CloudPool.Commit() or CommitAsync(),
    // no pool is actually created in the Batch service. This CloudPool instance is
    // therefore considered "unbound," and we can modify its properties.
    CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicated: 3,           // 3 compute nodes
            virtualMachineSize: "small",  // single-core, 1.75 GB memory, 224 GB disk
            cloudServiceConfiguration:
                new CloudServiceConfiguration(osFamily: "4")); // Win Server 2012 R2

    // Create and assign the StartTask that will be executed when compute nodes join
    // the pool. In this case, we copy the StartTask's resource files (that will be
    // automatically downloaded to the node by the StartTask) into the shared
    // directory that all tasks will have access to.
    pool.StartTask = new StartTask
    {
        // Specify a command line for the StartTask that copies the task application
        // files to the node's shared directory. Every compute node in a Batch pool
        // is configured with several pre-defined environment variables that you can
        // reference by using commands or applications run by tasks.

        // Since a successful execution of robocopy can return a non-zero exit code
        // (e.g. 1 when one or more files were successfully copied) we need to
        // manually exit with a 0 for Batch to recognize StartTask execution success.
        CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
        ResourceFiles = resourceFiles,
        WaitForSuccess = true
    };

    await pool.CommitAsync();
}
```

Als u een groep maakt met [CreatePool][net_pool_create], geeft u verschillende parameters, zoals het nummer van de compute nodes, de [grootte van de knooppunten](../cloud-services/cloud-services-sizes-specs.md)en de knooppunten besturingssysteem. *DotNetTutorial*, gebruiken we [CloudServiceConfiguration] [ net_cloudserviceconfiguration] Windows Server 2012 R2 van [Cloud Services](../cloud-services/cloud-services-guestos-update-matrix.md)opgeven. Echter door te geven van een [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] in plaats daarvan kunt u groepen van knooppunten die zijn gemaakt op basis van afbeeldingen op Marketplace, waaronder zowel Windows als Linux images — Zie [bepaling Linux computerknooppunten in Azure Batch van toepassingen](batch-linux-nodes.md) voor meer informatie.

> [AZURE.IMPORTANT] Worden in rekening gebracht voor resources in Batch berekenen. U kunt verlagen om de kosten te minimaliseren, `targetDedicated` 1 voordat u het voorbeeld uitvoert.

Met deze eigenschappen van het fysieke knooppunt, kunt u ook opgeven een [starttaak] [ net_pool_starttask] voor de groep. De starttaak wordt uitgevoerd op elk knooppunt dat knooppunt wordt toegevoegd aan de groep, en elke keer dat een knooppunt opnieuw is gestart. De starttaak is vooral handig voor het installeren van toepassingen op computerknooppunten voorafgaande aan de uitvoering van taken. Bijvoorbeeld, als uw taken verwerkt de gegevens met behulp van Python scripts, kunt u een starttaak Python installeren op de compute nodes.

In deze voorbeeldtoepassing de starttaak kopieert de bestanden die het downloaden van de opslag (die zijn opgegeven met behulp van de [starttaak][net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] eigenschap) uit de directory starttaak werken met de gedeelde map die u toegang *alle* taken op het knooppunt tot. In feite wordt Hiermee kopieert u `TaskApplication.exe` en bijbehorende afhankelijkheden met de gedeelde directory op elk knooppunt als het knooppunt lid van de groep, zodat alle taken die worden uitgevoerd op het knooppunt toegang.

> [AZURE.TIP] De functie **toepassingspakketten** batch Azure biedt een andere manier om uw toepassing naar de compute nodes in een groep. Zie de [implementatie van toepassingen met Azure Batch toepassingspakketten](batch-application-packages.md) voor meer informatie.

Let op in het bovenstaande codefragment is ook het gebruik van twee omgevingsvariabelen in de eigenschap *CommandLine* van de starttaak: `%AZ_BATCH_TASK_WORKING_DIR%` en `%AZ_BATCH_NODE_SHARED_DIR%`. Elk computerknooppunt in een Batch-groep wordt automatisch geconfigureerd met verschillende omgevingsvariabelen die specifiek voor de Batch zijn. Een proces dat door een taak is uitgevoerd, heeft toegang tot deze omgevingsvariabelen.

> [AZURE.TIP] Ga voor meer informatie over de omgeving Raadpleeg variabelen die beschikbaar op de computerknooppunten in een Batch van toepassingen en informatie over mappen taak werken zijn, de [omgevingsinstellingen voor taken](batch-api-basics.md#environment-settings-for-tasks) en [bestanden en mappen](batch-api-basics.md#files-and-directories) in het [overzicht van Batch-functie voor ontwikkelaars](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Stap 4: Maak een batchverwerking

![Batchverwerking maken][4]<br/>

Een Batch- **taak** is een verzameling taken en is gekoppeld aan een groep met computerknooppunten. De taken in een project worden uitgevoerd op de bijbehorende toepassingen computerknooppunten.

U kunt een taak niet alleen voor het ordenen en bijhouden van taken in de bijbehorende werklast, maar ook voor het opleggen van bepaalde beperkingen--zoals de maximale runtime voor de taak (en door uitbreiding van haar taken) en de prioriteit van de taak ten opzichte van andere taken in de Batch-account gebruiken. In dit voorbeeld wordt is de taak echter alleen gekoppeld aan de groep die is gemaakt in stap #3. Er zijn geen aanvullende eigenschappen geconfigureerd.

Alle taken zijn gekoppeld aan een specifieke groep. Deze koppeling geeft aan welke taken van de taak wordt uitgevoerd op knooppunten. Geef in dit met behulp van de [CloudJob.PoolInformation] [ net_job_poolinfo] eigenschap, zoals in het onderstaande stukje code.

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

Nu dat een taak is gemaakt, worden de taken toegevoegd voor het uitvoeren van het werk.

## <a name="step-5-add-tasks-to-job"></a>Stap 5: Taken toevoegen aan project

![Taken toevoegen aan project][5]<br/>
*(1) taken toegevoegd aan het project, (2) de taken uitvoeren op knooppunten worden gepland en (3) de taken voor het verwerken van bestanden downloaden*

Batch **taken** zijn de afzonderlijke eenheden van werk uitvoeren op de compute nodes. Een taak is een opdrachtregel en scripts of uitvoerbare bestanden die u in die regel opdracht uitgevoerd.

Werkelijk werk uitvoeren, moeten de taken worden toegevoegd aan een taak. Elke [CloudTask] [ net_task] is geconfigureerd met behulp van een eigenschap voor de opdrachtregel en [ResourceFiles] [ net_task_resourcefiles] (net als bij de groep van toepassingen starttaak) die de taak kan worden gedownload naar het knooppunt voordat u de opdrachtregel wordt automatisch uitgevoerd. Elke taak verwerkt in het voorbeeldproject *DotNetTutorial* slechts één bestand. Dus bevat de ResourceFiles-collectie een enkel element.

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks to job [{1}]...", inputFiles.Count, jobId);

    // Create a collection to hold the tasks that we'll be adding to the job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of the tasks. Because we copied the task application to the
    // node's shared directory with the pool's StartTask, we can access it via
    // the shared directory on the node that the task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add the tasks as a collection, as opposed to issuing a separate AddTask call
    // for each. Bulk task submission helps to ensure efficient underlying API calls
    // to the Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [AZURE.IMPORTANT] Wanneer zij toegang krijgen tot omgevingsvariabelen, zoals `%AZ_BATCH_NODE_SHARED_DIR%` of het uitvoeren van een toepassing niet gevonden in van het knooppunt `PATH`, opdrachtregels taak moet worden voorafgegaan door `cmd /c`. Dit expliciet de opdracht-interpreter uitvoeren en instructie te beëindigen na uitvoering van de opdracht. Deze eis is niet nodig als uw taken uitvoert van een toepassing in van het knooppunt `PATH` (zoals *robocopy.exe* of *powershell.exe*) en geen omgevingsvariabelen worden gebruikt.

In de `foreach` de lus in het bovenstaande stukje code, kunt u zien dat de opdrachtregel voor de taak is opgebouwd, dat drie argumenten voor de opdrachtregel worden doorgegeven aan *TaskApplication.exe*:

1. Het **eerste argument** is het pad van het bestand te verwerken. Dit is het lokale pad naar het bestand zoals dit op het knooppunt. Wanneer het ResourceFile-object `UploadFileToContainerAsync` is gemaakt, de bestandsnaam is gebruikt voor deze eigenschap (als een parameter van de constructor ResourceFile). Hiermee wordt aangegeven dat het bestand in dezelfde map als het *TaskApplication.exe*kan worden gevonden.

2. Het **tweede argument** geeft aan dat de top *N* woorden moeten worden opgenomen in het uitvoerbestand. In het voorbeeld wordt deze harde code zodat de bovenste drie woorden worden geschreven naar het uitvoerbestand.

3. Het **derde argument** is de handtekening van gedeelde toegang (SAS) die schrijftoegang tot de container voor **uitvoer** in Azure opslag biedt. Deze handtekening gedeelde toegang URL *TaskApplication.exe* gebruikt het bestand uploadt naar Azure opslag. Vindt u de code voor deze in de `UploadFileToContainer` -methode in de TaskApplication van het project `Program.cs` bestand:

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload the file (as a new blob) to the container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath, FileMode.Open);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when the Batch service
                // sets the CloudTask.ExecutionInformation.ExitCode for the task that
                // executed this application, it properly indicates that there was a
                // problem with the task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>Stap 6: Monitortaken

![Monitortaken][6]<br/>
*De clienttoepassing (1) de taken voor het invullen en succes status controleert en (2) de taken resultaatgegevens uploaden naar Azure opslag*

Wanneer taken worden toegevoegd aan een taak, worden ze automatisch in de wachtrij geplaatst en gepland voor uitvoering op computerknooppunten binnen de groep die aan de taak is gekoppeld. Op basis van de instellingen die u opgeeft, Batch worden verwerkt alle taak queuing, planning, opnieuw proberen en andere taak beheer taken voor u. Er zijn veel methoden voor monitoring van de uitvoering van taken. DotNetTutorial is een eenvoudig voorbeeld dat lidstaten alleen op voltooiing en taak fout of voltooid wordt doorgegeven.

In de `MonitorTasks` -methode in de DotNetTutorial `Program.cs`, er zijn drie Batch .NET concepten die discussie te rechtvaardigen. Ze staan hieronder in welke volgorde:

1. **ODATADetailLevel**: [ODATADetailLevel] geven[ net_odatadetaillevel] in de lijst bewerkingen (zoals het verkrijgen van een lijst met taken van een taak.) is van essentieel belang om ervoor te zorgen prestaties van toepassing. [De Batch Azure service efficiënt Query](batch-efficient-list-queries.md) aan uw lezen lijst toevoegen als u van plan bent een soort status controleren in de Batch-toepassingen uitvoeren.

2. **TaskStateMonitor**: [TaskStateMonitor] [ net_taskstatemonitor] Batch .NET toepassingen biedt met hulpprogramma hulpprogramma's voor het controleren van de lidstaten van de taak. In `MonitorTasks`, *DotNetTutorial* wacht voor alle taken te bereiken [TaskState.Completed] [ net_taskstate] binnen een termijn. Vervolgens wordt de taak beëindigd.

3. **TerminateJobAsync**: beëindigen van een project met [JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] (of de blokkerende JobOperations.TerminateJob) die taak wordt gemarkeerd als voltooid. Is het van belang te doen als uw Batch-oplossing maakt gebruik van een [JobReleaseTask][net_jobreltask]. Dit is een speciale soort taak, die wordt beschreven in de [voorbereiding en de voltooiing van taken](batch-job-prep-release.md).

De `MonitorTasks` methode van *DotNetTutorial*van `Program.cs` hieronder wordt weergegeven:

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed to reach the Completed state within the timeout period.";

    // Obtain the collection of tasks currently managed by the job. Note that we use
    // a detail level to  specify that only the "id" property of each task should be
    // populated. Using a detail level for all list operations helps to lower
    // response time from the Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor to monitor the state of our tasks. In this case, we
    // will wait for all tasks to reach the Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached the "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property to ensure that it did not encounter a scheduling error
    // or return a non-zero exit code.

    // Update the detail level to populate only the task id and executionInfo
    // properties. We refresh the tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate the task's properties with the latest info from the
        // Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.SchedulingError != null)
        {
            // A scheduling error indicates a problem starting the task on the node.
            // It is important to note that the task's state can be "Completed," yet
            // still have encountered a scheduling error.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a scheduling error: {1}",
                task.Id,
                task.ExecutionInformation.SchedulingError.Message);
        }
        else if (task.ExecutionInformation.ExitCode != 0)
        {
            // A non-zero exit code may indicate that the application executed by
            // the task encountered an error during execution. As not every
            // application returns non-zero on failure by default (e.g. robocopy),
            // your implementation of error checking may differ from this example.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within the specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>Stap 7: De uitvoer van de taak downloaden

![Uitvoer van de taak van de opslag te downloaden][7]<br/>

Nu dat de taak is voltooid, kunt u de uitvoer van de taken van Azure opslag downloaden. Dit wordt gedaan met een aanroep van `DownloadBlobsFromContainerAsync` in *DotNetTutorial*van `Program.cs`:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference to a previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all the block blobs in the specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference to the current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents to a file in the specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded to {0}", directoryPath);
}
```

> [AZURE.NOTE] De aanroep van `DownloadBlobsFromContainerAsync` in de *DotNetTutorial* toepassing geeft aan dat de bestanden moeten worden gedownload naar uw `%TEMP%` map. Altijd deze output locatie wijzigen.

## <a name="step-8-delete-containers"></a>Stap 8: Delete containers

Omdat u in rekening worden gebracht voor de gegevens die zich in opslag in Azure, is het altijd een goed idee om een BLOB's die niet langer nodig zijn voor uw batchtaken verwijderen. In de DotNetTutorial `Program.cs`, dit wordt gedaan met drie aanroepen van de helpmethode `DeleteContainerAsync`:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

De methode zelf wordt alleen een verwijzing naar de container en roept vervolgens de [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Stap 9: De taak en de groep verwijderen

In de laatste stap wordt de gebruiker gevraagd de taak en de toepassingen die zijn gemaakt door de toepassing van de DotNetTutorial te verwijderen. Hoewel u niet worden aangerekend voor projecten en taken zelf, u *worden* aangerekend voor computerknooppunten. Wij raden dus aan knooppunten te wijzen, alleen indien nodig. Het verwijderen van ongebruikte toepassingen kan deel uitmaken van uw proces voor onderhoud.

Van de BatchClient [JobOperations] [ net_joboperations] en [PoolOperations] [ net_pooloperations] hebben beide overeenkomstige verwijdering methoden die worden genoemd als de gebruiker verwijderen bevestigt:

```csharp
// Clean up the resources we've created in the Batch account if the user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [AZURE.IMPORTANT] Houd er rekening mee dat u in rekening voor compute resources gebracht, kosten voor het verwijderen van ongebruikte toepassingen te minimaliseren. Let op dat een groep verwijdert alle computerknooppunten binnen die groep en dat alle gegevens op de knooppunten kunnen niet worden teruggezet nadat de groep is verwijderd.

## <a name="run-the-dotnettutorial-sample"></a>Het monster *DotNetTutorial* uitvoeren

Wanneer u de voorbeeldtoepassing uitvoert, worden de console-uitvoer vergelijkbaar met de volgende. Tijdens de uitvoering, wordt er op een pauze `Awaiting task completion, timeout in 00:30:00...` terwijl computerknooppunten van de toepassingen worden gestart. Gebruik de [Azure portal] [ azure_portal] voor het controleren van uw groep computerknooppunten, taak, taken tijdens en na de uitvoering. Gebruik de [Azure portal] [ azure_portal] of de [Azure Opslagverkenner] [ storage_explorers] om de opslagbronnen (containers en BLOB's) die zijn gemaakt door de toepassing weer te geven.

Standaard uitvoeringstijd is **ongeveer 5 minuten** wanneer u de toepassing in de standaardconfiguratie uitvoert.

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe to container [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll to container [application]...
Uploading file ..\..\taskdata1.txt to container [input]...
Uploading file ..\..\taskdata2.txt to container [input]...
Uploading file ..\..\taskdata3.txt to container [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks to job [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Downloading all files from container [output]...
All files downloaded to C:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Volgende stappen

Altijd wijzigingen aanbrengen in *DotNetTutorial* en *TaskApplication* om te experimenteren met verschillende compute-scenario's. Probeer bijvoorbeeld vertraging toe te voegen een uitvoering met *TaskApplication*, zoals met [Thread.Sleep][net_thread_sleep], voor het simuleren van langdurige taken en controleren ze in de portal. Probeer meer taken toevoegen of aanpassen van het aantal knooppunten berekenen. Toevoegen van logica om te controleren en het gebruik van een bestaande toepassingen sneller worden uitgevoerd (*Tip*: Bekijk `ArticleHelpers.cs` in de [Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] project in [azure-batch-monsters][github_samples]).

Nu u bekend bent met de eenvoudige werkstroom van een Batch-oplossing, is het tijd om zich te verdiepen de extra functies van de Batch-service.

- Lees het [overzicht van Batch-functie voor ontwikkelaars](batch-api-basics.md), die voor alle nieuwe Batch gebruikers wordt aangeraden.
- Start op de andere partij ontwikkeling artikelen in **diepgaande ontwikkeling** in de [Batch leren pad][batch_learning_path].
- Kijk dan eens een andere implementatie van de 'top N woorden' werkbelasting verwerken met behulp van de Batch in de [TopNWords] [ github_topnwords] monster.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Containers in Azure opslag maken"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Toepassing van de taak en de invoer (gegevens) bestanden uploaden naar containers"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Batch-toepassingen maken"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Batchverwerking maken"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Taken toevoegen aan project"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Monitortaken"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Uitvoer van de taak van de opslag te downloaden"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Batch-oplossing-workflow (volledige diagram)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "Batch-referenties in de Portal"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Opslag van referenties in de Portal"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Batch-oplossing-workflow (minimale diagram)"


<properties 
    pageTitle="Beheer van elementen en gerelateerde entiteiten met mediaservices SDK voor .NET" 
    description="Informatie over het beheren van de activa en gerelateerde entiteiten met de Media Services SDK voor .NET." 
    authors="juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016"
    ms.author="juliako"/>


#<a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Beheer van elementen en gerelateerde entiteiten met mediaservices SDK voor .NET


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-manage-entities.md)
- [REST](media-services-rest-manage-entities.md)


In dit onderwerp ziet u hoe de volgende Media Services-beheertaken uitvoeren:

- De verwijzing van een activum 
- De taakverwijzing van een ophalen 
- Lijst van alle activa 
- Lijst met taken en activa 
- Lijst van alle-beleid 
- Lijst van alle Locators
- Inventariseren door middel van grote verzamelingen van entiteiten
- Verwijderen van een actief 
- Een taak verwijderen 
- Een-beleid verwijderen 

##<a name="prerequisites"></a>Vereisten 

Raadpleeg [uw omgeving instellen](media-services-set-up-computer.md)

##<a name="get-an-asset-reference"></a>De verwijzing van een activum

Er is een taak die vaak om een verwijzing naar een bestaand activum in Media Services. In het volgende voorbeeld ziet u hoe u krijgt een verwijzing activa uit de collectie van activa op de server contextobject, op basis van de id van een actief
In het volgende voorbeeld wordt een Linq query verwijst naar een bestaand object van IAsset gebruikt.

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();
    
        return asset;
    }

##<a name="get-a-job-reference"></a>De taakverwijzing van een ophalen

Wanneer u werkt met het verwerken van taken in de code van Media Services, moet u vaak een verwijzing naar een bestaand project op basis van een id. In het volgende voorbeeld ziet u hoe u een verwijzing naar een IJob-object uit de collectie taken.
WarningWarning u wellicht moet u taak bij het starten van een langdurige codering project, en controleer de status van de taak op een thread. Wanneer de methode geeft als van een thread resultaat, in dergelijke gevallen moet u een vernieuwde verwijzing naar een taak ophalen.

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();
    
        return job;
    }

##<a name="list-all-assets"></a>Lijst van alle activa

Groeien met het nummer van de activa die u opgeslagen hebt, is het handig om uw activa. In het volgende voorbeeld ziet u hoe u doorloopt de collectie activa op het serverobject van de context. Met elk activum schrijft in het codevoorbeeld ook een aantal waarden van de eigenschap aan de console. Elk activum kan bijvoorbeeld veel mediabestanden bevatten. Alle bestanden die zijn gekoppeld aan elk activum schrijft in het voorbeeld.



    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");
    
            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-jobs-and-assets"></a>Lijst met taken en activa

Activa van de lijst met de bijbehorende taak in Media Services is een belangrijke verwante taak. In het volgende voorbeeld ziet u hoe u elk object IJob en wordt voor elke taak, eigenschappen van de taak wordt weergegeven, alle verwante taken, alle activa en alle uitvoer activa worden ingevoerd. De code in dit voorbeeld is handig voor tal van andere opdrachten. Als u weergeven van de activa van de uitvoer van een of meer codering taken die u eerder hebt uitgevoerd wilt, bevat deze code toegang tot de activa van de uitvoer. Wanneer er een verwijzing naar een uitvoer actief is, kunt u vervolgens de inhoud leveren aan andere gebruikers of toepassingen te downloaden of URL's naar ons. 

Zie voor meer informatie over opties voor het leveren van activa, [Activa leveren met de Media Services SDK voor .NET](media-services-deliver-streaming-content.md).

    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");
    
    
            // For each job, display the associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }
    
            // For each job, display the list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {
    
                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
            // For each job, display the list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-all-access-policies"></a>Lijst van alle-beleid

In de Media-Services kunt u een-beleid op een actief of de bestanden. Een-beleid bepaalt de machtigingen voor een bestand of een actief (welk type toegang en de duur). In uw code Media Services u meestal een-beleid definiëren door een IAccessPolicy-object maken en dit vervolgens koppelen aan een bestaand activum. Vervolgens maakt u een ILocator-object kunt u directe toegang bieden tot de activa in de Media Services. De Visual Studio-project bij deze reeks documentatie bevat verschillende codevoorbeelden die hoe maken en tonen-beleid en locators toewijzen aan activa.

In het volgende voorbeeld ziet u hoe u alle-beleid op de server en worden de machtigingen die zijn gekoppeld aan elk. Een ander handig hulpmiddel-beleid bekijken is het weergeven van alle ILocator-objecten op de server en vervolgens voor elke locator, u kunt een lijst zijn gekoppeld-beleid met behulp van de eigenschap AccessPolicy.

    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");
    
        }
    }

##<a name="list-all-locators"></a>Lijst van alle Locators

Een locator is een URL die een directe pad voor toegang tot een actief, samen met de machtigingen die aan het activum zoals gedefinieerd door de bijbehorende access-beleid van de locator. Elk activum kan hebben een collectie van objecten worden gekoppeld op de eigenschap Locators ILocator. De servercontext heeft ook een Locators-collectie alle locators bevat.

In het volgende voorbeeld worden alle locators op de server. Voor elke locator wordt de Id voor het gerelateerde activa- en beleid. Wordt ook het type machtigingen, de vervaldatum en het volledige pad naar het activum.

Let erop dat een locator-pad naar een vast activum alleen een basis-URL voor het activum. Als u wilt maken een directe pad naar afzonderlijke bestanden die een gebruiker of toepassing te kan bladeren, moet uw code het specifieke pad toevoegen aan het pad locator. Voor meer informatie over hoe u dit doet, Zie het onderwerp [Leveren activa met de Media Services SDK voor .NET](media-services-deliver-streaming-content.md).

    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a>Inventariseren door middel van grote verzamelingen van entiteiten

Bij het opvragen van entiteiten, is er een limiet van 1000 entiteiten in één keer worden geretourneerd omdat openbare REST v2 queryresultaten 1000 resultaten beperkt. U wilt overslaan en nemen gebruiken bij het opsommen van door middel van grote verzamelingen van entiteiten. 
    
De volgende functie wordt door de taken in de opgegeven Media Services-Account. Media Services retourneert 1000 taken in de collectie taken. De functie wordt gebruik gemaakt van overslaan en nemen om ervoor te zorgen dat alle taken worden geïnventariseerd (als u meer dan 1000 taken in uw account hebt).
    
    static void ProcessJobs()
    {
        try
        {
    
            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;
    
            while (true)
            {
                // Loop through all Jobs (1000 at a time) in the Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }
    
                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

##<a name="delete-an-asset"></a>Verwijderen van een actief

In het volgende voorbeeld wordt een element verwijderd.

    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();
    
        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");
    
    }

##<a name="delete-a-job"></a>Een taak verwijderen

Als u wilt verwijderen van een taak, moet u de status van de afdruktaak controleren, zoals aangegeven in de eigenschap State. Taken die zijn voltooid of geannuleerd kunnen worden verwijderd terwijl de taken die in bepaalde lidstaten, zoals in de wachtrij, geplande of verwerking, moeten eerst worden geannuleerd, en vervolgens kunnen worden verwijderd.
In het volgende voorbeeld ziet u een methode voor het verwijderen van een taak door Project Staten controleren en vervolgens verwijderen wanneer de status is voltooid of geannuleerd. Deze code is afhankelijk van de vorige sectie in dit onderwerp voor het verkrijgen van een verwijzing naar een project: een project verwijzing.

    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;
    
        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);
    
            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync to do async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }
    
        }
    }


##<a name="delete-an-access-policy"></a>Een-beleid verwijderen

In het volgende voorbeeld ziet u hoe u naar een access-beleid op basis van een beleid-Id, waarna het beleid verwijdert.

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();
    
        policy.Delete();
    
    }
    


##<a name="media-services-learning-paths"></a>Leerfasen Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<properties 
    pageTitle="Azure wachtrij opslag gebruiken voor het controleren van meldingen van Media Services-project met .NET | Microsoft Azure" 
    description="Leren met Azure Queue storage voor het controleren van meldingen van Media Services-project. In het voorbeeld is geschreven in C# en de Media Services SDK voor .NET gebruikt." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016"   
    ms.author="juliako"/>

# <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications-with-net"></a>Azure wachtrij opslag gebruiken voor het controleren van meldingen van Media Services-project met .NET

Wanneer u taken uitvoert, moet u vaak een manier om bij te houden van de voortgang van het project. U kunt de voortgang controleren door de wachtrij Azure opslag gebruikt voor het controleren van meldingen van Media Services-functie (zoals beschreven in dit onderwerp) of een gebeurtenis-handler StateChanged definiëren (zoals beschreven in [Dit](media-services-check-job-progress.md) onderwerp.  

## <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications"></a>Azure wachtrij opslag gebruiken voor het controleren van meldingen van Media Services-taak

Microsoft Azure Media Services heeft de mogelijkheid om de kennisgeving berichten worden afgeleverd op de [Azure Queue storage](../storage-dotnet-how-to-use-queues.md#what-is) bij de verwerking van media projecten. In dit onderwerp ziet u hoe u deze meldingen uit de opslag van de wachtrij.

Berichten in wachtrij opslag toegankelijk vanuit overal in de wereld. De architectuur messaging Azure-wachtrij is betrouwbaar en uiterst schaalbaar. Polling Queue storage wordt aangeraden via andere methoden gebruiken. 

Met een gangbare scenario voor het luisteren naar de meldingen van Media Services als u ontwikkelt een content managementsysteem die nodig zijn voor het uitvoeren van sommige extra taak na een codering taak is voltooid (bijvoorbeeld trigger de volgende stap in een werkstroom of inhoud publiceren). 

###<a name="considerations"></a>Overwegingen met betrekking tot

Overweeg het volgende bij het ontwikkelen van Media Services-toepassingen die gebruikmaken van Azure opslag wachtrij.

- De wachtrijen-service biedt geen garanties van first in first out (FIFO) besteld levering. Zie [wachtrijen Azure en Azure Service Bus wachtrijen vergeleken en Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx)voor meer informatie.
- Azure opslag wachtrijen is geen push service; u hebt te vragen voor de wachtrij. 
- U kunt een willekeurig aantal wachtrijen hebben. Voor meer informatie Zie [Queue Service REST API](https://msdn.microsoft.com/library/azure/dd179363.aspx).
- Azure opslag wachtrijen heeft enkele beperkingen en specificaties die worden beschreven in het volgende artikel: [Azure wachtrijen en Azure Service Bus wachtrijen vergeleken en Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).

###<a name="code-example"></a>Voorbeeld van code

In het voorbeeld in deze sectie komt het volgende:

1. Definieert de klasse **EncodingJobMessage** die is toegewezen aan de berichtindeling van het bericht. De code deserializes berichten uit de wachtrij in objecten van het type **EncodingJobMessage** wordt ontvangen.
1. Laadt de gegevens van Media Services en de opslag van het bestand app.config. Deze informatie wordt gebruikt voor het maken van de **CloudMediaContext** en **CloudQueue** objecten.
1. Hiermee maakt u de wachtrij die meldingen over de codering taak ontvangt.
1. Het eindpunt van de kennisgeving die is toegewezen aan de wachtrij wordt gemaakt.
1. Het eindpunt van het bericht is gekoppeld aan de taak en dient de taak van de codering. U kunt meerdere meldingen eindpunten die is gekoppeld aan een taak hebben.
1. In dit voorbeeld bent we alleen geïnteresseerd in definitieve bewerkingsstadia de taak, zodat we **NotificationJobState.FinalStatesOnly** aan de methode **AddNew doorgeeft** . 
        
        job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, _notificationEndPoint);
1. Als u NotificationJobState.All, u kunt verwachten om alle meldingen staat: in de wachtrij -> geplande verwerking -> -> gereedgemeld. Echter, zoals eerder opgemerkt, de Azure opslag wachtrijen service garandeert niet aflevering. U kunt de eigenschap Timestamp (gedefinieerd op het type EncodingJobMessage in het onderstaande voorbeeld) om de volgorde waarin berichten. Het is mogelijk dat u een dubbele meldingen. Gebruikt u de eigenschap ETag (gedefinieerd op het type EncodingJobMessage) om te controleren op dubbele records. Het is ook mogelijk dat sommige meldingen staat wordt overgeslagen. 
1. De taak om de status voltooid aan de hand van de wachtrij elke 10 seconden wordt gewacht. Berichten verwijderen nadat ze zijn verwerkt.
1. Hiermee verwijdert u de wachtrij en het eindpunt van het bericht.

>[AZURE.NOTE]De aanbevolen manier voor het controleren van de status van een taak is door te luisteren naar de meldingen, zoals in het volgende voorbeeld wordt getoond.
>
>U kan ook controleren van de status van een taak met behulp van de eigenschap **IJob.State** .  Een melding over de voltooiing van een project kan komen voordat de status van het **IJob** is ingesteld op **voltooid**. De eigenschap **IJob.State** geeft de nauwkeurige staat met een kleine vertraging.

    
    using System;
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Web;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Runtime.Serialization.Json;
    
    namespace JobNotification
    {
        public class EncodingJobMessage
        {
            // MessageVersion is used for version control. 
            public String MessageVersion { get; set; }
        
            // Type of the event. Valid values are 
            // JobStateChange and NotificationEndpointRegistration.
            public String EventType { get; set; }
        
            // ETag is used to help the customer detect if 
            // the message is a duplicate of another message previously sent.
            public String ETag { get; set; }
        
            // Time of occurrence of the event.
            public String TimeStamp { get; set; }
    
            // Collection of values specific to the event.
    
            // For the JobStateChange event the values are:
            //     JobId - Id of the Job that triggered the notification.
            //     NewState- The new state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
            //     OldState- The old state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
    
            // For the NotificationEndpointRegistration event the values are:
            //     NotificationEndpointId- Id of the NotificationEndpoint 
            //          that triggered the notification.
            //     State- The state of the Endpoint. 
            //          Valid values are: Registered and Unregistered.
    
            public IDictionary<string, object> Properties { get; set; }
        }
    
        class Program
        {
            private static CloudMediaContext _context = null;
            private static CloudQueue _queue = null;
            private static INotificationEndPoint _notificationEndPoint = null;
    
            private static readonly string _singleInputMp4Path =
                Path.GetFullPath(@"C:\supportFiles\multifile\BigBuckBunny.mp4");
    
            static void Main(string[] args)
            {
                // Get the values from app.config file.
                string mediaServicesAccountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
                string mediaServicesAccountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];
                string storageConnectionString = ConfigurationManager.AppSettings["StorageConnectionString"];
    
    
                string endPointAddress = Guid.NewGuid().ToString();
    
                // Create the context. 
                _context = new CloudMediaContext(mediaServicesAccountName, mediaServicesAccountKey);
    
                // Create the queue that will be receiving the notification messages.
                _queue = CreateQueue(storageConnectionString, endPointAddress);
    
                // Create the notification point that is mapped to the queue.
                _notificationEndPoint = 
                        _context.NotificationEndPoints.Create(
                        Guid.NewGuid().ToString(), NotificationEndPointType.AzureQueue, endPointAddress);
    
    
                if (_notificationEndPoint != null)
                {
                    IJob job = SubmitEncodingJobWithNotificationEndPoint(_singleInputMp4Path);
                    WaitForJobToReachedFinishedState(job.Id);
                }
    
                // Clean up.
                _queue.Delete();      
                _notificationEndPoint.Delete();
           }
    
    
            static public CloudQueue CreateQueue(string storageAccountConnectionString, string endPointAddress)
            {
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageAccountConnectionString);
    
                // Create the queue client
                CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
                // Retrieve a reference to a queue
                CloudQueue queue = queueClient.GetQueueReference(endPointAddress);
    
                // Create the queue if it doesn't already exist
                queue.CreateIfNotExists();
    
                return queue;
            }
     
    
            public static IJob SubmitEncodingJobWithNotificationEndPoint(string inputMediaFilePath)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("My MP4 to Smooth Streaming encoding job");
    
                //Create an encrypted asset and upload the mp4. 
                IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.StorageEncrypted, 
                    inputMediaFilePath);
    
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
                // Create a task with the conversion details, using a configuration file. 
                ITask task = job.Tasks.AddNew("My encoding Task",
                    processor,
                    "H264 Multiple Bitrate 720p",
                    Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Add a notification point to the job. You can add multiple notification points.  
                job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, 
                    _notificationEndPoint);
    
                job.Submit();
    
                return job;
            }
    
            public static void WaitForJobToReachedFinishedState(string jobId)
            {
                int expectedState = (int)JobState.Finished;
                int timeOutInSeconds = 600;
    
                bool jobReachedExpectedState = false;
                DateTime startTime = DateTime.Now;
                int jobState = -1;
    
                while (!jobReachedExpectedState)
                {
                    // Specify how often you want to get messages from the queue.
                    Thread.Sleep(TimeSpan.FromSeconds(10));
    
                    foreach (var message in _queue.GetMessages(10))
                    {
                        using (Stream stream = new MemoryStream(message.AsBytes))
                        {
                            DataContractJsonSerializerSettings settings = new DataContractJsonSerializerSettings();
                            settings.UseSimpleDictionaryFormat = true;
                            DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(EncodingJobMessage), settings);
                            EncodingJobMessage encodingJobMsg = (EncodingJobMessage)ser.ReadObject(stream);
    
                            Console.WriteLine();
    
                            // Display the message information.
                            Console.WriteLine("EventType: {0}", encodingJobMsg.EventType);
                            Console.WriteLine("MessageVersion: {0}", encodingJobMsg.MessageVersion);
                            Console.WriteLine("ETag: {0}", encodingJobMsg.ETag);
                            Console.WriteLine("TimeStamp: {0}", encodingJobMsg.TimeStamp);
                            foreach (var property in encodingJobMsg.Properties)
                            {
                                Console.WriteLine("    {0}: {1}", property.Key, property.Value);
                            }
    
                            // We are only interested in messages 
                            // where EventType is "JobStateChange".
                            if (encodingJobMsg.EventType == "JobStateChange")
                            {
                                string JobId = (String)encodingJobMsg.Properties.Where(j => j.Key == "JobId").FirstOrDefault().Value;
                                if (JobId == jobId)
                                {
                                    string oldJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "OldState").FirstOrDefault().Value;
                                    string newJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "NewState").FirstOrDefault().Value;
    
                                    JobState oldJobState = (JobState)Enum.Parse(typeof(JobState), oldJobStateStr);
                                    JobState newJobState = (JobState)Enum.Parse(typeof(JobState), newJobStateStr);
    
                                    if (newJobState == (JobState)expectedState)
                                    {
                                        Console.WriteLine("job with Id: {0} reached expected state: {1}", 
                                            jobId, newJobState);
                                        jobReachedExpectedState = true;
                                        break;
                                    }
                                }
                            }
                        }
                        // Delete the message after we've read it.
                        _queue.DeleteMessage(message);
                    }
    
                    // Wait until timeout
                    TimeSpan timeDiff = DateTime.Now - startTime;
                    bool timedOut = (timeDiff.TotalSeconds > timeOutInSeconds);
                    if (timedOut)
                    {
                        Console.WriteLine(@"Timeout for checking job notification messages, 
                                            latest found state ='{0}', wait time = {1} secs",
                            jobState,
                            timeDiff.TotalSeconds);
    
                        throw new TimeoutException();
                    }
                }
            }
       
            static private IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
            {
                var asset = _context.Assets.Create("UploadSingleFile_" + DateTime.UtcNow.ToString(), 
                    assetCreationOptions);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading of {0}", assetFile.Name);
    
                return asset;
            }
    
            static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
    
                return processor;
            }
        }
    }

In het bovenstaande voorbeeld is de volgende uitvoer geproduceerd. U waarden zullen variëren.
    
    Created assetFile BigBuckBunny.mp4
    Upload BigBuckBunny.mp4
    Done uploading of BigBuckBunny.mp4
    
    EventType: NotificationEndPointRegistration
    MessageVersion: 1.0
    ETag: e0238957a9b25bdf3351a88e57978d6a81a84527fad03bc23861dbe28ab293f6
    TimeStamp: 2013-05-14T20:22:37
        NotificationEndPointId: nb:nepid:UUID:d6af9412-2488-45b2-ba1f-6e0ade6dbc27
        State: Registered
        Name: dde957b2-006e-41f2-9869-a978870ac620
        Created: 2013-05-14T20:22:35
    
    EventType: JobStateChange
    MessageVersion: 1.0
    ETag: 4e381f37c2d844bde06ace650310284d6928b1e50101d82d1b56220cfcb6076c
    TimeStamp: 2013-05-14T20:24:40
        JobId: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54
        JobName: My MP4 to Smooth Streaming encoding job
        NewState: Finished
        OldState: Processing
        AccountName: westeuropewamsaccount
    job with Id: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54 reached expected 
    State: Finished
    

## <a name="next-step"></a>Volgende stap

Leren werken met paden Media-Services controleren

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

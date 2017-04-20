<properties
    pageTitle="Aan de slag met de opslag van de wachtrij en Visual Studio verbonden services (WebJob projecten) | Microsoft Azure"
    description="Hoe aan de slag met Azure Queue storage in een WebJob project nadat services verbinding maken met een opslag-account met behulp van Visual Studio worden verbonden."
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-webjob-projects"></a>Aan de slag met Azure Queue storage en Visual Studio verbonden services (WebJob projecten)

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe aan de slag met Azure Queue storage in een project van Visual Studio Azure WebJob nadat u hebt gemaakt of waarnaar wordt verwezen een account Azure opslag met behulp van het dialoogvenster Visual Studio **Verbonden Services toevoegen** . Wanneer u een opslag-account aan een project van WebJob toevoegen met behulp van het dialoogvenster Visual Studio **Verbonden Services toevoegen** , de juiste Azure opslag NuGet pakketten zijn geïnstalleerd, de juiste referenties voor .NET worden toegevoegd aan het project en verbindingsreeksen voor de opslag in het App.config bestand worden bijgewerkt.  

In dit artikel wordt een C# codevoorbeelden waarin wordt aangegeven hoe de Azure WebJobs SDK versie 1.x met de Azure Queue storage-service.

Azure Queue storage is een service voor het opslaan van een groot aantal berichten dat kunnen worden geopend vanuit overal in de wereld via geverifieerde oproepen met behulp van HTTP of HTTPS. Een bericht van één wachtrij kan tot 64 KB in grootte en een wachtrij kan bevatten miljoenen berichten tot aan de totale capaciteit van een opslagruimte. Zie [aan de slag met Azure Queue Storage met .NET](storage-dotnet-how-to-use-queues.md) voor meer informatie. Voor meer informatie over ASP.NET Zie [ASP.NET](http://www.asp.net).



## <a name="how-to-trigger-a-function-when-a-queue-message-is-received"></a>Hoe een functie wordt geactiveerd wanneer een wachtrij-bericht is ontvangen.

Als u wilt een functie schrijven die de WebJobs SDK wordt aangeroepen wanneer een wachtrij-bericht wordt ontvangen, gebruikt u het kenmerk **QueueTrigger** . De constructor kenmerk tekenreeksparameter een waarmee de naam van de wachtrij op te vragen. Uitchecken als u wilt zien hoe u de naam van de wachtrij dynamisch instellen, [het instellen van configuratie-opties](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>String-berichten

In het volgende voorbeeld wordt bevat de wachtrij een Tekenreeksbericht **QueueTrigger** wordt toegepast op een parameter string met de naam **logMessage** met de inhoud van het bericht van de wachtrij. De functie [wordt een logboek voor het Dashboard](#how-to-write-logs).


        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

Naast de **reeks**, de parameter mogelijk een byte-matrix, een **CloudQueueMessage** -object of een POCO die u definieert.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(gewone oude CLR-Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) berichten in de wachtrij

In het volgende voorbeeld bevat het bericht wachtrij JSON voor een **BlobInformation** -object bevat de eigenschap **BlobName** . Het object wordt automatisch deserializes door de SDK.

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

De SDK gebruikt de [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) serialiseren en terugconverteren van berichten. Als u berichten in wachtrij plaatsen in een programma dat de WebJobs SDK maakt geen gebruik van maakt, kunt u code zoals in het volgende voorbeeld voor het maken van een bericht POCO wachtrij met de SDK parseren kunt.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a>Async-functies

De volgende asynchrone functie [schrijft een logboek voor het Dashboard](#how-to-write-logs).

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Asynchrone functies duurt een [annulering token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), zoals in het volgende voorbeeld wordt een blob gekopieerd. (Zie de sectie [BLOB's](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) voor een uitleg van de tijdelijke aanduiding voor de **queueTrigger** .)

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

## <a name="types-the-queuetrigger-attribute-works-with"></a>Het kenmerk QueueTrigger met werkt typen

U kunt **QueueTrigger** gebruiken met de volgende typen:

* **tekenreeks**
* Een geserialiseerd als JSON POCO-type
* **byte]**
* **CloudQueueMessage**

## <a name="polling-algorithm"></a>Polling-algoritme

De SDK implementeert een willekeurige exponentiële back-off-algoritme het effect van niet-actieve wachtrij polling op transactie opslagkosten verminderen.  Wanneer een bericht wordt gevonden, wordt de SDK Wacht twee seconden en wordt er gecontroleerd of een ander bericht. Wacht ongeveer vier seconden voordat het opnieuw te proberen als er geen bericht is gevonden. De tijd blijft na opeenvolgende mislukte pogingen om een bericht wachtrij te verhogen totdat de maximale wachttijd, namelijk één minuut bereikt. [De maximale wachttijd kan worden geconfigureerd](#how-to-set-configuration-options).

## <a name="multiple-instances"></a>Meerdere exemplaren

Als uw web app op meerdere instanties wordt uitgevoerd, een continue WebJobs wordt uitgevoerd op elke computer en elke machine zal wachten voor triggers en functies uitvoert. In sommige gevallen die kan dit leiden tot sommige functies tweemaal dezelfde gegevens verwerken, dus functies moeten idempotency is ingeschakeld (zodat het herhaaldelijk aanroepen met de ingevoerde gegevens van hetzelfde niet leiden tot dubbele resultaten geschreven).  

## <a name="parallel-execution"></a>Parallelle uitvoering

Als er meerdere functies in een andere wachtrij luisteren, belt de SDK ze parallel als berichten gelijktijdig worden ontvangen.

Hetzelfde geldt wanneer meerdere berichten zijn ontvangen voor één wachtrij. De SDK haalt een batch voor 16 berichten tegelijk en standaard wordt de functie die ze parallel verwerkt uitgevoerd. De [grootte van de partij kan worden geconfigureerd](#how-to-set-configuration-options). Als het nummer wordt verwerkt naar beneden tot de helft van de grootte van de partij, worden de SDK haalt een andere batch en start de verwerking van die berichten. Daarom is het maximum aantal gelijktijdige berichten per functie verwerkt een anderhalf maal de grootte van de partij. Deze limiet geldt afzonderlijk voor elke functie die een **QueueTrigger** -kenmerk heeft. Als u geen parallelle uitvoering voor berichten die worden ontvangen in een wachtrij wilt, moet u de batchgrootte ingesteld op 1.

## <a name="get-queue-or-queue-message-metadata"></a>Wachtrij of wachtrij bericht metagegevens ophalen

U kunt de volgende berichteigenschappen opvragen door parameters toe te voegen aan de methodehandtekening:

* **DateTimeOffset** expirationTime
* **DateTimeOffset** insertionTime
* **DateTimeOffset** nextVisibleTime
* **String** queueTrigger (bevat de tekst van bericht)
* **tekenreeks** -id
* **String** , popReceipt
* **int** dequeueCount

Als u werken rechtstreeks met de Azure opslag API wilt, kunt u ook een parameter **CloudStorageAccount** toevoegen.

In het volgende voorbeeld worden alle metagegevens van deze naar een INFO-toepassingslogboek geschreven. In het voorbeeld bevat zowel logMessage als queueTrigger de inhoud van het bericht van de wachtrij.

        public static void WriteLog([QueueTrigger("logqueue")] string logMessage,
            DateTimeOffset expirationTime,
            DateTimeOffset insertionTime,
            DateTimeOffset nextVisibleTime,
            string id,
            string popReceipt,
            int dequeueCount,
            string queueTrigger,
            CloudStorageAccount cloudStorageAccount,
            TextWriter logger)
        {
            logger.WriteLine(
                "logMessage={0}\n" +
            "expirationTime={1}\ninsertionTime={2}\n" +
                "nextVisibleTime={3}\n" +
                "id={4}\npopReceipt={5}\ndequeueCount={6}\n" +
                "queue endpoint={7} queueTrigger={8}",
                logMessage, expirationTime,
                insertionTime,
                nextVisibleTime, id,
                popReceipt, dequeueCount,
                cloudStorageAccount.QueueEndpoint,
                queueTrigger);
        }

Dit is een monster logboek geschreven door de voorbeeldcode:

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

## <a name="graceful-shutdown"></a>Systeem correct wordt afgesloten

Een functie die wordt uitgevoerd in een continue WebJob accepteert een parameter **CancellationToken** waarmee het besturingssysteem op de hoogte stellen van de functie als de WebJob wordt beëindigd. U kunt dit bericht om ervoor te zorgen dat de functie niet wordt onverwacht beëindigd op een manier die blijven de gegevens in een inconsistente toestand.

In het volgende voorbeeld ziet u hoe om te controleren voor beëindiging van een dreigende WebJob in een functie.

    public static void GracefulShutdownDemo(
                [QueueTrigger("inputqueue")] string inputText,
                TextWriter logger,
                CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(1000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }

**Opmerking:** Het Dashboard mogelijk niet correct weergegeven de status en de uitvoer van de functies die zijn afgesloten.

Zie voor meer informatie [WebJobs systeem correct wordt afgesloten](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).   

## <a name="how-to-create-a-queue-message-while-processing-a-queue-message"></a>Een bericht wachtrij tijdens het verwerken van een wachtrij maken

Als u wilt een functie schrijven die een nieuw bericht in de wachtrij wordt gemaakt, attribuut gebruiken voor de **wachtrij** . Net als **QueueTrigger**, geeft u in de naam van de wachtrij als een tekenreeks of kunt u [de naam van de wachtrij dynamisch instellen](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>String-berichten

In het volgende voorbeeld niet async maakt een nieuw bericht in de wachtrij in de wachtrij met de naam 'outputqueue' met dezelfde inhoud als de wachtrij bericht in de wachtrij met de naam 'inputqueue'. (Gebruik voor asynchrone functies **IAsyncCollector<T> ** zoals verderop in deze sectie wordt weergegeven.)


        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(gewone oude CLR-Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) berichten in de wachtrij

Een wachtrij om bericht te maken dat een POCO in plaats van een tekenreeks bevat, geeft u het type POCO als een uitvoerparameter aan de constructor **wachtrij** kenmerk.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

De SDK serialiseert automatisch de JSON-object. Een wachtrij-bericht wordt altijd gemaakt, zelfs als het object null is.

### <a name="create-multiple-messages-or-in-async-functions"></a>Maak meerdere berichten of in de asynchrone functies

Om meerdere berichten maken, maken het parametertype voor de wachtrij uitvoer **ICollector<T> ** of **IAsyncCollector<T>**, zoals in het volgende voorbeeld wordt weergegeven.

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Elk bericht wachtrij wordt gemaakt onmiddellijk wanneer de **Add** -methode wordt aangeroepen.

### <a name="types-that-the-queue-attribute-works-with"></a>Typen die geschikt is voor de wachtrij kenmerk

U kunt het kenmerk van de **wachtrij** voor de volgende parameter:

* **uit een tekenreeks** (bericht wachtrij maakt als parameterwaarde niet null is wanneer de functie eindigt)
* **van byte]** (werkt net als **tekenreeks**)
* **van CloudQueueMessage** (werkt net als **tekenreeks**)
* **uit POCO** (een serialiseerbaar type een bericht maken met een null-object als de parameter null is wanneer de functie eindigt)
* **ICollector**
* **IAsyncCollector**
* **CloudQueue** (voor het maken van berichten handmatig rechtstreeks via de API van Azure opslag)

### <a name="use-webjobs-sdk-attributes-in-the-body-of-a-function"></a>Kenmerken WebJobs SDK in de hoofdtekst van een functie te gebruiken

Als u werken moet voordat u een kenmerk WebJobs SDK zoals **wachtrij**, **Blob**of **tabel**in uw functie, kunt u de **IBinder** -interface.

In het volgende voorbeeld wordt een invoerwachtrij bericht en maakt u een nieuw bericht met dezelfde inhoud in een wachtrij voor uitvoer. De wachtrijnaam uitvoer wordt ingesteld door de code in de hoofdtekst van de functie.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

De **IBinder** -interface kan ook worden gebruikt met de **tabel** en de **Blob** .

## <a name="how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message"></a>Het lezen en schrijven van BLOB's en tabellen bij het verwerken van een wachtrij

De kenmerken van de **Blob** en een **tabel** kunnen u lezen en schrijven van BLOB's en tabellen. De voorbeelden in deze sectie van toepassing op de BLOB's. Voor codevoorbeelden waarin wordt aangegeven hoe u processen starten als BLOB's worden gemaakt of bijgewerkt, Zie [het gebruik van Azure blobopslag met de WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)en voor codevoorbeelden die lezen en schrijven van tabellen, [Azure tabelopslag met de WebJobs SDK gebruiken](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md).

### <a name="string-queue-messages-triggering-blob-operations"></a>Wachtrijberichten String bewerkingen blob activeren

**QueueTrigger** is een tijdelijke aanduiding die in de **Blob** -kenmerk **blobPath** -parameter met de inhoud van het bericht kunt u voor een wachtrij-bericht een tekenreeks bevat.

In het volgende voorbeeld wordt de **Stream** -objecten lezen en schrijven van BLOB's. Het bericht van de wachtrij is de naam van een blob die zich in de container textblobs. Een kopie van de blob met '-nieuwe ' toegevoegd aan de naam in dezelfde container wordt gemaakt.

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

De constructor **Blob** -kenmerk heeft een parameter **blobPath** die in de container en de naam van de blob. Zie voor meer informatie over deze tijdelijke aanduiding [gebruiken Azure blobopslag met de WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md).

Wanneer het kenmerk wordt verfraaid een **Stream** -object, geeft een andere constructor parameter de **FileAccess** als lezen, schrijven of lezen/schrijven.

In het volgende voorbeeld wordt een **CloudBlockBlob** -object om te verwijderen van een blob. Het bericht van de wachtrij is de naam van de blob.

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(gewone oude CLR-Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) berichten in de wachtrij

Voor een POCO als JSON in het bericht wachtrij opgeslagen, kunt u tijdelijke aanduidingen die de naameigenschappen van het object in de **wachtrij** van het kenmerk **blobPath** -parameter. U kunt ook metagegevens wachtrijnamen gebruiken als tijdelijke aanduidingen. Zie [wachtrij of wachtrij bericht metagegevens ophalen](#get-queue-or-queue-message-metadata).

In het volgende voorbeeld wordt een blob kopieert naar een nieuwe blob met een andere extensie. Het bericht van de wachtrij is een **BlobInformation** -object met de eigenschappen **BlobName** en **BlobNameWithoutExtension** . De namen van eigenschappen worden gebruikt als tijdelijke aanduidingen in de blob-pad voor de kenmerken van de **Blob** .

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

De SDK gebruikt de [Newtonsoft.Json NuGet package](http://www.nuget.org/packages/Newtonsoft.Json) serialiseren en terugconverteren van berichten. Als u berichten in wachtrij plaatsen in een programma dat de WebJobs SDK maakt geen gebruik van maakt, kunt u code zoals in het volgende voorbeeld voor het maken van een bericht POCO wachtrij met de SDK parseren kunt.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

Als u werken in de functie wilt voor een blob binden aan een object, kunt u het kenmerk in de hoofdtekst van de functie, zoals in de [WebJobs SDK gebruiken kenmerken in de hoofdtekst van een functie](#use-webjobs-sdk-attributes-in-the-body-of-a-function).

###<a name="types-you-can-use-the-blob-attribute-with"></a>U het kenmerk Blob met kunt typen

Het kenmerk **Blob** kan worden gebruikt met de volgende typen:

* **Stream** (Lees- of schrijfbewerkingen zijn, opgegeven met de parameter van de constructor FileAccess)
* **TextReader**
* **TextWriter**
* **tekenreeks** (lezen)
* **uit een tekenreeks** (schrijven, alleen als de tekenreeksparameter niet null is wanneer de functie resulteert in een blob maakt)
* POCO (lezen)
* uit POCO (schrijven; altijd een blob wordt gemaakt, wordt gemaakt als null-object als parameter POCO null is, de functie als resultaat wordt gegeven)
* **CloudBlobStream** (schrijven)
* **ICloudBlob** (Lees- of schrijftoegang)
* **CloudBlockBlob** (Lees- of schrijftoegang)
* **CloudPageBlob** (Lees- of schrijftoegang)

##<a name="how-to-handle-poison-messages"></a>Verwerking van gevaarlijke berichten

*Gevaarlijke berichten*, worden berichten waarvan de inhoud zorgt ervoor een functie dat niet genoemd. Wanneer de functie is mislukt, wordt het bericht wachtrij wordt niet verwijderd en uiteindelijk is opgenomen, waardoor de cyclus worden herhaald. De SDK kan automatisch de cyclus onderbreken na een beperkt aantal iteraties, of u kunt dit handmatig doen.

### <a name="automatic-poison-message-handling"></a>Verwerken van verontreinigde berichten automatisch

De SDK wordt een functie maximaal 5 keer aanroept voor het verwerken van een wachtrij. Als de vijfde poging mislukt, wordt het bericht verplaatst naar een wachtrij poison. Hier ziet u het maximale aantal nieuwe pogingen in [het instellen van configuratie-opties](#how-to-set-configuration-options)configureren.

De wachtrij verontreinigd is met de naam *{originalqueuename}*-poison. U kunt schrijven een functie op berichten uit de wachtrij verontreinigd door deze registratie of het verzenden van een melding die handmatige aandacht nodig is.

In het volgende voorbeeld is de **CopyBlob** mislukt functie wanneer u een bericht wachtrij bevat de naam van een blob die niet bestaat. Wanneer dat gebeurt, wordt het bericht uit de wachtrij copyblobqueue verplaatst naar de wachtrij copyblobqueue poison. De **ProcessPoisonMessage** meldt zich dan verontreinigd bericht.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

        public static void ProcessPoisonMessage(
            [QueueTrigger("copyblobqueue-poison")] string blobName, TextWriter logger)
        {
            logger.WriteLine("Failed to copy blob, name=" + blobName);
        }

De volgende illustratie toont de console-uitvoer van deze functies wanneer een poison-bericht wordt verwerkt.

![Console-uitvoer voor het verwerken van verontreinigde berichten](./media/vs-storage-webjobs-getting-started-queues/poison.png)

### <a name="manual-poison-message-handling"></a>Verwerken van verontreinigde berichten handmatig

U krijgt het aantal keren dat een bericht trok voor verwerking door een **int** -parameter met de naam **dequeueCount** aan de functie toe te voegen. U kunt vervolgens de wachtrij halen telling in de functiecode te controleren en uitvoeren van uw eigen poison afhandeling van berichten als het getal groter is dan een drempelwaarde, zoals in het volgende voorbeeld.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed to copy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <a name="how-to-set-configuration-options"></a>Het configuratie-opties instellen

Het type **JobHostConfiguration** kunt u de volgende configuratie-opties instellen:

* De SDK-verbindingsreeksen in code instellen.
* **QueueTrigger** configureren instellingen zoals het maximum aantal uit de wachtrij halen.
* Namen van wachtrijen ophalen van configuratie.

###<a name="set-sdk-connection-strings-in-code"></a>Verbindingsreeksen SDK instellen in code

De SDK-verbindingsreeksen instellen in code kunt u uw eigen verbinding tekenreeksnamen in configuratiebestanden of omgevingsvariabelen, zoals in het volgende voorbeeld wordt getoond.

        static void Main(string[] args)
        {
            var _storageConn = ConfigurationManager
                .ConnectionStrings["MyStorageConnection"].ConnectionString;

            var _dashboardConn = ConfigurationManager
                .ConnectionStrings["MyDashboardConnection"].ConnectionString;

            var _serviceBusConn = ConfigurationManager
                .ConnectionStrings["MyServiceBusConnection"].ConnectionString;

            JobHostConfiguration config = new JobHostConfiguration();
            config.StorageConnectionString = _storageConn;
            config.DashboardConnectionString = _dashboardConn;
            config.ServiceBusConnectionString = _serviceBusConn;
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="configure-queuetrigger--settings"></a>QueueTrigger-instellingen configureren

U kunt de volgende instellingen die voor de verwerking van de wachtrij berichten gelden configureren:

- Het maximum aantal berichten die gelijktijdig worden parallel uitgevoerd worden opgenomen (standaard is 16).
- Het maximum aantal pogingen voordat een bericht wachtrij wordt verzonden naar een wachtrij poison (de standaardwaarde is 5).
- De maximale wachttijd voordat het opnieuw polling wanneer een wachtrij leeg is (de standaardinstelling is 1 minuut).

In het volgende voorbeeld ziet u hoe u deze instellingen configureren:

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="set-values-for-webjobs-sdk-constructor-parameters-in-code"></a>Waarden instellen voor WebJobs SDK constructor parameters in de code

Soms wilt u een wachtrijnaam, een blob-naam of de container opgeven of een tabel de naam in plaats van harde code code. U wilt bijvoorbeeld de naam van de wachtrij voor **QueueTrigger** in een configuratie-bestand of de omgeving variabele opgeven.

U kunt dit doen door doorgeven in een **NameResolver** -object met het type **JobHostConfiguration** . U speciale tijdelijke aanduidingen omgeven door tekenen in de WebJobs SDK kenmerk constructor parameters procent (%) en geeft de werkelijke waarden worden gebruikt in plaats van de tijdelijke aanduidingen voor de **NameResolver** -code.

Stel bijvoorbeeld dat u wilt gebruiken een wachtrij met de naam logqueuetest in de testomgeving en één met de naam logqueueprod in de productie. In plaats van een hardcoded wachtrijnaam wilt u de naam opgeven van een item in de collectie **appSettings** waarvoor de werkelijke wachtrijnaam. Als de sleutel **appSettings** logqueue is, kan de functie eruitzien als in het volgende voorbeeld.

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

De klasse **NameResolver** kan vervolgens de naam van de wachtrij ophalen uit **appSettings** zoals in het volgende voorbeeld wordt getoond:

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

U doorgeven de klasse **NameResolver** in aan het **JobHost** -object, zoals in het volgende voorbeeld wordt getoond.

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

**Opmerking:** Wachtrij-, tabel- en blob-namen worden herleid, telkens wanneer een functie wordt aangeroepen, maar de blob container namen worden omgezet, alleen wanneer de toepassing wordt gestart. U kunt de containernaam blob niet wijzigen terwijl de taak wordt uitgevoerd.

## <a name="how-to-trigger-a-function-manually"></a>Hoe u handmatig een functie activeren

Gebruik de methode **aanroepen** of de **CallAsync** om een functie handmatig starten, op het **JobHost** -object en het kenmerk **NoAutomaticTrigger** van de functie, zoals in het volgende voorbeeld.

        public class Program
        {
            static void Main(string[] args)
            {
                JobHost host = new JobHost();
                host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
            }

            [NoAutomaticTrigger]
            public static void CreateQueueMessage(
                TextWriter logger,
                string value,
                [Queue("outputqueue")] out string message)
            {
                message = value;
                logger.WriteLine("Creating queue message: ", message);
            }
        }

## <a name="how-to-write-logs"></a>Het schrijven van logboekbestanden

Het Dashboard ziet u Logboeken op twee plaatsen: de pagina voor de WebJob en de pagina voor een specifieke WebJob-aanroep.

![Logboeken in WebJob pagina](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

![Logboeken in functie-aanroep pagina](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

Uitvoer van Console-methoden die u in een functie of de methode **Main() aanroept** wordt weergegeven in de Dashboard-pagina voor de WebJob, niet in de pagina voor het oproepen van een bepaalde methode. Uitvoer van de TextWriter-object dat u van een parameter in de methodehandtekening ontvangt wordt weergegeven op de pagina van het Dashboard voor een methodeaanroep.

Console-uitvoer kan niet worden gekoppeld aan een bepaalde methode aanroepen omdat de Console één thread, is terwijl veel functies tegelijkertijd kunnen worden uitgevoerd. Daarom is de SDK vindt u elke functie-aanroep met zijn eigen unieke logboek writer-object.

U schrijft de [toepassing traceringslogboeken](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview)gebruiken **Console.Out** (wordt gemaakt van logboekbestanden die zijn gemarkeerd als INFO) en **Console.Error** (maakt u zich aanmeldt als fout gemarkeerd). Een alternatief is het [traceren of TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), biedt uitgebreid-, waarschuwings- en kritische niveaus naast Info en fout. Toepassing traceringslogboeken worden weergegeven in de logboekbestanden web app, Azure tabellen, of Azure BLOB's afhankelijk van hoe u uw Azure web app configureren. Geldt voor alle uitvoer van de Console, weergegeven de meest recente 100 toepassingslogboeken ook in de Dashboard-pagina voor de WebJob, niet op de pagina voor een functie-aanroep.

Console-uitvoer wordt weergegeven in het Dashboard alleen als het programma wordt uitgevoerd in een WebJob Azure niet als het programma wordt uitgevoerd, lokaal of in een andere omgeving.

U kunt logboekregistratie uitschakelen door de verbindingsreeks Dashboard in te stellen op null. Zie [configuratie-opties instellen](#how-to-set-configuration-options)voor meer informatie.

In het volgende voorbeeld ziet u verschillende manieren naar Logboeken geschreven:

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

In de WebJobs SDK Dashboard, de uitvoer van het object **TextWriter** wordt weergegeven wanneer u Ga naar de pagina voor een bepaalde functie-aanroep en selecteer **Uitvoer in-of uitschakelen**:

![Aanroep koppeling](./media/vs-storage-webjobs-getting-started-queues/dashboardinvocations.png)

![Logboeken in functie-aanroep pagina](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

In het Dashboard van de SDK WebJobs de laatste 100 regels van de Console-uitvoer weergeven van wanneer u gaat u naar de pagina van de WebJob (niet voor de functie-aanroep) en selecteer **Uitvoer in-of uitschakelen**.

![Uitvoer van in-of uitschakelen](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

In een doorlopende WebJob weergegeven toepassingslogboeken in data/taken/continue /*{webjobname}*/job_log.txt in het bestandssysteem van de web app.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

In een Azure blob-het uiterlijk van de toepassing Logboeken als volgt: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hallo allemaal!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hallo allemaal!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hallo allemaal!,

En in een tabel Azure de logboeken voor **Console.Out** en **Console.Error** ziet er zo uit:

![Info log in tabel](./media/vs-storage-webjobs-getting-started-queues/tableinfo.png)

![Foutenlogboek in tabel](./media/vs-storage-webjobs-getting-started-queues/tableerror.png)

##<a name="next-steps"></a>Volgende stappen

Dit artikel heeft verstrekt codevoorbeelden waarin wordt aangegeven hoe u veelvoorkomende scenario's voor het werken met Azure wachtrijen verwerken. Zie voor meer informatie over het gebruik van Azure WebJobs en de WebJobs SDK [documentatiebronnen Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).

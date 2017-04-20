<properties
    pageTitle="IoT Hub apparaat naar cloud-berichten (.Net) | Microsoft Azure"
    description="Volg deze zelfstudie meer nuttige patronen IoT Hub apparaat naar cloud berichten verwerken."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="csharp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/05/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-net"></a>Zelfstudie: Het verwerken van IoT Hub apparaat-wolk-berichten met behulp van .net

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Inleiding

Azure IoT Hub is een volledig beheerde service waarmee betrouwbare en veilige bi-directionele communicatie tussen miljoenen IoT apparaten en een toepassing back-end. Andere zelfstudie ([aan de slag met IoT Hub] en [wolk naar apparaat verzenden met IoT Hub][lnk-c2d]) hoe u met de apparaat-cloud- en cloud-to-device messaging basisfuncties van IoT Hub.

Deze zelfstudie is gebaseerd op de code die wordt weergegeven in de zelfstudie [aan de slag met IoT Hub] en twee schaalbare patronen die u kunt apparaat-wolk-berichten worden weergegeven:

- De betrouwbare opslag van berichten in [Azure blob-opslag]apparaat naar cloud. Een gebruikelijk scenario is *koud pad* analytics, waarin u de telemetriegegevens in de BLOB's gebruiken als invoer in analytics processen opslaan. Deze processen kunnen worden aangestuurd door hulpmiddelen zoals [Azure Data Factory] of de [HDInsight (Hadoop)] -stack.

- De betrouwbare verwerking van *interactieve* apparaat-wolk-berichten. Apparaat-wolk-berichten zijn interactief wanneer ze direct triggers voor een reeks acties in de back-end van toepassing zijn. Een apparaat kan bijvoorbeeld een waarschuwing weergegeven dat activeert een ticket in te voegen in een CRM-systeem verzenden. Berichten *van de gegevenspunten* feed daarentegen gewoon in een analytics motor. Telemetrie van een apparaat dat moet worden opgeslagen voor latere analyse temperatuur is bijvoorbeeld een bericht van de punt.

Omdat IoT Hub een [Gebeurtenis Hub beschrijft][lnk-event-hubs]-compatibel apparaat aan cloud om berichten te ontvangen, deze zelfstudie wordt een exemplaar van de [EventProcessorHost] -eindpunt. Dit exemplaar:

* Op betrouwbare wijze worden de berichten *van de gegevenspunten* in Azure blob-opslag.
* *Interactieve* apparaat-wolk-berichten doorstuurt naar een Azure [Service Bus wachtrij] voor onmiddellijke verwerking.

Bus service zorgt betrouwbare verwerking van interactieve berichten, zoals per bericht controlepunten en de tijd venster verval dupliceren.

> [AZURE.NOTE] Een exemplaar van de **EventProcessorHost** is slechts één manier om interactieve berichten te verwerken. Andere opties zijn [Azure Service Fabric] [ lnk-service-fabric] en [Azure Stream Analytics][lnk-stream-analytics].

Aan het einde van deze zelfstudie kunt u drie apps voor Windows-console uitvoeren:

* **SimulatedDevice**, een gewijzigde versie van de app gemaakt in de zelfstudie [aan de slag met IoT Hub] , verzendt gegevens punt apparaat naar cloud berichten elke tweede en interactieve apparaat naar cloud berichten elke 10 seconden. Het protocol AMQP dit app gebruikt om te communiceren met IoT Hub.
* De klasse [EventProcessorHost] **ProcessDeviceToCloudMessages** gebruikt voor het ophalen van berichten vanuit het eindpunt van de gebeurtenis Hub-compatibel. Het vervolgens op betrouwbare wijze gegevens punt berichten worden opgeslagen in Azure blob-opslag en interactieve berichten doorstuurt naar een wachtrij Service Bus.
* **ProcessD2CInteractiveMessages** wachtrijen uit voor de interactieve berichten uit de wachtrij Service Bus.

> [AZURE.NOTE] IoT Hub heeft ondersteuning voor veel apparaat platforms en talen, zoals C, Java en JavaScript SDK. Zie meer informatie over het vervangen van het gesimuleerde apparaat in deze zelfstudie met een fysiek apparaat en apparaten aansluiten op een IoT Hub, [Azure IoT Developer Center].

Deze zelfstudie is rechtstreeks van toepassing op andere manieren te verbruiken gebeurtenis Hub-compatibele berichten, zoals [HDInsight (Hadoop)] -projecten. Zie [handleiding voor ontwikkelaars van Azure IoT Hub - apparaat naar cloud]voor meer informatie.

Deze zelfstudie hebt u het volgende nodig:

+ Microsoft Visual Studio 2015.

+ Een actieve account Azure. <br/>Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) maken in een paar minuten.

U moet een basiskennis hebben van [Azure opslag] en [Azure Service Bus].


## <a name="send-interactive-messages-from-a-simulated-device"></a>Interactieve berichten verzenden vanuit een gesimuleerde apparaat.

In deze sectie kunt wijzigen u de toepassing van het gesimuleerde apparaat die u hebt gemaakt in de zelfstudie [aan de slag met IoT Hub] interactieve apparaat-wolk-berichten verzenden naar de hub IoT.

1. Toevoegen de volgende methode aan de klasse **wordt** in het **SimulatedDevice** -project in Visual Studio.

    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();

        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);

        Task.Delay(10000).Wait();
      }
    }
    ```

    Deze methode is vergelijkbaar met de methode **SendDeviceToCloudMessagesAsync** in het **SimulatedDevice** -project. De enige verschillen zijn dat u nu de eigenschap **MessageId** -systeem en een eigenschap van het **messageType**genoemd.
    De code wordt een globally unique identifier (GUID) toegewezen aan de eigenschap **MessageId** . De Bus Service met deze id kunt dupliceren uit de berichten die zijn ontvangen. In het voorbeeld wordt de eigenschap **messageType** te onderscheiden van interactieve gegevens punt berichten. De toepassing geeft deze informatie in het bericht, in plaats van in de hoofdtekst van het bericht, zodat u de gebeurtenisverwerking hoeft niet te converteren van het bericht voor het uitvoeren van berichtroutering.

    > [AZURE.NOTE] Het is belangrijk dat de **MessageId** gebruikt voor het dupliceren van interactieve berichten in de apparaatcode ongedaan te maken. Netwerkcommunicatie met tussenpozen of andere fouten, kunnen resulteren in meerdere hertransmissies van hetzelfde bericht van dat apparaat. U kunt ook een semantische bericht-ID, zoals een hash van het desbetreffende bericht gegevensvelden, in plaats van een GUID.

2. De volgende methode in de methode **Main** toevoegen na de `Console.ReadLine()` regel:

    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````

    > [AZURE.NOTE] Omwille van de eenvoud in deze zelfstudie geen beleid opnieuw geïmplementeerd. In de productiecode, moet u een beleid opnieuw zoals exponentiële backoff, zoals aangegeven in het MSDN-artikel [Tijdelijke fout afhandeling]implementeert.

## <a name="process-device-to-cloud-messages"></a>Verwerken berichten van apparaat naar cloud

In dit gedeelte vindt u een console-app voor Windows waarmee berichten van IoT Hub apparaat naar cloud worden verwerkt. IOT Hub [gebeurtenis Hub] beschrijft-compatibele eindpunt een toepassing apparaat-wolk-berichten inschakelen. In deze zelfstudie wordt de klasse [EventProcessorHost] om deze berichten in een console app te verwerken. Zie voor meer informatie over hoe u berichten verwerken van de gebeurtenis Hubs, de zelfstudie [Aan de slag met de gebeurtenis Hubs] .

De uitdaging bij betrouwbare opslag van het gegevenspunt berichten te implementeren of doorsturen van berichten interactieve verwerking van de gebeurtenis is afhankelijk van de consument bericht ter controlepunten van de voortgang. Bovendien een hoge doorvoersnelheden bereiken bij het lezen van de gebeurtenis Hubs dient u de controlepunten in grote hoeveelheden. Deze benadering wordt gemaakt van de mogelijkheid van dubbele verwerken voor een groot aantal berichten als er een fout is en u kunt naar het vorige controlepunt terugkeren. In deze zelfstudie ziet u de Azure opslag schrijft en Service Bus verval duplicatie windows synchroniseren met **EventProcessorHost** controlepunten.

Om berichten naar Azure opslag op betrouwbare wijze geschreven, wordt de functie van de commit enkel blok van [blok BLOB's][Azure Block Blobs]. De gebeurtenisverwerking tijdje berichten in het geheugen staan totdat het tijd om een controlepunt. Bijvoorbeeld nadat de gecumuleerde buffer van berichten bereikt de maximale blokgrootte van 4 MB, of de Bus Service verval duplicatie tijdsduur is verstreken. Klik vóór het controlepunt, de code een nieuw blok aan de blob voert.

Gebeurtenis Hubs bericht compenseert de gebeurtenisverwerking gebruikt als id's blokkeren. Dit mechanisme kan de gebeurtenis processor een verval dubbels uitvoeren voordat deze het nieuwe blok worden doorgevoerd in de opslag, het verzorgen van een mogelijke crash tussen een blok en het controlepunt wordt doorgevoerd.

> [AZURE.NOTE] In deze zelfstudie wordt één opslag Azure rekening te schrijven van de berichten die zijn opgehaald uit de IoT Hub. Zie [opslag Azure schaalbaarheid richtlijnen]om te bepalen als u wilt meerdere accounts voor Azure opslag gebruiken in uw oplossing.

De toepassing gebruikt de Service Bus duplicatie van de functie om te voorkomen dat dubbele records wanneer deze interactieve berichten verwerkt. Een stempel van het gesimuleerde apparaat elke interactieve bericht met een unieke **MessageId**. Deze id's kunnen Bus Service om ervoor te zorgen dat geen twee berichten met de dezelfde **MessageId** in het venster van de tijd opgegeven duplicatie van verval aan de ontvangers worden geleverd. Dit verval duplicatie, samen met de per bericht voltooiing semantiek wachtrijen Bus Service, die gemakkelijk te implementeren, de betrouwbare verwerking van interactieve berichten.

Om ervoor te zorgen dat er geen bericht opnieuw wordt ingediend buiten het venster duplicatie van verval, synchroniseert de code het mechanisme van de checkpoint **EventProcessorHost** met de duplicatie van het venster met de Service Bus. Deze synchronisatie is gedaan door ten minste eenmaal een controlepunt dwingen telkens wanneer het venster duplicatie van verval is verstreken (in deze zelfstudie, het venster is één uur).

> [AZURE.NOTE] In deze zelfstudie wordt één gepartitioneerde Service Bus wachtrij naar de interactieve berichten opgehaald uit de IoT Hub proces. Zie de [Azure Service Bus] -documentatie voor meer informatie over het gebruik van wachtrijen Service Bus te voldoen aan de eisen van de schaalbaarheid van uw oplossing.

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Inrichten van een account Azure opslag en een wachtrij Service Bus
Als u wilt gebruiken de klasse [EventProcessorHost] , hebt u een account Azure opslag van de **EventProcessorHost** voor het vastleggen van checkpoint inschakelen. U kunt een bestaande opslag Azure-account of volg de instructies in de [Opslag over Azure] een nieuwe opmerking te maken. Maak een notitie van de verbindingsreeks Azure opslag account.

> [AZURE.NOTE] Wanneer u kopieert en plakt u de verbindingsreeks Azure opslag account, Controleer of er geen spaties opgenomen.

U moet ook een wachtrij Service Bus betrouwbare verwerking van interactieve berichten inschakelen. Kunt u een wachtrij via programmacode, met een venster één uur de duplicatie, zoals uiteengezet in [het gebruik van wachtrijen Bus][Service Bus wachtrij]. Ook kunt u de [Azure klassieke portal][lnk-classic-portal], door de volgende stappen:

1. Klik op **Nieuw** in de linkerbenedenhoek. Klik vervolgens op **Services App** > **Service Bus** > **wachtrij** > **Aangepaste maken**. Voer de naam **d2ctutorial**, selecteert u een regio, en een bestaande naamruimte gebruiken of maak een nieuwe Selecteer **dubbele detectie inschakelen**op de volgende pagina en de **dubbele detectie geschiedenisvenster** ingesteld op één uur. Vervolgens klikt u op het vinkje in de rechterbenedenhoek om het opslaan van de configuratie van de wachtrij.

    ![Een wachtrij maken in Azure portal][30]

2. In de lijst Service Bus wachtrijen op **d2ctutorial**en klik vervolgens op **configureren**. Maak twee beleidsregels voor gedeelde toegang, één naam **verzenden** met machtigingen **verzenden** en één wel **luisteren** met machtigingen **luisteren** . Als u klaar bent, klikt u op **Opslaan** onder.

    ![Een wachtrij in Azure portal configureren][31]

3. Klik op **Dashboard** boven, en vervolgens **de verbindingsgegevens** aan de onderkant. Maak een notitie van de twee tekenreeksen.

    ![Wachtrij dashboard in Azure portal][32]

### <a name="create-the-event-processor"></a>De gebeurtenis processor maken

1. In de huidige Visual Studio-oplossing een Visual C# Windows om project te maken met de **Console Application** projectsjabloon, klikt u op **bestand** > **Add** > **Nieuw Project**. Zorg ervoor dat de versie van .NET Framework 4.5.1 of hoger. Geef het project de **ProcessDeviceToCloudMessages**en klik op **OK**.

    ![Nieuw project in Visual Studio][10]

2. In de Solution Explorer met de rechtermuisknop op het **ProcessDeviceToCloudMessages** -project en klik vervolgens op **Beheren NuGet pakketten**. Het dialoogvenster **NuGet Package Manager** wordt weergegeven.

3. Zoeken naar **WindowsAzure.ServiceBus**, klik op **installeren**en accepteer de gebruiksvoorwaarden. Deze bewerking gedownload, geïnstalleerd en een verwijzing toegevoegd naar het [Azure Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus), met alle bijbehorende afhankelijkheden.

4. Zoeken naar **Microsoft.Azure.ServiceBus.EventProcessorHost**, klik op **installeren**en accepteer de gebruiksvoorwaarden. Deze bewerking gedownload, geïnstalleerd en een verwijzing toegevoegd naar de [Azure Service Bus gebeurtenis Hub - EventProcessorHost NuGet package](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), met alle bijbehorende afhankelijkheden.

5. Klik met de rechtermuisknop op het **ProcessDeviceToCloudMessages** -project, klikt u op **toevoegen**en klik op **klasse**. Naam van de nieuwe klasse **StoreEventProcessor**en klik vervolgens op **OK** om de klasse te maken.

6. De volgende instructies toevoegen aan het begin van het bestand StoreEventProcessor.cs:

    ```
    using System.IO;
    using System.Diagnostics;
    using System.Security.Cryptography;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

7. Vervangen door de volgende code voor de hoofdtekst van de klasse:

    ```
    class StoreEventProcessor : IEventProcessor
    {
      private const int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
      public static string StorageConnectionString;
      public static string ServiceBusConnectionString;

      private CloudBlobClient blobClient;
      private CloudBlobContainer blobContainer;
      private QueueClient queueClient;

      private long currentBlockInitOffset;
      private MemoryStream toAppend = new MemoryStream(MAX_BLOCK_SIZE);

      private Stopwatch stopwatch;
      private TimeSpan MAX_CHECKPOINT_TIME = TimeSpan.FromHours(1);

      public StoreEventProcessor()
      {
        var storageAccount = CloudStorageAccount.Parse(StorageConnectionString);
        blobClient = storageAccount.CreateCloudBlobClient();
        blobContainer = blobClient.GetContainerReference("d2ctutorial");
        blobContainer.CreateIfNotExists();
        queueClient = QueueClient.CreateFromConnectionString(ServiceBusConnectionString);
      }

      Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
      {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        return Task.FromResult<object>(null);
      }

      Task IEventProcessor.OpenAsync(PartitionContext context)
      {
        Console.WriteLine("StoreEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);

        if (!long.TryParse(context.Lease.Offset, out currentBlockInitOffset))
        {
          currentBlockInitOffset = 0;
        }
        stopwatch = new Stopwatch();
        stopwatch.Start();

        return Task.FromResult<object>(null);
      }

      async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
      {
        foreach (EventData eventData in messages)
        {
          byte[] data = eventData.GetBytes();

          if (eventData.Properties.ContainsKey("messageType") && (string) eventData.Properties["messageType"] == "interactive")
          {
            var messageId = (string) eventData.SystemProperties["message-id"];

            var queueMessage = new BrokeredMessage(new MemoryStream(data));
            queueMessage.MessageId = messageId;
            queueMessage.Properties["messageType"] = "interactive";
            await queueClient.SendAsync(queueMessage);

            WriteHighlightedMessage(string.Format("Received interactive message: {0}", messageId));
            continue;
          }

          if (toAppend.Length + data.Length > MAX_BLOCK_SIZE || stopwatch.Elapsed > MAX_CHECKPOINT_TIME)
          {
            await AppendAndCheckpoint(context);
          }
          await toAppend.WriteAsync(data, 0, data.Length);

          Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
            context.Lease.PartitionId, Encoding.UTF8.GetString(data)));
        }
      }

      private async Task AppendAndCheckpoint(PartitionContext context)
      {
        var blockIdString = String.Format("startSeq:{0}", currentBlockInitOffset.ToString("0000000000000000000000000"));
        var blockId = Convert.ToBase64String(ASCIIEncoding.ASCII.GetBytes(blockIdString));
        toAppend.Seek(0, SeekOrigin.Begin);
        byte[] md5 = MD5.Create().ComputeHash(toAppend);
        toAppend.Seek(0, SeekOrigin.Begin);

        var blobName = String.Format("iothubd2c_{0}", context.Lease.PartitionId);
        var currentBlob = blobContainer.GetBlockBlobReference(blobName);

        if (await currentBlob.ExistsAsync())
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var blockList = await currentBlob.DownloadBlockListAsync();
          var newBlockList = new List<string>(blockList.Select(b => b.Name));

          if (newBlockList.Count() > 0 && newBlockList.Last() != blockId)
          {
            newBlockList.Add(blockId);
            WriteHighlightedMessage(String.Format("Appending block id: {0} to blob: {1}", blockIdString, currentBlob.Name));
          }
          else
          {
            WriteHighlightedMessage(String.Format("Overwriting block id: {0}", blockIdString));
          }
          await currentBlob.PutBlockListAsync(newBlockList);
        }
        else
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var newBlockList = new List<string>();
          newBlockList.Add(blockId);
          await currentBlob.PutBlockListAsync(newBlockList);

          WriteHighlightedMessage(String.Format("Created new blob", currentBlob.Name));
        }

        toAppend.Dispose();
        toAppend = new MemoryStream(MAX_BLOCK_SIZE);

        // checkpoint.
        await context.CheckpointAsync();
        WriteHighlightedMessage(String.Format("Checkpointed partition: {0}", context.Lease.PartitionId));

        currentBlockInitOffset = long.Parse(context.Lease.Offset);
        stopwatch.Restart();
      }

      private void WriteHighlightedMessage(string message)
      {
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine(message);
        Console.ResetColor();
      }
    }
    ```

    De klasse **EventProcessorHost** roept deze klasse om het apparaat naar cloud-berichten ontvangen van IoT Hub. De code in deze klasse implementeert de logica voor het betrouwbaar opslaan van berichten in een blob-container en interactieve berichten doorsturen naar de wachtrij van de Bus-Service.

    De **OpenAsync** -methode initialiseert de variabele **currentBlockInitOffset** , die bijhoudt van de huidige verschuiving van het eerste bericht lezen door deze gebeurtenis processor. Houd er rekening mee dat elke processor verantwoordelijk voor een enkele partitie is.

    De methode **ProcessEventsAsync** een reeks berichten ontvangt van IoT Hub en deze als volgt verwerkt: interactieve berichten verzendt naar de wachtrij Service Bus en berichten van gegevens punt toegevoegd aan de geheugenbuffer **toAppend**genoemd. Als de geheugenbuffer de limiet van 4 MB bereikt of verval duplicatie tijdvensters (één uur na een controlepunt in deze zelfstudie) is verstreken, activeert de toepassing een controlepunt.

    De methode **AppendAndCheckpoint** genereert eerst een blockId voor het blok toe te voegen. Azure nodig alle id's als u wilt dat dezelfde lengte hebben, zodat de methode de verschuiving met voorloopnullen - pads blokkeren `currentBlockInitOffset.ToString("0000000000000000000000000")`. Vervolgens, als een blok met deze ID al in de blob is, de methode worden overschreven met de huidige inhoud van de buffer.

    > [AZURE.NOTE] Ter vereenvoudiging van de code wordt in deze zelfstudie een enkele blob per partitie de berichten worden opgeslagen. Een echte oplossing zou implementeren bestand ongedaan maken van extra bestanden na een bepaalde tijd of wanneer ze een bepaalde grootte bereikt. Houd er rekening mee dat een blok Azure blob maximaal 195 GB aan gegevens kan bevatten.

8. In de klasse **programma** toevoegen met de volgende instructie met **behulp van** boven:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

9. De **belangrijkste** methode in de klasse van het **programma** als volgt wijzigen. **{Iot hub verbindingsreeks}** vervangen door de tekenreeks **iothubowner** uit de handleiding [aan de slag met IoT Hub] . De verbindingsreeks opslag vervangen door de verbindingstekenreeks die u aan het begin van deze sectie hebt genoteerd. De verbindingsreeks Service Bus vervangen door machtigingen **verzenden** voor de wachtrij met de naam **d2ctutorial** die aan het begin van deze sectie wordt vermeld:

    ```
    static void Main(string[] args)
    {
      string iotHubConnectionString = "{iot hub connection string}";
      string iotHubD2cEndpoint = "messages/events";
      StoreEventProcessor.StorageConnectionString = "{storage connection string}";
      StoreEventProcessor.ServiceBusConnectionString = "{service bus send connection string}";

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, iotHubD2cEndpoint, EventHubConsumerGroup.DefaultGroupName, iotHubConnectionString, StoreEventProcessor.StorageConnectionString, "messages-events");
      Console.WriteLine("Registering EventProcessor...");
      eventProcessorHost.RegisterEventProcessorAsync<StoreEventProcessor>().Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

    > [AZURE.NOTE] Omwille van de eenvoud wordt in deze zelfstudie één exemplaar van de klasse [EventProcessorHost] . Zie de [Gebeurtenis Hubs Programming Guide]voor meer informatie.

## <a name="receive-interactive-messages"></a>Interactieve berichten ontvangen
In dit gedeelte vindt u een console-app voor Windows dat interactieve berichten uit de wachtrij Bus Service ontvangt. Zie [toepassingen met meerdere lagen met Bus Service maken][]voor meer informatie over het ontwerpen van een oplossing met Service Bus.

1. Maak een project van Visual C# Windows met behulp van de **Console Application** projectsjabloon in de huidige Visual Studio-oplossing. Naam van het project **ProcessD2CInteractiveMessages**.

2. In de Solution Explorer met de rechtermuisknop op het **ProcessD2CInteractiveMessages** -project en klik vervolgens op **Beheren NuGet pakketten**. Met deze bewerking wordt het venster **NuGet Package Manager** .

3. Zoeken naar **WindowsAzure.ServiceBus**, klik op **installeren**en accepteer de gebruiksvoorwaarden. Deze bewerking gedownload, geïnstalleerd en een verwijzing toegevoegd naar de [Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus), met alle bijbehorende afhankelijkheden.

4. De volgende instructies voor het **gebruik van** toevoegen boven aan het bestand **Program.cs** :

    ```
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Ten slotte, voeg de volgende regels aan de **Main** -methode. Vervang de verbindingsreeks met de machtigingen voor de **d2ctutorial**met de naam wachtrij **luisteren** :

    ```
    Console.WriteLine("Process D2C Interactive Messages app\n");

    string connectionString = "{service bus listen connection string}";
    QueueClient Client = QueueClient.CreateFromConnectionString(connectionString);

    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = false;
    options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

    Client.OnMessage((message) =>
    {
      try
      {
        var bodyStream = message.GetBody<Stream>();
        bodyStream.Position = 0;
        var bodyAsString = new StreamReader(bodyStream, Encoding.ASCII).ReadToEnd();

        Console.WriteLine("Received message: {0} messageId: {1}", bodyAsString, message.MessageId);

        message.Complete();
      }
      catch (Exception)
      {
        message.Abandon();
      }
    }, options);

    Console.WriteLine("Receiving interactive messages from SB queue...");
    Console.WriteLine("Press any key to exit.");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>De toepassingen worden uitgevoerd.

U bent nu gereed voor de toepassingen worden uitgevoerd.

1.  In Visual Studio Solution Explorer met de rechtermuisknop op uw oplossing en **opstarten van**projecten instellen. Selecteer **meerdere projecten voor opstarten**en vervolgens **starten** als de actie voor de projecten van **ProcessDeviceToCloudMessages**, **SimulatedDevice**en **ProcessD2CInteractiveMessages** .

2.  Druk op **F5** om de drie consoletoepassingen te starten. Elke interactieve bericht van de **SimulatedDevice** -toepassing moet worden verwerkt door de toepassing **ProcessD2CInteractiveMessages** .

  ![Drie consoletoepassingen.][50]

> [AZURE.NOTE] Overzicht van updates in de blob, moet u de constante **MAX_BLOCK_SIZE** in de klasse **StoreEventProcessor** verkleinen tot een kleinere waarde, zoals **1024**. Deze wijziging is nuttig omdat het duurt enige tijd tot de limiet van het blok met de gegevens die worden verzonden door het gesimuleerde apparaat. Met een kleiner blok hoeft niet te wachten zo lang totdat de blob wordt gemaakt en bijgewerkt. Met behulp van een groter blok heeft echter de toepassing meer schaalbaar.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe betrouwbaar gegevenspunt en interactieve apparaat-wolk-berichten worden verwerkt met behulp van de klasse [EventProcessorHost] .

[Het verzenden van berichten met IoT Hub cloud naar apparaat] [ lnk-c2d] hoe u berichten verzenden naar uw apparaten uit uw back-end.

Voorbeelden van volledige end-to-end oplossingen met IoT Hub Zie [Azure IoT Suite][lnk-suite].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, [IoT Hub Developer Guide].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[10]: ./media/iot-hub-csharp-csharp-process-d2c/create-identity-csharp1.png

[30]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue2.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue3.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue4.png

<!-- Links -->

[Azure blob-opslag]: ../storage/storage-dotnet-how-to-use-blobs.md
[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Queue Service Bus]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT Hub developer guide - apparaat naar cloud]: iot-hub-devguide-messaging.md

[Azure opslag]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[IoT Hub Developer Guide]: iot-hub-devguide.md
[Aan de slag met IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Fout met betrekking tot tijdelijke behandeling]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Over Azure opslag]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Aan de slag met gebeurtenis-Hubs]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[Azure opslag schaalbaarheid richtlijnen]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Gebeurtenis Hubs Programming Guide]: ../event-hubs/event-hubs-programming-guide.md
[Fout met betrekking tot tijdelijke behandeling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Bouwen van toepassingen met meerdere lagen met Bus Service]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

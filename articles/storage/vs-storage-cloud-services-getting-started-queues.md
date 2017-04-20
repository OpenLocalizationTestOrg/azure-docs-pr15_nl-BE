<properties
    pageTitle="Aan de slag met de opslag van de wachtrij en Visual Studio verbonden diensten (cloud) | Microsoft Azure"
    description="Hoe aan de slag met Azure wachtrij opslag in een cloud-service-project in Visual Studio nadat services verbinding maken met een opslag-account met behulp van Visual Studio worden verbonden"
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

# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Aan de slag met Azure Queue storage en Visual Studio verbonden services (cloud services-projecten)

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe aan de slag met Azure Queue storage in Visual Studio nadat u hebt gemaakt of een account Azure opslag in een cloud services-project waarnaar wordt verwezen in het dialoogvenster Visual Studio **Verbonden Services toevoegen** .

Leert u hoe u een wachtrij maakt in code. Ook leert u hoe u elementaire bewerkingen, zoals het toevoegen, wijzigen, lezen en verwijderen van berichten in wachtrij plaatsen. De monsters zijn geschreven in C#-code en de [Microsoft Azure opslag Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)gebruiken.

De bewerking **Verbonden Services toevoegen** installeert u de gewenste pakketten NuGet toegang tot Azure opslag in uw project en wordt de verbindingsreeks voor de opslag account toegevoegd aan uw project configuratiebestanden.

 - Zie [aan de slag met Azure wachtrij opslag met behulp van .NET](storage-dotnet-how-to-use-queues.md) voor meer informatie over het manipuleren van de wachtrijen in de code.
 - Zie [opslag-documentatie](https://azure.microsoft.com/documentation/services/storage/) voor algemene informatie over opslag Azure.
 - Zie de [documentatie van Cloud-Services](https://azure.microsoft.com/documentation/services/cloud-services/) voor algemene informatie over Azure cloud services.
 - Zie [ASP.NET](http://www.asp.net) voor meer informatie over het programmeren van ASP.NET-toepassingen.


Azure Queue storage is een service voor het opslaan van een groot aantal berichten dat kunnen worden geopend vanuit overal in de wereld via geverifieerde oproepen met behulp van HTTP of HTTPS. Een bericht van één wachtrij kan tot 64 KB in grootte en een wachtrij kan bevatten miljoenen berichten tot aan de totale capaciteit van een opslagruimte.


## <a name="access-queues-in-code"></a>Toegang tot wachtrijen in code

Voor toegang tot wachtrijen in Visual Studio Cloud Services-projecten, moet u de volgende items aan een C#-bronbestand die toegang hebben tot de wachtrij Azure opslag opnemen.

1. Zorg ervoor dat de naamruimtedeclaraties aan de bovenkant van de C#-bestand omvatten deze instructies **gebruiken** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Krijg een **CloudStorageAccount** -object met de gegevens van uw opslag. De volgende code gebruiken om de de verbindingsreeks voor opslag en opslag-accountgegevens van de configuratie van Azure service.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Krijg een **CloudQueueClient** -object te verwijzen naar de wachtrij-objecten in uw account voor opslag.  

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Krijg een **CloudQueue** -object te verwijzen naar een bepaalde wachtrij.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Opmerking:** Alle van de bovenstaande code voor de code gebruiken in de volgende voorbeelden.

## <a name="create-a-queue-in-code"></a>Een wachtrij maken in code

Maak de wachtrij in code, voegt u een aanroep van **CreateIfNotExists**toe.

    // Create the CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Een bericht aan een wachtrij toevoegen

Een bericht in een bestaande wachtrij worden ingevoegd, een nieuw **CloudQueueMessage** -object en vervolgens roept u de methode **AddMessage** .

Een **CloudQueueMessage** -object kan worden gemaakt van een tekenreeks (in de indeling UTF-8) of een matrix van bytes.

Hier is een voorbeeld dat het bericht "Hello, World" wordt ingevoegd.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Lezen van een bericht in een wachtrij

U kunt bekijken van het bericht aan de voorzijde van een wachtrij zonder door het aanroepen van de methode **PeekMessage** verwijderd uit de wachtrij.

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Lezen en verwijderen van een bericht in een wachtrij

Uw code kunt verwijderen (uit de wachtrij) een bericht uit een wachtrij in twee stappen.

1. Bel **GetMessage** als u het volgende bericht in een wachtrij. Een bericht dat wordt geretourneerd uit **GetMessage** wordt onzichtbaar is voor een andere code voor het lezen van berichten vanuit deze wachtrij. Standaard blijft dit bericht onzichtbaar gedurende 30 seconden.
2.  Als u klaar bent met het bericht uit de wachtrij verwijderen, roept u **DeleteMessage**.

Dit proces van het verwijderen van een bericht bent u verzekerd dat als de code niet werkt voor het verwerken van een bericht door defecte hardware of software, een ander exemplaar van uw code kunt hetzelfde bericht krijgen en probeer het opnieuw. De volgende code roept **DeleteMessage** rechts nadat het bericht is verwerkt.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-to-process-and-remove-queue-messages"></a>Extra opties gebruiken om te verwerken en verwijderen van berichten in wachtrij plaatsen

Er zijn twee manieren waarop u kunt ophalen van berichten uit een wachtrij.

 - U kunt een reeks berichten (maximaal 32).
 - U kunt een time-out invisibility langer of korter uw code zodat meer of minder tijd voor het verwerken van elk bericht volledig instellen. In het volgende voorbeeld wordt de methode **GetMessages** 20 berichten in één aanroep ophalen. Vervolgens wordt elk bericht met een **foreach** lus verwerkt. De time-out invisibility wordt ingesteld op vijf minuten voor elk bericht. Houd er rekening mee dat de 5 minuten gestart voor alle berichten op hetzelfde moment, dus na 5 minuten hebt die is verstreken sinds de aanroep van **GetMessages**, alle berichten die niet zijn verwijderd opnieuw zichtbaar worden.

Hier volgt een voorbeeld:

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete the message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-the-queue-length"></a>Lengte van de wachtrij ophalen

U kunt een schatting van het aantal berichten in een wachtrij. De methode **FetchAttributes** wordt u gevraagd de Queue-service voor het ophalen van de wachtrij-kenmerken, zoals het aantal berichten. De eigenschap **ApproximateMethodCount** retourneert de laatste waarde die is opgehaald met de methode **FetchAttributes** zonder het aanroepen van de service van de wachtrij.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-azure-queue-apis"></a>Gebruik de Async-wachten op een patroon met gemeenschappelijke Azure wachtrij-API 's

In dit voorbeeld ziet u hoe het patroon Async wachten met gemeenschappelijke Azure wachtrij API's gebruiken. Het monster roept de asynchrone versie van elk van de methoden gegeven, deze kunnen worden gezien door de **asynchrone** na correctie van elke methode. Wanneer u een asynchrone methode gebruikt de async-wachten op een patroon opschort lokale totdat de oproep is voltooid. Dit gedrag kan de huidige thread andere werkzaamheden die helpt bij het voorkomen van knelpunten en verbetert de algehele respons van de toepassing. Voor meer details over het gebruik van de asynchrone Await patroon in .NET [asynchrone en Await (C# en Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add the message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete the message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Een wachtrij verwijderen

Voor het verwijderen van een wachtrij en alle berichten die dit, roept u de methode **verwijderen** op het wachtrij-object.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

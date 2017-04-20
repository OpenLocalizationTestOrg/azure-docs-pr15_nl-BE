<properties
    pageTitle="Aan de slag met de opslag van de wachtrij en Visual Studio verbonden services (ASP.NET) | Microsoft Azure"
    description="Hoe aan de slag met Azure Queue storage in een ASP.NET-project in Visual Studio nadat services verbinding maken met een opslag-account met behulp van Visual Studio worden verbonden"
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
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services"></a>Aan de slag met Azure wachtrij opslag en Visual Studio verbonden services

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe aan de slag met Azure Queue storage in Visual Studio nadat u hebt gemaakt of een account Azure opslag in een ASP.NET-project waarnaar wordt verwezen in het dialoogvenster Visual Studio **Verbonden Services toevoegen** .

We laten u zien hoe maken en toegang tot een wachtrij Azure in uw account voor de opslag. Ook leert u hoe u elementaire bewerkingen, zoals het toevoegen, wijzigen, lezen en verwijderen van berichten in wachtrij plaatsen. De monsters zijn geschreven in C#-code en de [Microsoft Azure opslag Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)gebruiken. Voor meer informatie over ASP.NET Zie [ASP.NET](http://www.asp.net).

Azure Queue storage is een service voor het opslaan van een groot aantal berichten dat kunnen worden geopend vanuit overal in de wereld via geverifieerde oproepen met behulp van HTTP of HTTPS. Een bericht van één wachtrij kan tot 64 KB in grootte en een wachtrij kan bevatten miljoenen berichten tot aan de totale capaciteit van een opslagruimte.

## <a name="access-queues-in-code"></a>Toegang tot wachtrijen in code

Voor toegang tot wachtrijen in ASP.NET-projecten, moet u de volgende items aan een C#-bronbestand die toegang hebben tot de wachtrij Azure opslag opnemen.

1. Zorg ervoor dat de naamruimtedeclaraties aan de bovenkant van de C#-bestand omvatten deze instructies **gebruiken** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Krijg een **CloudStorageAccount** -object met de gegevens van uw opslag. De volgende code gebruiken om de de verbindingsreeks voor opslag en opslag-accountgegevens van de configuratie van Azure service.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Krijg een **CloudQueueClient** -object te verwijzen naar de wachtrij-objecten in uw account voor opslag.  

        // Create the CloudQueueClient object for this storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Krijg een **CloudQueue** -object te verwijzen naar een bepaalde wachtrij.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Opmerking** Alle van de bovenstaande code voor de code gebruiken in de volgende voorbeelden.

## <a name="create-a-queue-in-code"></a>Een wachtrij maken in code

Maak een Azure wachtrij in code, voegt u toe een aanroep van **CreateIfNotExists** in de bovenstaande code.

    // Create the messageQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Een bericht aan een wachtrij toevoegen

Een bericht in een bestaande wachtrij worden ingevoegd, een nieuw **CloudQueueMessage** -object en vervolgens roept u de methode **AddMessage** .

Een **CloudQueueMessage** -object kan worden gemaakt van een tekenreeks (in de indeling UTF-8) of een matrix van bytes.

Hier is een voorbeeld dat het bericht "Hello, World" wordt ingevoegd.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Lezen van een bericht in een wachtrij

U kunt bekijken van het bericht aan de voorzijde van een wachtrij zonder het te verwijderen uit de wachtrij van de PeekMessage ()-methode wordt aangeroepen.

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Lezen en verwijderen van een bericht in een wachtrij

Uw code kunt verwijderen (uit de wachtrij) een bericht uit een wachtrij in twee stappen.
1. Bel GetMessage() als u het volgende bericht in een wachtrij. Een bericht dat wordt geretourneerd uit de GetMessage() wordt onzichtbaar is voor een andere code voor het lezen van berichten vanuit deze wachtrij. Standaard blijft dit bericht onzichtbaar gedurende 30 seconden.
2.  Als u klaar bent met het bericht uit de wachtrij verwijderen, roept u **DeleteMessage**.

Dit proces van het verwijderen van een bericht bent u verzekerd dat als de code niet werkt voor het verwerken van een bericht door defecte hardware of software, een ander exemplaar van uw code kunt hetzelfde bericht krijgen en probeer het opnieuw. De volgende code roept **DeleteMessage** rechts nadat het bericht is verwerkt.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-for-de-queuing-messages"></a>Extra opties gebruiken voor berichten in de wachtrij plaatsen

Er zijn twee manieren waarop u kunt ophalen van berichten uit een wachtrij.
U krijgt eerst een reeks berichten (maximaal 32). Ten tweede kunt u instellen een time-out invisibility langer of korter waardoor uw code meer of minder tijd aan elk bericht volledig verwerken. In het volgende voorbeeld wordt de methode **GetMessages** 20 berichten in één aanroep ophalen. Vervolgens wordt elk bericht met een **foreach** lus verwerkt. De time-out invisibility wordt ingesteld op vijf minuten voor elk bericht. Houd er rekening mee dat de 5 minuten gestart voor alle berichten op hetzelfde moment, dus na 5 minuten hebt die is verstreken sinds de aanroep van **GetMessages**, alle berichten die niet zijn verwijderd opnieuw zichtbaar worden.

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Lengte van de wachtrij ophalen

U kunt een schatting van het aantal berichten in een wachtrij. De methode **FetchAttributes** wordt gevraagd de queueservice op te halen van de wachtrij-kenmerken, zoals het aantal berichten. De eigenschap **ApproximateMethodCount** retourneert de laatste waarde die is opgehaald met de methode **FetchAttributes** , zonder de queueservice.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-async-await-pattern-with-common-queueapis"></a>Wachten op Async patroon gebruiken met algemene queueAPIs

In dit voorbeeld ziet u hoe de Async-wachten op een patroon gebruiken met algemene queueAPIs. Het monster roept de asynchrone versie van elk van de methoden gegeven, deze kunnen worden gezien door de asynchrone na correctie van elke methode. Wanneer u een asynchrone methode gebruikt de async-wachten op een patroon opschort lokale totdat de oproep is voltooid. Dit gedrag kan de huidige thread andere werkzaamheden die helpt bij het voorkomen van knelpunten en verbetert de algehele respons van de toepassing. Voor meer details over het gebruik van de asynchrone Await patroon in .NET [asynchrone en Await (C# en Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Een wachtrij verwijderen

Voor het verwijderen van een wachtrij en alle berichten die dit, roept u de methode **verwijderen** op het wachtrij-object.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
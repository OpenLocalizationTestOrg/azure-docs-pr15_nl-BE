<properties
    pageTitle="Aan de slag met de opslag van de wachtrij en Visual Studio verbonden services (ASP.NET 5) | Microsoft Azure"
    description="Hoe aan de slag met Azure wachtrij opslag in een ASP.NET-5-project in Visual Studio"
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

# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-5"></a>Aan de slag met de opslag van de wachtrij en Visual Studio verbonden services (ASP.NET 5)

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

##<a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe aan de slag met Azure Queue storage in Visual Studio nadat u hebt gemaakt of een account Azure opslag in een ASP.NET-5-project waarnaar wordt verwezen in het dialoogvenster Visual Studio **Verbonden Services toevoegen** . De bewerking **Verbonden Services toevoegen** installeert u de gewenste pakketten NuGet toegang tot Azure opslag in uw project en wordt de verbindingsreeks voor de opslag account toegevoegd aan uw project configuratiebestanden.

Azure queue storage is een service voor het opslaan van een groot aantal berichten dat kunnen worden geopend vanuit overal in de wereld via geverifieerde oproepen met behulp van HTTP of HTTPS. Een enkele wachtrij bericht kan maximaal 64 kilobytes (KB) en een wachtrij kan bevatten miljoenen berichten tot aan de totale capaciteit van een opslagruimte.

Om te beginnen moet u eerst een Azure wachtrij maken in uw account voor opslag. Leert u hoe u een wachtrij maakt in code. Ook leert u hoe u de wachtrij voor eenvoudige bewerkingen, zoals het toevoegen, wijzigen, lezen en verwijderen van berichten in wachtrij plaatsen. De monsters zijn geschreven in C\# code en gebruikt de clientbibliotheek Azure opslag voor .NET. Voor meer informatie over ASP.NET Zie [ASP.NET](http://www.asp.net).

**Opmerking:** Sommige van de API's die aanroepen naar Azure opslag in ASP.NET 5 uitvoeren zijn asynchroon. Zie [asynchroon asynchrone en Await programmeren](http://msdn.microsoft.com/library/hh191443.aspx) voor meer informatie. De volgende code wordt ervan uitgegaan dat asynchrone programmering methoden worden gebruikt.

- Zie [aan de slag met Azure wachtrij opslag met behulp van .NET](storage-dotnet-how-to-use-queues.md) voor meer informatie over het programmatisch manipuleren van wachtrijen.
- Zie [opslag-documentatie](https://azure.microsoft.com/documentation/services/storage/) voor algemene informatie over opslag Azure.
- Zie de [documentatie van Cloud-Services](https://azure.microsoft.com/documentation/services/cloud-services/) voor algemene informatie over Azure cloud services.
- Zie [ASP.NET](http://www.asp.net) voor meer informatie over het programmeren van ASP.NET-toepassingen.





##<a name="access-queues-in-code"></a>Toegang tot wachtrijen in code

Voor toegang tot wachtrijen in ASP.NET 5 projecten, moet u zijn de volgende items aan een C# bronbestand dat Azure wachtrij opslag opent.

1. Zorg ervoor dat de naamruimtedeclaraties aan de bovenkant van de C#-bestand omvatten deze instructies **gebruiken** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Krijg een **CloudStorageAccount** -object met de gegevens van uw opslag. De volgende code gebruiken om de de verbindingsreeks voor opslag en opslag-accountgegevens van de configuratie van Azure service.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Krijg een **CloudQueueClient** -object te verwijzen naar de wachtrij-objecten in uw account voor opslag.  

        // Create the CloudQueueClient object for the storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Krijg een **CloudQueue** -object te verwijzen naar een bepaalde wachtrij.

        // Get a reference to the CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Opmerking:** Alle van de bovenstaande code voor de code gebruiken in de volgende voorbeelden.

###<a name="create-a-queue-in-code"></a>Een wachtrij maken in code

De Azure om wachtrij te maken in code, voegt u een aanroep naar **CreateIfNotExistsAsync**toe.

    // Create the CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

##<a name="add-a-message-to-a-queue"></a>Een bericht aan een wachtrij toevoegen

Een bericht in een bestaande wachtrij worden ingevoegd, een nieuw **CloudQueueMessage** -object en vervolgens roept u de methode **AddMessageAsync** .

Een **CloudQueueMessage** -object kan worden gemaakt van een tekenreeks (in de indeling UTF-8) of een matrix van bytes.

Hier is een voorbeeld dat het bericht "Hello, World" wordt ingevoegd.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

##<a name="read-a-message-in-a-queue"></a>Lezen van een bericht in een wachtrij

U kunt bekijken van het bericht aan de voorzijde van een wachtrij zonder het te verwijderen uit de wachtrij door het aanroepen van de methode **PeekMessageAsync** .

    // Peek the next message in the queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


##<a name="read-and-remove-a-message-in-a-queue"></a>Lezen en verwijderen van een bericht in een wachtrij

Uw code kunt verwijderen (wachtrij) een bericht uit een wachtrij in twee stappen.
1. Bel **GetMessageAsync** als u het volgende bericht in een wachtrij. Een bericht dat wordt geretourneerd uit de **GetMessageAsync** wordt onzichtbaar is voor een andere code voor het lezen van berichten vanuit deze wachtrij. Standaard blijft dit bericht onzichtbaar gedurende 30 seconden.
2.  Als u klaar bent met het bericht uit de wachtrij verwijderen, roept u **DeleteMessageAsync**.

Dit proces van het verwijderen van een bericht bent u verzekerd dat als de code niet werkt voor het verwerken van een bericht door defecte hardware of software, een ander exemplaar van uw code kunt hetzelfde bericht krijgen en probeer het opnieuw. De volgende code roept **DeleteMessageAsync** rechts nadat het bericht is verwerkt.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process the message in less than 30 seconds.

    // Then delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a>Extra opties voor berichten waarbij hefboomwerking

Er zijn twee manieren waarop u kunt ophalen van berichten uit een wachtrij.
U krijgt eerst een reeks berichten (maximaal 32). Ten tweede kunt u instellen een time-out invisibility langer of korter waardoor uw code meer of minder tijd aan elk bericht volledig verwerken. In het volgende voorbeeld wordt de methode **GetMessages** 20 berichten in één aanroep ophalen. Vervolgens wordt elk bericht met een **foreach** lus verwerkt. De time-out invisibility wordt ingesteld op 5 minuten voor elk bericht. Houd er rekening mee dat het 5 minuten start voor alle berichten op hetzelfde moment, dus na 5 minuten hebben doorgegeven na de aanroep van **GetMessages**, alle berichten die niet zijn verwijderd, weer zichtbaar worden.

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Lengte van de wachtrij ophalen

U kunt een schatting van het aantal berichten in een wachtrij. De methode **FetchAttributes** wordt u gevraagd de queue-service voor het ophalen van de wachtrij-kenmerken, zoals het aantal berichten. De eigenschap **ApproximateMethodCount** retourneert de laatste waarde die is opgehaald met de methode **FetchAttributes** zonder het aanroepen van de wachtrijservice.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display the number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>Het patroon Async wachten met de wachtrij met algemene API's gebruiken

In dit voorbeeld ziet hoe u het patroon Async wachten met wachtrij met algemene API's. Het monster roept de asynchrone versie van elk van de methoden gegeven. Deze kunnen worden gezien door de asynchrone na correctie van elke methode. Wanneer een asynchrone methode wordt gebruikt, opschort het wachten op Async patroon lokale totdat de oproep is voltooid. Dit gedrag kan de huidige thread andere werkzaamheden die helpt bij het voorkomen van knelpunten en verbetert de algehele respons van de toepassing. Zie voor meer informatie over het gebruik van de asynchrone Await patroon in .NET [asynchrone en Await (C# en Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to add to the queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## <a name="delete-a-queue"></a>Een wachtrij verwijderen

Voor het verwijderen van een wachtrij en alle berichten die dit, roept u de methode **verwijderen** op het wachtrij-object.

    // Delete the queue.
    messageQueue.Delete();


##<a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

<properties
    pageTitle="Aan de slag met Azure wachtrij opslag met behulp van .NET | Microsoft Azure"
    description="Azure wachtrijen bieden betrouwbare, asynchrone berichten tussen componenten van toepassing. Cloud messaging kunnen de componenten afzonderlijk geschaald."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="robinsh"/>

# <a name="get-started-with-azure-queue-storage-using-net"></a>Aan de slag met Azure wachtrij opslag met behulp van .NET

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht

Azure Queue storage biedt cloud messaging tussen toepassingsonderdelen van de. Bij het ontwerpen van toepassingen voor schaal, zijn componenten van toepassing vaak ontkoppeld, zodat ze kunnen onafhankelijk van elkaar worden geschaald. Wachtrij opslag biedt voor communicatie tussen toepassingscomponenten van, een systeem voor asynchrone berichtafhandeling of ze worden uitgevoerd in de cloud, op het bureaublad, op een server op locatie of op een mobiel apparaat. Wachtrij opslag biedt ook ondersteuning voor asynchrone taken beheren en processen proces te bouwen.

### <a name="about-this-tutorial"></a>Over deze handleiding

Deze zelfstudie laat zien hoe .NET code schrijven voor enkele algemene scenario's met Azure Queue storage. Scenario's die zijn maken en verwijderen van wachtrijen toe te voegen, lezen en verwijderen van berichten in wachtrij plaatsen.

**Geschatte tijd:** 45 minuten

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure opslag Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Configuration Manager for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- Een [Azure opslag account](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Naamruimtedeclaraties toevoegen

Voeg de volgende `using` instructies aan de bovenkant van de `program.cs` bestand:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types

### <a name="parse-the-connection-string"></a>De verbindingsreeks parseren

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a>De wachtrij-client maken

De klasse **CloudQueueClient** kunt u wachtrijen die zijn opgeslagen in de opslag van de wachtrij ophalen. Hier is één manier om de client te maken:

    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

U bent nu gereed voor de code schrijven die de gegevens leest uit en schrijft gegevens naar de wachtrij opslag.

## <a name="create-a-queue"></a>Een wachtrij maken

In dit voorbeeld ziet u hoe een wachtrij maken als deze nog niet bestaat:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a container.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

## <a name="insert-a-message-into-a-queue"></a>Plaats een bericht in een wachtrij

Een bericht in een bestaande wachtrij worden ingevoegd, moet u eerst een nieuwe **CloudQueueMessage**maken. Vervolgens wordt de methode **AddMessage** aanroepen. Een **CloudQueueMessage** kan worden gemaakt van een tekenreeks (in de indeling UTF-8) of een matrix van **bytes** . Dit is de code waarmee u een wachtrij maakt (als deze niet bestaat) en het bericht "Hello, World" wordt ingevoegd:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.AddMessage(message);

## <a name="peek-at-the-next-message"></a>Het volgende bericht bekijken

U kunt bekijken van het bericht aan de voorzijde van een wachtrij zonder door het aanroepen van de methode **PeekMessage** verwijderd uit de wachtrij.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Peek at the next message
    CloudQueueMessage peekedMessage = queue.PeekMessage();

    // Display message.
    Console.WriteLine(peekedMessage.AsString);

## <a name="change-the-contents-of-a-queued-message"></a>De inhoud van een bericht in de wachtrij wijzigen

U kunt de inhoud van een bericht ter plaatse in de wachtrij wijzigen. Als het bericht een werktaak, kunt u deze functie voor het bijwerken van de status van de werktaak. De volgende code het bericht wachtrij bijgewerkt met nieuwe inhoud en de zichtbaarheid van time-out uit te breiden van een andere 60 seconden. De status van de werkzaamheden die zijn gekoppeld aan het bericht wordt opgeslagen, en geeft de client een andere minuut om verder te werken op het bericht. U kunt deze techniek voor het bijhouden van werkstromen meerdere stappen op Wachtrijberichten, zonder te beginnen vanaf het begin als een stap door defecte hardware of software mislukt. Normaal gesproken zou u een aantal nieuwe pogingen ook behouden en als het bericht is meer dan *n* keer opnieuw geprobeerd, zou u deze verwijderen. Dit biedt bescherming tegen een bericht dat een fout wordt gegenereerd telkens wanneer die deze wordt verwerkt.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the message from the queue and update the message contents.
    CloudQueueMessage message = queue.GetMessage();
    message.SetMessageContent("Updated contents.");
    queue.UpdateMessage(message,
        TimeSpan.FromSeconds(60.0),  // Make it visible for another 60 seconds.
        MessageUpdateFields.Content | MessageUpdateFields.Visibility);

## <a name="de-queue-the-next-message"></a>Het volgende bericht uit de wachtrij

Uw code wachtrijen uit een bericht in een wachtrij in twee stappen. Wanneer u **GetMessage belt**, krijgt u het volgende bericht in een wachtrij. Een bericht dat wordt geretourneerd uit **GetMessage** wordt onzichtbaar is voor een andere code voor het lezen van berichten vanuit deze wachtrij. Standaard blijft dit bericht onzichtbaar gedurende 30 seconden. Als u klaar bent met het bericht uit de wachtrij te verwijderen, moet u **DeleteMessage**aanroepen. Dit proces van het verwijderen van een bericht bent u verzekerd dat als de code niet werkt voor het verwerken van een bericht door defecte hardware of software, een ander exemplaar van uw code kunt hetzelfde bericht krijgen en probeer het opnieuw. Nadat het bericht is verwerkt, roept de code **DeleteMessage** rechts.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the next message
    CloudQueueMessage retrievedMessage = queue.GetMessage();

    //Process the message in less than 30 seconds, and then delete the message
    queue.DeleteMessage(retrievedMessage);

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Async-wachten op een patroon met gemeenschappelijke Queue storage API's gebruiken

In dit voorbeeld ziet hoe u de Async-wachten op een patroon met gemeenschappelijke Queue storage API's. Het monster roept de asynchrone versie van elk van de bepaalde methoden, zoals aangegeven door de *Async* -achtervoegsel van elke methode. Wanneer een asynchrone methode wordt gebruikt, wordt de async-wachten op een patroon opschort lokale totdat de oproep is voltooid. Dit gedrag kan de huidige thread andere werkzaamheden, die helpt bij het voorkomen van knelpunten en verbetert de algehele respons van de toepassing. Zie voor meer informatie over het gebruik van de asynchrone Await patroon in .NET [asynchrone en Await (C# en Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create the queue if it doesn't already exist
    if(await queue.CreateIfNotExistsAsync())
    {
        Console.WriteLine("Queue '{0}' Created", queue.Name);
    }
    else
    {
        Console.WriteLine("Queue '{0}' Exists", queue.Name);
    }

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await queue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await queue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="leverage-additional-options-for-de-queuing-messages"></a>Maak gebruik van aanvullende opties voor de berichten in de wachtrij plaatsen

Er zijn twee manieren waarop u kunt ophalen van berichten uit een wachtrij.
U krijgt eerst een reeks berichten (maximaal 32). Ten tweede kunt u instellen een time-out invisibility langer of korter waardoor uw code meer of minder tijd aan elk bericht volledig verwerken. In het volgende voorbeeld wordt de methode **GetMessages** 20 berichten in één aanroep ophalen. Vervolgens wordt elk bericht met een **foreach** lus verwerkt. De time-out invisibility wordt ingesteld op vijf minuten voor elk bericht. Houd er rekening mee dat de 5 minuten gestart voor alle berichten op hetzelfde moment, dus na 5 minuten hebt die is verstreken sinds de aanroep van **GetMessages**, alle berichten die niet zijn verwijderd opnieuw zichtbaar worden.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

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

U kunt een schatting van het aantal berichten in een wachtrij. De methode **FetchAttributes** wordt u gevraagd de Queue-service voor het ophalen van de wachtrij-kenmerken, zoals het aantal berichten. De eigenschap **ApproximateMessageCount** retourneert de laatste waarde die is opgehaald met de methode **FetchAttributes** zonder het aanroepen van de service van de wachtrij.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Fetch the queue attributes.
    queue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = queue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="delete-a-queue"></a>Een wachtrij verwijderen

Voor het verwijderen van een wachtrij en alle berichten die dit, roept u de methode **verwijderen** op het wachtrij-object.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Delete the queue.
    queue.Delete();

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van Queue storage, klik op deze koppelingen voor meer informatie over complexere opslagtaken.

- Bekijk de wachtrij service-documentatie voor volledige informatie over beschikbare API's:
    - [Client-bibliotheek van opslag voor referentie .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [REST API: naslag](http://msdn.microsoft.com/library/azure/dd179355)
- Informatie over het vereenvoudigen van de code die u schrijft om te werken met Azure opslag met behulp van de [SDK van Azure WebJobs](../app-service-web/websites-dotnet-webjobs-sdk.md).
- Bekijk meer functie gidsen voor meer informatie over aanvullende opties voor het opslaan van gegevens in Azure.
    - [Aan de slag met Azure tabel opslag met behulp van .NET](storage-dotnet-how-to-use-tables.md) voor het opslaan van gestructureerde gegevens.
    - [Aan de slag met Azure Blob-opslag met behulp van .NET](storage-dotnet-how-to-use-blobs.md) ongestructureerde gegevens op te slaan.
    - [Verbinding maken met een SQL-Database met behulp van .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) voor het opslaan van relationele gegevens.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2

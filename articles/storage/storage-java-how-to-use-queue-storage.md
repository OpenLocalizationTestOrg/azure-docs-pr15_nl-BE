<properties
    pageTitle="Het gebruik van opslag van de wachtrij van Java | Microsoft Azure"
    description="Informatie over het gebruik van de service Azure wachtrij maken en verwijderen van wachtrijen en invoegen, ophalen en verwijderen van berichten. Monsters die zijn geschreven in Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-java"></a>Queue storage van Java gebruiken

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht

Deze handleiding wordt beschreven hoe u voor het uitvoeren van veelvoorkomende scenario's met behulp van de wachtrij Azure storage-service. De monsters zijn geschreven in Java en de [Azure opslag SDK for Java][]gebruiken. De scenario's bestreken omvatten berichten **Invoegen**, **inspecteren**, **ophalen**en **verwijderen** en wachtrijen **maken** en **verwijderen** . Zie de sectie [volgende stappen](#Next-Steps) voor meer informatie over wachtrijen.

Opmerking: Er is een SDK beschikbaar voor ontwikkelaars die werken met Azure opslag op Android-apparaten. Zie de [SDK van Azure opslag voor Android][]voor meer informatie.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Een Java-toepassing maken

In deze handleiding gebruikt u opslagfuncties die kunnen worden uitgevoerd binnen een Java-toepassing lokaal of in de code die wordt uitgevoerd binnen een webpagina rol of functie van de werknemer in Azure.

Hiertoe moet u de Java Development Kit (JDK) installeren en een account Azure opslag maken van je abonnement op Azure. Nadat u dit hebt gedaan, moet u controleren of uw ontwikkelingssysteem voldoet aan de minimumvereisten en de afhankelijkheden die zijn opgenomen in de bibliotheek [Azure opslag SDK voor Java][] op GitHub. Als uw systeem voldoet aan deze eisen wordt voldaan, kunt u de instructies voor downloaden en installeren van de bibliotheken Azure opslag voor Java op uw systeem uit die bibliotheek. Als u deze taken hebt voltooid, kunt u zijn kunnen maken van een Java-toepassing die wordt gebruikt in de voorbeelden in dit artikel.

## <a name="configure-your-application-to-access-queue-storage"></a>Uw toepassing configureren voor toegang tot opslag in wachtrij

De volgende importinstructies toevoegen aan de bovenkant van het Java-bestand waar u Azure opslag API's gebruiken om de toegang tot wachtrijen:

    // Include the following imports to use queue APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.queue.*;

## <a name="setup-an-azure-storage-connection-string"></a>Een verbindingsreeks Azure opslag Setup

Een opslag Azure-client gebruikt een verbindingsreeks opslag eindpunten en referenties voor het openen van gegevens wilt opslaan. Wanneer een clienttoepassing wordt uitgevoerd, moet u opgeven de verbindingsreeks voor opslag in de volgende indeling met de naam van uw account voor de opslag en de van primaire toegangssleutel voor de opslag in [Azure Portal](https://portal.azure.com) voor de *accountnaam* en *AccountKey* -waarden weergegeven. In dit voorbeeld ziet u hoe u een statisch veld voor het opslaan van de verbindingsreeks kunt declareren:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

In een toepassing die wordt uitgevoerd binnen een rol in Microsoft Azure, wordt deze tekenreeks kan worden opgeslagen in het configuratiebestand service, *ServiceConfiguration.cscfg*, en is toegankelijk via een aanroep van de methode **RoleEnvironment.getConfigurationSettings** . Hier volgt een voorbeeld van de verbindingsreeks opvragen van een **instelling** -element met de naam *StorageConnectionString* in het configuratiebestand van de service:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

De volgende voorbeelden wordt ervan uitgegaan dat u een van deze twee methoden hebt gebruikt om de verbindingsreeks voor opslag.

## <a name="how-to-create-a-queue"></a>Procedure: een wachtrij maken

Een **CloudQueueClient** -object kunt u de verwijzing naar objecten ophalen voor wachtrijen. De volgende code maakt een **CloudQueueClient** -object. (Opmerking: Er zijn meer manieren voor het maken van objecten **CloudStorageAccount** ; Zie **CloudStorageAccount** in de [Referentie voor Azure opslag Client SDK]voor meer informatie.)

Het **CloudQueueClient** -object gebruiken om een verwijzing naar de wachtrij die u wilt gebruiken. Als deze niet bestaat, kunt u de wachtrij maken.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the queue client.
       CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

       // Retrieve a reference to a queue.
       CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Create the queue if it doesn't already exist.
       queue.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-a-message-to-a-queue"></a>Procedure: een bericht aan een wachtrij toevoegen

Een bericht in een bestaande wachtrij worden ingevoegd, moet u eerst een nieuwe **CloudQueueMessage**maken. Vervolgens wordt de methode **addMessage** aanroepen. Een **CloudQueueMessage** kan worden gemaakt van een tekenreeks (in de indeling UTF-8) of een matrix van bytes. Dit is de code waarmee u een wachtrij maakt (als deze niet bestaat) en het invoegen van het bericht "Hello, World".

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Create the queue if it doesn't already exist.
        queue.createIfNotExists();

        // Create a message and add it to the queue.
        CloudQueueMessage message = new CloudQueueMessage("Hello, World");
        queue.addMessage(message);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-peek-at-the-next-message"></a>Procedure: het volgende bericht bekijken

U kunt bekijken van het bericht aan de voorzijde van een wachtrij zonder door het aanroepen van **peekMessage**verwijderd uit de wachtrij.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Peek at the next message.
        CloudQueueMessage peekedMessage = queue.peekMessage();

        // Output the message value.
        if (peekedMessage != null)
        {
          System.out.println(peekedMessage.getMessageContentAsString());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedure: de inhoud van een bericht in de wachtrij wijzigen

U kunt de inhoud van een bericht ter plaatse in de wachtrij wijzigen. Als het bericht een werktaak, kunt u deze functie voor het bijwerken van de status van de werktaak. De volgende code het bericht wachtrij bijgewerkt met nieuwe inhoud en de zichtbaarheid van time-out uit te breiden van een andere 60 seconden. De status van de werkzaamheden die zijn gekoppeld aan het bericht wordt opgeslagen, en geeft de client een andere minuut om verder te werken op het bericht. U kunt deze techniek voor het bijhouden van werkstromen meerdere stappen op Wachtrijberichten, zonder te beginnen vanaf het begin als een stap door defecte hardware of software mislukt. Normaal gesproken zou u een aantal nieuwe pogingen ook behouden en als het bericht is meer dan *n* keer opnieuw geprobeerd, zou u deze verwijderen. Dit biedt bescherming tegen een bericht dat een fout wordt gegenereerd telkens wanneer die deze wordt verwerkt.

De volgende code voorbeeld gezocht in de wachtrij van berichten, zoekt u naar het eerste bericht dat overeenkomt met "Hello, World" voor de inhoud, en vervolgens de weergegeven inhoud wijzigen en sluit af.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // The maximum number of messages that can be retrieved is 32.
        final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

        // Loop through the messages in the queue.
        for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
        {
            // Check for a specific string.
            if (message.getMessageContentAsString().equals("Hello, World"))
            {
                // Modify the content of the first matching message.
                message.setMessageContent("Updated contents.");
                // Set it to be visible in 30 seconds.
                EnumSet<MessageUpdateFields> updateFields =
                    EnumSet.of(MessageUpdateFields.CONTENT,
                    MessageUpdateFields.VISIBILITY);
                // Update the message.
                queue.updateMessage(message, 30, updateFields, null, null);
                break;
            }
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

In het volgende voorbeeld werkt ook alleen de eerste zichtbare bericht in de wachtrij.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage message = queue.retrieveMessage();

        if (message != null)
        {
            // Modify the message content.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 60 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 60, updateFields, null, null);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-get-the-queue-length"></a>Procedure: de lengte van de wachtrij ophalen

U kunt een schatting van het aantal berichten in een wachtrij. De methode **downloadAttributes** wordt de service van de wachtrij voor verschillende huidige waarden met een telling van het aantal berichten in een wachtrij worden gevraagd. De telling is alleen bij benadering, omdat berichten kunnen worden toegevoegd of verwijderd nadat de Queue-service op uw aanvraag reageert. De methode **getApproximateMessageCount** retourneert de laatste waarde die is opgehaald door het aanroepen van **downloadAttributes**, zonder de service van de wachtrij.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Download the approximate message count from the server.
        queue.downloadAttributes();

        // Retrieve the newly cached approximate message count.
        long cachedMessageCount = queue.getApproximateMessageCount();

        // Display the queue length.
        System.out.println(String.format("Queue length: %d", cachedMessageCount));
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-dequeue-the-next-message"></a>Procedure: het volgende bericht in wachtrij

Uw code dequeues een bericht uit een wachtrij in twee stappen. Als u **retrieveMessage**aanroept, krijgt u het volgende bericht in een wachtrij. Een bericht dat wordt geretourneerd uit de **retrieveMessage** wordt onzichtbaar is voor een andere code voor het lezen van berichten vanuit deze wachtrij. Standaard blijft dit bericht onzichtbaar gedurende 30 seconden. Als u klaar bent met het bericht uit de wachtrij te verwijderen, moet u **deleteMessage**aanroepen. Dit proces van het verwijderen van een bericht bent u verzekerd dat als de code niet werkt voor het verwerken van een bericht door defecte hardware of software, een ander exemplaar van uw code kunt hetzelfde bericht krijgen en probeer het opnieuw. Nadat het bericht is verwerkt, roept de code **deleteMessage** rechts.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage retrievedMessage = queue.retrieveMessage();

        if (retrievedMessage != null)
        {
            // Process the message in less than 30 seconds, and then delete the message.
            queue.deleteMessage(retrievedMessage);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }


## <a name="additional-options-for-dequeuing-messages"></a>Extra opties voor waarbij berichten

Er zijn twee manieren waarop u kunt ophalen van berichten uit een wachtrij. U krijgt eerst een reeks berichten (maximaal 32). Ten tweede kunt u instellen een time-out invisibility langer of korter waardoor uw code meer of minder tijd aan elk bericht volledig verwerken.

In het volgende voorbeeld wordt de methode **retrieveMessages** 20 berichten in één aanroep ophalen. Vervolgens wordt elk bericht met behulp van een lus **for** verwerkt. Ook wordt de invisibility-out ingesteld op vijf minuten (300 seconden) voor elk bericht. Dat de vijf minuten wordt gestart voor alle berichten op hetzelfde moment, dus wanneer vijf minuten verstreken sinds de aanroep van **retrieveMessages**, alle berichten die niet zijn verwijderd weer zichtbaar worden.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
        for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
            // Do processing for all messages in less than 5 minutes,
            // deleting each message after processing.
            queue.deleteMessage(message);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-queues"></a>Procedure: de wachtrijen weergeven

Als u een lijst met de huidige wachtrijen, roept de methode **CloudQueueClient.listQueues()** een collectie van objecten **CloudQueue retourneert** .

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient =
            storageAccount.createCloudQueueClient();

        // Loop through the collection of queues.
        for (CloudQueue queue : queueClient.listQueues())
        {
            // Output each queue name.
            System.out.println(queue.getName());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-queue"></a>Procedure: een wachtrij verwijderen

Voor het verwijderen van een wachtrij en alle berichten die dit, roept u de methode **deleteIfExists** van het **CloudQueue** -object.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Delete the queue if it exists.
        queue.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van de opslag van de wachtrij, volg deze koppelingen voor meer informatie over complexere opslagtaken.

- [Azure opslag SDK voor Java][]
- [Verwijzing naar Azure opslag Client SDK][]
- [Azure opslagservices REST API][]
- [Azure opslag-teamblog][]

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure opslag SDK voor Java]: https://github.com/azure/azure-storage-java
[Azure opslag SDK voor Android]: https://github.com/azure/azure-storage-android
[Verwijzing naar Azure opslag Client SDK]: http://dl.windowsazure.com/storage/javadoc/
[Azure opslagservices REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure opslag-teamblog]: http://blogs.msdn.com/b/windowsazurestorage/

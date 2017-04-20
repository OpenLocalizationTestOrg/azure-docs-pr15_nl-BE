<properties
    pageTitle="Het gebruik van opslag van de wachtrij (C++) | Microsoft Azure"
    description="Informatie over het gebruik van de wachtrij storage-service in Azure. Voorbeelden zijn geschreven in C++."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-queue-storage-from-c"></a>Het gebruik van opslag van C++ wachtrij  

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht
Deze handleiding wordt beschreven hoe u voor het uitvoeren van veelvoorkomende scenario's met behulp van de wachtrij Azure storage-service. De monsters worden geschreven in C++ en gebruikt de [Clientbibliotheek van Azure opslag voor C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). De scenario's die zijn **ingevoegd**, **inspecteren**, **ophalen**en **verwijderen van** berichten, alsmede **maken en verwijderen van wachtrijen**.

>[AZURE.NOTE] Deze handleiding is bedoeld voor de Azure opslag Client Library for C++ versie 1.0.0 en boven. De aanbevolen versie is opslag clientbibliotheek 2.2.0, beschikbaar via [NuGet](http://www.nuget.org/packages/wastorage) of [GitHub](http://github.com/Azure/azure-storage-cpp/).

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Maak een C++-toepassing  
In deze handleiding vindt u opslagfuncties die kunnen worden uitgevoerd binnen een C++-toepassing gebruikt.

Hiertoe moet u de Azure opslag Client-bibliotheek voor C++ installeren en een account Azure opslag maken van je abonnement op Azure.

Als u wilt installeren de Azure opslag Client Library for C++, kunt u de volgende methoden:

-   **Linux:** Volg de instructies in de [Azure opslag Client-bibliotheek voor C++ Leesmij-](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) pagina.
-   **Windows:** Klik in Visual Studio **Extra > NuGet Package Manager > Package Manager-Console**. Typ de volgende opdracht in de [console NuGet Package Manager](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , en druk op **ENTER**.

        Install-Package wastorage

## <a name="configure-your-application-to-access-queue-storage"></a>Uw toepassing configureren voor toegang tot opslag in wachtrij
Voeg dat de volgende instructies aan de bovenkant van de C++-bestand waar u de Azure opslag API's gebruiken om de toegang tot wachtrijen omvatten:  

    #include "was/storage_account.h"
    #include "was/queue.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Een verbindingsreeks Azure opslag instellen

Een opslag Azure-client gebruikt een verbindingsreeks opslag eindpunten en referenties voor het openen van gegevens wilt opslaan. Wanneer een clienttoepassing wordt uitgevoerd, moet u opgeven de verbindingsreeks voor opslag in de volgende indeling met de naam van uw account voor de opslag en de toegangstoets opslag voor de opslag in [Azure Portal](https://portal.azure.com) voor de *accountnaam* en *AccountKey* -waarden weergegeven. Zie voor meer informatie over opslag rekeningen en toegangstoetsen, [Azure opslag rekeningen](storage-create-storage-account.md). In dit voorbeeld ziet u hoe u een statisch veld voor het opslaan van de verbindingsreeks kunt declareren:  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Test uw toepassing in uw lokale Windows-computer, kunt u de Microsoft Azure [opslag emulator](storage-use-emulator.md) die met de [Azure SDK](https://azure.microsoft.com/downloads/)is geïnstalleerd. De opslag-emulator is een hulpprogramma waarmee de Blob, wachtrij en tabel services beschikbaar in Azure plaatselijke ontwikkeling simuleert. In het volgende voorbeeld ziet u hoe u een statisch veld voor het opslaan van de verbindingsreeks aan uw lokale opslag emulator kunt declareren:  

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Als de emulator Azure opslag, klik op de knop **Start** of de **Windows** -toets. Begin met typen **Azure opslag Emulator**en **Microsoft Azure opslag Emulator** selecteren uit de lijst met toepassingen.

De volgende voorbeelden wordt ervan uitgegaan dat u een van deze twee methoden hebt gebruikt om de verbindingsreeks voor opslag.

## <a name="retrieve-your-connection-string"></a>De verbindingsreeks ophalen
De klasse **cloud_storage_account** kunt u uw accountgegevens opslag vertegenwoordigen. Voor het ophalen van gegevens van uw opslag uit de verbindingsreeks voor de opslag, kunt u de methode voor **parseren** .

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-queue"></a>Procedure: een wachtrij maken
Een **cloud_queue_client** -object kunt u de verwijzing naar objecten ophalen voor wachtrijen. De volgende code maakt een **cloud_queue_client** -object.

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create a queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

Het **cloud_queue_client** -object gebruiken om een verwijzing naar de wachtrij die u wilt gebruiken. Als deze niet bestaat, kunt u de wachtrij maken.

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();  

## <a name="how-to-insert-a-message-into-a-queue"></a>Procedure: plaats een bericht in een wachtrij
Een bericht in een bestaande wachtrij worden ingevoegd, moet u eerst een nieuwe **cloud_queue_message**maken. Vervolgens wordt de methode **add_message** aanroepen. Een **cloud_queue_message** kan worden gemaakt van een tekenreeks of een matrix van **bytes** . Dit is de code waarmee u een wachtrij maakt (als deze niet bestaat) en het bericht "Hello, World" wordt ingevoegd:

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();

    // Create a message and add it to the queue.
    azure::storage::cloud_queue_message message1(U("Hello, World"));
    queue.add_message(message1);  

## <a name="how-to-peek-at-the-next-message"></a>Procedure: het volgende bericht bekijken
U kunt bekijken van het bericht aan de voorzijde van een wachtrij zonder het te verwijderen uit de wachtrij door het aanroepen van de methode **peek_message** .

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Peek at the next message.
    azure::storage::cloud_queue_message peeked_message = queue.peek_message();

    // Output the message content.
    std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedure: de inhoud van een bericht in de wachtrij wijzigen
U kunt de inhoud van een bericht ter plaatse in de wachtrij wijzigen. Als het bericht een werktaak, kunt u deze functie voor het bijwerken van de status van de werktaak. De volgende code het bericht wachtrij bijgewerkt met nieuwe inhoud en de zichtbaarheid van time-out uit te breiden van een andere 60 seconden. De status van de werkzaamheden die zijn gekoppeld aan het bericht wordt opgeslagen, en geeft de client een andere minuut om verder te werken op het bericht. U kunt deze techniek voor het bijhouden van werkstromen meerdere stappen op Wachtrijberichten, zonder te beginnen vanaf het begin als een stap door defecte hardware of software mislukt. Normaal gesproken zou u een aantal nieuwe pogingen ook behouden en als het bericht wordt opnieuw geprobeerd meer dan n keer, zou u deze verwijderen. Dit biedt bescherming tegen een bericht dat een fout wordt gegenereerd telkens wanneer die deze wordt verwerkt.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the message from the queue and update the message contents.
    // The visibility timeout "0" means make it visible immediately.
    // The visibility timeout "60" means the client can get another minute to continue
    // working on the message.
    azure::storage::cloud_queue_message changed_message = queue.get_message();

    changed_message.set_content(U("Changed message"));
    queue.update_message(changed_message, std::chrono::seconds(60), true);

    // Output the message content.
    std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  

## <a name="how-to-de-queue-the-next-message"></a>Procedure: het volgende bericht uit de wachtrij
Uw code wachtrijen uit een bericht in een wachtrij in twee stappen. Als u **get_message**aanroept, krijgt u het volgende bericht in een wachtrij. Een bericht dat wordt geretourneerd uit de **get_message** wordt onzichtbaar is voor een andere code voor het lezen van berichten vanuit deze wachtrij. Als u klaar bent met het bericht uit de wachtrij te verwijderen, moet u ook **delete_message**aanroepen. Dit proces van het verwijderen van een bericht bent u verzekerd dat als de code niet werkt voor het verwerken van een bericht door defecte hardware of software, een ander exemplaar van uw code kunt hetzelfde bericht krijgen en probeer het opnieuw. De code roept **delete_message** rechts nadat het bericht is verwerkt.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the next message.
    azure::storage::cloud_queue_message dequeued_message = queue.get_message();
    std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

    // Delete the message.
    queue.delete_message(dequeued_message);

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Procedure: gebruikmaken van extra opties voor berichten in de wachtrij plaatsen
Er zijn twee manieren waarop u kunt ophalen van berichten uit een wachtrij. U krijgt eerst een reeks berichten (maximaal 32). Ten tweede kunt u instellen een time-out invisibility langer of korter waardoor uw code meer of minder tijd aan elk bericht volledig verwerken. In het volgende voorbeeld wordt de methode **get_messages** 20 berichten in één aanroep ophalen. Vervolgens wordt elk bericht met behulp van een lus **for** verwerkt. De time-out invisibility wordt ingesteld op vijf minuten voor elk bericht. Houd er rekening mee dat de 5 minuten gestart voor alle berichten op hetzelfde moment, dus na 5 minuten hebt die is verstreken sinds de aanroep van **get_messages**, alle berichten die niet zijn verwijderd opnieuw zichtbaar worden.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
    // 5 minutes (300 seconds).
    azure::storage::queue_request_options options;
    azure::storage::operation_context context;

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

    for (auto it = messages.cbegin(); it != messages.cend(); ++it)
    {
        // Display the contents of the message.
        std::wcout << U("Get: ") << it->content_as_string() << std::endl;
    }

## <a name="how-to-get-the-queue-length"></a>Procedure: de lengte van de wachtrij ophalen
U kunt een schatting van het aantal berichten in een wachtrij. De methode **download_attributes** wordt u gevraagd de Queue-service voor het ophalen van de wachtrij-kenmerken, zoals het aantal berichten. De methode **approximate_message_count** haalt het geraamde aantal berichten in de wachtrij.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Fetch the queue attributes.
    queue.download_attributes();

    // Retrieve the cached approximate message count.
    int cachedMessageCount = queue.approximate_message_count();

    // Display number of messages.
    std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  

## <a name="how-to-delete-a-queue"></a>Procedure: een wachtrij verwijderen
Voor het verwijderen van een wachtrij en alle berichten die dit, roept u de methode **delete_queue_if_exists** van het wachtrijobject.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // If the queue exists and delete it.
    queue.delete_queue_if_exists();  

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van Queue storage, klik op deze koppelingen voor meer informatie over de opslag van Azure.

-   [Het gebruik van C++ Blob-opslag](storage-c-plus-plus-how-to-use-blobs.md)
-   [Het gebruik van opslag van C++ tabel](storage-c-plus-plus-how-to-use-tables.md)
-   [Lijst Azure opslagbronnen in C++](storage-c-plus-plus-enumeration.md)
-   [Client-bibliotheek voor C++ verwijzing van opslag](http://azure.github.io/azure-storage-cpp)
-   [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)


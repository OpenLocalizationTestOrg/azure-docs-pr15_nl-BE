<properties
    pageTitle="Hoe u blob-opslag (opslag van objecten) van C++ | Microsoft Azure"
    description="Ongestructureerde gegevens opslaan in de cloud met Azure Blob-opslag (opslag van objecten)."
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

# <a name="how-to-use-blob-storage-from-c"></a>Het gebruik van C++ Blob-opslag  

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

Azure Blob storage is een service die ongestructureerde gegevens worden opgeslagen in de cloud als objecten/BLOB's. BLOB-opslag kan tekst of binaire gegevens, zoals een document, bestand of installatieprogramma voor elk type worden opgeslagen. BLOB-opslag is ook opslag van objecten genoemd.

Deze handleiding demonstreert het uitvoeren van veelvoorkomende scenario's met behulp van de Azure Blob storage-service. De monsters worden geschreven in C++ en gebruikt de [Clientbibliotheek van Azure opslag voor C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). De scenario's die zijn **geüpload** **aanbieding**, **downloaden**en BLOB's **verwijderen** .  

>[AZURE.NOTE] Deze handleiding is bedoeld voor de Azure opslag Client Library for C++ versie 1.0.0 en boven. De aanbevolen versie is opslag clientbibliotheek 2.2.0, beschikbaar via [NuGet](http://www.nuget.org/packages/wastorage) of [GitHub](https://github.com/Azure/azure-storage-cpp).

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Maak een C++-toepassing
In deze handleiding vindt u opslagfuncties die kunnen worden uitgevoerd binnen een C++-toepassing gebruikt.  

Hiertoe moet u de Azure opslag Client-bibliotheek voor C++ installeren en een account Azure opslag maken van je abonnement op Azure.   

Als u wilt installeren de Azure opslag Client Library for C++, kunt u de volgende methoden:

-   **Linux:** Volg de instructies in de [Azure opslag Client-bibliotheek voor C++ Leesmij-](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) pagina.  
-   **Windows:** Klik in Visual Studio **Extra > NuGet Package Manager > Package Manager-Console**. Typ de volgende opdracht in de [console NuGet Package Manager](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , en druk op **ENTER**.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-blob-storage"></a>Uw toepassing configureren voor toegang tot de Blob-opslag  
Voeg dat de volgende instructies aan de bovenkant van de C++-bestand waar u de Azure opslag API's gebruikt om BLOB's omvatten:  

    #include "was/storage_account.h"
    #include "was/blob.h"

## <a name="setup-an-azure-storage-connection-string"></a>Een verbindingsreeks Azure opslag Setup
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

Vervolgens krijgt u een verwijzing naar een klasse **cloud_blob_client** omdat u voor het ophalen van objecten die staan voor containers en BLOB's opgeslagen in de Blob Storage-Service. De volgende code maakt een **cloud_blob_client** -object dat het accountobject van de opslag dat we boven opgehaald:  

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  

## <a name="how-to-create-a-container"></a>Procedure: een container maken

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

In dit voorbeeld ziet u hoe een container maken als deze nog niet bestaat:  

    try
    {
        // Retrieve storage account from connection string.
        azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

        // Create the blob client.
        azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

        // Retrieve a reference to a container.
        azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

        // Create the container if it doesn't already exist.
        container.create_if_not_exists();
    }
    catch (const std::exception& e)
    {
        std::wcout << U("Error: ") << e.what() << std::endl;
    }  

Standaard is de nieuwe container privé en moet u de toegangstoets opslag voor het BLOB's downloaden van deze container. Als u wilt dat de bestanden (BLOB's) binnen de container beschikbaar maken voor iedereen, kunt u instellen dat de container als public met de volgende code:  

    // Make the blob container publicly accessible.
    azure::storage::blob_container_permissions permissions;
    permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
    container.upload_permissions(permissions);  

Iedereen op Internet BLOB's in een openbare container kunt zien, maar u kunt wijzigen of ze alleen verwijderen als u de juiste sleutel hebt.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Procedure: een blob uploaden naar een container
Azure Blob-opslag ondersteunt BLOB's blokkeren en BLOB's pagina. Blok blob is in de meeste gevallen het aanbevolen type te gebruiken.  

Als u wilt een bestand uploadt naar een blob blok, u container en gebruikt het blok blob verwijst. Nadat u een verwijzing blob hebt, kunt u kunt de stroom van gegevens door het aanroepen van de methode **upload_from_stream** uploaden. Deze bewerking wordt de blob maken als deze niet eerder bestaan of deze overschrijven als deze bestaat. In het volgende voorbeeld ziet u hoe een blob uploaden naar een container en wordt ervan uitgegaan dat de container al is gemaakt.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Create or overwrite the "my-blob-1" blob with contents from a local file.
    concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
    blockBlob.upload_from_stream(input_stream);
    input_stream.close().wait();

    // Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
    // Retrieve a reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
    blob2.upload_text(U("more text"));

    // Retrieve a reference to a blob named "my-blob-3".
    azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
    blob3.upload_text(U("other text"));  

Ook kunt u de methode **upload_from_file** een bestand te uploaden naar een blob blokkeren.

## <a name="how-to-list-the-blobs-in-a-container"></a>Procedure: lijst van de BLOB's in een container
Als u het aantal BLOB's in een container, de eerste keer een verwijzing van de container. Vervolgens kunt u de container **list_blobs** methode voor het ophalen van de BLOB's en/of mappen binnen het. Als u de uitgebreide verzameling eigenschappen en methoden voor een geretourneerde **list_blob_item**, moet u de methode **list_blob_item.as_blob** om een **cloud_blob** -object, of de methode **list_blob.as_directory** om een cloud_blob_directory-object aanroepen. De volgende code laat zien hoe op te halen en de uitvoer van de URI van elk item in de container van **Mijn monster-container** :

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Output URI of each item.
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
        }
        else
        {
            std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
    }

Zie voor meer informatie over het weergeven van bewerkingen [Lijst Azure opslagbronnen in C++](storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Procedure: BLOB's downloaden
BLOB's downloaden, eerst een blob-objectverwijzing opgehaald en vervolgens de methode **download_to_stream** aanroepen. In het volgende voorbeeld wordt de methode **download_to_stream** gebruikt de blob inhoud overbrengen naar een streamobject dat u vervolgens naar een lokaal bestand kunt aanhouden.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Save blob contents to a file.
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    blockBlob.download_to_stream(output_stream);

    std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();

    outfile.write((char *)&data[0], buffer.size());
    outfile.close();  

Ook kunt u de methode **download_to_file** om de inhoud van een blob naar een bestand te downloaden.
Bovendien kun je de **download_text** -methode voor het downloaden van de inhoud van een blob als een tekenreeks.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

    // Download the contents of a blog as a text string.
    utility::string_t text = text_blob.download_text();

## <a name="how-to-delete-blobs"></a>Procedure: BLOB's verwijderen
De eerste keer een blob-referentie wilt verwijderen een blob, en vervolgens de methode **delete_blob** aanroepen op deze.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Delete the blob.
    blockBlob.delete_blob();

## <a name="next-steps"></a>Volgende stappen
U de basisbeginselen van blob-opslag hebt geleerd, klik op deze koppelingen voor meer informatie over de opslag van Azure.  

-   [Het gebruik van opslag van C++ wachtrij](storage-c-plus-plus-how-to-use-queues.md)
-   [Het gebruik van opslag van C++ tabel](storage-c-plus-plus-how-to-use-tables.md)
-   [Lijst Azure opslagbronnen in C++](storage-c-plus-plus-enumeration.md)
-   [Client-bibliotheek voor C++ verwijzing van opslag](http://azure.github.io/azure-storage-cpp)
-   [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)
- [Gegevens overbrengen met het opdrachtregelprogramma AzCopy](storage-use-azcopy.md)

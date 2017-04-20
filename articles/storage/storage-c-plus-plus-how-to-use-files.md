<properties
    pageTitle="Het gebruik van opslag van C++ | Microsoft Azure"
    description="Gegevens in een bestand opslaan in de cloud met Azure bestandsopslag."
    services="storage"
    documentationCenter=".net"
    authors="seguler"
    manager="jahogg"
    editor="tysonn" />

<tags ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="seguler" />

# <a name="how-to-use-file-storage-from-c"></a>Het gebruik van opslag van C++

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]
[AZURE.INCLUDE [storage-file-overview-include](../../includes/storage-file-overview-include.md)]

## <a name="about-this-tutorial"></a>Over deze handleiding

In deze zelfstudie leert u hoe u eenvoudige bewerkingen uitvoeren op het bestand met Microsoft Azure storage-service. Via de voorbeelden geschreven in C++, leert u shares en mappen maken, uploaden, weergeven en verwijderen van bestanden. Als u voor het opslaan van bestanden-service van Microsoft Azure, via de concepten in de volgende secties worden heel handig zijn bij het begrijpen van de monsters.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Maak een C++-toepassing

De monsters bouwen, moet u de Azure opslag clientbibliotheek 2.4.0 voor C++ installeren. U moet hebt een account Azure opslag ook gemaakt.

De Azure opslag Client installeert 2.4.0 voor C++, kunt u een van de volgende methoden:

-   **Linux:** Volg de instructies in de [Azure opslag Client-bibliotheek voor C++ Leesmij-](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) pagina.

-   **Windows:** Klik in Visual Studio **Extra &gt; NuGet Package Manager &gt; Package Manager-Console**. Typ de volgende opdracht in de [console NuGet Package Manager](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , en druk op **ENTER**.

    Wastorage installatiepakket

## <a name="set-up-your-application-to-use-file-storage"></a>Uw toepassing voor het gebruik van de opslag van bestanden instellen

Voeg dat de volgende bevatten instructies aan de bovenkant van de C++-bestand waar u de Azure opslag API's gebruiken om bestanden te openen:

    #include "was/storage_account.h"
    #include "was/file.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Een verbindingsreeks Azure opslag instellen

Voor het gebruik van opslag van bestanden, moet u verbinding maken met uw account Azure opslag. De eerste stap zou zijn voor het configureren van een verbindingstekenreeks waarmee we verbinding maken met uw account voor opslag. We definiëren een statische variabele kunt u doen.

    // Define the connection-string with your values.
    const utility::string_t 
    storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

## <a name="connecting-to-an-azure-storage-account"></a>Verbinding maken met een account met Azure opslag

De klasse **cloud_storage_account** kunt u uw accountgegevens opslag vertegenwoordigen. Voor het ophalen van gegevens van uw opslag uit de verbindingsreeks voor de opslag, kunt u de methode voor **parseren** .

    // Retrieve storage account from connection string. 
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-share"></a>Procedure: een Share maken

Alle bestanden en mappen in de opslag van bestanden bevinden zich in een zogenaamd een **Share**. Uw account opslag kan zoveel aandelen als de capaciteit van de account kan hebben. Om toegang te krijgen tot een share en de inhoud ervan, moet u een bestand opslag-client gebruiken.

    // Create the file storage client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();

Met de client bestand opslag, kunt u vervolgens verkrijgen een verwijzing naar een share.

    // Get a reference to the file share
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));

Gebruik de methode **create_if_not_exists** van het **cloud_file_share** -object om de share te maken.

    if (share.create_if_not_exists()) { 
        std::wcout << U("New share created") << std::endl;  
    }

Op dit punt bevat **delen** een verwijzing naar een share met de naam **Mijn monster-share**.

## <a name="how-to-upload-a-file"></a>Procedure: een bestand uploaden

Tenminste bevat een Azure-bestandsshare opslag een hoofdmap waar bestanden kunnen zich bevinden. In dit gedeelte leert u het uploaden van een bestand van lokale opslag naar de hoofdmap van een share.

De eerste stap bij het uploaden van een bestand is voor een verwijzing naar de map waarin deze zich moet bevinden. U doen dit door het aanroepen van de methode **get_root_directory_reference** van het object delen.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();

Nu dat er een verwijzing naar de hoofdmap van de share, kunt u een bestand naar het uploaden. In het volgende voorbeeld worden uit een bestand, tekst en een stroom.

    // Upload a file from a stream.
    concurrency::streams::istream input_stream = 
      concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

    azure::storage::cloud_file file1 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
    file1.upload_from_stream(input_stream);

    // Upload some files from text.
    azure::storage::cloud_file file2 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    file2.upload_text(_XPLATSTR("more text"));

    // Upload a file from a file.
    azure::storage::cloud_file file4 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    file4.upload_from_file(_XPLATSTR("DataFile.txt"));  

## <a name="how-to-create-a-directory"></a>Procedure: een map maken

Ook kunt u de opslag indelen door het plaatsen van bestanden in submappen in plaats van ze allemaal in de hoofdmap. De Azure bestand storage-service kunt u zoveel mappen maken als uw account toestaat. De volgende code maakt een map met de naam **Mijn monster-map** onder de hoofdmap als een submap met de naam **Mijn monster-submap**.
    
    // Retrieve a reference to a directory
    azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Return value is true if the share did not exist and was successfully created.
    directory.create_if_not_exists();
    
    // Create a subdirectory.
    azure::storage::cloud_file_directory subdirectory = 
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    subdirectory.create_if_not_exists();

## <a name="how-to-list-files-and-directories-in-a-share"></a>Procedure: bestanden en mappen in een lijst

Het verkrijgen van een lijst met bestanden en mappen binnen een share is gemakkelijk gedaan door het aanroepen van **list_files_and_directories** op **cloud_file_directory** verwijst. Als u de uitgebreide verzameling eigenschappen en methoden voor een geretourneerde **list_file_and_directory_item**, moet u de methode **list_file_and_directory_item.as_file** om een **cloud_file** -object, of de methode **list_file_and_directory_item.as_directory** om een **cloud_file_directory** -object aanroepen.

De volgende code laat zien hoe op te halen en de uitvoer van de URI van elk item in de hoofdmap van de share.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    // Output URI of each item.
    azure::storage::list_file_and_diretory_result_iterator end_of_results;
    
    for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
    {
        if(it->is_directory())
        {
            ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
        else if (it->is_file())
        {
            ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
        }       
    }
    

## <a name="how-to-download-a-file"></a>Procedure: een bestand downloaden

Eerst een verwijzing naar een bestand ophalen om bestanden te downloaden, en roept vervolgens de methode **download_to_stream** om de inhoud van het bestand overbrengen naar een stream-object dat u vervolgens naar een lokaal bestand kunt aanhouden. Ook kunt u de methode **download_to_file** om de inhoud van een bestand naar een lokaal bestand te downloaden. De methode **download_text** kunt u downloaden van de inhoud van een bestand als een tekenreeks.

Het volgende voorbeeld worden de methoden **download_to_stream** en **download_text** om aan te tonen met het downloaden van de bestanden die zijn gemaakt in de vorige hoofdstukken.

    // Download as text
    azure::storage::cloud_file text_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    utility::string_t text = text_file.download_text();
    ucout << "File Text: " << text << std::endl;
    
    // Download as a stream.
    azure::storage::cloud_file stream_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    stream_file.download_to_stream(output_stream);
    std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();
    outfile.write((char *)&data[0], buffer.size());
    outfile.close();

## <a name="how-to-delete-a-file"></a>Procedure: een bestand verwijderen

Een andere algemene opslag bestandsbewerking is verwijderen van bestanden. De volgende code verwijdert een bestand met de naam Mijn-monster-bestand-3 onder de hoofdmap opgeslagen.

    // Get a reference to the root directory for the share. 
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    file.delete_file_if_exists();

## <a name="how-to-delete-a-directory"></a>Procedure: een map verwijderen

Als u een map is een eenvoudige taak, hoewel moet worden opgemerkt dat u een map nog steeds bestanden bevat of andere mappen niet verwijderen.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // Get a reference to the directory.
    azure::storage::cloud_file_directory directory = 
      share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Get a reference to the subdirectory you want to delete.
    azure::storage::cloud_file_directory sub_directory =
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    
    // Delete the subdirectory and the sample directory.
    sub_directory.delete_directory_if_exists();
    
    directory.delete_directory_if_exists();

## <a name="how-to-delete-a-share"></a>Procedure: een Share verwijderen

Een share verwijderen wordt uitgevoerd door de **delete_if_exists** -methode aanroept voor een cloud_file_share-object. Hier is voorbeeldcode die dat doet.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // delete the share if exists
    share.delete_share_if_exists();

## <a name="set-the-maximum-size-for-a-file-share"></a>De maximale grootte voor een gedeeld bestand

U kunt quota (of de maximale grootte) instellen voor een gedeeld bestand in gigabytes. U kunt ook controleren om te zien hoeveel gegevens momenteel zijn opgeslagen op de share.

Het quotum voor een share instelt, kunt u de totale grootte van de bestanden op de share te beperken. Als de totale grootte van bestanden op de share het quotum ingesteld op de share overschrijdt, vervolgens kunnen clients geen vergroten van bestaande bestanden of nieuwe bestanden maken, tenzij deze leeg zijn.

In het volgende voorbeeld ziet u het controleren van het huidige gebruik voor een share en het instellen van de quota voor de share.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    if (share.exists())
    {
        std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();
    
        //This line sets the quota to 2560GB
        share.resize(2560);
    
        std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();
    
    }

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Een gedeelde access-handtekening voor een bestand of een bestandsshare genereren

U kunt genereren een gedeelde toegang handtekening (SAS) voor een gedeeld bestand of een afzonderlijk bestand. U kunt ook een beleid voor gedeelde toegang in een bestandsshare voor het beheren van gedeelde toegang handtekeningen maken. Maken van een gedeelde access-beleid wordt aanbevolen, aangezien deze een manier om de SA's intrekken biedt als moet worden aangetast.

In het volgende voorbeeld maakt u een gedeelde access-beleid op een share en vervolgens wordt gebruikt dat beleid de beperkingen van een SAS op een bestand in de share.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client and get a reference to the share
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    if (share.exists())
    {
        // Create and assign a policy
        utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

        azure::storage::file_shared_access_policy sharedPolicy = 
          azure::storage::file_shared_access_policy();

        //set permissions to expire in 90 minutes
        sharedPolicy.set_expiry(utility::datetime::utc_now() + 
          utility::datetime::from_minutes(90));

        //give read and write permissions
        sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

        //set permissions for the share
        azure::storage::file_share_permissions permissions; 

        //retrieve the current list of shared access policies
        azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;
        
        //add the new shared policy
        policies.insert(std::make_pair(policy_name, sharedPolicy));

        //save the updated policy list
        permissions.set_policies(policies);
        share.upload_permissions(permissions);

        //Retrieve the root directory and file references
        azure::storage::cloud_file_directory root_dir = 
          share.get_root_directory_reference();
        azure::storage::cloud_file file = 
          root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

        // Generate a SAS for a file in the share 
        //  and associate this access policy with it.       
        utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);
        
        // Create a new CloudFile object from the SAS, and write some text to the file.     
        azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
        utility::string_t text = _XPLATSTR("My sample content");        
        file_with_sas.upload_text(text);        
        
        //Download and print URL with SAS.
        utility::string_t downloaded_text = file_with_sas.download_text();      
        ucout << downloaded_text << std::endl;      
        ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;
    
    }

Zie voor meer informatie over het maken en het gebruik van handtekeningen voor gedeelde toegang [Met behulp van gedeelde toegang handtekeningen (SAS)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over opslag in Azure, deze bronnen te verkennen:

-   [Opslag Client Library for C++](https://github.com/Azure/azure-storage-cpp)

-   [Azure Opslagverkenner](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)

-   [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)

<properties
    pageTitle="Hoe u blob-opslag (opslag van objecten) van PHP | Microsoft Azure"
    description="Ongestructureerde gegevens opslaan in de cloud met Azure Blob-opslag (opslag van objecten)."
    documentationCenter="php"
    services="storage"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-blob-storage-from-php"></a>Het gebruik van PHP blob-opslag

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

Azure Blob storage is een service die ongestructureerde gegevens worden opgeslagen in de cloud als objecten/BLOB's. BLOB-opslag kan tekst of binaire gegevens, zoals een document, bestand of installatieprogramma voor elk type worden opgeslagen. BLOB-opslag is ook opslag van objecten genoemd.

Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's met de Azure blob-service uitvoeren. De voorbeelden zijn geschreven in PHP en gebruikt u de [SDK voor PHP Azure] [download]. De scenario's die zijn **geüpload** **aanbieding**, **downloaden**en BLOB's **verwijderen** . Voor meer informatie over BLOB's, Zie de sectie [volgende stappen](#next-steps) .

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Een PHP-toepassing maken

De enige vereiste voor het maken van een PHP-toepassing die toegang heeft tot de service Azure blob is de verwijzing naar klassen in de SDK Azure voor PHP van uw code. Kunt u alle ontwikkelprogramma's voor het maken van uw toepassing, zoals Kladblok.

In deze handleiding vindt u service-functies die kunnen worden aangeroepen vanuit een PHP-toepassing lokaal of code die wordt uitgevoerd binnen een webpagina Azure rol, rol van werknemer of website.

## <a name="get-the-azure-client-libraries"></a>De Azure clientbibliotheken ophalen

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Uw toepassing configureren voor toegang tot de service blob

Als u wilt de Azure blob service API's gebruikt, moet u:

1. Een verwijzing naar de autoloader-bestand met de instructie [require_once] en
2. Verwijzen naar alle klassen die u kunt gebruiken.

In het volgende voorbeeld ziet u hoe de autoloader-bestand opnemen en verwijzen naar de klasse **ServicesBuilder** .

> [AZURE.NOTE] In dit voorbeeld (en andere voorbeelden in dit artikel) wordt ervan uitgegaan dat u de PHP Client Libraries voor Azure via Composer hebt geïnstalleerd. Als u de bibliotheken handmatig hebt geïnstalleerd, moet u verwijzen naar de `WindowsAzure.php` autoloader-bestand.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


In de voorbeelden hieronder, de `require_once` instructie wordt altijd weergegeven, maar alleen de klassen die nodig zijn voor het voorbeeld uit te voeren naar worden verwezen.

## <a name="set-up-an-azure-storage-connection"></a>Een verbinding Azure opslag instellen

U moet een geldige verbindingsreeks hebben voor het concretiseren van een client van Azure blob. De indeling van de blob service connection string is:

Voor toegang tot een live-service:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Voor toegang tot de opslag-emulator:

    UseDevelopmentStorage=true


Elke client Azure service maken, moet u de klasse **ServicesBuilder** . U kunt:

* Door de verbindingsreeks door te geven aan deze of
* **CloudConfigurationManager (CCM)** gebruiken om te controleren van meerdere externe bronnen voor de verbindingsreeks:
    * Standaard wordt geleverd met ondersteuning voor een externe bron - omgevingsvariabelen.
    * U kunt nieuwe bronnen toevoegen door de klasse **ConnectionStringSource** uit te breiden.

Voor de hier beschreven voorbeelden, de verbindingsreeks rechtstreeks doorgegeven.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

## <a name="create-a-container"></a>Maken van een container

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Een **BlobRestProxy** -object kunt u een container blob maken met de methode **createContainer** . Bij het maken van een container, u kunt opties instellen op de container, maar dit is dus niet vereist. (In het volgende voorbeeld ziet u hoe de container toegangsbeheerlijst (ACL) en de metagegevens van de container instellen.)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
    use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    // OPTIONAL: Set public access policy and metadata.
    // Create container options object.
    $createContainerOptions = new CreateContainerOptions();

    // Set public access policy. Possible values are
    // PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
    // CONTAINER_AND_BLOBS:
    // Specifies full public read access for container and blob data.
    // proxys can enumerate blobs within the container via anonymous
    // request, but cannot enumerate containers within the storage account.
    //
    // BLOBS_ONLY:
    // Specifies public read access for blobs. Blob data within this
    // container can be read via anonymous request, but container data is not
    // available. proxys cannot enumerate blobs within the container via
    // anonymous request.
    // If this value is not specified in the request, container data is
    // private to the account owner.
    $createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

    // Set container metadata.
    $createContainerOptions->addMetaData("key1", "value1");
    $createContainerOptions->addMetaData("key2", "value2");

    try {
        // Create container.
        $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Het aanroepen van **setPublicAccess (PublicAccessType::CONTAINER\_en\_BLOB's)** maakt de container en blob-gegevens toegankelijk zijn via anonieme aanvragen. Het aanroepen van **setPublicAccess(PublicAccessType::BLOBS_ONLY)** maakt blob-gegevens toegankelijk via anonieme aanvragen. Zie voor meer informatie over toegangsbeheerlijsten container [Set container ACL (REST API)][container-acl].

Zie voor meer informatie over foutcodes voor service Blob [Blob Service foutcodes][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Gebruik de methode **BlobRestProxy -> createBlockBlob** voor het uploaden van een bestand als een blob. Hiermee maakt u de blob als deze niet bestaat, of het overschreven als dit het geval is. In het onderstaande voorbeeld wordt ervan uitgegaan dat de container al gemaakt is en maakt gebruik van [fopen] [ fopen] het bestand als een stream te openen.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    $content = fopen("c:\myfile.txt", "r");
    $blob_name = "myblob";

    try {
        //Upload blob
        $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Houd er rekening mee dat in het vorige voorbeeld een blob als een stroom uploadt. Echter een blob kan ook worden geüpload als een tekenreeks met bijvoorbeeld de [bestand\_u\_inhoud] [ file_get_contents] functie. U doet dit met behulp van het vorige voorbeeld, wijzigt u `$content = fopen("c:\myfile.txt", "r");` naar `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>Lijst van de BLOB's in een container

U kunt de BLOB's in een container gebruiken de **BlobRestProxy -> listBlobs** methode met een **foreach** lus tot en met het resultaat. De volgende code wordt de naam van elke blob als uitvoer in een container en wordt de URI naar de browser wordt weergegeven.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // List blobs.
        $blob_list = $blobRestProxy->listBlobs("mycontainer");
        $blobs = $blob_list->getBlobs();

        foreach($blobs as $blob)
        {
            echo $blob->getName().": ".$blob->getUrl()."<br />";
        }
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="download-a-blob"></a>Een blob downloaden

Om te downloaden van een blob, de **BlobRestProxy -> getBlob** methode aanroepen en vervolgens de methode **getContentStream** aangeroepen voor het resulterende **GetBlobResult** -object.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Get blob.
        $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
        fpassthru($blob->getContentStream());
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Houd er rekening mee dat in het bovenstaande voorbeeld een blob als een stream resource (de standaardinstelling wordt). U kunt echter de [stroom\_krijgen\_inhoud] [ stream-get-contents] functie geretourneerde stream converteren naar een tekenreeks.

## <a name="delete-a-blob"></a>Een blob verwijderen

Als u wilt verwijderen van een blob, geeft u de naam van de container en de naam van de blob aan **BlobRestProxy -> deleteBlob**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete blob.
        $blobRestProxy->deleteBlob("mycontainer", "myblob");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-a-blob-container"></a>Een blob-container

Ten slotte om een blob-container verwijdert, geeft u de containernaam van de aan **BlobRestProxy -> deleteContainer**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete container.
        $blobRestProxy->deleteContainer("mycontainer");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Volgende stappen

De basisbeginselen van de Azure blob-service kunt u hebt geleerd, klik op deze koppelingen voor meer informatie over complexere opslagtaken.

- Ga naar de [opslag van Azure-teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- Zie het [voorbeeld van de PHP-blok blob](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).
- Zie het [voorbeeld van de PHP pagina blob](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
- [Overdracht van gegevens met het hulpprogramma AzCopy voor opdrachtregel](storage-use-azcopy.md)
 
Zie ook de [PHP Developer Center](/develop/php/)voor meer informatie.


[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents

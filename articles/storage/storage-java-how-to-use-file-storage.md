<properties
    pageTitle="Het gebruik van opslag van Java | Microsoft Azure"
    description="Informatie over het gebruik van de service Azure bestand uploaden, downloaden, lijst en verwijderen van bestanden. Monsters die zijn geschreven in Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-file-storage-from-java"></a>Het gebruik van opslag van Java

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Overzicht

In deze handleiding leert u hoe u eenvoudige bewerkingen uitvoeren op het bestand met Microsoft Azure storage-service. Via de voorbeelden geschreven in Java leert u shares en mappen maken, uploaden, weergeven en verwijderen van bestanden. Als u voor het opslaan van bestanden-service van Microsoft Azure, via de concepten in de volgende secties worden heel handig zijn bij het begrijpen van de monsters.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Een Java-toepassing maken

De monsters bouwen, moet u de Java Development Kit (JDK) en de [Azure opslag SDK voor Java] []. U moet hebt een account Azure opslag ook gemaakt.

## <a name="setup-your-application-to-use-file-storage"></a>Uw toepassing voor het gebruik van de opslag van bestanden instellen

Voor het gebruik van de Azure opslag API's toevoegen met de volgende instructie aan de bovenkant van het Java-bestand waar u toegang wilt krijgen tot de storage-service uit.

    // Include the following imports to use blob APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.file.*;

## <a name="setup-an-azure-storage-connection-string"></a>Een verbindingsreeks Azure opslag Setup

Voor het gebruik van opslag van bestanden, moet u verbinding maken met uw account Azure opslag. De eerste stap zou zijn voor het configureren van een verbindingstekenreeks waarmee we verbinding maken met uw account voor opslag. We definiëren een statische variabele kunt u doen.

    // Configure the connection-string with your values
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account_name;" +
        "AccountKey=your_storage_account_key";

> [AZURE.NOTE] Your_storage_account_name en your_storage_account_key vervangen door de werkelijke waarden voor uw opslag-account.

## <a name="connecting-to-an-azure-storage-account"></a>Verbinding maken met een account met Azure opslag

Voor verbinding met uw account opslag, moet u het object **CloudStorageAccount** een verbindingsreeks doorgeven aan de methode voor **parseren** .

    // Use the CloudStorageAccount object to connect to your storage account
    try {
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
    } catch (InvalidKeyException invalidKey) {
        // Handle the exception
    }

**CloudStorageAccount.parse** genereert een InvalidKeyException en moet u deze in een try/catch-blok geplaatst.

## <a name="how-to-create-a-share"></a>Procedure: een Share maken

Alle bestanden en mappen in de opslag van bestanden bevinden zich in een zogenaamd een **Share**. Uw account opslag kan zoveel aandelen als de capaciteit van de account kan hebben. Om toegang te krijgen tot een share en de inhoud ervan, moet u een bestand opslag-client gebruiken.

    // Create the file storage client.
    CloudFileClient fileClient = storageAccount.createCloudFileClient();

Met de client bestand opslag, kunt u vervolgens verkrijgen een verwijzing naar een share.

    // Get a reference to the file share
    CloudFileShare share = fileClient.getShareReference("sampleshare");

Om daadwerkelijk de share maakt, gebruikt de methode **createIfNotExists** van het CloudFileShare-object.

    if (share.createIfNotExists()) {
        System.out.println("New share created");
    }

Op dit punt bevat **delen** een verwijzing naar een share met de naam **sampleshare**.

## <a name="how-to-upload-a-file"></a>Procedure: een bestand uploaden

Een opslag van Azure bestandsshare bevat in ieder geval een hoofdmap waar bestanden kunnen zich bevinden. In dit gedeelte leert u het uploaden van een bestand van lokale opslag naar de hoofdmap van een share.

De eerste stap bij het uploaden van een bestand is voor een verwijzing naar de map waarin deze zich moet bevinden. U doen dit door het aanroepen van de methode **getRootDirectoryReference** van het object delen.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

Nu dat er een verwijzing naar de hoofdmap van de share, kunt u een bestand op met de volgende code kunt uploaden.

    // Define the path to a local file.
    final String filePath = "C:\\temp\\Readme.txt";

    CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
    cloudFile.uploadFromFile(filePath);

## <a name="how-to-create-a-directory"></a>Procedure: een map maken

Ook kunt u de opslag indelen plaatst bestanden in onderliggende mappen in plaats van ze allemaal in de hoofdmap. De Azure bestand storage-service kunt u zoveel mappen maken als uw account toestaat. De volgende code maakt u een submap met de naam **sampledir** onder de hoofdmap.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the sampledir directory
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    if (sampleDir.createIfNotExists()) {
        System.out.println("sampledir created");
    } else {
        System.out.println("sampledir already exists");
    }

## <a name="how-to-list-files-and-directories-in-a-share"></a>Procedure: bestanden en mappen in een lijst

Het verkrijgen van een lijst met bestanden en mappen binnen een share is gemakkelijk gedaan door het aanroepen van **listFilesAndDirectories** op CloudFileDirectory verwijst. De methode geeft als resultaat een lijst met ListFileItem-objecten die u kunt bladeren op. Als voorbeeld wordt een lijst met de volgende code bestanden en mappen in de hoofdmap.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
        System.out.println(fileItem.getUri());
    }


## <a name="how-to-download-a-file"></a>Procedure: een bestand downloaden

Een van de meer frequente bewerkingen die u tegen opslag van bestanden uitvoert is om bestanden te downloaden. In het volgende voorbeeld wordt de code SampleFile.txt gedownload en wordt de inhoud weergegeven.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the directory that contains the file
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    //Get a reference to the file you want to download
    CloudFile file = sampleDir.getFileReference("SampleFile.txt");

    //Write the contents of the file to the console.
    System.out.println(file.downloadText());

## <a name="how-to-delete-a-file"></a>Procedure: een bestand verwijderen

Een andere algemene opslag bestandsbewerking is verwijderen van bestanden. De volgende code verwijdert een bestand met de naam SampleFile.txt opgeslagen in een map met de naam **sampledir**.


    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory where the file to be deleted is in
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    String filename = "SampleFile.txt"
    CloudFile file;

    file = containerDir.getFileReference(filename)
    if ( file.deleteIfExists() ) {
        System.out.println(filename + " was deleted");
    }


## <a name="how-to-delete-a-directory"></a>Procedure: een map verwijderen

Als u een map is een vrij eenvoudige taak, hoewel moet worden opgemerkt dat u een map nog steeds bestanden bevat of andere mappen niet verwijderen.

    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory you want to delete
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    // Delete the directory
    if ( containerDir.deleteIfExists() ) {
        System.out.println("Directory deleted");
    }


## <a name="how-to-delete-a-share"></a>Procedure: een Share verwijderen

Een share verwijderen wordt uitgevoerd door de **deleteIfExists** -methode aanroept voor een CloudFileShare-object. Hier is voorbeeldcode die dat doet.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the file client.
       CloudFileClient fileClient = storageAccount.createCloudFileClient();

       // Get a reference to the file share
       CloudFileShare share = fileClient.getShareReference("sampleshare");

       if (share.deleteIfExists()) {
           System.out.println("sampleshare deleted");
       }
    } catch (Exception e) {
        e.printStackTrace();
    }

## <a name="next-steps"></a>Volgende stappen

Klik op deze koppelingen als u wilt weten over andere Azure storage API's.

- [Java Developer Center](http://azure.microsoft.com/develop/java/)
- [Azure opslag SDK voor Java](https://github.com/azure/azure-storage-java)
- [Azure opslag SDK voor Android](https://github.com/azure/azure-storage-android)
- [Verwijzing naar Azure opslag Client SDK](http://dl.windowsazure.com/storage/javadoc/)
- [Azure opslagservices REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure opslag-teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- [Overdracht van gegevens met het hulpprogramma AzCopy voor opdrachtregel](storage-use-azcopy.md)

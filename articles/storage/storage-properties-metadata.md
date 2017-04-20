<properties
    pageTitle="Instellen en ophalen van eigenschappen en metagegevens voor de objecten in Azure opslag | Microsoft Azure"
    description="Aangepaste metagegevens worden opgeslagen over objecten in Azure-opslag en instellen en ophalen van Systeemeigenschappen."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="set-and-retrieve-properties-and-metadata"></a>Instellen en ophalen van eigenschappen en metagegevens #

## <a name="overview"></a>Overzicht

Objecten in Azure Storage support Systeemeigenschappen en door de gebruiker gedefinieerde metagegevens, naast de gegevens die ze bevatten:

*   **Systeemeigenschappen.** Systeemeigenschappen aanwezig op elke resource opslag. Ze kunnen lezen of instellen, terwijl anderen alleen-lezen zijn. Achter de corresponderen bepaalde Systeemeigenschappen met bepaalde standaard HTTP-headers. De client-bibliotheek van Azure opslag houdt deze voor u.  

*   **Door de gebruiker gedefinieerde metagegevens.** Door de gebruiker gedefinieerde metagegevens is metagegevens die u voor een bepaalde bron, in de vorm van een naam / waarde-paar opgeeft. U kunt metagegevens opslaan extra waarden aan een resource opslag; Deze waarden zijn voor uw eigen doeleinden alleen en hebben geen invloed op de werking van de bron.  

Ophalen van waarden voor eigenschappen en metagegevens voor een resource opslag is een proces. Voordat u deze waarden lezen kunt, moet u ze expliciet ophalen door het aanroepen van de methode **FetchAttributes** .

> [AZURE.IMPORTANT] Waarden van eigenschappen en metagegevens voor een resource opslag niet tenzij u een van de **FetchAttributes** -methoden aanroept. 

## <a name="setting-and-retrieving-properties"></a>Instellen en ophalen van eigenschappen

Om eigenschapswaarden te vinden, roept u de methode **FetchAttributes** van de blob of een container voor het vullen van de eigenschappen en vervolgens de waarden.

Eigenschappen worden ingesteld op een object, geven de waarde van de eigenschap en vervolgens roept u de methode **SetProperties** .

In het volgende voorbeeld maakt u een container en sommige van de waarden van de eigenschap schrijft naar een consolevenster:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
    //Create the service client object for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container. 
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it does not already exist.
    container.CreateIfNotExists();

    // Fetch container properties and write out their values.
    container.FetchAttributes();
    Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
    Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
    Console.WriteLine("ETag: {0}", container.Properties.ETag);
    Console.WriteLine();

## <a name="setting-and-retrieving-metadata"></a>Instellen en ophalen van metagegevens

U kunt metagegevens opgeven als een of meer naam / waarde-paren op een bron blob of container. Naam / waarde-paren toevoegen aan de collectie **metagegevens** op de resource metagegevens stelt vervolgens roept de methode **SetMetadata** om het opslaan van de waarden in de service.

> [AZURE.NOTE] De naam van de metagegevens moet voldoen aan de naamgevingsconventies voor C#-id.
 
In het volgende voorbeeld stelt metagegevens in een container. Een waarde wordt ingesteld met de methode **Add** van de collectie. De andere waarde is ingesteld met de syntaxis voor impliciete sleutelwaarde. Beide zijn geldig.

    public static void AddContainerMetadata(CloudBlobContainer container)
    {
        //Add some metadata to the container.
        container.Metadata.Add("docType", "textDocuments");
        container.Metadata["category"] = "guidance";

        //Set the container's metadata.
        container.SetMetadata();
    }

Als u metagegevens wilt ophalen, roept u de methode **FetchAttributes** van de blob of container voor het vullen van de collectie **metagegevens** leest u de waarden, zoals in het volgende voorbeeld wordt getoond.

    public static void ListContainerMetadata(CloudBlobContainer container)
    {
        //Fetch container attributes in order to populate the container's properties and metadata.
        container.FetchAttributes();

        //Enumerate the container's metadata.
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in container.Metadata)
        {
            Console.WriteLine("\tKey: {0}", metadataItem.Key);
            Console.WriteLine("\tValue: {0}", metadataItem.Value);
        }
    }

## <a name="see-also"></a>Zie ook  

- [Client-bibliotheek van Azure opslag voor referentie .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
- [Azure clientbibliotheek van opslag voor .NET pakket](https://www.nuget.org/packages/WindowsAzure.Storage/) 

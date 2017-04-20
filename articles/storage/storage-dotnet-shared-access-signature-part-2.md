<properties
    pageTitle="Maken en gebruiken van een SAS met Blob-opslag | Microsoft Azure"
    description="Deze zelfstudie toont u gedeelde toegang handtekeningen voor gebruik met Blob-opslag maken en hoe ze uw clienttoepassingen verbruiken."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Gedeelde toegang handtekeningen, deel 2: Maken en gebruiken van een SAS met Blob-opslag

## <a name="overview"></a>Overzicht

[Deel 1](storage-dotnet-shared-access-signature-part-1.md) van deze zelfstudie verkend gedeeld access handtekeningen (SAS) en aanbevolen procedures voor het gebruik ervan beschreven. Deel 2 ziet u hoe te genereren en vervolgens gedeelde toegang handtekeningen te gebruiken met Blob-opslag. De voorbeelden zijn geschreven in C# en gebruikt de clientbibliotheek Azure opslag voor .NET. De scenario's waarop bevatten deze aspecten van het werken met gedeelde toegang handtekeningen:

- Een handtekening voor gedeelde toegang in een container te genereren
- Een handtekening voor gedeelde toegang in een blob te genereren
- Een opgeslagen-beleid voor het beheren van handtekeningen op de middelen van de container maken
- De handtekeningen van het gedeeld gebruik van een clienttoepassing testen

## <a name="about-this-tutorial"></a>Over deze handleiding

In deze zelfstudie, zullen we richten ons op gedeelde toegang handtekeningen voor containers en BLOB's maken door twee consoletoepassingen te maken. De eerste consoletoepassing genereert handtekeningen voor gedeelde toegang in een container en op een blob. Deze toepassing heeft kennis van de sleutels opslag account. De tweede consoletoepassing die als een clienttoepassing fungeert, wordt de container en blob resources met behulp van de gedeelde toegang handtekeningen die zijn gemaakt met de eerste toepassing. Deze toepassing gebruikt de handtekeningen van gedeelde toegang alleen voor de verificatie van de toegang tot de container en resources blob - heeft geen kennis van de sleutels van de account.

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a>Deel 1: Maak een consoletoepassing voor het genereren van gedeelde toegang handtekeningen

Eerst, zorg ervoor dat de clientbibliotheek Azure opslag voor .NET is geïnstalleerd. U kunt het [pakket NuGet](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet-pakket") met de meest recente assembly's voor de clientbibliotheek installeren Dit is de aanbevolen methode om ervoor te zorgen dat u beschikt over de meest recente oplossingen. U kunt ook downloaden van de clientbibliotheek als onderdeel van de meest recente versie van de [SDK voor .NET Azure](https://azure.microsoft.com/downloads/).

Maak een nieuwe consoletoepassing voor Windows en geef deze de naam **GenerateSharedAccessSignatures**in Visual Studio. Verwijzingen naar **Microsoft.WindowsAzure.Configuration.dll** en **Microsoft.WindowsAzure.Storage.dll**, met een van de volgende manieren toevoegen:

-   Als u installeren van het pakket NuGet wilt, moet u eerst de [NuGet Client](https://docs.nuget.org/consume/installing-nuget)installeren. Selecteer **-Project in Visual Studio | NuGet pakketten beheren**online zoeken naar **Azure opslag**en volg de instructies voor het installeren.
-   U kunt ook de assembly's in de installatie van de SDK Azure en verwijzingen naar deze toevoegen.

Toevoegen aan de bovenkant van het bestand Program.cs de volgende instructies voor **Gebruik** :

    using System.IO;    
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

Het app.config bestand bewerken zodat deze een configuratie-instelling met een verbindingsreeks die naar uw account storage verwijst bevat. Het bestand app.config ziet lijkt op deze:

    <configuration>
      <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
      </startup>
      <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
      </appSettings>
    </configuration>

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>Een handtekening voor gedeelde toegang URI voor een Container genereren

Allereerst gaan we een methode voor het genereren van een handtekening voor gedeelde toegang in een nieuwe container toevoegen. In dit geval de handtekening is gekoppeld aan een opgeslagen-beleid, zodat zij de informatie die de machtigingen die het verleent en de verlooptijd aangeeft uitoefent op de URI.

Eerst moet u code toevoegen aan de methode **Main()** voor verificatie van toegang tot uw account opslag en maakt een nieuwe container:

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Insert calls to the methods created below here...

        //Require user input before closing the console window.
        Console.ReadLine();
    }

Vervolgens voegt u een nieuwe methode die de handtekening gedeelde toegang voor de container wordt gegenereerd en retourneert de URI van de handtekening:

    static string GetContainerSasUri(CloudBlobContainer container)
    {
        //Set the expiry time and permissions for the container.
        //In this case no start time is specified, so the shared access signature becomes valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List;

        //Generate the shared access signature on the container, setting the constraints directly on the signature.
        string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Voeg de volgende regels op de onderkant van de methode **Main()** voor de aanroep naar **Console.ReadLine()** **GetContainerSasUri()** aanroepen en de URI van de handtekening schrijven naar het consolevenster:

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

Compileren en uitvoeren om de uitvoer van de gedeelde toegang handtekening URI voor de nieuwe container. De URI is vergelijkbaar met de volgende URI:       

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D

Als u de code hebt uitgevoerd, zijn de handtekening gedeelde toegang dat u hebt gemaakt in de container zijn geldig gedurende de komende 24 uur. De handtekening verleent een client machtigingen voor lijst BLOB's in de container en een nieuwe blob schrijven naar de container.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>Een handtekening voor gedeelde toegang URI voor een Blob genereren

Vervolgens zult we vergelijkbare code voor het maken van een nieuwe blob in de container en genereren een gedeelde access-handtekening voor het schrijven. Deze handtekening gedeelde toegang is niet gekoppeld aan een opgeslagen-beleid, zodat de begintijd, verlooptijd en machtigingsgegevens op de URI bevat.

Een nieuwe methode waarmee een nieuwe blob toevoegen en tekst, vervolgens genereert een gedeelde access-handtekening en geeft als resultaat de URI van de handtekening schrijven:

    static string GetBlobSasUri(CloudBlobContainer container)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a Shared Access Signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Set the expiry time and permissions for the blob.
        //In this case the start time is specified as a few minutes in the past, to mitigate clock skew.
        //The shared access signature will be valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

        //Generate the shared access signature on the blob, setting the constraints directly on the signature.
        string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

Voeg de volgende regels om te **GetBlobSasUri()**, roepen voor de aanroep van **Console.ReadLine()**, aan de onderkant van de methode **Main()** en de handtekening van de gedeelde toegang URI schrijven naar het consolevenster:    

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();


Compileren en uitvoeren om de gedeelde toegang handtekening URI voor de nieuwe blob-uitvoer. De URI is vergelijkbaar met de volgende URI:

    https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D

### <a name="create-a-stored-access-policy-on-the-container"></a>Een opgeslagen-beleid maken voor de Container

Nu gaan we een opgeslagen-beleid op de container waarin de beperkingen voor alle gedeelde toegang handtekeningen die gekoppeld aan het zijn maken.

In de voorgaande voorbeelden we opgegeven de begintijd (impliciet of expliciet), de verlooptijd en de machtigingen voor de gedeelde toegang handtekening URI zelf. In de volgende voorbeelden zullen we deze op het toegangsbeleid opgeslagen en niet op de gedeelde toegang handtekening opgeven. Hierdoor kan wij deze beperkingen wijzigen zonder de handtekening van de gedeelde toegang opnieuw uitgeven.

Het is mogelijk dat een of meer van de beperkingen op de gedeelde toegang handtekening en de rest van het opgeslagen-beleid. U kunt echter alleen opgeven de begintijd, verlooptijd en machtigingen in één plaats of andere; u kan bijvoorbeeld machtigingen opgeven voor de handtekening van de gedeelde toegang en ook opgeven op het opgeslagen-beleid.

Houd er rekening mee dat wanneer u een access-beleid aan een container toevoegen, u moet u bestaande machtigingen van de container, het nieuwe-beleid toevoegen en stel vervolgens de machtigingen van de container.

Voeg een nieuwe methode die maakt u een nieuwe opgeslagen-beleid op een container en geeft als resultaat de naam van het beleid:

    static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
        string policyName)
    {
        //Create a new shared access policy and define its constraints.
        SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
        };

        //Get the container's existing permissions.
        BlobContainerPermissions permissions = container.GetPermissions();

        //Add the new policy to the container's permissions, and set the container's permissions.
        permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
        container.SetPermissions(permissions);
    }

De volgende regels toevoegen aan eerste wissen alle bestaande-beleid op de onderkant van de methode **Main()** voor de aanroep van **Console.ReadLine()**, en vervolgens de methode **CreateSharedAccessPolicy()** aanroepen:    

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

Houd er rekening mee dat als u het-beleid op een container wist, moet u eerst bestaande van de container machtigingen, en schakel de machtigingen uit en vervolgens de machtigingen opnieuw instellen.

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a>Een handtekening voor gedeelde toegang URI op de Container met een toegangsbeleid genereren

Vervolgens maken we een andere handtekening voor gedeelde toegang op de container die we eerder, maar deze keer die zullen we de handtekening koppelen aan het toegangsbeleid die in het vorige voorbeeld is gemaakt.

Voeg een nieuwe methode voor het genereren van een andere handtekening voor gedeelde toegang in de container:

    static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Generate the shared access signature on the container. In this case, all of the constraints for the
        //shared access signature are specified on the stored access policy.
        string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Onder aan de methode **Main()** voor de aanroep van **Console.ReadLine()**, voeg de volgende regels om de methode **GetContainerSasUriWithPolicy** aanroepen:

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a>Een handtekening voor gedeelde toegang URI op de Blob die gebruikmaakt van een toegangsbeleid genereren

Tot slot gaan we een soortgelijke methode voor het maken van een andere blob en genereren een gedeelde access-handtekening die is gekoppeld aan een-beleid toevoegen.

Een nieuwe methode voor het maken van een blob en genereren een gedeelde access-handtekening toevoegen:

    static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a shared access signature. " +
        "A stored access policy defines the constraints for the signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Generate the shared access signature on the blob.
        string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

Onder aan de methode **Main()** voor de aanroep van **Console.ReadLine()**, voeg de volgende regels om de methode **GetBlobSasUriWithPolicy** aanroepen:    

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

De methode **Main()** ziet er nu als volgt in zijn geheel. Het naar de gedeelde toegang handtekening URI's schrijven naar het consolevenster, en vervolgens kopieert en plakt in een tekstbestand voor gebruik in het tweede deel van deze zelfstudie uitvoeren.

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Generate a SAS URI for the container, without a stored access policy.
        Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, without a stored access policy.
        Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
        Console.WriteLine();

        //Clear any existing access policies on container.
        BlobContainerPermissions perms = container.GetPermissions();
        perms.SharedAccessPolicies.Clear();
        container.SetPermissions(perms);

        //Create a new access policy on the container, which may be optionally used to provide constraints for
        //shared access signatures on the container and the blob.
        string sharedAccessPolicyName = "tutorialpolicy";
        CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

        //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        Console.ReadLine();
    }

Wanneer u de toepassing GenerateSharedAccessSignatures console uitvoert, ziet u uitvoer die vergelijkbaar is met de volgende in het consolevenster. Dit zijn de gedeelde toegang handtekeningen waarmee u in deel 2 van de zelfstudie.

![SAS-console-uitvoer-1][sas-console-output-1]

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a>Deel 2: Maak een consoletoepassing voor het testen van de handtekeningen van gedeelde toegang

Als u wilt testen in de gedeelde toegang handtekeningen die zijn gemaakt in de voorgaande voorbeelden, maken we een tweede consoletoepassing die gebruikmaakt van de handtekeningen bewerkingen uit te voeren op de recipiënt en op een blob.

> [AZURE.NOTE] Als u meer dan 24 uur zijn verstreken sinds u het eerste gedeelte van de zelfstudie hebt voltooid, is de handtekeningen die u hebt gegenereerd niet langer geldig. In dit geval moet u de code uitvoeren in de eerste consoletoepassing voor het genereren van gedeelde toegang vers handtekeningen voor gebruik in het tweede deel van de zelfstudie.

Maak een nieuwe consoletoepassing voor Windows en geef deze de naam **ConsumeSharedAccessSignatures**in Visual Studio. Verwijzingen toevoegen aan **Microsoft.WindowsAzure.Configuration.dll** en **Microsoft.WindowsAzure.Storage.dll**, zoals u eerder hebt gedaan.

Toevoegen aan de bovenkant van het bestand Program.cs de volgende instructies voor **Gebruik** :

    using System.IO;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

In de hoofdtekst van de methode **Main()** toevoegen van de volgende constanten en hun waarden bijwerken tot de gedeelde toegang handtekeningen die u hebt gegenereerd in deel 1 van de zelfstudie.

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
    }

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a>Een methode om te proberen de Container bewerkingen met een gedeelde Access-handtekening toevoegen

Vervolgens gaan we een methode waarmee bepaalde representatieve container bewerkingen met een handtekening voor gedeelde toegang in de container wordt getest toevoegen. Houd er rekening mee dat de handtekening van de gedeelde toegang wordt gebruikt om een verwijzing naar de container, toegang tot de container op basis van de handtekening alleen verifiëren.

De volgende methode toevoegen aan Program.cs:

    static void UseContainerSAS(string sas)
    {
        //Try performing container operations with the SAS provided.

        //Return a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

        //Create a list to store blob URIs returned by a listing operation on the container.
        List<ICloudBlob> blobList = new List<ICloudBlob>();

        //Write operation: write a new blob to the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
            string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //List operation: List the blobs in the container.
        try
        {
            foreach (ICloudBlob blob in container.ListBlobs())
            {
                blobList.Add(blob);
            }
            Console.WriteLine("List operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("List operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Get a reference to one of the blobs in the container and read it.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            MemoryStream msRead = new MemoryStream();
            msRead.Position = 0;
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                Console.WriteLine(msRead.Length);
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        Console.WriteLine();

        //Delete operation: Delete a blob in the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Update voor de methode **Main()** **UseContainerSAS()** met beide van de handtekeningen van gedeelde toegang dat u hebt gemaakt in de container aanroepen:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        Console.ReadLine();
    }


### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a>Een methode om te proberen de Blob bewerkingen met een gedeelde Access-handtekening toevoegen

Tot slot gaan we een methode die bepaalde representatieve blob bewerkingen met een handtekening voor gedeelde toegang in de blob-tests toevoegen. In dit geval gebruiken we de constructor **CloudBlockBlob(String)**, in de handtekening gedeelde toegang geven als resultaat een verwijzing naar de blob. Geen andere verificatie is vereist. het gebaseerd op de handtekening alleen.

De volgende methode toevoegen aan Program.cs:

    static void UseBlobSAS(string sas)
    {
        //Try performing blob operations using the SAS provided.

        //Return a reference to the blob using the SAS URI.
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

        //Write operation: Write a new blob to the container.
        try
        {
            string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Read the contents of the blob.
        try
        {
            MemoryStream msRead = new MemoryStream();
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                msRead.Position = 0;
                using (StreamReader reader = new StreamReader(msRead, true))
                {
                    string line;
                    while ((line = reader.ReadLine()) != null)
                    {
                        Console.WriteLine(line);
                    }
                }
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Delete operation: Delete the blob.
        try
        {
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


De methode **Main()** **UseBlobSAS()** aanroepen met beide van de handtekeningen van gedeelde toegang dat u hebt gemaakt in de blob bijwerken:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
        UseBlobSAS(blobSAS);
        UseBlobSAS(blobSASWithAccessPolicy);

        Console.ReadLine();
    }

Voer de consoletoepassing en bekijk de uitvoer om te zien welke bewerkingen zijn toegestaan voor welke handtekeningen. De uitvoer in het consolevenster ziet er ongeveer als volgt:

![SAS-console-uitvoer-2][sas-console-output-2]

## <a name="next-steps"></a>Volgende stappen

[Gedeelde toegang handtekeningen, deel 1: Wat is het SAS-Model?](storage-dotnet-shared-access-signature-part-1.md)

[Anonieme leestoegang tot containers en BLOB's beheren](storage-manage-access-to-resources.md)

[Delegeren van toegang met een gedeelde Access-handtekening (REST API)](http://msdn.microsoft.com/library/azure/ee395415.aspx)

[Introductie van tabel- en SAS-wachtrij](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)

[sas-console-output-1]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-1.PNG
[sas-console-output-2]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-2.PNG

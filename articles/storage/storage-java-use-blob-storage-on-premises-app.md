<properties
    pageTitle="Toepassing met blob-opslag (Java) ruimten | Microsoft Azure"
    description="Informatie over het maken van een consoletoepassing die een afbeelding uploadt naar Azure en de afbeelding vervolgens weergegeven in de browser. Voorbeelden van code in Java."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="on-premises-application-with-blob-storage"></a>Toepassing op gebouwen met blob-opslag

## <a name="overview"></a>Overzicht

In het volgende voorbeeld ziet u hoe u afbeeldingen op te slaan in Azure Azure opslag kunt gebruiken. De code in dit artikel wordt voor de consoletoepassing van een die een afbeelding uploadt naar Azure en maakt vervolgens een HTML-bestand dat de afbeelding wordt weergegeven in uw browser.

## <a name="prerequisites"></a>Vereisten

- Een Java Developer Kit (JDK), versie 1.6 of hoger is geïnstalleerd.
- De Azure SDK is geïnstalleerd.
- Het oppervlak voor de Azure Libraries voor Java en alle potten afhankelijkheid van toepassing zijn geïnstalleerd en worden in het bouwpad gebruikt door de Java-compiler. Zie [de Azure SDK voor Java downloaden](java-download-azure-sdk.md)voor meer informatie over het installeren van de bibliotheken Azure voor Java.
- Een account Azure opslag zijn ingesteld. De naam en de sleutel van de account voor de opslag wordt gebruikt door de code in dit artikel. Zie [een opslag-Account maken](storage-create-storage-account.md#create-a-storage-account) voor informatie over het maken van een account voor opslag en [opslag-toegangstoetsen weergeven en kopiëren](storage-create-storage-account.md#view-and-copy-storage-access-keys) voor meer informatie over het ophalen van de sleutel van de account.

- U hebt een lokaal bestand met de naam gemaakt die wordt opgeslagen in het pad c:\\myimages\\image1.jpg. Ook de constructor   **FileInputStream** in het voorbeeld gebruik van een andere afbeelding pad en de naam wijzigen.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a>Azure blobopslag gebruiken om een bestand te uploaden

Een stapsgewijze procedure wordt hier weergegeven. Als u overslaan wilt, wordt de hele code verderop in dit artikel weergegeven.

Eerst de code met inbegrip van invoer voor Azure kernklassen opslag, de klassen Azure blob-client, de i/o Java-klassen en de klasse **URISyntaxException** .

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

Declareren van een klasse met de naam **StorageSample**en de vierkante haak openen, **{**bevatten.

    public class StorageSample {

In de klasse **StorageSample** declareert een variabele met het standaardprotocol voor het eindpunt en de naam van uw opslag de toegangstoets opslag, zoals opgegeven in uw account Azure opslag. Vervang de tijdelijke aanduiding waarden **uw\_account\_naam** en **uw\_account\_sleutel** rekening met uw eigen naam en account sleutel, respectievelijk.

    public static final String storageConnectionString =
           "DefaultEndpointsProtocol=http;" +
               "AccountName=your_account_name;" +
               "AccountKey=your_account_name";

Toevoegen in uw aangifte voor de **belangrijkste**en nemen de nodige open haakjes **{**bevatten een **try** -blok.

    public static void main(String[] args)
    {
        try
        {

Declareer variabelen van het volgende type (de beschrijvingen staan voor hoe ze worden gebruikt in dit voorbeeld):

-   **CloudStorageAccount**: gebruikt voor het initialiseren van het accountobject met uw accountnaam Azure opslag en de sleutel en het blob client-object te maken.
-   **CloudBlobClient**: gebruikt voor toegang tot de service blob.
-   **CloudBlobContainer**: gebruikt voor het maken van een blob-container, weergeven van de BLOB's in de container en de container verwijderen.
-   **CloudBlockBlob**: voor het uploaden van een lokaal bestand naar de container.

<!-- -->

    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;

Een waarde toewijzen aan de variabele **account** .

    account = CloudStorageAccount.parse(storageConnectionString);

Een waarde toewijzen aan de variabele **serviceClient** .

    serviceClient = account.createCloudBlobClient();

Een waarde toewijzen aan de variabele **container** . Wij geven je een verwijzing naar een container met de naam **gettingstarted**.

    // Container name must be lower case.
    container = serviceClient.getContainerReference("gettingstarted");

Maak de container. Deze methode maakt de container als deze niet bestaan (resultaat **true**). Als de container bestaat, wordt het resultaat **false**. Een alternatief voor **createIfNotExists** is de methode **create** (hetgeen resulteert in een fout als de container al bestaat).

    container.createIfNotExists();

Anonieme toegang voor de container instellen.

    // Set anonymous access on the container.
    BlobContainerPermissions containerPermissions;
    containerPermissions = new BlobContainerPermissions();
    containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
    container.uploadPermissions(containerPermissions);

U verwijst naar de blob blok, die staat voor de blob in Azure opslag.

    blob = container.getBlockBlobReference("image1.jpg");

Gebruik de constructor **bestand** een verwijzing naar het lokale bestand dat u uploadt. Zorg ervoor dat u dit bestand hebt gemaakt voordat u de code uitvoert.

    File fileReference = new File ("c:\\myimages\\image1.jpg");

Het lokale bestand uploaden via een aanroep van de methode **CloudBlockBlob.upload** . De eerste parameter aan de methode **CloudBlockBlob.upload** wordt een **FileInputStream** -object dat staat voor het lokale bestand die worden geüpload naar Azure opslag. De tweede parameter is de grootte in bytes van het bestand.

    blob.upload(new FileInputStream(fileReference), fileReference.length());

Een helper functie met de naam **MakeHTMLPage**te maken van een eenvoudige HTML-pagina met een ** &lt;afbeelding&gt; ** -element met de bron wordt ingesteld op de blob is nu in uw account Azure opslag. De code voor de **MakeHTMLPage** wordt verderop in dit artikel worden besproken.

    MakeHTMLPage(container);

Een statusbericht en informatie over de gemaakte HTML-pagina afdrukken.

    System.out.println("Processing complete.");
    System.out.println("Open index.html to see the images stored in your storage account.");

**Try** -blok sluiten door het invoegen van een vierkante haak sluiten: **}**

Ingang van de volgende uitzonderingen:

-   **FileNotFoundException**: door de constructors **FileInputStream** of **FileOutputStream** kan worden gegenereerd.
-   **StorageException**: door de Azure clientbibliotheek opslag kan worden gegenereerd.
-   **URISyntaxException**: kan worden gegenereerd door de methode **ListBlobItem.getUri** .
-   **Uitzondering**: algemene uitzondering verwerken.

<!-- -->

    catch (FileNotFoundException fileNotFoundException)
    {
        System.out.print("FileNotFoundException encountered: ");
        System.out.println(fileNotFoundException.getMessage());
        System.exit(-1);
    }
    catch (StorageException storageException)
    {
        System.out.print("StorageException encountered: ");
        System.out.println(storageException.getMessage());
        System.exit(-1);
    }
    catch (URISyntaxException uriSyntaxException)
    {
        System.out.print("URISyntaxException encountered: ");
        System.out.println(uriSyntaxException.getMessage());
        System.exit(-1);
    }
    catch (Exception e)
    {
        System.out.print("Exception encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

**Belangrijkste** sluiten door het invoegen van een vierkante haak sluiten: **}**

Maak een **MakeHTMLPage** voor het maken van een eenvoudige HTML-pagina met de naam. Deze methode heeft een parameter van het type **CloudBlobContainer**, die wordt gebruikt om de lijst met geüploade BLOB's doorlopen. Deze methode genereert uitzonderingen van het type **FileNotFoundException**, die kan worden gegenereerd door de constructor **FileOutputStream** en **URISyntaxException**, die door de methode **ListBlobItem.getUri** kan worden gegenereerd. Het haakje openen plaatst, **{**opnemen.

    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {

Maak een lokaal bestand met de naam **index.html**.

    PrintStream stream;
    stream = new PrintStream(new FileOutputStream("index.html"));

Schrijven naar het lokale bestand toe te voegen in de ** &lt;html&gt;**, ** &lt;kop&gt;**, en ** &lt;instantie&gt; ** elementen.

    stream.println("<html>");
    stream.println("<header/>");
    stream.println("<body>");

De lijst met geüploade BLOB's doorlopen. Voor elke blob, in de HTML-pagina maakt u een ** &lt;img&gt; ** -element met het kenmerk **src** naar de URI van de blob verzonden wanneer deze in uw account Azure opslag bestaat.
Hoewel u slechts één afbeelding in dit voorbeeld wordt toegevoegd als u meer toegevoegd, zou deze code al deze sequentieel.

Voor eenvoud, in dit voorbeeld wordt ervan uitgegaan dat iedere geüploade blob is een afbeelding. Als u BLOB's dan afbeeldingen of pagina BLOB's in plaats van een blok BLOB's hebt bijgewerkt, de code naar wens aanpassen.

    // Enumerate the uploaded blobs.
    for (ListBlobItem blobItem : container.listBlobs()) {
    // List each blob as an <img> element in the HTML body.
    stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
    }

Sluiten de ** &lt;instantie&gt; ** element en de ** &lt;html&gt; ** element.

    stream.println("</body>");
    stream.println("</html>");

Het lokale bestand sluiten.

    stream.close();

**MakeHTMLPage** door het invoegen van een vierkante haak sluiten sluiten: **}**

**StorageSample** door het invoegen van een vierkante haak sluiten sluiten: **}**

Hier volgt de volledige code voor dit voorbeeld. Vergeet niet om de waarden van de tijdelijke aanduiding wijzigen **uw\_account\_naam** en **uw\_account\_sleutel** respectievelijk uw accountnaam en account-sleutel gebruiken.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

    // Create an image, c:\myimages\image1.jpg, prior to running this sample.
    // Alternatively, change the value used by the FileInputStream constructor
    // to use a different image path and file that you have already created.
    public class StorageSample {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                       "AccountName=your_account_name;" +
                       "AccountKey=your_account_name";

        public static void main(String[] args) {
            try {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;
                CloudBlockBlob blob;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.createIfNotExists();

                // Set anonymous access on the container.
                BlobContainerPermissions containerPermissions;
                containerPermissions = new BlobContainerPermissions();
                containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
                container.uploadPermissions(containerPermissions);

                // Upload an image file.
                blob = container.getBlockBlobReference("image1.jpg");

                File fileReference = new File("c:\\myimages\\image1.jpg");
                blob.upload(new FileInputStream(fileReference), fileReference.length());

                // At this point the image is uploaded.
                // Next, create an HTML page that lists all of the uploaded images.
                MakeHTMLPage(container);

                System.out.println("Processing complete.");
                System.out.println("Open index.html to see the images stored in your storage account.");

            } catch (FileNotFoundException fileNotFoundException) {
                System.out.print("FileNotFoundException encountered: ");
                System.out.println(fileNotFoundException.getMessage());
                System.exit(-1);
            } catch (StorageException storageException) {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            } catch (URISyntaxException uriSyntaxException) {
                System.out.print("URISyntaxException encountered: ");
                System.out.println(uriSyntaxException.getMessage());
                System.exit(-1);
            } catch (Exception e) {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }

        // Create an HTML page that can be used to display the uploaded images.
        // This example assumes all of the blobs are for images.
        public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
        {
            PrintStream stream;
            stream = new PrintStream(new FileOutputStream("index.html"));

            // Create the opening <html>, <header>, and <body> elements.
            stream.println("<html>");
            stream.println("<header/>");
            stream.println("<body>");

            // Enumerate the uploaded blobs.
            for (ListBlobItem blobItem : container.listBlobs()) {
                // List each blob as an <img> element in the HTML body.
                stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
            }

            stream.println("</body>");

            // Complete the <html> element and close the file.
            stream.println("</html>");
            stream.close();
        }
    }

Naast uw lokale image uploaden naar Azure opslag, maakt de voorbeeldcode een lokaal bestand namedindex.html, dat u kunt openen in uw browser om te zien van uw geüploade afbeelding.

Omdat de code uw accountnaam en sleutel rekening bevat, ervoor te zorgen dat de broncode beveiligd is.

## <a name="to-delete-a-container"></a>Een container

Omdat u in rekening worden gebracht voor de opslag, kunt u de container **gettingstarted** verwijderen nadat u klaar bent met experimenteren met dit voorbeeld. Als u wilt verwijderen van een container, de **CloudBlobContainer.delete** -methode te gebruiken.

    container = serviceClient.getContainerReference("gettingstarted");
    container.delete();

Aanroepen van de methode **CloudBlobContainer.delete** , is het proces van **CloudStorageAccount**, **ClodBlobClient**en **CloudBlobContainer** objecten initialiseren hetzelfde als voor de methode **createIfNotExist** wordt weergegeven. Het volgende is een volledig voorbeeld dat de container met de naam **gettingstarted**wordt verwijderd.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;

    public class DeleteContainer {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                   "AccountName=your_account_name;" +
                   "AccountKey=your_account_key";

        public static void main(String[] args)
        {
            try
            {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.delete();

                System.out.println("Container deleted.");

            }
            catch (StorageException storageException)
            {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }
    }

Zie voor een overzicht van andere blob storage klassen en methoden, [hoe u Blob-opslag van Java](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Volgende stappen

Klik op deze koppelingen voor meer informatie over complexere opslagtaken.

- [Azure opslag SDK voor Java](https://github.com/azure/azure-storage-java)
- [Verwijzing naar Azure opslag Client SDK](http://dl.windowsazure.com/storage/javadoc/)
- [Azure opslagservices REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure opslag-teamblog](http://blogs.msdn.com/b/windowsazurestorage/)

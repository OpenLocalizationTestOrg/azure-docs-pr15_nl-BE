<properties
    pageTitle="Aan de slag met blob storage en Visual Studio verbonden services (WebJob projecten) | Microsoft Azure"
    description="Hoe aan de slag met Blob-opslag in een WebJob project nadat services verbinding maken met een Azure opslag met behulp van Visual Studio worden verbonden."
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Aan de slag met Azure Blob storage en Visual Studio verbonden services (WebJob projecten)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

Dit artikel bevat een C# codevoorbeelden die hoe een proces wordt geactiveerd tonen wanneer een Azure blob wordt gemaakt of bijgewerkt. De codevoorbeelden gebruiken de [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) versie 1.x. Wanneer u een opslag-account aan een project van WebJob toevoegen met behulp van het dialoogvenster Visual Studio **Verbonden Services toevoegen** , de juiste Azure opslag NuGet-pakket is geïnstalleerd, worden de juiste referenties voor .NET toegevoegd aan het project en verbindingsreeksen voor de opslag in het App.config bestand worden bijgewerkt.



## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a>Hoe een functie wordt geactiveerd wanneer een blob wordt gemaakt of bijgewerkt

In dit gedeelte ziet u hoe u het kenmerk **BlobTrigger** .

 **Opmerking:** De WebJobs SDK scant de logboekbestanden voor nieuwe of gewijzigde BLOB's bekijken. Dit proces is per definitie langzaam; een functie kan niet ophalen geactiveerd tot enkele minuten of meer nadat de blob is gemaakt.  Als uw toepassing BLOB's onmiddellijk te verwerken moet, is de aanbevolen methode om een bericht wachtrij maken wanneer u de blob maken en gebruiken van het kenmerk [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) in plaats van het kenmerk **BlobTrigger** van de functie die de blob verwerkt.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Één tijdelijke aanduiding voor de naam met de extensie blob  

In het volgende voorbeeld wordt tekst BLOB's die worden weergegeven in de container de container *uitvoer* *invoer* gekopieerd:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

De constructor kenmerk tekenreeksparameter een waarmee de containernaam van de en een tijdelijke aanduiding voor de naam van de blob. Als een blob met de naam *Blob1.txt* in de container *input gemaakt* , maakt de functie in dit voorbeeld een blob met de naam *Blob1.txt* in de container voor de *uitvoer* .

U kunt een naampatroon met tijdelijke aanduiding voor blob opgeven zoals in het volgende voorbeeld:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Deze code alleen blobs die hebben namen die beginnen met 'oorspronkelijke--' gekopieerd. *Origineel Blob1.txt* in de container *invoer* is bijvoorbeeld gekopieerd naar *Kopie-Blob1.txt* in de container voor de *uitvoer* .

Als u een naampatroon opgeven voor blob namen met accolades in de naam, dubbelklikt u de accolades. Bijvoorbeeld, als u wilt zoeken naar BLOB's in de container voor *afbeeldingen* met namen als volgt:

        {20140101}-soundfile.mp3

Gebruik deze optie voor het patroon:

        images/{{20140101}}-{name}

In het voorbeeld is de waarde van de tijdelijke aanduiding voor *de naam* *soundfile.mp3*.

### <a name="separate-blob-name-and-extension-placeholders"></a>Naam en de extensie afzonderlijk blob-tijdelijke aanduidingen

In het volgende voorbeeld verandert de bestandsextensie als BLOB's die worden weergegeven in de container *invoer* aan de container *uitvoer* worden gekopieerd. De code registreert de uitbreiding van de blob *invoer* en de uitbreiding van de blob *uitvoer* ingesteld op *.txt*.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a name="types-that-you-can-bind-to-blobs"></a>Typen die u aan BLOB's binden kunt

U kunt het kenmerk **BlobTrigger** van de volgende typen:

* **tekenreeks**
* **TextReader**
* **Stream**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Andere typen gedeserialiseerd door [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)

Als u werken rechtstreeks met de Azure opslag-account wilt, kunt u ook een parameter **CloudStorageAccount** toevoegen aan de handtekening van de methode.

## <a name="getting-text-blob-content-by-binding-to-string"></a>Inhoud van de tekst blob binding met de tekenreeks ophalen

Als de tekst BLOB's worden verwacht, kan de **BlobTrigger** worden toegepast op **een tekenreeksparameter** . In het volgende voorbeeld wordt een blob tekst aan een parameter **string** met de naam **logMessage**. De parameter wordt gebruikt om de inhoud van de blob schrijven naar het dashboard WebJobs SDK.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Blob inhoud ophalen worden geserialiseerd met behulp van ICloudBlobStreamBinder

In het volgende voorbeeld wordt een klasse die **ICloudBlobStreamBinder** als u het kenmerk **BlobTrigger** een blob binden aan het type **WebImage** wordt geïmplementeerd.

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK",
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

De code **WebImage** binding wordt geleverd in een **WebImageBinder** klasse die is afgeleid van **ICloudBlobStreamBinder**.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input,
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="how-to-handle-poison-blobs"></a>Het afhandelen van poison BLOB 's

Wanneer een functie **BlobTrigger** mislukt, roept de SDK deze opnieuw, in het geval de fout is veroorzaakt door een tijdelijke fout. Als de fout wordt veroorzaakt door de inhoud van de blob, wordt de functie niet elke keer dat de blob wordt verwerkt. Standaard roept de SDK een functie maximaal 5 keer voor een bepaalde blob. Als de vijfde poging mislukt, wordt een bericht toegevoegd aan een wachtrij met de naam *webjobs-blobtrigger-poison*van de SDK.

Het maximum aantal pogingen kan worden geconfigureerd. [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) dezelfde instelling wordt voor poison blob verwerking en afhandeling van poison wachtrij berichten gebruikt.

Het bericht wachtrij voor poison BLOB's is een JSON-object met de volgende eigenschappen:

* FunctionId (in de notatie *{naam WebJob}*. Functies. *{Naam}*bijvoorbeeld: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" of "PageBlob")
* ContainerName
* BlobName
* ETag (een blob-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

In het volgende voorbeeld heeft de functie **CopyBlob** code die ervoor zorgt dat niet wordt uitgevoerd telkens wanneer deze wordt aangeroepen. Nadat de SDK wordt aangeroepen voor het maximum aantal pogingen, een bericht in de wachtrij poison blob wordt gemaakt en dat bericht wordt verwerkt door de functie **LogPoisonBlob** .

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }

        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

De SDK deserializes automatisch de JSON-bericht. Dit is de **PoisonBlobMessage** -klasse:

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>BLOB polling-algoritme

De WebJobs SDK scant alle containers die zijn opgegeven door de kenmerken van de **BlobTrigger** bij begin van de toepassing. In een grote opslagcapaciteit account deze scan kan enige tijd duren, zodat het mogelijk is het een tijdje voordat nieuwe BLOB's worden gevonden en **BlobTrigger** functies worden uitgevoerd.

Nieuwe of gewijzigde BLOB's detecteren na toepassing start, leest de SDK periodiek van de blob storage Logboeken. De blob-logboeken worden gebufferd en weggeschreven alleen fysiek elke 10 minuten of zo, zodat er aanzienlijke vertraging optreden nadat een blob wordt gemaakt of voordat u de bijbehorende bijgewerkt functie **BlobTrigger** wordt uitgevoerd.

Er is een uitzondering voor BLOB's die u maakt met behulp van het kenmerk **Blob** . Wanneer de WebJobs SDK een nieuwe blob maakt, doorgegeven deze de nieuwe blob onmiddellijk aan de overeenkomende **BlobTrigger** functies. Daarom hebt u een keten van blob-ingangen en uitgangen, kunt de SDK verwerken ze efficiënt. Maar als u wilt een lage latentie met de blob-functies voor BLOB's die zijn gemaakt of bijgewerkt op een andere manier verwerken, wordt aangeraden met **QueueTrigger** in plaats van **BlobTrigger**.

### <a name="blob-receipts"></a>BLOB-ontvangsten

De SDK WebJobs zorgt ervoor dat geen **BlobTrigger** functie meer dan één keer wordt aangeroepen voor de dezelfde nieuwe of bijgewerkte blob. Dit gebeurt door het bijhouden van *blob ontvangsten* te bepalen als een bepaald blob-versie is verwerkt.

BLOB ontvangsten worden opgeslagen in een container met de naam *azure-webjobs-hosts* in de Azure opslag-account die is opgegeven door de tekenreeks AzureWebJobsStorage. Een blob ontvangst heeft de volgende informatie:

* De functie die is aangeroepen voor de blob ('*{naam WebJob}*. Functies. *{Naam}*', bijvoorbeeld: "WebJob1.Functions.CopyBlob")
* De containernaam van de
* Het type blob ("BlockBlob" of "PageBlob")
* De naam van de blob
* De ETag (een blob-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

Als u afdwingen dat opwerking van een blob wilt, kunt u de ontvangst voor dat blob blob handmatig verwijderen uit de container *azure-webjobs-hosts* .

## <a name="related-topics-covered-by-the-queues-article"></a>Verwante onderwerpen door het artikel van wachtrijen

Voor informatie over het afhandelen van blob verwerking geactiveerd door een bericht wachtrij of voor de WebJobs SDK Zie scenario's niet voor blob-verwerking, [Azure wachtrij opslag met de WebJobs SDK gebruiken](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).

Verwante onderwerpen in dit artikel zijn de volgende:

* Async-functies
* Meerdere exemplaren
* Systeem correct wordt afgesloten
* Kenmerken WebJobs SDK in de hoofdtekst van een functie te gebruiken
* De SDK-verbindingsreeksen in code instellen.
* Waarden instellen voor WebJobs SDK constructor parameters in de code
* **MaxDequeueCount** configureren voor het verwerken van poison blob.
* Handmatig een functie activeren
* Schrijven van Logboeken

## <a name="next-steps"></a>Volgende stappen

Dit artikel heeft verstrekt codevoorbeelden waarin wordt aangegeven hoe u veelvoorkomende scenario's voor het werken met Azure BLOB's verwerken. Zie voor meer informatie over het gebruik van Azure WebJobs en de WebJobs SDK [documentatiebronnen Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).

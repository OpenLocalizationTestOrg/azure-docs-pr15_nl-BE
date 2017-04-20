<properties 
    pageTitle="Azure blob-opslag gebruiken met de WebJobs SDK" 
    description="Informatie over het gebruik van Azure blob-opslag met de WebJobs SDK. Een proces wordt geactiveerd wanneer een nieuwe blob wordt weergegeven in een container en poison BLOB's verwerken." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a>Azure blob-opslag gebruiken met de WebJobs SDK

## <a name="overview"></a>Overzicht

Deze handleiding bevat C# codevoorbeelden die hoe een proces wordt geactiveerd tonen wanneer een Azure blob wordt gemaakt of bijgewerkt. De codevoorbeelden gebruiken [WebJobs SDK](websites-dotnet-webjobs-sdk.md) versie 1.x.

Zie voor codevoorbeelden waarin wordt aangegeven hoe u BLOB's maken, [Azure wachtrij opslag met de WebJobs SDK gebruiken](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 
        
De handleiding wordt ervan uitgegaan dat u weet [hoe u een WebJob-project in Visual Studio met tekenreeksen die naar uw account opslag verwijzen maken](websites-dotnet-webjobs-sdk-get-started.md) of [meerdere accounts voor opslag](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

## <a id="trigger"></a>Hoe een functie wordt geactiveerd wanneer een blob wordt gemaakt of bijgewerkt

In dit gedeelte ziet u hoe u met de `BlobTrigger` kenmerk. 

> [AZURE.NOTE] De WebJobs SDK scant de logboekbestanden voor nieuwe of gewijzigde BLOB's bekijken. Dit proces is geen real-time; een functie kan niet ophalen geactiveerd tot enkele minuten of meer nadat de blob is gemaakt. Daarnaast [opslag logboeken worden gemaakt van "goede bedoelingen"](https://msdn.microsoft.com/library/azure/hh343262.aspx) basis; Er is geen garantie dat alle gebeurtenissen worden vastgelegd. Onder bepaalde omstandigheden mogelijk Logboeken worden overgeslagen. Als de beperkingen voor snelheid en betrouwbaarheid van blob triggers niet toegestaan voor uw toepassing zijn, wordt u aangeraden een bericht wachtrij maken wanneer u de blob maken en gebruiken van het kenmerk [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) in plaats van de `BlobTrigger` -kenmerk op de functie die de blob verwerkt.

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

## <a id="types"></a>Typen die u aan BLOB's binden kunt

U kunt de `BlobTrigger` -kenmerk op de volgende typen:

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Andere typen gedeserialiseerd door [ICloudBlobStreamBinder](#icbsb) 

Als u werken rechtstreeks met de Azure opslag-account wilt, kunt u ook toevoegen een `CloudStorageAccount` -parameter voor de handtekening van de methode.

Zie voor voorbeelden van de [blob bindende code in de bibliotheek azure-webjobs-sdk op GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).

## <a id="string"></a>Inhoud van de tekst blob binding met de tekenreeks ophalen

Als de tekst BLOB's worden verwacht, `BlobTrigger` kan worden toegepast op een `string` parameter. In het volgende voorbeeld wordt een blob tekst naar een `string` parameter met de naam `logMessage`. De parameter wordt gebruikt om de inhoud van de blob schrijven naar het dashboard WebJobs SDK. 
 
        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a>Blob inhoud ophalen worden geserialiseerd met behulp van ICloudBlobStreamBinder

In het volgende voorbeeld wordt een klasse die implementeert `ICloudBlobStreamBinder` om de `BlobTrigger` kenmerk een blob te binden de `WebImage` type.

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

De `WebImage` code binding wordt geleverd in een `WebImageBinder` klasse die is afgeleid van `ICloudBlobStreamBinder`.

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

## <a name="getting-the-blob-path-for-the-triggering-blob"></a>Ophalen van de blob-pad voor de label die als trigger dient

Voor het ophalen van de container en blob-naam van de label die de functie heeft geactiveerd, bevatten een `blobTrigger` de opgegeven parameter in de functiehandtekening.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a id="poison"></a>Het afhandelen van poison BLOB 's

Wanneer een `BlobTrigger` functie mislukt, de SDK aangeroepen, in het geval de fout is veroorzaakt door een tijdelijke fout. Als de fout wordt veroorzaakt door de inhoud van de blob, wordt de functie niet elke keer dat de blob wordt verwerkt. Standaard roept de SDK een functie maximaal 5 keer voor een bepaalde blob. Als de vijfde poging mislukt, wordt een bericht toegevoegd aan een wachtrij met de naam *webjobs-blobtrigger-poison*van de SDK.

Het maximum aantal pogingen kan worden geconfigureerd. [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) dezelfde instelling wordt voor poison blob verwerking en afhandeling van poison wachtrij berichten gebruikt. 

Het bericht wachtrij voor poison BLOB's is een JSON-object met de volgende eigenschappen:

* FunctionId (in de notatie *{naam WebJob}*. Functies. *{Naam}*bijvoorbeeld: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" of "PageBlob")
* ContainerName
* BlobName
* ETag (een blob-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

In het volgende codevoorbeeld wordt de `CopyBlob` functie is code die ervoor zorgt dat niet wordt uitgevoerd telkens wanneer deze wordt aangeroepen. Nadat de SDK voor het maximum aantal pogingen aangeroepen, een bericht in de wachtrij poison blob is gemaakt en wordt dit bericht verwerkt door de `LogPoisonBlob` functie. 

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

De SDK deserializes automatisch de JSON-bericht. Hier is de `PoisonBlobMessage` klasse: 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a>BLOB polling-algoritme

De WebJobs SDK scant alle containers die zijn opgegeven door `BlobTrigger` kenmerken aan begin van de toepassing. In een grote opslagcapaciteit account deze scan kan enige tijd duren, zodat het mogelijk is het een tijdje voordat nieuwe BLOB's worden gevonden en `BlobTrigger` functies worden uitgevoerd.

Nieuwe of gewijzigde BLOB's detecteren na toepassing start, leest de SDK periodiek van de blob storage Logboeken. De blob-logboeken worden gebufferd en weggeschreven alleen fysiek elke 10 minuten of zo, dus kan er aanzienlijke vertraging nadat een blob wordt gemaakt of voordat u de bijbehorende bijgewerkt `BlobTrigger` functie wordt uitgevoerd. 

Er is een uitzondering voor BLOB's die u met behulp van maakt de `Blob` kenmerk. Als u de SDK WebJobs maakt een nieuwe blob, deze de nieuwe blob onmiddellijk doorgegeven aan een overeenkomende `BlobTrigger` functies. Daarom hebt u een keten van blob-ingangen en uitgangen, kunt de SDK verwerken ze efficiënt. Maar als u wilt een lage latentie de blob met functies voor het verwerken van BLOB's die zijn gemaakt of op een andere manier bijgewerkt, adviseren we om met behulp van `QueueTrigger` en niet `BlobTrigger`.

### <a id="receipts"></a>BLOB-ontvangsten

De SDK WebJobs zorgt ervoor dat er geen `BlobTrigger` functie meer dan één keer wordt aangeroepen voor de dezelfde nieuwe of bijgewerkte blob. Dit gebeurt door het bijhouden van *blob ontvangsten* te bepalen als een bepaald blob-versie is verwerkt.

BLOB ontvangsten worden opgeslagen in een container met de naam *azure-webjobs-hosts* in de Azure opslag-account die is opgegeven door de tekenreeks AzureWebJobsStorage. Een blob ontvangst heeft de volgende informatie:

* De functie die is aangeroepen voor de blob ('*{naam WebJob}*. Functies. *{Naam}*', bijvoorbeeld: "WebJob1.Functions.CopyBlob")
* De containernaam van de
* Het type blob ("BlockBlob" of "PageBlob")
* De naam van de blob
* De ETag (een blob-id, bijvoorbeeld: "0x8D1DC6E70A277EF")

Als u afdwingen dat opwerking van een blob wilt, kunt u de ontvangst voor dat blob blob handmatig verwijderen uit de container *azure-webjobs-hosts* .

## <a id="queues"></a>Verwante onderwerpen door het artikel van wachtrijen

Voor informatie over het afhandelen van blob verwerking geactiveerd door een bericht wachtrij of voor de WebJobs SDK Zie scenario's niet voor blob-verwerking, [Azure wachtrij opslag met de WebJobs SDK gebruiken](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Verwante onderwerpen in dit artikel zijn de volgende:

* Async-functies
* Meerdere exemplaren
* Systeem correct wordt afgesloten
* Kenmerken WebJobs SDK in de hoofdtekst van een functie te gebruiken
* De SDK-verbindingsreeksen in code instellen.
* Waarden instellen voor WebJobs SDK constructor parameters in de code
* Configureren `MaxDequeueCount` voor het verwerken van poison blob.
* Handmatig een functie activeren
* Schrijven van Logboeken

## <a id="nextsteps"></a>Volgende stappen

Deze handleiding heeft verstrekt codevoorbeelden waarin wordt aangegeven hoe u veelvoorkomende scenario's voor het werken met Azure BLOB's verwerken. Zie voor meer informatie over het gebruik van Azure WebJobs en de WebJobs SDK [Azure WebJobs aanbevolen bronnen](http://go.microsoft.com/fwlink/?linkid=390226).
 

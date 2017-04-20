<properties
 pageTitle="Vervaldatum van opslag Azure blob in Azure CDN beheren | Microsoft Azure"
 description="Meer informatie over de opties voor het instellen van time-to-live voor BLOB's in cache Azure CDN."
 services="cdn"
 documentationCenter=""
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="multiple"
 ms.topic="article"
 ms.date="09/15/2016"
 ms.author="casoper"/>


# <a name="manage-expiration-of-azure-storage-blob-content-in-azure-cdn"></a>Vervaldatum van opslag Azure blob in Azure CDN beheren

> [AZURE.SELECTOR]
- [Azure Web Apps/Cloud Services, ASP.NET en IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure blob Storage-service](cdn-manage-expiration-of-blob-content.md)

De [blob-service](../storage/storage-introduction.md#blob-storage) in [Azure opslag](../storage/storage-introduction.md) is een van de verschillende producten op basis van Azure oorsprongen geïntegreerd met Azure CDN.  De inhoud van een openbaar toegankelijke blob opgeslagen kan worden in Azure CDN totdat de time-to-live (TTL) is verstreken.  De TTL-waarde wordt bepaald door de [header *Cache-Control* ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in de HTTP-reactie van Azure opslag.

>[AZURE.TIP] U kunt geen TTL-waarde instellen voor een blob.  In dit geval past Azure CDN u een standaard-TTL van zeven dagen.
>
>Zie de [Azure CDN-overzicht](./cdn-overview.md)voor meer informatie over de werking van Azure CDN snelheid van toegang tot BLOB's en andere bestanden.
>
>Zie voor meer informatie over de opslag van Azure blob-service, [Blob-Service-begrippen](https://msdn.microsoft.com/library/dd179376.aspx). 

Deze zelfstudie laat zien op verschillende manieren u de TTL-waarde voor een blob in Azure opslag instellen kunt.  

## <a name="azure-powershell"></a>Azure PowerShell

[Azure PowerShell](../powershell-install-configure.md) is een van de snelste, meest krachtige manieren om uw Azure services te beheren.  Gebruik de `Get-AzureStorageBlob` cmdlet haalt een verwijzing naar de blob, stel de `.ICloudBlob.Properties.CacheControl` eigenschap. 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

>[AZURE.TIP] U kunt ook PowerShell gebruiken voor het [beheren van uw profielen CDN en eindpunten](./cdn-manage-powershell.md).

## <a name="azure-storage-client-library-for-net"></a>Azure opslag Client Library for .NET

Instellen van een blob-TTL met .NET, gebruiken de [Azure opslag Client Library for .NET](../storage/storage-dotnet-how-to-use-blobs.md) de eigenschap [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) in te stellen.

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
        
        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

>[AZURE.TIP] Er zijn veel meer voorbeelden van .NET-code beschikbaar in de [Azure Blob Storage monsters voor .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).

## <a name="other-methods"></a>Andere methoden

- [Azure Interface met opdrachtregel](../xplat-cli-install.md)

    Tijdens het uploaden van de blob, de instellen *cacheControl* eigenschap met de `-p` overschakelen.  In het volgende voorbeeld wordt de TTL-waarde tot 1 uur (3600 seconden).

    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```

- [Azure opslagservices REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)

    De eigenschap *x-ms-blob-cache-control* expliciet instellen op verzoek van een [Blob plaatsen](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Lijst geblokkeerd geplaatst](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)of [Blob-eigenschappen instellen](https://msdn.microsoft.com/library/azure/ee691966.aspx) .

- Beheerhulpprogramma's voor externe opslag

    Sommige externe opslag Azure beheerprogramma's kunnen u het instellen van de eigenschap *CacheControl* op BLOB's. 

## <a name="testing-the-cache-control-header"></a>Testen van de header *Cache-Control*

U kunt eenvoudig controleren of de TTL-waarde van de BLOB's.  Met behulp van de browser voor [ontwikkelprogramma's](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)test dat de blob met inbegrip van de response-header *Cache-Control* .  U kunt ook een hulpprogramma zoals **wget**, [Postman](https://www.getpostman.com/)of [Fiddler](http://www.telerik.com/fiddler) te onderzoeken van de reactie headers.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over de header *Cache-Control*](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
- [Informatie over het verlopen van inhoud in Azure CDN Cloud-Service beheren](./cdn-manage-expiration-of-cloud-service-content.md)


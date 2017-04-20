<properties
    pageTitle="Hoe u Blob-opslag van Node.js | Microsoft Azure"
    description="Ongestructureerde gegevens opslaan in de cloud met Azure Blob-opslag (opslag van objecten)."
    services="storage"
    documentationCenter="nodejs"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>



# <a name="how-to-use-blob-storage-from-nodejs"></a>Het gebruik van Node.js Blob-opslag

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe u voor het uitvoeren van veelvoorkomende scenario's met behulp van Blob-opslag. De monsters worden geschreven via de API Node.js. De scenario's waarvoor bevatten het uploaden, lijst, downloaden en verwijderen van BLOB's.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Een Node.js-toepassing maken

Voor instructies over het maken van een toepassing Node.js, Zie [een Node.js in Azure App Service web app maken], [bouwen en implementeren van een toepassing Node.js een Azure Cloud service] --met behulp van Windows PowerShell of [bouwen en implementeren van een Node.js web app met Azure met behulp van Web-Matrix].

## <a name="configure-your-application-to-access-storage"></a>Uw toepassing configureren voor toegang tot opslag

Azure opslag gebruikt, moet u de SDK Azure opslag voor Node.js, inclusief een set met gemak bibliotheken die met de REST opslagservices communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Knooppunt Package Manager (NPM) gebruiken voor het verkrijgen van het pakket

1.  Interface met een opdrachtregel zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix), gebruiken om te navigeren naar de map waar u de voorbeeldtoepassing gemaakt.

2.  Typ **npm azure opslag installeren** in het opdrachtvenster. Uitvoer van de opdracht is vergelijkbaar met het volgende voorbeeld.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  U kunt handmatig uitvoeren met de opdracht **ls** te controleren dat een **knooppunt\_modules** map is gemaakt. In deze map vinden het **azure opslag** -pakket de bibliotheken die u nodig hebt bevat voor toegang tot opslag.

### <a name="import-the-package"></a>Het pakket importeren

Met Kladblok of een andere teksteditor, Voeg het volgende toe boven aan het bestand **server.js** van de toepassing waarin u van plan bent opslag gebruiken:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Een verbinding Azure opslag instellen

De module Azure leest de omgevingsvariabelen `AZURE_STORAGE_ACCOUNT` en `AZURE_STORAGE_ACCESS_KEY`, of `AZURE_STORAGE_CONNECTION_STRING`, voor informatie die vereist is om verbinding met uw account Azure opslag. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de accountgegevens opgeven bij het aanroepen van **createBlobService**.

Zie voor een voorbeeld van het instellen van de omgevingsvariabelen in [Azure Portal](https://portal.azure.com) voor een Azure web app [Node.js web app met behulp van de tabel Azure Service].

## <a name="create-a-container"></a>Maken van een container

Het **BlobService** -object kunt u werken met containers en BLOB's. De volgende code maakt een **BlobService** -object. Voeg de volgende aan de bovenkant van de **server.js**:

    var blobSvc = azure.createBlobService();

> [AZURE.NOTE] U kunt toegang krijgen tot een blob anoniem met behulp van **createBlobServiceAnonymous** en het adres van de host bieden. Gebruik bijvoorbeeld `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

U maakt een nieuwe container, met **createContainerIfNotExists**. Het volgende voorbeeld wordt een nieuwe container met de naam 'mycontainer':

    blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
        if(!error){
          // Container exists and is private
        }
    });

Als de container pas gemaakt wordt, `result.created` is ingesteld op true. Als de container al bestaat, `result.created` is ingesteld op false. `response`bevat informatie over de werking, met inbegrip van de ETag-informatie voor de container.

### <a name="container-security"></a>Container-beveiliging

Standaard nieuwe containers zijn vertrouwelijk en anoniem kunnen niet worden geopend. Als u de container openbare zodat u toegang hebt tot het anoniem, kunt u het toegangsniveau van de container instellen op **blob** of **container**.

* **blob** - kan anonieme leestoegang tot de blob inhoud en metagegevens in deze container, maar niet aan de container metagegevens, zoals een lijst van alle BLOB's in een container

* **container** - kunnen anonieme leestoegang tot de blob inhoud en metagegevens, alsmede container metagegevens

In het volgende voorbeeld wordt getoond als u het toegangsniveau op **blob**:

    blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
        if(!error){
          // Container exists and allows
          // anonymous read access to blob
          // content and metadata within this container
        }
    });

Ook kunt u het toegangsniveau van een container met behulp van **setContainerAcl** geeft u het toegangsniveau wijzigen. In het volgende voorbeeld verandert het toegangsniveau in container:

    blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
      if(!error){
        // Container access level set to 'container'
      }
    });

Het resultaat bevat informatie over de werking, met inbegrip van de huidige **ETag** voor de container.

### <a name="filters"></a>Filters

Optionele filterbewerkingen kunt u toepassen op de bewerkingen die worden uitgevoerd met behulp van **BlobService**. Filteren van bewerkingen kunt opnemen, aanmelden, automatisch opnieuw wordt geprobeerd, enz. Filters zijn objecten die een methode met de handtekening te implementeren:

    function handle (requestOptions, next)

Na de voorverwerking van de opties van de aanvraag, de methode moet aanroepen 'volgende' geven een retouraanroep met de volgende handtekening:

    function (returnObject, finalCallback, next)

In dit mechanisme, en na de verwerking van de returnObject (de reactie van de aanvraag naar de server), moet de retouraanroep op volgende aanroepen als deze bestaat als u wilt doorgaan met het verwerken van andere filters of gewoon roepen finalCallback om het oproepen van de service beëindigen.

Twee filters die logica opnieuw implementeren zijn opgenomen in de SDK Azure voor Node.js, **ExponentialRetryPolicyFilter** en **LinearRetryPolicyFilter**. De volgende manier maakt een **BlobService** -object dat de **ExponentialRetryPolicyFilter**wordt gebruikt:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var blobSvc = azure.createBlobService().withFilter(retryOperations);

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Er zijn drie soorten BLOB's: BLOB's blokkeren, BLOB's pagina en BLOB's toevoegen. BLOB's blokkeren kunnen dat u efficiënt grote gegevens uploaden. Append BLOB's zijn geoptimaliseerd voor bewerkingen toevoegen. Pagina BLOB's zijn geoptimaliseerd voor lezen/schrijven-bewerkingen. Zie [Wat blok BLOB's, BLOB's toevoegen en BLOB's pagina](http://msdn.microsoft.com/library/azure/ee691964.aspx)voor meer informatie.

### <a name="block-blobs"></a>BLOB's blokkeren

Als u wilt gegevens uploaden naar een blob blokkeren, gebruikt u de volgende:

* **createBlockBlobFromLocalFile** - een nieuw blok blob maakt en uploadt u de inhoud van een bestand

* **createBlockBlobFromStream** - een nieuw blok blob maakt en uploadt u de inhoud van een stream

* **createBlockBlobFromText** - een nieuw blok blob maakt en uploadt u de inhoud van een tekenreeks.

* **createWriteStreamToBlockBlob** - biedt een stroom schrijven naar een blob blokkeren

In het volgende voorbeeld wordt de inhoud van het bestand **test.txt** uploads naar **myblob**.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

De `result` die door deze methoden wordt geretourneerd bevat informatie over de werking, zoals de **ETag** van de blob.

### <a name="append-blobs"></a>BLOB's toevoegen

Als u wilt uploaden van gegevens naar een nieuwe append blob, gebruikt u de volgende:

* **createAppendBlobFromLocalFile** - een nieuwe append blob maakt en uploadt u de inhoud van een bestand

* **createAppendBlobFromStream** - een nieuwe append blob maakt en uploadt u de inhoud van een stream

* **createAppendBlobFromText** - een nieuwe append blob maakt en uploadt u de inhoud van een tekenreeks

* **createWriteStreamToNewAppendBlob** - maakt een nieuwe append blob en geeft vervolgens een stroom om ernaar te schrijven

In het volgende voorbeeld wordt de inhoud van het bestand **test.txt** uploads naar **myappendblob**.

    blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Als u een tekstblok toevoegt aan een bestaande append blob, gebruikt u de volgende:

* **appendFromLocalFile** - de inhoud van een bestand toevoegen aan een bestaande append blob

* **appendFromStream** - de inhoud van een stream toevoegen aan een bestaande append blob

* **appendFromText** - de inhoud van een tekenreeks toevoegen aan een bestaande append blob

* **appendBlockFromStream** - de inhoud van een stream toevoegen aan een bestaande append blob

* **appendBlockFromText** - de inhoud van een tekenreeks toevoegen aan een bestaande append blob

> [AZURE.NOTE] appendFromXXX API's doet client-side validatie om snel om te voorkomen dat de oproep van unncessary mislukt. appendBlockFromXXX niet het geval.

In het volgende voorbeeld wordt de inhoud van het bestand **test.txt** uploads naar **myappendblob**.

    blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
      if(!error){
        // text appended
      }
    });


### <a name="page-blobs"></a>BLOB's pagina

Uploaden van gegevens naar een blob pagina, gebruikt u de volgende:

* **createPageBlob** - maakt een nieuwe pagina blob van een bepaalde lengte

* **createPageBlobFromLocalFile** - maakt een nieuwe pagina blob en uploadt u de inhoud van een bestand

* **createPageBlobFromStream** - maakt een nieuwe pagina blob en uploadt u de inhoud van een stream

* **createWriteStreamToExistingPageBlob** - biedt een stroom schrijven naar een bestaande pagina blob

* **createWriteStreamToNewPageBlob** - een nieuwe pagina blob wordt gemaakt en worden vervolgens een stroom om ernaar te schrijven

In het volgende voorbeeld wordt de inhoud van het bestand **test.txt** uploads naar **mypageblob**.

    blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

> [AZURE.NOTE] Pagina BLOB's bestaan uit 512 byte 'pagina's '. U ontvangt een fout bij het uploaden van gegevens met een grootte die is geen veelvoud van 512 bytes.

## <a name="list-the-blobs-in-a-container"></a>Lijst van de BLOB's in een container

Gebruik de methode **listBlobsSegmented** om de BLOB's in een container. Als u BLOB's met een bepaald voorvoegsel terug wilt, gebruikt u **listBlobsSegmentedWithPrefix**.

    blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
      if(!error){
          // result.entries contains the entries
          // If not all blobs were returned, result.continuationToken has the continuation token.
      }
    });

De `result` bevat een `entries` -collectie is een matrix met objecten die een beschrijving van elke blob. Als alle BLOB's kunnen niet worden geretourneerd, de `result` biedt ook een `continuationToken`, die met de tweede parameter kunt u aanvullende waarden worden opgehaald.

## <a name="download-blobs"></a>BLOB's downloaden

U downloadt gegevens van een blob, gebruikt u de volgende:

* **getBlobToLocalFile** - schrijft de blob inhoud naar het bestand

* **getBlobToStream** - schrijft de blob inhoud aan een stroom

* **getBlobToText** - schrijft de blob inhoud naar een tekenreeks

* **createReadStream** - biedt een stroom lezen uit de blob

In het volgende voorbeeld ziet u de inhoud van de blob **myblob** downloaden en opslaan naar het bestand **uitvoer.txt** met behulp van een stroom met behulp van **getBlobToStream** :

    var fs = require('fs');
    blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
      if(!error){
        // blob retrieved
      }
    });

De `result` bevat informatie over de blob, met inbegrip van **ETag** informatie.

## <a name="delete-a-blob"></a>Een blob verwijderen

Tenslotte roept u wilt verwijderen van een blob, **deleteBlob**. In het volgende voorbeeld wordt de label met de naam **myblob**verwijderd.

    blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
      if(!error){
        // Blob has been deleted
      }
    });

## <a name="concurrent-access"></a>Gelijktijdige toegang

Om gelijktijdige toegang tot een blob van meerdere clients of meerdere procesexemplaren, kunt u **ETags** of **lease**.

* **Etag** - biedt een manier om op te sporen die de label of container is gewijzigd door een ander proces

* **Lease** - biedt een manier om te downloaden, exclusieve, duurzame, schrijven of toegang tot een blob verwijderen voor een periode

### <a name="etag"></a>ETag

ETags gebruiken als u toestaan dat meerdere clients of exemplaren wilt te schrijven naar het blok of de pagina Blob Blob tegelijk. De ETag kunt u zien als de container of blob is gewijzigd sinds u aanvankelijk lezen of gemaakt, kunt u voorkomen dat overschrijven de wijzigingen doorgevoerd door een andere client of proces.

U kunt de ETag voorwaarden instellen met behulp van de optionele `options.accessConditions` parameter. In het volgende voorbeeld het bestand **test.txt** alleen uploads als de blob al bestaat en heeft de ETag-waarde die door `etagToMatch`.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
        if(!error){
        // file uploaded
      }
    });

Wanneer u ETags gebruikt, is het algemene patroon:

1. De ETag krijgen als gevolg van een maken, een lijst of een get-bewerking.

2. Een bewerking uitvoeren, controleren of de ETag-waarde niet is gewijzigd.

Als de waarde is gewijzigd, betekent dit dat een andere client of exemplaar gewijzigd blob of container nadat u de ETag-waarde opgehaald.

### <a name="lease"></a>Lease

Door de methode **acquireLease** gebruikt, wordt het blob of de container die u verkrijgen van een lease-overeenkomst wilt op te geven, kunt u een nieuwe lease aanschaffen. Bijvoorbeeld verkrijgt de volgende code een lease op **myblob**.

    blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
      if(!error) {
        console.log('leaseId: ' + result.id);
      }
    });

Verdere bewerkingen op **myblob** dient de `options.leaseId` parameter. De ID wordt geretourneerd als lease `result.id` van **acquireLease**.

> [AZURE.NOTE] Standaard is de leaseduur oneindig. U kunt een duur niet oneindig (tussen 15 en 60 seconden) opgeven door middel van de `options.leaseDuration` parameter.

U kunt een lease-overeenkomst, met **releaseLease**. Om een lease te verbreken, maar voorkomen dat anderen een nieuwe lease totdat de oorspronkelijke duur is verstreken, gebruikt u **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Werken met gedeelde toegang handtekeningen

Gedeelde toegang handtekeningen (SAS) zijn een veilige manier voor gedetailleerde toegang tot BLOB's en containers zonder dat u uw accountnaam voor opslag of sleutels. Gedeelde toegang handtekeningen worden vaak gebruikt om beperkte toegang tot uw gegevens, zoals een mobiele toepassingen toegang krijgen tot de BLOB's.

> [AZURE.NOTE] Terwijl u ook anonieme toegang tot BLOB's verlenen kunt, kunnen gedeelde toegang handtekeningen bieden meer beperkte toegang, moet u de SA's genereren.

Een vertrouwde toepassing, zoals een service cloud-gebaseerde handtekeningen van gedeelde toegang met behulp van de **generateSharedAccessSignature** van de **BlobService**genereert en biedt dit aan een niet-vertrouwde of semi-vertrouwde toepassing, zoals een mobiele app. Gedeelde toegang handtekeningen worden gegenereerd met behulp van een beleid waarin de begin- en einddatums die de gedeelde toegang handtekeningen geldig zijn, evenals het toegangsniveau verleend aan de houder van de handtekeningen gedeelde toegang.

In het volgende voorbeeld genereert een nieuwe gedeelde-beleid waarmee de houder gedeelde toegang handtekeningen te lezen bewerkingen uitvoeren op de **myblob** -blob en verloopt de 100 minuten na het tijdstip waarop dat deze is gemaakt.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
    var host = blobSvc.host;

Houd er rekening mee dat de moet hostinformatie worden verstrekt, zoals vereist is wanneer de houder van de handtekeningen gedeelde toegang probeert te krijgen tot de container.

Vervolgens de clienttoepassing maakt gebruik van gedeelde toegang handtekeningen met **BlobServiceWithSAS** voor het uitvoeren van bewerkingen op de blob. De volgende ophalen informatie over **myblob**.

    var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
    sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
      if(!error) {
        // retrieved info
      }
    });

Aangezien de gedeelde toegang handtekeningen zijn gegenereerd met alleen-lezen toegang als wordt geprobeerd de blob te wijzigen, wordt een fout geretourneerd.

### <a name="access-control-lists"></a>Access control list

U kunt ook een toegangsbeheerlijst (ACL) voor het instellen van het toegangsbeleid voor SA's. Dit is handig als u wilt dat meerdere clients toegang hebben tot een container, maar bieden verschillende-beleid voor elke client.

Een ACL is geïmplementeerd met behulp van een array-beleid, met een ID die is gekoppeld aan elk beleid. In het volgende voorbeeld definieert twee beleidsregels, één voor 'user1' en voor 'gebruiker2':

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
        Start: startDate,
        Expiry: expiryDate
      }
    };

In het volgende voorbeeld wordt de huidige ACL voor **mycontainer**en wordt het nieuwe beleid met behulp van **setBlobAcl**. Deze benadering kunt:

    var extend = require('extend');
    blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Zodra de ACL is ingesteld, kunt u vervolgens gedeelde toegang handtekeningen op basis van de ID voor een beleid. In het volgende voorbeeld maakt u nieuwe gedeelde toegang handtekeningen voor 'gebruiker2':

    blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie.

-   [Azure opslag SDK voor knooppunt API: naslag][]
-   [Azure opslag-teamblog][]
-   [Azure opslag SDK voor knooppunt][] opslagplaats op GitHub
-   [Node.js Developer Center](/develop/nodejs/)
-   [Gegevens overbrengen met het opdrachtregelprogramma AzCopy](storage-use-azcopy.md)

[Azure opslag SDK voor knooppunt]: https://github.com/Azure/azure-storage-node

[Een Node.js web app in Azure App Service maken]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
[Node.js web app met behulp van de tabel Azure Service]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[Bouwen en implementeren van een Node.js web app met Azure met behulp van Web-Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com
[Bouwen en implementeren van een toepassing Node.js een Azure Cloud-service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Azure opslag-teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure opslag SDK voor knooppunt API: naslag]: http://dl.windowsazure.com/nodestoragedocs/index.html

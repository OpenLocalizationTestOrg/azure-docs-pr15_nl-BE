<properties
    pageTitle="Het gebruik van Azure Blob-opslag van iOS | Microsoft Azure"
    description="Ongestructureerde gegevens opslaan in de cloud met Azure Blob-opslag (opslag van objecten)."
    services="storage"
    documentationCenter="ios"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-ios"></a>Het gebruik van iOS Blob-opslag

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe u voor het uitvoeren van veelvoorkomende scenario's met Microsoft Azure Blob-opslag. De monsters worden geschreven in doelstelling C en gebruikt de [Clientbibliotheek van Azure opslag voor iOS](https://github.com/Azure/azure-storage-ios). De scenario's die zijn **geüpload** **aanbieding**, **downloaden**en BLOB's **verwijderen** . Voor meer informatie over BLOB's, Zie de sectie [Volgende stappen](#next-steps) . U kunt ook downloaden van het [monster app](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) snel overzicht van het gebruik van Azure opslag in een toepassing voor iOS.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>De opslag van Azure iOS-bibliotheek importeren in uw toepassing

Met behulp van de [Azure opslag CocoaPod](https://cocoapods.org/pods/AZSClient) of door het bestand **kader** te importeren, kunt u de opslag van Azure iOS-bibliotheek importeren in uw toepassing.

## <a name="cocoapod"></a>CocoaPod

1. Als u nog niet hebt gedaan, [CocoaPods installeren](https://guides.cocoapods.org/using/getting-started.html#toc_3) op uw computer door een terminalvenster te openen en de volgende opdracht

        sudo gem install cocoapods

2. Vervolgens gaat u naar de projectmap (de map waarin de `.xcodeproj` bestand), maakt u een nieuw bestand met de naam `Podfile`(zonder extensie). Voeg de volgende aan `Podfile` en opslaan

        pod 'AZSClient'

3. Ga naar de projectmap in het venster terminal en voer de volgende opdracht

        pod install

4. Als uw `.xcodeproj` is geopend in Xcode, sluit u deze. Open het nieuwe project-bestand dat u hebt in de directory van uw project de `.xcworkspace` extensie. Dit is het bestand werkt u uit voor nu aan.

## <a name="framework"></a>Framework
Om de opslag van Azure iOS-bibliotheek gebruikt, moet u eerst het framework-bestand maken.

1. Eerst downloaden of de [azure-opslag-ios repo](https://github.com/azure/azure-storage-ios)klonen.

2. Ga naar de *azure-opslag-ios* -> *Lib* -> *Azure opslag Client-bibliotheek*en open `AZSClient.xcodeproj` in Xcode.

3. Op de linkerbovenhoek van Xcode, het actieve schema "Azure opslag Client Library"-"Framework" te wijzigen.

4. Compileer het project (⌘ + B). Hiermee maakt u een `AZSClient.framework` bestand op het bureaublad.

U kunt de framework-bestand vervolgens importeren in uw toepassing als volgt:

1. Maak een nieuw project of open het bestaande project Xcode.

2. Klik op het project in de linkernavigatiebalk en klik op het tabblad *Algemeen* boven aan de project editor.

3. Klik op de knop toevoegen (+) in de sectie *gekoppelde kaders en bibliotheken* .

4. Klik op *toevoegen als u...*. Navigeer naar en voeg de `AZSClient.framework` dat u hebt gemaakt.

5. In de sectie *gekoppelde kaders en bibliotheken* , klikt u nogmaals op de knop toevoegen (+).

6. Zoeken in de lijst met bibliotheken al geleverd, `libxml2.2.dylib` en voeg deze toe aan uw project.

7. Open het tabblad *Instellingen bouwen* aan de bovenkant van de project editor.

8. *Framework zoekpaden* Dubbelklik onder de sectie *Paden zoeken* en toevoegen het pad naar uw `AZSClient.framework` bestand.

## <a name="import-statement"></a>, Importinstructie
U moet de volgende aanwijzing import opnemen in het bestand waar u de Azure opslag API aanroepen.

    // Include the following import statement to use blob APIs.
    #import <AZSClient/AZSClient.h>

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Asynchrone bewerkingen
> [AZURE.NOTE] Alle methoden die een verzoek aan de service uitvoeren zijn asynchrone bewerkingen. In de codevoorbeelden vindt u dat deze methoden een handler is voltooid hebben. Code binnen de voltooiing-handler wordt uitgevoerd **nadat** die de aanvraag is voltooid. Code na de voltooiing-handler wordt uitgevoerd **terwijl** de aanvraag wordt uitgevoerd.

## <a name="create-a-container"></a>Maken van een container
Elke blob in Azure opslag moet zich bevinden in een container. In het volgende voorbeeld ziet u het maken van een zogenaamd, *newcontainer*, in uw account opslag als deze nog niet bestaat. Bij het kiezen van een naam voor de container worden gelet op de hierboven genoemde regels voor naamgeving.

    -(void)createContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

      // Create container in your Storage account if the container doesn't already exist
      [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
         if (error){
             NSLog(@"Error in creating container.");
         }
      }];
    }

U kunt bevestigen dat dit werkt door te kijken naar de [Microsoft Azure Opslagverkenner](http://storageexplorer.com) en die *newcontainer* is in de lijst van containers voor uw opslag-account controleren.

## <a name="set-container-permissions"></a>Container machtigingen instellen
De machtigingen van de container worden standaard geconfigureerd voor **privé** -toegang. Containers bieden echter een paar verschillende opties voor toegang tot de container:

- **Privé**: Container en blob-gegevens door de eigenaar van de account alleen kan worden gelezen.

- **BLOB**: Blob-gegevens in deze container via anonieme aanvragen kunnen worden gelezen, maar de container is niet beschikbaar. Clients kunnen niet opsommen BLOB's in de container via anonieme aanvragen.

- **Container**: Container en blob-gegevens kunnen worden gelezen via anonieme aanvragen. Clients BLOB's in de container via anonieme aanvragen kunnen opsommen, maar kunnen niet inventariseren containers in de opslag-account.

In het volgende voorbeeld wordt beschreven hoe u een container met **Container** toegangsmachtigingen openbaar, alleen-lezen toegang voor alle gebruikers op het Internet kunt maken:

    -(void)createContainerWithPublicAccess{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create container in your Storage account if the container doesn't already exist
        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
            if (error){
                NSLog(@"Error in creating container.");
            }
        }];
    }

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container
Zoals vermeld in de sectie [Blob-service-begrippen](#blob-service-concepts) , Blob-opslag biedt drie verschillende soorten BLOB's: BLOB's blokkeren, BLOB's toevoegen en BLOB's pagina. Op dit moment ondersteunt de opslag van Azure iOS-bibliotheek alleen BLOB's blokkeren. Blok blob is in de meeste gevallen het aanbevolen type te gebruiken.

In het volgende voorbeeld ziet u hoe een blob blok van een NSString uploaden. Als een blob met dezelfde naam al in deze container bestaat, wordt de inhoud van deze label worden overschreven.

    -(void)uploadBlobToContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
         {
             if (error){
                 NSLog(@"Error in creating container.");
             }
             else{
                 // Create a local blob object
                 AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                 // Upload blob to Storage
                 [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                     if (error){
                         NSLog(@"Error in creating blob.");
                     }
                 }];
             }
         }];
    }

U kunt bevestigen dat dit werkt door te kijken naar de [Microsoft Azure Opslagverkenner](http://storageexplorer.com) en verifiëren dat de container, *containerpublic*, de blob, *sampleblob bevat*. In dit voorbeeld hebben we een openbare container gebruikt zodat u ook controleren kunt of dit door te gaan naar de blobs URI heeft gewerkt:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Naast het uploaden van een blob blok van een NSString bestaan vergelijkbare methoden voor NSData, NSInputStream of een lokaal bestand.

## <a name="list-the-blobs-in-a-container"></a>Lijst van de BLOB's in een container
In het volgende voorbeeld ziet u hoe u alle BLOB's in een container. Als u deze bewerking uitvoert, worden gelet op de volgende parameters:     

- **continuationToken** - token voortzetting van de staat waar de aanbieding bewerking moet beginnen. Als er geen is opgegeven, wordt het BLOB's vanaf het begin een lijst. Een willekeurig aantal BLOB's kan worden weergegeven, van nul tot een maximale set. Zelfs als deze methode nul resultaten retourneert als `results.continuationToken` is niet nul is, is er mogelijk meer BLOB's op de service die niet is aangeboden.
- **prefix** - kunt u het voorvoegsel voor de aanbieding van de blob. Alleen de BLOB's die met dit voorvoegsel beginnen weergegeven.
- **useFlatBlobListing** - zoals vermeld in de sectie [Naming en verwijzende containers en BLOB's](#naming-and-referencing-containers-and-blobs) , hoewel de Blob-service een opslagschema met platte is kunt u een virtuele hiërarchie door de naam van de BLOB's met informatie over het pad. Aanbieding niet plat is echter momenteel niet ondersteund; Dit is binnenkort beschikbaar. Nu moet deze waarde worden`YES`
- **blobListingDetails** - kunt u opgeven welke artikelen moeten worden opgenomen bij het aanbieden van BLOB 's
    - `AZSBlobListingDetailsNone`: Alleen toegezegde BLOB's aanbieden en blob metagegevens niet terug.
    - `AZSBlobListingDetailsSnapshots`: Lijst doorgevoerd BLOB's en momentopnamen van de blob.
    - `AZSBlobListingDetailsMetadata`: Ophalen blob metagegevens voor elk blob geretourneerd in de aanbieding.
    - `AZSBlobListingDetailsUncommittedBlobs`: BLOB's vastgelegde en niet-doorgevoerde lijst.
    - `AZSBlobListingDetailsCopy`: Eigenschappen kopiëren in de aanbieding zijn.
    - `AZSBlobListingDetailsAll`: Alle beschikbare toegezegde BLOB's, niet-doorgevoerde BLOB's en momentopnamen weergeven en alle metagegevens en kopie status voor de BLOB's.
- **maxResults** - het maximum aantal resultaten worden geretourneerd voor deze bewerking. Gebruik -1 om een limiet niet instellen.
- **completionHandler** - het blok code uit te voeren met de resultaten van de aanbieding.

In dit voorbeeld wordt een helpmethode wordt gebruikt om recursief oproep de lijst BLOB's methode telkens wanneer een voortzetting token wordt geretourneerd.

    -(void)listBlobsInContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        //List all blobs in container
        [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
            if (error != nil){
                NSLog(@"Error in creating container.");
            }
        }];
    }

    //List blobs helper method
    -(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
    {
        [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
            if (error)
            {
                completionHandler(error);
            }
            else
            {
                for (int i = 0; i < results.blobs.count; i++) {
                    NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
                }
                if (results.continuationToken)
                {
                    [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
                }
                else
                {
                    completionHandler(nil);
                }
            }
        }];
    }


## <a name="download-a-blob"></a>Een blob downloaden

In het volgende voorbeeld ziet u hoe een blob downloaden naar een NSString-object.

    -(void)downloadBlobToString{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

        // Download blob
        [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
            if (error) {
                NSLog(@"Error in downloading blob");
            }
            else{
                NSLog(@"%@",text);
            }
        }];
    }

## <a name="delete-a-blob"></a>Een blob verwijderen

In het volgende voorbeeld ziet u hoe een blob te verwijderen.

    -(void)deleteBlob{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

        // Delete blob
        [blockBlob deleteWithCompletionHandler:^(NSError *error) {
            if (error) {
                NSLog(@"Error in deleting blob.");
            }
        }];
    }

## <a name="delete-a-blob-container"></a>Een blob-container

In het volgende voorbeeld ziet u hoe een container.

    -(void)deleteContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

      // Delete container
      [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
         if(error){
             NSLog(@"Error in deleting container");
         }
      }];
    }

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u Blob-opslag van iOS, volg deze koppelingen voor meer informatie over de i/o-bibliotheek en de Storage-service.

- [Azure clientbibliotheek van opslag voor iOS](https://github.com/azure/azure-storage-ios)
- [Azure opslag iOS documentatie](http://azure.github.io/azure-storage-ios/)
- [Azure opslagservices REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure opslag-teamblog](http://blogs.msdn.com/b/windowsazurestorage)

Als u vragen over deze bibliotheek hebt gerust naar onze [forum MSDN Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) of een [Stackoverloop](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).
Boek [Azure opslag](https://feedback.azure.com/forums/217298-storage/)feedback hebt u suggesties voor opslag van Azure functie.

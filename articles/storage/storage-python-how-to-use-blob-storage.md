<properties
    pageTitle="Het gebruik van Azure Blob-opslag (opslag van objecten) van Python | Microsoft Azure"
    description="Ongestructureerde gegevens opslaan in de cloud met Azure Blob-opslag (opslag van objecten)."
    services="storage"
    documentationCenter="python"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-azure-blob-storage-from-python"></a>Het gebruik van Azure Blob-opslag van Python

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

Azure Blob storage is een service die ongestructureerde gegevens worden opgeslagen in de cloud als objecten/BLOB's. BLOB-opslag kan tekst of binaire gegevens, zoals een document, bestand of installatieprogramma voor elk type worden opgeslagen. BLOB-opslag is ook opslag van objecten genoemd.

In dit artikel wordt beschreven hoe u voor het uitvoeren van veelvoorkomende scenario's met behulp van Blob-opslag. De monsters zijn geschreven in Python en de [SDK van Microsoft Azure opslag voor Python]gebruiken. De scenario's die zijn geüpload, aanbieding, downloaden en verwijderen van BLOB's.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a>Maken van een container

Op basis van het type blob die u wilt gebruiken, maakt u een object **BlockBlobService**, **AppendBlobService**of **PageBlobService** . De volgende code wordt een **BlockBlobService** -object. De volgende boven aan een Python-bestand waarin u Azure Blob blokopslag via programmering toegang wilt toevoegen.

    from azure.storage.blob import BlockBlobService

De volgende code maakt een **BlockBlobService** -object met behulp van de sleutel opslag account naam en account.  'Mijnaccount' en 'mykey' vervangen door uw naam en sleutel.

    block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

In het volgende codevoorbeeld kunt u een **BlockBlobService** -object voor het maken van de container als het niet bestaat.

    block_blob_service.create_container('mycontainer')

Nieuwe container is standaard persoonlijk, dus moet u de toegangstoets opslag (zoals u eerder hebt gedaan) voor het BLOB's downloaden van deze container. Als u wilt dat de BLOB's in de container beschikbaar maken voor iedereen, kunt u de container maken en geeft u het niveau van de toegang van het publiek met de volgende code.

    from azure.storage.blob import PublicAccess
    block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)

U kunt ook een container wijzigen nadat u het hebt gemaakt met de volgende code.

    block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)

Na deze wijziging ziet iedereen op Internet BLOB's in een openbare container, maar alleen wijzigen of verwijderen.

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Gebruik wilt maken van een blok blob en uploaden van gegevens, de **maken\_blob\_van\_pad**, **maken\_blob\_van\_stream**, **maken\_blob\_van\_bytes** of **maken\_blob\_van\_tekst** methoden. Ze zijn op hoog niveau methoden voor het uitvoeren van de noodzakelijke logische groepen te verdelen wanneer de grootte van de gegevens die langer is dan 64 MB.

**maken\_blob\_van\_pad** de inhoud van het opgegeven pad van een bestand uploadt en **maken\_blob\_van\_stroom** uploadt u de inhoud van een reeds geopende bestandsstroom. **maken\_blob\_van\_bytes** uploadt een matrix van bytes en **maken\_blob\_van\_tekst** uploadt u de waarde van de opgegeven tekst met de opgegeven codering (standaard wordt UTF-8).

In het volgende voorbeeld worden de inhoud van het bestand **sunset.png** in de **myblob** -blob.

    from azure.storage.blob import ContentSettings
    block_blob_service.create_blob_from_path(
        'mycontainer',
        'myblockblob',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png')
                )

## <a name="list-the-blobs-in-a-container"></a>Lijst van de BLOB's in een container

U kunt het aantal BLOB's in een container gebruiken de **lijst\_BLOB's** methode. Deze methode retourneert een generator. De volgende code wordt de **naam** van elke blob in een container aan de console.

    generator = block_blob_service.list_blobs('mycontainer')
    for blob in generator:
        print(blob.name)

## <a name="download-blobs"></a>BLOB's downloaden

Gebruiken om gegevens te downloaden van een blob, **krijgen\_blob\_te\_pad**, **ophalen\_blob\_te\_stream**, **krijgen\_blob\_te\_bytes**, of **krijgen\_blob\_te\_tekst**. Ze zijn op hoog niveau methoden voor het uitvoeren van de noodzakelijke logische groepen te verdelen wanneer de grootte van de gegevens die langer is dan 64 MB.

Het volgende voorbeeld worden **u\_blob\_te\_pad** de inhoud van de blob **myblob** downloaden en opslaan naar het bestand **out sunset.png** .

    block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')

## <a name="delete-a-blob"></a>Een blob verwijderen

Tenslotte roept u wilt verwijderen van een blob, **delete_blob**.

    block_blob_service.delete_blob('mycontainer', 'myblockblob')

## <a name="writing-to-an-append-blob"></a>Schrijven naar een blob toevoegen

Een toevoegquery blob is geoptimaliseerd voor append bewerkingen, zoals het aanmelden. Net als een blob blok een blob append bestaat uit blokken, maar als u een nieuw blok aan een blob toevoegen toevoegen, altijd wordt toegevoegd aan het einde van de blob. U kan bijwerken of verwijderen van een bestaand notitieblok in een blob toevoegen. De blokkering van id's voor een blob toevoegen niet beschikbaar zijn als voor een blob blokkeren.

Elk blok in een toevoegquery blob kan een ander formaat, met een maximum van 4 MB en een blob append een maximum van 50.000 blokken kunt opnemen. De maximale grootte van een toevoegquery blob is dus iets meer dan 195 GB (4 MB X 50.000 blokken).

In het volgende voorbeeld wordt een nieuwe append blob gemaakt en sommige worden gegevens toegevoegd aan het simuleren van een eenvoudige registratie.

    from azure.storage.blob import AppendBlobService
    append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

    # The same containers can hold all types of blobs
    append_blob_service.create_container('mycontainer')

    # Append blobs must be created before they are appended to
    append_blob_service.create_blob('mycontainer', 'myappendblob')
    append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

    append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Blob-opslag hebt geleerd, volg deze koppelingen voor meer informatie.

- [Python voor ontwikkelaars](/develop/python/)
- [Azure opslagservices REST API](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure opslag-teamblog]
- [Opslag van Azure Microsoft SDK for Python]

[Azure opslag-teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Opslag van Azure Microsoft SDK for Python]: https://github.com/Azure/azure-storage-python

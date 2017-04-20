<properties
    pageTitle="Gegevens verplaatsen en naar Azure Blob-opslag met behulp van Python | Microsoft Azure"
    description="Gegevens verplaatsen en naar Azure Blob-opslag met behulp van Python"
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a>Gegevens verplaatsen en naar Azure Blob-opslag met behulp van Python

In dit onderwerp wordt beschreven hoe u een lijst, uploaden en downloaden met de Python API BLOB's. Met de Python API in Azure SDK wordt geleverd, kunt u:

- Maken van een container
- Een blob uploaden naar een container
- BLOB's downloaden
- Lijst van de BLOB's in een container
- Een blob verwijderen

Zie voor meer informatie over het gebruik van de API van Python, [het gebruik van de Blob Storage-Service van Python](../storage/storage-python-how-to-use-blob-storage.md).

Advies over technologieën die worden gebruikt om gegevens te verplaatsen naar of uit de Azure Blob-opslag hier zijn gekoppeld:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Als u van VM die is ingesteld met de scripts die [gegevens wetenschap virtuele machines in Azure gebruikmaakt](machine-learning-data-science-virtual-machines.md), is vervolgens AzCopy al geïnstalleerd op de VM.

> [AZURE.NOTE] Verwijzen naar [Azure Blob grondbeginselen](../storage/storage-dotnet-how-to-use-blobs.md) en [Azure Blob](https://msdn.microsoft.com/library/azure/dd179376.aspx)-service voor een volledige inleiding op Azure blob-opslag.


## <a name="prerequisites"></a>Vereisten

Dit document wordt ervan uitgegaan dat er een Azure-abonnement, een opslag-account en de bijbehorende sleutel opslag voor die account. Voor het uploaden/downloaden van gegevens, moet u uw account Azure opslag de sleutel de naam en account weten.

- Zie instellen van een Azure-abonnement, [gratis proefperiode van een maand](https://azure.microsoft.com/pricing/free-trial/).

- Zie voor instructies over het maken van een account voor opslag en voor rekening en belangrijke informatie ophalen [over Azure opslag rekeningen](../storage/storage-create-storage-account.md).


## <a name="upload-data-to-blob"></a>Gegevens uploaden naar Blob

In het volgende fragment aan de bovenkant van Python code waarin u via programmering toegang tot opslag Azure wilt toevoegen:

    from azure.storage.blob import BlobService

Het **BlobService** -object kunt u werken met containers en BLOB's. De volgende code maakt een BlobService-object met behulp van de sleutel opslag account naam en account. Accountnaam en sleutel rekening met uw bestaande account en sleutel vervangen.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

De volgende methoden gebruiken voor het uploaden van gegevens naar een blob:

1. plaatsen\_blok\_blob\_van\_(de inhoud van het opgegeven pad van een bestand uploadt) pad
2. plaatsen\_block_blob\_van\_bestand (de inhoud van een reeds geopende bestandsstroom geüpload)
3. plaatsen\_blok\_blob\_van\_bytes (uploads een matrix van bytes)
4. plaatsen\_blok\_blob\_van\_(de waarde van de opgegeven tekst met de opgegeven codering uploads) tekst

De volgende voorbeeldcode wordt een lokaal bestand uploadt naar een container:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

De volgende voorbeeldcode uploadt u alle bestanden (uitgezonderd mappen) in een lokale map naar een blobopslag:

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a>Gegevens van de Blob downloaden

De volgende methoden gebruiken voor het downloaden van gegevens uit een blob:
1. Get\_blob\_te\_pad
2. Get\_blob\_te\_bestand
3. Get\_blob\_te\_bytes
4. Get\_blob\_te\_tekst

Deze methoden voor het uitvoeren van de noodzakelijke logische groepen te verdelen wanneer de grootte van de gegevens die langer is dan 64 MB.

De volgende code downloadt de inhoud van een blob in een container in een lokaal bestand:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

De volgende code downloadt alle BLOB's van een container. Deze lijst gebruikt\_BLOB's als u de lijst met beschikbare BLOB's in de container en downloadt naar een lokale map.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name

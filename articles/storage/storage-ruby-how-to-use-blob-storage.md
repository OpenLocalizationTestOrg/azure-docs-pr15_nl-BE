<properties
    pageTitle="Hoe u Blob-opslag (opslag van objecten) van Ruby | Microsoft Azure"
    description="Ongestructureerde gegevens opslaan in de cloud met Azure Blob-opslag (opslag van objecten)."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-blob-storage-from-ruby"></a>Het gebruik van Ruby Blob-opslag

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Overzicht

Azure Blob storage is een service die ongestructureerde gegevens worden opgeslagen in de cloud als objecten/BLOB's. BLOB-opslag kan tekst of binaire gegevens, zoals een document, bestand of installatieprogramma voor elk type worden opgeslagen. BLOB-opslag is ook opslag van objecten genoemd.

Deze handleiding wordt beschreven hoe u voor het uitvoeren van veelvoorkomende scenario's met behulp van Blob-opslag. De monsters worden geschreven met de API voor Ruby. De scenario's die bevatten **worden geüpload, weergeven, downloaden,** en het **verwijderen van** BLOB's.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Een Ruby-toepassing maken

Maak een Ruby-toepassing. Zie voor instructies de [Ruby op Rails webtoepassing op een Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)

## <a name="configure-your-application-to-access-storage"></a>Uw toepassing configureren voor toegang tot opslag

Azure opslag gebruikt, moet u downloaden en gebruiken van de Ruby azure-pakket, inclusief een set met gemak bibliotheken die met de REST opslagservices communiceren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems downloaden van het pakket te gebruiken

1. Interface met een opdrachtregel zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix) gebruiken.

2. Typ "azure gem installeren" in het opdrachtvenster de gem en afhankelijkheden wilt installeren.

### <a name="import-the-package"></a>Het pakket importeren

Met uw favoriete teksteditor, voeg de volgende boven aan de Ruby-bestand waar u van plan bent opslag gebruiken:

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Setup een verbinding Azure opslag

De module azure leest de omgevingsvariabelen **AZURE\_opslag\_ACCOUNT** en **AZURE\_opslag\_ACCESS_KEY** voor meer informatie verbinding maken met uw account Azure opslag vereist. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de gegevens voordat u **Azure::Blob::BlobService** met de volgende code:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"


Als u deze waarden uit een klassieke of Resource Manager opslag account in Azure portal:

1. Log in op de [Azure portal](https://portal.azure.com).
2. Navigeer naar de opslag-account die u wilt gebruiken.
3. In het blad van de instellingen aan de rechterkant, klikt u op de **Toegangstoetsen**.
4. In het blad van de toegang tot sleutels die wordt weergegeven, ziet u de toegangstoets 1 en toegangssleutel 2. U kunt een van beide gebruiken. 
5. Klik op het pictogram kopiëren om de sleutel naar het Klembord kopiëren. 

Als u deze waarden uit een klassieke opslag account in de klassieke Azure portal:

1. Log in op de [klassieke Azure portal](https://manage.windowsazure.com).
2. Navigeer naar de opslag-account die u wilt gebruiken.
3. Klik op de **TOEGANGSTOETSEN beheren** onder aan het navigatiedeelvenster.
4. In het dialoogvenster pop ziet u de accountnaam voor opslag, toegang voor primaire en secundaire toegangssleutel. Voor een sneltoets, kunt u een primaire of secundaire een. 
5. Klik op het pictogram kopiëren om de sleutel naar het Klembord kopiëren.

## <a name="create-a-container"></a>Maken van een container

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Het **Azure::Blob::BlobService** -object kunt u werken met containers en BLOB's. Gebruik wilt maken van een container, de **maken\_container()** methode.

In het volgende voorbeeld wordt een container of het afdrukken van de fout gemaakt als een.

    azure_blob_service = Azure::Blob::BlobService.new
    begin
      container = azure_blob_service.create_container("test-container")
    rescue
      puts $!
    end

Als u wilt dat de bestanden in de container om openbaar te maken, kunt u de machtigingen van de container instellen.

U kunt alleen wijzigen de <strong>maken\_container()</strong> oproep door te geven de **: openbare\_toegang\_niveau** optie:

    container = azure_blob_service.create_container("test-container",
      :public_access_level => "<public access level>")


Geldige waarden voor de **: openbare\_access\_niveau** optie zijn:

* **blob:** Hiermee geeft u een volledige openbare leestoegang voor container- en blob-gegevens. Clients BLOB's in de container via anonieme aanvragen kunnen opsommen, maar kunnen niet inventariseren containers in de opslag-account.

* **container:** Hiermee geeft u een openbare leestoegang voor BLOB's. BLOB-gegevens in deze container via anonieme aanvragen kunnen worden gelezen, maar de container is niet beschikbaar. Clients kunnen niet opsommen BLOB's in de container via anonieme aanvragen.

U kunt het niveau van de toegang van het publiek van een container ook wijzigen met behulp van **instellen\_container\_acl()** methode voor het instellen van de toegang van het publiek.

In het volgende voorbeeld wordt het niveau van de toegang van het publiek naar **container**:

    azure_blob_service.set_container_acl('test-container', "container")

## <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container

Gebruiken om de inhoud uploadt naar een blob, de **maken\_blok\_blob()** methode voor het maken van de blob, een bestand of tekenreeks gebruikt als de inhoud van de blob.

De volgende code uploadt u het bestand **test.png** als een nieuwe blob met de naam "image-blob' in de container.

    content = File.open("test.png", "rb") { |file| file.read }
    blob = azure_blob_service.create_block_blob(container.name,
      "image-blob", content)
    puts blob.name

## <a name="list-the-blobs-in-a-container"></a>Lijst van de BLOB's in een container

U kunt de containers **list_containers()** methode te gebruiken.
U kunt het aantal BLOB's in een container gebruiken **lijst\_blobs()** methode.

Hiermee wordt de URL's van alle BLOB's in de containers voor de account.

    containers = azure_blob_service.list_containers()
    containers.each do |container|
      blobs = azure_blob_service.list_blobs(container.name)
      blobs.each do |blob|
        puts blob.name
      end
    end

## <a name="download-blobs"></a>BLOB's downloaden

Als u wilt downloaden van BLOB's de **u\_blob()** methode voor het ophalen van de inhoud.

In het volgende voorbeeld wordt gedemonstreerd met **u\_blob()** downloaden van de inhoud van 'afbeelding '-blob en het schrijven naar een lokaal bestand.

    blob, content = azure_blob_service.get_blob(container.name,"image-blob")
    File.open("download.png","wb") {|f| f.write(content)}

## <a name="delete-a-blob"></a>Een Blob verwijderen
Ten slotte gebruiken als u wilt verwijderen van een blob, de **verwijderen\_blob()** methode. In het volgende voorbeeld laat zien hoe een blob te verwijderen.

    azure_blob_service.delete_blob(container.name, "image-blob")

## <a name="next-steps"></a>Volgende stappen

Meer informatie over complexere opslagtaken, klik op deze koppelingen:

- [Azure opslag-teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) bibliotheek op GitHub
- [Overdracht van gegevens met het hulpprogramma AzCopy voor opdrachtregel](storage-use-azcopy.md)

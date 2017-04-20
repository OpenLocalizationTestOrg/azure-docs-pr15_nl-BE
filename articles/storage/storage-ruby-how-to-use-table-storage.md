<properties
    pageTitle="Het gebruik van Azure tabelopslag van Ruby | Microsoft Azure"
    description="Gestructureerde gegevens opslaan in de cloud opslag Azure-tabel, een gegevensarchief NoSQL."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor=""/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-ruby"></a>Het gebruik van Azure tabelopslag van Ruby

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht

Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's voor het gebruik van de tabel Azure service uitvoeren. De monsters worden geschreven met de API voor Ruby. De scenario's die zijn **maken en verwijderen van een tabel invoegen en entiteiten in een tabel opvragen**.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Een Ruby-toepassing maken

Voor instructies hoe een toepassing voor Ruby, [Ruby op Rails webtoepassing op een Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)Zie.


## <a name="configure-your-application-to-access-storage"></a>Uw toepassing configureren voor toegang tot opslag

Azure opslag gebruikt, moet u downloaden en gebruiken van de Ruby azure pakket waarin een verzameling gemak bibliotheken die met de REST van de opslag-services communiceren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems downloaden van het pakket te gebruiken

1. Interface met een opdrachtregel zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix) gebruiken.

2. Typ **gem azure installeren** in het opdrachtvenster de gem en afhankelijkheden wilt installeren.

### <a name="import-the-package"></a>Het pakket importeren

Gebruik uw favoriete teksteditor, Voeg het volgende toe boven aan de Ruby-bestand waar u van plan bent opslag gebruiken:

    require "azure"

## <a name="set-up-an-azure-storage-connection"></a>Een verbinding Azure opslag instellen

De module azure leest de omgevingsvariabelen **AZURE\_opslag\_ACCOUNT** en **AZURE\_opslag\_ACCESS\_sleutel** voor de informatie die nodig is om verbinding met uw account Azure opslag. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de gegevens voordat u **Azure::TableService** met de volgende code:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"

Als u deze waarden uit een klassieke of Resource Manager opslag account in Azure Portal:

1. Log in op de [Azure Portal](https://portal.azure.com).
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

## <a name="create-a-table"></a>Een tabel maken

Het **Azure::TableService** -object kunt u werken met tabellen en entiteiten. U kunt een tabel maken de **maken\_table()** methode. In het volgende voorbeeld wordt een tabel of het afdrukken van de fout gemaakt als een.

    azure_table_service = Azure::TableService.new
    begin
      azure_table_service.create_table("testtable")
    rescue
      puts $!
    end

## <a name="add-an-entity-to-a-table"></a>Een entiteit aan een tabel toevoegen

Als u wilt toevoegen van een entiteit, een hash-object dat de entiteitseigenschappen definieert eerst te maken. Houd er rekening mee dat voor elke entiteit moet u een **PartitionKey** en **RowKey**. Deze zijn de unieke id's van de entiteiten en zijn waarden die veel sneller dan de andere eigenschappen kunnen worden opgezocht. Azure opslag gebruikt de **PartitionKey** entiteiten van de tabel automatisch over veel opslagknooppunten verdelen. Entiteiten met de dezelfde **PartitionKey** worden opgeslagen op hetzelfde knooppunt. De **RowKey** is de unieke ID van de entiteit in de partitie waartoe het behoort.

    entity = { "content" => "test entity",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.insert_entity("testtable", entity)

## <a name="update-an-entity"></a>Een entiteit bijwerken

Er zijn meerdere methoden voor het bijwerken van een bestaande entiteit:

* **update\_entity():** Een bestaande entiteit bijwerken door deze te vervangen.
* **samenvoegen\_entity():** Een bestaande entiteit bijgewerkt met nieuwe waarden samenvoegen in bestaande entiteit.
* **invoegen\_of\_samenvoegen\_entity():** Een bestaande entiteit worden bijgewerkt door deze te vervangen. Als er geen entiteit bestaat, wordt er een nieuwe ingevoegd:
* **invoegen\_of\_vervangen door\_entity():** Een bestaande entiteit bijgewerkt met nieuwe waarden samenvoegen in bestaande entiteit. Als er geen entiteit bestaat, wordt een nieuw ingevoegd.

Het volgende voorbeeld wordt het bijwerken van een entiteit die **update\_entity()**:

    entity = { "content" => "test entity with updated content",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.update_entity("testtable", entity)

Met **update\_entity()** en **samenvoegen\_entity()**, als de entiteit die u wilt bijwerken en vervolgens de update mislukt bestaat niet. Dus als u opslaan van een entiteit wilt, ongeacht of deze al bestaat, moet u in plaats daarvan gebruiken **invoegen\_of\_vervangen door\_entity()** of **invoegen\_of\_samenvoegen\_entity()**.

## <a name="work-with-groups-of-entities"></a>Werken met groepen van entiteiten

Soms is het verstandig meerdere bewerkingen samen in een batch zodat atomaire verwerkt door de server te verzenden. Vandaar dat u eerst een **Batch** -object maken en vervolgens de **uitvoeren\_batch()** methode op **TableService**. In het volgende voorbeeld ziet u twee entiteiten met RowKey 2 en 3 in een batch indienen. Merk op dat deze functie alleen voor entiteiten met de dezelfde PartitionKey werkt.

    azure_table_service = Azure::TableService.new
    batch = Azure::Storage::Table::Batch.new("testtable",
      "test-partition-key") do
      insert "2", { "content" => "new content 2" }
      insert "3", { "content" => "new content 3" }
    end
    results = azure_table_service.execute_batch(batch)

## <a name="query-for-an-entity"></a>Query voor een entiteit

Als u een entiteit in een tabel, een query wilt gebruiken de **ophalen\_entity()** methode, door de naam van de tabel, **PartitionKey** en **RowKey**.

    result = azure_table_service.get_entity("testtable", "test-partition-key",
      "1")

## <a name="query-a-set-of-entities"></a>Een verzameling van entiteiten query

Om een set van de entiteiten in een tabel een query, een query-hash-object maken en gebruiken de **query\_entities()** methode. Het volgende voorbeeld worden alle entiteiten met de dezelfde **PartitionKey**verkrijgen:

    query = { :filter => "PartitionKey eq 'test-partition-key'" }
    result, token = azure_table_service.query_entities("testtable", query)

> [AZURE.NOTE] Als de resultaatset is te groot voor een enkele query om terug te keren, een token voortzetting geretourneerd die u kunt gebruiken voor het ophalen van de volgende pagina's.

## <a name="query-a-subset-of-entity-properties"></a>Een subset van de entiteitseigenschappen opvragen

Een query in een tabel kunt u een paar eigenschappen ophalen uit een entiteit. Deze techniek, genaamd "projectie", minder bandbreedte en kan query's sneller, met name voor grote entiteiten. De select-component gebruiken en geef de namen van de eigenschappen die u wilt overdragen naar de client.

    query = { :filter => "PartitionKey eq 'test-partition-key'",
      :select => ["content"] }
    result, token = azure_table_service.query_entities("testtable", query)

## <a name="delete-an-entity"></a>Een entiteit verwijderen

Als u een entiteit verwijdert, wilt gebruiken de **verwijderen\_entity()** methode. U moet doorgeven in de naam van de tabel waarin de entiteit, de PartitionKey en de RowKey van de entiteit.

        azure_table_service.delete_entity("testtable", "test-partition-key", "1")

## <a name="delete-a-table"></a>Een tabel verwijderen

Om een tabel te verwijderen, gebruikt u de **verwijderen\_table()** methode en de besturing van de naam van de tabel die u wilt verwijderen.

        azure_table_service.delete_table("testtable")

## <a name="next-steps"></a>Volgende stappen

Meer informatie over complexere opslagtaken, klik op deze koppelingen:

- [Azure opslag-teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK for Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) bibliotheek op GitHub

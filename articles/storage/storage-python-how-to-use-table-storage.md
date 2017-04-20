<properties
    pageTitle="Het gebruik van opslag van Python tabel | Microsoft Azure"
    description="Gestructureerde gegevens opslaan in de cloud opslag Azure-tabel, een gegevensarchief NoSQL."
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


# <a name="how-to-use-table-storage-from-python"></a>Het gebruik van opslag van Python tabel

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Overzicht

Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's uitvoeren met behulp van de tabel Azure storage-service. De monsters zijn geschreven in Python en de [SDK van Microsoft Azure opslag voor Python]gebruiken. De gedekte scenario's omvatten maken en verwijderen van een tabel invoegen en entiteiten in een tabel opvragen.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-table"></a>Een tabel maken

Het **TableService** -object kunt u werken met tabel-services. De volgende code maakt een **TableService** -object. Voeg de code aan de bovenkant van de Python-bestand waarin u wilt via programmering toegang tot opslag Azure:

    from azure.storage.table import TableService, Entity

De volgende code wordt een **TableService** -object gemaakt met behulp van de sleutel opslag account naam en account.  'Mijnaccount' en 'mykey' vervangen door uw naam en sleutel.

    table_service = TableService(account_name='myaccount', account_key='mykey')

    table_service.create_table('tasktable')

## <a name="add-an-entity-to-a-table"></a>Een entiteit aan een tabel toevoegen

Als u wilt toevoegen van een entiteit, maakt u eerst een woordenlijst of een lichaam waarin de entiteit de namen van eigenschappen en waarden. Houd er rekening mee dat voor elke entiteit, moet u **PartitionKey** en **RowKey**. Dit zijn de unieke id's van de entiteiten. U kunt zoeken met deze waarden veel sneller dan kunt u de andere eigenschappen opvragen. **PartitionKey** gebruikt door het systeem automatisch de tabel entiteiten over veel opslagknooppunten te verdelen.
Entiteiten die de dezelfde **PartitionKey** hebben worden op hetzelfde knooppunt opgeslagen. **RowKey** is de unieke ID van de entiteit in de partitie waartoe het behoort.

Doorgeven als een entiteit toevoegen aan uw tabel, een dictionary-object met de **plaats\_entiteit** methode.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
    table_service.insert_entity('tasktable', task)

U kunt ook een exemplaar van de klasse **entiteit** doorgeven de **plaats\_entiteit** methode.

    task = Entity()
    task.PartitionKey = 'tasksSeattle'
    task.RowKey = '2'
    task.description = 'Wash the car'
    task.priority = 100
    table_service.insert_entity('tasktable', task)

## <a name="update-an-entity"></a>Een entiteit bijwerken

Deze code laat zien hoe de oude versie van een bestaande entiteit te vervangen door een bijgewerkte versie.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage', 'priority' : 250}
    table_service.update_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

Als de entiteit die wordt bijgewerkt niet bestaat, wordt de updatebewerking mislukt. Als u een entiteit, ongeacht of deze vóór bevatte opslaan, gebruikt u **invoegen\_of\_replace_entity**.
In het volgende voorbeeld wordt de eerste aanroep vervangen bestaande entiteit. Het tweede gesprek een nieuwe entiteit wordt ingevoegd na geen entiteit met de opgegeven **PartitionKey** en **RowKey** in de tabel voorkomt.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage again', 'priority' : 250}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '3', 'description' : 'Buy detergent', 'priority' : 300}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

## <a name="change-a-group-of-entities"></a>Een groep van entiteiten wijzigen

Soms is het verstandig meerdere bewerkingen samen in een batch zodat atomaire verwerkt door de server te verzenden. Vandaar dat u de klasse **TableBatch** . Als u wilt dat de batch indienen, roept u **commit\_batch**. Houd er rekening mee dat alle entiteiten mag op dezelfde partitie als een batch worden gewijzigd. In het volgende voorbeeld worden twee entiteiten samen in een batch toegevoegd.

    from azure.storage.table import TableBatch
    batch = TableBatch()
    task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
    task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
    batch.insert_entity(task10)
    batch.insert_entity(task11)
    table_service.commit_batch('tasktable', batch)

Partijen kunnen ook worden gebruikt met de contex-syntaxis:

    task12 = {'PartitionKey': 'tasksSeattle', 'RowKey': '12', 'description' : 'Go grocery shopping', 'priority' : 400}
    task13 = {'PartitionKey': 'tasksSeattle', 'RowKey': '13', 'description' : 'Clean the bathroom', 'priority' : 100}

    with table_service.batch('tasktable') as batch:
        batch.insert_entity(task12)
        batch.insert_entity(task13)


## <a name="query-for-an-entity"></a>Query voor een entiteit

Als u een entiteit in een tabel, een query wilt gebruiken de **ophalen\_entiteit** methode door **PartitionKey** en **RowKey**.

    task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
    print(task.description)
    print(task.priority)

## <a name="query-a-set-of-entities"></a>Een verzameling van entiteiten query

In dit voorbeeld wordt gezocht naar dat alle taken in Seattle op basis van **PartitionKey**.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
    for task in tasks:
        print(task.description)
        print(task.priority)

## <a name="query-a-subset-of-entity-properties"></a>Een subset van de entiteitseigenschappen opvragen

Een query in een tabel kunt u een paar eigenschappen ophalen uit een entiteit.
Deze techniek, *projectie*, genaamd minder bandbreedte en kan query's sneller, met name voor grote entiteiten. Gebruik de parameter **selecteren** en geeft u de namen van de eigenschappen die u wilt overdragen naar de client.

De query in de volgende code retourneert alleen de beschrijvingen van de entiteiten in de tabel.

[AZURE.NOTE] In het volgende fragment werkt alleen tegen de cloud storage-service. Dit wordt niet ondersteund door de emulator opslag.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
    for task in tasks:
        print(task.description)

## <a name="delete-an-entity"></a>Een entiteit verwijderen

U kunt een entiteit met behulp van de sleutel voor de partitie en rij verwijderen.

    table_service.delete_entity('tasktable', 'tasksSeattle', '1')

## <a name="delete-a-table"></a>Een tabel verwijderen

De volgende code verwijdert een tabel uit een opslag-account.

    table_service.delete_table('tasktable')

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van de opslag van de tabel, klik op deze koppelingen voor meer informatie.

- [Python voor ontwikkelaars](/develop/python/)
- [Azure opslagservices REST API](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure opslag-teamblog]
- [Opslag van Azure Microsoft SDK for Python]

[Azure Storage Team blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Opslag van Azure Microsoft SDK for Python]: https://github.com/Azure/azure-storage-python

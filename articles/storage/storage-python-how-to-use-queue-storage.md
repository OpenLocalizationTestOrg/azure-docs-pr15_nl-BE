<properties
    pageTitle="Het gebruik van opslag van de wachtrij van Python | Microsoft Azure"
    description="Informatie over het gebruik van de service Azure wachtrij van Python maken en verwijderen van wachtrijen en invoegen, ophalen en verwijderen van berichten."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-python"></a>Het gebruik van opslag van Python wachtrij

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht

Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's met behulp van de wachtrij Azure storage-service uitvoeren. De monsters zijn geschreven in Python en de [SDK van Microsoft Azure opslag voor Python]gebruiken. De scenario's die zijn **ingevoegd**, **inspecteren**, **ophalen**en **verwijderen van** berichten, alsmede **maken en verwijderen van wachtrijen**. Raadpleeg de sectie [Vervolgstappen] voor meer informatie over wachtrijen.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a>Procedure: Een wachtrij maken

Het **QueueService** -object kunt u werken met wachtrijen. De volgende code maakt een **QueueService** -object. Voeg de volgende aan de bovenkant van de Python bestanden waarmee u via programmering toegang tot opslag Azure wenst:

    from azure.storage.queue import QueueService

De volgende code maakt een **QueueService** -object met behulp van de sleutel opslag account naam en account. 'Mijnaccount' en 'mykey' vervangen door uw naam en sleutel.

    queue_service = QueueService(account_name='myaccount', account_key='mykey')

    queue_service.create_queue('taskqueue')


## <a name="how-to-insert-a-message-into-a-queue"></a>Procedure: Plaats een bericht in een wachtrij

Om een bericht in een wachtrij wilt invoegen, gebruikt u de **plaatsen\_bericht** methode om een nieuw bericht maken en toevoegen aan de wachtrij.

    queue_service.put_message('taskqueue', u'Hello World')


## <a name="how-to-peek-at-the-next-message"></a>Procedure: Het volgende bericht bekijken

U kunt bekijken van het bericht aan de voorzijde van een wachtrij zonder het te verwijderen uit de wachtrij door het aanroepen van de **peek\_berichten** methode. Standaard **peek\_berichten** peeks op één bericht.

    messages = queue_service.peek_messages('taskqueue')
    for message in messages:
        print(message.content)


## <a name="how-to-dequeue-messages"></a>Procedure: Berichten in wachtrij

De code wordt een bericht in een wachtrij in twee stappen. Wanneer u belt **u\_berichten**, krijgt u het volgende bericht in een wachtrij standaard. Een bericht dat wordt geretourneerd door **u\_berichten** onzichtbaar is voor een andere code voor het lezen van berichten vanuit deze wachtrij wordt. Standaard blijft dit bericht onzichtbaar gedurende 30 seconden. Als u klaar bent met het bericht uit de wachtrij te verwijderen, moet u ook aanroepen **verwijderen\_bericht**. Dit proces van het verwijderen van een bericht bent u verzekerd dat wanneer uw code niet werkt voor het verwerken van een bericht door defecte hardware of software, een ander exemplaar van uw code kunt hetzelfde bericht krijgen en probeer het opnieuw. Het aanroepen van code **verwijderen\_bericht** zodra het bericht is verwerkt.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)

Er zijn twee manieren waarop u kunt ophalen van berichten uit een wachtrij.
U krijgt eerst een reeks berichten (maximaal 32). Ten tweede kunt u instellen een time-out invisibility langer of korter waardoor uw code meer of minder tijd aan elk bericht volledig verwerken. Het volgende codevoorbeeld wordt de **u\_berichten** methode 16 berichten in één aanroep ophalen. En vervolgens de verwerking van elk bericht met een for-lus. De time-out invisibility wordt ingesteld op vijf minuten voor elk bericht.

    messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)      


## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedure: De inhoud van een bericht in de wachtrij wijzigen

U kunt de inhoud van een bericht ter plaatse in de wachtrij wijzigen. Als het bericht een werktaak, kunt u deze functie voor het bijwerken van de status van de werktaak. De code hieronder wordt de **update\_bericht** methode voor het bijwerken van een bericht. De zichtbaarheid van time-out is ingesteld op 0, wat betekent dat het bericht onmiddellijk wordt weergegeven en de inhoud wordt bijgewerkt.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')

## <a name="how-to-get-the-queue-length"></a>Procedure: De lengte van de wachtrij ophalen

U kunt een schatting van het aantal berichten in een wachtrij. De **u\_wachtrij\_metagegevens** methode vraagt de wachtrijservice om terug te keren metagegevens over de wachtrij en de **approximate_message_count**. Het resultaat is alleen bij benadering, omdat de berichten worden toegevoegd of verwijderd nadat de queue-service op uw aanvraag reageert.

    metadata = queue_service.get_queue_metadata('taskqueue')
    count = metadata.approximate_message_count

## <a name="how-to-delete-a-queue"></a>Procedure: Een wachtrij verwijderen

Voor het verwijderen van een wachtrij en alle berichten die dit, roept de **verwijderen\_wachtrij** methode.

    queue_service.delete_queue('taskqueue')

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van Queue storage, klik op deze koppelingen voor meer informatie.

- [Python voor ontwikkelaars](/develop/python/)
- [Azure opslagservices REST API](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure opslag-teamblog]
- [Opslag van Azure Microsoft SDK for Python]

[Azure opslag-teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Opslag van Azure Microsoft SDK for Python]: https://github.com/Azure/azure-storage-python

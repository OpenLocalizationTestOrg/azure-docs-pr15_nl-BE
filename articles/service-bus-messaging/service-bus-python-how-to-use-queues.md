<properties 
    pageTitle="Het gebruik van de Service Bus wachtrijen met Python | Microsoft Azure" 
    description="Informatie over het gebruik van wachtrijen van Python Azure Service Bus." 
    services="service-bus" 
    documentationCenter="python" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="sethm;lmazuel"/>


# <a name="how-to-use-service-bus-queues"></a>Het gebruik van wachtrijen Service Bus

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Dit artikel wordt beschreven hoe u wachtrijen Service Bus. De monsters zijn geschreven in Python en het [pakket Python Azure Service Bus][]gebruiken. De scenario's die gedekt zijn **wachtrijen maken, verzenden en ontvangen van berichten**en **wachtrijen verwijderen**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

> [AZURE.NOTE] Zie de [Installatiehandleiding van Python](../python-how-to-install.md)Python of [Python Azure Service Bus-pakket][]installeren.

## <a name="create-a-queue"></a>Een wachtrij maken

Het **ServiceBusService** -object kunt u werken met wachtrijen. Voeg de volgende code in de buurt van de bovenkant van de Python-bestand waarin u wilt via programmering toegang tot Service Bus:

```
from azure.servicebus import ServiceBusService, Message, Queue
```

De volgende code maakt een **ServiceBusService** -object. Vervangen `mynamespace`, `sharedaccesskeyname`, en `sharedaccesskey` met de naamruimte, gedeelde toegang handtekening (SAS) naam en waarde.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

De waarden voor de naam van de SA's en de waarde kunnen worden gevonden in de verbindingsgegevens [Azure klassieke portal][] of in het deelvenster **Eigenschappen** van Visual Studio bij het selecteren van de naamruimte Service Bus in Server Explorer (zoals in de vorige sectie).

```
bus_service.create_queue('taskqueue')
```

**create_queue** ondersteunt ook extra opties waarmee u kunt overschrijven standaard wachtrij-instellingen zoals bericht tijd live (TTL) of maximale grootte. Het volgende voorbeeld wordt de maximale grootte van 5GB en de TTL-waarde op 1 minuut:

```
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a>Berichten naar een wachtrij verzenden

Een bericht verzenden naar een wachtrij Service Bus, het aanroepen van de toepassing de **verzenden\_wachtrij\_bericht** methode voor het **ServiceBusService** -object.

In het volgende voorbeeld wordt gedemonstreerd hoe u een testbericht verzenden naar de wachtrij *met behulp van taskqueue* met de naam **verzenden\_wachtrij\_bericht**:

```
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Service Bus wachtrijen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de van standaard- en aangepaste toepassingseigenschappen bevat, hebben een maximale grootte van 64 KB. Er is geen limiet op het aantal berichten in een wachtrij is gehouden, maar er is een cap op de totale grootte van de berichten die door een wachtrij. De grootte van de wachtrij is gedefinieerd bij het maken, met een maximum van 5 GB. Zie voor meer informatie over quota, [contingenten Service Bus][].

## <a name="receive-messages-from-a-queue"></a>Berichten uit een wachtrij

Berichten worden ontvangen van een wachtrij met behulp van de **ontvangen\_wachtrij\_bericht** methode voor het object **ServiceBusService** :

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

Berichten uit de wachtrij worden verwijderd als ze worden gelezen wanneer de parameter **peek\_lock** is ingesteld op **False**. U kunt lezen (peek) en vergrendelen van het bericht zonder het te verwijderen uit de wachtrij door de parameter **peek\_lock** op **True**.

Het gedrag van lezen en het bericht verwijderen als onderdeel van het ontvangen van de eenvoud en het meest geschikt voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Om dit te begrijpen, kunt u overwegen een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze worden verwerkt. Omdat Service Bus wordt het bericht als het wordt verbruikt, gemarkeerd en vervolgens, wanneer de toepassing wordt opnieuw opgestart en begint berichten opnieuw te gebruiken, deze zal hebt gemist het bericht dat is verbruikt vóór de crash.

Als de **peek\_lock** parameter is ingesteld op **True**, wordt de ontvangen een bewerking twee fase, die het mogelijk maakt voor van ondersteuningstoepassingen die ontbrekende berichten niet kunnen tolereren. Bus-Service een aanvraag ontvangt, deze vindt u het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten ontvangen als geeft als resultaat de toepassing. Nadat de toepassing is het bericht verwerkt (of betrouwbaar voor toekomstige verwerking opgeslagen), wordt de tweede fase van de procedure ontvangen door het aanroepen van de methode **delete** voor **het berichtobject** voltooid. De methode **delete** wordt markeren van het bericht worden gebruikt en uit de wachtrij verwijderen.

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het afhandelen van toepassing crashes en berichten onleesbaar

Bus service biedt functies waarmee u zonder problemen herstellen van fouten in uw toepassing of moeilijkheden verwerken van een bericht. Als een toepassing van de ontvanger kan het bericht verwerkt voor een of andere reden, kan deze de methode **ontgrendelen** aanroepen voor het object **weergegeven** . Hierdoor wordt de Service Bus te ontgrendelen van het bericht in de wachtrij en beschikbaar stellen aan opnieuw worden ontvangen door de toepassing in beslag nemen of door een andere toepassing voor in beslag nemen.

Er is ook een time-out is gekoppeld aan een bericht in de wachtrij is vergrendeld en als de aanvraag niet verwerken het bericht voordat u de vergrendeling time-out is verstreken (bijv. Als de toepassing loopt vast), vervolgens Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen.

In het geval dat de toepassing vastloopt nadat het bericht is verwerkt, maar voordat de methode **delete** wordt aangeroepen, zal vervolgens het bericht worden geleverd aan de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **Tenminste eenmaal verwerken**dat elk bericht wordt ten minste eenmaal verwerkt maar in bepaalde situaties kan hetzelfde bericht worden geleverd. Als het scenario kan niet dubbele verwerking tolereren, kunnen ontwikkelaars van toepassingen moeten extra logica toevoegen aan hun toepassing voor aflevering van dubbele berichten verwerken. Dit wordt vaak bereikt met behulp van de eigenschap **MessageId** van het bericht over zelf constant blijven.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de grondbeginselen van Service Bus wachtrijen, klik op deze koppelingen voor meer informatie.

-   Zie [wachtrijen, onderwerpen, en abonnementen][].

[Azure klassieke portal]: https://manage.windowsazure.com
[Pakket Python Azure Service Bus]: https://pypi.python.org/pypi/azure-servicebus  
[Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
[Quota Service Bus]: service-bus-quotas.md
 

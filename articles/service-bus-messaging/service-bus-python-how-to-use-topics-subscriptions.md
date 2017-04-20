<properties 
    pageTitle="Het gebruik van de Service Bus onderwerpen met Python | Microsoft Azure" 
    description="Informatie over het gebruik van Azure Service Bus onderwerpen en Python-abonnementen." 
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
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Het gebruik van de Service Bus onderwerpen en abonnementen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dit artikel wordt beschreven hoe u Service Bus onderwerpen en abonnementen. De monsters worden geschreven in Python en de [Azure Python pakket][]. De scenario's die zijn **onderwerpen en abonnementen maken** **abonnement filters maken**, **berichten verzenden naar een onderwerp**, **ontvangen van berichten van een abonnement**en **onderwerpen en abonnementen verwijderen**. Zie de sectie [Volgende stappen](#next-steps) voor meer informatie over onderwerpen en abonnementen.

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

**Opmerking:** Als u installeren van Python of [Azure Python pakket wilt][], Zie de [Installatiehandleiding van Python](../python-how-to-install.md).

## <a name="create-a-topic"></a>Een onderwerp maken

Het **ServiceBusService** -object kunt u werken met onderwerpen. Voeg de volgende aan de bovenkant van de Python bestanden waarmee u programmatisch toegang verkrijgen tot de Service Bus wenst:

```
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

De volgende code maakt een **ServiceBusService** -object. Vervangen `mynamespace`, `sharedaccesskeyname`, en `sharedaccesskey` met de werkelijke naamruimte gedeelde toegang handtekening (SAS) sleutelwaarde naam en sleutel.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Kunt u de waarden voor de naam van de SA's en de waarde van de [portal Azure][].

```
bus_service.create_topic('mytopic')
```

**maken\_onderwerp** biedt ook ondersteuning voor aanvullende opties waarmee u kunt de standaardinstellingen voor onderwerp zoals bericht tijd om live of onderwerp maximale grootte negeren. Het volgende voorbeeld wordt de grootte van het onderwerp van maximaal 5 GB en een keer met live (TTL)-waarde van 1 minuut:

```
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Abonnementen maken

Abonnementen op onderwerpen worden ook gemaakt met het **ServiceBusService** -object. Abonnementen zijn naam en een optionele filter dat ervoor zorgt dat de reeks berichten die in de virtuele wachtrij van het abonnement kunnen hebben.

> [AZURE.NOTE] Abonnementen zijn permanent en blijft bestaan totdat zij, of het onderwerp waarop zij zijn geabonneerd, worden verwijderd.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement met het standaard (MatchAll)-filter maken

Het **MatchAll** -filter is het standaard-filter die wordt gebruikt als er geen filter is opgegeven bij het maken van een nieuw abonnement. Als u het filter **MatchAll** gebruikt, worden alle berichten die zijn gepubliceerd op het onderwerp in van het abonnement virtuele wachtrij geplaatst. In het volgende voorbeeld wordt gemaakt van een abonnement met de naam 'AllMessages' en wordt het standaardfilter voor **MatchAll** .

```
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken

Ook kunt u filters waarmee u kunt aangeven welke berichten worden verzonden naar een onderwerp weergegeven in een bepaald onderwerp-abonnement.

De meest flexibele type filter wordt ondersteund door abonnementen is een **SqlFilter**, die een subset van de SQL92-standaard implementeert. SQL-filters worden toegepast op de eigenschappen van de berichten die worden gepubliceerd naar het onderwerp. Zie voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter, de syntaxis [SqlFilter.SqlExpression][] .

U kunt filters toevoegen aan een abonnement met behulp van de **maken\_regel** methode van het object **ServiceBusService** . Met deze methode kunt u nieuwe filters toevoegen aan een bestaand abonnement.

> [AZURE.NOTE] Omdat het standaardfilter automatisch op alle nieuwe abonnementen toegepast wordt, moet u eerst het standaardfilter te verwijderen of de **MatchAll** , heeft voorrang op andere filters die u kunt opgeven. U kunt de standaardregel verwijderen met behulp van de **verwijderen\_regel** methode van het object **ServiceBusService** .

In het volgende voorbeeld maakt u een abonnement met de naam `HighMessages` met een **SqlFilter** dat alleen berichten die groter is dan 3 **messagenumber** van de aangepaste eigenschap selecteert:

```
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Ook in het volgende voorbeeld maakt u een abonnement met de naam `LowMessages` met een **SqlFilter** die u selecteert alleen berichten met een **messagenumber** eigenschap kleiner is dan of gelijk aan 3:

```
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Nu, wanneer een bericht wordt verzonden naar `mytopic` altijd wordt bezorgd bij de ontvangers geabonneerd op het onderwerp **AllMessages** abonnement en selectief wordt geleverd met een abonnement op de **HighMessages** en **LowMessages** onderwerp abonnementen (afhankelijk van de inhoud van het bericht)-ontvangers.

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp

Als u een bericht verzendt naar een onderwerp Service Bus, uw toepassing moet gebruiken de **verzenden\_onderwerp\_bericht** methode van het object **ServiceBusService** .

In het volgende voorbeeld laat zien hoe vijf testberichten te verzenden `mytopic`. Merk op dat de waarde van de eigenschap **messagenumber** van elk bericht is afhankelijk van de herhaling van de lus (Hiermee wordt bepaald welke abonnementen ontvangen):

```
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Onderwerpen Service Bus ondersteunt een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [premie laag](service-bus-premium-messaging.md). De kop, die de van standaard- en aangepaste toepassingseigenschappen bevat, hebben een maximale grootte van 64 KB. Er is geen limiet op het aantal berichten in een onderwerp gehouden, maar er is een cap op de totale grootte van de berichten die door een onderwerp. De grootte van dit onderwerp is gedefinieerd bij het maken, met een maximum van 5 GB. Zie voor meer informatie over quota, [contingenten Service Bus][].

## <a name="receive-messages-from-a-subscription"></a>Berichten ontvangen van een abonnement

Berichten worden ontvangen van een abonnement met behulp van de **ontvangen\_abonnement\_bericht** methode voor het object **ServiceBusService** :

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Berichten van het abonnement worden verwijderd als ze worden gelezen wanneer de parameter **peek\_lock** is ingesteld op **False**. U kunt lezen (peek) en vergrendelen van het bericht zonder het te verwijderen uit de wachtrij door de parameter **peek\_lock** op **True**.

Het gedrag van lezen en het bericht verwijderen als onderdeel van het ontvangen van de eenvoud en het meest geschikt voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Om dit te begrijpen, kunt u overwegen een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze worden verwerkt. Omdat Service Bus wordt het bericht als het wordt verbruikt, gemarkeerd en vervolgens, wanneer de toepassing wordt opnieuw opgestart en begint berichten opnieuw te gebruiken, deze zal hebt gemist het bericht dat is verbruikt vóór de crash.

Als de **peek\_lock** parameter is ingesteld op **True**, wordt de ontvangen een bewerking twee fase, die het mogelijk maakt voor van ondersteuningstoepassingen die ontbrekende berichten niet kunnen tolereren. Bus-Service een aanvraag ontvangt, deze vindt u het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten ontvangen als geeft als resultaat de toepassing. Nadat de toepassing is het bericht verwerkt (of betrouwbaar voor toekomstige verwerking opgeslagen), wordt de tweede fase van de procedure ontvangen voltooid door de methode **delete** aanroept voor **het berichtobject** . De methode **delete** markeert het bericht worden gebruikt en verwijderd uit het abonnement.

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het afhandelen van toepassing crashes en berichten onleesbaar

Bus service biedt functies waarmee u zonder problemen herstellen van fouten in uw toepassing of moeilijkheden verwerken van een bericht. Als een toepassing van de ontvanger kan het bericht verwerkt voor een of andere reden, kan deze de methode **ontgrendelen** aanroepen voor het object **weergegeven** . Hierdoor wordt de Service Bus te ontgrendelen van het bericht binnen het abonnement en beschikbaar stellen aan opnieuw worden ontvangen door de toepassing in beslag nemen of door een andere toepassing voor in beslag nemen.

Er is ook een time-out is gekoppeld aan een bericht in het abonnement is vergrendeld en als de aanvraag niet verwerken het bericht voordat u de vergrendeling time-out is verstreken (bijvoorbeeld, als de toepassing loopt vast), vervolgens Bus-Service het bericht automatisch wordt ontgrendeld en beschikbaar moeten opnieuw worden ontvangen.

In het geval dat de toepassing vastloopt nadat het bericht is verwerkt, maar voordat de methode **delete** wordt aangeroepen, zal vervolgens het bericht worden geleverd aan de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **Tenminste eenmaal verwerken**dat elk bericht wordt ten minste eenmaal verwerkt maar in bepaalde situaties kan hetzelfde bericht worden geleverd. Als het scenario kan niet dubbele verwerking tolereren, kunnen ontwikkelaars van toepassingen moeten extra logica toevoegen aan hun toepassing voor aflevering van dubbele berichten verwerken. Dit wordt vaak bereikt met behulp van de eigenschap **MessageId** van het bericht over zelf constant blijven.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen

Onderwerpen abonnementen zijn permanent en moeten expliciet worden verwijderd door de [Azure portal][] of via programmering. In het volgende voorbeeld ziet u hoe te verwijderen van het onderwerp met de naam `mytopic`:

```
bus_service.delete_topic('mytopic')
```

Als u een onderwerp verwijdert verwijderd alle abonnementen die zijn geregistreerd bij het onderwerp ook. Abonnementen kunnen ook afzonderlijk worden verwijderd. De volgende code toont het verwijderen van een abonnement met de naam `HighMessages` van de `mytopic` onderwerp:

```
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de grondbeginselen van Service Bus-onderwerpen, klik op deze koppelingen voor meer informatie.

-   Zie [wachtrijen, onderwerpen, en abonnementen][].
-   Verwijzing naar [SqlFilter.SqlExpression][].

[Azure portal]: https://portal.azure.com
[Pakket Python Azure]: https://pypi.python.org/pypi/azure  
[Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Quota Service Bus]: service-bus-quotas.md 

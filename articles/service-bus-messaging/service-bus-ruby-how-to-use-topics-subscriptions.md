<properties
    pageTitle="Het gebruik van de Service Bus onderwerpen (Ruby) | Microsoft Azure"
    description="Informatie over het in Azure Service Bus onderwerpen en abonnementen gebruiken. Codevoorbeelden zijn geschreven voor Ruby-toepassingen."
    services="service-bus"
    documentationCenter="ruby"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topicssubscriptions"></a>Het gebruik van Bus onderwerpen/serviceabonnementen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In dit artikel wordt beschreven hoe Service Bus onderwerpen en abonnementen van Ruby toepassingen te gebruiken. De scenario's die zijn **maken van onderwerpen en abonnementen, maken abonnement filters, berichten verzenden** naar een onderwerp, **ontvangen van berichten van een abonnement**en **onderwerpen en abonnementen te verwijderen**. Voor meer informatie over onderwerpen en abonnementen, Zie de sectie [Volgende stappen](#next-steps) .

## <a name="service-bus-topics-and-subscriptions"></a>Service Bus onderwerpen en abonnementen

Ondersteuning voor een *publiceren/abonneren* communicatiemodel messaging service Bus onderwerpen en abonnementen. Wanneer u onderwerpen en abonnementen, communiceren onderdelen van een gedistribueerde toepassing niet met elkaar. Zij wisselen in plaats daarvan berichten via een onderwerp dat als een tussenpersoon fungeert.

![TopicConcepts](./media/service-bus-ruby-how-to-use-topics-subscriptions/sb-topics-01.png)

In tegenstelling tot Service Bus wachtrijen, waarbij elk bericht wordt verwerkt door een enkele consument, vindt onderwerpen en abonnementen u een **een-op-veel** -vorm van communicatie, met behulp van een patroon publiceren/abonneren. Het is mogelijk meerdere abonnementen naar een onderwerp te registreren. Wanneer een bericht wordt verzonden naar een onderwerp, worden deze daarna beschikbaar gesteld aan elk abonnement te verwerken, onafhankelijk van elkaar.

Een abonnement onderwerp lijkt op een virtuele wachtrij die u ontvangt een exemplaar van de berichten die zijn verzonden naar het onderwerp. Eventueel kunt u filterregels voor een onderwerp op een basis per abonnement, waarmee u kunt filteren of te beperken welke berichten naar een onderwerp worden ontvangen door welke abonnementen onderwerp registreren.

Service Bus onderwerpen en abonnementen kunnen u voor het verwerken van een groot aantal berichten over een groot aantal gebruikers en toepassingen.

## <a name="create-a-namespace"></a>Een naamruimte maken

Om te beginnen met behulp van Service Bus wachtrijen in Azure, moet u een naamruimte te maken. Een naamruimte biedt een scope-container voor het adresseren van Service Bus bronnen binnen uw toepassing. Omdat de [Azure portal][] de naamruimte met een ACS-verbinding maakt, moet u de naamruimte via de opdrachtregelinterface van maken.

Een naamruimte maken:

1. Open een console Azure Powershell venster.

2. Typ de volgende opdracht om een naamruimte te maken. De naamruimtewaarde van uw eigen opgeven en geef dezelfde regio als de toepassing.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true
    ```

    ![Namespace maken](./media/service-bus-ruby-how-to-use-topics-subscriptions/showcmdcreate.png)

## <a name="obtain-default-management-credentials-for-the-namespace"></a>Standaard referenties voor de naamruimte ophalen

Om het van beheerbewerkingen uitvoeren, zoals het maken van een wachtrij op de nieuwe naamruimte, moet u de referenties voor de naamruimte.

De PowerShell-cmdlet die werd uitgevoerd als de Service Bus-naamruimte wilt maken, wordt de sleutel die u gebruiken kunt om de naamruimte te beheren. Kopieer de waarde **DefaultKey** . U gebruikt deze waarde uw code later in deze zelfstudie.

![Kopieer sleutel](./media/service-bus-ruby-how-to-use-topics-subscriptions/defaultkey.png)

> [AZURE.NOTE]
> U vindt deze sleutel als u zich aanmeldt bij de [Azure portal][] en navigeer naar de verbindingsgegevens voor de naamruimte.

## <a name="create-a-ruby-application"></a>Een Ruby-toepassing maken

Zie [een toepassing Ruby op Azure maken](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)voor instructies.

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor gebruik Service Bus

Als u Service Bus, downloaden en u het pakket Ruby Azure bevat een set met gemak bibliotheken die met de REST opslagservices communiceren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems downloaden van het pakket te gebruiken

1. Interface met een opdrachtregel zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix) gebruiken.

2. Typ "azure gem installeren" in het opdrachtvenster de gem en afhankelijkheden wilt installeren.

### <a name="import-the-package"></a>Het pakket importeren

Met uw favoriete teksteditor, voeg de volgende boven aan de Ruby-bestand waarin u van plan bent opslag gebruiken:

```
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Een Service Bus verbinding instellen

De module Azure leest de omgevingsvariabelen **AZURE\_SERVICEBUS\_NAAMRUIMTE** en **AZURE\_SERVICEBUS\_ACCESS\_sleutel** informatie nodig voor de naamruimte. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de naamruimte-informatie voordat u **Azure::ServiceBusService** met de volgende code:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Stel de naamruimtewaarde aan de waarde die u hebt gemaakt in plaats van de volledige URL. Gebruik bijvoorbeeld **'yourexamplenamespace'**, niet 'yourexamplenamespace.servicebus.windows.net'.

## <a name="create-a-topic"></a>Een onderwerp maken

Het **Azure::ServiceBusService** -object kunt u werken met onderwerpen. De volgende code maakt een **Azure::ServiceBusService** -object. U kunt een onderwerp maken de **maken\_topic()** methode. In het volgende voorbeeld maakt een onderwerp of de fouten wordt afgedrukt als die er zijn.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

U kunt ook een **Azure::ServiceBus::Topic** met extra opties waarmee u kunt het onderwerp standaardinstellingen negeren zoals bericht naar live of Maximale wachtrijgrootte doorgeven. In het volgende voorbeeld ziet u de maximale grootte van 5GB en tijd tot op 1 minuut live instellen:

```
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Abonnementen maken

Onderwerp abonnementen worden ook gemaakt met het **Azure::ServiceBusService** -object. Abonnementen zijn naam en een optionele filter dat ervoor zorgt dat de reeks berichten die in de virtuele wachtrij van het abonnement kunnen hebben.

Abonnementen zijn permanent en blijft bestaan totdat ofwel ze of het onderwerp zijn gekoppeld, worden verwijderd. Als uw toepassing bevat de logica voor het maken van een abonnement, moet eerst controleren of het abonnement al bestaat met de methode getSubscription.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement met het standaard (MatchAll)-filter maken

Het **MatchAll** -filter is het standaard-filter die wordt gebruikt als er geen filter is opgegeven bij het maken van een nieuw abonnement. Als u het filter **MatchAll** gebruikt, worden alle berichten die zijn gepubliceerd op het onderwerp in van het abonnement virtuele wachtrij geplaatst. In het volgende voorbeeld wordt gemaakt van een abonnement met de naam 'alle berichten' en wordt het standaardfilter voor **MatchAll** .

```
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken

Ook kunt u filters waarmee u kunt aangeven welke berichten worden verzonden naar een onderwerp weergegeven in een bepaald abonnement.

De meest flexibele type filter wordt ondersteund door abonnementen is de **Azure::ServiceBus::SqlFilter**, die een subset van de SQL92-standaard implementeert. SQL-filters worden toegepast op de eigenschappen van de berichten die worden gepubliceerd naar het onderwerp. Controleer de syntaxis van de [SqlFilter.SqlExpression](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx) voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter.

U kunt filters toevoegen aan een abonnement met behulp van de **maken\_rule()** methode van het object **Azure::ServiceBusService** . Deze methode kunt u nieuwe filters toevoegen aan een bestaand abonnement.

Aangezien het standaardfilter automatisch op alle nieuwe abonnementen toegepast wordt, moet u eerst het standaardfilter te verwijderen of de **MatchAll** , heeft voorrang op andere filters die u kunt opgeven. U kunt de standaardregel verwijderen met behulp van de **verwijderen\_rule()** methode voor het **Azure::ServiceBusService** -object.

In het volgende voorbeeld maakt u een abonnement met de naam 'high-berichten' met een **Azure::ServiceBus::SqlFilter** die u selecteert alleen berichten met een aangepaste **bericht\_nummer** eigenschap groter dan 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Het volgende voorbeeld wordt op dezelfde manier een abonnement met de naam 'low-berichten' met een **Azure::ServiceBus::SqlFilter** die u selecteert alleen berichten met een **message_number** eigenschap kleiner is dan of gelijk aan 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Wanneer een bericht wordt nu verzonden naar 'test onderwerp', wordt het altijd geleverd met een abonnement op het onderwerp 'alle berichten' abonnement en selectief wordt geleverd met een abonnement op 'hoge berichten' en 'lage berichten' onderwerp abonnementen (afhankelijk van de inhoud van het bericht)-ontvangers-ontvangers.

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp

Als u een bericht verzendt naar een onderwerp Service Bus, uw toepassing moet gebruiken de **verzenden\_onderwerp\_message()** methode voor het **Azure::ServiceBusService** -object. Berichten die worden verzonden naar Service Bus onderwerpen zijn exemplaren van de **Azure::ServiceBus::BrokeredMessage** objecten. **Azure::ServiceBus::BrokeredMessage** -objecten hebben een aantal standaard eigenschappen (zoals **label** en **tijd\_aan\_live**), een woordenlijst die wordt gebruikt om specifieke eigenschappen van aangepaste toepassingen en een hoofdtekst van tekenreeksen. Een toepassing kunt instellen in de hoofdtekst van het bericht wordt doorgegeven aan een string-waarde de **verzenden\_onderwerp\_message()** methode en eventuele vereiste standaardeigenschappen wordt gevuld met de standaardwaarden.

In het volgende voorbeeld ziet u het verzenden van vijf berichten aan 'test onderwerp' testen. Merk op dat de **message_number** waarde van elk bericht is afhankelijk van de herhaling van de lus (Hiermee wordt bepaald welke abonnement ontvangt):

```
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Onderwerpen Service Bus ondersteunt een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [premie laag](service-bus-premium-messaging.md). De kop, die de van standaard- en aangepaste toepassingseigenschappen bevat, hebben een maximale grootte van 64 KB. Er is geen limiet op het aantal berichten in een onderwerp gehouden, maar er is een cap op de totale grootte van de berichten die door een onderwerp. De grootte van dit onderwerp is gedefinieerd bij het maken, met een maximum van 5 GB.

## <a name="receive-messages-from-a-subscription"></a>Berichten ontvangen van een abonnement

Berichten worden ontvangen van een abonnement met behulp van de **ontvangen\_abonnement\_message()** methode voor het **Azure::ServiceBusService** -object. Berichten zijn read(peak) en standaard vergrendeld zonder het te verwijderen uit het abonnement. U kunt lezen en verwijderen van het bericht van het abonnement door de **peek\_lock** optie in op **false**.

Het standaardgedrag kunt lezen en verwijderen van een bewerking van twee fasen, waardoor ook ter ondersteuning van toepassingen die niet kunnen ontbrekende berichten tolereren. Bus-Service een aanvraag ontvangt, deze vindt u het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten ontvangen als geeft als resultaat de toepassing. Nadat de toepassing is het bericht verwerkt (of betrouwbaar voor toekomstige verwerking opgeslagen), de tweede fase van de procedure ontvangen is voltooid door het aanroepen van **verwijderen\_abonnement\_message()** methode en het bericht moet worden verwijderd als een parameter te leveren. De **verwijderen\_abonnement\_message()** methode wordt het bericht markeren als wordt verbruikt en verwijderen uit het abonnement.

Als de **: peek\_lock** parameter is ingesteld op **false**, lezen en verwijderen van het bericht wordt de eenvoud en het meest geschikt voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Om dit te begrijpen, kunt u overwegen een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze worden verwerkt. Omdat Service Bus wordt het bericht als het wordt verbruikt, gemarkeerd en vervolgens, wanneer de toepassing wordt opnieuw opgestart en begint berichten opnieuw te gebruiken, deze zal hebt gemist het bericht dat is verbruikt vóór de crash.

In het volgende voorbeeld wordt gedemonstreerd hoe de berichten kunnen worden ontvangen en verwerkt met **ontvangen\_abonnement\_message()**. Eerst ontvangt en een bericht verwijdert uit het abonnement 'laag '-berichten via het voorbeeld **: peek\_lock** ingesteld op **false**, wordt het nog een bericht uit de "hoge berichten" ontvangt en vervolgens verwijdert u het bericht met behulp van **verwijderen\_abonnement\_message()**:

```
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="handle-application-crashes-and-unreadable-messages"></a>Vastlopende toepassingen en onleesbare berichten verwerken

Bus service biedt functies waarmee u zonder problemen herstellen van fouten in uw toepassing of moeilijkheden verwerken van een bericht. Als een toepassing van de ontvanger kan het bericht voor een of andere reden, dan kan aanroepen de **ontgrendelen\_abonnement\_message()** methode voor het **Azure::ServiceBusService** -object. Dit zorgt ervoor dat de Bus-Service het bericht binnen het abonnement te ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen door de toepassing in beslag nemen, of door een andere toepassing van de in beslag.

Er is ook een time-out is gekoppeld aan een bericht in het abonnement is vergrendeld en als de aanvraag niet verwerken het bericht voordat u de vergrendeling time-out is verstreken (bijvoorbeeld, als de toepassing loopt vast), vervolgens Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen.

In het geval dat de toepassing loopt vast na het verwerken van de berichten, maar voordat de **verwijderen\_abonnement\_message()** methode wordt aangeroepen, wordt het bericht wordt geleverd aan de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **Tenminste één keer verwerken**; elk bericht wordt ten minste eenmaal verwerkt maar in bepaalde situaties kan hetzelfde bericht worden geleverd. Als het scenario kan niet dubbele verwerking tolereren, kunnen ontwikkelaars van toepassingen moeten extra logica toevoegen aan hun toepassing voor aflevering van dubbele berichten verwerken. Deze logica wordt vaak bereikt met behulp van het **bericht\_id** eigenschap van het bericht over zelf constant blijven.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen

Onderwerpen abonnementen zijn permanent en moeten expliciet worden verwijderd door de [Azure portal][] of via programmering. In het volgende voorbeeld laat zien hoe het onderwerp met de naam 'test '-onderwerp verwijderen.

```
azure_service_bus_service.delete_topic("test-topic")
```

Als u een onderwerp verwijdert verwijderd alle abonnementen die zijn geregistreerd bij het onderwerp ook. Abonnementen kunnen ook afzonderlijk worden verwijderd. De volgende code toont hoe u het abonnement met de naam 'hoge berichten' in het onderwerp 'test '-onderwerp verwijderen:

```
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de grondbeginselen van Service Bus-onderwerpen, klik op deze koppelingen voor meer informatie.

- Zie [wachtrijen, onderwerpen, en abonnementen](service-bus-queues-topics-subscriptions.md).
- API-Naslaggids voor [SqlFilter](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx).
- Ga naar de opslagplaats [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) op GitHub.
 
[Azure portal]: https://portal.azure.com

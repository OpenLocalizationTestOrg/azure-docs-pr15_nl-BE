<properties
    pageTitle="Het gebruik van de Service Bus wachtrijen met Ruby | Microsoft Azure"
    description="Informatie over het gebruik van wachtrijen Service Bus in Azure. Voorbeelden van code geschreven in Ruby."
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

# <a name="how-to-use-service-bus-queues"></a>Het gebruik van wachtrijen Service Bus

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Deze handleiding wordt beschreven hoe u wachtrijen Service Bus. De monsters zijn geschreven in Ruby en gebruik de Azure gem. De scenario's die gedekt zijn **wachtrijen maken, verzenden en ontvangen van berichten**en **wachtrijen verwijderen**. Zie de sectie [Volgende stappen](#next-steps) voor meer informatie over Service Bus wachtrijen.

## <a name="what-are-service-bus-queues"></a>Wat zijn Service Bus wachtrijen?

Service Bus wachtrijen ondersteuning voor een *brokered messaging* communicatiemodel. Met wachtrijen communiceren onderdelen van een gedistribueerde toepassing niet met elkaar. in plaats daarvan wisselen zij berichten via een wachtrij, die als een tussenpersoon fungeert. De producent van een bericht (afzender) u een bericht naar de wachtrij en gaat vervolgens door de verwerking.
Asynchroon bericht consument (ontvanger) het bericht uit de wachtrij worden opgehaald en verwerkt deze. De producent hoeft niet te wachten op een antwoord van de consument om verder te kunnen verwerken en verdere berichten verzenden. Wachtrijen aanbieden **First In, eerste Out (FIFO)** bericht af te leveren aan een of meer concurrerende consumenten. Dat wil zeggen, worden berichten meestal ontvangen en verwerkt door de ontvangers in de volgorde waarin ze zijn toegevoegd aan de wachtrij en elk bericht is ontvangen en verwerkt door de consument slechts één bericht.

![QueueConcepts](./media/service-bus-ruby-how-to-use-queues/sb-queues-08.png)

Service Bus wachtrijen zijn een algemene technologie die kan worden gebruikt voor verschillende scenario's:

-   Communicatie tussen de website en werknemer rollen in een [meerlagige toepassing Azure](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md).
-   Communicatie tussen apps op gebouwen en Azure gehost apps in een [hybride oplossing](service-bus-dotnet-hybrid-app-using-service-bus-relay.md).
-   De communicatie tussen onderdelen van een gedistribueerde toepassing die wordt uitgevoerd op gebouwen in verschillende organisaties of van een bedrijfsafdeling.

Wachtrijen kunt u uw toepassingen beter schaalbaar en meer herstellingsvermogen voor uw architectuur inschakelen.

## <a name="create-a-namespace"></a>Een naamruimte maken

Om te beginnen met behulp van Service Bus wachtrijen in Azure, moet u een naamruimte te maken. Een naamruimte biedt een scope-container voor het adresseren van Service Bus bronnen binnen uw toepassing. Omdat de Azure portal de naamruimte met een ACS-verbinding maakt, moet u de naamruimte via de opdrachtregelinterface van maken.

Een naamruimte maken:

1. Open een console Azure Powershell.

2. Typ de volgende opdracht voor het maken van een naamruimte Service Bus. De naamruimtewaarde van uw eigen opgeven en geef dezelfde regio als de toepassing.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true

    ![Create Namespace](./media/service-bus-ruby-how-to-use-queues/showcmdcreate.png)
    ```

## <a name="obtain-management-credentials-for-the-namespace"></a>Referenties voor de naamruimte ophalen

Om het van beheerbewerkingen uitvoeren, zoals het maken van een wachtrij op de nieuwe naamruimte, moet u de referenties voor de naamruimte.

De PowerShell-cmdlet die werd uitgevoerd als de Azure service bus naamruimte wilt maken, geeft de sleutel die u gebruiken kunt om de naamruimte te beheren. Kopieer de waarde **DefaultKey** . U gebruikt deze waarde uw code later in deze zelfstudie.

![Kopieer sleutel](./media/service-bus-ruby-how-to-use-queues/defaultkey.png)

> [AZURE.NOTE] U vindt deze sleutel als u zich aanmeldt bij de [Azure portal](https://portal.azure.com/) en navigeer naar de verbindingsgegevens voor de naamruimte Service Bus.

## <a name="create-a-ruby-application"></a>Een Ruby-toepassing maken

Maak een Ruby-toepassing. Zie [een toepassing Ruby op Azure maken](/develop/ruby/tutorials/web-app-with-linux-vm/)voor instructies.

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor gebruik van de Service Bus

Azure Service Bus, downloaden en gebruiken het pakket Ruby Azure bevat een set met gemak bibliotheken die met de REST opslagservices communiceren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems downloaden van het pakket te gebruiken

1. Interface met een opdrachtregel zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix) gebruiken.

2. Typ "azure gem installeren" in het opdrachtvenster de gem en afhankelijkheden wilt installeren.

### <a name="import-the-package"></a>Het pakket importeren

Met uw favoriete teksteditor, voeg de volgende boven aan de Ruby-bestand waar u van plan bent opslag gebruiken:

```
require "azure"
```

## <a name="set-up-an-azure-service-bus-connection"></a>Een Azure Service Bus verbinding instellen

De module Azure leest de omgevingsvariabelen **AZURE\_SERVICEBUS\_NAAMRUIMTE** en **AZURE\_SERVICEBUS\_ACCESS_KEY** voor meer informatie verbinding maken met uw Service Bus naamruimte vereist. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de naamruimte-informatie voordat u **Azure::ServiceBusService** met de volgende code:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Stel de naamruimtewaarde aan de waarde die u hebt gemaakt, in plaats van de volledige URL. Gebruik bijvoorbeeld **'yourexamplenamespace'**, niet 'yourexamplenamespace.servicebus.windows.net'.

## <a name="how-to-create-a-queue"></a>Een wachtrij maken

Het **Azure::ServiceBusService** -object kunt u werken met wachtrijen. Een wachtrij wil maken, gebruikt u de methode **create_queue()** . In het volgende voorbeeld maakt een wachtrij of eventuele fouten wordt afgedrukt.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

U kunt ook een **Azure::ServiceBus::Queue** -object met extra opties waarmee u kunt overschrijven de standaard wachtrij-instellingen, zoals live tijd bericht of maximale grootte doorgeven. In het volgende voorbeeld ziet u hoe u de maximale grootte ingesteld op 5GB en tijd met live op 1 minuut:

```
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a>Het verzenden van berichten naar een wachtrij

Een bericht verzenden naar een wachtrij Service Bus u toepassing aanroepen van de **verzenden\_wachtrij\_message()** methode voor het **Azure::ServiceBusService** -object. Berichten verzonden naar (en ontvangen van) Service Bus wachtrijen zijn **Azure::ServiceBus::BrokeredMessage** objecten en hebben een aantal standaard eigenschappen (zoals **label** en **tijd\_aan\_live**), een woordenlijst die wordt gebruikt om specifieke eigenschappen van aangepaste toepassingen en een willekeurige toepassing gegevens. Een toepassing kunt instellen in de hoofdtekst van het bericht een string-waarde als het bericht wordt doorgegeven en alle vereiste eigenschappen van de standaard wordt gevuld met de standaardwaarden.

In het volgende voorbeeld wordt gedemonstreerd hoe u een testbericht verzenden naar de wachtrij met de naam "test wachtrij" met behulp van **verzenden\_wachtrij\_message()**:

```
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Service Bus wachtrijen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de van standaard- en aangepaste toepassingseigenschappen bevat, hebben een maximale grootte van 64 KB. Er is geen limiet op het aantal berichten in een wachtrij is gehouden, maar er is een cap op de totale grootte van de berichten die door een wachtrij. De grootte van de wachtrij is gedefinieerd bij het maken, met een maximum van 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Het ontvangen van berichten uit een wachtrij

Berichten worden ontvangen van een wachtrij met behulp van de **ontvangen\_wachtrij\_message()** methode voor het **Azure::ServiceBusService** -object. Standaard worden berichten lezen en vergrendeld zonder wordt verwijderd uit de wachtrij. Echter u kunt berichten uit de wachtrij verwijderen als ze worden gelezen door de **: peek_lock** optie in op **false**.

Het standaardgedrag kunt lezen en verwijderen van een bewerking van twee fasen, waardoor ook ter ondersteuning van toepassingen die niet kunnen ontbrekende berichten tolereren. Bus-Service een aanvraag ontvangt, deze vindt u het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten ontvangen als geeft als resultaat de toepassing. Nadat de toepassing is het bericht verwerkt (of betrouwbaar voor toekomstige verwerking opgeslagen), de tweede fase van de procedure ontvangen is voltooid door het aanroepen van **verwijderen\_wachtrij\_message()** methode en het bericht moet worden verwijderd als een parameter te leveren. De **verwijderen\_wachtrij\_message()** methode wordt het bericht markeren als wordt verbruikt en uit de wachtrij verwijderen.

Als de **: peek\_lock** parameter is ingesteld op **false**, lezen en verwijderen van het bericht wordt de eenvoud en het meest geschikt voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Om dit te begrijpen, kunt u overwegen een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze worden verwerkt. Omdat Service Bus wordt het bericht als het wordt gebruikt wanneer de toepassing wordt opnieuw opgestart en begint het opnieuw gebruiken van berichten hebt gemarkeerd, beschikt het hebt gemist het bericht dat is verbruikt vóór de crash.

Het volgende voorbeeld wordt het ontvangen en verwerken van berichten met behulp van **wordt\_wachtrij\_message()**. Eerst ontvangt en een bericht verwijdert met behulp van het voorbeeld **: peek\_lock** ingesteld op **false**, wordt het nog een bericht ontvangt en vervolgens verwijdert u het bericht met behulp van **verwijderen\_wachtrij\_message()**:

```
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het afhandelen van toepassing crashes en berichten onleesbaar

Bus service biedt functies waarmee u zonder problemen herstellen van fouten in uw toepassing of moeilijkheden verwerken van een bericht. Als een toepassing van de ontvanger kan het bericht voor een of andere reden, dan kan aanroepen de **ontgrendelen\_wachtrij\_message()** methode voor het **Azure::ServiceBusService** -object. Hierdoor zijn Service Bus te ontgrendelen van het bericht in de wachtrij en beschikbaar stellen aan opnieuw worden ontvangen door de toepassing in beslag nemen of door een andere toepassing voor in beslag nemen.

Er is ook een time-out is gekoppeld aan een bericht in de wachtrij is vergrendeld en als de aanvraag niet verwerken het bericht voordat u de vergrendeling time-out is verstreken (bijvoorbeeld, als de toepassing loopt vast), vervolgens Bus-Service het bericht automatisch wordt ontgrendeld en beschikbaar moeten opnieuw worden ontvangen.

In het geval dat de toepassing loopt vast na het verwerken van de berichten, maar voordat de **verwijderen\_wachtrij\_message()** methode wordt aangeroepen, wordt het bericht zal worden geleverd aan de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **Tenminste één keer verwerken**; elk bericht wordt verwerkt, ten minste één keer maar in bepaalde situaties kan hetzelfde bericht worden geleverd. Als het scenario kan niet dubbele verwerking tolereren, kunnen ontwikkelaars van toepassingen moeten extra logica toevoegen aan hun toepassing voor aflevering van dubbele berichten verwerken. Dit wordt vaak bereikt met behulp van het **bericht\_id** eigenschap van het bericht over zelf constant blijft.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de grondbeginselen van Service Bus wachtrijen, klik op deze koppelingen voor meer informatie.

-   Overzicht van [wachtrijen, onderwerpen, en abonnementen](service-bus-queues-topics-subscriptions.md).
-   Ga naar de opslagplaats [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) op GitHub.

Zie voor een vergelijking tussen de Azure Service Bus wachtrijen die worden beschreven in dit artikel en Azure wachtrijen die worden besproken in het artikel [het gebruik van opslag van Ruby wachtrij](../storage/storage-ruby-how-to-use-queue-storage.md) , [wachtrijen Azure en Azure Service Bus wachtrijen - vergeleken en Contrasted](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
 

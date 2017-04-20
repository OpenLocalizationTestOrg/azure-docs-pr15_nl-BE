<properties
    pageTitle="Het gebruik van wachtrijen Service Bus met Java | Microsoft Azure"
    description="Informatie over het gebruik van wachtrijen Service Bus in Azure. Voorbeelden van code is geschreven in Java."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Het gebruik van wachtrijen Service Bus

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Dit artikel wordt beschreven hoe u wachtrijen Service Bus. De monsters zijn geschreven in Java en de [SDK voor Java Azure][]. De scenario's die zijn **het maken van wachtrijen** **verzenden en ontvangen van berichten**en **wachtrijen verwijderen**.

## <a name="what-are-service-bus-queues"></a>Wat zijn Service Bus wachtrijen?

Service Bus wachtrijen ondersteuning voor een **brokered messaging** communicatiemodel. Wanneer u de wachtrijen, communiceren onderdelen van een gedistribueerde toepassing niet met elkaar. in plaats daarvan wisselen zij berichten via een wachtrij, die als intermediair (broker fungeert). De producent van een bericht (afzender) u een bericht naar de wachtrij en gaat vervolgens door de verwerking.
Asynchroon bericht consument (ontvanger) het bericht uit de wachtrij worden opgehaald en verwerkt deze. De producent hoeft niet te wachten op een antwoord van de consument om verder te kunnen verwerken en verdere berichten verzenden. Wachtrijen aanbieden **First In, eerste Out (FIFO)** bericht af te leveren aan een of meer concurrerende consumenten. Dat wil zeggen, worden berichten meestal ontvangen en verwerkt door de ontvangers in de volgorde waarin ze zijn toegevoegd aan de wachtrij en elk bericht is ontvangen en verwerkt door de consument slechts één bericht.

![QueueConcepts](./media/service-bus-java-how-to-use-queues/sb-queues-08.png)

Service Bus wachtrijen zijn een algemene technologie die kan worden gebruikt voor verschillende scenario's:

- Communicatie tussen de website en werknemer rollen in een meerlagige toepassing Azure.
- Communicatie tussen apps op gebouwen en Azure gehost apps in een hybride oplossing.
- De communicatie tussen onderdelen van een gedistribueerde toepassing die wordt uitgevoerd op gebouwen in verschillende organisaties of van een bedrijfsafdeling.

Met wachtrijen kunt u schaalbare toepassingen eenvoudiger en tolerantie in uw architectuur inschakelen.

## <a name="create-a-service-namespace"></a>Een Servicenaamruimte maken

Om te beginnen met behulp van Service Bus wachtrijen in Azure, moet u een naamruimte te maken. Een naamruimte biedt een scope-container voor het adresseren van Service Bus bronnen binnen uw toepassing.

Een naamruimte maken:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor gebruik van de Service Bus

Zorg ervoor dat u de [Azure SDK for Java][] hebt geïnstalleerd voordat u dit voorbeeld bouwt. Als u Eclips gebruikt, kunt u de [Toolkit voor Eclips Azure][] waarin de Azure SDK voor Java installeren. Vervolgens kunt u de **Microsoft Azure Libraries voor Java** toevoegen aan uw project:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Voeg de volgende `import` instructies aan de bovenkant van het Java-bestand:

```
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Een wachtrij maken

Beheerbewerkingen voor Service Bus wachtrijen kunnen worden uitgevoerd via de klasse **ServiceBusContract** . Een **ServiceBusContract** -object is gemaakt met een juiste configuratie die het token SAS met machtigingen kapselt voor het beheren en de klasse **ServiceBusContract** is het enige punt van communicatie met Azure.

De klasse **ServiceBusService** bevat methoden voor het maken, opsommen en verwijderen van wachtrijen. In het volgende voorbeeld ziet u hoe een **ServiceBusService** -object kan worden gebruikt voor het maken van een wachtrij met de naam 'TestQueue', met een naamruimte met de naam 'HowToSample':

```
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Er zijn methoden voor **QueueInfo** waarmee de eigenschappen van de wachtrij moet worden afgestemd (bijvoorbeeld: voor het instellen van de time-to-live (TTL) standaardwaarde moet worden toegepast op berichten die worden verzonden naar de wachtrij). In het volgende voorbeeld ziet u hoe een wachtrij met de naam `TestQueue` met een maximale grootte van 5GB:

````
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Houd er rekening mee dat kunt u de methode **listQueues** op **ServiceBusContract** -objecten om te controleren of een wachtrij met een opgegeven naam al in de Servicenaamruimte van een bestaat.

## <a name="send-messages-to-a-queue"></a>Berichten naar een wachtrij verzenden

Als u een bericht verzendt naar een wachtrij Service Bus, beheersbaarheid voor uw toepassing een **ServiceBusContract** -object. De volgende code toont hoe u een bericht te verzenden voor de `TestQueue` wachtrij hebt gemaakt de `HowToSample` naamruimte:

```
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Berichten worden verzonden naar en ontvangen van Service Bus wachtrijen zijn instanties van de klasse [BrokeredMessage][] . [BrokeredMessage][] -objecten hebben een aantal standaard eigenschappen (zoals [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) en [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), een woordenlijst die wordt gebruikt om specifieke eigenschappen van aangepaste toepassingen en een willekeurige toepassing gegevens. Een toepassing kunt instellen in de hoofdtekst van het bericht een geserialiseerd object wordt doorgegeven aan de constructor van de [BrokeredMessage][]en de juiste serializer wordt vervolgens gebruikt voor het serialiseren van het object. U kunt ook een java **bieden. I/O. InputStream** object.

In het volgende voorbeeld laat zien hoe vijf testberichten te verzenden de `TestQueue` **MessageSender** we in de vorige codefragment verkregen:

```
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

Service Bus wachtrijen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de van standaard- en aangepaste toepassingseigenschappen bevat, hebben een maximale grootte van 64 KB. Er is geen limiet op het aantal berichten in een wachtrij is gehouden, maar er is een cap op de totale grootte van de berichten die door een wachtrij. De grootte van de wachtrij is gedefinieerd bij het maken, met een maximum van 5 GB.

## <a name="receive-messages-from-a-queue"></a>Berichten uit een wachtrij

De belangrijkste manier voor het ontvangen van berichten uit een wachtrij is een **ServiceBusContract** -object te gebruiken. Ontvangen berichten kunnen in twee verschillende modi werken: **ReceiveAndDelete** en **PeekLock**.

Wanneer de modus **ReceiveAndDelete** wordt één single-shot - dat wil zeggen, wanneer Service Bus een leesbevestiging voor een bericht in een wachtrij ontvangt, wordt gemarkeerd als wordt verbruikt en terug naar de toepassing. **ReceiveAndDelete** modus (dit is de standaardmodus) is de eenvoudigste en meest geschikt voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Om dit te begrijpen, kunt u overwegen een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze worden verwerkt.
Omdat Service Bus wordt het bericht als het wordt verbruikt, gemarkeerd en vervolgens, wanneer de toepassing wordt opnieuw opgestart en begint berichten opnieuw te gebruiken, deze zal hebt gemist het bericht dat is verbruikt vóór de crash.

Ontvangen in de **PeekLock** modus wordt een bewerking twee fase, die het mogelijk maakt voor van ondersteuningstoepassingen die ontbrekende berichten niet kunnen tolereren. Bus-Service een aanvraag ontvangt, deze vindt u het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten ontvangen als geeft als resultaat de toepassing. Nadat de toepassing is het bericht verwerkt (of betrouwbaar voor toekomstige verwerking opgeslagen), wordt de tweede fase van de procedure ontvangen door het aanroepen van **verwijderen** voor het ontvangen bericht voltooid. Wanneer Service Bus het gesprek **verwijderen** , wordt het markeren van het bericht worden gebruikt en uit de wachtrij verwijderen.

In het volgende voorbeeld wordt gedemonstreerd hoe berichten kunnen worden ontvangen en verwerkt met behulp van **PeekLock** -modus (niet de standaardmodus). In het volgende voorbeeld wordt een oneindige lus en berichten verwerkt die aankomen in onze 'TestQueue':

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het afhandelen van toepassing crashes en berichten onleesbaar

Bus service biedt functies waarmee u zonder problemen herstellen van fouten in uw toepassing of moeilijkheden verwerken van een bericht. Als een ontvanger het bericht verwerkt voor een of andere reden niet is, kan deze de methode **unlockMessage** aanroepen voor het ontvangen bericht (in plaats van de methode **deleteMessage** ). Hierdoor wordt de Service Bus te ontgrendelen van het bericht in de wachtrij en beschikbaar stellen aan opnieuw worden ontvangen door de toepassing in beslag nemen of door een andere toepassing voor in beslag nemen.

Er is ook een time-out is gekoppeld aan een bericht in de wachtrij is vergrendeld en als de aanvraag niet verwerken het bericht voordat u de vergrendeling time-out is verstreken (bijv. Als de toepassing loopt vast), vervolgens Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen.

In het geval dat de toepassing vastloopt nadat het bericht is verwerkt, maar voordat de **deleteMessage** -aanvraag wordt ingediend, zal vervolgens het bericht worden geleverd aan de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **Tenminste eenmaal verwerken**dat elk bericht wordt ten minste eenmaal verwerkt maar in bepaalde situaties kan hetzelfde bericht worden geleverd. Als het scenario kan niet dubbele verwerking tolereren, kunnen ontwikkelaars van toepassingen moeten extra logica toevoegen aan hun toepassing voor aflevering van dubbele berichten verwerken. Dit wordt vaak bereikt met behulp van de methode **getMessageId** van het bericht over zelf constant blijven.

## <a name="next-steps"></a>Volgende stappen

Zie u hebt geleerd dat de grondbeginselen van Service Bus wachtrijen, [wachtrijen, onderwerpen, en abonnementen][] voor meer informatie.

Zie [Java Developer Center](/develop/java/)voor meer informatie.


  [Azure SDK voor Java]: http://azure.microsoft.com/develop/java/
  [Azure Toolkit voor Eclips]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx


<properties
    pageTitle="Het gebruik van de Service Bus onderwerpen met Java | Microsoft Azure"
    description="Informatie over het in Azure Service Bus onderwerpen en abonnementen gebruiken. Codevoorbeelden zijn geschreven voor Java-toepassingen."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Het gebruik van de Service Bus onderwerpen en abonnementen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Deze handleiding wordt beschreven hoe u Service Bus onderwerpen en abonnementen. De monsters zijn geschreven in Java en de [SDK voor Java Azure][]. De scenario's die zijn **onderwerpen en abonnementen maken** **abonnement filters maken**, **berichten verzenden naar een onderwerp**, **ontvangen van berichten van een abonnement**en **onderwerpen en abonnementen verwijderen**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Wat zijn Service Bus onderwerpen en abonnementen?

Ondersteuning voor een *publiceren/abonneren* communicatiemodel messaging service Bus onderwerpen en abonnementen. Wanneer u onderwerpen en abonnementen, communiceren onderdelen van een gedistribueerde toepassing niet met elkaar. in plaats daarvan wisselen zij berichten via een onderwerp dat als een tussenpersoon fungeert.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

In tegenstelling tot Service Bus wachtrijen, waarin elk bericht wordt verwerkt door een enkele consument vindt onderwerpen en abonnementen u een 'één-op-velen' vorm van communicatie, met behulp van een patroon publiceren/abonneren. Het is mogelijk meerdere abonnementen naar een onderwerp te registreren. Wanneer een bericht wordt verzonden naar een onderwerp, worden deze daarna beschikbaar gesteld aan elk abonnement te verwerken/proces afzonderlijk.

Een abonnement op een onderwerp lijkt op een virtuele wachtrij die u ontvangt een exemplaar van de berichten die zijn verzonden naar het onderwerp. Eventueel kunt u filterregels voor een onderwerp op een basis per abonnement, kunt u filteren of te beperken welke berichten naar een onderwerp worden ontvangen door welke abonnementen onderwerp registreren.

Service Bus onderwerpen en abonnementen kunnen u voor een zeer groot aantal berichten over een zeer groot aantal gebruikers en toepassingen te verwerken.

## <a name="create-a-service-namespace"></a>Een Servicenaamruimte maken

Om te beginnen met Service Bus onderwerpen en abonnementen in Azure, moet u de naamruimte van een service te maken. Een naamruimte biedt een scope-container voor het adresseren van Service Bus bronnen binnen uw toepassing.

Een naamruimte maken:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor gebruik van de Service Bus

Zorg ervoor dat u de [Azure SDK for Java][] hebt geïnstalleerd voordat u dit voorbeeld bouwt. Als u Eclips gebruikt, kunt u de [Toolkit voor Eclips Azure][] waarin de Azure SDK voor Java installeren. Vervolgens kunt u de **Microsoft Azure Libraries voor Java** toevoegen aan uw project:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

De volgende importinstructies aan de bovenkant van het Java-bestand toevoegen:

```
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

De Azure Libraries voor Java toevoegen aan uw pad maken en opnemen in uw project implementatie assembly.

## <a name="create-a-topic"></a>Een onderwerp maken

Beheerbewerkingen voor Service Bus onderwerpen kunnen worden uitgevoerd via de klasse **ServiceBusContract** . Een **ServiceBusContract** -object is gemaakt met een juiste configuratie die het token SAS met machtigingen kapselt voor het beheren en de klasse **ServiceBusContract** is het enige punt van communicatie met Azure.

De klasse **ServiceBusService** bevat methoden voor het maken, opsommen en verwijderen van onderwerpen. In het volgende voorbeeld ziet u hoe een **ServiceBusService** -object kan worden gebruikt voor het maken van een onderwerp met de naam `TestTopic`, met een naamruimte genoemd `HowToSample`:

    Configuration config =
        ServiceBusConfiguration.configureWithSASAuthentication(
          "HowToSample",
          "RootManageSharedAccessKey",
          "SAS_key_value",
          ".servicebus.windows.net"
          );

    ServiceBusContract service = ServiceBusService.create(config);
    TopicInfo topicInfo = new TopicInfo("TestTopic");
    try  
    {
        CreateTopicResult result = service.createTopic(topicInfo);
    }
    catch (ServiceException e) {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Er zijn methoden voor **TopicInfo** waarmee de eigenschappen van het onderwerp moet worden ingesteld (bijvoorbeeld: voor het instellen van de time-to-live (TTL) standaardwaarde moet worden toegepast op berichten die worden verzonden naar het onderwerp). In het volgende voorbeeld ziet u het maken van een onderwerp met de naam `TestTopic` met een maximale grootte van 5 GB:

    long maxSizeInMegabytes = 5120;  
    TopicInfo topicInfo = new TopicInfo("TestTopic");  
    topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateTopicResult result = service.createTopic(topicInfo);

Houd er rekening mee dat kunt u de methode **listTopics** op **ServiceBusContract** -objecten om te controleren of een onderwerp met een opgegeven naam al in de Servicenaamruimte van een bestaat.

## <a name="create-subscriptions"></a>Abonnementen maken

Abonnementen op onderwerpen worden ook gemaakt met de klasse **ServiceBusService** . Abonnementen worden benoemd en een optionele filter dat ervoor zorgt dat de reeks berichten die worden doorgegeven aan de virtuele wachtrij van het abonnement kunnen hebben.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement met het standaard (MatchAll)-filter maken

Het **MatchAll** -filter is het standaard-filter die wordt gebruikt als er geen filter is opgegeven bij het maken van een nieuw abonnement. Als u het filter **MatchAll** gebruikt, worden alle berichten die zijn gepubliceerd op het onderwerp in van het abonnement virtuele wachtrij geplaatst. In het volgende voorbeeld maakt u een abonnement met de naam 'AllMessages' en het standaardfilter **MatchAll** gebruikt.

    SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken

U kunt ook filters waarmee u naar een bereik dat berichten worden verzonden naar een onderwerp in een bepaald onderwerp abonnement weergegeven maken.

De meest flexibele type filter wordt ondersteund door abonnementen is de [SqlFilter][], die een subset van de SQL92-standaard implementeert. SQL-filters worden toegepast op de eigenschappen van de berichten die worden gepubliceerd naar het onderwerp. Controleer de syntaxis van de [SqlFilter.SqlExpression][] voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter.

In het volgende voorbeeld maakt u een abonnement met de naam `HighMessages` met een [SqlFilter][] -object dat u selecteert alleen berichten met een aangepaste eigenschap op de **MessageNumber** groter dan 3:

```
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Ook in het volgende voorbeeld maakt u een abonnement met de naam `LowMessages` met een [SqlFilter][] -object dat u selecteert alleen berichten met een **MessageNumber** eigenschap kleiner is dan of gelijk aan 3:

```
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Wanneer nu een bericht wordt verzonden naar `TestTopic`, wordt altijd geleverd met ontvangers geabonneerd op de `AllMessages` -abonnement en selectief geleverde naar ontvangers die zijn geabonneerd op de `HighMessages` en `LowMessages` abonnementen (afhankelijk van de inhoud van het bericht).

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp

Als u een bericht verzendt naar een onderwerp Service Bus, beheersbaarheid voor uw toepassing een **ServiceBusContract** -object. De volgende code laat zien hoe een bericht te verzenden voor de `TestTopic` onderwerp eerder hebt gemaakt in de `HowToSample` naamruimte:

```
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

Berichten die worden verzonden naar Service Bus onderwerpen zijn exemplaren van de klasse [BrokeredMessage][] . [BrokeredMessage][] *-objecten hebben een set van standaardmethoden (zoals * *setLabel* * en * *TimeToLive**), een woordenlijst die wordt gebruikt om aangepaste toepassing-specifieke eigenschappen en een willekeurige toepassing gegevens. Een toepassing kunt instellen in de hoofdtekst van het bericht een geserialiseerd object wordt doorgegeven aan de constructor van de [BrokeredMessage][]en de juiste * *DataContractSerializer* * wordt vervolgens gebruikt voor het serialiseren van het object. U kunt ook een * *java.io.InputStream** kan worden geleverd.

In het volgende voorbeeld laat zien hoe vijf testberichten te verzenden de `TestTopic` **MessageSender** we in het bovenstaande codefragment verkregen.
U ziet hoe de waarde van de eigenschap **MessageNumber** van elk bericht is afhankelijk van de herhaling van de lus (dit veld bepaalt welke abonnementen ontvangen):

```
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

Onderwerpen Service Bus ondersteunt een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [premie laag](service-bus-premium-messaging.md). De kop, die de van standaard- en aangepaste toepassingseigenschappen bevat, hebben een maximale grootte van 64 KB. Er is geen limiet op het aantal berichten in een onderwerp gehouden, maar er is een cap op de totale grootte van de berichten die door een onderwerp. De grootte van dit onderwerp is gedefinieerd bij het maken, met een maximum van 5 GB.

## <a name="how-to-receive-messages-from-a-subscription"></a>Het ontvangen van berichten van een abonnement

Om berichten te ontvangen van een abonnement, een **ServiceBusContract** -object te gebruiken. Ontvangen berichten kunnen in twee verschillende modi werken: **ReceiveAndDelete** en **PeekLock**.

Wanneer de modus **ReceiveAndDelete** wordt één single-shot - dat wil zeggen, wanneer Service Bus een leesbevestiging voor een bericht ontvangt, wordt gemarkeerd als wordt verbruikt en terug naar de toepassing. **ReceiveAndDelete** modus is de eenvoudigste en meest geschikt voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Om dit te begrijpen, kunt u overwegen een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze worden verwerkt. Omdat Service Bus wordt het bericht als het wordt verbruikt, gemarkeerd en vervolgens, wanneer de toepassing wordt opnieuw opgestart en begint berichten opnieuw te gebruiken, deze zal hebt gemist het bericht dat is verbruikt vóór de crash.

Ontvangen in de **PeekLock** modus wordt een bewerking twee fase, die het mogelijk maakt voor van ondersteuningstoepassingen die ontbrekende berichten niet kunnen tolereren. Bus-Service een aanvraag ontvangt, deze vindt u het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten ontvangen als geeft als resultaat de toepassing. Nadat de toepassing is het bericht verwerkt (of betrouwbaar voor toekomstige verwerking opgeslagen), wordt de tweede fase van de procedure ontvangen door het aanroepen van **verwijderen** voor het ontvangen bericht voltooid. Wanneer Service Bus het gesprek **verwijderen** , wordt het markeren van het bericht worden gebruikt en verwijderen uit het onderwerp.

In het volgende voorbeeld wordt gedemonstreerd hoe berichten kunnen worden ontvangen en verwerkt met behulp van **PeekLock** -modus (niet de standaardmodus). In het volgende voorbeeld wordt een lus uitgevoerd en verwerkt berichten in het abonnement 'HighMessages' en vervolgens afgesloten als er geen berichten meer (ook deze kan worden ingesteld om te wachten op nieuwe berichten).

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the topic message.
            System.out.print("From topic: ");
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
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
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

Bus service biedt functies waarmee u zonder problemen herstellen van fouten in uw toepassing of moeilijkheden verwerken van een bericht. Als een ontvanger het bericht verwerkt voor een of andere reden niet is, kan deze de methode **unlockMessage** aanroepen voor het ontvangen bericht (in plaats van de methode **deleteMessage** ). Hierdoor wordt de Service Bus te ontgrendelen van het bericht in het onderwerp en beschikbaar stellen aan opnieuw worden ontvangen door de toepassing in beslag nemen of door een andere toepassing voor in beslag nemen.

Er is ook een time-out is gekoppeld aan een bericht vergrendeld in het onderwerp en als de aanvraag niet verwerken het bericht voordat u de vergrendeling time-out is verstreken (bijvoorbeeld, als de toepassing loopt vast), vervolgens Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen.

In het geval dat de toepassing vastloopt nadat het bericht is verwerkt, maar voordat de **deleteMessage** -aanvraag wordt ingediend, zal vervolgens het bericht worden geleverd aan de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd **Tenminste eenmaal verwerken**dat elk bericht wordt ten minste eenmaal verwerkt maar in bepaalde situaties kan hetzelfde bericht worden geleverd. Als het scenario kan niet dubbele verwerking tolereren, kunnen ontwikkelaars van toepassingen moeten extra logica toevoegen aan hun toepassing voor aflevering van dubbele berichten verwerken. Dit wordt vaak bereikt met behulp van de methode **getMessageId** van het bericht over zelf constant blijven.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen

De belangrijkste manier om onderwerpen en abonnementen te verwijderen is een **ServiceBusContract** -object te gebruiken. Een onderwerp worden ook verwijdert, alle abonnementen die zijn geregistreerd bij het onderwerp. Abonnementen kunnen ook afzonderlijk worden verwijderd.

```
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Volgende stappen

Zie u hebt geleerd dat de grondbeginselen van Service Bus wachtrijen, [wachtrijen Service Bus, onderwerpen, en abonnementen][] voor meer informatie.

  [Azure SDK voor Java]: http://azure.microsoft.com/develop/java/
  [Azure Toolkit voor Eclips]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Azure classic portal]: http://manage.windowsazure.com/
  [Service Bus wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx 
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  
  [0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
  [2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
  [3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png

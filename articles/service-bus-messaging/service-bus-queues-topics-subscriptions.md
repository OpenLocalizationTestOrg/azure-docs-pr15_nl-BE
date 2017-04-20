<properties 
    pageTitle="Service Bus wachtrijen, onderwerpen en abonnementen | Microsoft Azure"
    description="Overzicht van entiteiten messaging Service-Bus."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/14/2016"
    ms.author="sethm" />

# <a name="service-bus-queues-topics-and-subscriptions"></a>Service Bus wachtrijen, onderwerpen en abonnementen

Microsoft Azure Service Bus ondersteunt een reeks cloud-gebaseerde message-oriented middleware technologieën met inbegrip van betrouwbare message queuing en duurzame publiceren/abonneren messaging. Deze brokered messaging mogelijkheden kunnen worden beschouwd als een losgekoppelde messaging-functies die ondersteuning publicatie abonnementen, tijdelijke ontkoppeling en scenario's de fabric messaging Service-Bus met taakverdeling. Losgekoppelde communicatie heeft veel voordelen; clients en servers kunnen bijvoorbeeld verbinding maken als nodig is en bewerkingen op asynchrone wijze.

De messaging entiteiten die de kern van de brokered messaging-mogelijkheden in de Bus-Service vormen worden wachtrijen en onderwerpen abonnementen/regels/acties.

## <a name="queues"></a>Wachtrijen

Wachtrijen bieden First In, eerste Out (FIFO) berichtlevering aan een of meer concurrerende consumenten. Dat wil zeggen berichten meestal naar verwachting worden ontvangen en verwerkt door de ontvangers in de volgorde waarin ze zijn toegevoegd aan de wachtrij en elk bericht is ontvangen en verwerkt door de consument slechts één bericht. Een belangrijk voordeel van het gebruik van wachtrijen is te bereiken «tijdelijke ontkoppeling» van onderdelen. Met andere woorden, hoeft de (afzenders) producenten en consumenten (ontvangers) niet te worden, verzenden en ontvangen van berichten op hetzelfde moment, omdat berichten blijvend worden opgeslagen in de wachtrij. De producent hoeft bovendien niet te wachten op een antwoord van de consument om verder te kunnen verwerken en verzenden van berichten.

Een verwante vergoeding is 'laden herverdeling' waarmee de producenten en consumenten te verzenden en ontvangen van berichten met verschillende snelheden. In veel toepassingen varieert de systeembelasting in de tijd; de verwerkingstijd voor elke eenheid van het werk is echter meestal constant. Intermediating bericht producenten en consumenten met een wachtrij betekent dit dat de toepassing in beslag nemen alleen om de gemiddelde belasting in plaats van piekbelasting verwerkt te kunnen worden ingericht. De diepte van de wachtrij groeit en contracten van de binnenkomende belastingstoestand. Dit bespaart u direct geld op het gebied van de infrastructuur die nodig zijn voor de belasting van toepassing. Als de belasting toeneemt, kunnen er meer werkprocessen te lezen uit de wachtrij worden toegevoegd. Elk bericht wordt verwerkt door slechts één van de werkprocessen. Bovendien maakt deze pull-based taakverdeling optimaal gebruik van de computers van de werknemer zelfs als de werknemer computers met betrekking tot de verwerkingskracht verschillen, als ze hun maximale snelheid berichten haalt. Dit patroon wordt vaak aangeduid als het patroon "concurrentie consument".

Intermediair tussen bericht producenten en consumenten wachtrijen biedt een inherente losse koppeling tussen de onderdelen. Omdat de producenten en consumenten zich niet bewust zijn van elkaar, kan een consument zonder gevolgen voor de producent worden bijgewerkt.

Een wachtrij maken is een proces met meerdere stappen. U uitvoeren beheerbewerkingen voor Service Bus entiteiten (wachtrijen en onderwerpen) berichten via de klasse [Microsoft.ServiceBus.NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , die is samengesteld door het opgeven van het basisadres van de naamruimte Service Bus en referenties van de gebruiker. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) biedt methoden voor het maken, opsommen en verwijderen van messaging entiteiten. Na het maken van een [Microsoft.ServiceBus.TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) -object uit de naam van de SA's en sleutel en een service namespace-object management, kunt u de methode [Microsoft.ServiceBus.NamespaceManager.CreateQueue](https://msdn.microsoft.com/library/azure/hh293157.aspx) om de wachtrij te maken. Bijvoorbeeld:

```
// Create management credentials
TokenProvider credentials = TokenProvider. CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

Vervolgens kunt u maken een wachtrij-object en een messaging fabriek met de Service Bus URI als argument. Bijvoorbeeld:

```
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

Vervolgens kunt u berichten verzenden naar de wachtrij. Bijvoorbeeld, als u een lijst met brokered berichten genoemd `MessageList`, de code lijkt op het volgende:

```
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

U kunt vervolgens ontvangen berichten uit de wachtrij, als volgt:

```
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

In de modus [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) wordt de ontvangstbewerking single-shot; dat wil zeggen, wanneer Bus-Service het verzoek ontvangt, het markeren van het bericht worden gebruikt en terug naar de toepassing. [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) modus is de eenvoudigste en meest geschikt voor scenario's waarin de toepassing kan tolereren niet verwerken van een bericht bij een storing. Om dit te begrijpen, kunt u overwegen een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze worden verwerkt. Omdat Service Bus is gemarkeerd als wordt gebruikt wanneer de toepassing wordt opnieuw opgestart en begint het opnieuw gebruiken van berichten, beschikt het hebt gemist het bericht dat is verbruikt vóór de crash.

In de [PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) modus wordt de ontvangstbewerking twee fasen, waardoor de ondersteuning voor toepassingen die ontbrekende berichten niet kan tolereren. Wanneer Bus-Service het verzoek ontvangt, gezocht het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten ontvangen en stuurt het naar de toepassing. Nadat de toepassing is het bericht verwerkt (of betrouwbaar voor toekomstige verwerking opgeslagen), wordt de tweede fase van de procedure ontvangen door het aanroepen van [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) voor het ontvangen bericht voltooid. Wanneer Service Bus [volledig](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) aanroept, worden gemarkeerd als wordt verbruikt.

Als de toepassing niet kan het bericht verwerkt voor een of andere reden, kan de methode [Abandon](https://msdn.microsoft.com/library/azure/hh181837.aspx) aanroepen op het ontvangen bericht ( [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)). Hierdoor zijn Service Bus te ontgrendelen van het bericht en moet opnieuw worden ontvangen door de consument dezelfde of een andere concurrerende consument beschikbaar. In de tweede plaats is er een time-out van de vergrendeling en als de toepassing niet wordt het bericht verwerkt voordat de vergrendeling time-out is verstreken (bijvoorbeeld, als de toepassing loopt vast) en vervolgens Service Bus ontgrendelt u het bericht en beschikbaar moeten opnieuw worden ontvangen (er in feite een bewerking [Annuleren](https://msdn.microsoft.com/library/azure/hh181837.aspx) standaard).

Houd er rekening mee dat in het geval dat de toepassing vastloopt nadat het bericht is verwerkt, maar voordat de aanvraag [volledig](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) is afgegeven, het bericht wordt geleverd aan de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd *Tenminste eenmaal *verwerking; dat wil zeggen wordt elk bericht ten minste eenmaal verwerkt. In bepaalde situaties kan hetzelfde bericht worden geleverd. Als u het scenario kan niet zonder dubbele verwerking wordt extra logica is vereist in de toepassing voor het detecteren van dubbele records die kunnen worden bereikt op basis van de eigenschap **MessageId** van het bericht over zelf constant blijft. Dit staat bekend als *Slechts één keer* verwerken.

Zie de [Service Bus brokered .NET zelfstudie messaging](service-bus-brokered-tutorial-dotnet.md)voor meer informatie en een werkend voorbeeld van het maken en verzenden van berichten naar en uit de wachtrij.

## <a name="topics-and-subscriptions"></a>Onderwerpen en abonnementen

In tegenstelling tot wachtrijen, waarin elk bericht wordt verwerkt door een enkele consument vindt *onderwerpen* en *abonnementen* u een een-op-veel-formulier van communicatie in een patroon *publiceren/abonneren* . Handig voor een zeer groot aantal geadresseerden schalen, elke gepubliceerde bericht beschikbaar wordt gemaakt voor elk abonnement dat is geregistreerd bij het onderwerp. Berichten worden verzonden naar een onderwerp en geleverd aan een of meer bijbehorende abonnementen, afhankelijk van de filterregels die kunnen worden ingesteld op basis van per abonnement. De abonnementen kunnen u extra filters gebruiken voor het beperken van de berichten die ze willen ontvangen. Berichten worden verzonden naar een onderwerp op dezelfde manier als ze naar een wachtrij worden verzonden, maar de berichten worden niet ontvangen van het onderwerp rechtstreeks. Ze worden ontvangen van abonnementen. Een abonnement onderwerp lijkt op een virtuele wachtrij die u ontvangt een exemplaar van de berichten die worden verzonden naar het onderwerp. Berichten worden ontvangen van een abonnement identiek aan de manier waarop die ze worden ontvangen van een wachtrij.

Ter vergelijking, de functionaliteit van een wachtrij voor bericht verzenden direct aan een onderwerp gekoppeld en de functionaliteit van het bericht ontvangen wordt toegewezen aan een abonnement. Onder andere, dit betekent dat abonnementen dezelfde patronen die eerder zijn beschreven in deze sectie voor wachtrijen ondersteunen: concurrerende consument, tijdelijke ontkoppeling laden herverdeling en taakverdeling.

Een onderwerp maken is vergelijkbaar met het maken van een wachtrij, zoals in het voorbeeld in de vorige sectie. De URI-service maken en maakt u met de klasse [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) de naamruimte-client. Vervolgens maakt u een onderwerp met de methode [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) . Bijvoorbeeld:

```
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

Voeg vervolgens abonnementen naar wens:

```
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

Vervolgens maakt u een onderwerp-client. Bijvoorbeeld:

```
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

Met behulp van de afzender van het bericht kunt u verzenden en ontvangen van berichten van en naar het onderwerp, zoals in de vorige sectie. Bijvoorbeeld:

```
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

Evenals wachtrijen berichten worden ontvangen van een abonnement met een [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) -object in plaats van een [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) -object. Maak de abonnement-client de naam van het onderwerp, de naam van het abonnement en (optioneel) op de modus ontvangen als parameters doorgeven. Bijvoorbeeld met het abonnement op **voorraad** :

```
// Create the subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### <a name="rules-and-actions"></a>Regels en acties

In veel gevallen moeten de berichten met specifieke kenmerken op verschillende manieren worden verwerkt. U kunt abonnementen om te zoeken naar berichten die u hebt de gewenste eigenschappen en voert u bepaalde wijzigingen aanbrengen in de eigenschappen configureren om dit mogelijk. Terwijl abonnementen Service Bus alle berichten die naar het onderwerp ziet, kunt u alleen een subset van deze berichten kopiëren naar de wachtrij virtuele abonnement. Dit wordt bereikt met behulp van filters abonnement. Dergelijke wijzigingen worden genoemd van *filteracties*. Wanneer een abonnement wordt gemaakt, kunt u opgeven met een expressie die wordt toegepast op de eigenschappen van het bericht, de eigenschappen van het bestandssysteem (bijvoorbeeld **Label**) en de eigenschappen van aangepaste toepassingen (bijvoorbeeld **StoreName**.) De filterexpressie SQL is optioneel in dit geval; zonder een filter SQL-expressie, wordt een filteractie die is gedefinieerd op een abonnement uitgevoerd op alle berichten voor dit abonnement.

Het vorige voorbeeld, om berichten te filteren die afkomstig zijn van **Store1**, alleen maakt u het abonnement Dashboard als volgt:

```
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

Met dit filter abonnement in plaats van alleen de berichten met de `StoreName` de eigenschap ingesteld op `Store1` worden gekopieerd naar de virtuele wachtrij voor de `Dashboard` abonnement.

Raadpleeg de documentatie bij de klassen [SqlFilter](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx) en [SqlRuleAction](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlruleaction.aspx) voor meer informatie over mogelijke filterwaarden. Zie ook de [Brokered Messaging: geavanceerde Filters](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) en monsters [Onderwerp Filters](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters) .

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie en voorbeelden van het gebruik van de Service Bus brokered messaging entiteiten geavanceerde.

- [Messaging service Bus-overzicht](service-bus-messaging-overview.md)
- [Zelfstudie voor messaging .NET Service Bus brokered](service-bus-brokered-tutorial-dotnet.md)
- [Service Bus brokered messaging REST zelfstudie](service-bus-brokered-tutorial-rest.md)
- [Onderwerp Filters monster](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters)
- [Brokered Messaging: Geavanceerde Filters monster](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)


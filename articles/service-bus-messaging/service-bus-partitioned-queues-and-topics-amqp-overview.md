<properties 
    pageTitle="AMQP 1.0-ondersteuning voor Service Bus worden gepartitioneerd wachtrijen en onderwerpen | Microsoft Azure" 
    description="Informatie over het gebruik van de geavanceerde Message Queuing-Protocol (AMQP) 1.0 met Service Bus gepartitioneerd wachtrijen en onderwerpen." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="hillaryc" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="hillaryc;sethm"/>

# <a name="amqp-10-support-for-service-bus-partitioned-queues-and-topics"></a>AMQP 1.0-ondersteuning voor Service Bus worden gepartitioneerd wachtrijen en onderwerpen 

Azure Service Bus biedt nu ondersteuning voor de geavanceerde Message Queuing-Protocol (**AMQP**) 1.0 voor Service Bus **wachtrijen gepartitioneerd en onderwerpen.**

**AMQP** is een open standaard message Queuing-protocol waarmee u voor het ontwikkelen van toepassingen voor meerdere platforms met verschillende programmeertalen. Voor algemene informatie over AMQP in Bus Service ondersteunen, Zie [AMQP 1.0 in Bus Service ondersteunen](service-bus-amqp-overview.md).

**Partitioned-wachtrijen en onderwerpen**, ook bekend als *gepartitioneerde entiteiten*, bieden een hogere beschikbaarheid, betrouwbaarheid en doorvoer dan conventionele niet-gepartitioneerde wachtrijen en onderwerpen. Zie voor meer informatie over entiteiten gepartitioneerde [Gepartitioneerde Messaging-diensten](service-bus-partitioning.md).

De toevoeging van AMQP 1.0 als een protocol voor communicatie met de gepartitioneerde wachtrijen en onderwerpen kunt u interoperabele toepassingen maken die kunnen profiteren van de hogere mate van beschikbaarheid, betrouwbaarheid, en wordt aangeboden in de gehele Service Bus gepartitioneerd entiteiten.

Zie voor een gedetailleerde draad niveau AMQP 1.0-protocol gids, waarin wordt uitgelegd hoe de Service Bus wordt geïmplementeerd en is gebaseerd op de technische specificatie van OASIS AMQP, de [AMQP 1.0 in Azure Service Bus en Hubs gebeurtenis protocol guide](service-bus-amqp-protocol-guide.md).    

## <a name="use-amqp-with-partitioned-queues"></a>Gebruik AMQP met gepartitioneerde wachtrijen

Wachtrijen zijn handig voor scenario's waarvoor tijdelijke ontkoppeling, load herverdeling taakverdeling en losse koppeling. Met een wachtrij, uitgevers berichten naar de wachtrij verzenden en ontvangen van berichten uit de wachtrij, in situaties waarin een bericht kan alleen worden ontvangen eenmaal consumenten. Een goed voorbeeld hiervan is een systeem waarin POS-terminals gegevens naar een wachtrij in plaats van rechtstreeks naar het voorraadbeheersysteem publiceren. Het voorraadbeheersysteem neemt vervolgens de gegevens op elk gewenst moment voorraadaanvulling beheren. Dit heeft diverse voordelen, waaronder het voorraadbeheersysteem hoeven niet altijd on line zijn. Zie voor meer informatie over Service Bus wachtrijen [maken toepassingen die gebruikmaken van wachtrijen Service Bus](service-bus-create-queues.md). 

Een gepartitioneerde wachtrij verder vergroot de beschikbaarheid, betrouwbaarheid en doorvoer voor toepassingen, zoals deze wachtrijen worden gepartitioneerd voor meerdere bericht makelaars en messaging-winkels.     

### <a name="create-partitioned-queues"></a>Gepartitioneerde wachtrijen maken

U kunt een gepartitioneerde wachtrij met de [Azure klassieke portal][] en de SDK Service Bus. Als u een gepartitioneerde wachtrij, stelt de eigenschap [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) op **true** in het exemplaar van de [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx) . De volgende code ziet u hoe een gepartitioneerde wachtrij met de SDK Service Bus te maken. 
 
```
// Create partitioned queue
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var queueDescription = new QueueDescription("myQueue");
queueDescription.EnablePartitioning = true;
nm.CreateQueue(queueDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Verzenden en ontvangen van berichten met behulp van AMQP

U kunt berichten te verzenden en ontvangen van berichten uit een gepartitioneerde wachtrij AMQP als protocol, door de eigenschap [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) voor [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx) zoals in de volgende code gebruiken.  

```
// Sending and receiving messages to and from a queue
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
var queueClient = QueueClient.CreateFromConnectionString(amqpConnectionString, "myQueue");

BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
queueClient.Send(message);

var receivedMessage = queueClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="use-amqp-with-partitioned-topics"></a>Gebruik AMQP met gepartitioneerde onderwerpen

Onderwerpen zijn conceptueel vergelijkbaar met wachtrijen, maar onderwerpen kunnen een kopie van hetzelfde bericht naar meerdere *abonnementen*worden doorgestuurd. Uitgevers berichten verzenden naar het onderwerp in een onderwerp en consumenten ontvangen van abonnementen. In de voorraad systeem verkooppunt scenario publiceren terminals gegevens naar het onderwerp. Het voorraadbeheersysteem vervolgens ontvangt berichten van een abonnement. Een systeem van toezicht kan ook hetzelfde bericht ontvangen van een ander abonnement. Zie voor meer informatie over Service Bus onderwerpen en abonnementen [maken toepassingen die gebruikmaken van Service Bus onderwerpen en abonnementen](service-bus-create-topics-subscriptions.md). 

Net als bij wachtrijen, vergroten gepartitioneerde onderwerpen verder de beschikbaarheid, betrouwbaarheid en doorvoer voor toepassingen, zoals deze onderwerpen en hun abonnementen worden gepartitioneerd voor meerdere bericht makelaars en messaging-winkels 

### <a name="create-partitioned-topics"></a>Gepartitioneerde onderwerpen maken

U kunt een gepartitioneerde onderwerp met de [Azure klassieke portal][] en de SDK Service Bus. Als u een gepartitioneerde onderwerp, stelt u de eigenschap [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx) op **true** in het exemplaar van de [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) . De volgende code ziet u hoe een gepartitioneerde onderwerp met de SDK Service Bus te maken.
    
```
// Create partitioned topic
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var topicDescription = new TopicDescription("myTopic");
topicDescription.EnablePartitioning = true;
nm.CreateTopic(topicDescription);

var subscriptionDescription = new SubscriptionDescription("myTopic", "mySubscription");
nm.CreateSubscription(subscriptionDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Verzenden en ontvangen van berichten met behulp van AMQP

U kunt berichten te verzenden en ontvangen van berichten van een gepartitioneerde onderwerp abonnement AMQP als een protocol, door de eigenschap [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) voor [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx), zoals in de volgende code gebruiken.  

```
// Sending and receiving messages to a topic and from a subscription
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
    
var topicClient = TopicClient.CreateFromConnectionString(amqpConnectionString, "myTopic");
BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
topicClient.Send(message);
    
var subcriptionClient = SubscriptionClient.CreateFromConnectionString(amqpConnectionString, "myTopic", "mySubscription");
var receivedMessage = subcriptionClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende aanvullende informatie voor meer informatie over de gepartitioneerde messaging entiteiten zoals AMQP.

*    [Gepartitioneerde messaging entiteiten](service-bus-partitioning.md)
*    [OASIS geavanceerde Message Queuing-Protocol (AMQP) versie 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)
*    [Ondersteuning van AMQP 1.0 in Service Bus](service-bus-amqp-overview.md)
*    [AMQP 1.0 in Azure Service Bus en Hubs gebeurtenis protocol-handleiding](service-bus-amqp-protocol-guide.md)
*    [Het gebruik van Java Message Service (JMS) API met Service Bus en AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
*    [AMQP 1.0 gebruiken met de Service Bus .NET API](service-bus-dotnet-advanced-message-queuing.md)

[Azure klassieke portal]: http://manage.windowsazure.com

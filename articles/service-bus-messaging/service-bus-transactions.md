<properties 
    pageTitle="Bus service transacties | Microsoft Azure" 
    description="Overzicht van Azure Service Bus atomische transacties en verzenden via" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="10/04/2016"
    ms.author="clemensv;sethm"/>

# <a name="overview-of-service-bus-transaction-processing"></a>Overzicht van de transactieverwerking Service Bus

Dit artikel wordt de transactiemogelijkheden van Azure Service Bus. Veel van de discussie wordt geïllustreerd door de [Atomische transacties met monster Service Bus](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions). Dit artikel is beperkt tot een overzicht van de verwerking van transacties en *verzenden* in Service Bus, terwijl het monster atomische transacties groter en complexer in het bereik is.

## <a name="transactions-in-service-bus"></a>Transacties in Service Bus

Een [transactie](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions#what-are-transactions) worden twee of meer transacties samen in een *bereik worden uitgevoerd*. Een dergelijke transactie moet ervoor zorgen dat alle bewerkingen die behoren tot een bepaalde groep bewerkingen slagen of gezamenlijk mislukken door aard. In dit opzicht fungeren transacties als één eenheid wordt vaak *atomiciteit*genoemd. 

Service Bus is een transactioneel bericht broker en transactionele integriteit voor alle interne bewerkingen tegen haar berichtenarchieven zorgt. Alle overdrachten van berichten in de Bus-Service, zoals het verplaatsen van berichten naar een [wachtrij voor onbestelbare berichten](service-bus-dead-letter-queues.md) of [automatisch doorsturen](service-bus-auto-forwarding.md) van berichten tussen entiteiten, zijn transactioneel. Als zodanig als Service Bus een bericht accepteert, is al opgeslagen en voorzien van een volgnummer. Daarna bericht overdracht binnen Service Bus gecoördineerde bewerkingen zijn over diensten en zal niet leiden tot verlies (slaagt bron en doel mislukt) of dubbele (mislukt bron en doel slaagt) van het bericht.

Service Bus ondersteunt groepeerbewerkingen tegen messaging rechtspersoon (wachtrij, onderwerp, abonnement) binnen het bereik van een transactie. Zo kunt u meerdere berichten verzenden naar een wachtrij uit in het kader van een transactie en de berichten worden alleen vastgelegd in het logboek van de wachtrij wanneer de transactie met succes is voltooid.

## <a name="operations-within-a-transaction-scope"></a>Activiteiten binnen de scope van een transactie 

De bewerkingen die kunnen worden uitgevoerd binnen een transactiebereik zijn als volgt:

- ** [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx), [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx), [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx)**: verzenden, SendAsync, SendBatch, SendBatchAsync 

- **[BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)**: Complete, CompleteAsync, oefening, AbandonAsync, onbestelbare, DeadletterAsync, uitstellen, DeferAsync, RenewLock, RenewLockAsync 

Ontvangen bewerkingen worden niet opgenomen, omdat ervan wordt uitgegaan dat de toepassing berichten krijgt met de modus [ReceiveMode.PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) binnen enkele lus ontvangen of met een [OnMessage](https://msdn.microsoft.com/library/azure/dn369601.aspx) terugbellen en vervolgens pas wordt geopend een transactiebereik voor het verwerken van het bericht.

Vervolgens de bestemming van het bericht (voltooid, oefening, onbestelbaar bericht uitstellen) vindt plaats binnen het toepassingsgebied van, en afhankelijk van de algehele uitkomst van de transactie.

## <a name="transfers-and-send-via"></a>Overdrachten en 'verzenden '

Bus Service biedt ondersteuning voor *transfers*zodat transactionele overdracht van gegevens van een wachtrij aan een processor en aan een andere wachtrij. Een afzender verzendt een bericht eerst naar een wachtrij"overdracht" in een overdrachtsbewerking voor en de overdracht wachtrij onmiddellijk het bericht wordt verplaatst naar de bestemming van het wachtrij met dezelfde krachtige overdracht-implementatie die afhankelijk zijn van het automatisch doorsturen van berichten. Het bericht wil nooit logboek van de overdracht wachtrij zodanig dat deze zichtbaar is voor de consument van de overdracht wachtrij.

De kracht van deze transactionele functionaliteit wordt zichtbaar wanneer de overdracht wachtrij zelf de bron van invoer berichten van de afzender is. Met andere woorden, Service Bus kunt overbrengen het bericht naar de doelwachtrij "via" de overdracht wachtrij, tijdens het uitvoeren van een volledige (of uitstellen, of onbestelbare) bewerking op het invoerbericht, alles in één atomaire bewerking. 

### <a name="see-it-in-code"></a>Zie het in code

Als u dergelijke overdrachten instelt, maakt u een afzender die gericht is op de doelwachtrij via de overdracht wachtrij. U hebt ook een ontvanger die berichten van die dezelfde wachtrij worden opgehaald. Bijvoorbeeld:

```
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Een eenvoudige transactie vervolgens wordt gebruikt voor deze elementen, zoals in het volgende voorbeeld:

```
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over Service Bus wachtrijen:

- [Chaining Service Bus entiteiten met automatisch doorsturen](service-bus-auto-forwarding.md)
- [Voorbeeld van de auto-forward](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AutoForward)
- [Atomische transacties met Bus-Service-voorbeeld](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions)
- [Azure wachtrijen en Service Bus ten opzichte van wachtrijen](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Het gebruik van wachtrijen Service Bus](service-bus-dotnet-get-started-with-queues.md)
<properties 
    pageTitle="Stuur automatisch door entiteiten messaging Service-Bus | Microsoft Azure"
    description="Hoe u een wachtrij of een abonnement aan een andere wachtrij of onderwerp."
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
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Chaining Service Bus entiteiten met automatisch doorsturen

De functie *automatisch doorsturen* kunt u een wachtrij of een abonnement aan een andere wachtrij of onderwerp dat deel uitmaakt van dezelfde naamruimte keten. Wanneer automatisch doorsturen is ingeschakeld, wordt Service Bus automatisch verwijdert berichten die worden geplaatst in de eerste wachtrij of abonnement (bron) en plaatst u deze in de tweede wachtrij of onderwerp (doel). Opmerking Het is nog steeds mogelijk rechtstreeks een bericht verzenden naar de doelentiteit. Het is ook niet mogelijk naar een subwachtrij, zoals een wachtrij aan een andere wachtrij of onderwerp koppelen.

## <a name="using-auto-forwarding"></a>Met behulp van automatisch doorsturen

U kunt automatisch doorsturen inschakelen door het instellen van de eigenschappen van de [SubscriptionDescription.ForwardTo][] of [QueueDescription.ForwardTo][] op de [QueueDescription][] of [SubscriptionDescription][] -objecten voor de bron, zoals in het volgende voorbeeld.

```
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

De doelentiteit moet bestaan op het moment dat de bron-entiteit wordt gemaakt. Als de doelentiteit niet bestaat, retourneert Service Bus een uitzondering als u wordt gevraagd de bron-entiteit te maken.

U kunt automatisch doorsturen voor het schalen van een onderwerp. Bus service beperkt het [aantal abonnementen op een bepaald onderwerp](service-bus-quotas.md) tot 2.000. Geschikt voor extra abonnementen op het tweede niveau onderwerpen maken. Houd er rekening mee dat zelfs als u niet door de Service Bus beperking van het aantal abonnementen gebonden zijn, toevoegen van een tweede onderwerpen de algehele doorvoer van uw onderwerp verbeteren kunt.

![Scenario automatisch doorsturen][0]

U kunt ook automatisch doorsturen loskoppelen van afzenders van berichten van ontvangers. Neem bijvoorbeeld een ERP-systeem dat uit drie modules bestaat: orderverwerking, voorraadbeheer en Customer Relationship Management. Elk van deze modules worden berichten die in de wachtrij in een bijbehorende onderwerp zijn gegenereerd. Alice en Bob zijn vertegenwoordigers die zijn geïnteresseerd in alle berichten die betrekking op hun klanten hebben. Voor het ontvangen van deze berichten, Alice en Bob een persoonlijke wachtrij en maken een abonnement op elk van de ERP-onderwerpen die u automatisch alle berichten naar de wachtrij doorsturen.

![Scenario automatisch doorsturen][1]

Als Lisa op vakantie, haar persoonlijke wachtrij, in plaats van het onderwerp ERP gaat, wordt opgevuld. In dit scenario, omdat een vertegenwoordiger niet berichten ontvangen, bereiken geen van de onderwerpen ERP ooit quotum.

## <a name="auto-forwarding-considerations"></a>Overwegingen bij het automatisch doorsturen

Als de doelentiteit gecumuleerde veel berichten en het quotum overschrijdt, of de doelentiteit is uitgeschakeld, wordt de bronentiteit de berichten toegevoegd aan de [wachtrij voor onbestelbare berichten](service-bus-dead-letter-queues.md) totdat er ruimte beschikbaar in de bestemming (of de entiteit opnieuw ingeschakeld is). Deze berichten blijven wonen in de wachtrij voor onbestelbare berichten, zodat u expliciet moet ontvangen en deze in de wachtrij voor onbestelbare berichten verwerken.

Het wordt aanbevolen dat er een beperkt aantal inschrijvingen op het onderwerp van het eerste niveau en veel abonnementen op de onderwerpen van het tweede niveau bij het koppelen van afzonderlijke onderwerpen als u een samengestelde onderwerp met veel abonnementen. Bijvoorbeeld, kunnen een onderwerp van het eerste niveau met 20 abonnementen, elk van hen zijn geschakeld naar, een onderwerp van het tweede niveau met 200-abonnementen, hogere doorvoersnelheid dan een onderwerp van het eerste niveau met 200-abonnementen, elk geschakeld naar een onderwerp van het tweede niveau met 20 abonnementen.

Service Bus stuklijsten één bewerking voor elk bericht. Bijvoorbeeld is een bericht verzenden naar een onderwerp met 20 abonnementen, elk van hen worden geconfigureerd voor het automatisch doorsturen van berichten naar een andere wachtrij of onderwerp, gefactureerd als 21 bewerkingen als eerste niveau abonnementen een kopie van het bericht ontvangt.

Als u wilt een abonnement dat wordt geschakeld naar een andere wachtrij of onderwerp, moet de maker van het abonnement machtigingen **beheren** op zowel de bron en de bestemming. Verzenden van berichten naar het onderwerp van de bron alleen nodig machtigingen **verzenden** op het onderwerp van de bron.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende naslaginformatie voor gedetailleerde informatie over het automatisch doorsturen:

- [SubscriptionDescription.ForwardTo][]
- [QueueDescription][]
- [SubscriptionDescription][]

Meer informatie over Service Bus prestatieverbeteringen, Zie [Partitioned messaging entiteiten][].

  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [SubscriptionDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.forwardto.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [SubscriptionDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.aspx
  [0]: ./media/service-bus-auto-forwarding/IC628631.gif
  [1]: ./media/service-bus-auto-forwarding/IC628632.gif
  [Gepartitioneerde messaging entiteiten]: service-bus-partitioning.md
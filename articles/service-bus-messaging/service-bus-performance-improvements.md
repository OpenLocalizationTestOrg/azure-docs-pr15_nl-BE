<properties 
    pageTitle="Aanbevolen procedures voor het verbeteren van de prestaties met Bus Service | Microsoft Azure"
    description="Beschrijving van Azure Service Bus om prestaties te optimaliseren wanneer brokered berichten uitwisselen."
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
    ms.date="10/25/2016"
    ms.author="sethm" />

# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Aanbevolen procedures voor betere prestaties met Messaging Service-Bus

In dit onderwerp wordt beschreven hoe Azure Service Bus berichten gebruiken om prestaties te optimaliseren wanneer brokered berichten uitwisselen. Het eerste deel van dit onderwerp beschrijft de verschillende mechanismen die worden aangeboden om te helpen de prestaties te verhogen. Het tweede deel biedt u informatie over Bus-Service gebruiken op een manier die de beste prestaties in een bepaald scenario kunt aanbieden.

In dit onderwerp wordt de term 'client' verwijst naar een entiteit die toegang heeft tot de Service Bus. Een client kan duren voordat de rol van een afzender of een ontvanger. De term "verzender" wordt gebruikt voor een Service Bus wachtrij of onderwerp client berichten naar een wachtrij Service Bus of onderwerp verzendt. De term "ontvanger" verwijst naar een Service Bus wachtrij of abonnement client ontvangt berichten uit een wachtrij Service Bus of abonnement.

Deze secties introduceren verschillende concepten die Service Bus gebruikt om hogere prestaties te.

## <a name="protocols"></a>Protocollen

Bus service kunnen clients voor het verzenden en ontvangen van berichten via drie protocollen

1. Geavanceerde Message Queuing-Protocol (AMQP)

2. Service Bus Messaging Protocol (SBMP)

3. HTTP

Zowel AMQP en SBMP zijn efficiënter, omdat die de verbinding met de Service Bus onderhouden, zolang de messaging fabriek bestaat. Ook worden geïmplementeerd batchen en veelgevraagde. Tenzij expliciet vermeld, is alle informatie in dit onderwerp wordt verondersteld dat het gebruik van AMQP of SBMP.

## <a name="reusing-factories-and-clients"></a>Hergebruik van fabrieken en clients

Bus Service client-objecten, zoals [QueueClient][] of [MessageSender][], worden door middel van een [MessagingFactory][] -object ook interne beheer van de verbindingen biedt gemaakt. Niet sluit messaging fabrieken of wachtrij, onderwerp en Abonnementclients nadat u een bericht verzenden en opnieuw maken als u het volgende bericht verzendt. Een messaging fabriek sluiten Hiermee verwijdert u de verbinding met de Service Bus service en een nieuwe verbinding tot stand is gebracht bij het opnieuw genereren van de fabriek. Een verbinding tot stand te brengen is geen erg efficiënte bewerking die u opnieuw met de dezelfde fabriek en clientobjecten voor meerdere bewerkingen kunt voorkomen. U kunt het object [QueueClient][] veilig gebruiken voor het verzenden van berichten van gelijktijdige asynchrone bewerkingen en meerdere threads. 

## <a name="concurrent-operations"></a>Gelijktijdige bewerkingen.

Een bewerking (verzenden, ontvangen, verwijderen, enz.) neemt enige tijd in. Deze tijd omvat de verwerking van de bewerking door de Bus Service service naast de latentie van de aanvraag en het antwoord. Om het aantal bewerkingen per keer te verhogen, moeten tegelijkertijd bewerkingen uitvoeren. U kunt dit op verschillende manieren doen:

-   **Asynchrone bewerkingen**: de client bewerkingen wordt door het uitvoeren van asynchrone bewerkingen. De volgende aanvraag wordt gestart voordat de vorige aanvraag is voltooid. Hier volgt een voorbeeld van een verzendbewerking asynchroon:

    ```
    BrokeredMessage m1 = new BrokeredMessage(body);
    BrokeredMessage m2 = new BrokeredMessage(body);
    
    Task send1 = queueClient.SendAsync(m1).ContinueWith((t) => 
      {
        Console.WriteLine("Sent message #1");
      });
    Task send2 = queueClient.SendAsync(m2).ContinueWith((t) => 
      {
        Console.WriteLine("Sent message #2");
      });
    Task.WaitAll(send1, send2);
    Console.WriteLine("All messages sent");
    ```

    Dit is een voorbeeld van een asynchrone bewerking ontvangen:
    
    ```
    Task receive1 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
    Task receive2 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
    
    Task.WaitAll(receive1, receive2);
    Console.WriteLine("All messages received");
    
    async void ProcessReceivedMessage(Task<BrokeredMessage> t)
    {
      BrokeredMessage m = t.Result;
      Console.WriteLine("{0} received", m.Label);
      await m.CompleteAsync();
      Console.WriteLine("{0} complete", m.Label);
    }
    ```

-   **Meerdere fabrieken**: alle clients (afzenders naast ontvangers) die zijn gemaakt met dezelfde fabriek een TCP-verbinding delen. De maximale doorvoer wordt beperkt door het aantal bewerkingen dat dit TCP-verbinding kunt doorlopen. De doorvoer die kan worden verkregen met een enkele fabriek is sterk afhankelijk van retourtijden TCP en de grootte van het bericht. Als u hogere doorvoersnelheden, moet u meerdere berichten fabrieken.

## <a name="receive-mode"></a>Modus ontvangen

Wanneer u een wachtrij of abonnement client maakt, kunt u de modus ontvangen: *Peek-lock* of *ontvangen en te verwijderen*. Standaard wordt de modus is [PeekLock][]. Wanneer in deze modus werkt, verzendt de client een aanvraag voor een bericht ontvangt van een Service Bus. Nadat de client het bericht heeft ontvangen, verzendt deze een aanvraag voor het voltooien van het bericht.

Als u de modus ontvangen op [ReceiveAndDelete][], worden beide stappen worden gecombineerd in een enkele aanvraag. Dit vermindert het totale aantal bewerkingen en de algehele doorvoer van berichten afneemt kunt verbeteren. Deze prestatieverbetering wordt geleverd indien raken berichten kwijt.

Bus-service ondersteunt geen transacties voor ontvangen en verwijderen. Bovendien zijn semantiek peek lock vereist voor alle scenario's waarin de client wil uitstellen of [onbestelbare berichten](service-bus-dead-letter-queues.md) een bericht.

## <a name="client-side-batching"></a>Client-side batchen

Client-side batchen, kunnen clients wachtrij of onderwerp moet worden gewacht op het verzenden van een bericht voor een bepaalde periode. Als de client verzendt een extra berichten tijdens deze periode, verzonden berichten in één batch. Client-side batchen veroorzaakt ook een wachtrij/abonnement client meerdere aanvragen voor **volledige** batch in een enkele aanvraag. Batchverwerking is alleen beschikbaar voor asynchrone bewerkingen voor **verzenden** en **voltooid** . Synchrone bewerkingen worden onmiddellijk verzonden naar de Bus-Service-service. Niet batchen optreden voor peek of ontvangstbewerkingen noch batchen treedt op clients.

Als de batch de maximale berichtgrootte overschrijdt, het laatste bericht is verwijderd uit de partij en de client stuurt onmiddellijk de batch. Het laatste bericht wordt het eerste bericht van de volgende batch. Een client gebruikt standaard een batch-interval van 20 MS. Het interval voor kunt u wijzigen door de eigenschap [BatchFlushInterval][] alvorens de fabriek messaging te maken. Deze instelling is van invloed op alle clients die deze factory.

Schakel batchen stelt u de eigenschap [BatchFlushInterval][] aan **TimeSpan.Zero**. Bijvoorbeeld:

```
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

Heeft geen invloed op het aantal te factureren messaging batchen en is alleen beschikbaar voor het protocol van de client Service Bus. Het HTTP-protocol biedt geen ondersteuning voor batchverwerking.

## <a name="batching-store-access"></a>Archief toegang batchen

U verhoogt de doorvoer van een wachtrij, de onderwerp/het abonnement, batches Bus-Service meerdere berichten het wegschrijven van de interne opslag. Als op een onderwerp of een wachtrij is ingeschakeld, schrijven van berichten in het archief wordt een batch worden geplaatst. Als ingeschakeld op een wachtrij of abonnement, verwijderen van berichten uit het archief wordt een batch worden geplaatst. Als batch archief toegang is ingeschakeld voor een entiteit, vertraagd Service Bus een schrijfbewerking winkel met betrekking tot die entiteit door maximaal 20 MS. Winkel extra bewerkingen die tijdens dit interval plaatsvinden worden toegevoegd aan de batch. Batch archief toegang geldt alleen voor **verzenden** en **Complete** behandelingen; ontvangen bewerkingen worden niet beïnvloed. Batch archief toegang is een eigenschap van een entiteit. Batchen vindt plaats via alle entiteiten die batch archief toegang inschakelen.

Wanneer u een nieuwe wachtrij, onderwerp of een abonnement, wordt batch archief toegang standaard ingeschakeld. Als batch archief toegang uitschakelt, moet u de eigenschap [EnableBatchedOperations][] instellen op **false** voordat de entiteit te maken. Bijvoorbeeld:

```
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

Batch winkel heeft geen invloed op het aantal te factureren messaging, en is een eigenschap van een wachtrij, het onderwerp of het abonnement. Het is afhankelijk van de modus ontvangen en het protocol dat wordt gebruikt tussen een client en de Bus-Service-service.

## <a name="prefetching"></a>Veelgevraagde

Veelgevraagde kunt de wachtrij of abonnement client extra berichten laden van de service bij het uitvoeren van een ontvangstbewerking. Deze berichten worden opgeslagen in een lokale cache van de client. De grootte van de cache wordt bepaald door de eigenschappen [QueueClient.PrefetchCount][] of [SubscriptionClient.PrefetchCount][] . Elke client waarmee veelgevraagde behoudt de eigen cache. Een cache wordt niet door clients gedeeld. Als de client een ontvangstbewerking initieert en de cache leeg is, verzendt de service een batch van berichten. De grootte van de partij gelijk is aan de grootte van de cache of 256KB, indien dit kleiner is. Als de client een ontvangstbewerking initieert en de cache een bericht bevat, het bericht uit de cache opgehaald.

Wanneer een bericht is tabelruimte, vergrendelt u de service het prefetched weergegeven. Op deze manier kan niet het prefetched bericht worden ontvangen door een andere ontvanger. Als het bericht de ontvanger kan niet worden voltooid voordat de vergrendeling is verstreken, wordt het bericht beschikbaar voor andere ontvangers. De prefetched kopie van het bericht blijft in de cache. De ontvanger die de verlopen in de cache opgeslagen kopie verbruikt ontvangt een uitzondering wanneer wordt geprobeerd een bericht te voltooien. Standaard wordt de vergrendeling van het bericht verloopt na 60 seconden. Deze waarde kan worden uitgebreid tot 5 minuten. Als u wilt voorkomen dat het verbruik van verlopen berichten, moet grootte van de cache altijd kleiner zijn dan het aantal berichten die door een client kan worden gebruikt binnen de time-outinterval vergrendelen.

Als u de vergrendeling verlopen 60 seconden, is een goede waarde voor [SubscriptionClient.PrefetchCount][] 20-maal de maximale verwerking tarieven van alle ontvangers van de fabriek. Bijvoorbeeld een fabriek 3 ontvangers wordt gemaakt en elke ontvanger kan maximaal 10 berichten per seconde verwerken. De prefetch-telling mag niet meer dan 20\*3\*10 = 600. [QueueClient.PrefetchCount][] is standaard ingesteld op 0, wat betekent dat er geen berichten meer van de service worden opgehaald.

Veelgevraagde berichten neemt de algehele doorvoer voor een wachtrij of een abonnement, omdat het daardoor niet het totale aantal bewerkingen bericht of retouren. Ophalen van het eerste bericht, maar duurt langer (als gevolg van de toegenomen berichtgrootte). Ontvangen berichten tabelruimte zijn sneller omdat al deze berichten zijn gedownload door de client.

De time-to-live (TTL) eigenschap van een bericht wordt gecontroleerd door de server op het moment dat de server het bericht naar de client verzendt. De client controleert de berichteigenschap TTL niet wanneer het bericht is ontvangen. In plaats daarvan kan het bericht worden ontvangen zelfs als de TTL van het bericht is verlopen terwijl het bericht door de client in de cache is opgeslagen.

Veelgevraagde heeft geen invloed op het aantal te factureren messaging en is alleen beschikbaar voor het protocol van de client Service Bus. Veelgevraagde biedt geen ondersteuning voor het HTTP-protocol. Veelgevraagde is beschikbaar voor het ontvangen van synchrone en asynchrone bewerkingen.

## <a name="express-queues-and-topics"></a>Express wachtrijen en onderwerpen

Express diensten inschakelen hoge doorvoer en latentie scenario's verminderd. Met express entiteiten, als een bericht wordt verzonden naar een wachtrij of onderwerp, het bericht niet meteen opgeslagen in het berichtenarchief. In plaats daarvan is het in het geheugen opgeslagen. Als een bericht in de wachtrij voor meer dan een paar seconden blijft, wordt het automatisch geschreven naar een stabiele opslaglocatie, dus beschermd tegen verlies als gevolg van een stroomstoring. Schrijven van het bericht in een cachegeheugen verhoogt de doorvoersnelheid en wordt latentie verlaagd, omdat er geen toegang tot een stabiele opslaglocatie op het moment dat het bericht is verzonden. Berichten die worden gebruikt binnen een paar seconden, niet opgeslagen in het berichtenarchief. In het volgende voorbeeld wordt het onderwerp van een uitdrukkelijke gemaakt.

```
TopicDescription td = new TopicDescription(TopicName);
td.EnableExpress = true;
namespaceManager.CreateTopic(td);
```

Als een bericht met de essentiële informatie die niet verloren worden moet wordt verzonden naar een uitdrukkelijke entiteit, kan de afzender Bus-Service het bericht naar een stabiele opslaglocatie met de eigenschap [ForcePersistence][] op **true**om onmiddellijk kunt afdwingen.

## <a name="use-of-partitioned-queues-or-topics"></a>Gebruik van gepartitioneerde wachtrijen of onderwerpen

Intern Service Bus wordt hetzelfde knooppunt en berichten opslaan, verwerken en opslaan van alle berichten voor een messaging-dienst (wachtrij of onderwerp). Een gepartitioneerde wachtrij of onderwerp, aan de andere kant is verdeeld over meerdere knooppunten en messaging-winkels. Gepartitioneerde wachtrijen en onderwerpen niet alleen een hogere doorvoersnelheid dan gewone wachtrijen en onderwerpen opleveren, ze vertonen uitstekende beschikbaarheid. Als u een gepartitioneerde eenheid, stelt u de eigenschap [EnablePartitioning][] op **true**, zoals in het volgende voorbeeld wordt getoond. Zie voor meer informatie over gepartitioneerde entiteiten, [gepartitioneerd messaging entiteiten][].

```
// Create partitioned queue.
QueueDescription qd = new QueueDescription(QueueName);
qd.EnablePartitioning = true;
namespaceManager.CreateQueue(qd);
```

## <a name="use-of-multiple-queues"></a>Gebruik van meerdere wachtrijen

Als het is niet mogelijk een gepartitioneerde wachtrij of onderwerp of de verwachte belasting kan niet worden verwerkt door een enkele wachtrij van gepartitioneerde of onderwerp, moet u meerdere-entiteiten. Wanneer u meerdere entiteiten, een speciale client voor elke entiteit, in plaats van dezelfde client voor alle entiteiten maken.

## <a name="development--testing-features"></a>Ontwikkeling en testen van functies

Bus service is een functie die wordt gebruikt voor het ontwikkelen die **nooit in productie configuraties moet worden gebruikt**.

[TopicDescription.EnableFilteringMessagesBeforePublishing](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing.aspx)
- Wanneer nieuwe regels of Filters worden toegevoegd aan het onderwerp, kan de EnableFilteringMessagesBeforePublishing worden gebruikt om te controleren of de nieuwe filterexpressie functioneert zoals verwacht.

## <a name="scenarios"></a>Scenario 's

In de volgende secties beschrijven typische berichtverzending en een overzicht van de beste Service Bus-instellingen. Doorvoersnelheden die zijn ingedeeld als klein (minder dan 1 bericht per seconde), Gemiddeld (1 bericht per seconde of meer doch minder dan 100 berichten per seconde) en hoog (100 berichten/tweede of hoger). Het aantal clients worden geclassificeerd als kleine (5 of minder), matig (meer dan 5 en lager dan of gelijk aan 20), en groot (meer dan 20).

### <a name="high-throughput-queue"></a>Hoge gegevensdoorvoer wachtrij

Doel: De doorvoer van één wachtrij te maximaliseren. Het nummer van de afzenders en ontvangers is klein.

-   Gebruik een gepartitioneerde wachtrij voor betere prestaties en beschikbaarheid.

-   Gebruik meerdere bericht fabrieken maken van afzenders wilt verhogen de verzendsnelheid naar de wachtrij. Gebruik voor elke afzender, asynchrone bewerkingen of meerdere threads.

-   Gebruik meerdere bericht fabrieken maken van ontvangers wilt verhogen de snelheid ontvangen uit de wachtrij.

-   Asynchrone bewerkingen gebruiken om te profiteren van client-side batchen.

-   De batchen interval instellen op 50ms te verminderen het aantal client-beveiligingsprotocollen Service Bus. Als meerdere afzenders worden gebruikt, verhoogt u het interval batchen 100ms.

-   Batch archief toegang ingeschakeld laten. Dit verhoogt de snelheid waarmee berichten in de wachtrij kunnen worden opgeslagen.

-   De prefetch-aantal ingesteld op 20 maal de maximale verwerking tarieven van alle ontvangers van een fabriek. Dit vermindert het aantal client-beveiligingsprotocollen Service Bus.

### <a name="multiple-high-throughput-queues"></a>Meerdere wachtrijen met hoge gegevensdoorvoer

Doel: Maximale totale doorvoersnelheid van meerdere wachtrijen. De doorvoer van afzonderlijke wachtrij is Gemiddeld of hoog.

Gebruik de instellingen beschreven om de doorvoer van één wachtrij maximaliseren om maximale doorvoer via meerdere wachtrijen. Daarnaast gebruiken verschillende fabrieken clients die bij het verzenden of ontvangen van andere wachtrijen maken.

### <a name="low-latency-queue"></a>Lage latentie wachtrij

Doel: Minimaliseren latentie van end-to-end van een wachtrij of onderwerp. Het nummer van de afzenders en ontvangers is klein. De doorvoer van de wachtrij is klein of Gemiddeld.

-   Gebruik een gepartitioneerde wachtrij voor een betere beschikbaarheid.

-   Client-side batchen uit te schakelen. De client stuurt onmiddellijk een bericht.

-   Batch opslaan uitschakelen. De service schrijft het bericht direct naar de winkel.

-   Als één client, stelt u de telling prefetch aan 20-maal de verwerking snelheid van de ontvanger. Als u meerdere berichten in de wachtrij op hetzelfde moment, verzendt de client Service Bus protocol deze allemaal op hetzelfde moment. Wanneer de client het volgende bericht ontvangt, wordt dit bericht al in de lokale cache. De cache moet klein zijn.

-   Als u meerdere clients, moet u de telling prefetch ingesteld op 0. Op deze manier kan de tweede client het tweede bericht ontvangen tijdens de eerste client het eerste bericht steeds verwerkt nog.

### <a name="queue-with-a-large-number-of-senders"></a>Met een groot aantal afzenders in de wachtrij

Doel: De doorvoer van een wachtrij of een onderwerp met een groot aantal afzenders maximaliseren. Elke afzender wordt verzonden berichten met een matige snelheid. Het aantal ontvangers is klein.

Bus service kunnen maximaal 1000 gelijktijdige verbindingen met een messaging-entiteit (of 5000 met behulp van AMQP). Deze limiet wordt afgedwongen op het niveau van de naamruimte en onderwerpen-wachtrijen/abonnementen worden afgetopt op het maximum aantal gelijktijdige verbindingen per naamruimte. Dit nummer wordt voor wachtrijen gedeeld tussen verzenders en ontvangers. Als alle 1000 verbindingen nodig om afzenders zijn, vervangt u de wachtrij met een onderwerp en een enkel abonnement. Een onderwerp accepteert maximaal 1000 gelijktijdige verbindingen van afzenders, dat het abonnement een extra 1000 gelijktijdige verbindingen van ontvangers accepteert. Als er meer dan 1000 gelijktijdige afzenders zijn vereist, moeten de afzenders berichten verzenden naar de Service Bus protocol via HTTP.

Om de doorvoer maximaliseren, het volgende doen:

-   Gebruik een gepartitioneerde wachtrij voor betere prestaties en beschikbaarheid.

-   Als de afzender zich in een ander proces bevindt, gebruikt u dezelfde fabriek per proces.

-   Asynchrone bewerkingen gebruiken om te profiteren van client-side batchen.

-   Gebruik de standaard interval van 20 MS batchen verminderen het aantal client-beveiligingsprotocollen Service Bus.

-   Batch archief toegang ingeschakeld laten. Dit verhoogt de snelheid waarmee berichten kunnen worden geschreven in de wachtrij of onderwerp.

-   De prefetch-aantal ingesteld op 20 maal de maximale verwerking tarieven van alle ontvangers van een fabriek. Dit vermindert het aantal client-beveiligingsprotocollen Service Bus.

### <a name="queue-with-a-large-number-of-receivers"></a>In de wachtrij met een groot aantal ontvangers

Doel: De snelheid van het ontvangen van een wachtrij of een abonnement met een groot aantal ontvangers te maximaliseren. Elke ontvanger ontvangt berichten met een matige snelheid. Het aantal afzenders is klein.

Bus service kunnen maximaal 1000 gelijktijdige verbindingen naar een entiteit. Als een wachtrij meer dan 1000 ontvangers zijn vereist, moet u de wachtrij vervangen door een onderwerp en meerdere abonnementen. Elk abonnement ondersteunt maximaal 1000 gelijktijdige verbindingen. U kunt ook ontvangers hebben toegang tot de wachtrij via het HTTP-protocol.

Om de doorvoer maximaliseren, het volgende doen:

-   Gebruik een gepartitioneerde wachtrij voor betere prestaties en beschikbaarheid.

-   Als de ontvanger zich in een ander proces bevindt, gebruikt u dezelfde fabriek per proces.

-   Ontvangers kunt synchrone of asynchrone bewerkingen. Gezien de frequentie matig ontvangen van een individuele ontvanger, client-side batchen van een volledige aanvraag heeft geen invloed op de doorvoer van de ontvanger.

-   Batch archief toegang ingeschakeld laten. Hierdoor wordt de totale belasting van de entiteit. Het vermindert ook de snelheid waarmee berichten kunnen worden geschreven in de wachtrij of onderwerp.

-   De prefetch-telling op een kleine waarde instellen (bijvoorbeeld PrefetchCount = 10). Hierdoor wordt voorkomen dat ontvangers inactiviteit terwijl andere ontvangers een groot aantal berichten in de cache opgeslagen hebben.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Onderwerp met een klein aantal abonnementen

Doel: De doorvoer van een onderwerp met een klein aantal abonnementen te maximaliseren. Een bericht wordt ontvangen door veel abonnementen, wat betekent dat de snelheid van de gecombineerde ontvangen over alle abonnementen groter is dan de verzendsnelheid. Het aantal afzenders is klein. Het aantal ontvangers per abonnement is klein.

Om de doorvoer maximaliseren, het volgende doen:

-   Een gepartitioneerde onderwerp gebruiken voor betere prestaties en beschikbaarheid.

-   Gebruik meerdere bericht fabrieken maken van afzenders wilt verhogen de verzendsnelheid in het onderwerp. Gebruik voor elke afzender, asynchrone bewerkingen of meerdere threads.

-   Gebruik meerdere bericht fabrieken maken van ontvangers wilt verhogen de snelheid ontvangen van een abonnement. Gebruik voor elke ontvanger, asynchrone bewerkingen of meerdere threads.

-   Asynchrone bewerkingen gebruiken om te profiteren van client-side batchen.

-   Gebruik de standaard interval van 20 MS batchen verminderen het aantal client-beveiligingsprotocollen Service Bus.

-   Batch archief toegang ingeschakeld laten. Dit verhoogt de snelheid waarmee berichten kunnen worden geschreven in het onderwerp.

-   De prefetch-aantal ingesteld op 20 maal de maximale verwerking tarieven van alle ontvangers van een fabriek. Dit vermindert het aantal client-beveiligingsprotocollen Service Bus.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Onderwerp met een groot aantal abonnementen

Doel: De doorvoer van een onderwerp met een groot aantal abonnementen te maximaliseren. Een bericht wordt ontvangen door veel abonnementen, wat betekent dat de gecombineerde ontvangen over alle abonnementen is veel groter dan de verzendsnelheid. Het aantal afzenders is klein. Het aantal ontvangers per abonnement is klein.

Onderwerpen met een groot aantal abonnementen bieden doorgaans een lage totale doorvoersnelheid als alle berichten worden doorgestuurd naar alle abonnementen. Dit wordt veroorzaakt door het feit dat elk bericht wordt vaak ontvangen en alle berichten die zijn opgenomen in een onderwerp en alle bijbehorende abonnementen op dezelfde locatie worden opgeslagen. Wordt uitgegaan van het aantal afzenders en het aantal ontvangers per abonnement klein is. De Bus service biedt ondersteuning voor maximaal 2.000 abonnementen per onderwerp.

Om de doorvoer maximaliseren, het volgende doen:

-   Een gepartitioneerde onderwerp gebruiken voor betere prestaties en beschikbaarheid.

-   Asynchrone bewerkingen gebruiken om te profiteren van client-side batchen.

-   Gebruik de standaard interval van 20 MS batchen verminderen het aantal client-beveiligingsprotocollen Service Bus.

-   Batch archief toegang ingeschakeld laten. Dit verhoogt de snelheid waarmee berichten kunnen worden geschreven in het onderwerp.

-   De prefetch-telling 20 keer de verwachte ontvangen rente in seconden instellen. Dit vermindert het aantal client-beveiligingsprotocollen Service Bus.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het optimaliseren van de prestaties van de Service Bus, Zie [Partitioned messaging entiteiten][].

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [PeekLock]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx
  [ReceiveAndDelete]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx
  [BatchFlushInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.netmessagingtransportsettings.batchflushinterval.aspx
  [EnableBatchedOperations]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations.aspx
  [QueueClient.PrefetchCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.prefetchcount.aspx
  [SubscriptionClient.PrefetchCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.prefetchcount.aspx
  [ForcePersistence]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.forcepersistence.aspx
  [EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [Gepartitioneerde messaging entiteiten]: service-bus-partitioning.md
  
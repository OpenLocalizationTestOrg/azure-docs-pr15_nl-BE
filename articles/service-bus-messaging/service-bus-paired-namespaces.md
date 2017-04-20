<properties 
    pageTitle="Bus service gekoppeld naamruimten | Microsoft Azure"
    description="Gekoppelde naamruimte implementatiedetails en kosten"
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
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Implementatiegegevens naamruimte gekoppeld en de gevolgen

De methode [PairNamespaceAsync][] , met een instantie [SendAvailabilityPairedNamespaceOptions][] voert taken uit zichtbaar voor jou. Omdat er zijn kosten overwegingen bij het gebruik van de functie, is het nuttig te weten de taken die u het gedrag verwacht wanneer dit gebeurt. De API voert de volgende automatische werking voor jou:

-   Het maken van wachtrijen reserve.
-   Het maken van een [MessageSender][] -object dat gesprekken wachtrijen of onderwerpen voert.
-   Wanneer een messaging-dienst niet beschikbaar is, ping verzendt berichten voor de entiteit in een poging om te detecteren die entiteit weer beschikbaar is.
-   Eventueel wordt gemaakt van een reeks "pompen bericht" dat berichten uit de reserve-wachtrijen verplaatst naar de primaire wachtrijen.
-   Coördinaten/vastgelopen afsluiting van de primaire en secundaire [MessagingFactory][] exemplaren.

Op een hoog niveau, werkt de functie als volgt: wanneer de primaire entiteit in orde is, geen wijziging van gedrag optreden. Wanneer de duur van de [FailoverInterval][] is verstreken en de primaire entiteit niet geslaagde ziet stuurt na een tijdelijke [MessagingException][] of een [TimeoutException][], gebeurt het volgende:

1.  Verzenden bewerkingen aan de primaire entiteit worden uitgeschakeld en het systeem de primaire entiteit wordt gepingd totdat pings kunnen worden afgeleverd.

2.  Een willekeurige reserve-wachtrij is ingeschakeld.

3.  [BrokeredMessage][] -objecten worden gerouteerd naar de wachtrij gekozen reserve.

1.  Als een verzendbewerking naar de wachtrij gekozen reserve mislukt, die wachtrij opgehaald uit de rotatie en een nieuwe wachtrij is ingeschakeld. Alle afzenders van het exemplaar van de [MessagingFactory][] informatie van de storing.

De volgende cijfers weer voor de reeks. Eerst worden berichten verzonden door de afzender.

![Gepaarde naamruimten][0]

Bij een fout te verzenden naar de primaire wachtrij, begint de afzender om berichten naar een wachtrij reserve willekeurig gekozen. Tegelijkertijd wordt een ping taak gestart.

![Gepaarde naamruimten][1]

Op dit moment de berichten nog steeds in de wachtrij van de secundaire en niet aan de primaire wachtrij zijn geleverd. Nadat u de primaire wachtrij weer in orde is, moet ten minste één proces de syphon worden uitgevoerd. De syphon biedt de berichten van de verschillende reserve wachtrijen op de juiste bestemming entiteiten (wachtrijen en onderwerpen).

![Gepaarde naamruimten][2]

De rest van dit onderwerp worden de specifieke details van de werking van deze onderdelen besproken.

## <a name="creation-of-backlog-queues"></a>Het maken van wachtrijen reserve

Het [SendAvailabilityPairedNamespaceOptions][] -object doorgegeven aan de methode [PairNamespaceAsync][] geeft het aantal reserve-wachtrijen die u wilt gebruiken. Elke reserve-wachtrij wordt vervolgens gemaakt met de volgende eigenschappen expliciet instellen (alle andere waarden worden ingesteld op de standaardwaarden van [QueueDescription][] ):

| Pad                                   | [namespace primaire] / x-servicebus-overdracht / [index] waar [index] is een in [0, BacklogQueueCount) |
|----------------------------------------|------------------------------------------------------------------------------------------------------|
| MaxSizeInMegabytes                     | 5120                                                                                                 |
| MaxDeliveryCount                       | int. MaxValue                                                                                         |
| DefaultMessageTimeToLive               | TimeSpan.MaxValue                                                                                    |
| AutoDeleteOnIdle                       | TimeSpan.MaxValue                                                                                    |
| LockDuration                           | 1 minuut                                                                                             |
| EnableDeadLetteringOnMessageExpiration | True                                                                                                 |
| EnableBatchedOperations                | True                                                                                                 |

Bijvoorbeeld de eerste reserve-wachtrij wordt gemaakt voor de naamruimte **contoso** heet `contoso/x-servicebus-transfer/0`.

Bij het maken van de wachtrijen de code eerst gecontroleerd of deze wachtrij bestaat. Als de wachtrij niet bestaat, is wordt de wachtrij gemaakt. De code niet het opschonen van wachtrijen reserve "extra". Met name als de toepassing met de primaire naamruimte **contoso** vraagt om vijf reserve wachtrijen maar geen wachtrij reserve met het pad `contoso/x-servicebus-transfer/7` bestaat, die extra reserve-wachtrij is nog steeds aanwezig, maar wordt niet gebruikt. Het systeem kan expliciet extra reserve wachtrijen bestaan die niet worden gebruikt. Als de eigenaar van de naamruimte bent u verantwoordelijk voor het opruimen van ongebruikte/ongewenste reserve wachtrijen. De reden voor dit besluit is dat Bus-Service kan niet weet welke doeleinden er bestaan voor de wachtrijen in de naamruimte. Bovendien, als een wachtrij met de opgegeven naam bestaat, maar niet voldoet aan de veronderstelde [QueueDescription][], vervolgens uw redenen zijn eigen om het standaardgedrag te wijzigen. Geen garanties worden gemaakt voor wijzigingen in de reserve wachtrijen door uw code. Zorg ervoor dat de wijzigingen grondig testen.

## <a name="custom-messagesender"></a>Aangepaste MessageSender

Bij het verzenden, gaan alle berichten via een interne [MessageSender][] -object dat zich normaal gedraagt wanneer alles werkt en wordt omgeleid naar de reserve-wachtrijen als "problemen." Een tijdelijke fout wordt ontvangen, wordt een timer gestart. Na een [tijdsduur][] die bestaan uit de waarde van de eigenschap [FailoverInterval][] waarin geen succesvolle berichten zijn verzonden, is de failover-functie ingeschakeld. Op dit moment plaats de volgende voor elke entiteit:

- Een ping taak wordt uitgevoerd voor elke [PingPrimaryInterval][] om te controleren of de entiteit beschikbaar is. Zodra deze taak is voltooid, wordt alle clientcode die gebruikmaakt van de entiteit onmiddellijk gestart nieuwe berichten verzenden naar de primaire naamruimte.

- Toekomstige aanvragen verzenden naar die dezelfde entiteit van elke andere afzenders resulteert in de [BrokeredMessage][] wordt verzonden moet worden gewijzigd om te gaan zitten in de wachtrij van de reserve. De wijziging bepaalde eigenschappen van het object [BrokeredMessage][] verwijderd en elders opgeslagen. De volgende eigenschappen worden uitgeschakeld en onder een nieuwe alias, waardoor de Bus-Service en de SDK om berichten te verwerken op uniforme wijze toegevoegd:

| Oude naam van de eigenschap       | Naam van de nieuwe eigenschap |
|-------------------------|-------------------|
| Sessie-id               | x-ms-sessie-id    |
| TimeToLive              | x-ms-timetolive   |
| ScheduledEnqueueTimeUtc | x-ms-pad         |

De oorspronkelijke bestemmingspad wordt ook opgeslagen in het bericht als een eigenschap met de naam ms-x-pad. Dit ontwerp stelt berichten voor veel entiteiten worden gecombineerd in een wachtrij één reserve. De eigenschappen worden omgezet door de syphon terug.

Het object met aangepaste [MessageSender][] kan problemen ondervinden wanneer berichten de limiet van 256 KB benaderen en failover is ingeschakeld. Het aangepaste [MessageSender][] object berichten worden opgeslagen voor alle wachtrijen en onderwerpen samen in de reserve-wachtrijen. Dit object gemengd berichten van veel primaire kleuren bij elkaar in de reserve-wachtrijen. Voor het verwerken van de taakverdeling tussen de vele klanten die elkaar niet kent, neemt de SDK willekeurig één reserve wachtrij voor elk [QueueClient][] of [TopicClient][] die u in de code maakt.

## <a name="pings"></a>Pings

Een ping-bericht is een lege [BrokeredMessage][] met de eigenschap [ContentType][] is ingesteld op application/vnd.ms-servicebus-ping en een [TimeToLive][] -waarde van 1 seconde. Deze ping heeft een speciaal kenmerk in Service Bus: de server levert nooit een ping als een beller een [BrokeredMessage][]aanvraagt. Dus hebt nooit u informatie over het ontvangen en deze berichten negeren. Elke entiteit (unieke wachtrij of onderwerp) per [MessagingFactory][] exemplaar per client zal worden gepingd wanneer ze worden beschouwd als niet beschikbaar. Standaard is dit gebeurt eenmaal per minuut. Ping-berichten worden beschouwd als gewone Service Bus berichten en kunnen resulteren in kosten voor bandbreedte en berichten. Als de clients wordt gedetecteerd dat het systeem beschikbaar is, worden de berichten stoppen.

## <a name="the-syphon"></a>De syphon

Ten minste één uitvoerbaar programma in de toepassing moet actief zijn de syphon. De syphon voert een lange enquête ontvangen die 15 minuten duurt. Wanneer alle entiteiten beschikbaar zijn en u 10 reserve wachtrijen hebt, roept de toepassing die fungeert als host voor de syphon de ontvangstbewerking 40 keer per uur, 960 keer per dag en tijden 28800 in 30 dagen. Wanneer de syphon actief berichten uit de reserve naar de primaire wachtrij verplaatst is, krijgt elk bericht met de volgende toeslagen (standaard toeslagen voor de grootte van het bericht en de bandbreedte van toepassing in alle stadia):

1.  Verzenden naar de reserve.

2.  Ontvangen van de reserve.

3.  Verzenden naar de primaire.

4.  Ontvangen van de primaire.

## <a name="closefault-behavior"></a>Sluiten/fout gedrag

Binnen een toepassing die fungeert als host voor de syphon, zodra de primaire of secundaire [MessagingFactory][] fouten of is gesloten zonder dat de partner wordt ook mislukt/gesloten en de syphon detecteert deze staat de syphon fungeert. Als de andere [MessagingFactory][] niet is gesloten binnen 5 seconden, wordt de syphon het nog open [MessagingFactory][]fault.

## <a name="next-steps"></a>Volgende stappen

Zie [asynchrone berichtenafhandeling patronen en hoge beschikbaarheid][] voor gedetailleerde informatie over de Service Bus voor asynchrone berichtafhandeling. 

  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [SendAvailabilityPairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [FailoverInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.failoverinterval.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [TimeSpan]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
  [PingPrimaryInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.pingprimaryinterval.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [ContentType]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx
  [TimeToLive]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
  [Asynchrone berichtenafhandeling patronen en hoge beschikbaarheid]: service-bus-async-messaging.md
  [0]: ./media/service-bus-paired-namespaces/IC673405.png
  [1]: ./media/service-bus-paired-namespaces/IC673406.png
  [2]: ./media/service-bus-paired-namespaces/IC673407.png

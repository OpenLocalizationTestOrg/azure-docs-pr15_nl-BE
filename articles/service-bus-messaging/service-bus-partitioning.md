<properties 
    pageTitle="Wachtrijen en onderwerpen gepartitioneerd | Microsoft Azure"
    description="Beschrijving van Service Bus wachtrijen en onderwerpen partitioneren met behulp van meerdere bericht makelaars."
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
    ms.date="09/02/2016"
    ms.author="sethm;hillaryc" />

# <a name="partitioned-queues-and-topics"></a>Gepartitioneerde wachtrijen en onderwerpen

Azure Service Bus maakt gebruik van meerdere bericht makelaars om berichten te verwerken en meerdere berichten winkels voor het opslaan van berichten. Een conventionele wachtrij of een onderwerp wordt behandeld door een enkel bericht makelaar en opgeslagen in een berichtenarchief. Bus service kunt ook wachtrijen of onderwerpen aan meerdere bericht makelaars en messaging winkels worden gepartitioneerd. Dit betekent dat de algehele doorvoer van een gepartitioneerde wachtrij of een onderwerp niet langer wordt beperkt door de prestaties van een enkel bericht broker of berichtenarchief. Bovendien weer een tijdelijke storing van een berichtenarchief niet een gepartitioneerde wachtrij of onderwerp niet beschikbaar. Gepartitioneerde wachtrijen en onderwerpen kunnen bevatten alle geavanceerde functies zoals ondersteuning voor transacties en sessies Service Bus.

Zie het onderwerp [Service Bus-architectuur][] voor meer informatie over Service Bus interne werking.

## <a name="how-it-works"></a>Hoe het werkt

Elke wachtrij gepartitioneerde of onderwerp bestaat uit meerdere fragmenten. Elk fragment is opgeslagen in een andere winkel voor messaging en verwerkt door een ander bericht broker. Wanneer een bericht wordt verzonden naar een wachtrij gepartitioneerde of onderwerp, toegewezen Bus-Service het bericht op een van de fragmenten. De selectie is willekeurig gedaan door Service Bus of een partitie gebruikt die de afzender kunt opgeven.

Wanneer een client een bericht wilt ontvangen van een gepartitioneerde wachtrij of van een abonnement op een gepartitioneerde onderwerp, Bus Service query's alle fragmenten voor berichten, geeft als resultaat het eerste bericht dat wordt verkregen uit een van de messaging-archieven op de ontvanger. Service Bus caches van de andere berichten en ze geretourneerd als deze ontvangt meer ontvangen aanvragen. Een ontvangende client is niet op de hoogte van de partities; de client gerichte werking van een gepartitioneerde wachtrij of een onderwerp (bijvoorbeeld lezen, uitvoeren, uitstellen, onbestelbare, veelgevraagde) is gelijk aan het gedrag van een geregelde dienst.

Er is geen extra kosten bij het verzenden van een bericht of het bericht in een wachtrij van gepartitioneerde of onderwerp.

## <a name="enable-partitioning"></a>Partitionering inschakelen

De SDK Azure versie 2.2 of hoger gebruiken of Geef voor het gebruik van gepartitioneerde wachtrijen en onderwerpen met Azure Service Bus, `api-version=2013-10` in de HTTP-aanvragen.

U kunt wachtrijen Service Bus en onderwerpen maken in 1, 2, 3, 4 of 5 GB formaten (de standaardinstelling is 1 GB). Met partitionering ingeschakeld, maakt Service Bus 16 partities voor elke GB die u opgeeft. Als zodanig, als u een wachtrij die is 5 GB groot maakt, is door de partities van 16 de maximale grootte (5 \* 16) = 80 GB. Door te kijken naar de vermelding op de [Azure portal][]ziet u de maximale grootte van de wachtrij van gepartitioneerde of onderwerp.

Er zijn verschillende manieren voor het maken van een gepartitioneerde wachtrij of onderwerp. Wanneer u de wachtrij of het onderwerp van uw toepassing maakt, kunt u voor de wachtrij of onderwerp-partitioning door respectievelijk de eigenschap [QueueDescription.EnablePartitioning][] of [TopicDescription.EnablePartitioning][] instelt op **true**. Deze eigenschappen moeten worden ingesteld op het moment dat de wachtrij of onderwerp is gemaakt. Het is niet mogelijk om deze eigenschappen op een onderwerp of een bestaande wachtrij wijzigen. Bijvoorbeeld:

```
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Ook kunt u een wachtrij van gepartitioneerde of onderwerp in Visual Studio of in de [portal Azure][]. Wanneer u een nieuwe wachtrij of een onderwerp in de portal maakt, stelt u de optie **Inschakelen partitioneren** in het blad **algemene instellingen** van de wachtrij of onderwerp **Instellingen** venster op **true**. Klik op het selectievakje **Partities inschakelen** in het dialoogvenster **Nieuwe wachtrij** of een **Nieuw onderwerp** in Visual Studio.

## <a name="use-of-partition-keys"></a>Gebruik van partitiesleutels

Wanneer een bericht in de wachtrij in een wachtrij van gepartitioneerde of onderwerp, controleert Bus Service de aanwezigheid van een partitiesleutel. Als deze wordt gevonden, selecteert deze het fragment op basis van die sleutel. Als een partitiesleutel niet wordt gevonden, selecteert deze het fragment op basis van een intern algoritme.

### <a name="using-a-partition-key"></a>Een partitie wordt gebruikt

Bepaalde scenario's, zoals de sessies of transacties, moeten berichten worden opgeslagen in een bepaald fragment. Al deze scenario's vereisen het gebruik van de partitiesleutel van een. Alle berichten die met dezelfde partitiesleutel worden toegewezen aan het hetzelfde fragment. Als het fragment tijdelijk niet beschikbaar is is, wordt een fout geretourneerd door Service Bus.

Afhankelijk van het scenario, worden verschillende berichteigenschappen gebruikt als een partitiesleutel:

**Sessie-id**: als een bericht heeft de eigenschap [BrokeredMessage.SessionId][] is ingesteld, wordt de Service Bus deze eigenschap wordt gebruikt als de partitiesleutel. Op deze manier worden alle berichten die deel uitmaken van dezelfde sessie verwerkt door de makelaar met hetzelfde bericht. Hiermee kunt de Bus Service te garanderen en de consistentie van de sessiestatus bestellen bericht.

**PartitionKey**: als een bericht heeft de eigenschap [BrokeredMessage.PartitionKey][] , maar niet de [BrokeredMessage.SessionId][] eigenschap is ingesteld, wordt de Service Bus de eigenschap [PartitionKey][] wordt gebruikt als de partitiesleutel. Als het bericht zowel de [sessie-id][] en de [PartitionKey][] eigenschappen heeft, moet beide eigenschappen identiek zijn. Als de eigenschap [PartitionKey][] is ingesteld op een andere waarde dan de eigenschap [SessionId][] , retourneert Service Bus een **InvalidOperationException** -uitzondering. De eigenschap [PartitionKey][] moet worden gebruikt als een afzender niet sessie bewust transactionele berichten worden verzonden. De partitiesleutel zorgt ervoor dat alle berichten die binnen een transactie worden verzonden door de makelaar voor dezelfde berichten worden verwerkt.

**MessageId**: als het onderwerp of de wachtrij de eigenschap [QueueDescription.RequiresDuplicateDetection][] is ingesteld op **true heeft** en de eigenschappen [BrokeredMessage.SessionId][] of [BrokeredMessage.PartitionKey][] niet is ingesteld, wordt de eigenschap [BrokeredMessage.MessageId][] wordt gebruikt als de partitiesleutel. (Houd er rekening mee dat de bibliotheken van Microsoft .NET en AMQP een bericht-ID automatisch toewijzen als niet door de verzendende toepassing.) In dit geval worden alle kopieën van hetzelfde bericht verwerkt door de makelaar met hetzelfde bericht. Hierdoor kan de Service Bus te detecteren en elimineren dubbele berichten. Als de eigenschap [QueueDescription.RequiresDuplicateDetection][] niet is ingesteld op **true**, rekening Service Bus geen met de eigenschap [MessageId][] als een partitiesleutel.

### <a name="not-using-a-partition-key"></a>Niet met behulp van de partitiesleutel van een

Bij afwezigheid van een partitiesleutel verdeelt Service Bus berichten in een round robin-Mode op de fragmenten van de gepartitioneerde wachtrij of onderwerp. Als het gekozen fragment niet beschikbaar is, wordt het bericht naar een ander fragment in Service Bus toegewezen. Op deze manier slaagt de verzendbewerking ondanks de tijdelijke onbeschikbaarheid van een berichtenarchief.

Service Bus om moet voldoende tijd in de wachtrij plaatsen van het bericht in een ander fragment, de [MessagingFactorySettings.OperationTimeout][] -waarde die door de client die het bericht groter dan 15 seconden. Het is aanbevolen dat u de eigenschap [OperationTimeout][] ingesteld op de standaardwaarde van 60 seconden.

Houd er rekening mee dat een partitiesleutel "' een bericht naar een bepaald fragment kledingwinkelketen. Als u het berichtenarchief waarin dit fragment niet beschikbaar is, wordt een fout geretourneerd door Service Bus. Bij gebreke van een partitiesleutel Service Bus een ander fragment kunt kiezen en de bewerking is geslaagd. Daarom verdient het aanbeveling een partitiesleutel niet te verstrekken indien noodzakelijk.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Geavanceerde onderwerpen: transacties met entiteiten gepartitioneerde gebruiken

Berichten die worden verzonden als onderdeel van een transactie moeten een partitiesleutel opgeven. Dit is een van de volgende eigenschappen: [BrokeredMessage.SessionId][], [BrokeredMessage.PartitionKey][]of [BrokeredMessage.MessageId][]. Alle berichten die worden verzonden als onderdeel van dezelfde transactie moeten dezelfde partitiesleutel opgeven. Als u een bericht zonder partitiesleutel binnen een transactie verzendt probeert, wordt Service Bus een **InvalidOperationException** -uitzondering. Als u probeert te verzenden van meerdere berichten binnen dezelfde transactie met andere partitiesleutels, retourneert Service Bus een **InvalidOperationException** -uitzondering. Bijvoorbeeld:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Als de eigenschappen die als een partitiesleutel dienen zijn ingesteld, kledingwinkelketen Bus-Service het bericht naar een bepaald fragment. Dit probleem treedt ook op als een transactie wordt gebruikt. Het wordt aanbevolen dat u een partitiesleutel opgeeft als het niet nodig is.

## <a name="using-sessions-with-partitioned-entities"></a>Met behulp van sessies met gepartitioneerde entiteiten

Als u wilt een transactioneel bericht naar een sessie bekend onderwerp of een wachtrij stuurt, moet het bericht de [BrokeredMessage.SessionId][] eigenschap zijn ingesteld. Als de eigenschap [BrokeredMessage.PartitionKey][] ook is opgegeven, moet deze zijn identiek aan de eigenschap [SessionId][] . Als ze verschillen, wordt Service Bus een **InvalidOperationException** -uitzondering.

In tegenstelling tot gewone (niet-gepartitioneerde) wachtrijen of onderwerpen is het niet mogelijk om met een enkele transactie meerdere berichten verzenden naar sessies van andere. Als geprobeerd is, retourneert Service Bus een **InvalidOperationException **-uitzondering. Bijvoorbeeld:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Automatische bericht doorsturen met gepartitioneerde entiteiten

Azure Service Bus ondersteunt automatische bericht doorsturen van, naar of tussen gepartitioneerde entiteiten. Als u wilt dat automatische bericht doorstuurt, stelt u de eigenschap [QueueDescription.ForwardTo][] op de wachtrij of abonnement. Als het bericht een partitiesleutel ([sessie-id][], [PartitionKey][]of [MessageId][]) is opgegeven, wordt die partitiesleutel gebruikt voor de doelentiteit.

## <a name="considerations-and-guidelines"></a>Richtlijnen en overwegingen

- **Functies voor hoge consistentie**: als een entiteit maakt gebruik van functies zoals sessies, detectie van dubbele of expliciete controle van het partitioneren van de sleutel, is de messaging bewerkingen zijn altijd doorgestuurd naar specifieke fragmenten. Als een van de fragmenten veel verkeer ondervindt of de onderliggende opslag beschadigd is, deze bewerkingen mislukken en beschikbaarheid wordt beperkt. Over het algemeen is de consistentie nog steeds veel hoger dan de niet-gepartitioneerde entiteiten; alleen een subset van verkeer ondervindt problemen, in plaats van alle verkeer.
- **Management**: bewerkingen zoals het maken, bijwerken en verwijderen van de fragmenten van de entiteit moeten worden uitgevoerd. Als een fragment beschadigd is kan dit resulteren in fouten voor deze bewerkingen. Voor de bewerking Get moet informatie zoals telt bericht worden samengevoegd uit alle fragmenten. Als een fragment beschadigd is, wordt de status van de beschikbaarheid van entiteit vermeld als beperkt.
- **Scenario's bericht voor laag volume**: voor dergelijke scenario's, met name wanneer het HTTP-protocol, wellicht voor het uitvoeren van meerdere bewerkingen voor het verkrijgen van alle berichten te ontvangen. Voor ontvangstaanvragen, de front-endtoepassing voert een ontvangen op alle fragmenten en slaat alle antwoorden ontvangen. Een aanvraag ontvangen voor latere op dezelfde verbinding zou profiteren van deze in de cache opslaan en ontvangen vertragingstijden lager zijn. Als u meerdere verbindingen hebt of HTTP, constateert dat een nieuwe verbinding voor elke aanvraag. Als zodanig is er geen garantie dat het land zou op hetzelfde knooppunt. Als alle bestaande berichten zijn vergrendeld en in een andere front-end opgeslagen, retourneert de ontvangstbewerking **null**. Berichten uiteindelijk is verlopen en u opnieuw kunt ontvangen. HTTP keep-alive wordt aanbevolen.
- **Bladeren/Peek, berichten**: PeekBatch het aantal berichten dat is opgegeven in de [eigenschap MessageCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecount.aspx)niet altijd terug. Er zijn twee veelvoorkomende redenen. Een reden is dat de geaggregeerde grootte van de collectie van berichten groter is dan de maximale grootte van 256KB. Een andere reden is dat als de wachtrij of onderwerp de [eigenschap EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) ingesteld op **true heeft**, een partitie kan niet genoeg berichten voor het voltooien van het gevraagde aantal berichten. In het algemeen als een toepassing wil het aantal berichten ontvangt, moet dit aanroepen [PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) herhaaldelijk totdat u met deze eigenschap wordt het aantal berichten van, of er zijn geen berichten meer te kijken. Zie voor meer informatie, inclusief codevoorbeelden, [QueueClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) of [SubscriptionClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.peekbatch.aspx).

## <a name="latest-added-features"></a>Laatste toegevoegde functies

- Toevoegen of verwijderen regel wordt nu ondersteund met gepartitioneerde entiteiten. Anders dan niet-gepartitioneerde entiteiten, deze bewerkingen worden niet ondersteund in transacties. 
- AMQP wordt nu ondersteund voor het verzenden en ontvangen van berichten en van een gepartitioneerde entiteit.
- AMQP wordt nu ondersteund voor de volgende bewerkingen: [Batch verzenden](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.sendbatch.aspx), [Batch wordt](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.receivebatch.aspx) [ontvangen door het volgnummer](https://msdn.microsoft.com/library/azure/hh330765.aspx), [Peek](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peek.aspx), [Lock vernieuwen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.renewmessagelock.aspx), [Schema bericht](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.schedulemessageasync.aspx), [Gepland bericht annuleren](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.cancelscheduledmessageasync.aspx), [Regel toevoegen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Regel verwijderen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Sessie vernieuwen vergrendelen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.renewlock.aspx), [Set Session State](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx), [Get Session State](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx), [Peek sessie berichten](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.peek.aspx)en [Sessies opsommen](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.getmessagesessionsasync.aspx).

## <a name="partitioned-entities-limitations"></a>Beperkingen van gepartitioneerde entiteiten

Momenteel Service Bus gelden de volgende beperkingen van gepartitioneerde wachtrijen en onderwerpen:

-   Gepartitioneerde wachtrijen en onderwerpen bieden geen ondersteuning voor het verzenden van berichten die deel uitmaken van verschillende sessies in een enkele transactie.
-   Bus service kan momenteel maximaal 100 gepartitioneerde wachtrijen of onderwerpen per naamruimte. Elke wachtrij gepartitioneerde of onderwerp telt op het quotum van 10.000 entiteiten per naamruimte (geldt niet voor de Premium-laag).

## <a name="next-steps"></a>Volgende stappen

Zie de bespreking van [AMQP 1.0-ondersteuning voor Service Bus worden gepartitioneerd wachtrijen en onderwerpen][] voor meer informatie over het partitioneren van messaging entiteiten. 

  [Service Bus-architectuur]: service-bus-architecture.md
  [Azure portal]: https://portal.azure.com
  [QueueDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [TopicDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx
  [BrokeredMessage.SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [BrokeredMessage.PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [Sessie-id]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [MessagingFactorySettings.OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [AMQP 1.0-ondersteuning voor Service Bus worden gepartitioneerd wachtrijen en onderwerpen]: service-bus-partitioned-queues-and-topics-amqp-overview.md

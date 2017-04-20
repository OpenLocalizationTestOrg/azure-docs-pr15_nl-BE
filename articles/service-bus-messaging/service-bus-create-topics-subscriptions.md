<properties 
    pageTitle="Toepassingen die gebruikmaken van de Service Bus onderwerpen en abonnementen maken | Microsoft Azure"
    description="Inleiding tot het publiceren-abonneren mogelijkheden aangeboden door Service Bus onderwerpen en abonnementen."
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

# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Toepassingen die gebruikmaken van de Service Bus onderwerpen en abonnementen maken

Azure Service Bus ondersteunt een reeks met inbegrip van betrouwbare message Queuing-cloud-gebaseerde, message-oriented middleware-technologieën en duurzaam publiceren/abonneren messaging. Dit artikel is gebaseerd op de informatie in [maken toepassingen die gebruikmaken van wachtrijen Service Bus](service-bus-create-queues.md) en biedt een inleiding tot het publiceren/abonneren mogelijkheden aangeboden door Service Bus onderwerpen.

## <a name="evolving-retail-scenario"></a>Retail-scenario in ontwikkeling

Dit artikel blijft het retail-scenario gebruikt in [maken toepassingen die gebruikmaken van wachtrijen Service Bus](service-bus-create-queues.md). Intrekken die verkoopgegevens van afzonderlijke punt van verkoop POS-terminals moeten worden gerouteerd naar een voorraad management systeem waarin die gegevens worden gebruikt om te bepalen wanneer de voorraad is aangevuld worden. De verkoopcijfers rapporten elk POS-terminal door het verzenden van berichten naar de wachtrij van de **DataCollectionQueue** , waar ze blijven totdat ze worden ontvangen door de voorraad management systeem, zoals hier wordt weergegeven:

![Service Bus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

Om dit scenario evolueren, een nieuwe eis is toegevoegd aan het systeem: de winkeleigenaar van de wil kunnen controleren hoe de winkel het in real-time.

Om deze eis, moet het systeem "Tik" uit de verkoop van de gegevensstroom. Willen we nog steeds elk bericht dat is verzonden door de POS-terminals worden verzonden naar de voorraad management systeem voor, maar we willen nog een kopie van elk bericht dat we gebruiken kunnen om de dashboardweergave naar de winkeleigenaar van de.

U kunt in een situatie zoals dit, waarin u elk bericht dat wordt verbruikt door meerdere partijen nodig Service Bus *onderwerpen*. Onderwerpen bevatten een patroon publiceren/abonneren waarin elke gepubliceerde bericht beschikbaar wordt gemaakt voor een of meer abonnementen geregistreerd bij het onderwerp. In tegenstelling met wachtrijen elk bericht ontvangen door een enkele consument.

Berichten worden verzonden naar een onderwerp op dezelfde manier als ze naar een wachtrij worden verzonden. Echter worden berichten niet ontvangen van het onderwerp direct; ze worden ontvangen van abonnementen. U kunt een abonnement op een onderwerp als een virtuele wachtrij die u ontvangt een exemplaar van de berichten die worden verzonden naar dat onderwerp zien. Berichten worden ontvangen van een abonnement op dezelfde manier zoals ze zijn ontvangen uit een wachtrij.

Gaat het scenario voor de detailhandel, de wachtrij wordt vervangen door een onderwerp en een abonnement wordt toegevoegd, die de voorraad management system-component kunt gebruiken. Het systeem wordt nu als volgt weergegeven:

![Service Bus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

De configuratie hier voert identiek aan het vorige ontwerp op basis van wachtrijen. Dat wil zeggen worden berichten die worden verzonden naar het onderwerp gerouteerd naar het abonnement waarvan het **Voorraadbeheersysteem** deze verbruikt **voorraad** .

Ter ondersteuning van het management dashboard maken we een tweede abonnement op het onderwerp, zoals hier wordt weergegeven:

![Service Bus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

Met deze configuratie wordt elk bericht van de POS-terminals beschikbaar gemaakt voor het **Dashboard** en de **voorraad** abonnementen.

## <a name="show-me-the-code"></a>De code weergeven

[Maken toepassingen die gebruikmaken van Service Bus wachtrijen](service-bus-create-queues.md) beschreven hoe aanmelden voor een account met Azure en maakt een Servicenaamruimte. Een toepassing moet verwijzen naar de assemblage Service Bus, specifiek Microsoft.ServiceBus.dll voor het gebruik van een naamruimte Service Bus. De eenvoudigste manier om te verwijzen naar de Bus Service afhankelijkheden is de Service Bus [Nuget package](https://www.nuget.org/packages/WindowsAzure.ServiceBus/)installeren. U kunt ook de assembly als onderdeel van de SDK Azure vinden. De download is beschikbaar op de [downloadpagina van Azure SDK](https://azure.microsoft.com/downloads/).

### <a name="create-the-topic-and-subscriptions"></a>Het onderwerp en abonnementen maken

Beheerbewerkingen voor Service Bus messaging (wachtrijen en publiceren/abonneren onderwerpen) entiteiten worden uitgevoerd via de klasse [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) . Juiste referenties zijn vereist om te kunnen maken van een exemplaar van de [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) voor een bepaalde naamruimte. Bus-service maakt gebruik van een [Gedeeld Access handtekening (SAS)](service-bus-sas-overview.md) gebaseerde beveiligingsmodel. De klasse [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) vertegenwoordigt een beveiligingstokenprovider fabriek ingebouwde methoden die sommige bekende token providers te retourneren. Voor het opslaan van de SAS-referenties gebruiken we een methode [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) . Het [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) exemplaar is vervolgens samengesteld met het basisadres van de Service Bus-naamruimte en de token provider.

De klasse [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) bevat methoden voor het maken, opsommen en verwijderen van messaging entiteiten. De hier getoonde code ziet u hoe het [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) exemplaar is gemaakt en gebruikt voor het maken van het onderwerp **DataCollectionTopic** .

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
     
TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);
 
namespaceManager.CreateTopic("DataCollectionTopic");
```

Opmerking Er zijn overbelastingen waarmee u de eigenschappen van het onderwerp van de methode [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) . U kunt bijvoorbeeld instellen dat de standaard time-to-live (TTL) waarde voor berichten die worden verzonden naar het onderwerp. Voeg vervolgens de **voorraad** en de **Dashboard** -abonnementen.

```
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-to-the-topic"></a>Berichten verzenden naar het onderwerp

Voor runtime-bewerkingen voor Service Bus entiteiten; bijvoorbeeld berichten verzenden en ontvangen, een toepassing moet eerst een [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) object maken. Vergelijkbaar met de klasse [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , het [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) exemplaar is gemaakt op basis van het basisadres van de naamruimte van de service en de token provider.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Berichten worden verzonden naar en ontvangen van Service Bus onderwerpen, zijn instanties van de klasse [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Deze klasse bestaat uit een set van standaard-eigenschappen (zoals [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) en [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), een woordenlijst die wordt gebruikt om eigenschappen van toepassingen en een instantie van een willekeurige toepassingsgegevens. Een toepassing kan de instantie door doorgeven in een geserialiseerd object (in het volgende voorbeeld wordt doorgegeven in een **SalesData** -object met de verkoopgegevens van de POS-terminal), die de [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) gebruikt voor het serialiseren van het object ingesteld. U kunt ook kan een [Stream](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) -object worden geleverd.

```
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

De eenvoudigste manier om berichten te verzenden naar het onderwerp is een [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) -object rechtstreeks vanuit het [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) -exemplaar maken met [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) .

```
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Berichten ontvangen van een abonnement

Dit is vergelijkbaar met het gebruik van wachtrijen, om berichten te ontvangen van een abonnement die kunt u een [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) -object waarmee u rechtstreeks vanuit de [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) met [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx)maken. U kunt een van de twee verschillende modi (**ReceiveAndDelete** en **PeekLock**) ontvangen, zoals besproken in [maken toepassingen die gebruikmaken van wachtrijen Service Bus](service-bus-create-queues.md).

Houd er rekening mee dat bij het maken van een [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) voor abonnementen met de parameter *entityPath* van het formulier is `topicPath/subscriptions/subscriptionName`. Daarom wilt maken van een [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) voor het abonnement van de **voorraad** van het onderwerp **DataCollectionTopic** , *entityPath* moet worden ingesteld op `DataCollectionTopic/subscriptions/Inventory`. De code er als volgt uitziet:

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

## <a name="subscription-filters"></a>Abonnement filters

Tot nu toe in dit geval worden alle berichten die naar het onderwerp beschikbaar gesteld aan alle geregistreerde abonnementen. Hier de belangrijkste woordgroep is "ter beschikking gesteld." Terwijl abonnementen Service Bus alle berichten die naar het onderwerp ziet, kunt u alleen een subset van deze berichten naar de wachtrij virtuele abonnement. Dit wordt uitgevoerd met behulp van abonnement *filters*. Wanneer u een abonnement maakt, kunt u een expressie in de vorm van een SQL92-standaard stijl predikaat dat via de eigenschappen van het bericht, zowel de eigenschappen van het bestandssysteem (bijvoorbeeld [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)) en de toepassingseigenschappen, zoals **StoreName** in het vorige voorbeeld werkt geven.

In het scenario dient ter illustratie van deze ontwikkeling, is een tweede winkel moet worden toegevoegd aan onze retail-scenario. Verkoopgegevens van alle van de POS-terminals van beide winkels nog steeds moet worden doorgestuurd naar het centrale inventory management system, maar een winkelmanager hulpprogramma voor het dashboard is alleen geïnteresseerd in de prestaties van die winkel. U kunt filteren dit abonnement. Houd er rekening mee dat bij de POS-terminals berichten publiceren, ze de **StoreName** toepassing eigenschap op het bericht instellen. Twee winkels, bijvoorbeeld **Redmond** en **Seattle**, gegeven opslaan de POS-terminals in de Redmond stempel hun verkoopgegevens berichten met een **StoreName** gelijk aan **Redmond**, terwijl de Seattle winkel POS-terminals een **StoreName gebruikt** gelijk aan **Seattle**. Store manager van de winkel Roermond wil alleen gegevens van de POS-terminals. Het systeem wordt als volgt weergegeven:

![Service-Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

Als u dit bewerkingsplan wilt, maken u het abonnement **Dashboard** als volgt:

```
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

Met dit filter abonnement worden alleen berichten met de eigenschap **StoreName** is ingesteld op **Redmond** gekopieerd naar de virtuele wachtrij voor het abonnement **Dashboard** . Er is veel meer voor het abonnement filteren, echter. Toepassingen kunnen meerdere filterregels per abonnement naast de mogelijkheid voor het wijzigen van de eigenschappen van een bericht als dit wordt doorgegeven aan de virtuele wachtrij van een abonnement hebben.

## <a name="summary"></a>Samenvatting

Alle redenen om queuing beschreven in [toepassingen die gebruikmaken van Service Bus wachtrijen maken](service-bus-create-queues.md) gebruiken ook van toepassing op onderwerpen, met name:

- Tijdelijke ontkoppeling – bericht producenten en consumenten hebben niet tegelijkertijd online zijn.

- Laden van herverdeling: pieken in de belasting door het beslag toepassingen worden ingericht voor de gemiddelde belasting in plaats van piekbelasting onderwerp zijn uitgevlakt.

- Taakverdeling – net als bij een wachtrij, kunt u meerdere concurrerende consumenten luisteren op een enkel abonnement met elk bericht dat is doorgegeven aan slechts één van de consumenten, waardoor load balancing hebben.

- Losse koppeling – kunt u het netwerk ontwikkelen zonder de bestaande eindpunten; bijvoorbeeld abonnementen toevoegen of wijzigen van filters in een onderwerp dat voor nieuwe consumenten.

## <a name="next-steps"></a>Volgende stappen

Zie [toepassingen die gebruikmaken van Service Bus wachtrijen maken](service-bus-create-queues.md) voor informatie over het gebruik van wachtrijen in de retail POS-scenario.
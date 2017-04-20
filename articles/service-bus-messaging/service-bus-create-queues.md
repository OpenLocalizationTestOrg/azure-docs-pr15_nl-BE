<properties 
    pageTitle="Schrijven van toepassingen die gebruikmaken van wachtrijen Service Bus | Microsoft Azure"
    description="Het schrijven van een eenvoudige wachtrij-gebaseerde toepassing die gebruikmaakt van de Service Bus."
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
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-queues"></a>Toepassingen die gebruikmaken van de Service Bus wachtrijen maken

In dit onderwerp wordt beschreven Service Bus wachtrijen en ziet u hoe u een eenvoudige wachtrij-gebaseerde toepassing die gebruikmaakt van de Service Bus schrijven.

Neem een scenario uit de wereld van retail waarin verkoopgegevens van terminals voor afzonderlijke Point-of-Sale (POS) moet worden gerouteerd naar een voorraad management systeem dat de gegevens worden gebruikt om te bepalen wanneer de voorraad is aangevuld worden. Deze oplossing wordt Service Bus berichten voor de communicatie tussen de terminals en het inventory management system, zoals geïllustreerd in de volgende afbeelding:

![Afbeelding van de wachtrijen Service Bus 1](./media/service-bus-create-queues/IC657161.gif)

De verkoopcijfers rapporten elk POS-terminal door berichten te sturen naar de **DataCollectionQueue**. Deze berichten blijven in deze wachtrij totdat zij worden opgehaald door het voorraadbeheersysteem. Dit patroon wordt *asynchroon berichtenverkeer*, vaak genoemd, omdat de POS-terminal niet hoeft te wachten op een antwoord van de voorraad management systeem om te gaan met de verwerking.

## <a name="why-queuing"></a>Waarom queuing?

Voordat we naar de code die nodig is voor deze toepassing hebt ingesteld, kunt u de voordelen van een wachtrij in dit scenario in plaats van de POS-terminals rechtstreeks praten (synchroon) aan het inventory management system.

### <a name="temporal-decoupling"></a>Tijdelijke ontkoppeling

Met de asynchrone berichtenafhandeling patroon, producenten en consumenten geen tegelijkertijd online zijn. De infrastructuur voor berichtafhandeling op betrouwbare wijze berichten worden opgeslagen totdat de verbruikende partij is klaar om te ontvangen. Dit betekent dat de onderdelen van de gedistribueerde toepassing wordt verbroken, hetzij uit eigen beweging; bijvoorbeeld voor onderhoud, of als gevolg van een crash component, zonder invloed op het hele systeem. Bovendien hoeft de toepassing in beslag nemen gedurende bepaalde periodes van de dag online zijn. In dit scenario retail kan het voorraadbeheersysteem alleen bijvoorbeeld on line worden gebracht na het einde van de werkdag.

### <a name="load-leveling"></a>Herverdeling laden

In veel toepassingen varieert systeembelasting na verloop van tijd dat de verwerkingstijd voor elke eenheid van werk doorgaans constant is. Intermediating bericht producenten en consumenten met een wachtrij betekent dat het beslag toepassing (werknemer) alleen heeft een gemiddelde belasting in plaats van een belasting-service worden ingericht. De diepte van de wachtrij kan groeien en de binnenkomende belastingstoestand contract. Dit bespaart u direct geld op het gebied van de infrastructuur die nodig zijn voor de belasting van toepassing.

![Afbeelding van de wachtrijen Service Bus 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Taakverdeling

Als de belasting toeneemt, kunnen er meer werkprocessen te lezen uit de wachtrij van de werknemer worden toegevoegd. Elk bericht wordt verwerkt door slechts één van de werkprocessen. Bovendien kan deze pull-based taakverdeling voor het optimale gebruik van de computers van de werknemer zelfs als de werknemer computers met betrekking tot de verwerkingskracht verschillen, terwijl ze hun eigen maximale snelheid berichten haalt. Dit patroon wordt vaak aangeduid als het concurrerende patroon van de consument.

![Afbeelding van de wachtrijen Service Bus 3](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Losse koppeling

Werken met message queuing op tussenliggende tussen bericht producenten en consumenten biedt een ingebouwde losse koppeling tussen de onderdelen. Omdat de producenten en consumenten zich niet bewust zijn van elkaar, kan een consument zonder gevolgen voor de producent worden bijgewerkt. Bovendien kan de topologie messaging zich ontwikkelen zonder dat de bestaande eindpunten. We bespreken dit meer als we over het publiceren/abonneren praten.

## <a name="show-me-the-code"></a>De code weergeven

In het volgende gedeelte ziet u hoe Bus-Service gebruiken om deze toepassing te maken.

### <a name="sign-up-for-an-azure-account"></a>Aanmelden voor een account Azure

Een Azure-account moet u werken met Bus-Service gestart. Als u nog geen een, kunt u zich aanmelden voor een gratis account [hier](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Een naamruimte maken

Als u een abonnement hebt, kunt u [een nieuwe naamruimte maken](service-bus-create-namespace-portal.md). Elke naamruimte fungeert als een container voor een reeks Service Bus entiteiten scope. Geef uw nieuwe naamruimte een unieke naam alle Bus Service accounts. 

### <a name="install-the-nuget-package"></a>Installeer het pakket NuGet

Een toepassing moet verwijzen naar de assemblage Service Bus, specifiek Microsoft.ServiceBus.dll voor het gebruik van de Service Bus-naamruimte. U vindt deze assembly als onderdeel van de SDK van Microsoft Azure en de download is beschikbaar op de [downloadpagina van Azure SDK](https://azure.microsoft.com/downloads/). De [Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus) is echter de eenvoudigste manier om de Service Bus-API en voor het configureren van uw toepassing met alle afhankelijkheden Service Bus.

### <a name="create-the-queue"></a>De wachtrij maken

Beheerbewerkingen voor Service Bus messaging (wachtrijen en publiceren/abonneren onderwerpen) entiteiten worden uitgevoerd via de klasse [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) . Bus-service maakt gebruik van een [Gedeeld Access handtekening (SAS)](service-bus-sas-overview.md) gebaseerde beveiligingsmodel. De klasse [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) vertegenwoordigt een beveiligingstokenprovider fabriek ingebouwde methoden die sommige bekende token providers te retourneren. Voor het opslaan van de SAS-referenties gebruiken we een methode [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) . Het [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) exemplaar is vervolgens samengesteld met het basisadres van de Service Bus-naamruimte en de token provider.

De klasse [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) bevat methoden voor het maken, opsommen en verwijderen van messaging entiteiten. De hier getoonde code ziet u hoe het [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) exemplaar is gemaakt en gebruikt voor het maken van de **DataCollectionQueue** -wachtrij.

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", 
                "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
 
TokenProvider tokenProvider = 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = 
    new NamespaceManager(uri, tokenProvider);
namespaceManager.CreateQueue("DataCollectionQueue");
```

Opmerking Er zijn overbelastingen van de [CreateQueue](https://msdn.microsoft.com/library/azure/hh322663.aspx) -methode waarmee u de eigenschappen van de wachtrij moet worden afgestemd. U kunt bijvoorbeeld instellen dat de standaard time-to-live (TTL) waarde voor berichten die worden verzonden naar de wachtrij.

### <a name="send-messages-to-the-queue"></a>Berichten naar de wachtrij verzenden.

Voor runtime-bewerkingen voor Service Bus entiteiten; bijvoorbeeld berichten verzenden en ontvangen, een toepassing moet eerst een [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) object maken. Vergelijkbaar met de klasse [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , het [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) exemplaar is gemaakt op basis van het basisadres van de naamruimte van de service en de token provider.

```
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Berichten worden verzonden naar en ontvangen van Service Bus wachtrijen zijn instanties van de klasse [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . Deze klasse bestaat uit een set van standaard-eigenschappen (zoals [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) en [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), een woordenlijst die wordt gebruikt om eigenschappen van toepassingen en een instantie van een willekeurige toepassingsgegevens. Een toepassing kan de instantie door doorgeven in een geserialiseerd object (in het volgende voorbeeld wordt doorgegeven in een **SalesData** -object met de verkoopgegevens van de POS-terminal), die de [DataContractSerializer](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) gebruikt voor het serialiseren van het object ingesteld. U kunt ook kan een [Stream](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) -object worden geleverd.

De eenvoudigste manier om berichten te verzenden naar een bepaalde wachtrij, in ons geval de **DataCollectionQueue**is een [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) -object rechtstreeks vanuit het [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) -exemplaar maken met [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) .

```
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-the-queue"></a>Ontvangen van berichten uit de wachtrij

Voor het ontvangen van berichten uit de wachtrij, kunt u een [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) -object waarmee u rechtstreeks vanuit de [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) met [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx)maken. Ontvangers van berichten kunnen in twee verschillende modi werken: **ReceiveAndDelete** en **PeekLock**. De [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) wordt ingesteld wanneer de ontvanger van het bericht is gemaakt, als een parameter voor het aanroepen van [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx) .


Wanneer u de **ReceiveAndDelete** -modus, is de ontvangen een single-shot-bewerking; dat wil zeggen, wanneer Bus-Service het verzoek ontvangt, het markeren van het bericht worden gebruikt en terug naar de toepassing. **ReceiveAndDelete** -modus is het eenvoudigste model en het meest geschikt voor scenario's waarin de toepassing verwerken van een bericht niet tolereren kan als een fout optreedt. Om dit te begrijpen, kunt u overwegen een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze worden verwerkt. Omdat Service Bus gemarkeerd als wordt gebruikt wanneer de toepassing wordt opnieuw opgestart en die berichten opnieuw is gestart, deze zal hebt gemist het bericht dat is verbruikt vóór de crash.

In de **PeekLock** modus wordt de ontvangen een bewerking van twee fasen, waardoor het mogelijk is voor de ondersteuning van toepassingen die niet kunnen ontbrekende berichten tolereren. Wanneer Bus-Service het verzoek ontvangt, gezocht het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten ontvangen en stuurt het naar de toepassing. Nadat de toepassing is het bericht verwerkt (of betrouwbaar voor toekomstige verwerking opgeslagen), wordt de tweede fase van de procedure ontvangen door het aanroepen van [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) voor het ontvangen bericht voltooid. Wanneer Service Bus [volledig](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) aanroept, worden gemarkeerd als wordt verbruikt.

Twee andere uitkomsten zijn mogelijk. Eerst, als de toepassing niet kan het bericht verwerkt voor een of andere reden, kan aanroepen [verlaten](https://msdn.microsoft.com/library/azure/hh181837.aspx) op het ontvangen bericht ( [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)). Dit zorgt ervoor dat de Service Bus te ontgrendelen van het bericht en moet opnieuw worden ontvangen door de consument dezelfde of een andere voltooien consument beschikbaar. Ten tweede, er is een time-out van de vergrendeling en als de toepassing het bericht kan niet worden verwerkt voordat de vergrendeling time-out verloopt (bijvoorbeeld, als de toepassing loopt vast) en vervolgens Service Bus wordt het bericht ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen (er in feite een bewerking [Annuleren](https://msdn.microsoft.com/library/azure/hh181837.aspx) standaard).

Houd er rekening mee dat als de toepassing vastloopt nadat het bericht verwerkt, maar voordat de [volledige](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) aanvraag is ingediend, het bericht zal worden geleverd aan de toepassing opnieuw wordt gestart. Dit wordt *Tenminste eenmaal* verwerking vaak genoemd. Dit betekent dat elk bericht wordt ten minste eenmaal verwerkt, maar in bepaalde situaties kan hetzelfde bericht worden geleverd. Als het scenario kan geen dubbele verwerking tolereren, is extra logica vereist in de toepassing voor het detecteren van dubbele records. Dit kan worden bereikt op basis van de eigenschap [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) van het bericht. De waarde van deze eigenschap blijft constant over de leveringspogingen. Dit wordt *Slechts éénmaal* verwerking genoemd.

De code die er als volgt ontvangen en een bericht met behulp van de **PeekLock** -modus de standaardinstelling is als geen waarde [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) uitdrukkelijk verwerkt.

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionQueue");
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

### <a name="use-the-queue-client"></a>De wachtrij-client

De voorbeelden die eerder in deze sectie [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) en [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objecten rechtstreeks vanuit de [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) voor het verzenden en ontvangen van berichten uit de wachtrij, respectievelijk gemaakt. Een alternatieve methode is het gebruik van de klasse [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) , welke ondersteunt zowel bij het verzenden en ontvangen van bewerkingen meer geavanceerde functies, zoals de sessies.

```
QueueClient queueClient = factory.CreateQueueClient("DataCollectionQueue");
queueClient.Send(bm);
            
BrokeredMessage message = queueClient.Receive();

try
{
    ProcessMessage(message);
    message.Complete();
}
catch (Exception e)
{
    message.Abandon();
} 
```

## <a name="next-steps"></a>Volgende stappen

Nu u de basisbeginselen van wachtrijen hebt geleerd, kijken [maken toepassingen die Service Bus onderwerpen en abonnementen gebruiken](service-bus-create-topics-subscriptions.md) deze discussie met de mogelijkheden voor publiceren/abonneren Service Bus onderwerpen en abonnementen.
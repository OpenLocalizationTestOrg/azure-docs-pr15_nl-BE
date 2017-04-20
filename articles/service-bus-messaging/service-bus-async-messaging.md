<properties 
    pageTitle="Service Bus voor asynchrone berichtafhandeling | Microsoft Azure"
    description="Beschrijving van Service Bus voor asynchrone berichtafhandeling."
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

# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Asynchrone berichtenafhandeling patronen en hoge beschikbaarheid

Systeem voor asynchrone berichtafhandeling kan op verschillende manieren worden geïmplementeerd. Azure Service Bus ondersteunt met wachtrijen, onderwerpen en abonnementen, asynchrony via een winkel en een mechanisme vooruit. In normale werking (synchroon), kunt u berichten verzenden naar wachtrijen en onderwerpen en berichten ontvangen van wachtrijen en abonnementen. Toepassingen die u schrijft, is afhankelijk van deze entiteiten worden altijd beschikbaar. Wanneer de gezondheid van de entiteit als gevolg van verschillende omstandigheden wijzigt, moet u een oplossing voor een beperkte mogelijkheid entiteit die aan de meeste behoeften kan voldoen.

Patronen voor asynchrone berichtenafhandeling toepassingen meestal gebruikt om een aantal scenario's voor communicatie. Toepassingen waarin clients berichten naar services, verzenden kunnen zelfs als de service niet wordt uitgevoerd, kunt u bouwen. Voor toepassingen die ervaring bursts van communicatie is dit een wachtrij kunnen helpen bij het laden niveau door middel van een interne buffer communicatie. Ten slotte krijgt u een eenvoudige maar effectieve taakverdeling berichten op meerdere computers te distribueren.

Om de beschikbaarheid van een van deze entiteiten, kunt u overwegen een aantal verschillende manieren waarop deze entiteiten kunnen worden weergegeven niet beschikbaar voor een duurzaam berichtensysteem. In het algemeen zien we de entiteit niet meer beschikbaar voor toepassingen die we op de volgende manieren schrijven:

- Kan geen berichten verzenden.

- Kan geen berichten ontvangen.

- Kan geen entiteiten beheren (maken, ophalen, bijwerken of verwijderen van entiteiten).

- Kan geen verbinding maken met de service.

Voor elk van deze storingen bestaan verschillende storingsmodi waarmee een toepassing om verder te werken op een niveau van verminderde functionaliteit. Bijvoorbeeld, een systeem dat u kunt berichten verzenden maar niet ontvangen krijgen orders van klanten maar die orders kan niet worden verwerkt. Dit onderwerp wordt besproken van potentiële problemen die kunnen optreden en hoe deze problemen worden beperkt. Bus-service heeft een aantal beperkingen die u moet kiezen in geïntroduceerd en in dit onderwerp wordt ook de regels voor het gebruik van de opt-in-oplossingen beschreven.

## <a name="reliability-in-service-bus"></a>Betrouwbaarheid in Service Bus

Er zijn verschillende manieren voor het verwerken van bericht-en entiteit en er zijn richtsnoeren voor het gebruik van deze beperkingen. Om de richtlijnen te begrijpen, moet u eerst begrijpen wat in de Bus-Service kan mislukken. Vanwege het ontwerp van Azure systemen, al deze problemen zijn meestal tijdelijk. Op een hoog niveau uitzien de verschillende oorzaken van onbeschikbaarheid als volgt:

-   Beperking van een extern systeem waarvan Bus Service afhankelijk is. Beperken plaatsvindt van de interacties met opslag-en compute.

-   Het probleem voor een systeem waarvan de Bus Service afhankelijk is. Een bepaald gedeelte van opslag kan bijvoorbeeld problemen ondervinden.

-   Slechte Service Bus op één subsysteem. In dit geval een compute node in een inconsistente toestand kunt krijgen en moet opnieuw worden opgestart, waardoor alle entiteiten die het dient te worden verdeeld naar andere knooppunten. Dit op zijn beurt kan leiden tot een korte periode van trage verwerking.

-   Slechte Service Bus binnen een datacenter Azure. Dit is een foutbericht 'Onherstelbare fout"gedurende welke het systeem niet bereikbaar voor een aantal minuten tot een paar uur is.

> [AZURE.NOTE] De term **opslag** kan leiden tot Azure opslag zowel SQL Azure.

Bus-service bevat een aantal oplossingen voor deze problemen. De volgende secties komen elk probleem en hun respectieve Beperkende factoren.

### <a name="throttling"></a>Bandbreedtebeperking

Met de Bus-Service kan beperken coöperatieve bericht frequentie management. Elke afzonderlijke Service Bus knooppunt ook veel entiteiten nieuwste. Elk van deze diensten kunt u aanvragen op het systeem in termen van CPU, geheugen, opslag en andere facetten. Wanneer een van deze aspecten gebruik detecteert die gedefinieerde drempels overschrijdt, Bus Service een bepaalde aanvraag kunt weigeren. De beller ontvangt een [ServerBusyException][] en pogingen na 10 seconden.

Als een beperking, moet de code de fout bij het lezen en alle pogingen van het bericht voor ten minste 10 seconden onderbroken. Omdat de fout zich in stukken van de Klanttoepassing voordoen kan, verwacht wordt dat elk stuk afzonderlijk wordt de logica opnieuw uitgevoerd. De code verkleint de kans van wordt verminderd doordat de partitionering van een wachtrij of onderwerp.

### <a name="issue-for-an-azure-dependency"></a>Probleem met een afhankelijkheid Azure

Andere Azure-onderdelen kunnen af en toe serviceproblemen hebben. Bijvoorbeeld, wanneer een systeem waarin Service Bus wordt bijgewerkt, kan dat systeem tijdelijk optreden minder mogelijkheden. Om dit soort problemen, Service Bus regelmatig onderzoekt en beperkende maatregelen worden geïmplementeerd. Neveneffecten van deze beperkingen worden weergegeven. Voor het afhandelen van problemen met opslag van voorbijgaande aard, bijvoorbeeld implementeert Service Bus een systeem waarmee het bericht verzenden bewerkingen op consistente wijze werken. Een verzonden bericht kan maximaal 15 minuten worden weergegeven in de desbetreffende wachtrij of abonnement en klaar voor een ontvangstbewerking nemen vanwege de aard van de oplossing. In het algemeen zullen de meeste entiteiten ondervinden dit probleem niet. Echter, gezien het aantal entiteiten Service Bus in Azure, deze beveiliging is soms nodig voor een klein aantal klanten Service Bus.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Failure Service Bus op een enkel subsysteem

Omstandigheden kunnen leiden tot een intern onderdeel van de Service Bus inconsistent worden met een willekeurige toepassing. Wanneer de Service Bus gedetecteerd, worden gegevens verzameld uit de toepassing als hulpmiddel bij het vaststellen van wat er is gebeurd. Zodra de gegevens worden verzameld, wordt de toepassing gestart in een poging het terugkeren naar een consistente status. Dit proces gebeurt vrij snel en resultaten in een entiteit worden weergegeven zijn niet beschikbaar op een paar minuten als typische omlaag tijden zijn veel korter.

In deze gevallen genereert de clienttoepassing een uitzondering [System.TimeoutException][] of [MessagingException][] . Bus-service bevat een oplossing voor dit probleem in de vorm van logica voor automatische client opnieuw. Nadat de periode is uitgeput en het bericht niet is bezorgd, kunt u met behulp van andere functies zoals [gepaarde naamruimten][]kunt verkennen. Naamruimten paren hebben andere beperkingen die in dit artikel worden besproken.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Storing in een datacenter Azure Service Bus

De meest waarschijnlijke oorzaak van de fout in een datacenter Azure is een mislukte upgrade implementatie van Bus Service of een afhankelijke systeem. Als het platform is ontwikkeld, is de kans op dit soort storing afgenomen. Fout in een datacenter kan ook gebeuren om redenen die het volgende bevatten:

-   Elektrische storing (voeding en genereren power verdwijnen).
-   Verbinding (internet-einde tussen de clients en de Azure).

In beide gevallen heeft een natuurlijke of kunstmatige rampen het probleem veroorzaakt. U kunt dit probleem voorkomen en zorg ervoor dat u kunt nog steeds berichten verzenden, kunt u [paarsgewijs naamruimten][] waarmee berichten worden verzonden naar een tweede locatie terwijl u de primaire locatie opnieuw in orde is gemaakt. Zie [Aanbevolen procedures voor isolerende toepassingen tegen Bus Service storingen en calamiteiten][]voor meer informatie.

## <a name="paired-namespaces"></a>Gepaarde naamruimten

De functie [gepaarde naamruimten][] ondersteunt scenario's waarin een entiteit Service Bus of gebruik binnen een datacenter meer beschikbaar is. Tijdens deze gebeurtenis doet zich zelden voor, moeten gedistribueerde systemen nog steeds worden voorbereid voor het verwerken van worst case-scenario's. Meestal treedt deze gebeurtenis op omdat sommige element waarvan Bus Service afhankelijk is, is er een op korte termijn optreden. Om de beschikbaarheid van toepassingen behouden tijdens een stroomstoring, Service Bus gebruikers kunnen gebruiken twee aparte naamruimten, bij voorkeur in aparte datacenters, voor het hosten van hun messaging entiteiten. De rest van deze sectie worden de volgende terminologie gebruikt:

-   Primaire naamruimte: de naamruimte waarmee uw toepassing communiceert, voor het verzenden en ontvangen van bewerkingen.

-   Secundaire naamruimte: de naamruimte die als een back-up naar de primaire naamruimte fungeert. Toepassingslogica communiceert niet met deze naamruimte.

-   Failover-interval: de hoeveelheid tijd normale storingen accepteren voordat de toepassing wordt overgeschakeld van de primaire naamruimte naar de secundaire naamruimte.

Paren van naamruimten ondersteuning *verzenden beschikbaarheid*. Verzenden beschikbaarheid behoudt de mogelijkheid om berichten te verzenden. Uw toepassing moet voldoen aan de volgende vereisten voor het gebruik van verzenden beschikbaarheid:

1.  Berichten worden alleen uit de primaire naamruimte ontvangen.

2.  Berichten naar een bepaalde wachtrij of onderwerp kan een verkeerde volgorde binnenkomen.

3.  Als uw toepassing gebruikmaakt van sessies, kan berichten binnen een sessie verkeerde volgorde binnenkomen. Dit is een onderbreking van de normale functionaliteit van sessies. Dit betekent dat uw toepassing gebruikmaakt van sessies berichten logisch groeperen. De sessiestatus wordt alleen behouden op de primaire naamruimte.

4.  Berichten in een sessie kunnen een verkeerde volgorde binnenkomen. Dit is een onderbreking van de normale functionaliteit van sessies. Dit betekent dat uw toepassing gebruikmaakt van sessies berichten logisch groeperen.

5.  De sessiestatus wordt alleen behouden op de primaire naamruimte.

6.  De primaire wachtrij kan on line en berichten worden geaccepteerd voordat de tweede wachtrij alle berichten in de wachtrij primaire biedt start.

De volgende secties worden de API's, hoe de API's zijn geïmplementeerd en ziet u een voorbeeld van code die gebruikmaakt van de functie. Opmerking Er zijn facturering implicaties die zijn gekoppeld aan deze functie.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>De MessagingFactory.PairNamespaceAsync-API

De functie gepaarde naamruimten bevat de methode [PairNamespaceAsync][] op de [Microsoft.ServiceBus.Messaging.MessagingFactory][] -klasse:

```
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Wanneer de taak is voltooid, wordt ook de combinatie van de naamruimte is voltooid en klaar om te reageren op voor elke [MessageReceiver][], [QueueClient][], of [TopicClient][] gemaakt met de [MessagingFactory][] instantie. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions][] is de basisklasse voor de verschillende typen koppelen die beschikbaar in een [MessagingFactory][] -object zijn. Momenteel is de enige afgeleide klasse een benoemde [SendAvailabilityPairedNamespaceOptions][], waarmee de vereiste verzenden beschikbaarheid worden geïmplementeerd. [SendAvailabilityPairedNamespaceOptions][] heeft een reeks constructors die zijn gebaseerd op elkaar. De constructor kijken met de meeste parameters, kunt u het gedrag van de andere constructors begrijpen.

```
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Deze parameters hebben de volgende betekenis:

-   *secondaryNamespaceManager*: een geïnitialiseerde [NamespaceManager][] exemplaar voor de secundaire naamruimte die de methode [PairNamespaceAsync][] gebruiken kunt voor het instellen van de secundaire naamruimte. Namespace manager wordt gebruikt om de lijst van wachtrijen in de naamruimte te verkrijgen en om ervoor te zorgen dat de vereiste reserve wachtrijen bestaan. Als deze wachtrijen niet bestaan, worden gemaakt. [NamespaceManager][] moet kunnen maken van een token met de claim **beheren** .

-   *messagingFactory*: de [MessagingFactory][] instantie voor de secundaire naamruimte. Het [MessagingFactory][] -object wordt gebruikt voor het verzenden en als de eigenschap [EnableSyphon][] is ingesteld op **true**, worden berichten ontvangen van de reserve-wachtrijen.

-   *backlogQueueCount*: het aantal reserve-wachtrijen te maken. Deze waarde moet minimaal 1 zijn. Bij het verzenden van berichten naar de reserve, wordt een van deze wachtrijen willekeurig gekozen is. Als u de waarde op 1 instelt, kunt wordt slechts één wachtrij ooit gebruikt. Wanneer dit gebeurt en de reserve voor één wachtrij fouten genereert, wordt de client is niet mogelijk om te proberen een andere reserve-wachtrij en wordt mogelijk niet het bericht te verzenden. Als deze waarde op sommige grotere waarde en standaardwaarde de waarde 10 wordt aangeraden. U kunt dit wijzigen in een hogere of lagere waarde afhankelijk van hoeveel gegevens een toepassing worden verzonden per dag. Elke reserve-wachtrij kan maximaal 5 GB aan berichten bevatten.

-   *failoverInterval*: de hoeveelheid tijd waarin u fouten op de primaire naamruimte accepteren wilt voordat u overschakelt van een enkele entiteit via de secundaire naamruimte. Failover plaatsvinden op basis van de entiteit per entiteit. Entiteiten in één naamruimte live regelmatig in verschillende knooppunten binnen Service Bus. Een storing in één entiteit betekent niet dat een storing in de andere. U kunt deze waarde instellen op [System.TimeSpan.Zero][] voor failover naar de secundaire onmiddellijk na de eerste, een tijdelijke fout. Fouten die resulteren in de failover-timer, een [MessagingException][] waarin de eigenschap [IsTransient][] is ingesteld op false of een [System.TimeoutException][]zijn. Andere uitzonderingen zoals [UnauthorizedAccessException][] veroorzaken geen failover, omdat ze geven aan dat de client niet juist is geconfigureerd. Een [ServerBusyException][] veroorzaakt geen failover omdat het juiste patroon wacht 10 seconden, vervolgens het bericht opnieuw verzenden.

-   *enableSyphon*: Hiermee wordt aangegeven dat deze specifieke combinatie moet ook syphon berichten van de secundaire naamruimte terug naar de primaire naamruimte. Toepassingen die berichten verzenden moeten in het algemeen is deze waarde ingesteld op **false**; toepassingen die berichten te ontvangen, moeten deze waarde ingesteld op **true**. De reden hiervoor is dat vaak het geval is, er minder ontvangers van berichten dan afzenders van berichten zijn. Afhankelijk van het aantal ontvangers kunt u een enkele toepassingsinstantie verwerken de syphon rechten hebben. Met behulp van veel ontvangers facturering implicaties heeft voor elke wachtrij reserve.

Maakt gebruik van de code, een primaire [MessagingFactory][] instantie, een tweede exemplaar van de [MessagingFactory][] , een tweede exemplaar van de [NamespaceManager][] en een [SendAvailabilityPairedNamespaceOptions][] exemplaar. De oproep is net zo eenvoudig als het volgende:

```
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Als de taak die wordt geretourneerd door de methode [PairNamespaceAsync][] voltooit, is alles ingesteld en klaar voor gebruik. Voordat de taak als resultaat wordt gegeven, kan niet voltooid al het werk achtergrond nodig zijn voor de combinatie werken rechts. Als gevolg hiervan moet u niet starten berichten verzenden totdat de taak als resultaat gegeven. Als er fouten zijn opgetreden, zoals ongeldige referenties of bij het maken van de reserve-wachtrijen worden deze uitzonderingen gegenereerd zodra de taak is voltooid. Zodra de taak als resultaat geeft, controleert u of de wachtrijen zijn gevonden of gemaakt door de eigenschap [BacklogQueueCount][] op uw [SendAvailabilityPairedNamespaceOptions][] -exemplaar. Voor de bovenstaande code die bewerking wordt als volgt weergegeven:

```
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van een systeem voor asynchrone berichtafhandeling in Service Bus, lees meer informatie over [naamruimten paren][].

  [ServerBusyException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.serverbusyexception.aspx
  [System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [Aanbevolen procedures voor de isolatie van toepassingen tegen Bus Service storingen en calamiteiten]: service-bus-outages-disasters.md
  [Microsoft.ServiceBus.Messaging.MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [MessageReceiver]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [SendAvailabilityPairedNamespaceOptions]:https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [EnableSyphon]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.enablesyphon.aspx
  [System.TimeSpan.Zero]: https://msdn.microsoft.com/library/azure/system.timespan.zero.aspx
  [IsTransient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.istransient.aspx
  [UnauthorizedAccessException]: https://msdn.microsoft.com/library/azure/system.unauthorizedaccessexception.aspx
  [BacklogQueueCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.backlogqueuecount.aspx
  [gepaarde naamruimten]: service-bus-paired-namespaces.md
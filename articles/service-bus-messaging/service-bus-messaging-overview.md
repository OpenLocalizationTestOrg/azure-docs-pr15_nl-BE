<properties
    pageTitle="Messaging service Bus-overzicht | Microsoft Azure"
    description="Service Bus Messaging: levering flexibele data in de cloud"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="sethm"/>


# <a name="service-bus-messaging-flexible-data-delivery-in-the-cloud"></a>Messaging service Bus: levering van flexibele data in de cloud

Microsoft Azure Service Bus is een betrouwbare informatie bezorgservice. Het doel van deze service is om communicatie te vergemakkelijken. Wanneer twee of meer partijen wisselen informatie uit, ze hebben een mechanisme nodig. Service Bus is een mechanisme brokered of derden. Dit is vergelijkbaar met een postdienst in de fysieke wereld. Postdiensten u heel gemakkelijk voor het verzenden van verschillende soorten brieven en pakjes met allerlei garanties, levering, overal ter wereld.

Net als bij het postkantoor bezorgen brieven, Service Bus is flexibele levering van zowel de afzender als de geadresseerde. De messaging-service zorgt ervoor dat de informatie wordt geleverd, zelfs als de twee partijen nooit zowel on line op hetzelfde moment, of als ze zijn niet beschikbaar op exact hetzelfde moment. Op deze manier is berichten vergelijkbaar met het verzenden van een brief, terwijl niet-brokered communicatie vergelijkbaar is met het plaatsen van een telefoongesprek (of hoe een telefoongesprek te zijn - voordat het gesprek wachttijden en beller-ID, die veel meer lijken op brokered messaging gebruikt).

Afzender van het bericht kan ook een groot aantal kenmerken van levering met inbegrip van transacties, detectie van dubbele, op basis van tijd verlopen en batchen vereist. Deze patronen zijn ook per post vergelijkingen: Herhaal levering, handtekening vereist, adres wijzigen of intrekken.

Bus-service ondersteunt twee verschillende berichten patronen: *Relay* en *brokered messaging*.

## <a name="service-bus-relay"></a>Service Bus Relay

[Het relayonderdeel van de Bus-Service](../service-bus-relay/service-bus-relay-overview.md) is een centrale (maar zeer verdeeld) service een groot aantal verschillende transportprotocollen en normen ondersteunt. Dit geldt ook voor SOAP, WS-*, en zelfs de REST. De [relay-service](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) biedt allerlei verschillende relay connectiviteitsopties en kan helpen bij het onderhandelen over de directe peer-to-peer-verbindingen wanneer het mogelijk is. Service Bus is geoptimaliseerd voor .NET ontwikkelaars die gebruikmaken van Windows Communication Foundation (WCF), zowel ten aanzien van prestaties en het gebruiksgemak, en biedt volledige toegang tot de relay-service via SOAP en de REST-interfaces. Hierdoor kunnen voor een SOAP of REST programmeeromgeving te integreren met de Service Bus.

De relayservice ondersteunt de traditionele unilaterale messaging, aanvraag/reactie-berichten en peer-to-peer berichten. Verdeling in het Internet-bereik inschakelen gebeurtenis ondersteunt ook publicatie abonnementen scenario's en socket bi-directionele communicatie voor point-to-point verhoogde efficiëntie. In het patroon van het indirecte messaging, een service op locatie verbinding maakt met de relay-service via een uitgaande poort en maakt een bi-directionele socket voor communicatie is gebonden aan een bepaalde rendezvous-adres. De client kan vervolgens communiceren met de service op locatie door berichten te sturen naar de relayservice gericht op het adres rendezvous. De relay-service wordt vervolgens "" berichten doorsturen naar de on-premises-service via de bi-directionele socket reeds. De client hoeft niet een directe verbinding met de service in de lokalen, noch het nodig is om te weten waar de service is geïnstalleerd en de service op locatie hoeft niet alle poorten voor inkomend verkeer geopend in de firewall.

U start de verbinding tussen uw service op locatie en de relay-service via een reeks WCF 'relay'-bindingen. Achter de schermen wordt toewijzen de relay-Bindingen om bindende elementen die WCF channel-onderdelen maken die met Bus Service in de cloud integreren.

Service Bus Relay biedt veel voordelen, maar moet dat de server en de client zowel on line worden tegelijkertijd te verzenden en ontvangen van berichten. Dit is niet optimaal voor HTTP-stijl communicatie, dat de aanvragen niet mogelijk meestal lange levensduur, en evenmin voor clients die verbinding maken met slechts af en toe, zoals browsers, mobiele toepassingen, enzovoort. Brokered messaging losgekoppelde communicatie ondersteunt, en heeft zijn eigen voordelen; clients en servers kunnen verbinden wanneer nodig en op asynchrone wijze bewerkingen.

## <a name="brokered-messaging"></a>Brokered messaging

In tegenstelling tot de regeling relay kan [brokered messaging](service-bus-queues-topics-subscriptions.md) worden beschouwd als asynchroon, of 'ondersteuningsbeheerder ontkoppeld.' (Afzenders) producenten en verbruikers (ontvangers) hoeft niet tegelijkertijd on line zijn. Infrastructuur voor berichtafhandeling op betrouwbare wijze worden de berichten in een "broker" (bijvoorbeeld een wachtrij) totdat de verbruikende partij is klaar om te ontvangen. Hierdoor kunnen de onderdelen van de gedistribueerde toepassing worden verbroken, hetzij uit eigen beweging; bijvoorbeeld voor onderhoud, of als gevolg van een crash component, zonder invloed op het hele systeem. Bovendien hoeft de ontvangende toepassing gedurende bepaalde periodes van de dag, bijvoorbeeld een voorraadbeheersysteem dat alleen moet worden uitgevoerd aan het einde van de dag on line worden gebracht.

De belangrijkste onderdelen van de infrastructuur voor Service Bus-brokered zijn wachtrijen, onderwerpen en abonnementen.  Het belangrijkste verschil is dat onderwerpen publiceren/abonneren mogelijkheden die kunnen worden gebruikt voor geavanceerde inhoud gebaseerde routering en aflevering logica, inclusief verzenden naar meerdere ontvangers ondersteunen. Deze onderdelen nieuw asynchrone berichtenafhandeling scenario's mogelijk, zoals tijdelijke ontkoppeling publiceren/abonneren en taakverdeling. Zie voor meer informatie over deze entiteiten messaging [Service Bus wachtrijen, onderwerpen, en abonnementen](service-bus-queues-topics-subscriptions.md).

Net als bij de Relay-infrastructuur, vindt de brokered messaging mogelijkheden voor programmeurs WCF en .NET Framework en via de REST.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over Service Bus berichten.

- [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
- [Service Bus wachtrijen, onderwerpen en abonnementen](service-bus-queues-topics-subscriptions.md)
- [Het gebruik van wachtrijen Service Bus](service-bus-dotnet-get-started-with-queues.md)
- [Het gebruik van de Service Bus onderwerpen en abonnementen](./service-bus-dotnet-how-to-use-topics-subscriptions.md)
 

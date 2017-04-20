<properties 
    pageTitle="Service-busarchitectuur | Microsoft Azure"
    description="Beschrijving van het bericht en de architectuur van Azure Service Bus verwerking relay."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/11/2016"
    ms.author="sethm" />

# <a name="service-bus-architecture"></a>Service Bus-architectuur

Dit artikel wordt beschreven, het bericht en de architectuur van Azure Service Bus verwerking relay.

## <a name="service-bus-scale-units"></a>Schaaleenheden Service Bus

Service Bus is geordend op *schaaleenheden*. Een schaal is een implementatie-eenheid en bevat alle vereiste onderdelen voor de service wordt uitgevoerd. Elke regio implementeert een of meer Service Bus schaaleenheden.

Een naamruimte Service Bus is toegewezen aan een schaaleenheid. De schaaleenheid verwerkt alle typen entiteiten Service Bus: relais en brokered messaging entiteiten (wachtrijen, onderwerpen, abonnementen). Een Service Bus schaaleenheid bestaat uit de volgende onderdelen:

- **Een verzameling knooppunten gateway.** Gateway knooppunten verificatie van binnenkomende aanvragen en verwerken van aanvragen doorsturen. Elk knooppunt gateway heeft een openbaar IP-adres.

- **Een verzameling knooppunten broker messaging.** Messaging broker knooppunten verwerken aanvragen betreffende messaging entiteiten.

- **Een gateway-archief.** Het gateway-archief bevat de gegevens voor elke entiteit die is gedefinieerd in de schaaleenheid van deze. Het archief van de gateway wordt geïmplementeerd op een Azure SQL-database.

- **Meerdere berichten winkels.** Messaging winkels houdt de berichten van wachtrijen, onderwerpen en abonnementen die zijn gedefinieerd in de schaaleenheid van deze. Het bevat ook alle abonnementsgegevens. Tenzij de [gepartitioneerde entiteiten messaging](service-bus-partitioning.md) is ingeschakeld, is een onderwerp of een wachtrij toegewezen aan een berichtenarchief. Abonnementen worden opgeslagen in het hetzelfde berichtenarchief als hun bovenliggende onderwerp. Met uitzondering van de Service Bus [Premium Messaging](service-bus-premium-messaging.md), messaging winkels geïmplementeerd op Azure SQL-databases.

## <a name="containers"></a>Containers

Elke entiteit messaging is toegewezen een specifieke container. Een container is een logische constructie die precies één messaging winkel alle relevante gegevens voor deze container gebruikt. Elke container is toegewezen aan een knooppunt met messaging broker. Er zijn meestal meer containers dan berichten broker knooppunten. Elk knooppunt met messaging broker wordt daarom meerdere containers geladen. De verdeling van de containers naar knooppunt messaging broker is georganiseerd, dat alle knooppunten voor messaging broker gelijkelijk worden geladen. Als het laden van patroon verandert (bijvoorbeeld een van de containers haalt bezet) of als een messaging broker knooppunt tijdelijk niet beschikbaar is wordt, worden de containers tussen de knooppunten van de messaging broker gedistribueerd.

## <a name="processing-of-incoming-messaging-requests"></a>Verwerking van binnenkomende berichten aanvragen

Wanneer een client een aanvraag naar Service Bus verzendt, stuurt het door de Azure taakverdeling naar de gateway-knooppunten. Het knooppunt gateway machtigt de aanvraag. Indien de aanvraag betrekking heeft op een messaging-dienst (wachtrij, onderwerp, abonnement), wordt het knooppunt gateway de entiteit in de winkel gateway worden opgezocht en wordt bepaald welke berichtenarchief de entiteit is gevestigd. Vervolgens opgezocht welke messaging broker knooppunt momenteel deze container wordt en stuurt de aanvraag naar knooppunt messaging broker. Het messaging broker knooppunt verwerkt de aanvraag en werkt de entiteit staat in het archief van de container. Het messaging broker knooppunt stuurt het antwoord naar het knooppunt gateway stuurt een passende reactie naar de client die de oorspronkelijke aanvraag uitgegeven.

![Verwerking van binnenkomende berichten aanvragen](./media/service-bus-architecture/IC690644.png)

## <a name="processing-of-incoming-relay-requests"></a>Verwerking van binnenkomende aanvragen voor relay

Wanneer een client een aanvraag naar Service Bus verzendt, stuurt het door de Azure taakverdeling naar de gateway-knooppunten. Als de aanvraag een aanvraag voor een luisteren is, maakt het knooppunt gateway een nieuwe relay. Als de aanvraag een aanvraag voor een specifieke relay is, stuurt het knooppunt gateway het verzoek om verbinding met het gateway-knooppunt dat eigenaar is van de relay. Het gateway-knooppunt dat eigenaar is van de relay verzendt een verzoek rendezvous voor het luisteren van de client, de listener voor het maken van een tijdelijk kanaal naar het knooppunt gateway die de verbindingsaanvraag wordt ontvangen.

Als de relay-verbinding tot stand is gebracht, kunnen de clients berichten via de gateway-knooppunt dat wordt gebruikt voor de rendezvous uitwisselen.

![Verwerking van binnenkomende aanvragen voor Relay](./media/service-bus-architecture/IC690645.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt gelezen is een overzicht van de Service-busarchitectuur, aan de slag gaat u naar de volgende koppelingen:

- [Messaging service Bus-overzicht](service-bus-messaging-overview.md)
- [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
- [Een wachtrij beheeroplossing met behulp van Service Bus wachtrijen](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)

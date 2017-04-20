<properties 
    pageTitle="Gebeurtenis Hubs Veelgestelde vragen (FAQ) | Microsoft Azure"
    description="Gebeurtenis Hubs Veelgestelde vragen."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/01/2016"
    ms.author="sethm" />

# <a name="event-hubs-faq"></a>Gebeurtenis Hubs Veelgestelde vragen

Gebeurtenis Hubs biedt grootschalige inname, persistentie en verwerking van van Gegevensgebeurtenissen uit hoge gegevensdoorvoer gegevensbronnen en/of miljoenen apparaten. Wanneer gekoppeld aan wachtrijen Service Bus en onderwerpen, kunnen gebeurtenis Hubs permanente opdracht- en implementaties voor scenario's [Internet dingen (IoT)](https://azure.microsoft.com/services/iot-hub/) .

In dit artikel vindt u informatie die prijzen en antwoorden op enkele veelgestelde vragen over gebeurtenis Hubs:

## <a name="pricing-information"></a>Prijsinformatie

Zie de [gebeurtenis Hubs prijsdetails](https://azure.microsoft.com/pricing/details/event-hubs/)voor volledige informatie over gebeurtenis Hubs prijzen.

## <a name="how-are-event-hubs-ingress-events-calculated"></a>Gebeurtenis Hubs ingress gebeurtenissen berekening?

Elke gebeurtenis die wordt verzonden naar een gebeurtenis Hub telt als een bericht te factureren. Een *gebeurtenis ingress* wordt gedefinieerd als een eenheid die kleiner is dan of gelijk aan 64 KB. Elke gebeurtenis die kleiner is dan of gelijk aan 64 KB in grootte, wordt beschouwd als een gebeurtenis van de te factureren. Als de gebeurtenis groter dan 64 KB is, wordt het aantal te factureren gebeurtenissen berekend op basis van de grootte van de gebeurtenis in een veelvoud van 64 KB. Bijvoorbeeld een 8 KB gebeurtenis verzonden naar de gebeurtenis Hub is gefactureerd als één gebeurtenis, maar een 96 KB-bericht verzonden naar de gebeurtenis Hub is gefactureerd als twee gebeurtenissen.

Gebeurtenissen uit een Hub gebeurtenis als verbruikt evenals beheertaken uit te voeren en aanroepen zoals controlepunten te bepalen, worden niet meegeteld als ingress factureerbare gebeurtenissen, maar toenemen tot de vergoeding van de eenheid doorvoer.

## <a name="what-are-event-hubs-throughput-units"></a>Wat zijn Hubs gebeurtenis doorvoer eenheden?

U selecteren expliciet gebeurtenis Hubs doorvoer eenheden via de portal Azure of gebeurtenis Hubs resource manager sjablonen. Doorvoer maateenheden worden toegepast op alle gebeurtenis Hubs in een naamruimte gebeurtenis Hubs en elke eenheid doorvoer recht geeft de naamruimte aan de volgende mogelijkheden:

- Van 1 MB per seconde van ingress-gebeurtenissen (gebeurtenissen die worden verzonden naar een gebeurtenis Hub), maar niet meer dan 1000 ingress gebeurtenissen, beheertaken uit te voeren of een besturingselement API-aanroepen per seconde.

- Tot 2 MB per seconde van de egress-gebeurtenissen (gebeurtenissen uit een gebeurtenis Hub verbruikt).

- 84 GB opslag van gebeurtenis (voldoende voor de bewaarperiode standaard 24 uur per dag).

Gebeurtenis Hubs doorvoer eenheden worden gefactureerd per uur, op basis van het maximum aantal eenheden die zijn geselecteerd tijdens het uur gegeven.

## <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Hoe kan ik gebeurtenis Hubs doorvoer eenheid beperkingen afgedwongen?

De totale ingress doorvoer of het percentage van de totale ingress gebeurtenis via alle Hubs van de gebeurtenis in een naamruimte groter is dan de eenheid geaggregeerde doorvoer vergoedingen, afzenders wordt de snelheid als er fouten optreden die aangeeft dat de ingress is overschreden.

De totale egress doorvoer of het percentage van de totale gebeurtenis egress via alle Hubs van de gebeurtenis in een naamruimte groter is dan de eenheid geaggregeerde doorvoer vergoedingen, ontvangers worden verminderd als er fouten optreden die aangeeft dat de egress is overschreden. Ingress- en egress quota gelden apart, zodat geen afzender gebeurtenis verbruik veroorzaken kan te verlagen, noch kan een ontvanger verhinderen dat gebeurtenissen op een Hub gebeurtenis wordt verzonden.

Let erop dat de selectie van de eenheid doorvoer onafhankelijk van het aantal partities gebeurtenis Hubs. Elke partitie biedt een maximale doorvoersnelheid van 1 MB per tweede ingress (met een maximum van 1000 gebeurtenissen per seconde) en 2 MB per tweede egress, maar er is geen vaste kosten voor de partities zelf. De toeslag is voor de geaggregeerde doorvoer eenheden op alle gebeurtenis Hubs in een naamruimte gebeurtenis Hubs. Met dit patroon, kunt u genoeg partities ter ondersteuning van de verwachte maximale belasting voor hun systemen, zonder eventuele toeslagen doorvoer eenheid totdat de gebeurtenis load op het systeem werkelijk hogere doorvoer nummers vereist en zonder te veranderen van de structuur en de architectuur van uw systemen als de belasting van het systeem wordt verhoogd.

## <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Is er een limiet op het aantal eenheden van doorvoer die kunnen worden geselecteerd?

Er is een standaardquotum van 20 eenheden van doorvoer per naamruimte. U kunt een groter contingent van doorvoer eenheden door het indienen van een ticket support aanvragen. Voorbij de limiet van de eenheid doorvoer 20 zijn bundels beschikbaar in 20-100 eenheden van doorvoer. Houd er rekening mee dat met meer dan 20 eenheden van doorvoer de mogelijkheid het aantal eenheden van de doorvoer wijzigen zonder een support ticket indienen verwijderd.

## <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Is er een toeslag voor gebeurtenissen gebeurtenis Hubs voor meer dan 24 uur bewaren?

De gebeurtenis Hubs standaard laag kan bericht bewaren in perioden van meer dan 24 uur, tot een maximum van 30 dagen. Als de grootte van het totale aantal opgeslagen gebeurtenissen groter is dan de uitkering van de opslag voor het aantal eenheden van de geselecteerde doorvoer (84 GB per eenheid doorvoer), het formaat dat groter is dan de vergoeding in rekening wordt gebracht op de gepubliceerde Azure Blob storage tarief. De vergoeding voor opslag in elke eenheid doorvoer dekt alle kosten voor opslag voor een periode van 24 uur bewaren (de standaardinstelling) zelfs indien de doorvoer eenheid is op de maximale ingress-uitkering.

## <a name="what-is-the-maximum-retention-period"></a>Wat is de maximale retentietijd?

Gebeurtenis standaard Hubs laag ondersteunt op dit moment een maximale periode van 7 dagen. Noteer gebeurtenis Hubs zijn niet bedoeld als een blijvende gegevensopslag. Bewaarperiode groter is dan 24 uur zijn bedoeld voor scenario's waarin het is handig voor de stream van een gebeurtenis in de dezelfde systemen; Als u bijvoorbeeld de trein of een nieuw model van machine learning op bestaande gegevens controleren.

## <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Hoe is de maximale grootte van het gebeurtenis Hubs berekend en in rekening gebracht?

De totale grootte van alle opgeslagen gebeurtenissen, inclusief eventuele interne overhead voor kopteksten van gebeurtenissen of op schijfstructuren opslag in alle Hubs gebeurtenis, wordt gemeten gedurende de dag. Aan het einde van de dag, wordt de omvang van het piek berekend. De dagvergoeding voor de opslag wordt berekend op basis van het minimum aantal eenheden doorvoer die gedurende de dag (elke eenheid doorvoer biedt een correctie van 84 GB) zijn geselecteerd. Als de totale grootte groter is dan de berekende dagvergoeding opslag, de opslag van de overtollige gefactureerd met Azure Blob storage tarieven (en met het **Lokaal redundante opslag** ).

## <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-event-hubs-and-service-bus-queuestopics"></a>Kan ik een enkele AMQP verbinding gebruiken voor het verzenden en ontvangen van de gebeurtenis Hubs en wachtrijen Service Bus/onderwerpen

Ja, zolang de gebeurtenis Hubs, wachtrijen en onderwerpen in dezelfde naamruimte. Zo kunt u bi-directionele, brokered connectiviteit te veel apparaten, met subsecond-vertragingstijden implementeren in een voordelig en vergaand schaalbare manier.

## <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Pas brokered telefoonkosten met Hubs gebeurtenis?

Van afzenders geldt verbinding alleen wanneer de AMQP-protocol wordt gebruikt. Er zijn geen telefoonkosten voor het verzenden van gebeurtenissen met behulp van HTTP, ongeacht het aantal systemen of apparaten verzenden. Als u van plan bent te gebruiken van AMQP (bijvoorbeeld efficiënter gebeurtenis streaming bereiken of bidirectionele communicatie in IoT opdracht- en scenario's inschakelen), raadpleegt u de pagina [Service Bus prijsinformatie](https://azure.microsoft.com/pricing/details/service-bus/) voor informatie over wat een brokered verbinding vormt en hoe ze worden gemeten.

## <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Wat is het verschil tussen de gebeurtenis Hubs Basic en Standard lagen?

Gebeurtenis biedt standaard Hubs functies dan beschikbaar in de gebeurtenis Hubs Basic, en in sommige concurrerende systemen is. Deze functies omvatten de bewaarperiode van meer dan 24 uur en de mogelijkheid om een enkele AMQP verbinding gebruiken om opdrachten te verzenden naar een groot aantal apparaten met subsecond vertragingstijden alsmede de telemetrie van die apparaten verzenden in gebeurtenis Hubs. Zie de [gebeurtenis Hubs prijsgegevens](https://azure.microsoft.com/pricing/details/event-hubs/)voor de lijst van functies.

## <a name="geographic-availability"></a>Geografische beschikbaarheid

Gebeurtenis Hubs is beschikbaar in de volgende gebieden:

|Geo|Regio 's|
|---|---|
|Verenigde Staten|Centrale VS, Oost-VS, Oost-VS 2, Zuid-centraal-Amerikaanse, Amerikaanse West|
|Europa|Noord-Europa, West-Europa|
|Azië/Stille Oceaangebied|Oost-Azië, Zuidoost-Azië|
|Japan|Oost West Japan Japan|
|Brazilië|Brazilië-Zuid|
|Australië|Australië, Oost, Zuidoost Australië|

## <a name="support-and-sla"></a>Ondersteuning en SLA

Technische ondersteuning voor gebeurtenis Hubs is beschikbaar via de [forums in de community](https://social.msdn.microsoft.com/forums/azure/home). Ondersteuning voor facturering en een abonnement is kosteloos.

Meer informatie over onze SLA, Zie de pagina [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/) .

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de gebeurtenis Hubs, Zie de volgende artikelen:

- [Overzicht van de gebeurtenis Hubs][].
- Een volledig [voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs][].

[Overzicht van de gebeurtenissen Hubs]: event-hubs-overview.md
[de voorbeeldtoepassing met gebeurtenis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097

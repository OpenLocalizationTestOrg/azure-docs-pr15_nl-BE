<properties 
    pageTitle="Bus service prijzen en facturering | Microsoft Azure"
    description="Overzicht van Service Bus prijsstructuur."
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
    ms.date="10/06/2016"
    ms.author="sethm" />

# <a name="service-bus-pricing-and-billing"></a>Bus service prijzen en facturering

Bus service wordt aangeboden in de niveaus Basic, Standard en [Premium](service-bus-premium-messaging.md) . U kunt een servicelaag voor elke Service Bus service-naamruimte die u maakt en deze laag selectie van toepassing is op alle entiteiten binnen die naamruimte gemaakt.

>[AZURE.NOTE] Zie voor gedetailleerde informatie over de huidige prijzen Service Bus de [Azure Service Bus pagina prijzen](https://azure.microsoft.com/pricing/details/service-bus/)en de [Veelgestelde vragen over Service-Bus](service-bus-faq.md#service-bus-pricing).

Bus-service gebruikt de volgende twee meter voor wachtrijen en onderwerpen/abonnementen:

1. **Messaging bewerkingen**: gedefinieerd als API-aanroepen tegen wachtrij of onderwerp/abonnement service-endpoints. Deze meter wordt vervangen door de berichten verzonden of ontvangen als de primaire eenheid van factureerbare verbruik voor wachtrijen en onderwerpen/abonnementen.

2. **Brokered verbindingen**: gedefinieerd als het maximum aantal permanente verbindingen geopend met wachtrijen, onderwerpen of abonnementen gedurende een bepaalde bemonstering van één uur. Deze meter is alleen van toepassing in de standaard laag, waarin u kunt extra verbindingen te openen (voorheen verbindingen zijn beperkt tot 100 per onderwerp-wachtrij/abonnement) voor de kosten van een nominale verbinding.

De **standaard** laag introduceert geleidelijk stijgende prijzen voor bewerkingen die worden uitgevoerd met wachtrijen en onderwerpen/abonnementen, wat resulteert in kortingen tot 80% op de hoogste niveaus voor gebruik op basis van het volume. Er is ook een standaard laag base toeslag van $10 per maand, waardoor u bewerkingen van 12,5 miljoen per maand zonder extra kosten.

De **Premium** biedt resource isolatie op de CPU en het geheugen laag zodat de werklast voor elke klant wordt uitgevoerd in een geïsoleerde omgeving. Deze container resource heet een *messaging-eenheid*. Elke naamruimte premium wordt ten minste één messaging eenheid worden toegewezen. U kunt 1, 2 of 4 messaging eenheden voor elke naamruimte Service Bus Premium kopen. Een enkele werkbelasting of entiteit kan meerdere berichten eenheden omvatten en het aantal berichten eenheden kan worden gewijzigd op wordt, hoewel facturering in 24 uur of dagelijks bijdragen is. Het resultaat is een voorspelbare en herhaalbare prestaties voor uw oplossing op basis van een Service Bus. Niet alleen is deze prestaties meer voorspelbare en beschikbaar, maar is ook sneller. Azure Service Bus Premium messaging is gebaseerd op de opslag-engine geïntroduceerd in Azure gebeurtenis Hubs. Premium messaging is topprestaties veel sneller dan de standaard laag.

Houd er rekening mee dat de standaard base toeslag in rekening slechts eenmaal per maand per abonnement Azure gebracht. Dit betekent dat als u een één standaard of Premium-tier-Service Bus naamruimte hebt gemaakt, u zoveel extra Standard of Premium tier naamruimten zult gewenste onder dezelfde Azure abonnement, zonder extra kosten van de basis te maken.

Alle bestaande Service Bus naamruimten gemaakt vóór 1 November 2014 zijn automatisch in de standaard laag geplaatst. Dit zorgt ervoor dat u toegang hebben tot alle momenteel beschikbare functies bij Service Bus blijven. Vervolgens kunt u de [Azure klassieke portal][] downgraden naar de eenvoudige laag indien gewenst.

De volgende tabel bevat een overzicht van de functionele verschillen tussen de niveaus Basic en Standard/Premium.

|Mogelijkheden|Basic|Standaard/Premium|
|---|---|---|
|Wachtrijen|Ja|Ja|
|Geplande berichten|Ja|Ja|
|Onderwerpen/abonnementen|Nee|Ja|
|Relais|Nee|Ja|
|Transacties|Nee|Ja|
|Duplicaten verwijderen|Nee|Ja|
|Sessies|Nee|Ja|
|Grote berichten|Nee|Ja|
|ForwardTo|Nee|Ja|
|SendVia|Nee|Ja|
|Brokered verbindingen (inbegrepen)|100 per Bus Service-naamruimte|1.000 per abonnement Azure|
|Brokered verbindingen (overschot toegestaan)|Nee|Ja (factureerbare)|

## <a name="messaging-operations"></a>Messaging bewerkingen

Als onderdeel van het nieuwe prijsmodel wordt facturering voor wachtrijen en onderwerpen/abonnementen gewijzigd. Deze entiteiten zijn overstappen van facturering per bericht naar facturering per bewerking. Een "verrichting" verwijst naar een API-aanroep ten opzichte van een wachtrij of onderwerp/abonnement service-eindpunt. Dit geldt ook voor beheer, verzenden/ontvangen en session state bewerkingen.

|Bewerkingstype|Beschrijving|
|---|---|
|Management|Maken, lezen, bijwerken, verwijderen (CRUD) tegen wachtrijen of onderwerpen/abonnementen.|
|Messaging|Berichten verzenden en ontvangen met wachtrijen of onderwerpen/abonnementen.|
|Sessiestatus|Ophalen of instellen van de sessiestatus in een wachtrij of onderwerp/abonnement.|

De volgende prijzen zijn effectief vanaf 1 November 2014:

|Basic|Kosten|
|---|---|
|Bewerkingen|$0,05 per miljoen bewerkingen|

|Standaard|Kosten|
|---|---|
|Gratis basis|$10/ maand|
|Eerst 12,5 miljoen transacties per maand|Opgenomen|
|12,5-100 miljoen transacties per maand|$0,80 per miljoen bewerkingen|
|100 miljoen - 2.500 miljoen transacties per maand|$0,50 per miljoen bewerkingen|
|Meer dan 2.500 miljoen transacties per maand|$0,20 per miljoen bewerkingen|

|Premium|Kosten|
|---|---|
|Dagelijks|$11.13 vast rentetarief per eenheid van bericht|

## <a name="brokered-connections"></a>Brokered verbindingen

*Brokered verbindingen* ten behoeve van gebruikspatronen klant met betrekking tot een groot aantal afzenders/ontvangers 'permanent verbonden' tegen wachtrijen, onderwerpen of abonnementen. Permanent verbonden afzenders/ontvangers zijn die verbinding maken met behulp van AMQP of HTTP-met een niet-nul-time-out (kan bijvoorbeeld in HTTP worden lange polling) ontvangen. HTTP-afzenders en ontvangers met een onmiddellijke time-out genereren geen brokered verbindingen.

Wachtrijen en onderwerpen/abonnementen had eerder een limiet van 100 gelijktijdige verbindingen per URL. Het huidige schema voor facturering worden verwijderd van de limiet per URL voor wachtrijen en onderwerpen/abonnementen en quota en de meting op brokered verbindingen op het niveau van de Azure abonnement en Service Bus-naamruimte geïmplementeerd.

De basis laag bevat en is strikt beperkt is tot 100 brokered verbindingen per Bus Service-naamruimte. Verbindingen boven dit getal wordt afgewezen in de Basic laag. De standaard laag verwijdert u de limiet per naamruimte en gebruik van statistische brokered verbinding via de Azure abonnement telt. In de standaard laag worden 1000 brokered verbindingen per abonnement Azure mogen zonder extra kosten (buiten de basis kosten). Met meer dan 1000 brokered verbindingen via standaard-tier-Service Bus wordt naamruimten in een Azure-abonnement gefactureerd op een geijkte planning, zoals in de volgende tabel.

|Brokered verbindingen (standaard laag)|Kosten|
|---|---|
|Eerste 1.000 per maand|Met de gratis basis opgenomen|
|1000-100.000 per maand|$0,03 per verbinding per maand|
|100.000-500.000 per maand|$0,025 per verbinding per maand|
|Boven 500.000 per maand|$0.015 per verbinding per maand|

>[AZURE.NOTE] 1000 brokered verbindingen zijn opgenomen in de standaard messaging laag (via de basis kosten) en kunnen worden gedeeld met alle wachtrijen, onderwerpen en abonnementen in de bijbehorende Azure abonnement.

<br />

>[AZURE.NOTE] Facturering is gebaseerd op het maximum aantal gelijktijdige verbindingen en wordt verdeeld per uur gebaseerd op 744 uur per maand.

|Premium-laag
|---|
|Brokered verbindingen worden niet in rekening gebracht in de Premium-laag.|

Zie de sectie [Veelgestelde vragen](#faq) verderop in dit onderwerp voor meer informatie over brokered verbindingen.

## <a name="relay"></a>Relay

Relais zijn alleen beschikbaar in de Standard-tier-naamruimten. Anders, prijzen en verbinding quota voor relais blijven ongewijzigd. Dit betekent dat relais blijft in rekening worden gebracht op het aantal berichten (geen bewerkingen) en de relay-uren.

|Relay-prijzen|Kosten|
|---|---|
|Relay-uren|$0,10 voor elke relay 100 uur|
|Berichten|$0,01 voor elke 10.000 berichten|

## <a name="faq"></a>FAQ

### <a name="how-is-the-relay-hours-meter-calculated"></a>Hoe wordt de meter Relay uren berekend?

Raadpleeg [dit onderwerp](service-bus-faq.md#how-is-the-relay-hours-meter-calculated).

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Wat zijn brokered verbindingen en hoe ik krijgen in rekening gebracht voor hen?

Een brokered verbinding wordt gedefinieerd als een van de volgende opties:

1. Een AMQP verbinding vanaf een client naar een wachtrij Service Bus of onderwerp/abonnement.

2. Een HTTP-oproep een bericht ontvangt van een Service Bus onderwerp of een wachtrij met een time-outwaarde voor ontvangen groter is dan nul.

Bus service kosten voor het maximum aantal gelijktijdige brokered verbindingen die groter zijn dan de opgenomen hoeveelheid (1000 in de standaard laag). Pieken zijn gemeten op uurbasis, evenredig worden verdeeld door te delen door 744 uur per maand en over de maandelijkse facturering periode opgeteld. De opgenomen hoeveelheid (1000 brokered verbindingen per maand) wordt aan het einde van de betalingsperiode tegen de som van de pro rata pieken per uur toegepast.

Bijvoorbeeld:

1. Elk van de 10.000 apparaten verbinding maakt via een enkele verbinding van AMQP en opdrachten ontvangt van een onderwerp Service Bus. De apparaten verzenden telemetrie gebeurtenissen op een gebeurtenis Hub. Als alle apparaten verbinding voor 12 uur per dag maken, de volgende verbinding kosten van toepassing (naast andere Service Bus onderwerp heffingen): 10.000 verbindingen *12 uur* 31 dagen / brokered 744 = 5000 verbindingen. Na de maandelijkse toelage van 1000 brokered verbindingen u aangerekend voor 4000 brokered verbindingen met een snelheid van $0,03 per brokered verbinding, voor een totaal van $120.

2. 10.000 apparaten ontvangen berichten uit een wachtrij Service Bus via HTTP, een niet-nul-out op te geven. Als alle apparaten verbinding voor 12 uur per dag maken, ziet u de volgende kosten verbinding (naast andere Service Bus heffingen): 10.000 ontvangen HTTP-verbindingen *12 uur per dag* 31 dagen / uren 744 = 5.000 brokered verbindingen.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>Pas brokered telefoonkosten wachtrijen en onderwerpen/abonnementen?

Ja. Er zijn geen telefoonkosten voor het verzenden van gebeurtenissen met behulp van HTTP, ongeacht het aantal systemen of apparaten verzenden. Gebeurtenissen ontvangen met HTTP met behulp van een groter is dan nul, ook wel genoemd "lange polling," time-out genereert brokered telefoonkosten. Verbindingen van AMQP genereren brokered telefoonkosten, ongeacht of de verbindingen worden gebruikt voor het verzenden of ontvangen. Houd er rekening mee dat 100 brokered verbindingen zijn toegestaan in een standaard naamruimte kosteloos. Dit is ook het maximum aantal brokered verbindingen zijn toegestaan voor het abonnement Azure. De eerste 1000 brokered verbindingen via alle standaard naamruimten in een Azure-abonnement zijn opgenomen zonder extra kosten (buiten de basis kosten). Omdat deze vergoedingen voldoende zijn zijn voor veel services berichtverzending, pas brokered telefoonkosten meestal relevant als u van plan bent lange-polling van AMQP of HTTP gebruiken met een groot aantal clients; Zo bereiken meer efficiënte gebeurtenis streaming-of bidirectionele communicatie met veel apparaten of toepassingsexemplaren.

## <a name="next-steps"></a>Volgende stappen

- Zie de [Azure Service Bus prijzen pagina](https://azure.microsoft.com/pricing/details/service-bus/)voor meer informatie over Service Bus prijzen.

- Zie de [Veelgestelde vragen over Service-Bus](service-bus-faq.md#service-bus-pricing) voor sommige algemene veelgestelde vragen rond bus Service prijzen en facturering.

[Azure klassieke portal]: http://manage.windowsazure.com
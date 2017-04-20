<properties 
    pageTitle="Isolatie van toepassingen tegen storingen en calamiteiten Service Bus | Microsoft Azure"
    description="Beschrijving van technieken die kunt u toepassingen tegen een mogelijke Service Bus stroomstoring te beschermen."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/02/2016"
    ms.author="sethm" />

# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Aanbevolen procedures voor de isolatie van toepassingen tegen Bus Service storingen en calamiteiten

Bedrijfskritieke toepassingen moeten voortdurend, functioneren ook in aanwezigheid van ongeplande storingen of calamiteiten. In dit onderwerp wordt beschreven technieken die kunt u toepassingen tegen een mogelijke servicestoring of calamiteiten Service Bus te beschermen.

Een storing wordt gedefinieerd als de tijdelijke onbeschikbaarheid van Azure Service Bus. De storing kan invloed hebben op sommige onderdelen van de Bus-Service, zoals een berichtenarchief of zelfs het hele datacenter. Nadat het probleem is opgelost, weer Bus Service beschikbaar is. Een storing leidt meestal niet tot verlies van berichten of andere gegevens. Een voorbeeld van een onderdeel het laat afweten, is het ontbreken van een bepaalde winkel voor berichtafhandeling. Een voorbeeld van een stroomstoring gehele datacenter is een stroomstoring van het datacenter of een defecte datacenter netwerkswitch. Een storing kan laatste enkele minuten tot een paar dagen.

Een ramp is gedefinieerd als permanent verlies van een Service Bus schaaleenheid of datacenter. Het datacenter kan of mag niet weer beschikbaar. Meestal verloren een ramp sommige of alle berichten of andere gegevens. Voorbeelden van rampen zijn brand, overstroming of een aardbeving.

## <a name="current-architecture"></a>Huidige architectuur

Meerdere winkels voor messaging service Bus gebruikt voor het opslaan van berichten die worden verzonden naar wachtrijen of onderwerpen. Een wachtrij voor niet-gepartitioneerde of onderwerp is toegewezen aan een berichtenarchief. Als dit berichtenarchief niet beschikbaar is, niet alle bewerkingen op de desbetreffende wachtrij of onderwerp.

Alle Service Bus berichten entiteiten (wachtrijen, onderwerpen, relais) bevinden zich in de Servicenaamruimte van een die is gekoppeld aan een datacenter. Bus-service is niet ingeschakeld voor automatische geo-replicatie van gegevens, noch toestaan een naamruimte te omspannen meerdere datacenters.

## <a name="protecting-against-acs-outages"></a>Beschermen tegen storingen van de ACS

Als u met behulp van referenties ACS en ACS niet beschikbaar is, kunnen clients niet langer tokens ophalen. Clients die een token op het moment dat ACS uitvalt kunnen blijven gebruiken Service Bus totdat de tokens zijn verlopen. De standaardlevensduur voor token is 3 uur.

Gebruiken om te beschermen tegen storingen van de ACS, gedeelde toegang handtekening (SAS)-tokens. In dit geval verifieert de client met Service Bus rechtstreeks door een zelfstandige minted token te ondertekenen met een geheime sleutel. ACS-aanroepen zijn niet langer vereist. Zie voor meer informatie over tokens SAS [Service Bus-verificatie][].

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Bescherming van wachtrijen en onderwerpen tegen storingen winkel messaging

Een wachtrij voor niet-gepartitioneerde of onderwerp is toegewezen aan een berichtenarchief. Als dit berichtenarchief niet beschikbaar is, niet alle bewerkingen op de desbetreffende wachtrij of onderwerp. Een gepartitioneerde wachtrij aan de andere kant bestaat uit meerdere fragmenten. Elk fragment wordt opgeslagen in een andere winkel voor berichtafhandeling. Wanneer een bericht wordt verzonden naar een wachtrij gepartitioneerde of onderwerp, toegewezen Bus-Service het bericht op een van de fragmenten. Als het overeenkomstige berichtenarchief niet beschikbaar is, schrijft Bus-Service het bericht indien mogelijk naar een ander fragment. Zie voor meer informatie over gepartitioneerde entiteiten, [gepartitioneerd messaging entiteiten][].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Bescherming tegen datacenter storingen of calamiteiten

Als u wilt toestaan voor een failover-tussen twee datacenters, kunt u een naamruimte Service Bus service in elk datacenter. Bijvoorbeeld de Bus Service service namespace **contosoPrimary.servicebus.windows.net** bevindt zich wellicht in de Verenigde Staten Noord/centrale regio en **contosoSecondary.servicebus.windows.net** kunnen in de regio ons Zuid/centraal staan. Als een entiteit messaging Service-Bus toegankelijk voor de aanwezigheid van een storing in datacenter blijft, kunt u die entiteit in beide naamruimten maken.

Zie voor meer informatie de sectie 'Fout bij de Bus in een datacenter Azure Service' in de [asynchrone berichtenafhandeling patronen en hoge beschikbaarheid][].

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Relay-eindpunten tegen datacenter storingen of calamiteiten te beschermen

Geo-replicatie van relay eindpunten kan een service die beschikbaar worden gesteld een eindpunt relay bereikbaar met Bus Service storingen worden. Voor geo-replicatie, moet de service relay eindpunten in verschillende naamruimten maken. De naamruimten moet zich bevinden in verschillende datacentra en de twee eindpunten moeten verschillende namen hebben. Bijvoorbeeld is een primair eindpunt bereikbaar onder **contosoPrimary.servicebus.windows.net/myPrimaryService**, terwijl de secundaire tegenhanger daarvan onder **contosoSecondary.servicebus.windows.net/mySecondaryService**kan worden bereikt.

De service luistert vervolgens op beide eindpunten en een client de service via een eindpunt kunt oproepen. Een clienttoepassing willekeurig een van de relais als het primaire eindpunt picks en haar verzoek verzendt naar de actieve eindpunt. Als de bewerking met foutcode mislukt, deze fout geeft aan dat het eindpunt van de relay is niet beschikbaar. De toepassing wordt een kanaal geopend naar het eindpunt van de back-up en databasebron moet worden gestuurd van de aanvraag. Op dat moment overschakelen rollen de actieve en de eindpunten van de back-up: de clienttoepassing beschouwt de oude actieve eindpunt aan het eindpunt van het nieuwe back-up en het eindpunt van de oude back-up op het nieuwe endpoint actief worden. Als beide mislukken verzenden, de rollen van de twee entiteiten blijven ongewijzigd en wordt een fout geretourneerd.

De [Geo-replicatie met berichten doorgestuurd van Bus-Service][] -voorbeeld wordt gedemonstreerd hoe relais repliceren.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Wachtrijen en onderwerpen tegen datacenter storingen of calamiteiten te beschermen

Om te bereiken weerstandsvermogen tegen storingen datacenter als brokered met behulp van messaging, Service Bus ondersteunt twee benaderingen: *actieve* en *passieve* replicatie. Voor elke benadering, als een bepaalde wachtrij of onderwerp toegankelijk voor de aanwezigheid van een storing in datacenter blijft, kunt u deze in beide naamruimten. Beide entiteiten kunnen dezelfde naam hebben. Bijvoorbeeld is een primaire wachtrij bereikbaar onder **contosoPrimary.servicebus.windows.net/myQueue**, terwijl de secundaire tegenhanger daarvan onder **contosoSecondary.servicebus.windows.net/myQueue**kan worden bereikt.

Als de toepassing geen permanente communicatie van de afzender naar ontvanger nodig heeft, kan de toepassing een duurzame client-side wachtrij bericht verlies te voorkomen en voorkomt dat de afzender van tijdelijke fouten Bus Service implementeren.

## <a name="active-replication"></a>Active-replicatie

Active-replicatie gebruikt entiteiten in beide naamruimten voor elke bewerking. Een client die een bericht stuurt twee exemplaren van hetzelfde bericht. Het eerste exemplaar wordt verzonden naar de primaire entiteit (bijvoorbeeld **contosoPrimary.servicebus.windows.net/sales**) en de tweede kopie van het bericht wordt verzonden naar de secundaire entiteit (bijvoorbeeld **contosoSecondary.servicebus.windows.net/sales**).

Een client ontvangt berichten van beide wachtrijen. De ontvanger verwerkt de eerste kopie van een bericht en de tweede kopie wordt onderdrukt. Om dubbele berichten te onderdrukken, de afzender moet elk bericht met een unieke id-code. Beide kopieën van het bericht moeten worden gelabeld met dezelfde id. U kunt de eigenschappen [BrokeredMessage.MessageId][] of [BrokeredMessage.Label][] of een aangepaste eigenschap tag van het bericht. De ontvanger moet een lijst bijhouden van berichten die al zijn ontvangen.

De [Geo-replicatie met Service Bus Brokered berichten][] ziet u het actieve replicatie van de messaging-diensten.

> [AZURE.NOTE] De aanpak van actieve replicatie verdubbelt het aantal bewerkingen, dus deze aanpak kan leiden tot hogere kosten.

## <a name="passive-replication"></a>Passieve replicatie

In het geval storingsvrije passieve replicatie maakt gebruik van slechts één van de twee entiteiten messaging. Een client verzendt het bericht naar de actieve entiteit. Als het mislukt met een foutcode die aangeeft het datacenter die fungeert als host voor de actieve dienst is mogelijk niet beschikbaar op de actieve entiteit, verzendt de client een kopie van het bericht naar de back-entiteit. Op dat moment de actieve en de back-up diensten gaan rollen: de verzendende client beschouwt de oude actieve entiteit worden de nieuwe back-entiteit en de oude back-entiteit is de nieuwe actieve entiteit. Als beide mislukken verzenden, de rollen van de twee entiteiten blijven ongewijzigd en wordt een fout geretourneerd.

Een client ontvangt berichten van beide wachtrijen. Omdat er een kans dat de ontvanger twee exemplaren van hetzelfde bericht ontvangt, moet de ontvanger dubbele berichten onderdrukken. U kunt dubbele onderdrukken op dezelfde manier zoals beschreven voor de replicatie van active.

In het algemeen is passieve replicatie voordeliger dan de replicatie van active omdat in de meeste gevallen slechts één bewerking wordt uitgevoerd. Latentie, doorvoer en kosten zijn gelijk aan het scenario niet gerepliceerd.

Bij het gebruik van passieve replicatie in de volgende scenario's kunnen berichten worden verloren of twee keer ontvangen:

-   **Bericht vertraging of verlies**: Stel dat de afzender een bericht m1 is verzonden naar de primaire wachtrij, en vervolgens de wachtrij niet beschikbaar is voordat de ontvanger m1 ontvangt. De afzender verzendt een volgend bericht m2 naar de tweede wachtrij. Als u de primaire wachtrij is tijdelijk niet beschikbaar is, ontvangt de ontvanger m1 nadat de wachtrij weer beschikbaar is. De ontvanger kan een noodsituatie m1 nooit ontvangen.

-   **Dubbele ontvangst**: wordt ervan uitgegaan dat de afzender een m naar de primaire wachtrij bericht. Service Bus m verwerkt is, maar om een antwoord te verzenden is mislukt. Nadat er is een time-out opgetreden bij de verzendbewerking, verzendt de afzender een identieke kopie van m naar de tweede wachtrij. Als de ontvanger kan de eerste kopie van m ontvangen voordat u de primaire wachtrij niet beschikbaar is, ontvangt de ontvanger beide kopieën van m op ongeveer hetzelfde tijdstip. Als de ontvanger niet kan ontvangen van de eerste kopie van m voordat u de primaire wachtrij niet beschikbaar is, wordt de ontvanger krijgt in eerste instantie alleen het tweede exemplaar van m, maar vervolgens een tweede exemplaar van m ontvangt wanneer de primaire wachtrij beschikbaar.

De [Geo-replicatie met Service Bus Brokered berichten][] ziet u het passieve replicatie van de messaging-diensten.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over herstel na storingen, Zie de volgende artikelen:

- [Bedrijfscontinuïteit Azure SQL-Database][]
- [Technische richtsnoeren Azure tolerantie][]

  [Verificatie van Service Bus]: service-bus-authentication-and-authorization.md
  [Gepartitioneerde messaging entiteiten]: service-bus-partitioning.md
  [Asynchrone berichtenafhandeling patronen en hoge beschikbaarheid]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
  [Geo-replicatie met Service Bus berichten doorsturen]: http://code.msdn.microsoft.com/Geo-replication-with-16dbfecd
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [BrokeredMessage.Label]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx
  [Geo-replicatie met Service Bus Brokered berichten]: http://code.msdn.microsoft.com/Geo-replication-with-f5688664
  [Bedrijfscontinuïteit Azure SQL-Database]: ../sql-database/sql-database-business-continuity.md
  [Technische richtsnoeren Azure tolerantie]: ../resiliency/resiliency-technical-guidance.md

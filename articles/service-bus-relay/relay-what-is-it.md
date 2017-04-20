<properties
    pageTitle="Wat is Azure relay? | Microsoft Azure"
    description="Overzicht van Azure Relay"
    services="service-bus"
    documentationCenter=".net"
    authors="banisadr"
    manager="timlt"
    editor="" />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="babanisa" />

# <a name="what-is-azure-relay"></a>Wat is Azure Relay?

Azure relayservice vereenvoudigt uw hybride toepassingen doordat u veilig toegankelijk maken van services die zich bevinden binnen een onderneming corporate netwerk naar de openbare cloud zonder open firewallverbinding met een, of opdringerige wijzigingen in een bedrijfsnetwerkinfrastructuur vereist. Relay ondersteunt een groot aantal verschillende transportprotocollen en normen.

De relayservice ondersteunt traditionele eenrichtingsvertrouwensrelatie verzoek antwoord en peer-to-peer-verkeer. Het ondersteunt ook de verdeling van de gebeurtenis in het internet-bereik om scenario's publiceren/abonneren en socket bi-directionele communicatie voor point-to-point verhoogde efficiëntie. 

In het patroon indirecte data-overdracht is een service op locatie verbinding maakt met de relay-service via een uitgaande poort en maakt een bi-directionele socket voor communicatie is gebonden aan een bepaalde rendezvous-adres. De client kan vervolgens communiceren met de service op locatie door te sturen verkeer naar de relayservice gericht op het adres rendezvous. De relay-service wordt vervolgens "doorsturen" gegevens naar de on-premises-service door middel van een bi-directionele socket toegewezen aan elke client. De client niet een directe verbinding met de service op locatie nodig heeft, hoeft niet te weten waar de service is geïnstalleerd en de service op locatie hoeft niet alle poorten voor inkomend verkeer geopend in de firewall.

De mogelijkheid belangrijke elementen geleverd door Relay zijn bi-directionele, niet-gebufferde communicatie buiten de grenzen van het netwerk met TCP-achtige beperken, endpoint discovery, verbindingsstatus en eindpunt beveiliging overlapt. De relay-mogelijkheden verschillen van op netwerkniveau integratietechnologieën zoals VPN in dat deze kan worden binnen het bereik van een verzamelaanvraag eindpunt op een enkele computer, terwijl VPN-technologie veel meer nog is als deze is afhankelijk van de netwerkomgeving te wijzigen.

Azure Relay heeft twee functies:

1. [Hybride verbindingen](#hybrid-connections) - maakt gebruik van de open standaard Web Sockets inschakelen van scenario's voor meerdere platforms

2. [Relais WCF](#wcf-relays) - Windows Communication Foundation gebruikt om externe procedure-oproepen

Hybride verbindingen en WCF relais inschakelen beveiligde verbinding met assests die bestaan binnen een onderneming zakelijk netwerk Gebruik van een op de andere is afhankelijk van uw specifieke wensen hieronder beschreven:

|                                    | WCF-Relay | Hybride verbindingen |
| ---------------------------------- |:---------:|:------------------:|
| **WCF**                            |     x     |                    |
| **.NET-core**                      |           |         x          |
| **.NET framework**                 |     x     |         x          |
| **JavaScript/NodeJS**              |           |         x          |
| **Java***                          |           |         x          |
| **Op standaarden gebaseerde Open Protocol**  |           |         x          |
| **Meerdere RPC Programing modellen** |           |         x          |
*<sub>Door de algemene beschikbaarheid</sub>

## <a name="hybrid-connections"></a>Hybride verbindingen

Azure Relay hybride verbindingen mogelijkheid is een open protocol, veilige ontwikkeling van de bestaande Relay functies die kunnen worden geïmplementeerd op elk platform en in elke taal die u heeft een eenvoudige WebSocket mogelijkheid waarin expliciet de API WebSocket gemeenschappelijke webbrowsers. Hybride verbindingen is gebaseerd op http- en WebSockets.

## <a name="wcf-relays"></a>WCF relais

De Relay WCF werkt voor het volledige .NET Framework (NETFX) en voor WCF. U de verbinding tot stand brengen tussen uw lokale service en de relayservice via een reeks WCF 'relay'-bindingen. Achter de schermen wordt de relay-bindingen toewijzen aan nieuwe transport bindende elementen ontworpen WCF kanaal componenten die worden geïntegreerd met de Bus in de cloud-Service maken.

## <a name="service-history"></a>Servicehistorie

Hybride verbindingen het eerste evenredig met de naam 'BizTalk-Services'-functie die is gebaseerd op de Azure Service Bus WCF Relay verspeende. De nieuwe mogelijkheid van hybride verbindingen vormt een aanvulling op de bestaande WCF-Relay, en deze twee mogelijkheden zal bestaan naast elkaar in de Relay-service voor de nabije toekomst; ze delen een gemeenschappelijke gateway, maar anders zijn verschillende implementaties.

WCF-Relay is de oude Relay bieden dat veel klanten al met hun WCF programing modellen kunnen gebruiken.

## <a name="next-steps"></a>Volgende stappen:

- [Veelgestelde vragen over doorsturen](relay-faq.md)
- [Een naamruimte maken](relay-create-namespace-portal.md)
- [Aan de slag met .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Aan de slag met het knooppunt](relay-hybrid-connections-node-get-started.md)
<properties 
    pageTitle="Overzicht Service Bus AMQP met Java | Microsoft Azure" 
    description="Informatie over het gebruik van Java met de geavanceerde Message Queuing-Protocol (AMQP) 1.0 in Azure." 
    services="service-bus" 
    documentationCenter="java" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="amqp-10-support-in-service-bus"></a>Ondersteuning van AMQP 1.0 in Service Bus

De Azure Service Bus cloud-service en de op prem ondersteund door de geavanceerde Message Queuing Protocol (AMQP) 1.0 [Service Bus voor Windows Server (Service Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) . AMQP kunt u cross-platform, hybride toepassingen met behulp van een open standaard protocol maken. U kunt toepassingen met behulp van onderdelen die zijn gemaakt met behulp van verschillende talen en -kaders en die worden uitgevoerd op verschillende besturingssystemen maken. Al deze onderdelen verbinding met de Service Bus en naadloos maken kunnen wisselen berichten uit gestructureerde business efficiënt en volledig betrouwbaar.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Inleiding: Wat is AMQP 1.0 en waarom zijn deze belangrijk?

Traditioneel wordt message-oriented middleware ' producten oorspronkelijke protocollen gebruikt voor communicatie tussen clienttoepassingen en makelaars. Dit betekent dat wanneer u een bepaalde leverancier messaging broker hebt geselecteerd, moet u de bibliotheken van de leverancier verbinding maken met de makelaar die uw client-toepassingen. Dit resulteert in een mate van afhankelijkheid van die leverancier, aangezien het overdragen van een toepassing naar een ander product codewijzigingen in de gekoppelde toepassingen. 

Bovendien is het lastig messaging makelaars aansluiten bij verschillende leveranciers. Meestal is vereist op toepassingsniveau overbruggen om berichten te verplaatsen van het ene systeem naar het andere en tussen hun eigen indelingen omzetten. Dit is een algemene vereiste; bijvoorbeeld, wanneer u moet bieden een nieuwe, eenduidige interface voor oudere ongelijksoortige systemen, of na een fusie van IT-systemen te integreren.

De software-industrie is een snel bewegende bedrijf; nieuwe programmeertalen en toepassingskaders worden soms heel breed tempo geïntroduceerd. Op dezelfde manier mettertijd veranderen de eisen van IT-systemen en ontwikkelaars willen profiteren van de nieuwste functies van het platform. Echter, soms de geselecteerde leverancier messaging biedt geen ondersteuning voor deze platforms. Messaging-protocollen zijn eigen, is het niet mogelijk voor andere bibliotheken bieden voor deze nieuwe platforms. Daarom moet u benaderingen zoals gateways of waarmee u kunt doorgaan met het gebruik van het product van de messaging-bruggen bouwen.

De ontwikkeling van de geavanceerde Message Queuing-Protocol (AMQP) 1.0 is gemotiveerd door deze problemen. Deze afkomstig is van JP Morgan Chase, die net als de meeste financiële dienstverleners, zwaar gebruik van het message-oriented middleware '. De doelstelling is eenvoudig: voor het maken van een open standaard messaging protocol dat bericht gebaseerde toepassingen met behulp van onderdelen die zijn gebouwd met behulp van verschillende talen, kaders en besturingssystemen, maken alle gebruik van beste beschikbare onderdelen van een aantal leveranciers.

## <a name="amqp-10-technical-features"></a>AMQP 1.0 technische functies

AMQP 1.0 is een efficiënte, betrouwbare, draad niveau messaging protocol waarmee u kunt bouwen robuuste, platforms, toepassingen voor berichtafhandeling. Het protocol heeft een eenvoudige doelstelling: het mechanisme van de veilige, betrouwbare en efficiënte overdracht van berichten tussen twee partijen te definiëren. De berichten zelf worden gecodeerd met behulp van een weergave voor overdraagbare gegevenstypen waarmee heterogene afzenders en ontvangers voor het uitwisselen van gestructureerde zakelijke berichten op volledig betrouwbaar. Hieronder volgt een samenvatting van de belangrijkste functies:

*    **Efficiënt**: AMQP 1.0 is een verbindingsgeoriënteerd protocol dat wordt gebruikt voor het protocol-instructies en de zakelijke berichten een binaire codering overgedragen. Het is uitgerust met geavanceerde datatransportbesturing regelingen te maximaliseren van het gebruik van het netwerk en de verbonden onderdelen. Dat zei, het protocol is ontworpen op een evenwicht tussen efficiëntie, flexibiliteit en interoperabiliteit te vinden zijn.
*    **Betrouwbaar**: de AMQP 1.0-protocol kunnen berichten worden uitgewisseld met een bereik van betrouwbaarheid, garanties van brand-en-vergeet aan betrouwbare, precies-levering eenmaal bevestigd.
*    **Flexibel**: AMQP 1.0 is een flexibele protocol die kan worden gebruikt ter ondersteuning van verschillende topologieën. Hetzelfde protocol kan worden gebruikt voor de communicatie van client-naar-client en client-naar-makelaar makelaar tot makelaar.
*    **Onafhankelijke Broker model**: de AMQP 1.0-specificatie maakt geen vereisten voor een model van de berichten die worden gebruikt door een makelaar. Dit betekent dat het mogelijk eenvoudig AMQP 1.0-ondersteuning toevoegen aan bestaande messaging makelaars is.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 is een standaard (met de hoofdletter ')

AMQP 1.0 is in ontwikkeling sinds 2008 door een kerngroep van meer dan 20 bedrijven, zowel technologie leveranciers en eindgebruikers ondernemingen. Gedurende die tijd gebruiker ondernemingen hun concrete zakelijke vereisten hebben bijgedragen en de technologieleveranciers het protocol om te voldoen aan deze eisen hebben ontwikkeld. Gedurende het hele proces, hebben leveranciers deelgenomen aan workshops waarin ze de handen in elkaar om de interoperabiliteit tussen hun implementaties te valideren.

In oktober 2011 werd de ontwikkelwerkzaamheden overgestapt naar een technisch comité binnen de organisatie voor de bevordering van Structured Information Standards (OASIS) en de standaard OASIS AMQP 1.0 uitgebracht in oktober 2012. De volgende bedrijven deelgenomen aan het technisch comité tijdens de ontwikkeling van de standaard:

*    **Leveranciers van technologie**: Axway Software, Huawei Technologies, vorm Software, INETCO systemen, Kaazing, Microsoft, Mitre Corporation, Primeton Technologies, voortgang Software, Red Hat, SITA, Software AG, Solace systemen, VMware, WSO2, Zenika.
*    **Gebruiker ondernemingen**: Bank of America, Credit Suisse, Deutsche Boerse, Goldman Sachs, JPMorgan Chase.

Enkele voordelen van de vaak geciteerde van open standaarden zijn:

*    Minder kans op vaste leverancier
*    Interoperabiliteit
*    Ruime beschikbaarheid van bibliotheken en gereedschappen
*    Bescherming tegen veroudering
*    Beschikbaarheid van competente medewerkers
*    Lagere en beheersbare risico

## <a name="amqp-10-and-service-bus"></a>AMQP 1.0 en Service Bus

Ondersteuning in Azure Service Bus AMQP 1.0 betekent dat u kunt nu profiteren van de Service Bus queuing en publiceren/abonneren brokered functies voor berichten van een aantal platforms met behulp van een efficiënte binaire protocol. Bovendien kunt u toepassingen bestaat uit onderdelen die zijn gebouwd met behulp van een combinatie van talen, kaders en besturingssystemen maken.

De volgende afbeelding ziet u een voorbeeld van de implementatie waar Java clients op Linux, geschreven met de standaard Java Message Service (JMS) API en .NET-clients op Windows, uitwisselen van berichten via Service Bus met behulp van AMQP 1.0.

![][0]

**Afbeelding 1: Voorbeeld-inzetten met cross-platform messaging Service Bus en AMQP 1.0**

Op dit moment zijn de volgende clientbibliotheken bekend om te werken met de Service Bus:

| Taal | Bibliotheek                                                                       |
|----------|-------------------------------------------------------------------------------|
| Java     | Client Apache Qpid Java Message Service (JMS)<br/>VORM Software SwiftMQ Java client |
| C        | Apache Qpid Proton-C                                                          |
| PHP      | Apache Qpid Proton-PHP                                                        |
| Python   | Apache Qpid Proton-Python                                                     |


**Afbeelding 2: Tabel van AMQP 1.0-client libraries**

Zie voor meer informatie over het verkrijgen en gebruiken van deze bibliotheken met Bus Service [Service Bus AMQP Developer's Guide][]. Zie de sectie [volgende stappen](service-bus-java-amqp-overview.md#next-steps) voor koppelingen naar meer informatie.

## <a name="summary"></a>Samenvatting

*    AMQP 1.0 is een open, betrouwbare messaging protocol waarmee u kunt cross-platform, hybride toepassingen te bouwen. AMQP 1.0 is een standaard OASIS.
*    Ondersteuning van AMQP 1.0 is nu beschikbaar in Azure Service Bus als Service Bus voor Windows Server (Service Bus 1.1). Prijzen is hetzelfde als voor de bestaande protocollen.

## <a name="next-steps"></a>Volgende stappen

Ga naar de volgende koppelingen voor meer informatie over ondersteuning van AMQP in Service Bus.

*    [AMQP 1.0 gebruiken met de Service Bus .NET API](service-bus-dotnet-advanced-message-queuing.md)
*    [Het gebruik van Java Message Service (JMS) API met Bus Service & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Service Bus AMQP Developer's Guide][]
*    [OASIS geavanceerde Message Queuing-Protocol (AMQP) 1.0-specificatie](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)

[0]: ./media/service-bus-java-amqp-overview/Example1.png
[Service Bus AMQP Developer's Guide]: service-bus-amqp-dotnet.md

 

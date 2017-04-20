<properties
    pageTitle="Service Bus Premium en Standard Messaging prijzen lagen overzicht | Microsoft Azure"
    description="Service Bus Premium en Standard Messaging"
    services="service-bus"
    documentationCenter=".net"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/02/2016"
    ms.author="darosa;sethm"/>

# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Service Bus Premium en Standard messaging lagen 

Service Bus berichten, waaronder berichten entiteiten zoals wachtrijen en combineert berichtenuitwisseling voor grote mogelijkheden met rich-onderwerpen publiceren-abonneren semantiek op schaal cloud. Messaging service Bus wordt de ruggengraat voor de communicatie gebruikt voor vele geavanceerde cloud-oplossingen.

De *Premium* -laag van messaging Service Bus adressen aanvragen van gemeenschappelijke klanten rond schaal, prestaties en beschikbaarheid voor bedrijfskritieke toepassingen. Hoewel de functiesets vrijwel identiek zijn, worden deze twee lagen van messaging Service Bus bedoeld voor andere use-cases.

Enkele verschillen op hoog niveau worden gemarkeerd in de onderstaande tabel.

| Premium                               | Standaard                       |
|---------------------------------------|--------------------------------|
| Hoge doorvoer                       | Variabele doorvoer            |
| Voorspelbare prestaties               | Variabele latentie               |
| Voorspelbare prijzen                   | Pay as you go variabele prijzen |
| Vermogen uit te breiden en werkbelasting | N.V.T.                            |
| Het formaat van bericht > 256KB                  | Grootte van het bericht is 256KB          |

**Service Bus Premium Messaging** biedt resource isolatie op de CPU en het geheugen laag, zodat de werklast voor elke klant wordt uitgevoerd in een geïsoleerde omgeving. Deze container resource heet een *messaging-eenheid*. Elke naamruimte premium wordt ten minste één messaging eenheid worden toegewezen. U kunt 1, 2 of 4 messaging eenheden voor elke naamruimte Service Bus Premium kopen. Een enkele werkbelasting of entiteit kan meerdere berichten eenheden omvatten en het aantal berichten eenheden kan worden gewijzigd op wordt, hoewel facturering in 24 uur of dagelijks bijdragen is. Het resultaat is een voorspelbare en herhaalbare prestaties voor uw oplossing op basis van een Service Bus.

Niet alleen is deze prestaties meer voorspelbare en beschikbaar, maar is ook sneller. Messaging service Bus Premium is gebaseerd op de opslag-engine geïntroduceerd in [Azure gebeurtenis Hubs](https://azure.microsoft.com/services/event-hubs/). Premium messaging is topprestaties veel sneller dan met de standaard laag.

## <a name="premium-messaging-technical-differences"></a>Premium Messaging technische verschillen

De volgende zijn enkele verschillen tussen Premium en Standard messaging lagen.

### <a name="partitioned-queues-and-topics"></a>Gepartitioneerde wachtrijen en onderwerpen

Gepartitioneerde wachtrijen en onderwerpen van Premium messaging worden ondersteund, maar werken deze niet op dezelfde manier als in de standaard en Basic lagen van messaging Service Bus. Premium messaging gebruikt geen SQL als gegevensarchief en niet langer is de concurrentie van de bronnen die zijn gekoppeld aan een gemeenschappelijke platform. Partitioneren is hierdoor niet nodig. Bovendien is het aantal partities van 16 partities in standaard messaging 2 partities in Premium gewijzigd. Met twee partities beschikbaarheid garandeert en een meer geschikt is voor de Premium-runtime-omgeving. Zie voor meer informatie over het partitioneren van [Partitioned-wachtrijen en onderwerpen](service-bus-partitioning.md).

### <a name="express-entities"></a>Express diensten

Aangezien Messaging Premium wordt uitgevoerd in een volledig geïsoleerd runtimeomgeving, express diensten niet worden ondersteund in naamruimten Premium. Zie de eigenschap [Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx) voor meer informatie over de functie express.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over Service Bus berichten.

- [Introductie van Azure Service Bus Premium messaging (blogbericht)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [Introductie van Azure Service Bus Premium messaging (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [Messaging service Bus-overzicht](service-bus-messaging-overview.md)
- [Het gebruik van wachtrijen Service Bus](service-bus-dotnet-get-started-with-queues.md)

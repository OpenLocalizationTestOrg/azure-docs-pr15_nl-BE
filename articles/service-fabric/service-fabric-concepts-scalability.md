<properties
   pageTitle="Schaalbaarheid van configuratieservices Service | Microsoft Azure"
   description="Hierin wordt beschreven hoe u Service configuratieservices schalen"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="scaling-service-fabric-applications"></a>Schaling, toepassingen voor Service Fabric
Azure Service Fabric kunt u schaalbare toepassingen met taakverdeling services, partities en replica's op alle knooppunten in een cluster maken. Hierdoor kunt maximale brongebruik.

Hoge schaal voor Service Fabric-toepassingen kan worden bereikt op twee manieren:

1. Schalen op het niveau van partitie

2. Op het niveau van service naam schaling

## <a name="scaling-at-the-partition-level"></a>Schalen op het niveau van partitie
Fabric-service ondersteunt partitioneren van een afzonderlijke service in meerdere kleinere partities. De [partitionering overzicht](service-fabric-concepts-partitioning.md) vindt u informatie over de soorten partitionering schema's die worden ondersteund. De replica's van elke partitie verspreid zijn over de knooppunten in een cluster. U kunt een service die gebruikmaakt van een ranged partitieschema met een lage sleutel van 0 en een hoge sleutel van 99 vier partities. In een cluster met drie knooppunten, kan de service met vier replica's die de bronnen op elk knooppunt delen als volgt worden ingedeeld:

![Partitie-indeling met drie knooppunten](./media/service-fabric-concepts-scalability/layout-three-nodes.png)

Verhoging van het aantal knooppunten kunt Fabric-Service de bronnen op de nieuwe knooppunten door enkele van de replica's verplaatsen naar lege knooppunten gebruiken. Doordat het aantal knooppunten op vier heeft de service nu drie replica's die worden uitgevoerd op elk knooppunt (van verschillende partities), waardoor een beter gebruik van bronnen en prestaties.

![Partitie-indeling met vier knooppunten](./media/service-fabric-concepts-scalability/layout-four-nodes.png)

## <a name="scaling-at-the-service-name-level"></a>Op het niveau van service naam schaling
Een service-exemplaar is een specifiek exemplaar van een naam en de naam van een service-type (Zie [levensduur Service Fabric-toepassing](service-fabric-application-lifecycle.md)). Tijdens het maken van een service, u de partitie scheme (Zie [Service-configuratieservices partitioneren](service-fabric-concepts-partitioning.md)) moet worden gebruikt.

Het eerste niveau van de schaal is door de naam van de service. Nieuwe exemplaren van een service, kunt u maken met de verschillende niveaus van partitionering, als de oudere exemplaren van de service worden bezet. Hierdoor kunnen nieuwe service consumenten minder bezet-service-exemplaren, in plaats van veelgebruikte velden gebruiken.

Eén optie voor het verhogen van capaciteit, alsook de stijgende of dalende partitie telt, is een nieuwe service-exemplaar maken met een nieuwe partitieschema. Dit voegt complexiteit, echter als beslag clients moeten weten wanneer en hoe u de service anders benoemd.

### <a name="example-scenario-embedded-dates"></a>Voorbeeldscenario: ingesloten datums
Een voorbeeldscenario zou zijn datumgegevens gebruiken als onderdeel van de servicenaam van de. Zo kunt u met een bepaalde naam voor alle klanten die in 2013 tot een service-exemplaar en een andere naam voor klanten die in 2014 tot. Dit schema voor naamgeving kunt u programmatisch verhogen de namen afhankelijk van de datum (2014 nadert, het exemplaar van de voor 2014 kan worden gemaakt op verzoek).

Deze aanpak is echter gebaseerd op de clients met toepassingsspecifieke naamgevingsinformatie die buiten het bereik van de Service Fabric kennis.

- *Met behulp van een naamgevingsconventie*: In 2013, wanneer uw toepassing live gaat, maakt u een service genaamd fabric: app/service2013. In de buurt van het tweede kwartaal van 2013, maakt u een andere service, genaamd fabric: app/service2014. Beide services zijn van hetzelfde servicetype. In deze benadering moet de client logica voor het maken van de juiste naam op basis van het jaar in dienst.

- *Met behulp van een opzoeken service*: een ander patroon is bedoeld als een service voor het secundaire zoeken, die de naam van de service voor een gewenste sleutel kan bieden. Nieuwe exemplaren van de service kunnen worden gemaakt door de service voor het zoeken. De lookup-service zelf wordt niet behouden alle toepassingsgegevens alleen gegevens over de servicenamen die wordt gemaakt. Dus het jaar, op basis van bovenstaande voorbeeld, de client zou eerst contact opnemen met de service voor het zoeken naar de naam van de service voor het verwerken van gegevens voor een bepaald jaar, en gebruikt u die servicenaam voor het uitvoeren van de huidige bewerking. Het resultaat van de eerste zoekopdracht opgeslagen kan worden.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie op Fabric Service concepten:

- [Beschikbaarheid van de Service configuratieservices](service-fabric-availability-services.md)

- [Service configuratieservices partitioneren](service-fabric-concepts-partitioning.md)

- [Definiëren en beheren van staat](service-fabric-concepts-state.md)

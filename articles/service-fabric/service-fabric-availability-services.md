<properties
   pageTitle="Beschikbaarheid van de Service configuratieservices | Microsoft Azure"
   description="Beschrijving van foutenopsporing, failover en herstel voor services"
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

# <a name="availability-of-service-fabric-services"></a>Beschikbaarheid van de Service configuratieservices
Azure Service configuratieservices kunnen stateful of stateless zijn. Dit artikel geeft een overzicht van hoe Fabric-Service de beschikbaarheid van een service in het geval van storingen houdt.

## <a name="availability-of-service-fabric-stateless-services"></a>Beschikbaarheid van de Service stateless configuratieservices
Een stateless service is een toepassing die geen [lokale permanente status](service-fabric-concepts-state.md).

Een stateless service maken, moet een exemplaar-telling is het aantal exemplaren van de staatloze service die moet worden uitgevoerd in het cluster te definiëren. Dit is het aantal exemplaren van de toepassingslogica die zal worden gemaakt in het cluster. Het aantal exemplaren te verhogen, is de aanbevolen manier van het schalen van stateless service.

Bij een storing op elk exemplaar van een staatloos service wordt gevonden, wordt een nieuw exemplaar gemaakt op sommige in aanmerking komende knooppunt in het cluster.

## <a name="availability-of-service-fabric-stateful-services"></a>Beschikbaarheid van de Service stateful configuratieservices
Een stateful service heeft een bepaalde status. In Service-Fabric is een stateful service gemodelleerd als een set van replica's. Elke replica is een exemplaar van de code van de service die beschikt over een kopie van de staat. Lezen en schrijven-bewerkingen worden uitgevoerd in de ene replica (de primaire genoemd). Wijzigingen in de status van schrijven bewerkingen zijn *gerepliceerd* naar verschillende andere replica's (actieve secundaire servers genoemd). De combinatie van primaire en actieve secundaire replica's is de replicaset van de service.

Kan er slechts één primaire replica onderhoud lees- en schrijfopdrachten, maar kunnen er meerdere actieve secundaire replica's. Het aantal actieve secundaire replica's kan worden geconfigureerd en een hoger aantal replica's kan een groter aantal gelijktijdige software- en hardwarestoringen tolereren.

In het geval van een probleem (als de primaire replica uitvalt) Fabric-Service is een van de actieve secundaire replica's de nieuwe primaire replica. Deze actieve secundaire replica is al de bijgewerkte versie van de staat (via *replicatie*), en kan doorgaan met het verwerken van verdere lezen en schrijven.

Dit concept--van een replicaset wordt een primaire of een actieve secundaire--als de rol van de replica is bekend.

### <a name="replica-roles"></a>Replica-rollen
De rol van een replica die wordt gebruikt voor het beheren van de levenscyclus van de status van de replica die wordt beheerd. Aanvragen voor een replica waarvan de functie is de primaire services lezen. Ook verzorgt schrijfopdrachten door het bijwerken van de status en de wijzigingen te repliceren naar de actieve secundaire servers in de replicaset. De rol van een actieve secundaire is ontvangen de status verandert dat de primaire replica is gerepliceerd en de weergave van de staat.

>[AZURE.NOTE] Op een hoger niveau programming modellen zoals de [betrouwbare actoren framework](service-fabric-reliable-actors-introduction.md) abstracte weg het begrip van de rol van de ontwikkelaar van de replica.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie op Fabric Service concepten:

- [Schaalbaarheid van Service configuratieservices](service-fabric-concepts-scalability.md)

- [Service configuratieservices partitioneren](service-fabric-concepts-partitioning.md)

- [Definiëren en beheren van staat](service-fabric-concepts-state.md)

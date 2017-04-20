<properties
   pageTitle="Introductie van de Service Fabric Cluster Resource Manager | Microsoft Azure"
   description="Een inleiding tot de Service Cluster Resource Manager Fabric."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Introductie van de Service Fabric cluster resourcemanager
Traditioneel beheer van IT-systemen of een aantal services betekende een paar fysieke of virtuele machines die speciaal voor die specifieke services of systemen. Veel belangrijke services zijn onderverdeeld in een laag 'web' en een 'data' of 'storage' tier, misschien met een paar andere gespecialiseerde onderdelen zoals een cache. Andere typen toepassingen zou een messaging-tier waar aanvragen gestroomd in-en uitchecken, verbinding hebben met een laag werk voor analyse- of transformatie nodig als onderdeel van de berichtgeving. Elk type werklast hebt u een specifieke machines die speciaal voor dit: de database hebt u een paar computers toegewezen, de webservers een weinig. Als een bepaald type werkbelasting van de machines veroorzaakt was te warm uitvoeren en vervolgens meer machines met dat soort werkbelasting geconfigureerd voor uitvoering op het toe te voegen, of een paar van de machines vervangen door grotere machines. Eenvoudig. Als een machine is mislukt, is dat deel van de algemene toepassing werd uitgevoerd op lagere capaciteit totdat de computer kan worden hersteld. Nog steeds vrij eenvoudig (als dat niet per se leuk).

Nu, laten we zeggen je hebt gevonden moet worden geschaald naar buiten en hebben genomen, containers en/of microservice zich aanmeldt. Plotseling u uzelf met tientallen, honderden of zelfs duizenden machines, tientallen verschillende soorten diensten, misschien wel honderden verschillende exemplaren van deze diensten, elk met een of meer instanties of replica's voor High Availability (HA).

Plotseling beheer van uw omgeving is niet zo gemakkelijk als het beheren van een paar machines speciaal voor enkele soorten werkbelasting. Uw servers zijn virtuele en namen niet meer hebben (u *hebt* overgeschakeld mindsets van [huisdieren op runderen](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) na alle). Configuratie is minder over de machines en meer informatie over de diensten zelf. Specifieke hardware is een nu tot het verleden en services zijn geworden kleine spanning meerdere kleinere stukken van hardware grondstoffen gedistribueerde systemen.

Als gevolg van uw app voorheen monolithische, gelaagde verdelen in afzonderlijke services uitvoert op hardware van goederen, hebt u nu veel meer combinaties te behandelen. Wie beslist wat soorten werklasten kunnen worden uitgevoerd op welke hardware, of hoe veel? Welke werkbelasting op dezelfde hardware goed werkt en die in strijd zijn? Als een computer uitvalt... wat er nog actief is? Wie is verantwoordelijk voor ervoor te zorgen dat belasting wordt gestart opnieuw uit te voeren? U wachten tot de machine (virtual?) terugkomen of uw werkbelasting automatisch een failover naar andere computers en uitvoering? Menselijke tussenkomst vereist is? Wat over upgrades in dit soort omgeving?

Als ontwikkelaars en operatoren die in dit soort wereld gaan we enkele hulp bij het beheren van deze complexiteit en krijgt u het gevoel dat een binge aanneemt en probeert om het papier over de complexiteit met mensen is niet het juiste antwoord.

Wat te doen?

## <a name="introducing-orchestrators"></a>Introductie van orchestrators
Een "Orchestrator" is de algemene term voor een stukje software waarmee beheerders beheren van dit soort omgevingen. Orchestrators zijn de onderdelen die worden uitgevoerd in aanvragen, zoals 'Ik wil graag 5 exemplaren van deze service in Mijn omgeving', maakt u deze waarde true is, en vervolgens (proberen) te houden op die manier.

Orchestrators (geen mens) zijn wat slingeren in actie als een computer uitvalt of een werklast voor een onverwachte reden beëindigd. De meeste Orchestrators meer dan alleen omgaan met fout, zoals helpen met nieuwe installaties, upgrades verwerken en omgaan met een verbruik van middelen, maar alle zijn fundamenteel over het onderhouden van dat sommige gewenste status van de configuratie in de omgeving. U wilt dat een Orchestrator vertellen wat u wilt en deze doen het zware werk. Chronos Marathon op Mesos vloot, Swarm, Kubernetes en Service weefsel zijn allemaal voorbeelden van Orchestrators (of hebben ze ingebouwd). Meer de tijd als de complexiteit van de echte wereld implementaties in verschillende omgevingen worden gemaakt en voorwaarden groeien en wijzigen.

## <a name="orchestration-as-a-service"></a>Orchestration als een service
De taak van de Orchestrator binnen een cluster Service Fabric, voornamelijk door het Cluster Resource Manager wordt verwerkt. De Service Fabric Cluster Resource Manager is een van de System-Services binnen Service-structuur en wordt automatisch gestart binnen een cluster.  Taak van het Cluster Resource Manager is meestal onderverdeeld in drie delen:

1. Regels afdwingen
2. Optimaliseren van uw omgeving
3. Assisteren bij andere processen

### <a name="what-it-isnt"></a>Het is niet
In traditionele N-tier web-apps is er altijd een begrip van een "taakverdeling", waarnaar gewoonlijk wordt verwezen als een Load Balancer NLB (Network) of een toepassing Load Balancer (ALB) afhankelijk van waar het in de-netwerkstack zat. Sommige netwerktaakverdeling zoals F5 van BigIP bieden op basis van Hardware, anderen zijn bijvoorbeeld van Microsoft software beheer van Netwerktaakverdeling. In andere omgevingen kan worden weergegeven als HAProxy in deze rol. Deze architectuur is de taak van netwerktaakverdeling om ervoor te zorgen dat alle van de verschillende stateless front-end machines of andere computers in het cluster ontvangen (ongeveer) dezelfde hoeveelheid werk. Strategieën voor deze variëren van elk ander gesprek naar een andere server verzenden naar sessie vastzetten/kleverigheid, werkelijke schatting en oproep toewijzing op basis van de verwachte kosten en de huidige computer laden.

Merk op dat dit met de beste was het mechanisme om ervoor te zorgen dat de weblaag bleef min of meer in evenwicht. Strategieën voor de verdeling van de gegevenslaag zijn volledig anders en afhankelijk van het gegevensopslagmechanisme, meestal rond sharding gegevens centreren, caching, beheerde weergaven en opgeslagen procedures, enz.

Enkele van deze strategieën zijn interessante, is Service Fabric Cluster Resource Manager niet iets als een netwerk-taakverdeling of een cache. Terwijl een Network Load Balancer zorgt ervoor dat de front-ends in evenwicht zijn doordat verkeer naar waar de services worden uitgevoerd, de Service Fabric Cluster Resource Manager heeft een geheel andere strategie – fundamenteel, Fabric Service *services* te maken, indien zij het meeste nut verplaatst (en verwacht dat verkeer of het laden te volgen). Dit kan zijn, bijvoorbeeld knooppunten die momenteel koud zijn, omdat de diensten die er zijn veel werk niet nu doen of die zijn verwijderd of verplaatst. Een ander voorbeeld kan de Cluster Resource Manager ook een service van een machine die is bijgewerkt of die overbelast is als gevolg van een plotselinge piek in het verbruik door de services die actief waren op het verplaatsen. Omdat de Cluster Resource Manager is verantwoordelijk voor het verplaatsen van diensten rond (geen netwerkverkeer aan waar al zijn geleverd), bevat een aanzienlijk verschilt functieset in vergelijking met wat u in een netwerk-taakverdeling vinden zou en maakt gebruik van fundamenteel verschillende strategieën om ervoor te zorgen dat ook de hardwarebronnen in het cluster worden gebruikt.

## <a name="next-steps"></a>Volgende stappen
- Raadpleeg [dit artikel](service-fabric-cluster-resource-manager-architecture.md) voor informatie over de architectuur en informatie stroom binnen het Cluster Resource manager,
- De Cluster Resource Manager heeft een groot aantal opties voor het beschrijven van het cluster. Meer informatie over deze Kijk uit dit artikel bij het [beschrijven van een cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)
- Voor meer informatie over de opties beschikbaar voor het configureren van services het onderwerp uitchecken op de andere Cluster Resource Manager-configuraties beschikbaar, [informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)
- Meetgegevens worden hoe de Service Fabric Cluster Resource Manager beheert, verbruik en capaciteit in het cluster. Voor meer Kijk informatie over deze en hoe u deze configureert dan eens [in dit artikel](service-fabric-cluster-resource-manager-metrics.md)
- De Cluster Resource Manager werkt met mogelijkheden voor Service-structuur. Lees [dit artikel](service-fabric-cluster-resource-manager-management-integration.md) als u wilt meer weten over deze integratie,
- Bekijk voor meer informatie over hoe het Cluster Resource Manager beheert en saldi laden in het cluster, het artikel op [load balancing](service-fabric-cluster-resource-manager-balancing.md)

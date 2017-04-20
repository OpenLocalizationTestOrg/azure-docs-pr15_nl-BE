<properties
   pageTitle="Verschillen tussen Cloud diensten en Service Fabric | Microsoft Azure"
   description="Algemene informatie voor het migreren van toepassingen van Cloud-Services aan de Fabric-Service."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Meer informatie over de verschillen tussen de Cloud diensten en Service-structuur voordat u migreert toepassingen.
Microsoft Azure Service Fabric is een platform voor de volgende generatie wolk voor sterk schaalbare, uiterst betrouwbaar en gedistribueerde toepassingen. Het geeft veel nieuwe functies voor verpakking, implementatie, bijwerken en beheren van cloud gedistribueerde toepassingen. 

Dit is een inleidende gids voor het migreren van toepassingen van Cloud-Services aan de Fabric-Service. Het richt zich voornamelijk op architectuur en ontwerp verschillen tussen Cloud diensten en Service-Fabric.
 
## <a name="applications-and-infrastructure"></a>Toepassingen en infrastructuur

Een fundamenteel verschil tussen Cloud diensten en Service-structuur is de relatie tussen VMs, werkbelasting en toepassingen. Hier een werkbelasting wordt gedefinieerd als de code schrijven naar een bepaalde taak of een service te bieden.
 
 - **Cloud Services is over het implementeren van toepassingen als VMs.** De code die u schrijft, is nauw verbonden met een VM-exemplaar, zoals een webpagina of een functie van de werknemer. Is voor de implementatie van een of meer VM-exemplaren die worden uitgevoerd de werkbelasting voor de implementatie van een werkbelasting in Cloud-Services. Er is geen scheiding van de toepassingen en VMs en dus er is geen formele definitie van een toepassing. Een toepassing kan beschouwd worden als een set Web of werknemer rol exemplaren binnen een Cloud Services-implementatie of een hele Cloud Services-implementatie. In dit voorbeeld wordt een toepassing weergegeven als een aantal exemplaren van de rol.
 
![Cloud Services-toepassingen en -topologie][1]

 - **Service Fabric is over de implementatie van toepassingen aan bestaande VMs of Fabric-Service uitgevoerd onder Windows of Linux machines.** De services die u schrijft zijn volledig van de onderliggende infrastructuur, beschouwing weg door het weefsel Service application platform, zodat een toepassing kan worden geïmplementeerd in omgevingen met meerdere losgekoppelde. Een werkbelasting in Fabric-Service is een 'service' genoemd en een of meer services zijn gegroepeerd in een formeel gedefinieerde toepassing die wordt uitgevoerd op een platform voor de Service-Fabric. Meerdere toepassingen kunnen worden geïmplementeerd op een enkele Service Fabric '-cluster.
 
![Service Fabric-toepassingen en -topologie][2]
 
Service Fabric zelf is platform toepassingslaag die compatibel is met Windows of Linux, terwijl Cloud Services een systeem is voor het implementeren van beheerde Azure VMs met werklasten gekoppeld.
Het toepassingsmodel Fabric-Service heeft een aantal voordelen:

 - Snelle implementatietijden. VM-instanties maken kan enige tijd duren. VMs zijn in Service Fabric, alleen geïmplementeerd als een cluster vormen die fungeert als host voor platform voor de Service-Fabric. Vanaf dat moment kunnen toepassingspakketten worden geïmplementeerd met de cluster zeer snel.
 - Met hoge dichtheid die als host fungeert. Een werknemer rol VM host in Cloud-Services, een werkbelasting. In Service-Fabric zijn toepassingen gescheiden van het VMs die worden uitgevoerd, wat betekent dat u een groot aantal toepassingen op een klein aantal VMs die de totale kosten voor grotere implementaties verlaagt kunt implementeren.
 - Service platform van overal die uitvoeren kunt stof is Windows Server en Linux machines, Azure of op locatie. Het platform biedt een abstractie laag boven de onderliggende infrastructuur zodat de toepassing kan worden uitgevoerd op verschillende omgevingen. 
 - Beheer van gedistribueerde toepassingen. Service-structuur is een platform dat niet alleen host voor toepassingen gedistribueerde, maar ook helpt hun levenscyclus onafhankelijk van de VM hosting of de levenscyclus van de machine beheren.

## <a name="application-architecture"></a>Toepassingsarchitectuur

De architectuur van een Cloud Services-toepassing bevat meestal verschillende externe serviceafhankelijkheden zoals Service Bus, tabel Azure Blob-opslag, SQL, bestand Vgx. en anderen voor het beheren van de staat en de gegevens van een toepassing en de communicatie tussen Web en rollen van de werknemer in een Cloud Services-implementatie. Een voorbeeld van een volledige toepassing van de Cloud-Services uitzien als volgt:  

![Cloud Services-architectuur][9]

Service Fabric-toepassingen kunt gebruiken dezelfde externe services in een volledige toepassing. Cloud Services-architectuur van het volgende voorbeeld is het eenvoudigste migratiepad dat van Cloud-Services aan de Fabric-Service de implementatie van de Cloud-Services vervangen door een toepassing Service Fabric, houden de algehele opzet hetzelfde. Het Web en werknemer rollen worden overgezet naar Service stateless configuratieservices met minimale code wijzigt.

![Na de migratie eenvoudig Fabric '-architectuur-service][10]

In dit stadium, moet het systeem blijven werken hetzelfde als voorheen. Voordeel te halen uit de Service-Fabric stateful functies externe staat winkels kan worden internalized als stateful services, indien van toepassing. Dit is iets ingewikkelder dan een eenvoudige migratie van Web- en rollen van de werknemer naar Service stateless configuratieservices, het schrijven van aangepaste services dezelfde functionaliteit aan uw toepassing bieden als de externe diensten voor nodig. De voordelen hiervan zijn: 

 - Externe afhankelijkheden verwijderen 
 - De implementatie, beheer en upgrades modellen te verenigen. 
 
Een voorbeeld van de resulterende architectuur van deze services internalizing kan er zo uit:

![Service Fabric '-architectuur na volledige migratie][11]

## <a name="communication-and-workflow"></a>Communicatie en workflow

De meeste Cloud Service toepassingen bestaan uit meer dan één laag. Op dezelfde manier bestaat een Service Fabric-toepassing uit meer dan één service (meestal veel services). Twee gemeenschappelijke communicatie-modellen zijn directe communicatie en via een duurzame externe opslag.

### <a name="direct-communication"></a>Directe communicatie

Met directe communicatie communiceren niveaus rechtstreeks via het eindpunt door elke laag weergegeven. In omgevingen met stateless zoals Cloud Services, dit betekent dat een exemplaar van een rol VM ofwel willekeurig selecteren of round robin-saldo, laden en een directe verbinding naar het eindpunt.

![Cloud Services, directe communicatie][5]

 Directe communicatie is een gemeenschappelijke communicatiemodel in Fabric-Service. Het belangrijkste verschil tussen Service weefsel en Cloud Services is die in Cloud-Services u verbinding met een VM maakt, dat Service stof met een service verbonden. Dit is een belangrijk onderscheid voor een paar redenen:

 - Services in Fabric-Service zijn niet gekoppeld aan het VMs die als host fungeren Services kunnen navigeren in het cluster en in feite worden verwacht om te navigeren om verschillende redenen: Resource taakverdeling, failover, upgrades van toepassingen en infrastructuur, en beperkingen voor plaatsing of laden. Dit betekent dat een service-exemplaar adres op elk gewenst moment kunt wijzigen. 
 - Een VM Service stof kan host meerdere services, elk met unieke eindpunten.

Fabric-service biedt service discovery-mechanisme, de Naming Service, die kan worden gebruikt voor het omzetten van adressen van services eindpunt genoemd. 

![Directe communicatie Fabric-service][6]

### <a name="queues"></a>Wachtrijen

Een gemeenschappelijk mechanisme voor de communicatie tussen de lagen in stateless omgevingen zoals Cloud Services is een wachtrij voor externe opslag met blijvend opgeslagen taken vanaf één laag naar de andere. Een gebruikelijk scenario is een weblaag die taken verzendt aan een wachtrij Azure of Service Bus waar exemplaren van de rol van de werknemer kunnen in wachtrij en verwerken van de taken.

![Cloud Services wachtrij communicatie][7]

Hetzelfde communicatiemodel kan worden gebruikt in Fabric-Service. Dit kan handig zijn bij het migreren van bestaande toepassingen aan de Fabric Service Cloud-Services. 

![Directe communicatie Fabric-service][8]
 
## <a name="next-steps"></a>Volgende stappen

Het eenvoudigste migratiepad dat van Cloud-Services aan de Fabric-Service is de implementatie van de Cloud-Services vervangen door een toepassing Service Fabric, houden van de algehele architectuur van uw toepassing min of meer hetzelfde. Het volgende artikel bevat richtlijnen voor het helpen de rol van werknemer of Web converteren naar een stateless Fabric-Service-service.

 - [Eenvoudige migratie: een webpagina of de rol van de werknemer omzetten in een stateless Fabric-Service-service](./service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png

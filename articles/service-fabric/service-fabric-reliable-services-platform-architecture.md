<properties
   pageTitle="Betrouwbare service architectuur | Microsoft Azure"
   description="Overzicht van de architectuur van betrouwbare Service voor-stateless en stateful"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/30/2016"
   ms.author="alanwar"/>

# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Architectuur voor stateful en stateless betrouwbare diensten

Een betrouwbare Service voor Azure Service Fabric mogelijk stateful of stateless. Elk type van de service wordt uitgevoerd binnen een bepaalde architectuur. Deze architectuur worden in dit artikel beschreven.
Zie voor meer informatie over de verschillen tussen-stateless en stateful [betrouwbare Service-Overzicht](service-fabric-reliable-services-introduction.md) .

## <a name="stateful-reliable-services"></a>Stateful betrouwbare diensten

### <a name="architecture-of-a-stateful-service"></a>Architectuur van een stateful service
![Architectuurdiagram van een stateful service](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>Stateful betrouwbare Service

Een stateful betrouwbare Service kunt afleiden uit de StatefulService of de StatefulServiceBase klasse. Deze basis klassen worden geleverd door Fabric-Service. Ze bieden verschillende niveaus van ondersteuning en abstractie voor de stateful-service aan de Service-structuur en deel te nemen als een service in de cluster-Service Fabric.

StatefulService afgeleid van StatefulServiceBase. StatefulServiceBase services biedt meer flexibiliteit, maar vereist meer inzicht in de interne werking van weefsel van de Service.
Zie het [overzicht van de betrouwbare Service](service-fabric-reliable-services-introduction.md) en [betrouwbare Service gebruik van geavanceerde](service-fabric-reliable-services-advanced-usage.md) voor meer informatie over de specifieke kenmerken van het schrijven van services met behulp van de klassen StatefulService en StatefulServiceBase.

Beide basisklassen beheren de levensduur en de rol van de uitvoering van de service. De uitvoering van de service kan virtuele methoden van de klasse base negeren indien de uitvoering van de service te doen op die punten in de levenscyclus van de service implementatie-- of als deze wil een communicatie-listener-object te maken. Houd er rekening mee dat hoewel de implementatie van een service kan een eigen communicatie listenerobject ICommunicationListener, dat in het bovenstaande diagram implementeren de listener communicatie wordt geïmplementeerd door het Service-structuur als de uitvoering van de service maakt gebruik van een communicatie-listener die is geïmplementeerd door Fabric-Service.

De manager betrouwbare staat een stateful betrouwbare Service gebruikt om te profiteren van betrouwbare collecties. Collecties met betrouwbare lokale gegevensstructuren die uiterst beschikbaar zijn voor de service--, zijn altijd beschikbaar, ongeacht de service failover. Elk type van betrouwbare collectie wordt geïmplementeerd door een provider betrouwbare staat.
Zie voor meer informatie over collecties voor betrouwbare, [betrouwbare collecties overzicht](service-fabric-reliable-services-reliable-collections.md).

### <a name="reliable-state-manager-and-state-providers"></a>Betrouwbare staat manager en aanbieders van staat

De betrouwbare staat manager is het object dat staat voor betrouwbare aanbieders. Heeft de functionaliteit voor het maken, verwijderen, inventariseren en ervoor te zorgen dat de leveranciers van betrouwbare staat persistente en hoge beschikbaarheid. Een exemplaar van de provider staat betrouwbare vertegenwoordigt een exemplaar van een permanente en maximaal beschikbare gegevensstructuur, zoals een woordenlijst of een wachtrij.

Elke provider betrouwbare staat beschrijft een interface die wordt gebruikt door een stateful service kan interactief worden uitgevoerd met de provider die staat voor betrouwbare. Bijvoorbeeld IReliableDictionary wordt gebruikt om de interface met de woordenlijst voor betrouwbare, terwijl IReliableQueue wordt gebruikt interface met de betrouwbare wachtrij. Alle providers van betrouwbare staat implementeren de IReliableState-interface.

De betrouwbare staat manager heeft een interface met de naam IReliableStateManager die toegang van een stateful-service. Interfaces aan betrouwbare staat de leveranciers worden geretourneerd via IReliableStateManager.

De betrouwbare staat een architectuur voor invoegtoepassingen wordt gebruikt zodat nieuwe typen collecties betrouwbaar kunnen worden aangesloten dynamisch.

De betrouwbare woordenboek en betrouwbare wachtrij zijn gebaseerd op de uitvoering van een krachtige, versioned differentiële winkel.

### <a name="transactional-replicator"></a>Transactionele replicatie

Het onderdeel voor transactionele replicatie is verantwoordelijk voor dat de status van een service (de staat binnen de staat betrouwbare manager en de betrouwbare collecties) consistent is op alle replica's met de service. Ook zorgt ervoor dat de staat in het logboek is vastgelegd. De betrouwbare staat manager interfaces met transactionele replicatie via een privé-mechanisme.

Transactionele replicatie gebruikt een netwerkprotocol staat met andere replica's van het exemplaar van de communicatie, zodat alle replica's informatie over de actuele status hebben.

Transactionele replicatie wordt een logboek staat informatie behouden, zodat de informatie staat proces blijft of knooppunt vastloopt. De interface naar het logboek is via een privé-mechanisme.

### <a name="log"></a>Logboek

De logboek-component biedt een krachtige blijvende opslag die kan worden geoptimaliseerd voor schrijfbewerkingen met draaiende of solid-state schijven.  Het ontwerp van het logboek is het voor de permanente opslag (dat wil zeggen, vaste schijven) worden lokaal op de knooppunten die de stateful-service wordt uitgevoerd. Hiermee vertragingstijden voor lage en hoge doorvoersnelheden in vergelijking met externe permanente opslag, niet lokaal op het knooppunt is.

Het onderdeel van het logboek meerdere logboekbestanden worden gebruikt. Er is een hele knooppunt gedeelde logboekbestand met alle replica's als deze de latentie van de laagste en hoogste doorvoersnelheid bieden kan voor het opslaan van gegevens. Standaard wordt het gedeelde logboek geplaatst in de directory Service Fabric knooppunt werk, maar kan ook worden geconfigureerd om te worden geplaatst op een andere locatie, bij voorkeur op een schijf gereserveerd voor alleen het gedeelde logboek. Elke replica voor de service heeft ook een speciale logboekbestand en het speciale logboek in werk-directory van de service is geplaatst. Er is geen mechanisme voor het configureren van de speciale logboek op een andere locatie wordt geplaatst.

Het gedeelde logboek is een overgangsgebied voor informatie over de status van de replica, terwijl het specifieke bestand is de uiteindelijke bestemming waar persistent wordt gemaakt. In dit ontwerp wordt de statusinformatie eerst naar het gedeelde bestand geschreven en lazily verplaatst naar de speciale logboekbestand op de achtergrond. Op deze manier zou het schrijven naar het gedeelde logboek hebben de latentie van de laagste en hoogste doorvoersnelheid waardoor de service vooruitgang te boeken sneller.

Lezen en schrijven naar het gedeelde logboek worden gedaan via directe I/O naar de vooraf toegewezen ruimte op de schijf voor het gedeelde bestand. Als u optimaal gebruik van schijfruimte op het station met speciale Logboeken, de speciale logboekbestand als een verspreid bestand NTFS. Houd er rekening mee dat hierdoor overprovisioning schijfruimte en zal de gespecialiseerde logboekbestanden met veel meer schijfruimte dan werkelijk wordt gebruikt door het besturingssysteem aangeven.

Naast een minimale gebruikersmodus interface naar het logboek, wordt het logboek opgeslagen als een stuurprogramma voor kernel-modus. Door te voeren als een stuurprogramma voor kernel-modus, kan het logboek bieden de hoogste prestaties bij alle services die gebruikmaken van deze.

Zie voor meer informatie over het configureren van het logboek [Stateful betrouwbare Services configureren](service-fabric-reliable-services-configuration.md).

## <a name="stateless-reliable-service"></a>Stateless betrouwbare Service

### <a name="architecture-of-a-stateless-service"></a>Architectuur van een staatloos service
![Architectuurdiagram van een staatloos service](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Stateless betrouwbare Service

Stateless-serverimplementaties zijn afgeleid van de klasse StatelessService of StatelessServiceBase. De klasse StatelessServiceBase kunt meer flexibiliteit dan de klasse StatelessService.
Beide basisklassen beheren de levensduur en de rol van een service.

De uitvoering van de service kan virtuele methoden van de klasse base negeren als de service te doen op die punten in de levenscyclus van de service-- of als deze wil een communicatie-listener-object te maken. Opmerking de listener communicatie door Service Fabric, geïmplementeerd maar de service kan een eigen communicatie listenerobject ICommunicationListener, dat in het bovenstaande diagram implementeren zoals die implementatie service maakt gebruik van een communicatie-listener die is geïmplementeerd door Fabric-Service.

Zie het [overzicht van de betrouwbare Service](service-fabric-reliable-services-introduction.md) en [betrouwbare Service gebruik van geavanceerde](service-fabric-reliable-services-advanced-usage.md) voor meer informatie over de specifieke kenmerken van het schrijven van services met behulp van de klassen StatelessService en StatelessServiceBase.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service-structuur:

[Betrouwbare service-overzicht](service-fabric-reliable-services-introduction.md)

[Snel starten](service-fabric-reliable-services-quick-start.md)

[Betrouwbare collecties-overzicht](service-fabric-reliable-services-reliable-collections.md)

[Betrouwbare service gebruik van geavanceerde](service-fabric-reliable-services-advanced-usage.md)

[Configuratie van betrouwbare service](service-fabric-reliable-services-configuration.md)  

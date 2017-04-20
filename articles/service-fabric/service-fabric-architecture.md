<properties
   pageTitle="Service Fabric '-architectuur | Microsoft Azure"
   description="Service-structuur is een gedistribueerde systemen platform gebruikt voor het bouwen van schaalbare, betrouwbare en eenvoudig beheerd toepassingen voor de cloud. In dit artikel wordt de architectuur van weefsel van de Service."
   services="service-fabric"
   documentationCenter=".net"
   authors="rishirsinha"
   manager="timlt"
   editor="rishirsinha"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/09/2016"
   ms.author="rsinha"/>

# <a name="service-fabric-architecture"></a>Service Fabric '-architectuur

Service Fabric is gebouwd met gelaagde subsystemen. Deze subsystemen kunnen u toepassingen die zijn geschreven:

* Hoge beschikbaarheid
* Schaalbare
* Te beheren
* Testable

In het volgende diagram ziet u de grootste subsystemen van weefsel van de Service.

![Diagram van Service Fabric '-architectuur](media/service-fabric-architecture/service-fabric-architecture.png)

In een gedistribueerd systeem is de mogelijkheid om veilige communicatie tussen knooppunten in een cluster van cruciaal belang. Aan de basis van de stack is het subsysteem vervoer, waarmee u beveiligde communicatie tussen knooppunten. Subsysteem ligt boven het vervoer het subsysteem federation, die de verschillende knooppunten in één enkele entiteit (naam clusters) clusters zodat Fabric-Service kan fouten opsporen, leader election uitvoeren en consistent te routeren. Het subsysteem van betrouwbaarheid, extra boven op de federation-subsysteem is verantwoordelijk voor de betrouwbaarheid van de Service configuratieservices via mechanismen voor replicatie, resourcemanagement en failover. De federation-subsysteem aan ook ten grondslag ligt het subsysteem hosting en activering, die de levenscyclus van een toepassing op een enkel knooppunt beheert. Het subsysteem voor beheer van beheert de levenscyclus van de toepassingen en services. Het subsysteem testbaarheid helpt ontwikkelaars hun diensten met gesimuleerde fouten testen vóór en na de implementatie van toepassingen en services voor productieomgevingen. Fabric-service biedt de mogelijkheid op te lossen servicelocaties via de communicatie subsysteem. De application programming modellen aan ontwikkelaars aangeboden liggen in lagen op deze subsystemen die samen met de toepassingsmodel tooling inschakelen.

## <a name="transport-subsystem"></a>Transport-subsysteem
Het subsysteem transport implementeert een point-to-point datagram communicatiekanaal. Dit kanaal wordt gebruikt voor communicatie binnen service fabric clusters en communicatie tussen service fabric '-cluster en clients. Biedt ondersteuning voor één richting en communicatiepatronen verzoek beantwoorden, die de basis vormt voor de uitvoering van broadcast en multicast in de Federation-laag. Het subsysteem transport communicatie beveiligt met behulp van X509 certificaten of Windows-beveiliging. Dit subsysteem wordt intern gebruikt door de Service weefsel en niet rechtstreeks toegankelijk is voor ontwikkelaars voor het programmeren van toepassing.

## <a name="federation-subsystem"></a>Federation-subsysteem
U moet over een reeks knooppunten reden in een gedistribueerd systeem, hebben een consistente weergave van het systeem. Het subsysteem federation maakt gebruik van de communicatie-primitieven die door het subsysteem voor transport en de verschillende knooppunten in een centrale cluster die u kunt deze reden over dan aan elkaar gehecht. Het biedt de gedistribueerde systemen primitieven die nodig zijn voor andere subsystemen - foutdetectie leader election en consistente routering. De federation-subsysteem is gebouwd op gedistribueerde hashtabellen met een 128-bits token ruimte. Het subsysteem maakt een ringtopologie via de knooppunten, met elk knooppunt in de ring wordt een subset van de token ruimte toegewezen voor eigendom. Voor foutdetectie gebruikt de laag een leasing mechanisme dat op basis van hart kloppen in de race- en arbitrageregelingen. Het subsysteem federation garandeert ook door middel van complexe join en vertrek protocollen die slechts één eigenaar van een token aanwezig op elk gewenst moment. Dit leader election en consistente routering garanties bieden.

## <a name="reliability-subsystem"></a>Subsysteem voor betrouwbaarheid
Het subsysteem betrouwbaarheid biedt het mechanisme om de status van een service Service Fabric hoge beschikbaarheid met behulp van de _Replicator_, _Failover-Manager_en _De verdeling van resources_.

* Replicatie zorgt ervoor dat de status verandert in de primaire service replica automatisch worden gerepliceerd naar secundaire replica's, consistentie tussen de primaire en secundaire replica in een replicaset service van. De replicator is verantwoordelijk voor het beheer van de replica's in de replicaset quorum. Deze samenwerkt met de failover-eenheid om de lijst van bewerkingen die worden gerepliceerd en de nieuwe configuratie-agent biedt dit met de configuratie van de replicaset. Deze configuratie geeft aan welke replica's die de bewerkingen moeten worden gerepliceerd. Fabric-service biedt een standaard replicator genoemd Fabric Replicator, die kan worden gebruikt door de API met programmeren te maken van de service staat zeer stabiel en betrouwbaar.
* De failover-Manager zorgt ervoor dat wanneer de knooppunten worden toegevoegd aan of verwijderd uit het cluster, de belasting automatisch opnieuw verdeeld over de beschikbare knooppunten. Als een knooppunt in het cluster uitvalt, wordt het cluster automatisch opnieuw geconfigureerd de service replica's om de beschikbaarheid te onderhouden.
* De bronnenbeheerder service replica's geplaatst in het domein van de storing in het cluster en zorgt ervoor dat alle eenheden voor failover-operationeel zijn. De bronnenbeheerder saldi ook servicebronnen over de onderliggende gedeelde toepassingen van clusterknooppunten voor een optimale werkbelasting voor uniforme verdeling.

## <a name="management-subsystem"></a>Subsysteem voor beheer
Het subsysteem management biedt end-to-end-service en levenscyclusbeheer voor uw toepassing. Administratieve API's en PowerShell-cmdlets kunt u inrichten, implementeren, patch, upgrade en opgeheven inrichten toepassingen zonder verlies van beschikbaarheid. Het subsysteem management voert deze via de volgende services.

* **Cluster Manager**: dit is de primaire service, die met de failover-Manager van betrouwbaarheid samenwerkt om de toepassingen op de knooppunten op basis van de beperkingen van de plaatsing service. De bronnenbeheerder in failover-subsysteem zorgt ervoor dat de beperkingen die nooit verbroken. De cluster manager beheert de levenscyclus van de toepassingen van de bepaling uit inrichten. Geïntegreerd met de gezondheid manager om ervoor te zorgen dat de beschikbaarheid van de toepassing niet verloren van een semantische gezondheid perspectief tijdens upgrades is.
* **Gezondheid Manager**: Hiermee kunnen statuscontrole van toepassingen, services en diensten van de cluster. Cluster entiteiten (zoals knooppunten-partities en replica's) kunnen rapporteren over gezondheid, die vervolgens wordt samengevoegd in het archief van de gezondheid van de centrale. Deze gezondheidsinformatie biedt een gezondheid algemeen point-in-time momentopname van de services en de knooppunten die zijn verdeeld over meerdere knooppunten in het cluster, zodat u de benodigde corrigerende actie ondernemen. Gezondheid query dat API 's kunnen u de gebeurtenissen voor de gezondheid van een query worden gemeld aan het subsysteem gezondheid. De gezondheid query API's geven de onbewerkte gezondheidsgegevens opgeslagen in het archief van de gezondheid of de geaggregeerde gezondheidsgegevens voor een specifiek cluster entiteit geïnterpreteerd.
* **Afbeelding opslaan**: deze service omvat de opslag en distributie van de binaire bestanden van toepassing. Deze service biedt een eenvoudige gedistribueerde bestandsarchief waar de toepassingen worden geüpload naar en downloaden van.


## <a name="hosting-subsystem"></a>Hosting-subsysteem
De cluster manager informeert het hosting-subsysteem (uitgevoerd op elk knooppunt) welke services nodig voor het beheer van een bepaald knooppunt. Het subsysteem hosting beheert vervolgens de levenscyclus van de toepassing op dat knooppunt. Deze samenwerkt met de betrouwbaarheid en de gezondheid-componenten om ervoor te zorgen dat de replica's correct zijn ingevoerd en in orde zijn.

## <a name="communication-subsystem"></a>Communicatie-subsysteem
Dit subsysteem biedt betrouwbare berichten binnen de ontdekking en de cluster-service via de Naming service. De Naming service-namen worden omgezet in een locatie in het cluster en kan gebruikers voor het beheren van servicenamen en eigenschappen. Gebruik van de service Naming communiceren clients veilig met een willekeurig knooppunt in het cluster kunt oplossen door de naam van een service en service metagegevens ophalen. Met een eenvoudige Naming client API, kunnen gebruikers van de Service Fabric ontwikkelen services en clients met het oplossen van de huidige locatie ondanks de dynamiek van knooppunt of het wijzigen van het formaat van het cluster.

## <a name="testability-subsystem"></a>Testbaarheid subsysteem
Testbaarheid is een suite met hulpprogramma's die speciaal zijn ontworpen voor het testen van services gebaseerd op Service Fabric. De hulpprogramma's kunnen een ontwikkelaar gemakkelijk duidelijke fouten veroorzaken en Testscenario's om te oefenen en valideren van de verschillende statussen en overgangen die een service gedurende de levensduur, allemaal in een gecontroleerde en veilige manier ondervinden zal worden uitgevoerd. Testbaarheid biedt ook een mechanisme voor uitvoering langer tests die u kunnen bladeren door verschillende mogelijke storingen zonder verlies van beschikbaarheid. Zo krijgt u een test in productie-omgeving.

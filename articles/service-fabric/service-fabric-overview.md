<properties
   pageTitle="Overzicht van Service Fabric | Microsoft Azure"
   description="Een overzicht van de Service Fabric, waar toepassingen zijn samengesteld uit vele microservices schaal en veerkracht te geven. Service-structuur is een platform voor gedistribueerde systemen gebruikt voor het bouwen van schaalbare, betrouwbare en gemakkelijk beheerde toepassingen voor de cloud"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="mfussell"/>

# <a name="overview-of-service-fabric"></a>Overzicht van Service Fabric
Service-structuur is een platform voor gedistribueerde systemen waarmee u gemakkelijk inpakken, implementeren en beheren van schaalbare en betrouwbare microservices. Fabric-service ook een oplossing voor de grote uitdagingen bij de ontwikkeling en het beheer van cloud-toepassingen. Oplossen van problemen met de complexe infrastructuur en focus in plaats daarvan op de implementatie van bedrijfskritieke, veeleisende werklast weten dat het schaalbare, betrouwbare en beheerbare kunnen ontwikkelaars en beheerders voorkomen. Service-structuur vertegenwoordigt de volgende generatie middleware platform voor het bouwen en beheren van deze ondernemingen, Tier 1 cloud-toepassingen.

Deze [korte video](https://aka.ms/servicefabricvideo) biedt een inleiding tot Service weefsel en microservices.


## <a name="applications-composed-of-microservices"></a>Toepassingen die zijn samengesteld uit microservices
Fabric-service kunt u bouwen en beheren van schaalbare en betrouwbare toepassingen bestaat uit microservices uitgevoerd met zeer hoge dichtheid op een gedeelde groep van machines (hierna aangeduid als een cluster). Het biedt een geavanceerde runtime voor het bouwen van gedistribueerde, schaalbare stateless en stateful microservices. Het biedt ook uitgebreide toepassing beheermogelijkheden voor inrichten, implementatie, bewaking, upgraden/patches en verwijderen van toepassingen worden geïmplementeerd.

Waarom is een microservices benadering belangrijk? De twee belangrijkste redenen zijn:

1. Hiermee kunt u verschillende gedeelten van uw toepassing afhankelijk van de behoeften.

2. Ontwikkelteams kunnen worden bij het uitstippelen van de wijzigingen gemakkelijker en zodoende functies aan uw klanten sneller en vaker.

Veel Microsoft-services vandaag, met inbegrip van Azure SQL-Database een stimulans Fabric service, Azure, DocumentDB, Cortana, Power BI, Microsoft Intune, Azure gebeurtenis Hubs, Azure IoT, Skype voor bedrijven en veel Azure services core.

Fabric-service is geschikt voor de cloud native services die kunnen begin klein, indien nodig, en groeien op grote schaal met honderden of duizenden computers maken.

De hedendaagse Internet-schaal diensten zijn opgebouwd uit microservices. Voorbeelden van microservices protocol gateways, gebruikersprofielen, winkelen winkelwagentjes, voorraad, verwerking, wachtrijen, en in de cache opgeslagen. Service-structuur is een platform microservices waarmee elke microservice een unieke naam die stateless of stateful.

Fabric-service biedt uitgebreide mogelijkheden voor runtime en de levenscyclus voor toepassingen die zijn samengesteld uit deze microservices. Het host microservices in containers geïmplementeerd en geactiveerd via de Service Fabric '-cluster. Verplaatsen van VMs naar containers is mogelijk een hogere orde van grootte in dichtheid. Een andere orde van grootte in dichtheid wordt zo mogelijk door het verplaatsen van containers naar microservices. Een cluster Azure SQL-Database omvat bijvoorbeeld honderden tienduizenden containers die een totaal van honderden of duizenden databases host-computers. Elke database is een stateful microservice Fabric-Service. Hetzelfde geldt van de andere services die eerder gezegd, dat is de reden waarom de term "' hyperscale ' wordt gebruikt als omschrijving Fabric Service mogelijkheden. Als containers met hoge dichtheid, vervolgens geven microservices u ' hyperscale '.

Lees voor meer informatie over de aanpak van microservices, [Waarom een microservices aanpak voor het bouwen van toepassingen?](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>Container-implementatie en integratie
Service-structuur is een [orchestrator](service-fabric-cluster-resource-manager-introduction.md) van microservices in een cluster van computers. Microservices kunnen worden ontwikkeld op tal van manieren gebruiken [programmeermodellen Fabric Service](service-fabric-choose-framework.md) implementeren [Gast uitvoerbare bestanden](service-fabric-deploy-existing-app.md). Fabric service services in container afbeeldingen kunt implementeren en belangrijker is kunt u zowel in processen en diensten in containers samen in dezelfde toepassing mengen. Als u alleen [implementeren en beheren van afbeeldingen van de container](service-fabric-containers-overview.md) in een cluster van machines wilt, is weefsel van de Service een perfecte keuze voor deze.


## <a name="create-service-fabric-clusters-anywhere"></a>Overal stof Service-clusters maken
U kunt Service Fabric-clusters maken in veel omgevingen, met inbegrip van Azure in gebouwen, op Windows Server of op Linux. Bovendien is de ontwikkelomgeving in de SDK identiek aan de productie-omgeving met geen betrokken emulators. Met andere woorden, als deze wordt uitgevoerd op de plaatselijke ontwikkeling cluster implementeert deze naar hetzelfde cluster in andere omgevingen.

Voor meer informatie over het maken van clusters lokale, [maken van een cluster op Windows Server of Linux](service-fabric-deploy-anywhere.md) lezen of Azure maken van een cluster [via het portal Azure](service-fabric-cluster-creation-via-portal.md).

![Fabric service platform][Image1]

## <a name="stateless-and-stateful-service-fabric-microservices"></a>Stateless en stateful Service Fabric microservices

Fabric-service kunt u bestaande uit microservices toepassingen te bouwen. Stateless microservices (protocol gateways, web proxy's, enz.) gedurende niet een veranderlijke staat buiten een bepaalde aanvraag en het antwoord van de service. Azure Cloud Services werknemer rollen zijn een voorbeeld van een staatloos service. Stateful microservices (gebruikersaccounts, databases, apparaten, winkelen Karren, wachtrijen, enz.) houden een veranderlijke, bindende status dan het verzoek en de reactie daarop. De hedendaagse Internet-toepassingen bestaan uit een combinatie van stateless en stateful microservices.

Waarom zijn stateful microservices samen met die stateless? De twee belangrijkste redenen zijn:

1. De mogelijkheid om hoge doorvoer, lage latentie, fout tolerant on line verwerking van transacties (OLTP) services doordat code en gegevens dicht op dezelfde computer te maken. Enkele voorbeelden zijn interactieve winkelobjecten zoeken, Internet dingen (IoT) systemen, handelssystemen, creditcard verwerking en fraude detectie systemen en persoonlijke records management.

2. Ontwerp vereenvoudiging van toepassing. Stateful microservices de behoefte aan aanvullende wachtrijen verwijderen en in de cache opgeslagen, gewoonlijk nodig om de beschikbaarheid en de latentie eisen van een zuiver stateless toepassing. Stateful zijn natuurlijk hoge beschikbaarheid en een lage latentie, vermindering van het aantal bewegende delen in uw toepassing als geheel beheren.

Voor meer informatie over de toepassing van patronen met Service Fabric, Lees [scenario's van toepassing](service-fabric-application-scenarios.md) en [een programming model kader te kiezen](service-fabric-choose-framework.md) voor uw service

## <a name="application-lifecycle-management"></a>Levenscyclusbeheer voor uw toepassing
Fabric-service biedt eersteklas ondersteuning voor de volledige toepassing lifecycle beheer (ALM) van cloud toepassingen--van ontwikkeling tot implementatie, dagelijkse beheer en onderhoud uiteindelijk buiten gebruik stellen.

De Service Fabric ALM kunnen beheerders / IT-operators gebruiken eenvoudige, low-touch werkstromen te verrichten, patch, implementeert en toepassingen te controleren. Deze ingebouwde werkstromen minder aanzienlijk de belasting van de IT-operators toepassingen continu beschikbaar houden.

De meeste toepassingen bestaan uit een combinatie van stateless en stateful microservices en andere uitvoerbare bestanden/runtimes tegelijk worden geïmplementeerd. Doordat de sterke typen van de toepassingen en verpakte microservices kunt Fabric-Service de implementatie van meerdere toepassingsexemplaren. Elk exemplaar wordt beheerd en onafhankelijk van elkaar worden bijgewerkt. Belangrijker is, kan Fabric-Service ** uitvoerbare bestanden of runtime implementeren en betrouwbaar maken. Bijvoorbeeld: Service Fabric ASP.NET Core 1, Node.js, Java VMs, scripts of iets anders dat uw toepassing implementeert.

Voor meer informatie over application lifecycle management, Lees [Application lifecycle](service-fabric-application-lifecycle.md) en over de implementatie van code [implementeren uitvoerbare Gast](service-fabric-deploy-existing-app.md)

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden
Fabric-Service kunt u:

- Zelfherstellende sterk schaalbare toepassingen ontwikkelen.

- Ontwikkelen van microservices met het programmeermodel Fabric-Service bestaat. Of gewoon host Gast uitvoerbare bestanden en andere toepassingskaders van uw keuze, zoals Core 1 van ASP.NET of Node.js.

- Zeer betrouwbare stateless en stateful microservices ontwikkelen.

- Implementeren en evoluerende containers bevatten Windows containers en Docker containers in een cluster. Deze containers kunnen container Gast uitvoerbare bestanden of betrouwbare stateless en stateful microservices.  In beide gevallen krijgt u poort host-poorttoewijzing, container ontdekken en geautomatiseerde failover-container.

- Het ontwerp van uw toepassing vereenvoudigen met behulp van stateful microservices in plaats van caches en wachtrijen.

- Azure of voor bedrijfsruimten wolken met Windows Server of Linux met nul codewijzigingen implementeren. Eenmaal schrijven en overal implementeren op een cluster Service Fabric.

- Met een 'datacenter op uw machine'-benadering te ontwikkelen. De plaatselijke ontwikkelingsomgeving is dezelfde code die wordt uitgevoerd in de Azure datacenters.

- Toepassingen implementeren in seconden.

- Implementatie van toepassingen met hogere dichtheid dan honderden of duizenden toepassingen per computer implementeren van virtuele machines.

- Verschillende versies van dezelfde toepassing naast elkaar, elk afzonderlijk uitbreidbaar implementeren.

- De levenscyclus van de stateful toepassingen zonder enige uitvaltijd, met inbegrip van breken en vaste upgrades beheren.

- Beheren van toepassingen met behulp van API's van .NET, Java (Linux), PowerShell, Azure CLI (Linux) of REST-interfaces.

- Upgrade en onafhankelijk microservices binnen toepassingen patch.

- Controleren en diagnosticeren van de gezondheid van uw toepassingen en beleid instellen voor het uitvoeren van automatische reparaties.

- Mogelijkheid om te schalen of schaal in het aantal knooppunten in een cluster, evenals de opschaling of schalen naar de grootte van elk knooppunt, wetende dat uw toepassingen automatisch schalen en volgens de beschikbare middelen worden verdeeld.

- De verdeling van de zelfherstellende resource Watch evoluerende het opnieuw distribueren van toepassingen in het cluster. Fabric-service hersteld na storingen en optimaliseert de verdeling van de belasting op basis van beschikbare resources.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

* Voor meer informatie:
    * [Waarom een microservices benadering voor het bouwen van toepassingen?](service-fabric-overview-microservices.md)
    * [Overzicht van de terminologie](service-fabric-technical-overview.md)
* Uw Service Fabric- [ontwikkelomgeving](service-fabric-get-started.md) instellen  
* [Een kader met programming model kiezen](service-fabric-choose-framework.md) voor uw service

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png

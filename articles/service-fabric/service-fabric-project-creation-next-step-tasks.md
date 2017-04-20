<properties
   pageTitle="Service Fabric project maken Vervolgstappen | Microsoft Azure"
   description="Dit artikel bevat koppelingen naar een aantal kerntaken ontwikkeling voor Service Fabric"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="your-service-fabric-application-and-next-steps"></a>Uw Service Fabric-toepassing en de volgende stappen
Uw Azure Service Fabric-toepassing is gemaakt. De samenstelling van het project en een aantal mogelijke volgende stappen wordt beschreven.

## <a name="your-application"></a>Uw toepassing
Elke nieuwe toepassing omvat een application-project. Er zijn één of twee extra projecten, afhankelijk van het type service gekozen.

### <a name="the-application-project"></a>De application-project
De application-project bestaat uit:

- Een aantal verwijzingen naar de services van uw toepassing.

- Twee Publicatieprofielen (lokaal en voor Cloud) waarmee u voorkeuren kunt voor het werken met verschillende omgevingen, zoals voorkeuren die betrekking hebben op het eindpunt van een cluster en of voor het uitvoeren van de upgrade-implementaties standaard behouden.

- Twee parameter toepassingsbestanden (lokaal en voor Cloud) kunt u toepassingsconfiguraties voor milieu-specifieke, zoals het aantal partities te maken voor een service beheren.

- Een script voor een implementatie die u gebruiken kunt voor de implementatie van uw toepassing vanaf de opdrachtregel of als onderdeel van een geautomatiseerde doorlopende pijpleiding integratie en implementatie.

- Manifest van de toepassing, waar de toepassing wordt beschreven. Het manifest kunt u vinden onder de map ApplicationPackageRoot.

### <a name="stateless-service"></a>Stateless service
Wanneer u een nieuwe service stateless toevoegt, Visual Studio een project service toegevoegd aan de oplossing die een type dat is afgeleid van `StatelessService`. De service wordt verhoogd voor een lokale variabele in een teller.

### <a name="stateful-service"></a>Stateful-service
Wanneer u een nieuwe service stateful toevoegt, Visual Studio een project service toegevoegd aan de oplossing die een type dat is afgeleid van `StatefulService`. De service wordt verhoogd voor een item in de `RunAsync` methode en slaat het resultaat in een `ReliableDictionary`.

### <a name="actor-service"></a>Actor-service
Wanneer u een nieuwe, betrouwbare acteur toevoegt, Visual Studio voegt twee projecten toe aan uw oplossing: een actor-project en een interface-project.

De acteur-project biedt methoden voor het instellen en ophalen van de waarde van een teller die wordt op betrouwbare wijze in staat van de actor. Het project interface biedt een interface die andere services gebruiken kunnen om op te roepen de actor.

### <a name="stateless-web-api"></a>Stateless Web API
Het stateless Web API-project biedt een eenvoudige web-service die u gebruiken kunt om uw toepassing aan externe clients te openen. Het project is gestructureerd, Zie voor meer informatie over het [Service Fabric Web API-services met de OWIN zelf hosten](service-fabric-reliable-services-communication-webapi.md).

### <a name="aspnet-core"></a>ASP.NET-core

De Service Fabric SDK biedt dezelfde reeks ASP.NET Core sjablonen die beschikbaar zijn voor zelfstandige Core ASP.NET-projecten: leeg, [Web API][aspnet-webapi], en [Web Application][aspnet-webapp].

## <a name="next-steps"></a>Volgende stappen
### <a name="create-an-azure-cluster"></a>Maak een cluster Azure
De Service Fabric SDK biedt een lokale cluster voor ontwikkelen en testen. Zie een cluster maken in Azure, [het instellen van een cluster Service Fabric vanuit Azure portal][create-cluster-in-portal].

### <a name="try-deploying-to-azure-for-free-with-party-clusters"></a>Probeer gratis partij clusters op Azure implementeren

Als u implementeren en beheren van toepassingen in Azure zonder dat het instellen van uw eigen clusters wilt, kunt u de gratis [partij cluster-service](http://aka.ms/tryservicefabric).

### <a name="publish-your-application-to-azure"></a>Publiceer de toepassing naar Azure
U kunt uw toepassing rechtstreeks vanuit Visual Studio aan een cluster Azure publiceren. Voor meer informatie Zie [publiceren van uw toepassing naar Azure][publish-app-to-azure].

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Service Fabric Explorer gebruiken om te visualiseren uw cluster.
Service Fabric Explorer biedt een eenvoudige manier uw cluster, met inbegrip van de gebruikte toepassingen en fysieke lay-out te visualiseren. Voor meer informatie, Zie [het cluster met behulp van Service Fabric Explorer visualiseren][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Versie en upgrade uw service
Fabric-service kunnen onafhankelijke versies weergeven en upgraden van onafhankelijke services in een toepassing. Zie voor meer informatie, [versiebeheer en upgraden van uw services][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Continue integratie met Visual Studio Team Services configureren
Zie voor meer informatie hoe kunt u een doorlopende integratieproces instellen voor uw toepassing Fabric Service [continue integratie met Visual Studio Team Services configureren][ci-with-vso].


<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html

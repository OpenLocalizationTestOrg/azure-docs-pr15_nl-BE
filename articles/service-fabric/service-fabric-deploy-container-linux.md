<properties
   pageTitle="Stof en distributie Containers in Linux | Microsoft Azure"
   description="Fabric-service en het gebruik van containers Docker microservice toepassingen implementeren. Dit artikel beschrijft de mogelijkheden die Fabric-Service voor containers biedt en het implementeren van een image van de container Docker in een cluster"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="deploy-a-docker-container-to-service-fabric"></a>Een container Docker aan Fabric Service implementeren

> [AZURE.SELECTOR]
- [Implementatie van Windows-Container](service-fabric-deploy-container.md)
- [Docker Container implementeren](service-fabric-deploy-container-linux.md)

Dit artikel helpt u bij het bouwen van beperkte services in recipiënten van Docker op Linux.

Fabric-service heeft diverse mogelijkheden voor container die u helpen bij het bouwen van toepassingen die zijn samengesteld van microservices die beperkte zijn. Deze worden beperkte services genoemd.

De mogelijkheden zijn onder andere;

- Container image te implementeren en activeren
- Resource governance
- Verificatie van de opslagplaats
- Container-poorttoewijzing host-poort
- Container-container-detectie en communicatie
- Mogelijkheid tot configureren en omgevingsvariabelen instellen


## <a name="packaging-a-docker-container-with-yeoman"></a>Een container docker verpakking met yeoman
Wanneer de verpakking een container op Linux, kunt u ofwel een yeoman sjabloon gebruiken of [het toepassingspakket handmatig maken](service-fabric-deploy-container.md#manually-packaging-and-deploying-a-container).

De toepassing van een Service Fabric kan een of meer recipiënten, elk met een specifieke rol in het leveren van de functionaliteit van de toepassing bevatten. De Service Fabric-SDK voor Linux bevat een [Yeoman](http://yeoman.io/) generator waarmee u eenvoudig uw toepassing maken en toevoegen van een afbeelding van de container. Yeoman we gebruiken voor het maken van een nieuwe toepassing met een enkele Docker zogenaamd *SimpleContainerApp*. U kunt toevoegen dat meer services later door het bewerken van de gegenereerde manifest van de bestanden.

## <a name="create-the-application"></a>De toepassing maken

1. In een terminal, typ **yo azuresfguest**.

2. Kies de **Container** voor het kader.

3. Naam van uw toepassing bijvoorbeeld SimpleContainerApp

4. Geef de URL voor de afbeelding van de container van een DockerHub repo. Dit heeft de vorm [repo] / [image name]

![Service Fabric Yeoman generator voor containers][sf-yeoman]

## <a name="deploy-the-application"></a>De toepassing implementeert

Zodra de toepassing is gemaakt, kunt u deze implementeren aan het lokale cluster met behulp van de CLI Azure.

1. Verbinding maken met de lokale Service Fabric '-cluster.

    ```bash
    azure servicefabric cluster connect
    ```

2. Gebruik het script voor installatie in de sjabloon die aan het toepassingspakket kopiëren naar het archief van de installatiekopie van het cluster, het toepassingstype registreren en maakt een instantie van de toepassing.

    ```bash
    ./install.sh
    ```

3. Open een browser en Ga naar Service Fabric Explorer op http://localhost:19080/Explorer (vervangen localhost met het particuliere IP-van de VM als Vagrant op Mac OS X).

4. Vouw het knooppunt toepassingen en er is nu een vermelding voor uw toepassing en een andere voor het eerste exemplaar van dat type.

5. Gebruik het verwijderingsscript voor dat in de sjabloon naar een exemplaar van de toepassing verwijderen en de registratie van het toepassingstype.

    ```bash
    ./uninstall.sh
    ```

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van Service weefsel en containers](service-fabric-containers-overview.md)
- [Interactie met weefsel van Service-clusters met behulp van de CLI Azure](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman.png


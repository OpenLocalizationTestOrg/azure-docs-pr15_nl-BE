<properties
   pageTitle="De eerste Service Fabric-toepassing maken op Linux met C# | Microsoft Azure"
   description="Maken en implementeren van een Service Fabric-toepassing met behulp van C#"
   services="service-fabric"
   documentationCenter="csharp"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="csharp"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="subramar"/>


# <a name="create-your-first-azure-service-fabric-application"></a>Uw eerste Azure Service Fabric-toepassing maken

> [AZURE.SELECTOR]
- [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
- [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)

Fabric-service biedt SDK's voor services voor gebouw op Linux in Java en .NET Core. In deze zelfstudie bekijken we hoe een toepassing maken voor Linux en bouwen van een service met C# (.NET Core).

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat [de Linux-ontwikkelomgeving instellen](service-fabric-get-started-linux.md). Als u Mac OS X gebruikt, kunt u [een Linux - in-één-omgeving in een virtuele machine met Vagrant instellen](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>De toepassing maken

De toepassing van een Service Fabric kan een of meer services, elk met een specifieke rol in het leveren van de functionaliteit van de toepassing bevatten. De Service Fabric-SDK voor Linux bevat een [Yeoman](http://yeoman.io/) generator waarmee u gemakkelijk uw eerste service maken en later toevoegen. Yeoman we gebruiken voor het maken van een toepassing met een enkele service.

1. Typ de volgende opdracht om te beginnen met het samenstellen van de steiger in een terminal:`yo azuresfcsharp`

2. De naam van uw toepassing.

3. Kies het type van de eerste service en geef deze de naam. Voor de toepassing van deze zelfstudie kiest u een betrouwbare Actor-Service.

  ![Service Fabric Yeoman generator voor C#][sf-yeoman]

>[AZURE.NOTE] Zie [Service Fabric programming model-overzicht](service-fabric-choose-framework.md)voor meer informatie over de opties.

## <a name="build-the-application"></a>De toepassing bouwen

De Service Fabric Yeoman-sjablonen bevatten een script maken waarmee u maken van de app vanaf de terminal kunt (na het navigeren naar de map application).

  ```bash
 cd myapp 
 ./build.sh 
  ```

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

## <a name="start-the-test-client-and-perform-a-failover"></a>Start de testclient en een failover uitvoeren

Acteur projecten doen alles op hun eigen. Zij eisen dat een andere service of client voor het verzenden van berichten. De acteur-sjabloon bevat een eenvoudige test-script kunt u interactief werken met de acteur-service.

1. Het script verschijnt de uitvoer van de actor-service met het hulpprogramma controle uitvoeren.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Zoek in Explorer Service Fabric, knooppunten die als host fungeert voor de primaire replica van de actor-service. Het screenshot hieronder is het knooppunt 3.

    ![De primaire replica zoeken in Service Fabric Explorer][sfx-primary]

3. Klik op het knooppunt dat u in de vorige stap hebt gevonden, en selecteer vervolgens **(opnieuw opstarten) deactiveren** in het menu Acties. Deze actie start opnieuw op een van de vijf knooppunten in het lokale cluster forceren van een failover naar een secundaire replica uitgevoerd op een ander knooppunt. Als u deze actie uitvoert, aandacht besteden aan de uitvoer uit de testclient en merk op dat de teller blijft verhogen ondanks de failover-functie.


## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over betrouwbare actoren](service-fabric-reliable-actors-introduction.md)
- [Interactie met weefsel van Service-clusters met behulp van de CLI Azure](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png

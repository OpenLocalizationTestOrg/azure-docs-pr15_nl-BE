<properties
   pageTitle="Instellen van uw ontwikkelomgeving op Mac OS X | Microsoft Azure"
   description="De runtime, hulpprogramma's en SDK te installeren en het maken van een cluster van plaatselijke ontwikkeling. Nadat setup is voltooid, zult u klaar om te bouwen van toepassingen op Mac OS X."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="seanmck"/>

# <a name="set-up-your-development-environment-on-mac-os-x"></a>Instellen van uw ontwikkelomgeving op Mac OS X

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

U kunt Service Fabric-toepassingen worden uitgevoerd op Mac OS X met Linux-clusters maken. Dit artikel heeft betrekking op het instellen van uw Mac voor ontwikkeling.

## <a name="prerequisites"></a>Vereisten

Fabric-service wordt niet standaard uitgevoerd op OS X. Als u wilt uitvoeren op een lokale Service Fabric '-cluster, bieden wij een vooraf geconfigureerde Ubuntu virtual machine met Vagrant en VirtualBox. Voordat u begint, moet u het volgende nodig:

- [Vagrant (v1.8.4 of hoger)](http://wwww.vagrantup.com/downloads)
- [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

## <a name="create-the-local-vm"></a>De lokale VM maken

De lokale VM met een 5-node Service Fabric-cluster maken, het volgende doen:

1. De Vagrantfile repo klonen

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```

2. Ga naar de lokale kopie van de repo

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```

3. (Optioneel) Wijzig de standaardinstellingen voor VM

    Standaard wordt de lokale VM als volgt geconfigureerd:

    - 3 GB geheugen toegewezen
    - Persoonlijke host netwerk geconfigureerd IP-192.168.50.50 passthrough van verkeer van de Mac-host inschakelen

    U kunt een van deze instellingen wijzigen of andere configuratie toevoegen aan de VM in de Vagrantfile. Zie de [documentatie van Vagrant](http://www.vagrantup.com/docs) voor de volledige lijst met opties.

4. De VM maken

    ```bash
    vagrant up
    ```

    Deze stap downloadt de vooraf geconfigureerde VM afbeelding lokaal en stel vervolgens een lokale Service Fabric cluster in het opstarten. U kunt verwachten dat een paar minuten duren. Als setup is voltooid, ziet u een bericht in de uitvoer die aangeeft dat het cluster wordt gestart.

    ![Setup wordt gestart na het inrichten van VM cluster][cluster-setup-script]

5. Test of het cluster is juist ingesteld door te navigeren naar Service Fabric Explorer op http://192.168.50.50:19080/Explorer (ervan uitgaande dat u de standaard particuliere netwerk-IP bewaard).

    ![Service Fabric Explorer weergegeven van de host-Mac][sfx-mac]


## <a name="install-the-service-fabric-plugin-for-eclipse-neon-optional"></a>Installeer de invoegtoepassing Fabric Service voor Eclips Neon (optioneel)

Fabric-service biedt een invoegtoepassing voor de Eclips Neon IDE die het proces van het maken en implementeren van Java-services kunt vereenvoudigen.

1. Eclips, ervoor te zorgen dat u Buildship versie 1.0.17 of hoger zijn geïnstalleerd. U kunt de versies van geïnstalleerde onderdelen controleren door te kiezen **Help > Installatiedetails**. U kunt bijwerken met behulp van de instructies Buildship [hier][buildship-update].

2. De Service Fabric-invoegtoepassing installeren, kiest u **Help > installeren van nieuwe Software...**

3. Voer in het tekstvak 'Werken met': http://dl.windowsazure.com/eclipse/servicefabric.

4. Klik op toevoegen.

    ![Eclips Neon plugin voor Service Fabric][sf-eclipse-plugin-install]

5. De invoegtoepassing Fabric-Service en klik op volgende.

6. Ga door met de installatie en accepteer de gebruiksrechtovereenkomst.

## <a name="next-steps"></a>Volgende stappen

- [Maak uw eerste Service Fabric-toepassing voor Linux](service-fabric-create-your-first-linux-application-with-java.md)

<!-- Links -->

- [Een Service Fabric '-cluster maken in de portal voor Azure](service-fabric-cluster-creation-via-portal.md)
- [Een Service Fabric '-cluster met behulp van bronbeheer Azure maken](service-fabric-cluster-creation-via-arm.md)
- [Het toepassingsmodel Service Fabric begrijpen](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

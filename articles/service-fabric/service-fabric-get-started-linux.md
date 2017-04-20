<properties
   pageTitle="Instellen van uw ontwikkelomgeving op Linux | Microsoft Azure"
   description="Installatie van de SDK en runtime en het maken van een cluster van plaatselijke ontwikkeling op Linux. Nadat setup is voltooid, zult u klaar zijn om toepassingen te bouwen."
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
   ms.date="09/26/2016"
   ms.author="seanmck"/>

# <a name="prepare-your-development-environment-on-linux"></a>Bereid uw ontwikkelomgeving op Linux


> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Implementeren en [Azure Service Fabric-toepassingen](service-fabric-application-model.md) uitvoeren op uw machine Linux ontwikkeling, de gemeenschappelijke SDK en runtime installeren. U kunt ook optionele SDK voor Java en .NET Core installeren.

## <a name="prerequisites"></a>Vereisten
### <a name="supported-operating-system-versions"></a>Ondersteunde besturingssystemen
Voor de ontwikkeling van de volgende besturingssystemen ondersteund:

- Ubuntu 16.04 (Xerus Xenial)

## <a name="update-your-apt-sources"></a>De apt bronnen bijwerken

U moet uw apt bronnen bijwerken om de installatie van de SDK en de bijbehorende runtime pakket via apt-get.

1. Open een terminal.
2. De repo Fabric-Service toevoegen aan uw lijst met bronnen.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. De nieuwe GPG-sleutel toevoegen aan uw keyring enz.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```

4. Vernieuw uw pakket lijsten op basis van de zojuist toegevoegde opslagplaatsen.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk"></a>Installeren en instellen van de SDK

Wanneer u uw bronnen worden bijgewerkt, kunt u de SDK installeren.

1. De Service Fabric SDK-pakket installeren. U wordt gevraagd de installatie te bevestigen en om een gebruiksrechtovereenkomst te accepteren.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

2. Voer het script SDK setup.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

## <a name="set-up-the-azure-cross-platform-cli"></a>De Azure platforms CLI instellen

De [Azure platforms CLI] [ azure-xplat-cli-github] bevat opdrachten voor het communiceren met de Service Fabric entiteiten, met inbegrip van clusters en toepassingen. Het is gebaseerd op Node.js dus [Zorg ervoor dat u de knooppunten hebt geïnstalleerd] [ install-node] voordat u verdergaat met de volgende instructies.

1. De github repo op de ontwikkelcomputer klonen.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Overschakelen naar de gekloonde repo en installeert u het knooppunt Package Manager (npm) met behulp van de CLI-afhankelijkheden.

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Een symlink vanuit de map bin/azure van de gekloonde repo te /usr/bin/azure zo maken dat wordt toegevoegd aan het pad en de opdrachten beschikbaar vanuit een andere map zijn.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Ten slotte Fabric Service-opdrachten automatisch aanvullen inschakelen.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

## <a name="set-up-a-local-cluster"></a>Een lokale cluster instellen

Als alles is geïnstalleerd, kunt u moet kunnen een cluster lokaal starten.

1. Het installatiescript van het cluster worden uitgevoerd.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Open een webbrowser en Ga naar http://localhost:19080/Explorer. Als het cluster is gestart, ziet u het dashboard Service Fabric Explorer.

    ![Service Fabric Explorer op Linux][sfx-linux]

U bent op dit moment kunt implementeren vooraf gebouwde Service Fabric toepassingspakketten of nieuwe bestanden op basis van de Gast containers of Gast uitvoerbare bestanden. Volg onderstaande instellingsstappen optionele voor het opbouwen van nieuwe services met behulp van Java of .NET Core SDK's.

## <a name="install-the-java-sdk-and-eclipse-neon-plugin-optional"></a>Installeren van de Java SDK en Eclips Neon-invoegtoepassing (optioneel)

De Java-SDK biedt de bibliotheken en sjablonen met Java Service configuratieservices gebouwd.

1. Installeer het pakket Java-SDK.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. Voer het script SDK setup.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

U kunt de invoegtoepassing Eclips installeren voor Service-structuur van de Eclips Neon IDE.

1. Eclips, ervoor te zorgen dat u Buildship versie 1.0.17 of hoger zijn geïnstalleerd. U kunt de versies van geïnstalleerde onderdelen controleren door te kiezen **Help > Installatiedetails**. U kunt bijwerken met behulp van de instructies Buildship [hier][buildship-update].

2. De Service Fabric-invoegtoepassing installeren, kiest u **Help > installeren van nieuwe Software...**

3. Voer in het tekstvak 'Werken met': http://dl.windowsazure.com/eclipse/servicefabric

4. Klik op toevoegen.

    ![Eclips plugin][sf-eclipse-plugin]

5. De invoegtoepassing Fabric-Service en klik op volgende.

6. Ga door met de installatie en accepteer de gebruiksrechtovereenkomst.

## <a name="install-the-net-core-sdk-optional"></a>Installatie van de SDK voor .NET Core (optioneel)

De Core .NET SDK biedt de bibliotheken en sjablonen die nodig zijn om te bouwen met behulp van cross-platform .NET Core Service configuratieservices.

1. De SDK voor .NET Core-pakket installeren.

    ```bash
    sudo apt-get install servicefabricsdkcsharp
    ```

2. Voer het script SDK setup.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
    ```

## <a name="next-steps"></a>Volgende stappen

- [Uw eerste Java-toepassing maken op Linux](service-fabric-create-your-first-linux-application-with-java.md)

- [Bereid uw ontwikkelomgeving op OSX](service-fabric-get-started-mac.md)


<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png

<properties
   pageTitle="Aan de slag met docker met swarm op Azure"
   description="Beschrijving van het VMs een groep maken met de extensie Docker VM en swarm gebruiken voor het maken van een cluster Docker."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="squillace"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="01/04/2016"
   ms.author="rasquill"/>

# <a name="how-to-use-docker-with-swarm"></a>Het gebruik van docker met swarm

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Dit onderwerp bevat een zeer eenvoudige manier om [docker](https://www.docker.com/) met [swarm](https://github.com/docker/swarm) gebruiken voor het maken van een cluster beheerde swarm op Azure. Vier virtuele machines wordt gemaakt in Azure, een op te treden als beheerder van de swarm en drie als onderdeel van het cluster van docker hosts. Wanneer u klaar bent, kunt u swarm zien het cluster en typ de docker via. Ook de CLI Azure oproepen in dit onderwerp gebruiken de service management (asm)-modus. 

> [AZURE.NOTE] In dit onderwerp wordt docker met swarm en de CLI Azure *zonder* met **docker machine** om aan te tonen hoe de verschillende functies samenwerken maar onafhankelijk blijven. **docker machine** heeft **--swarm** schakelopties waarmee u kunt **docker-machine** rechtstreeks knooppunten toevoegen aan een swarm gebruiken. Raadpleeg de documentatie van de [docker machine](https://github.com/docker/machine) voor een voorbeeld. Als u **docker machine** uitgevoerd tegen Azure VMs gemist, Zie [docker-machine met Azure gebruiken](virtual-machines-linux-docker-machine.md).

## <a name="create-docker-hosts-with-azure-virtual-machines"></a>Docker hosts met Azure virtuele Machines maken

In dit onderwerp maakt vier VMs, maar u kunt elk getal dat u wilt gebruiken. Bel het volgende bij * &lt;wachtwoord&gt; * vervangen door het wachtwoord dat u hebt gekozen.

    azure vm docker create swarm-master -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-1 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-2 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-3 -l "East US" -e 22 $imagename ops <password>

Wanneer u klaar bent kunt u moeten kunnen **azure vm lijst** gebruiken om te zien uw Azure VMs:

    $ azure vm list | grep "swarm-[mn]"
    data:    swarm-master     ReadyRole           East US       swarm-master.cloudapp.net                               100.78.186.65
    data:    swarm-node-1     ReadyRole           East US       swarm-node-1.cloudapp.net                               100.66.72.126
    data:    swarm-node-2     ReadyRole           East US       swarm-node-2.cloudapp.net                               100.72.18.47  
    data:    swarm-node-3     ReadyRole           East US       swarm-node-3.cloudapp.net                               100.78.24.68  

## <a name="installing-swarm-on-the-swarm-master-vm"></a>Swarm op de master swarm VM installeren

In dit onderwerp wordt de die wordt [container model van de installatie van de docker swarm documentatie](https://github.com/docker/swarm#1---docker-image) -- maar u kunt ook SSH aan de **swarm-master**. Als een container met swarm docker wordt **swarm** in dit model gedownload. Hieronder voeren we deze stap *van onze laptop met behulp van docker op afstand* verbinding maken met de **swarm-master** VM en zien het cluster-id maken opdracht **swarm maken**. De id van de cluster is hoe de leden van de groep swarm **swarm** ontdekt. (U kunt ook klonen van de opslagplaats en dit zelf, zodat u volledig beheer en foutopsporing inschakelen.)

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm create
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    36731c17189fd8f450c395db8437befd

Die laatste regel is de id van de cluster; Kopieer het ergens omdat u dit opnieuw gebruiken zal als u het knooppunt VMs de swarm-model te maken van de 'swarm'. In dit voorbeeld wordt de id van de cluster **36731c17189fd8f450c395db8437befd**.

> [AZURE.NOTE] Net als u wilt wissen, gebruiken we onze lokale docker installatie verbinding maken met de **swarm-master** VM in Azure en instructie **swarm-master** te downloaden, installeren en uitvoeren van de opdracht **create** , die als resultaat van onze cluster-id die we later voor discovery doeleinden gebruiken.
<!-- -->
> Uitvoeren om dit te controleren, `docker -H tcp://` * &lt;hostnaam&gt; * ` images` om de container processen op de **swarm-master** -computer en op een ander knooppunt voor vergelijking (omdat we de vorige swarm-opdracht uitvoeren met de schakeloptie **--rm** , de container is verwijderd nadat het volledig is, met behulp van **docker ps - een** niet als resultaat iets).:


        $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        swarm               latest              92d78d321ff2        11 days ago         7.19 MB
        $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        $
<P />
>Als u vertrouwd met **docker bent**, weet u dat de andere knooppunten geen posten hebben geen afbeeldingen hebt gedownload en nog uitgevoerd.

## <a name="join-the-node-vms-to-our-docker-cluster"></a>Het VMs-knooppunt toevoegen aan onze docker-cluster

Voor elk knooppunt, het eindpuntgegevens met behulp van de CLI Azure weer te geven. Hieronder doen we dat voor de **swarm-knooppunt-1** docker host voor het verkrijgen van het knooppunt docker poort.

    $ azure vm endpoint list swarm-node-1
    info:    Executing command vm endpoint list
    + Getting virtual machines
    data:    Name    Protocol  Public Port  Private Port  Virtual IP      EnableDirectServerReturn  Load Balanced
    data:    ------  --------  -----------  ------------  --------------  ------------------------  -------------
    data:    docker  tcp       2376         2376          138.91.112.194  false                     No
    data:    ssh     tcp       22           22            138.91.112.194  false                     No
    info:    vm endpoint list command OK


Met behulp van **docker** en de `-H` optie op de client docker op uw knooppunt VM, dat knooppunt toevoegen aan de swarm maakt u door de cluster-id en van het knooppunt docker poort (de laatste met **--addr**):

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 run -d swarm join --addr=138.91.112.194:2376 token://36731c17189fd8f450c395db8437befd
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    bbf88f61300bf876c6202d4cf886874b363cd7e2899345ac34dc8ab10c7ae924

Die er goed uitziet. Om te bevestigen dat **swarm** wordt uitgevoerd op de **swarm-knooppunt-1** die we typt:

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 ps -a
        CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
        bbf88f61300b        swarm:latest        "/swarm join --addr=   13 seconds ago      Up 12 seconds       2375/tcp            angry_mclean

Herhaal dit voor alle andere knooppunten in het cluster. In ons geval doen we dat voor **swarm-knooppunt-2** en **swarm-knooppunt-3**.

## <a name="begin-managing-the-swarm-cluster"></a>Beginnen met het beheren van het cluster swarm

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run -d -p 2375:2375 swarm manage token://36731c17189fd8f450c395db8437befd
    d7e87c2c147ade438cb4b663bda0ee20981d4818770958f5d317d6aebdcaedd5

en om vervolgens van de knooppunten in het cluster:

    ralph@local:~$ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm list token://73f8bc512e94195210fad6e9cd58986f
    54.149.104.203:2375
    54.187.164.89:2375
    92.222.76.190:2375

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

Ga dingen uitvoeren op uw swarm. Zie inspiratie zoekt, [https://github.com/docker/swarm/](https://github.com/docker/swarm/), of misschien een [video](https://www.youtube.com/watch?v=EC25ARhZ5bI).

<!-- links -->

[docker-machine-azure]: virtual-machines-linux-docker-machine.md
 

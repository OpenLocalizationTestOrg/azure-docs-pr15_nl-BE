<properties
    pageTitle="Gebruik de extensie Docker VM voor Linux op Azure"
    description="Docker en de extensies Azure virtuele Machines worden beschreven en wordt aangegeven hoe u programmatisch maken van virtuele Machines op Azure die hosts docker vanaf de opdrachtregel met behulp van de CLI Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="08/29/2016"
    ms.author="rasquill"/>

# <a name="using-the-docker-vm-extension-from-the-azure-command-line-interface-azure-cli"></a>Met de extensie Docker VM vanaf de opdrachtregel Azure Interface (CLI Azure)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]



In dit onderwerp wordt beschreven hoe een VM maken met de extensie Docker VM service management (asm) modus in Azure CLI op elk platform. [Docker](https://www.docker.com/) is een van de meest populaire virtualisatie benaderingen die [containers Linux](http://en.wikipedia.org/wiki/LXC) in plaats van virtuele machines gebruikt als een manier om gegevens te isoleren en computergebruik op gedeelde bronnen. U kunt de extensie Docker VM en de [Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) maken een VM Docker die fungeert als host voor een willekeurig aantal containers voor uw toepassingen op Azure. Een discussie op hoog niveau van de recipiënten en hun voordelen, Zie [Docker hoog niveau Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).


##<a name="how-to-use-the-docker-vm-extension-with-azure"></a>Het gebruik van de extensie Docker VM met Azure
Gebruik de extensie Docker VM met Azure, moet u een versie van het [opdrachtregelprogramma Azure-Interface](https://github.com/Azure/azure-sdk-tools-xplat) (CLI Azure) hoger dan 0.8.6 (als van dit schrijven de huidige versie 0.10.0 is). U kunt de Azure CLI installeren op Mac, Linux en Windows.


Het volledige proces gebruik van Docker op Azure is eenvoudig:

+ De CLI Azure en de bijbehorende afhankelijkheden installeren op de computer van waaruit u bepalen Azure wilt (op Windows, dit is een Linux distributie wordt uitgevoerd als een virtuele machine)
+ De Azure CLI Docker-opdrachten gebruiken voor het maken van een host Docker VM in Azure
+ De lokale Docker-opdrachten gebruiken voor het beheren van uw containers Docker in uw Docker VM in Azure.


### <a name="install-the-azure-command-line-interface-azure-cli"></a>Installeer de Azure opdrachtregelinterface (CLI Azure)

Zie als u wilt installeren en configureren van de CLI Azure, [het installeren van de opdrachtregelinterface van Azure](../xplat-cli-install.md). Om de installatie te bevestigen, typt u `azure` bij de opdrachtprompt en na korte tijd u de Azure CLI ASCII-illustratie ziet waarin de basisopdrachten voor u beschikbaar. Als de installatie goed werkt, moet u kunnen typen `azure help vm` en ziet dat een van de lijst met opdrachten 'docker'.

> [AZURE.NOTE] Docker bevat hulpprogramma's voor Windows, [Docker Machine](https://docs.docker.com/installation/windows/), die u ook gebruiken kunt voor het automatiseren van het maken van een docker-client die u gebruiken kunt om met VMs Azure als docker hosts te werken.

### <a name="connect-the-azure-cli-to-to-your-azure-account"></a>De Azure CLI te verbinden met uw Account Azure
Voordat u de CLI Azure kunt gebruiken moet u uw accountreferenties Azure koppelen aan de CLI Azure op uw platform. De sectie [verbinding maken met uw abonnement Azure](../xplat-cli-connect.md) wordt uitgelegd hoe downloaden en importeren van het bestand **.publishsettings** , of om uw Azure CLI koppelen aan een organisatie-id.

> [AZURE.NOTE] Er zijn enkele verschillen in gedrag wanneer één of de andere methoden voor verificatie, dus Lees het document om te begrijpen van de verschillende functies worden gebruikt.

### <a name="install-docker-and-use-the-docker-vm-extension-for-azure"></a>Docker installeren en gebruiken de extensie Docker VM voor Azure
Volg de [installatie-instructies voor Docker](https://docs.docker.com/installation/#installation) Docker lokaal installeren op uw computer.

Docker moet met een Azure Virtual Machine, de Linux-afbeelding die wordt gebruikt voor de VM hebben de [Azure Linux VM Agent](virtual-machines-linux-agent-user-guide.md) is geïnstalleerd. Er zijn twee soorten afbeeldingen waarmee u dit:

+ Een afbeelding van Ubuntu uit de galerie Azure afbeelding of

+ Een aangepaste afbeelding voor Linux die u hebt gemaakt met de Azure Linux VM-Agent wordt geïnstalleerd en geconfigureerd. Zie [Azure Linux VM Agent](virtual-machines-linux-agent-user-guide.md) voor meer informatie over het bouwen van een aangepaste Linux VM met de Agent Azure VM.

### <a name="using-the-azure-image-gallery"></a>Met behulp van de galerie Azure afbeelding

Vanuit een Bash of een sessie, dient u het volgende Azure CLI op zoek naar de meest recente Ubuntu afbeelding in de galerie VM te gebruiken door te typen

`azure vm image list | grep Ubuntu-14_04`

en selecteer een van de namen van de afbeelding, zoals `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, en gebruik de volgende opdracht maakt u een nieuwe VM met behulp van die afbeelding.

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

waar:

+ * &lt;vm cloudservice naam&gt; * is de naam van die de hostcomputer Docker-container in Azure VM

+  * &lt;gebruikersnaam&gt; * is de gebruikersnaam van de gebruiker standaard root van VM

+ * &lt;wachtwoord&gt; * is het wachtwoord van de account *gebruikersnaam* die voldoet aan de normen van de complexiteit voor Azure

> [AZURE.NOTE] Op dit moment een wachtwoord moet ten minste 8 tekens, één kleine letter en één hoofdletter, een getal en een speciaal teken zoals een van de volgende tekens bevatten: `!@#$%^&+=`. Nee, de punt aan het einde van de voorgaande zin is niet een speciaal teken.

Als de opdracht uitgevoerd is, worden er ongeveer uit zoals in de volgende, afhankelijk van de precieze argumenten en opties die u gebruikt:

![](./media/virtual-machines-linux-classic-cli-use-docker/dockercreateresults.png)

> [AZURE.NOTE] Maken van een virtuele machine kan enkele minuten duren, maar nadat deze is ingericht (de waarde staat `ReadyRole`) de daemon Docker (de Docker-service) wordt gestart en u kunt verbinding maken met de host Docker container.

De VM Docker testen die u hebt gemaakt in Azure, type

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

waar * &lt;vm-naam-u-gebruikt&gt; * is de naam van de virtuele machine die u in de aanroep van gebruikt `azure vm docker create`. Ziet u iets dat vergelijkbaar is met de volgende, die aangeeft dat de Host Docker VM omhoog en in Azure en wachten op uw opdrachten. 

Nu u proberen kunt verbinding te maken met uw docker-client om informatie te verkrijgen (in sommige Docker client-instellingen, zoals die op de Mac, hebt u mogelijk gebruik `sudo`):

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

Alleen als u zeker weet dat alle werken, kunt u de VM voor de extensie Docker bestuderen:

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Docker Host VM-verificatie

Naast het maken van de VM Docker de `azure vm docker create` opdracht maakt u automatisch ook de nodige certificaten als u wilt dat uw computer verbinding maken met de container Azure host via HTTPS Docker en de certificaten zijn opgeslagen op de client en de host-computers, zo nodig. Op de daaropvolgende pogingen de bestaande certificaten opnieuw worden gebruikt en gedeeld met de nieuwe host.

Certificaten worden standaard geplaatst `~/.docker`, en Docker worden geconfigureerd om te worden uitgevoerd op poort **2376**. Als u wilt gebruiken een andere poort of een map, dan mag u een van de volgende `azure vm docker create` opdrachtregelopties voor het configureren van uw container Docker host VM aan een andere poort of andere certificaten gebruiken voor het aansluiten van clients:

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

De daemon Docker op de host is geconfigureerd om te luisteren naar en het verifiëren van verbindingen op de opgegeven poort met de certificaten die door de `azure vm docker create` opdracht. De clientcomputer moet deze certificaten toegang te krijgen tot de host Docker hebben.

> [AZURE.NOTE] Een netwerkhost uitgevoerd zonder dat deze certificaten worden iedereen die kunt u verbinding maken met de computer. Voordat u de standaardconfiguratie wijzigen, moet u ervoor zorgen dat u begrijpt dat de risico's voor uw computers en toepassingen.

## <a name="next-steps"></a>Volgende stappen

* Bent u klaar, gaat u naar de [Gebruikershandleiding voor Docker] en uw VM Docker gebruiken. Zie voor informatie over het maken van een VM Docker is ingeschakeld in de nieuwe portal [gebruiken de extensie Docker VM met de Portal].

* De extensie Azure Docker VM ondersteunt ook Docker opstelt, die een declaratieve YAML-bestand wordt gebruikt voor een ontwikkelaar gemodelleerd toepassing ze in elke omgeving en het genereren van een consistente implementatie. Zie [Aan de slag met Docker en opstellen voor het definiëren en uitvoeren van een toepassing met meerdere container op een Azure virtuele machine].  

<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Next steps]: #next-steps

[How to use the Docker VM Extension with Azure]: #How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]: #Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]: #Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
[De extensie Docker VM gebruiken met de Portal]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Gebruikershandleiding voor docker]: https://docs.docker.com/userguide/
 
[Aan de slag met Docker en opstellen voor het definiëren en uitvoeren van een toepassing met meerdere container op een Azure virtuele machine]:virtual-machines-linux-docker-compose-quickstart.md
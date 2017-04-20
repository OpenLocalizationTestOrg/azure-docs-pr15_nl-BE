<properties
    pageTitle="Met behulp van Docker VM uitbreiding voor Linux | Microsoft Azure"
    description="Beschrijving van Docker en de extensies Azure virtuele Machines en het maken van Azure Virtual Machines die zijn docker hosts met behulp van de CLI Azure in klassieke implementatiemodel."
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
    ms.date="05/27/2016"
    ms.author="rasquill"/>


# <a name="using-the-docker-vm-extension-with-the-azure-classic-portal"></a>De extensie Docker VM gebruiken met het klassieke Azure portal

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


[Docker](https://www.docker.com/) is een van de meest populaire virtualisatie benaderingen die [containers Linux](http://en.wikipedia.org/wiki/LXC) in plaats van virtuele machines gebruikt als een manier om gegevens te isoleren en computergebruik op gedeelde bronnen. U kunt de VM Docker extensie beheerd door [Azure Linux Agent] maken een VM Docker die fungeert als host voor een willekeurig aantal containers voor uw toepassingen op Azure.

> [AZURE.NOTE] In dit onderwerp wordt beschreven hoe een VM Docker van de klassieke Azure portal maken. Het maken van een VM Docker op de opdrachtregel, Zie [het gebruik van de Docker VM-extensie uit de Azure-opdrachtregelinterface (CLI Azure)]. Een discussie op hoog niveau van de recipiënten en hun voordelen, Zie [Docker hoog niveau Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

## <a name="create-a-new-vm-from-the-image-gallery"></a>Maak een nieuwe VM uit de galerie met afbeelding
De eerste stap vereist een Azure VM vanaf een Linux-installatiekopie die de extensie Docker VM, met een Ubuntu 14.04 LTS van afbeeldingen uit de galerij afbeelding als de afbeelding van een voorbeeld van de server en Ubuntu 14.04 bureaublad als een client ondersteunt. Klik in de portal **+ nieuwe** in de linkerbenedenhoek maakt een nieuw exemplaar van de VM en een Ubuntu 14.04 LTS afbeelding selecteren uit de beschikbare selecties of de volledige afbeelding galerie, zoals hieronder wordt weergegeven.

> [AZURE.NOTE] Op dit moment ondersteunen alleen Ubuntu 14.04 LTS afbeeldingen recenter dan juli 2014 de extensie Docker VM.

![Maak een nieuwe afbeelding voor Ubuntu](./media/virtual-machines-linux-classic-portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Docker certificaten maken

Nadat de VM hebt gemaakt, moet u ervoor zorgen dat Docker op uw computer is geïnstalleerd. (Zie [Docker installatie-instructies](https://docs.docker.com/installation/#installation)voor meer informatie.)

Het certificaat en de sleutel-bestanden maken voor Docker communicatie volgens [Docker uitgevoerd met https] en plaats deze in de **`~/.docker`** map op uw clientcomputer.

> [AZURE.NOTE] De uitbreiding van de VM Docker in de portal wordt momenteel base64-gecodeerde referenties vereist.

Gebruik op de opdrachtregel **`base64`** of een ander hulpprogramma voor favoriete codering base64-gecodeerd onderwerpen maken. Deze bestanden en de sleutel met een eenvoudige manier kan uitzien:

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-the-docker-vm-extension"></a>De uitbreiding van de VM Docker toevoegen
Zoek het VM-exemplaar dat u hebt gemaakt om toe te voegen de extensie Docker VM, en Ga naar **extensies** en klikt u erop om VM-extensies, zoals hieronder wordt weergegeven.
> [AZURE.NOTE] Deze functionaliteit wordt ondersteund in de portal voorbeeld slechts: https://portal.azure.com/

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickExtensions.png)
### <a name="add-an-extension"></a>Een uitbreidingsmodule toevoegen
Klik op de **+ toevoegen** om de mogelijke VM-extensies die u aan deze VM toevoegen kunt weer te geven.

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickAdd.png)
### <a name="select-the-docker-vm-extension"></a>Selecteer de extensie Docker VM
Kies de extensie Docker VM, want dan de Docker en de belangrijke koppelingen wordt, en klik op **maken** onder aan het begin van de installatieprocedure.

![](./media/virtual-machines-linux-classic-portal-use-docker/ChooseDockerExtension.png)

![](./media/virtual-machines-linux-classic-portal-use-docker/CreateButtonFocus.png)
### <a name="add-your-certificate-and-key-files"></a>Het certificaat en de bestanden toevoegen:

Voer in de velden van het formulier de base64-gecodeerde versies van het CA-certificaat, het certificaat van de Server en uw sleutel Server zoals in de volgende afbeelding.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddExtensionFormFilled.png)

> [AZURE.NOTE] Houd er rekening mee dat (zoals in de vorige afbeelding) de 2376 wordt ingevuld door standaard. Kunt u hier eindpunt, maar de volgende stap is om de overeenkomende eindpunt te openen. Als u de standaardinstelling wijzigt, moet het overeenkomende eindpunt in de volgende stap te openen.

## <a name="add-the-docker-communication-endpoint"></a>Het eindpunt van de communicatie Docker toevoegen
Bij het bekijken van de resourcegroep die u hebt gemaakt, selecteert u de netwerk-beveiligingsgroep die is gekoppeld aan uw VM en op **Regels voor binnenkomende** regels bekijken, zoals hier wordt weergegeven.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddingEndpoint.png)

Klik op **+ toevoegen** als een andere regel wilt toevoegen en voer een naam voor het eindpunt (in dit voorbeeld **Docker**) en 2376 'bestemming poort Range' in de standaardsituatie. Stel de protocolwaarde **TCP**met, en klik op **OK** om de regel te maken.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddEndpointFormFilledOut.png)


## <a name="test-your-docker-client-and-azure-docker-host"></a>Test uw Docker-Client en de Host Azure Docker
Zoek en kopieer de naam van het domein van de VM en op de opdrachtregel van de clientcomputer, het type `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (waarbij *dockerextension* wordt vervangen door het subdomein voor uw VM).

Het resultaat ziet er ongeveer zo uitziet:

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

Nadat u de bovenstaande stappen hebt voltooid, hebt u nu een volledig functionele Docker host op een Azure VM, geconfigureerd voor verbinding met extern van andere clients worden uitgevoerd.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

Bent u klaar, gaat u naar de [Gebruikershandleiding voor Docker] en uw VM Docker gebruiken. Als u automatiseren maken Docker hosts op Azure VMs via de opdrachtregelinterface wilt, Zie [Docker VM de extensie uit de Azure-opdrachtregelinterface (CLI Azure)]

<!--Anchors-->
[Create a new VM from the Image Gallery]: #createvm
[Create Docker Certificates]: #dockercerts
[Add the Docker VM Extension]: #adddockerextension
[Test Docker Client and Azure Docker Host]: #testclientandserver
[Next steps]: #next-steps

<!--Image references-->
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[Het gebruik van de Docker VM-extensie uit de Azure-opdrachtregelinterface (CLI Azure)]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux-Agent]: virtual-machines-linux-agent-user-guide.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md

[Docker uitgevoerd met https]: http://docs.docker.com/articles/https/
[Gebruikershandleiding voor docker]: https://docs.docker.com/userguide/

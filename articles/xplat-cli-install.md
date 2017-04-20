<properties
    pageTitle="Installeer de opdrachtregelinterface van Azure | Microsoft Azure"
    description="De Azure opdrachtregelinterface (CLI) voor Mac, Linux en Windows aan de slag met Azure services installeren"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="squillace"
    services="virtual-machines-linux,virtual-network,storage,azure-resource-manager"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="rasquill"/>
    
# <a name="install-the-azure-cli"></a>De Azure CLI installeren

> [AZURE.SELECTOR]
- [PowerShell](powershell-install-configure.md)
- [Azure CLI](xplat-cli-install.md)

De Azure opdrachtregelinterface (CLI Azure) om een reeks open source shell-opdrachten gebruiken voor het maken en beheren van bronnen in Microsoft Azure snel installeren. U hebt verschillende opties voor deze platforms hulpprogramma's te installeren op uw computer: 

* **pakket npm** - npm (de package manager voor JavaScript) naar het meest recente Azure CLI-pakket installeren op uw Linux-distributie of OS uitvoeren. Node.js en npm op uw computer vereist.
* **Installer** - Download een installatieprogramma voor een eenvoudige installatie op een Mac of Windows.
* **Docker container** - Start met behulp van de nieuwste CLI in een kant-en-klaar Docker container. Docker-host op uw computer vereist.
    
Zie voor meer opties en achtergrond, de opslagplaats project op [GitHub](https://github.com/azure/azure-xplat-cli). 

Zodra de CLI Azure is geïnstalleerd, [verbinding maken met uw abonnement Azure](xplat-cli-connect.md) en de **azure** -opdrachten vanaf de opdrachtregelinterface (Bash, Terminal, opdrachtprompt, enzovoort) uitvoeren om te werken met uw Azure bronnen.



## <a name="option-1-install-an-npm-package"></a>Optie 1: Installeer een pakket npm

Om de CLI van een npm-pakket installeert, zorg ervoor dat u hebt gedownload en geïnstalleerd de [laatste Node.js en npm](https://nodejs.org/en/download/package-manager/). Voer de **npm installeren** om de azure cli-pakket te installeren: 

    npm install -g azure-cli

Op Linux-distributies moet u mogelijk gebruik **sudo** om uit te voeren met de opdracht __npm__ , als volgt:

    sudo npm install -g azure-cli

> [AZURE.NOTE]Als u moet installeren of bijwerken van Node.js en npm op de distributie van Linux of OS, wordt aangeraden dat u de meest recente versie van Node.js LTS (4.x) installeren. Als u een oudere versie gebruikt, krijgt u mogelijk fouten. 

Als u liever, download de nieuwste Linux [tar-bestand] [ linux-installer] voor het npm pakket lokaal. Vervolgens installeert u de gedownloade npm-pakket als volgt (op Linux-distributies moet u wellicht gebruiken **sudo**):

    npm install -g <path to downloaded tar file>

## <a name="option-2-use-an-installer"></a>Optie 2: Gebruik een installatieprogramma

Als u een Mac of Windows-computer, zijn de volgende CLI-installatieprogramma's beschikbaar voor download:

* [Mac OS X installer][mac-installer]

* [Windows MSI][windows-installer] 

>[AZURE.TIP]In Windows, kunt u ook downloaden van het [Web Platform Installer](https://go.microsoft.com/?linkid=9828653) installeert de CLI. Dit installatieprogramma kunt u de optie voor het installeren van extra Azure SDK en de opdrachtregelprogramma's na de installatie van de CLI. 


## <a name="option-3-use-a-docker-container"></a>Optie 3: Een Docker container gebruiken

Als u uw computer als host voor [Docker](https://docs.docker.com/engine/understanding-docker/) hebt ingesteld, kunt u de meest recente Azure CLI uitvoeren in een container Docker. De volgende opdracht uitvoeren (op Linux-distributies moet u wellicht gebruiken **sudo**):

```
docker run -it microsoft/azure-cli
```


## <a name="run-azure-cli-commands"></a>Azure CLI-opdrachten uitvoeren
Nadat de CLI Azure is geïnstalleerd, voert u de opdracht **azure** vanuit opdrachtregel gebruikersinterface (Bash, Terminal, opdrachtprompt, enzovoort). Bijvoorbeeld, als u wilt uitvoeren de opdracht help, typt u het volgende:

```
azure help
```
> [AZURE.NOTE]Op sommige Linux-distributies wordt er mogelijk een foutbericht vergelijkbaar met `/usr/bin/env: ‘node’: No such file or directory`. Deze fout komt van recente installaties van Node.js wordt geïnstalleerd op /usr/bin/nodejs. Om dit te corrigeren, maakt u een symbolische koppeling naar /usr/bin/node door het uitvoeren van deze opdracht:

```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Overzicht van de versie van de Azure CLI die u hebt geïnstalleerd, typt u het volgende:

```
azure --version
```

U bent klaar! Toegang tot de CLI-opdrachten voor het werken met uw eigen bronnen, [verbinding maken met uw Azure abonnement op de CLI Azure](xplat-cli-connect.md).

>[AZURE.NOTE] Wanneer u eerst de CLI Azure gebruikt, ziet u een bericht waarin wordt gevraagd of u dat Microsoft wilt voor het verzamelen van informatie over het gebruik. Deelname is vrijwillig. Als u ervoor kiest om deel te nemen, kunt u stoppen op elk gewenst moment door te voeren `azure telemetry --disable`. Als u deelname op elk gewenst moment, voert u `azure telemetry --enable`.


## <a name="update-the-cli"></a>Update de CLI

Microsoft brengt regelmatig bijgewerkte versies van de CLI Azure. De CLI met behulp van de installer voor uw besturingssysteem opnieuw installeren of uitvoeren van de meest recente Docker container. Of hebt u de meest recente Node.js en npm geïnstalleerd, werken met de volgende (op Linux-distributies moet u mogelijk gebruik van **sudo**).

```
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Tab voltooiing inschakelen

Tab voltooiing van CLI-opdrachten wordt ondersteund voor Mac en Linux.

Als u deze in zsh, voert:

```
echo '. <(azure --completion)' >> .zshrc
```

Als u deze in bash, voert:

```
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Volgende stappen 

* [Verbinding maken vanaf de CLI om uw abonnement op Azure](xplat-cli-connect.md) maken en beheren van bronnen Azure.

* Voor meer informatie over de CLI Azure, download de broncode, rapport problemen, of bijdragen aan het project, gaat u naar de [opslagplaats voor de CLI Azure GitHub](https://github.com/azure/azure-xplat-cli).

* Als u vragen hebt over de Azure CLI of Azure, Ga naar de [Forums Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).

* Als u wilt, kunt u ook de Python gebaseerde [Azure CLI 2.0 Preview](https://github.com/azure/azure-cli).

[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: virtual-machines-command-line-tools.md
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md

<properties
   pageTitle="Docker en opstellen op een virtuele machine | Microsoft Azure"
   description="Korte inleiding tot het werken met de opstel- en Docker op Linux virtuele machines in Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="iainfou"/>

# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-on-an-azure-virtual-machine"></a>Aan de slag met Docker en opstellen voor het definiëren en uitvoeren van een toepassing met meerdere container op een Azure virtuele machine

Aan de slag met Docker en [opstellen](http://github.com/docker/compose) voor het definiëren en uitvoeren van een complexe toepassing op een virtuele Linux machine in Azure. Bij het opstellen gebruikt u een eenvoudig tekstbestand voor het definiëren van een toepassing die bestaat uit meerdere Docker containers. U film van uw toepassing met een enkele opdracht die alles doet voor de implementatie van uw omgeving gedefinieerd. 

Als u bijvoorbeeld in dit artikel wordt beschreven hoe u een WordPress blog met een back-end MariaDB SQL-database op een Ubuntu VM snel instellen. U kunt ook opstellen voor het instellen van complexe toepassingen.


## <a name="step-1-set-up-a-linux-vm-as-a-docker-host"></a>Stap 1: Een VM Linux als host Docker instellen

Kunt u verschillende Azure procedures en beschikbare afbeeldingen of bronnenbeheerder sjablonen op Marketplace Azure een Linux VM maken en deze instellen als een host Docker. Zie bijvoorbeeld [de extensie Docker VM voor de implementatie van uw omgeving](virtual-machines-linux-dockerextension.md) snel een Ubuntu VM met de extensie Azure Docker VM maken met behulp van een [sjabloon quickstart](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Wanneer u de extensie Docker VM, uw VM wordt automatisch ingesteld als een host Docker en opstellen is al geïnstalleerd. In het voorbeeld in dit artikel wordt het gebruik van de [opdrachtregel-interface voor Mac, Linux en Windows Azure](../xplat-cli-install.md) (de Azure CLI) in de modus voor het maken van de VM Resource Manager.

De eenvoudige opdracht uit het vorige document maakt u een groep met de naam `myResourceGroup` in de `West US` locatie en implementeert een VM met de extensie Azure Docker VM is geïnstalleerd:

```bash
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

## <a name="step-2-verify-that-compose-is-installed"></a>Stap 2: Controleer of opstellen is geïnstalleerd

Zodra de installatie is voltooid, SSH naar uw nieuwe Docker host met behulp van de DNS-server de naam u hebt opgegeven tijdens de implementatie. U kunt `azure vm show -g myDockerResourceGroup -n myDockerVM` om details van uw VM, met inbegrip van de DNS-naam te geven.

Als u wilt controleren of opstellen op de VM is geïnstalleerd, moet u de volgende opdracht uitvoeren:

```bash
docker-compose --version
```

Ziet u uitvoer die vergelijkbaar is met `docker-compose 1.6.2, build 4d72027`.

>[AZURE.TIP] Als u een andere methode gebruikt voor het maken van een host Docker en moet opstellen zelf installeren, Zie de [documentatie opstellen](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="step-3-create-a-docker-composeyml-configuration-file"></a>Stap 3: Maak een configuratiebestand docker compose.yml

Vervolgens maakt u een `docker-compose.yml` -bestand, dat gewoon een tekst configuratiebestand is voor het definiëren van de containers Docker uit te voeren op de VM. Het bestand wordt de afbeelding uit te voeren op elke container (of is het mogelijk een gecompileerde versie van een Dockerfile) nodig omgevingsvariabelen en afhankelijkheden, poorten en de koppelingen tussen de containers. Zie voor informatie over de syntaxis van de bestanden yml, [opstellen bestand waarnaar wordt verwezen](http://docs.docker.com/compose/yml/).

Maak de `docker-compose.yml` -bestand als volgt:

```bash
touch docker-compose.yml
```

Gebruik uw favoriete teksteditor bepaalde gegevens toevoegen aan het bestand. In het volgende voorbeeld wordt de `vi` editor:

```bash
vi docker-compose.yml
```

In het volgende voorbeeld wordt in het tekstbestand plakken. Deze configuratie maakt gebruik van afbeeldingen uit het [Register DockerHub](https://registry.hub.docker.com/_/wordpress/) WordPress (het open-source bloggen en inhoud management system) en een gekoppelde back-end MariaDB SQL-database installeren. Voer uw eigen `MYSQL_ROOT_PASSWORD` als volgt:

```bash
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="step-4-start-the-containers-with-compose"></a>Stap 4: Start de containers met opstellen

In dezelfde map als uw `docker-compose.yml` bestand, voert u de volgende opdracht (afhankelijk van uw omgeving, moet u mogelijk uitvoeren `docker-compose` met `sudo`.):

```bash
docker-compose up -d

```

Deze opdracht start u de containers Docker is opgegeven in `docker-compose.yml`. Het duurt een minuut of twee voor deze stap te voltooien. Uitvoer van de volgende strekking weergegeven:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

>[AZURE.NOTE] Zorg ervoor dat de optie **-d** op opstarten gebruiken zodat de containers voortdurend op de achtergrond uitgevoerd.

Als u wilt controleren of de recipiënten van, typt u `docker-compose ps`. Worden er ongeveer als volgt:

```bash
Name             Command             State              Ports
-------------------------------------------------------------------------
wordpress_db_1     /docker-           Up                 3306/tcp
             entrypoint.sh
             mysqld
wordpress_wordpr   /entrypoint.sh     Up                 0.0.0.0:80->80
ess_1              apache2-for ...                       /tcp
```

U kunt nu verbinding maken met WordPress rechtstreeks op de VM op poort 80. Open een webbrowser en typ de DNS-naam van uw VM (zoals `http://myresourcegroup.westus.cloudapp.azure.com`). U ziet nu de WordPress startscherm, waar u kunt de installatie voltooien en aan de slag met de toepassing.

![WordPress startscherm][wordpress_start]


## <a name="next-steps"></a>Volgende stappen

* Ga naar de [VM Docker extensie gebruikershandleiding](https://github.com/Azure/azure-docker-extension/blob/master/README.md) voor meer opties voor het configureren van Docker en opstellen in de VM Docker. Één optie is bijvoorbeeld rechtstreeks in de configuratie van de VM Docker-extensie om de opstellen yml bestand (geconverteerd naar JSON) te plaatsen.
* Bekijk de [gebruikershandleiding](http://docs.docker.com/compose/) voor meer voorbeelden van het maken en distribueren van toepassingen met meerdere container en [opstellen bij de opdrachtregel](http://docs.docker.com/compose/reference/) .
* Een sjabloon Azure Resource Manager gebruiken uw eigen of een bijgedragen uit de [Gemeenschap](https://azure.microsoft.com/documentation/templates/)voor de implementatie van een Azure VM met Docker en een toepassing instellen met opstellen. De sjabloon [distribueren een WordPress blog met Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) gebruikt bijvoorbeeld Docker en opstellen WordPress snel implementeren met een MySQL backend op een Ubuntu VM.
* Probeer Docker opstellen met een cluster [Docker Swarm](virtual-machines-linux-docker-swarm.md) te integreren. Zie [Met behulp van opstellen met Swarm](https://docs.docker.com/compose/swarm/) voor scenario's.

<!--Image references-->

[wordpress_start]: ./media/virtual-machines-linux-docker-compose-quickstart/WordPress.png

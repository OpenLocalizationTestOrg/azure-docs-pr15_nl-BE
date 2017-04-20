<properties
   pageTitle="Docker hosts in Azure Docker machine maken | Microsoft Azure"
   description="Beschrijving van het gebruik van de computer voor het maken van docker hosts in Azure Docker."
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="mlearned" />

# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Docker Hosts in Azure Docker machine maken

Uitvoeren van [Docker](https://www.docker.com/) containers, moet een host waarop de daemon docker VM.
In dit onderwerp wordt beschreven hoe u de opdracht [docker machine](https://docs.docker.com/machine/) gebruikt voor het maken van nieuwe Linux VMs, geconfigureerd met de Docker-daemon uitgevoerd in Azure. 

**Opmerking:** 
- *Dit artikel is afhankelijk van docker machine versie 0.7.0 of hoger*
- *Containers van Windows worden ondersteund door middel van docker-machine in de nabije toekomst*

## <a name="create-vms-with-docker-machine"></a>VMs Docker machine maken

Docker in Azure met VMs host maakt het `docker-machine create` met behulp van de opdracht de `azure` stuurprogramma. 

Het stuurprogramma Azure moet uw abonnement-ID. Kunt u de [Azure CLI](xplat-cli-install.md) of de [Azure Portal](https://portal.azure.com) voor het ophalen van uw abonnement Azure. 

**Met behulp van de Portal Azure**
- Abonnementen van de pagina navigatiebalk aan de linkerkant selecteren en kopiëren naar de abonnement-id.

**Met behulp van de CLI Azure**
- Type ```azure account list``` en kopieert u de abonnement-id.

Type `docker-machine create --driver azure` voor een overzicht van de opties en hun standaardwaarden.
U ziet ook de [Docker Azure stuurprogramma-documentatie](https://docs.docker.com/machine/drivers/azure/) voor meer informatie. 

In het volgende voorbeeld is afhankelijk van de standaardwaarden, maar het poort 80 op de VM voor toegang tot het internet te openen. 

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-open-port 80 mydockerhost
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Kies een host docker docker machine
Zodra er een vermelding in docker machine voor uw host, kunt u de standaard-host bij het uitvoeren van opdrachten voor docker instellen.
##<a name="using-powershell"></a>Met behulp van PowerShell

```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

##<a name="using-bash"></a>Met behulp van Bash

```bash
eval $(docker-machine env MyDockerHost)
```

U kunt nu docker opdrachten uitvoeren op de opgegeven host

```
docker ps
docker info
```

## <a name="run-a-container"></a>Uitvoeren van een container

Met een host is geconfigureerd, kunt u nu uitvoeren van een eenvoudige webserver om te controleren of de host correct is geconfigureerd.
Hier we een afbeelding standaard nginx gebruiken, geeft u deze moet luisteren op poort 80 en als de VM-host opnieuw is opgestart, wordt de container wordt opnieuw ook (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

De uitvoer ziet er ongeveer zo uit:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>Testen van de container

Onderzoeken uitgevoerd containers met `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

En controleert u de actieve container, typt u `docker-machine ip <VM name>` om te zoeken naar het IP-adres in te voeren in de browser:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Actieve ngnix container](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

##<a name="summary"></a>Samenvatting
Docker-machine kunt u eenvoudig docker hosts in Azure inrichten voor uw individuele docker host validaties.
Zie voor het hosten van productie van containers, de [Azure Container Service](http://aka.ms/AzureContainerService)

Voor het ontwikkelen van Core-toepassingen met Visual Studio .NET, Zie [Docker's voor Visual Studio](http://aka.ms/DockerToolsForVS)

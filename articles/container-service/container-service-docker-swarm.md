<properties
   pageTitle="Azure Container container servicebeheer met Docker Swarm | Microsoft Azure"
   description="Containers op een Docker Swarm in Azure Container Service implementeren"
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="timlt"/>

# <a name="container-management-with-docker-swarm"></a>Containerbeheer met Swarm Docker

Docker Swarm biedt een omgeving voor het implementeren van beperkte werklast van een set gegroepeerde Docker hosts. Docker Swarm gebruikt de native Docker API. De workflow voor het beheer van containers op een Docker Swarm is vrijwel identiek aan wat op een host één container zijn zou. Dit document vindt u enkele eenvoudige voorbeelden van de implementatie van beperkte werklast in een exemplaar Azure Container Service van Docker Swarm. Zie voor meer gedetailleerde documentatie op Docker Swarm [Docker Swarm op Docker.com](https://docs.docker.com/swarm/).

Vereisten voor de oefeningen in dit document:

[Maak een cluster Swarm in Azure Container Service](container-service-deployment.md)

[Verbinding maken met het cluster Swarm in Azure Container Service](container-service-connect.md)

## <a name="deploy-a-new-container"></a>Implementeren van een nieuwe container

U kunt maken van een nieuwe container in de Docker Swarm de `docker run` opdracht (en zorg ervoor dat u een SSH-tunnel de modellen aan de hand van de bovenstaande voorwaarden hebt geopend). In dit voorbeeld wordt een container van de `yeasy/simple-web` afbeelding:


```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

Nadat de container is gemaakt, gebruikt u `docker ps` haalt u informatie over de container. U ziet hier dat de Swarm-agent die als host voor de container fungeert wordt weergegeven:


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

U kunt nu toegang tot de toepassing die in deze container via de openbare DNS-naam van de taakverdeling Swarm-agent wordt uitgevoerd. U kunt deze informatie vinden in de Azure portal:  


![Ga naar de werkelijke resultaten](media/real-visit.jpg)  

Standaard is de Load Balancer poorten 80, 8080 en 443 openen. Als u aansluiten op een andere poort wilt, moet u die poort in de taakverdelingsvoorziening Azure openen voor de Agent-toepassingen.

## <a name="deploy-multiple-containers"></a>Meerdere containers implementeren

Als u meerdere containers worden gestart door het uitvoeren van 'docker uitvoeren' meerdere keren kunt u de `docker ps` opdracht te zien die fungeert als host voor de containers op worden uitgevoerd. In het volgende voorbeeld worden drie containers gelijkmatig verspreid over de drie Swarm agenten:  


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Containers implementeren met behulp van Docker opstellen

U kunt Docker opstellen voor het automatiseren van de implementatie en configuratie van meerdere containers. Hiervoor zorgen ervoor dat een tunnel Secure Shell (SSH) is gemaakt en dat de DOCKER_HOST-variabele is ingesteld (Zie de bovenstaande minimumvereisten).

Maak een docker compose.yml-bestand op uw lokale systeem. Gebruik hiervoor dit [monster](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

Uitvoeren van `docker-compose up -d` de container-implementaties te starten:


```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

Ten slotte, de lijst van de containers worden geretourneerd. Deze lijst geeft de containers die zijn geïmplementeerd met behulp van Docker opstellen:


```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

Natuurlijk kunt u `docker-compose ps` te onderzoeken alleen de containers die zijn gedefinieerd in de `compose.yml` bestand.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over Docker Swarm](https://docs.docker.com/swarm/)

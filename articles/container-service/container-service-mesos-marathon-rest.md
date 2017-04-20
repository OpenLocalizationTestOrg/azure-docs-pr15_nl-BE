<properties
   pageTitle="Azure Container Service containerbeheer via de REST API | Microsoft Azure"
   description="Containers met een Azure Container Service Mesos cluster implementeren met behulp van de Marathon REST API."
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

# <a name="container-management-through-the-rest-api"></a>Containerbeheer via de REST API

DC-en OS biedt een omgeving voor het implementeren en schalen geclusterde werklast, terwijl de onderliggende hardware samenvatten. Op DC/OS is er een raamwerk dat beheert het plannings- en werkbelasting berekenen wordt uitgevoerd.

Hoewel frameworks beschikbaar voor veel populaire werkbelasting zijn, dit document wordt beschreven hoe u kunt maken en schalen container implementaties met behulp van Marathon. Voordat u deze voorbeelden moet u een DC-en OS-cluster dat is geconfigureerd in Azure Container Service. U moet ook externe verbinding hebt met dit cluster. Raadpleeg de volgende artikelen voor meer informatie over deze items:

- [Een cluster Azure Container Service implementeren](container-service-deployment.md)
- [Verbinding maken met een cluster Azure Container Service](container-service-connect.md)

Nadat u met het cluster Azure Container Service verbonden bent, kunt u de verwante REST-API's en DC-en OS via http://localhost:local poort openen. De voorbeelden in dit document wordt ervan uitgegaan dat u via tunneling op poort 80. Bijvoorbeeld, het eindpunt van de Marathon is bereikbaar op nummer `http://localhost/marathon/v2/`. Zie de documentatie van de Mesosphere voor de [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) en de [Chronos API](https://mesos.github.io/chronos/docs/api.html)en de Apache-documentatie voor de [Mesos Scheduler API](http://mesos.apache.org/documentation/latest/scheduler-http-api/)voor meer informatie over de verschillende API's.

## <a name="gather-information-from-dcos-and-marathon"></a>Verzamel informatie van DC/OS en Marathon

Voordat u de containers met de DC-en OS-cluster implementeert, sommige gegevens verzamelen over de DC-en OS-cluster, zoals de namen en de huidige status van de DC-en OS-agenten. Query hiervoor de `master/slaves` eindpunt van de DC-en OS REST API. Als alles goed gaat, verschijnt een lijst met de DC-en OS-agenten en een aantal eigenschappen voor elk.

```bash
curl http://localhost/mesos/master/slaves
```

Gebruik nu de Marathon `/apps` eindpunt om huidige implementaties van toepassing op het DC-en OS-cluster te controleren. Als dit een nieuw cluster, ziet u een lege matrix voor apps.

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Implementeren van een container Docker-indeling

U implementeren containers via Marathon Docker opgemaakt met behulp van een JSON-bestand met een beschrijving van de beoogde implementatie. In het volgende voorbeeld wordt de container Nginx, poort 80 in de binding van de DC-en OS-agent op poort 80 van de container te implementeren. Rekening mee dat de eigenschap 'acceptedResourceRoles' is ingesteld op 'slave_public'. Dit implementeert de container aan een agent in de agent van de publieke schaal.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Implementeren van een container Docker opgemaakt, JSON-bestand maken of gebruik van het voorbeeld [demo Azure Container Service](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json)aangeboden. Sla het op een toegankelijke locatie. Voer de volgende opdracht voor de implementatie van de container. Geef de naam van het bestand JSON.

```
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

De uitvoer is vergelijkbaar met het volgende:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Nu, als u een query Marathon voor toepassingen uitvoert, deze nieuwe toepassing wordt weergegeven in de uitvoer.

```
curl localhost/marathon/v2/apps
```

## <a name="scale-your-containers"></a>Schalen van uw containers

Ook kunt u de API Marathon schaalt of schalen in implementaties van toepassing. In het vorige voorbeeld geïmplementeerd u één exemplaar van een toepassing. Laten we dit uit aan drie exemplaren van een toepassing schalen. Hiertoe een JSON-bestand maken met behulp van de volgende JSON en slaat u deze op een toegankelijke locatie.

```json
{ "instances": 3 }
```

Voer de volgende opdracht om de schaal van de toepassing.

>[AZURE.NOTE] De URI worden http://localhost/marathon/v2/apps/ en vervolgens de ID van de toepassing moet worden geschaald. Als u het voorbeeld Nginx hier gebruikt, worden de URI http://localhost/marathon/v2/apps/nginx.

```json
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Ten slotte het eindpunt Marathon voor toepassingen een query. U ziet dat er nu drie van de Nginx-containers zijn.

```
curl localhost/marathon/v2/apps
```

## <a name="use-powershell-for-this-exercise-marathon-rest-api-interaction-with-powershell"></a>Voor deze oefening gebruikt u PowerShell: interactie met PowerShell Marathon REST API

U kunt deze dezelfde acties uitvoeren met behulp van PowerShell-opdrachten op een Windows-systeem.

Voer de volgende opdracht om informatie te verzamelen over de DC-en OS-cluster, zoals agent namen en de status van de agent.

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

U implementeren containers via Marathon Docker opgemaakt met behulp van een JSON-bestand met een beschrijving van de beoogde implementatie. In het volgende voorbeeld wordt de container Nginx, poort 80 in de binding van de DC-en OS-agent op poort 80 van de container te implementeren.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

JSON-bestand maken of gebruik het monster [demo Azure Container Service](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json)aangeboden. Sla het op een toegankelijke locatie. Voer de volgende opdracht voor de implementatie van de container. Geef de naam van het bestand JSON.

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Ook kunt u de API Marathon schaalt of schalen in implementaties van toepassing. In het vorige voorbeeld geïmplementeerd u één exemplaar van een toepassing. Laten we dit uit aan drie exemplaren van een toepassing schalen. Hiertoe een JSON-bestand maken met behulp van de volgende JSON en slaat u deze op een toegankelijke locatie.

```json
{ "instances": 3 }
```

Voer de volgende opdracht om de schaal van de toepassing.

> [AZURE.NOTE] De URI worden http://localhost/marathon/v2/apps/ en vervolgens de ID van de toepassing moet worden geschaald. Als u het Nginx monster die hier gebruikt, worden de URI http://localhost/marathon/v2/apps/nginx.

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Volgende stappen

- [Lees meer over de Mesos HTTP-eindpunten]( http://mesos.apache.org/documentation/latest/endpoints/).
- [Voor meer informatie over de Marathon REST API]( https://mesosphere.github.io/marathon/docs/rest-api.html).

<properties
   pageTitle="Saldo containers in een cluster Azure Container Service laden | Microsoft Azure"
   description="Taken te verdelen over meerdere containers in een cluster Azure Container Service."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, Micro-services, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="rogardle"/>

# <a name="load-balance-containers-in-an-azure-container-service-cluster"></a>Saldo containers in een cluster Azure Container Service laden

In dit artikel wordt besproken voor het maken van een interne taakverdeling in een een DC/OS Azure Container Service met Marathon LB beheerd. Hierdoor kunt u horizontaal schalen van uw toepassingen. Zo ook kunt u profiteren van de agent van de openbare en particuliere clusters door de netwerktaakverdeling op uw containers van toepassing op het cluster particuliere en het openbare cluster.

## <a name="prerequisites"></a>Vereisten

[Een exemplaar van Azure Container Service implementeren](container-service-deployment.md) met orchestrator type DC-en OS- en [ervoor te zorgen dat de client verbinding met uw cluster maken kan](container-service-connect.md). 

## <a name="load-balancing"></a>Taakverdeling

Er zijn twee lagen-taakverdeling in de Container Service cluster maakt: 

  1. Azure Load Balancer biedt openbare toegangspunten (degene die eindgebruikers wordt geraakt). Dit wordt automatisch aangeleverd door Azure Container Service en is standaard geconfigureerd voor poort 80, 443, en 8080.
  2. De taakverdeling Marathon (marathon lb) stuurt binnenkomende aanvragen naar container exemplaren die deze aanvragen. Als de schaal van de containers bieden onze webservice, de marathon lb dynamisch aangepast. Deze taakverdeling is niet standaard meegeleverd in uw Container Service, maar het is heel eenvoudig te installeren.

## <a name="marathon-load-balancer"></a>Marathon Load Balancer

Marathon Load Balancer opnieuw dynamisch op basis van de containers die u hebt geïmplementeerd. Het is ook tegen het verlies van een container of een agent - als dit gebeurt, Apache Mesos gewoon de container elders opnieuw marathon lb worden overeenkomstig aangepast.

Als u wilt installeren web de Marathon taakverdeling kunt u ofwel de DC-en OS-gebruikersinterface of vanaf de opdrachtregel.

### <a name="install-marathon-lb-using-dcos-web-ui"></a>Marathon LB installeren met behulp van DC/OS Web UI

  1. Klik op 'Universum'
  2. Zoek naar 'Marathon LB'
  3. Klik op 'Install'

![Marathon-lb via de webinterface DC/OS installeren](./media/dcos/marathon-lb-install.png)

### <a name="install-marathon-lb-using-the-dcos-cli"></a>Installeren met behulp van de CLI DC/OS Marathon-LB

Na het installeren van de DC-en OS-CLI en ervoor te zorgen dat u verbinding kunt maken met uw cluster, voert de volgende opdracht uit vanaf de clientcomputer:

```bash
dcos package install marathon-lb
```

Met deze opdracht installeert automatisch de taakverdeling op het cluster openbare agenten.

## <a name="deploy-a-load-balanced-web-application"></a>Implementeert een Load Balanced Web Application

Nu dat we de marathon lb-pakket hebt, kunnen wij een toepassingscontainer die we willen verdelen implementeren. In dit voorbeeld wordt we een eenvoudige webserver implementeren met behulp van de volgende configuratie:

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}

```

  * Stel de waarde van `HAProxy_0_VHOST` aan de FQDN-naam van de verdeling van de belasting voor de agenten. Dit is in het formulier `<acsName>agents.<region>.cloudapp.azure.com`. Als u een cluster Container Service maakt met de naam bijvoorbeeld `myacs` in de regio `West US`, is de FQDN-naam `myacsagents.westus.cloudapp.azure.com`. U kunt ook vinden door te zoeken naar de load balancer op "agent" in de naam wanneer u op zoek bent via de resources in de resourcegroep die u hebt gemaakt voor Container Service in [Azure portal](https://portal.azure.com).
  * De servicePort instellen op een poort > = 10.000. Dit geeft de service die wordt uitgevoerd in deze container--marathon lb wordt dit gebruikt voor identificatie van services die deze moet alle in balans.
  * Stel de `HAPROXY_GROUP` tot 'externe' label.
  * Stel `hostPort` op 0. Dit betekent dat Marathon wordt willekeurig een beschikbare poort toegewezen.
  * Stel `instances` om het aantal exemplaren dat u wilt maken. U kunt altijd de schaal deze omhoog en omlaag later.

Het verdient aanbeveling noing dat standaard die marathon aan de particuliere cluster gaat implementeren, dit houdt in dat de bovenstaande implementatie alleen toegankelijk via de taakverdeling is meestal het gedrag dat wij wensen.

### <a name="deploy-using-the-dcos-web-ui"></a>Met behulp van de on line gebruikersinterface DC/OS implementeren

  1. Bezoek de Marathon op http://localhost/marathon (na het instellen van de [tunnel SSH](container-service-connect.md) en klik op`Create Appliction`
  2. In de `New Application` dialoogvenster klikt u op `JSON Mode` in de rechterbovenhoek
  3. De bovenstaande JSON in de editor plakken.
  4. Klik op`Create Appliction`

### <a name="deploy-using-the-dcos-cli"></a>Met behulp van de CLI DC/OS implementeren

Voor de implementatie van deze toepassing met de DC-en OS-CLI gewoon kopiëren de bovenstaande JSON in een bestand met de naam `hello-web.json`, en uit te voeren:

```bash
dcos marathon app add hello-web.json
```

## <a name="azure-load-balancer"></a>Azure Load Balancer

Standaard beschrijft Azure Load Balancer poorten 80 8080 en 443. Als u een van deze drie poorten (net als wij in het bovenstaande voorbeeld), dan is er niets hoeft te doen. U moet mogelijk treffers in de FQDN-naam van de agent taakverdeling-- en elke keer dat u vernieuwen u zult druk op een van de drie webservers in een round robin. Als u een andere poort gebruikt, moet u een regel round-robin en een sonde toevoegen op de verdeling van de belasting voor de poort die u gebruikt. U kunt dit doen vanuit de [CLI Azure](../xplat-cli-azure-resource-manager.md), met de opdrachten `azure network lb rule create` en `azure network lb probe create`. U kunt dit ook doen met behulp van de Portal Azure.


## <a name="additional-scenarios"></a>Aanvullende scenario 's

U kunt een scenario waar u verschillende domeinen gebruiken om verschillende services beschikbaar te hebben. Bijvoorbeeld:

mydomain1.com -> LB:80 Azure -> marathon-lb:10001 -> mycontainer1:33292  
mydomain2.com -> LB:80 Azure -> marathon-lb:10002 -> mycontainer2:22321

Hiertoe uitchecken [virtuele hosts](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/), die kunt u domeinen aan specifieke marathon-lb paden te koppelen.

U kunt ook andere poorten worden blootgesteld en deze toewijzen aan de juiste service achter marathon lb. Bijvoorbeeld:

Azure lb:80 -> marathon-lb:10001 -> mycontainer:233423  
Azure lb:8080 -> marathon-lb:1002 -> mycontainer2:33432


## <a name="next-steps"></a>Volgende stappen

Raadpleeg de documentatie van de DC-en OS voor meer op [marathon lb](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/).

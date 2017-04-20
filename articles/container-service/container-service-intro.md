<properties
   pageTitle="Azure Container introduceert | Microsoft Azure"
   description="Azure Container Service biedt een manier om te vereenvoudigen het maken, configureren en beheren van een cluster van virtuele machines die vooraf zijn geconfigureerd voor beperkte toepassingen uitvoeren."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="azure-container-service-introduction"></a>Azure Container Service-Inleiding

Azure Container Service maakt het eenvoudiger voor u maken, configureren en beheren van een cluster van virtuele machines die vooraf zijn geconfigureerd voor beperkte toepassingen uitvoeren. Een geoptimaliseerde configuratie van de populaire open source-plannings- en orchestration's wordt gebruikt. Hiermee kunt u uw bestaande vaardigheden of tekenen op een groot en groeiend geheel van communautaire deskundigheid, implementeren en beheren van toepassingen op basis van een container op Microsoft Azure.


![Azure Container Service biedt een manier voor het beheren van beperkte toepassingen op meerdere hosts op Azure.](./media/acs-intro/acs-cluster.png)


Azure Container Service maakt gebruik van de indeling Docker container om ervoor te zorgen dat uw toepassing containers volledig overdraagbaar zijn. Ondersteunt ook de keuze van de Marathon en DC/OS of Docker Swarm zodat u kunt deze toepassingen containers duizenden of zelfs tienduizenden schalen.

Azure Container Service gebruikt, kunt u profiteren van de enterprise-functies van Azure, terwijl zij toch toepassing draagbaarheid--met inbegrip van de overdraagbaarheid op de lagen orchestration.

<a name="using-azure-container-service"></a>Met behulp van Azure Container Service
-----------------------------

Ons doel met Azure Container Service is bedoeld als een container hostomgeving met behulp van open source-hulpprogramma's en technologieën die vandaag populair onder onze klanten zijn. Te dien einde bieden we de standaard API eindpunten voor de gekozen orchestrator (DC/OS of Docker Swarm). Met behulp van deze eindpunten, kunt u gebruikmaken van alle software die kan praten met deze eindpunten. Bij het eindpunt Docker Swarm kunt u bijvoorbeeld gebruik van de opdrachtregelinterface (CLI) van Docker. U kunt kiezen voor DC-en OS, de DCOS CLI gebruiken.

<a name="creating-a-docker-cluster-by-using-azure-container-service"></a>Docker clusters maken met behulp van Azure Container Service
-------------------------------------------------------

Om te beginnen met Azure Container Service implementeren een cluster Azure Container Service via de portal (zoeken naar 'Azure Container Service'), met behulp van een sjabloon Azure Resource Manager ([Docker Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm) of [DC-en OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)) of met de [CLI](/documentation/articles/xplat-cli-install/). De opgegeven quickstart-sjablonen kunnen bevatten aanvullende of geavanceerde Azure configuratie worden aangepast. Zie [een cluster Azure Container Service implementeren](container-service-deployment.md)voor meer informatie over het implementeren van een cluster Azure Container Service.

<a name="deploying-an-application"></a>Implementeren van een toepassing
------------------------

Azure Container Service biedt een keuze van Docker Swarm of DC-en OS voor orchestration. Hoe u uw toepassing distribueert, hangt af van uw keuze van orchestrator.

### <a name="using-dcos"></a>Met behulp van DC/OS

DC-en OS is een gedistribueerde besturingssysteem gebaseerd op de kernel Apache Mesos gedistribueerde systemen. Mesos Apache is ondergebracht bij de Apache Software Foundation en bevat een aantal van de [grootste namen in IT](http://mesos.apache.org/documentation/latest/powered-by-mesos/) als de gebruikers en medewerkers.

![Azure Container Service geconfigureerd voor Swarm met agenten en modellen.](media/acs-intro/dcos.png)

DC-en OS- en Apache Mesos bevatten een set met indrukwekkende functies:

-   Bewezen schaalbaarheid

-   Fouttolerante master en gerepliceerd met behulp van Apache ZooKeeper slaves

-   Ondersteuning voor containers Docker-indeling

-   Native isolatie tussen taken met Linux containers

-   Multiresource plannen (geheugen, CPU, schijf en poorten)

-   Java, Python en C++ APIs voor het ontwikkelen van nieuwe parallelle toepassingen

-   Een on line gebruikersinterface voor het weergeven van cluster

DC-en OS op Azure Container Service bevat standaard de Marathon orchestration platform voor het plannen van de werkbelasting. Opgenomen met de DC-en OS-implementatie van de ACS is echter het Mesosphere universum van services die kunnen worden toegevoegd aan uw service, hierbij Spark, Hadoop, Cassandra en nog veel meer.

![DC-en OS-universum in Azure Container Service](media/dcos/universe.png)

#### <a name="using-marathon"></a>Met behulp van Marathon

Marathon is een cluster-brede init- en controlesysteem voor services in cgroups-- of, in geval van Azure Container Service, containers Docker opgemaakt. Marathon biedt een web-UI waarmee u uw toepassingen kan implementeren. U kunt deze openen via een URL die er ongeveer als volgt `http://DNS_PREFIX.REGION.cloudapp.azure.com` waar DNS\_VOORVOEGSEL en regio zijn gedefinieerd bij de implementatie. U kunt natuurlijk ook uw eigen DNS-naam opgeven. Zie voor meer informatie over het uitvoeren van een container met de webgebruikersinterface Marathon [containerbeheer via het web UI](container-service-mesos-marathon-ui.md).

![Lijst met toepassingen marathon](media/dcos/marathon-applications-list.png)

U kunt ook de REST API's gebruiken om te communiceren met de Marathon. Er zijn een aantal clientbibliotheken die beschikbaar voor elk hulpprogramma zijn. Dekking van een verscheidenheid aan talen-- en natuurlijk kunt u het HTTP-protocol in elke taal. Daarnaast bieden veel populaire DevOps extra ondersteuning voor Marathon. Dit biedt maximale flexibiliteit voor het operationele team wanneer u met een cluster Azure Container Service werkt. Zie voor meer informatie over het uitvoeren van een container met de Marathon REST API [Container management met de REST API](container-service-mesos-marathon-rest.md).

### <a name="using-docker-swarm"></a>Met behulp van Swarm Docker

Docker Swarm biedt native clusters voor Docker. Omdat Docker Swarm de standaard Docker API dient, kunt een tool die al met een daemon Docker communiceert Swarm transparant schalen naar meerdere hosts in Azure Container Service.

![Azure Container Service is geconfigureerd voor het gebruik van de DC-en OS--met jumpbox, agents en modellen.](media/acs-intro/acs-swarm2.png)

Ondersteunde hulpprogramma's voor het beheer van containers op een cluster Swarm omvatten, maar zijn niet beperkt tot het volgende:

-   Dokku

-   Docker CLI en Docker opstellen

-   Krane

-   Jenkins

<a name="videos"></a>Video 's
------

Aan de slag met Azure Container Service (101):  

> [AZURE.VIDEO azure-container-service-101]

Toepassingen maken gebruik van de Service Azure Container (Build 2016)

> [AZURE.VIDEO build-2016-building-applications-using-the-azure-container-service]

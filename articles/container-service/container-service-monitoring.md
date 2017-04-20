<properties
   pageTitle="Een cluster Azure Container Service met Datadog controleren | Microsoft Azure"
   description="Een cluster Azure Container Service met Datadog controleren. Via het DC-en OS-web UI de agenten Datadog implementeren in uw cluster."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, DC/OS, Swarm Docker, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure"   
   ms.date="07/28/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Een cluster Azure Container Service met Datadog controleren

In dit artikel zullen we Datadog agenten op alle knooppunten in het cluster Azure Container Service agent implementeren. Voor deze configuratie moet u een account met Datadog. 

## <a name="prerequisites"></a>Vereisten 

[Implementeren](container-service-deployment.md) en [verbinding maken met](container-service-connect.md) een cluster door Azure Container Service geconfigureerd. Verken de [Marathon UI](container-service-mesos-marathon-ui.md). Ga naar [http://datadoghq.com](http://datadoghq.com) voor het instellen van een account Datadog. 

## <a name="datadog"></a>Datadog 

Datadog is een controle gegevens worden verzameld uit de containers in het cluster Azure Container Service monitoring service. Datadog heeft een Docker integratie Dashboard waar u specifieke parameters binnen uw containers kunt zien. Metrische gegevens die worden verzameld uit de containers zijn geordend per CPU, geheugen, netwerk en i/o. Metrics Datadog gesplitst in containers en afbeeldingen. Een voorbeeld van de gebruikersinterface ziet er als voor het CPU-gebruik is hieronder.

![Datadog UI](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Een implementatie Datadog configureren met Marathon

Deze stappen wordt beschreven hoe u configureren en distribueren van toepassingen op uw cluster met Marathon Datadog. 

Toegang tot uw DC-en OS-UI via [http://localhost:80 /](http://localhost:80/). Eenmaal in de gebruikersinterface van de DC-en OS-Ga naar de "heelal" op de links onder en zoek naar 'Datadog' en klik op "Installeren".

![Pakket Datadog binnen het universum DC/OS](./media/container-service-monitoring/datadog1.png)

Nu om de configuratie te voltooien, moet u een account Datadog of een gratis proefversie account. Nadat u bent aangemeld bij het uiterlijk van de website Datadog naar links en Ga naar integratie -> vervolgens API's. 

![Datadog API-sleutel](./media/container-service-monitoring/datadog2.png)

Voer uw API-sleutel in de configuratie Datadog binnen het DC-en OS-universum. 

![Datadog configuratie in het heelal DC/OS](./media/container-service-monitoring/datadog3.png) 

In de bovenstaande configuratie exemplaren zijn ingesteld op 10000000 dat wanneer een nieuw knooppunt wordt toegevoegd aan het cluster Datadog implementeert een agent automatisch naar dat knooppunt. Dit is een tijdelijke oplossing. Zodra de installatie van het pakket dient u terug te keren naar de website van Datadog en 'Dashboards'. Daar ziet u aangepast en Dashboards voor integratie. Het Dashboard Docker integratie hebben alle container maatstaven die u nodig hebt voor de bewaking van het cluster. 

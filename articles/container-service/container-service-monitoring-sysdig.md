<properties
   pageTitle="Controleren van een cluster Azure Container Service met Sysdig | Microsoft Azure"
   description="Een cluster Azure Container Service met Sysdig controleren."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Monitor een cluster Azure Container Service met Sysdig

In dit artikel implementeert we Sysdig agenten op alle knooppunten in het cluster Azure Container Service agent. Voor deze configuratie moet u een account met Sysdig. 

## <a name="prerequisites"></a>Vereisten 

[Implementeren](container-service-deployment.md) en [verbinding maken met](container-service-connect.md) een cluster door Azure Container Service geconfigureerd. Verken de [Marathon UI](container-service-mesos-marathon-ui.md). Ga naar [http://app.sysdigcloud.com](http://app.sysdigcloud.com) voor het instellen van een account Sysdig wolk. 

## <a name="sysdig"></a>Sysdig

Sysdig is een monitoring-service waarmee u uw containers in het cluster controleren. Sysdig bekend is bij het oplossen van problemen met maar heeft ook de basic monitoring metrics voor CPU, netwerk-, geheugen en I/O. Sysdig kunt u eenvoudig zien welke containers werkt de hardest of in hoofdzaak met behulp van het meeste geheugen en CPU. Deze weergave is in de sectie 'Overzicht', die momenteel in beta. 

![Sysdig UI](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>De implementatie van een Sysdig met Marathon configureren

Deze stappen wordt beschreven hoe u configureren en distribueren van toepassingen op uw cluster met Marathon Sysdig. 

Toegang tot uw DC-en OS-UI via [http://localhost:80 /](http://localhost:80/) eenmaal in de gebruikersinterface van de DC-en OS-Ga naar de "heelal", die aan de onderzijde links en zoek naar 'Sysdig'.

![Sysdig in DC-en OS-universum](./media/container-service-monitoring-sysdig/sysdig1.png)

Nu om de configuratie te voltooien moet u een Sysdig cloud-account of een gratis proefversie account. Zodra u bent aangemeld bij de website van de wolk Sysdig, klik op uw gebruikersnaam en klik op de pagina ziet u de 'toegangstoets'. 

![Sysdig API-sleutel](./media/container-service-monitoring-sysdig/sysdig2.png) 

Voer vervolgens de toegangstoets in de configuratie van de Sysdig binnen het universum DC/OS. 

![Sysdig configuratie in het heelal DC/OS](./media/container-service-monitoring-sysdig/sysdig3.png)

Een agent nu set de exemplaren wilt 10000000 wanneer een nieuw knooppunt wordt toegevoegd aan het cluster Sysdig krijgt automatisch implementeren op dat het nieuwe knooppunt. Dit is een tussentijdse oplossing om ervoor te zorgen dat Sysdig met alle nieuwe stoffen binnen het cluster gaat implementeren. 

![Sysdig configuratie in de DC/OS universum-exemplaren](./media/container-service-monitoring-sysdig/sysdig4.png)

Zodra u hebt geïnstalleerd kunt het pakket Ga terug naar de Sysdig UI en u verkennen van het gebruik van verschillende maatstaven voor de containers in het cluster. 
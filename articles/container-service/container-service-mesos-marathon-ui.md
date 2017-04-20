<properties
   pageTitle="Azure Container Service containerbeheer via het web UI | Microsoft Azure"
   description="Containers voor een clusterservice Azure Container Service implementeren met behulp van de Marathon webgebruikersinterface."
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
   ms.date="09/19/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-web-ui"></a>Containerbeheer via het web UI

DC-en OS biedt een omgeving voor het implementeren en schalen geclusterde werklast, terwijl de onderliggende hardware samenvatten. Op DC/OS is er een raamwerk dat beheert het plannings- en werkbelasting berekenen wordt uitgevoerd.

Kaders zijn beschikbaar voor veel populaire werkbelasting, dit document beschrijft hoe u kunt maken en implementaties met Marathon container schalen. Voordat u deze voorbeelden moet u een DC-en OS-cluster dat is geconfigureerd in Azure Container Service. U moet ook externe verbinding hebt met dit cluster. Raadpleeg de volgende artikelen voor meer informatie over deze items:

- [Een cluster Azure Container Service implementeren](container-service-deployment.md)
- [Verbinding maken met een cluster Azure Container Service](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>DC-en OS UI verkennen

Ga naar http://localhost/ met een Secure Shell (SSH)-tunnel tot stand gebracht. Dit wordt geladen het DC-en OS-web UI en informatie over de cluster, zoals de gebruikte bronnen, actieve stoffen en met services.

![DC-EN OS-UI](media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Verken de Marathon UI

Overzicht van de UI Marathon, Ga naar http://localhost/Marathon. Vanuit dit scherm, kunt u een nieuwe container of een andere toepassing starten op het cluster Azure Container Service DC/OS. U kunt ook informatie over het uitvoeren van containers en toepassingen zien.  

![Marathon UI](media/dcos/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Implementeren van een container Docker-indeling

Implementatie van een nieuwe container via Marathon, klik op de knop **Toepassing maken** en voer de volgende gegevens in het formulier:

Veld           | Waarde
----------------|-----------
ID              | nginx
Afbeelding           | nginx
Netwerk         | Overbrugd
Host-poort       | 80
Protocol        | TCP

![Nieuwe toepassing UI - algemeen](media/dcos/dcos4.png)

![Nieuwe toepassing UI--Docker Container](media/dcos/dcos5.png)

![Nieuwe toepassing UI - poorten en Service Discovery](media/dcos/dcos6.png)

Als u wilt dat de poort container statisch worden toegewezen aan een poort van de agent, moet u JSON-modus gebruiken. Hiervoor de wizard nieuwe **JSON** -modus te schakelen met behulp van de in-of uitschakelen. Voer de volgende handelingen, onder de `portMappings` sectie van de toepassingsdefinitie. In het volgende voorbeeld wordt poort 80 van de container naar poort 80 van de DC-en OS-agent. Nadat u deze wijziging hebt aangebracht, kunt u deze wizard JSON modus schakelen.

```none
"hostPort": 80,
```

![Nieuwe toepassing UI--voorbeeld poort 80](media/dcos/dcos13.png)

De DC-en OS-cluster wordt geïmplementeerd met een set van particuliere en openbare ambtenaren. Voor het cluster kunnen toegang krijgen tot toepassingen van het Internet, moet u voor de implementatie van de toepassingen op een openbare agent. Hiertoe selecteert u het tabblad **optioneel** van de wizard nieuwe toepassing en **slave_public** invoeren voor de **Resource rollen geaccepteerd**.

![Nieuwe toepassing UI--openbare agent instellen](media/dcos/dcos14.png)

Terug op de hoofdpagina Marathon ziet u de implementatiestatus van de container.

![Hoofdpagina marathon UI--Implementatiestatus container](media/dcos/dcos7.png)

Wanneer u terugschakelt naar de DC-en OS-web UI (http://localhost/), ziet u dat een taak (in dit geval een container Docker-indeling) op de DC-en OS-cluster wordt uitgevoerd.

![DC-en OS-web UI--taak op het cluster](media/dcos/dcos8.png)

U ziet ook het clusterknooppunt waarop de taak wordt uitgevoerd.

![DC-en OS web UI--taak clusterknooppunt](media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>Schalen van uw containers

U kunt de UI Marathon schalen de telling van het exemplaar van een container. Navigeer naar de pagina **Marathon** hiervoor selecteert u de container die u wilt schalen en klik op de knop **schaal** . Het aantal exemplaren container die u wilt invoeren in het dialoogvenster **Schaal toepassing** en **Schaal toepassing**selecteren.

![Marathon UI--dialoogvenster schaal toepassing](media/dcos/dcos10.png)

Als de schaal-bewerking is voltooid, ziet u meerdere exemplaren van dezelfde taak DC-en OS-agents worden verspreid.

![DC-en OS web UI dashboard--taak verspreiding via agenten](media/dcos/dcos11.png)

![DC-en OS-web UI - knooppunten](media/dcos/dcos12.png)

## <a name="next-steps"></a>Volgende stappen

- [Werken met de DC-en OS en de Marathon-API](container-service-mesos-marathon-rest.md)

Grondige Kennismaking met Mesos op de Azure Container

> [AZURE. Azurecon-2015-deep-dive-on-the-azure-container-service-with-mesos VIDEO]]

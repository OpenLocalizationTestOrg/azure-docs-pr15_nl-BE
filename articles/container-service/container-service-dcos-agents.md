<properties
   pageTitle="Openbare en particuliere DC-en OS-Agent voor groepen ACS | Microsoft Azure"
   description="Hoe werken de openbare en particuliere agent-toepassingen met een cluster Azure Container Service."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
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
   ms.date="08/16/2016"
   ms.author="timlt"/>

# <a name="dcos-agent-pools-for-azure-container-service"></a>DC-en OS-Agent Pools voor Azure Container Service

DC-en OS Azure Container Service agenten in openbare of particuliere toepassingen wordt verdeeld. Een implementatie kan worden gemaakt met een groep, op het gebied van toegankelijkheid tussen computers in de container service. De machines kunnen worden blootgesteld aan internet (openbaar) of interne (particuliere) gehouden. Dit artikel bevat een kort overzicht van waarom er openbare en particuliere groepen zijn.

### <a name="private-agents"></a>Privé-agenten

Persoonlijke agent knooppunten worden uitgevoerd via een niet-routeerbaar netwerk. Dit netwerk is alleen toegankelijk vanuit de zone admin of via de openbare zone rand router. DC-en OS start standaard apps op particuliere agent knooppunten. Raadpleeg de [documentatie van de DC-en OS](https://dcos.io/docs/1.7/administration/securing-your-cluster/) voor meer informatie over de beveiliging van het netwerk.

### <a name="public-agents"></a>Openbare agenten

DC-en OS-toepassingen en diensten van openbare agent knooppunten worden uitgevoerd via een openbaar netwerk. Raadpleeg de [documentatie van de DC-en OS](https://dcos.io/docs/1.7/administration/securing-your-cluster/) voor meer informatie over de beveiliging van het netwerk.

## <a name="using-agent-pools"></a>Met behulp van groepen van agent

Standaard implementeert de **Marathon** elke nieuwe toepassing met de *persoonlijke* agent knooppunten. U moet expliciet de toepassing op het knooppunt *openbare* tijdens het maken van de toepassing te implementeren. Klik op het tabblad **optioneel** en **slave_public** voor de **Resource rollen aanvaarde** waarde invoeren. Dit proces wordt gedocumenteerd [hier](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) en in de documentatie van de [DC\OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) .

## <a name="next-steps"></a>Volgende stappen

Lees meer informatie over [uw DC-en OS-containers beheren](container-service-mesos-marathon-ui.md).

Meer informatie over [de firewall te openen](container-service-enable-public-access.md) die door Azure voor openbare toegang tot uw DC-en OS-container.
<properties
   pageTitle="Gebruikersspecifieke Marathon service of toepassing | Microsoft Azure"
   description="Maken van een toepassing of service van gebruikersspecifieke Marathon"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, Marathon, Micro-services, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>

# <a name="create-an-application-or-user-specific-marathon-service"></a>Maken van een toepassing of service van gebruikersspecifieke Marathon

Azure Container Service biedt een set van master-servers waarop we Apache Mesos en Marathon vooraf configureren. Deze goedkeuringen door uw toepassingen in het cluster kunnen worden gebruikt, maar het is beter geen master-servers voor dit doel. Bijvoorbeeld aanpassingen van de configuratie van de Marathon moet aanmelden bij de masterservers zelf en wijzigingen--dit moedigt unieke masterservers die verschillen van de standaard en moeten worden verzorgd en onafhankelijk. De configuratie vereist voor een team mogelijk ook niet de optimale configuratie voor een ander team.

In dit artikel leggen we uit hoe u een toepassing of een bepaalde gebruiker Marathon service toevoegen.

Aangezien deze service aan een gebruiker of team behoort, worden ze vrij te configureren op een manier die ze wensen. Bovendien, Azure Container Service voor te zorgen dat de service blijft actief. Als de service mislukt, wordt Azure Container Service opnieuw. De meeste van de tijd won't u zelfs opmerkt dat uitvaltijd had.

## <a name="prerequisites"></a>Vereisten

[Een exemplaar van Azure Container Service implementeren](container-service-deployment.md) met orchestrator type DC-en OS- en [ervoor te zorgen dat de client verbinding met uw cluster maken kan](container-service-connect.md). Ook de volgende stappen uitvoeren.

[AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>Maken van een toepassing of service van gebruikersspecifieke Marathon

Begin met het maken van een JSON-configuratiebestand waarin de naam van de toepassingsservice die u wilt maken. Hier gebruiken we `marathon-alice` als de naam van het framework. Sla het bestand op als iets anders, zoals `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Gebruik vervolgens de CLI DC/OS het Marathon exemplaar installeren met de opties die zijn ingesteld in het configuratiebestand:

```bash
dcos package install --options=marathon-alice.json marathon
```

U ziet nu de `marathon-alice` -service wordt uitgevoerd op het tabblad Services van de DC-en OS-gebruikersinterface. De gebruikersinterface is `http://<hostname>/service/marathon-alice/` als u rechtstreeks toegang tot.

## <a name="set-the-dcos-cli-to-access-the-service"></a>De DC-en OS-CLI toegang tot de service instellen

U kunt desgewenst uw DC-en OS-CLI voor toegang tot deze service door de `marathon.url` eigenschap om te verwijzen naar de `marathon-alice` exemplaar als volgt:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

U kunt controleren welke instantie van Marathon waarmee de CLI werkt tegen de `dcos config show` opdracht. U kunt herstellen met behulp van de master Marathon service met de opdracht `dcos config unset marathon.url`.

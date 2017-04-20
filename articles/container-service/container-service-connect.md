<properties
   pageTitle="Verbinding maken met een cluster Azure Container Service | Microsoft Azure"
   description="Verbinding maken met een cluster Azure Container Service met behulp van een SSH-tunnel."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>


# <a name="connect-to-an-azure-container-service-cluster"></a>Verbinding maken met een cluster Azure Container Service

De DC-en OS- en Docker Swarm-clusters die zijn geïmplementeerd met Azure Container Service REST eindpunten worden blootgesteld. Deze eindpunten zijn echter niet open voor de buitenwereld. Om deze eindpunten beheren, moet u een tunnel Secure Shell (SSH). Na een SSH is tunnel gebracht, kunt u opdrachten uitvoeren op de eindpunten van het cluster en de cluster UI via een browser bekijken op uw eigen systeem. Dit document helpt u bij het maken van een tunnel SSH van Linux, OS X en Windows.

>[AZURE.NOTE] U kunt een SSH-sessie maken met een cluster management-systeem. We raden echter niet aan dit. Rechtstreeks aan een beheersysteem beschrijft het risico op onbedoelde wijzigingen.   

## <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Maak een SSH-tunnel op Linux of OS X

Zoek de naam van de openbare DNS-taakverdeling modellen is allereerst dat u bij het maken van een tunnel SSH op Linux of OS X. Vouw hiervoor de resourcegroep, zodat elke resource wordt weergegeven. Zoek en selecteer het openbare IP-adres van het model. Dit wordt een blade dat informatie bevat over het openbare IP-adres, inclusief de DNS-naam geopend. Deze naam voor later gebruik opslaan. <br />


![Openbare DNS-naam](media/pubdns.png)

Nu opent een shell en voer de volgende opdracht waarbij:

**Poort** is de poort van het eindpunt dat u wilt weergeven. Dit is voor Swarm, 2375. Voor DC-en OS, poort 80 gebruikt.  
**Gebruikersnaam** is de gebruikersnaam die is opgegeven als u het cluster hebt geïmplementeerd.  
**DNSPREFIX** is het DNS-voorvoegsel die u verstrekt wanneer u het cluster hebt geïmplementeerd.  
**Gebied** is het gebied waarin de groep zich bevindt.  
**PATH_TO_PRIVATE_KEY** [Optioneel] het pad is naar de persoonlijke sleutel die overeenkomt met de openbare sleutel die u hebt opgegeven bij het maken van de cluster-Service-Container. Gebruik deze optie met de i - markering.

```bash
ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> De poort van SSH-verbinding is 2200--niet de standaard poort 22.

## <a name="dcos-tunnel"></a>DC-en OS-tunnel

Wilt openen in een tunnel naar de eindpunten DC, OS-gerelateerde, die vergelijkbaar is met de volgende opdracht worden uitgevoerd:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

U hebt nu toegang tot de eindpunten DC, OS-betrekking op:

- DC-EN OS:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

U kunt ook de rest API's voor elke toepassing door middel van deze tunnel bereiken.

## <a name="swarm-tunnel"></a>Swarm-tunnel

Om te openen in een tunnel naar het eindpunt van de Swarm, die op de volgende lijkt opdracht worden uitgevoerd:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Nu kunt u de omgevingsvariabele DOCKER_HOST als volgt instellen. U kunt doorgaan met uw Docker-opdrachtregelinterface (CLI) gebruiken als normale.

```bash
export DOCKER_HOST=:2375
```

## <a name="create-an-ssh-tunnel-on-windows"></a>Maak een SSH-tunnel in Windows

Er zijn meerdere opties voor het maken van SSH-tunnels in Windows. Dit document wordt beschreven hoe stopverf gebruiken om dit te doen.

PuTTY downloaden naar uw Windows-systeem en de toepassing wordt uitgevoerd.

Typ een hostnaam uit de naam van het cluster admin-gebruiker en de openbare DNS-naam van het eerste model in het cluster bestaat. De **Hostnaam** wordt ziet er zo uit: `adminuser@PublicDNS`. 2200 invoeren voor de **poort**.

![Configuratie 1 stopverf](media/putty1.png)

Selecteer **verificatie**- en **SSH** . Voeg het bestand met de persoonlijke sleutel voor verificatie.

![Configuratie 2 stopverf](media/putty2.png)

**Tunnels** selecteren en configureren van de volgende poorten doorgestuurd:
- **Bron:** Uw voorkeur--gebruik 80 voor DC-en OS- of 2375 voor Swarm.
- **Doel:** Gebruik localhost:80 voor DC-en OS- of localhost:2375 voor Swarm.

In het volgende voorbeeld is geconfigureerd voor DC-en OS, maar ziet er nu ongeveer voor Docker Swarm.

>[AZURE.NOTE] Poort 80 mag niet worden gebruikt bij het maken van deze tunnel.

![Stopverf configuratie 3](media/putty3.png)

Wanneer u klaar bent, sla de verbindingsconfiguratie en de stopverf sessie verbinden. Wanneer u verbinding maakt, ziet u de poortconfiguratie in het gebeurtenislogboek stopverf.

![Stopverf gebeurtenislogboek](media/putty4.png)

Wanneer u de tunnel voor DC-en OS hebt geconfigureerd, kunt u de verwante eindpunt bij openen:

- DC-EN OS:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Wanneer u de tunnel voor het Docker Swarm hebt geconfigureerd, kunt u de toegang tot het cluster Swarm via de CLI Docker. U moet eerst configureren van een Windows-omgevingsvariabele met de naam `DOCKER_HOST` met een waarde van ` :2375`.

## <a name="next-steps"></a>Volgende stappen

Implementeren en beheren van containers met DC/OS of Swarm:

- [Werken met Azure Container Service en DC/OS](container-service-mesos-marathon-rest.md)
- [Werken met de Azure Container Service en Swarm Docker](container-service-docker-swarm.md)

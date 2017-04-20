<properties
   pageTitle="DC-en OS CLI installeren | Microsoft Azure"
   description="DC-en OS CLI installeren."
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
   ms.date="05/10/2016"
   ms.author="rogardle"/>

>[AZURE.NOTE] Dit is voor het werken met ACS DC, OS-gebaseerde clusters. Er is niet nodig om dit te doen voor ACS Swarm gebaseerde clusters.

Eerst [verbinding maken met uw cluster ACS DC, OS-gebaseerd](../articles/container-service/container-service-connect.md). Als u dit hebt gedaan, kunt u de CLI DC/OS installeren op de clientcomputer met de volgende opdrachten:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Als u een oudere versie van Python, zult u merken dat sommige 'InsecurePlatformWarnings'. U kunt deze gewoon negeren.

Als u wilt beginnen zonder de shell opnieuw te starten, worden uitgevoerd:

```bash
source ~/.bashrc
```

Deze stap meer niet nodig bij het starten van nieuwe houders.

Nu kunt u controleren of de CLI is geïnstalleerd:

```bash
dcos --help
```
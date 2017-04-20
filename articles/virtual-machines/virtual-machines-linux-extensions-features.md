<properties
 pageTitle="Functies en uitbreidingen van de virtuele machine | Microsoft Azure"
 description="Informatie over welke extensies beschikbaar zijn voor Azure virtuele machines, gegroepeerd op wat ze leveren of te verbeteren."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>Over functies en uitbreidingen van de virtuele machine

## <a name="azure-vm-extensions"></a>Azure VM-extensies

Azure Virtual Machine-extensies zijn kleine toepassingen maken die na implementatie configuratie en automatisering taak Azure virtuele Machines. Bijvoorbeeld als een virtuele Machine software geïnstalleerd, anti-virus bescherming of Docker configuratie nodig is, kan een VM-extensie kan worden gebruikt om deze taken te voltooien. Azure VM-extensies kunnen worden uitgevoerd met behulp van de CLI Azure, PowerShell, sjablonen beheren van de Resource en de Azure portal. Extensies kunnen worden gebundeld met de implementatie van een nieuwe virtuele machine of voor ieder bestaand systeem uitgevoerd.

Dit document bevat de vereisten voor de uitbreiding van Azure Virtual Machine en richtlijnen voor het detecteren van beschikbare uitbreidingsmodules voor VM. 

## <a name="azure-vm-agent"></a>Azure VM-Agent

De Agent Azure VM beheert de interactie tussen een Azure Virtual Machine en de Azure Fabric-Controller. De VM-agent is verantwoordelijk voor veel functionele aspecten van de implementatie en het beheer van Azure Virtual Machines, met inbegrip van de VM-serverextensies wordt uitgevoerd. De Agent Azure VM op Azure-afbeeldingen vooraf is geïnstalleerd en op ondersteunde besturingssystemen kan worden geïnstalleerd. 

Zie voor meer informatie over ondersteunde besturingssystemen en installatie-instructies, [Azure Linux Agent User Guide](./virtual-machines-linux-agent-user-guide.md).

## <a name="discover-vm-extensions"></a>VM-extensies ontdekken

Veel verschillende VM extensies zijn beschikbaar voor gebruik met Azure virtuele Machines. Een volledige lijst wilt bekijken, moet u de volgende opdracht uitvoeren met de CLI Azure, de locatie vervangen door de gewenste locatie.

```none
azure vm extension-image list westus
```

<br />

## <a name="common-vm-extensions"></a>Algemene VM-extensies

|Extensienaam   |Beschrijving   |Meer informatie   |
|---|---|---|
|Aangepast Script-uitbreiding voor Linux  | Scripts uitvoeren tegen een Azure Virtual Machine  |[Aangepast Script-uitbreiding voor Linux](./virtual-machines-linux-extensions-customscript.md)   |
|Uitbreiding van docker |Hiermee installeert u de daemon Docker ter ondersteuning van externe Docker opdrachten.  | [Docker VM-extensie](./virtual-machines-linux-dockerextension.md)  |
|VM-uitbreiding voor toegang tot | Opnieuw toegang krijgen tot Azure Virtual Machine  |[VM-uitbreiding voor toegang tot](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
|Diagnostische gegevens van Azure-extensie |Azure diagnostische gegevens beheren |[Diagnostische gegevens van Azure-extensie](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |


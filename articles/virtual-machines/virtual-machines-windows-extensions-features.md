<properties
 pageTitle="Functies en uitbreidingen van de virtuele machine | Microsoft Azure"
 description="Informatie over welke extensies beschikbaar zijn voor Azure virtuele machines, gegroepeerd op wat ze leveren of te verbeteren."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/30/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>Over functies en uitbreidingen van de virtuele machine

## <a name="azure-vm-extensions"></a>Azure VM-extensies

Azure Virtual Machine-extensies zijn kleine toepassingen maken die na implementatie configuratie en automatisering taak Azure virtuele Machines. Bijvoorbeeld als een virtuele Machine software geïnstalleerd, anti-virus bescherming of Docker configuratie nodig is, kan een VM-extensie kan worden gebruikt om deze taken te voltooien. Azure VM-extensies kunnen worden uitgevoerd met behulp van de CLI Azure, PowerShell, sjablonen beheren van de Resource en de Azure portal. Extensies kunnen worden gebundeld met de implementatie van een nieuwe virtuele machine of voor ieder bestaand systeem uitgevoerd.

Dit document bevat de vereisten voor de uitbreiding van Azure Virtual Machine en richtlijnen voor het detecteren van beschikbare uitbreidingsmodules voor VM. 

## <a name="azure-vm-agent"></a>Azure VM-Agent

De Agent Azure VM beheert de interactie tussen een Azure Virtual Machine en de Azure Fabric-Controller. De VM-agent is verantwoordelijk voor veel functionele aspecten van de implementatie en het beheer van Azure Virtual Machines, met inbegrip van de VM-serverextensies wordt uitgevoerd. De Agent Azure VM op Azure-afbeeldingen vooraf is geïnstalleerd en op ondersteunde besturingssystemen kan worden geïnstalleerd. 

Zie voor informatie over ondersteunde besturingssystemen en installatie-instructies, [Azure Virtual Machine-Agent](./virtual-machines-windows-classic-agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>VM-extensies ontdekken

Veel verschillende VM extensies zijn beschikbaar voor gebruik met Azure virtuele Machines. Een volledige lijst wilt bekijken, moet u de volgende opdracht uitvoeren met de CLI Azure, de locatie vervangen door de gewenste locatie.

```none
Get-AzureVMAvailableExtension | Select ExtensionName, Version
```

<br />

## <a name="common-vm-extensions"></a>Algemene VM-extensies

|Extensienaam   |Beschrijving   |Meer informatie   |
|---|---|---|
|Aangepast Script-extensie voor Windows  | Scripts uitvoeren tegen een Azure Virtual Machine  |[Aangepast Script-extensie voor Windows](./virtual-machines-windows-extensions-customscript.md)   |
|DSC-extensie voor Windows | Uitbreiding van PowerShell DSC (gewenste configuratie staat).  | [Docker VM-extensie](./virtual-machines-windows-extensions-dsc-overview.md)  |
|Diagnostische gegevens van Azure-extensie | Azure diagnostische gegevens beheren |[Diagnostische gegevens van Azure-extensie](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |

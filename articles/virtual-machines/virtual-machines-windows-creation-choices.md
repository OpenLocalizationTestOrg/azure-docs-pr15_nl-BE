<properties
    pageTitle="Verschillende manieren voor het maken van een Windows VM | Microsoft Azure"
    description="Hier worden de verschillende manieren voor het maken van een virtuele Windows-computer met Resource Manager."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="different-ways-to-create-a-windows-virtual-machine-with-resource-manager"></a>Verschillende manieren voor het maken van een virtuele Windows-computer met Resource Manager

Azure biedt verschillende manieren voor het maken van een virtuele machine omdat virtuele machines zijn geschikt voor verschillende gebruikers en doeleinden. Dit betekent dat u moet enkele keuzes maken over de virtuele machine en het maken van het. Dit artikel bevat een overzicht van deze keuzen en koppelingen naar instructies.

## <a name="azure-portal"></a>Azure portal

Gebruik de Azure portal is een eenvoudige manier om te proberen om een virtuele machine, vooral als u net met Azure beginnen bent. 

[Maak een virtuele machine met Windows met behulp van de portal](virtual-machines-windows-hero-tutorial.md)

## <a name="template"></a>Sjabloon

Virtuele machines is een combinatie van resources vereist (zoals een beschikbaarheid sets en opslag rekeningen). In plaats van de implementatie en het beheer van elke resource afzonderlijk, kunt u een sjabloon Azure Resource Manager implementeert en bepalingen alle bronnen in een enkele, gecoördineerde werking.

- [Maak een virtuele Windows-computer met een sjabloon Resource Manager](virtual-machines-windows-ps-template.md)


## <a name="azure-powershell"></a>Azure PowerShell

Als u werkt in een opdrachtshell wilt, kunt u Azure PowerShell.

- [Maak een Windows VM met PowerShell](virtual-machines-windows-ps-create.md)


## <a name="visual-studio"></a>Visual Studio

Visual Studio te bouwen, beheren en implementeren VMs met de Azure-hulpprogramma's voor Visual Studio en de Azure SDK gebruiken.

[Azure-hulpprogramma's voor Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)


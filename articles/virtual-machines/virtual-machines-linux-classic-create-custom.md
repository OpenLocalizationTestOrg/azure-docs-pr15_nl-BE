<properties
    pageTitle="Maak een Linux VM | Microsoft Azure"
    description="Informatie over het maken van een aangepaste virtuele machine met het klassieke implementatiemodel met het Linux-besturingssysteem."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="iainfou"/>

# <a name="how-to-create-a-custom-linux-vm"></a>Het maken van een aangepaste Linux VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zie voor de Resource Manager versie [hier](virtual-machines-linux-create-cli-complete.md).

In dit onderwerp wordt beschreven hoe een aangepaste virtuele machine (VM) maken met de Azure CLI met behulp van het implementatiemodel klassiek. We gebruiken een Linux-afbeelding van de beschikbare **INSTALLATIEKOPIEËN** op Azure. De Azure CLI opdrachten geven de volgende configuratie-opties, onder andere:

- De VM verbinding met een virtueel netwerk
- De VM toe te voegen aan een bestaande cloud-service
- De VM toe te voegen aan een bestaande account voor opslag
- De VM toe te voegen aan een set beschikbaarheid of locatie

> [AZURE.IMPORTANT] Als u wilt dat de virtuele machine te gebruiken van een virtueel netwerk zodat kunt u deze rechtstreeks door de hostnaam of het instellen van verbindingen tussen ruimten, Controleer of u het virtuele netwerk wanneer u de virtuele machine maakt. Een virtuele machine kan worden geconfigureerd als u wilt deelnemen aan een virtueel netwerk alleen wanneer u de virtuele machine maakt. Zie voor meer informatie over virtuele netwerken, [Azure Virtual Network overzicht](http://go.microsoft.com/fwlink/p/?LinkID=294063).


## <a name="how-to-create-a-linux-virtual-machine-using-the-classic-deployment-model"></a>Het maken van een Linux virtuele machine met behulp van het implementatiemodel klassiek

[AZURE.INCLUDE [virtual-machines-create-LinuxVM](../../includes/virtual-machines-create-linuxvm.md)]

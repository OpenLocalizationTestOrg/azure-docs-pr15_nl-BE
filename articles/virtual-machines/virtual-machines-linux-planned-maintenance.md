<properties
    pageTitle="Gepland onderhoud voor Linux VMs | Microsoft Azure"
    description="Begrijpen wat Azure gepland onderhoud is en hoe dit van invloed op uw Linux virtuele machines die worden uitgevoerd in Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="drewm"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="drewm"/>

# <a name="planned-maintenance-for-linux-virtual-machines-in-azure"></a>Planmatig onderhoud voor Linux virtuele machines in Azure

Begrijpen wat Azure gepland onderhoud is en hoe deze invloed hebben op de beschikbaarheid van uw Linux virtuele machines. Dit artikel is ook beschikbaar voor [Windows virtuele machines](virtual-machines-windows-planned-maintenance.md). 

Dit artikel bevat een achtergrond dat het proces Azure gepland onderhoud. Als u waarom uw VM opnieuw is opgestart willen zijn, kunt u [lezen van deze blog post met informatie over het weergeven van VM Logboeken opnieuw](https://azure.microsoft.com/blog/viewing-vm-reboot-logs/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="why-azure-performs-planned-maintenance"></a>Waarom gepland onderhoud wordt uitgevoerd in Azure

Microsoft Azure voert regelmatig updates over de hele wereld verbeteren de betrouwbaarheid, prestaties en beveiliging van de infrastructuur van de host waarop de virtuele machines. Veel van deze updates worden uitgevoerd zonder dat de overlast voor uw virtuele machines of Cloud-Services, waaronder updates geheugen behouden.

Sommige updates hoeven echter opnieuw worden opgestart om de vereiste updates toepassen op de infrastructuur voor uw virtuele machines. De virtuele machines zijn afgesloten terwijl we de infrastructuur patch en vervolgens de virtuele machines worden opgestart.

Houd er rekening mee dat er twee soorten onderhoud die invloed op de beschikbaarheid van uw virtuele machines hebben kan: geplande en ongeplande. Deze pagina wordt beschreven hoe Microsoft Azure planmatig onderhoud uitvoert. Zie voor meer informatie over niet-gepland onderhoud, [begrijp wat gepland of ongepland onderhoud](virtual-machines-linux-manage-availability.md).

[AZURE.INCLUDE [virtual-machines-common-planned-maintenance](../../includes/virtual-machines-common-planned-maintenance.md)]

<properties
    pageTitle="Verbinding maken met Windows VMs in een cloud service | Microsoft Azure"
    description="Verbinding maken met Windows virtuele machines voor de van klassieke implementatiemodel naar een Azure cloud-service of een virtueel netwerk gemaakt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="connect-windows-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Verbinding gemaakt met het implementatiemodel klassiek met een virtueel netwerk of cloud-service van Windows virtuele machines

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Windows virtuele machines zijn gemaakt met het klassieke implementatiemodel altijd worden geplaatst in een cloud-service. De cloud-service fungeert als een container en biedt een unieke naam, een openbaar IP-adres en een set van eindpunten voor toegang tot de virtuele machine via het Internet. De cloud-service kan in een virtueel netwerk, maar is niet vereist. U kunt ook [verbinding maken met Linux virtuele machines met virtual network of cloud-service](virtual-machines-linux-classic-connect-vms.md).

Als een cloud-service niet in een virtueel netwerk, is het een *zelfstandige* cloud-service genoemd. De virtuele machines in een zelfstandige cloud-service kan alleen communiceren met andere virtuele machines met behulp van de andere virtuele machines openbare DNS-namen en het verkeer overdracht via Internet. Als u een cloud-service is in een virtueel netwerk, de virtuele machines cloud service kan communiceren met andere virtuele machines in het virtuele netwerk zonder dat al het verkeer via het Internet worden verzonden.

Als u uw virtuele machines in dezelfde zelfstandige cloud-service plaatst, kunt u taakverdeling en beschikbaarheid wordt ingesteld. Zie [virtuele machines voor taakverdeling](virtual-machines-windows-load-balance.md) en [de beschikbaarheid van virtuele machines beheren](virtual-machines-windows-manage-availability.md)voor meer informatie. Echter kan de virtuele machines op subnetten organiseren of aansluiten van een zelfstandige cloud-service op uw netwerk op gebouwen. Hier volgt een voorbeeld:

[AZURE.INCLUDE [virtual-machines-common-classic-connect-vms](../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Volgende stappen

Als u een virtuele machine hebt gemaakt, is een goed idee om [een gegevensschijf toevoegen](virtual-machines-windows-classic-attach-disk.md) zodat de services en de werkbelasting van een locatie voor het opslaan van gegevens. 
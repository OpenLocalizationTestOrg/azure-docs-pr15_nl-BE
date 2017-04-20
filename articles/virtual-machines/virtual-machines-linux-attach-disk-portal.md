<properties
    pageTitle="Een schijf met gegevens koppelen aan een VM Linux | Microsoft Azure"
    description="Het nieuwe of bestaande gegevensschijf koppelen aan een Linux VM in Azure portal met behulp van het implementatiemodel Resource Manager."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-vm-in-the-azure-portal"></a>Het koppelen van een schijf met gegevens aan een Linux VM in Azure portal

In dit artikel wordt beschreven hoe u nieuwe en bestaande schijven koppelen aan een virtuele Linux machine via de portal Azure. U kunt ook [een schijf met gegevens naar een Windows VM in Azure portal koppelen](virtual-machines-windows-attach-disk-portal.md). Voordat u dit doet, lees dan deze tips:

- De omvang van de virtuele machine bepaalt hoeveel gegevensschijven u kunt koppelen. Zie voor meer informatie [voor virtuele machines](virtual-machines-linux-sizes.md).
- Premium-opslagruimte gebruikt, moet u een virtuele machine van DS-serie of de GS-serie. Met deze virtuele machines kunt u schijven van Premium en Standard zowel opslag-accounts. Premium-opslag is beschikbaar in bepaalde regio's. Zie voor meer informatie [Premium-opslag: krachtige opslag voor een standaardwerkbelasting Azure Virtual Machine](../storage/storage-premium-storage.md).
- Schijven die zijn gekoppeld aan virtuele machines zijn daadwerkelijk VHD-bestanden in een account Azure opslag. Zie voor meer informatie [over schijven en VHD's voor virtuele machines](virtual-machines-linux-about-disks-vhds.md).
- Voor een nieuwe schijf hoeft u niet eerst maken omdat Azure gemaakt wanneer u dit aansluit.
- Voor een bestaande schijf moet het VHD-bestand beschikbaar zijn in een account Azure opslag. U kunt een VHD die al is er, als deze niet gekoppeld aan een andere virtuele machine of upload uw eigen VHD-bestand naar de rekening voor opslag.


[AZURE.INCLUDE [virtual-machines-common-attach-disk-portal](../../includes/virtual-machines-common-attach-disk-portal.md)]

## <a name="next-steps"></a>Volgende stappen

Nadat de schijf wordt toegevoegd, moet u voorbereiden voor gebruik. In het besturingssysteem van de virtuele machine Zie: ' hoe: initialiseren van een gegevensschijf in Linux ' in dit [artikel](virtual-machines-linux-classic-attach-disk.md#how-to-initialize-a-new-data-disk-in-linux).

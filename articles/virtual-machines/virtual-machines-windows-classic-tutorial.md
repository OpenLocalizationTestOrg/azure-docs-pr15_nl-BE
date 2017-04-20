<properties
    pageTitle="Een VM maken in de klassieke portal | Microsoft Azure"
    description="Maak een virtuele Windows-computer in de klassieke Azure portal."
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
    ms.date="10/18/2016"
    ms.author="cynthn"/>

# <a name="create-a-virtual-machine-running-windows-in-the-azure-classic-portal"></a>Een virtuele machine met Windows in de klassieke Azure portal maken

> [AZURE.SELECTOR]
- [Azure klassieke portal](virtual-machines-windows-classic-tutorial.md)
- [PowerShell: Implementatie klassiek](virtual-machines-windows-classic-create-powershell.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Meer informatie over het [uitvoeren van deze stappen het implementatiemodel Resource Manager](virtual-machines-windows-hero-tutorial.md) met behulp van de **nieuwe portal van Azure**. 

In deze zelfstudie wordt beschreven hoe u een Azure VM (virtual machine) waarop Windows wordt uitgevoerd in de klassieke Azure portal maken. Een installatiekopie van Windows Server wordt gebruikt als voorbeeld, maar dat is slechts een van de vele afbeeldingen die Azure biedt. Houd er rekening mee dat de opties in de installatiekopie is afhankelijk van uw abonnement. Bijvoorbeeld kunnen Windows desktop-images zijn voor MSDN-abonnees.

In deze sectie wordt beschreven hoe u de optie **Galerij uit** in de klassieke Azure portal gebruiken voor het maken van de virtuele machine. Deze optie biedt meer configuratiemogelijkheden dan de optie **Snel maken** . Bijvoorbeeld, als u deelnemen aan een virtuele machine met een virtueel netwerk wilt, moet u het gebruik van de optie **Galerij uit** .

U kunt ook VMs met [uw eigen afbeeldingen](virtual-machines-windows-classic-createupload-vhd.md)maken. Zie meer informatie over deze en andere methoden, [verschillende manieren voor het maken van een virtuele Windows-computer](virtual-machines-windows-creation-choices.md).



## <a name="video-walkthrough"></a>Video overzicht

Hier volgt een overzicht van deze zelfstudie.

[AZURE.VIDEO creating-a-windows-vm-on-microsoft-azure-classic-portal]

## <a id="createvirtualmachine"> </a>De virtuele machine maken

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [een VM met behulp van het implementatiemodel Resource Manager maken](virtual-machines-windows-hero-tutorial.md) in de nieuwe portal Azure. 

- Meld u op de virtuele machine. Zie [aanmelden bij een virtuele machine met Windows Server](virtual-machines-windows-classic-connect-logon.md)voor meer informatie.

- Een schijf voor het opslaan van gegevens hebt toegevoegd. U kunt zowel lege schijven en schijven met gegevens kunt koppelen. Zie voor instructies het [toevoegen van een schijf met gegevens naar een virtuele Windows-computer met het klassieke implementatiemodel gemaakt](virtual-machines-windows-classic-attach-disk.md).

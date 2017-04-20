<properties
    pageTitle="Een schijf toevoegen aan een VM | Microsoft Azure"
    description="Een schijf met gegevens koppelen aan een virtuele Windows-computer met het klassieke implementatiemodel gemaakt en deze te initialiseren."
    services="virtual-machines-windows, storage"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="cynthn"/>

# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Een schijf met gegevens koppelen aan een virtuele Windows-computer met het klassieke implementatiemodel gemaakt

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Als u de nieuwe portal gebruiken, raadpleegt u [het koppelen van een schijf met gegevens naar een Windows VM in Azure portal](virtual-machines-windows-attach-disk-portal.md).

Als u een gegevensschijf als u meer nodig hebt, kunt u een lege schijf of een bestaande schijf met gegevens koppelen aan een VM. In beide gevallen worden de schijven VHD-bestanden die zich in een opslagruimte Azure-account bevinden. In het geval van een nieuwe schijf nadat u de schijf hebt aangesloten moet ook u voor het initialiseren zodat deze gereed voor gebruik door een Windows VM is.

Zie voor meer informatie over schijven [over schijven en VHD's voor virtuele Machines](virtual-machines-windows-about-disks-vhds.md).


[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## <a name="initialize-the-disk"></a>Initialiseer de schijf

1. Verbinding maken met de virtuele machine. Zie voor instructies [hoe u aan te melden op een virtuele machine met Windows Server][logon].

2. Nadat u zich aanmeldt bij de virtuele machine, opent u **Server Manager**. Selecteer in het linkerdeelvenster **Bestands- en opslagservices**.

    ![Serverbeheer openen](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)

3. Vouw het menu en selecteer de **schijven**.

4. De sectie **schijven** worden de schijven. In de meeste gevallen heeft dit schijf 0 en schijf 1 diskette 2. Schijf 0 is de schijf en diskette 2 is de gegevensschijf die u zojuist hebt gekoppeld aan de VM schijf 1 is de tijdelijke schijf. De nieuwe gegevensschijf wordt de partitie een lijst als **Onbekend**. Klik met de rechtermuisknop op de schijf en selecteer **initialiseren**.

5.  U bent gewaarschuwd dat alle gegevens worden gewist wanneer de schijf wordt geïnitialiseerd. Klik op **Ja** om de waarschuwing bevestigen en initialiseer de schijf. Als het eenmaal is voltooid, wordt de Partion weergegeven als **GPT**. Opnieuw met de rechtermuisknop op de schijf en selecteer **NieuwVolume**.

6.  Voltooi de wizard met behulp van de standaardwaarden. Wanneer de wizard is voltooid, worden de **Volumes** gedeelte het nieuwe volume. De schijf is nu on line en gereed is om gegevens op te slaan.

    ![Volume is geïnitialiseerd](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [AZURE.NOTE] De grootte van de VM bepaalt hoeveel schijven u kunt koppelen aan. Zie voor meer informatie [voor virtuele machines](virtual-machines-linux-sizes.md).

## <a name="additional-resources"></a>Aanvullende bronnen

[Het ontkoppelen van een schijf vanaf een virtuele machine onder Windows](virtual-machines-windows-classic-detach-disk.md)

[Over schijven en VHD's voor virtuele machines](virtual-machines-linux-about-disks-vhds.md)

[logon]: virtual-machines-windows-classic-connect-logon.md

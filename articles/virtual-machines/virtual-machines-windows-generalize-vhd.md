<properties
    pageTitle="Een Windows VHD generalize | Microsoft Azure"
    description="Leer hoe u met Sysprep een Windows VM gebruiken met het implementatiemodel Resource Manager generaliseren."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
    
# <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Een virtuele Windows-computer met behulp van Sysprep generalize

In deze sectie wordt beschreven hoe u de virtuele Windows-computer voor gebruik als een afbeelding generaliseren. Sysprep verwijdert alle uw persoonlijke gegevens, onder andere, en zorgt ervoor dat de machine moet worden gebruikt als een afbeelding. Zie voor meer informatie over Sysprep [hoe u Sysprep gebruikt: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).

Zorg ervoor dat de serverfuncties die op de computer door Sysprep worden ondersteund. Zie [Sysprep-ondersteuning voor Server-rollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles) voor meer informatie.

>[AZURE.IMPORTANT] Als u Sysprep uitvoert voordat u de VHD uploadt naar Azure voor de eerste keer, controleert u of u [Bereid uw VM](virtual-machines-windows-prepare-for-upload-vhd-image.md) voordat u Sysprep uitvoert. 

1. Log in om de virtuele Windows-computer.

2. Open het venster opdrachtprompt als administrator. Wijzigen in de map **%windir%\system32\sysprep**en voer `sysprep.exe`.

3. Selecteer **systeem invoeren Out-of-Box Experience (OOBE)**en zorg ervoor dat het selectievakje **generaliseren** is geselecteerd in het dialoogvenster **Hulpprogramma voor systeemvoorbereiding** .

4. Selecteer in de **Opties voor afsluiten**, **Afsluiten**.

5. Klik op **OK**.

    ![Start Sysprep](./media/virtual-machines-windows-upload-image/sysprepgeneral.png)

6. Wanneer Sysprep is voltooid, wordt deze afgesloten de virtuele machine. 

## <a name="next-steps"></a>Volgende stappen

- Als de VM op gebouwen, kunt u [de VHD naar Azure uploaden](virtual-machines-windows-upload-image.md).
- Als de VM al in Azure, kunt u nu [een afbeelding van de algemene VM maken](virtual-machines-windows-capture-image.md).
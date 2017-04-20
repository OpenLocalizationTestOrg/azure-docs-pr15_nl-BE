<properties
    pageTitle="Een installatiekopie van een Windows Azure VM | Microsoft Azure"
    description="Een installatiekopie van een Windows Azure virtual machine gemaakt met het klassieke implementatiemodel."
    services="virtual-machines-windows"
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
    ms.date="09/27/2016"
    ms.author="cynthn"/>

#<a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Een installatiekopie van een Windows Azure virtual machine gemaakt met het klassieke implementatiemodel.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zie [Windows VM kopie maken in Azure uitgevoerd](virtual-machines-windows-vhd-copy.md)voor bronbeheer model informatie.


In dit artikel wordt beschreven hoe u voor het vastleggen van een Azure virtuele machine met Windows, zodat u deze als een afbeelding gebruiken kunt andere virtuele machines maken. Deze afbeelding bevat de schijf en gegevensschijven die zijn gekoppeld aan de virtuele machine. Het bevat geen netwerkconfiguraties, dus u configureren die moet bij het maken van de andere virtuele machines waarop de afbeelding.

Azure slaat de afbeelding onder **Mijn afbeeldingen**. Dit is de plaats waar de afbeeldingen die je hebt geüpload zijn opgeslagen. Zie voor meer informatie over afbeeldingen, [afbeeldingen voor virtuele machines](virtual-machines-linux-classic-about-images.md).

##<a name="before-you-begin"></a>Voordat u begint##

Deze stappen wordt ervan uitgegaan dat u hebt al een Azure virtuele machine gemaakt en geconfigureerd, het besturingssysteem, met inbegrip van alle gegevensschijven koppelen. Als u dit nog niet hebt gedaan, raadpleegt u deze instructies:

- [Een virtuele machine maken van een installatiekopie](virtual-machines-windows-classic-createportal.md)
- [Het koppelen van een schijf met gegevens op een virtuele machine](virtual-machines-windows-classic-attach-disk.md)
- Zorg ervoor dat de serverfuncties die worden ondersteund met Sysprep. Zie [Sysprep-ondersteuning voor Server-rollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)voor meer informatie.

> [AZURE.WARNING] Dit proces wordt de oorspronkelijke virtuele machine verwijderd nadat deze vastgelegd. 

Voordat u een afbeelding van een Azure virtual machine caputuring, is het raadzaam de target virtuele machine back-up gemaakt. Azure virtuele machines kunnen worden gestaafd met Azure back-up. Zie [Back-up van Azure virtual machines](../backup/backup-azure-vms.md). Andere oplossingen zijn beschikbaar van gecertificeerde partners. Als u wilt weten wat er momenteel beschikbaar is, zoekt de markt Azure.


##<a name="capture-the-virtual-machine"></a>De virtuele machine vastleggen

1. In de [Azure klassieke portal](http://manage.windowsazure.com), **verbinding maken** met de virtuele machine. Zie voor instructies [hoe u zich kunt aanmelden bij een virtuele machine met Windows Server] [].

2.  Open een opdrachtvenster als beheerder.

3.  Activeer de map `%windir%\system32\sysprep`, en Voer sysprep.exe.

4.  Het dialoogvenster **Hulpprogramma voor systeemvoorbereiding** wordt weergegeven. Voer de volgende handelingen uit:

    - In **Systeem opschonen actie**, selecteer **systeem invoeren Out-of-Box Experience (OOBE)** en controleer of **generaliseren** is ingeschakeld. Zie voor meer informatie over het gebruik van Sysprep [hoe u Sysprep gebruikt: An Introduction][].

    - Selecteer in de **Opties voor afsluiten**, **Afsluiten**.

    - Klik op **OK**.

    ![Sysprep uitvoeren](./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png)

7.  Sysprep is afgesloten van de virtuele machine die de status van de virtuele machine in de klassieke Azure portal **gestopt verandert**.

8.  In de klassieke Azure portal op **virtuele Machines** en de virtuele machine die u wilt vastleggen selecteren.

9.  Klik op **vastleggen**op de opdrachtbalk.

    ![Virtuele machine vastleggen](./media/virtual-machines-windows-classic-capture-image/CaptureVM.png)

    Het **vastleggen van de virtuele Machine** van het dialoogvenster wordt weergegeven.

10. Typ in het vak **Naam**een naam voor de nieuwe afbeelding.

11. Voordat u een installatiekopie van Windows Server aan uw set van aangepaste installatiekopieën toevoegen, moet u deze algemene door Sysprep uit te voeren volgens de instructies in de vorige stappen. Klik op **ik Sysprep op de virtuele machine hebt uitgevoerd** om aan te geven u dat hebt gedaan.

12. Klik op het selectievakje voor het vastleggen van de afbeelding. De nieuwe afbeelding is nu beschikbaar voor **afbeeldingen**.

    ![Vastleggen van het image voltooid](./media/virtual-machines-windows-classic-capture-image/VMCapturedImageAvailable.png)

##<a name="next-steps"></a>Volgende stappen

De afbeelding is klaar om te worden gebruikt voor het maken van virtuele machines. Hiertoe maakt u een virtuele machine met behulp van het menu-item **Uit de galerie** en selecteert u de afbeelding die u zojuist hebt gemaakt. Zie [een virtuele machine uit een afbeelding maken](virtual-machines-windows-classic-createportal.md)voor meer informatie.



[Hoe kunt u zich aanmelden bij een virtuele machine met Windows Server]: virtual-machines-windows-classic-connect-logon.md
[Het gebruik van Sysprep: een inleiding]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/virtual-machines-windows-classic-capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png

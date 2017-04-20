<properties
    pageTitle="Maken en uploaden van de afbeelding van een VM met Powershell | Microsoft Azure"
    description="Informatie over maken en uploaden van een algemene Server Windows-installatiekopie (VHD) met de klassieke implementatiemodel en Azure Powershell."
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
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a>Maken en uploaden van een Windows Server-VHD naar Azure

In dit artikel wordt beschreven hoe u uw eigen algemene VM om afbeelding te uploaden als een virtuele harde schijf (VHD) zodat u deze gebruiken kunt voor het maken van virtuele machines. Zie voor meer informatie over schijven en VHD's in Microsoft Azure [over schijven en VHD's voor virtuele Machines](virtual-machines-linux-about-disks-vhds.md).


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]. U kunt ook [uploaden van](virtual-machines-windows-upload-image.md) een virtuele machine met behulp van het model Resource Manager. 

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u beschikt over:

- **Abonnement op een Azure** - als u niet hebt, kunt u [gratis een Azure account openen](/pricing/free-trial/?WT.mc_id=A261C142F).

- **[Microsoft Azure PowerShell](../powershell-install-configure.md)** - hebt u de module Microsoft Azure PowerShell geïnstalleerd en geconfigureerd voor het gebruik van uw abonnement. 

- **A . VHD-bestand** - ondersteunde Windows-besturingssysteem in het VHD-bestand opgeslagen en gekoppeld aan een virtuele machine. Controleer als de serverfuncties op de VHD worden door Sysprep ondersteund. Zie [Sysprep-ondersteuning voor Server-rollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)voor meer informatie.

> [AZURE.IMPORTANT] De VHDX-indeling wordt niet ondersteund in Microsoft Azure. U kunt de schijf converteren naar met Hyper-V-beheer of de [cmdlet converteren VHD](http://technet.microsoft.com/library/hh848454.aspx)VHD-indeling. Zie deze [blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx)voor meer informatie.

## <a name="step-1-prep-the-vhd"></a>Stap 1: Voorbereiding de VHD 

Voordat u de VHD naar Azure uploadt, moet deze worden generalized met behulp van het hulpprogramma Sysprep. Dit zorgt ervoor dat de VHD moet worden gebruikt als een afbeelding. Zie voor meer informatie over Sysprep [hoe u Sysprep gebruikt: An Introduction](http://technet.microsoft.com/library/bb457073.aspx). Back-up van de VM voordat u Sysprep uitvoert.

Van de virtuele machine waar het besturingssysteem is geïnstalleerd, voert u de volgende procedure:

1. Aanmelden bij het besturingssysteem.

2. Open een opdrachtvenster als beheerder. Wijzigen in de map **%windir%\system32\sysprep**en voer `sysprep.exe`.

    ![Een opdrachtpromptvenster openen](./media/virtual-machines-windows-classic-createupload-vhd/sysprep_commandprompt.png)

3.  Het dialoogvenster **Hulpprogramma voor systeemvoorbereiding** wordt weergegeven.

    ![Start Sysprep](./media/virtual-machines-windows-classic-createupload-vhd/sysprepgeneral.png)

4.  In het **Hulpprogramma voor systeemvoorbereiding** **Voer systeem Out of Box Experience (OOBE)** selecteren en zorg ervoor dat het **generaliseren** wordt gecontroleerd.

5.  Selecteer in de **Opties voor afsluiten**, **Afsluiten**.

6.  Klik op **OK**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Stap 2: Maak een opslag en een container

U moet een account voor opslag in Azure zodat u beschikt over een plaats in het VHD-bestand te uploaden. Deze stap wordt beschreven hoe u een account maken of ophalen van de informatie die u nodig hebt van een bestaande account. Vervang de variabelen in &lsaquo; haken &rsaquo; met uw eigen gegevens.

1. Aanmelding

        Add-AzureAccount

1. Stel uw abonnement Azure.

        Select-AzureSubscription -SubscriptionName <SubscriptionName> 

2. Maak een nieuwe account voor opslag. De naam van de account van de opslag moet uniek zijn, 3-24 tekens. De naam kan een willekeurige combinatie van letters en cijfers zijn. U moet ook een locatie zoals 'East ons'
        
        New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>

3. De nieuwe opslag-account als standaard instellen.
        
        Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>

4. Maak een nieuwe container.

        New-AzureStorageContainer -Name <ContainerName> -Permission Off

 

## <a name="step-3-upload-the-vhd-file"></a>Stap 3: Het VHD-bestand uploaden

Gebruik de [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) voor het uploaden van de VHD.

Typ de volgende opdracht in het venster Azure PowerShell u in de vorige stap hebt gebruikt en vervangt de variabelen in &lsaquo; haken &rsaquo; met uw eigen gegevens.

        Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>


## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a>Stap 4: De afbeelding toevoegen aan de lijst met aangepaste images

Gebruik de cmdlet [Add-AzureVMImage](https://msdn.microsoft.com/library/mt589167.aspx) aan de afbeelding toevoegen aan de lijst van uw aangepaste images.

        Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"


## <a name="next-steps"></a>Volgende stappen

U kunt nu [een aangepaste VM maken](virtual-machines-windows-classic-createportal.md) met behulp van de afbeelding die u hebt geüpload.


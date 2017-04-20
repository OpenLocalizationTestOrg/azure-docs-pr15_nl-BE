<properties
    pageTitle="Een SQL Server virtuele Machine maken in Azure PowerShell (klassiek) | Microsoft Azure"
    description="Stappen en PowerShell-scripts biedt voor het maken van een Azure VM met SQL Server virtuele machine galerijafbeeldingen. In dit onderwerp wordt de implementatie van de klassieke modus gebruikt."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/07/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Inrichten van een SQL Server virtuele machine met Azure PowerShell (klassiek)

## <a name="overview"></a>Overzicht

Dit artikel bevat stappen voor het maken van een SQL Server virtuele machine in Azure met de PowerShell-cmdlets.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zie [voorziening een SQL Server virtuele machine Azure PowerShell Resource Manager gebruiken](virtual-machines-windows-ps-sql-create.md)voor de Resource Manager-versie van dit onderwerp.

### <a name="install-and-configure-powershell"></a>Installeer en configureer PowerShell:

1. Als u niet een Azure-account hebt, gaat u naar [Azure gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

2. [Download en installeer de meest recente Azure PowerShell-opdrachten](../powershell-install-configure.md).

3. Start Windows PowerShell en verbinding maken met uw Azure-abonnement met de opdracht **Add-AzureAccount** .

        Add-AzureAccount

## <a name="determine-your-target-azure-region"></a>Bepaal uw doel Azure regio

De virtuele Machine van SQL Server wordt beheerd in een cloud-service die een specifiek gebied van Azure. De volgende stappen u helpen bij het bepalen van uw regio, opslag account en cloud-service die wordt gebruikt voor de rest van de zelfstudie.

1. Bepaal het datacenter die u gebruiken wilt als host voor uw SQL Server-VM. De beschikbare regio's weergegeven de volgende PowerShell-opdrachten in detail met een overzicht aan het einde.

        Get-AzureLocation
        (Get-AzureLocation).Name

2.  Zodra u uw gewenste plaats hebt bepaald, stelt een variabele met de naam **$dcLocation** in dat gebied.

        $dcLocation = "<region name>"

## <a name="set-your-subscription-and-storage-account"></a>Instellen van uw account abonnement en opslag

1. Bepaal de Azure abonnement dat u voor de nieuwe virtuele machine gebruiken wilt.

        (Get-AzureSubscription).SubscriptionName

1. Het doel van Azure abonnement toewijzen aan de variabele **$subscr** . Stel dit in als je huidige abonnement Azure.

        $subscr="<subscription name>"
        Select-AzureSubscription -SubscriptionName $subscr –Current

1. Controleer vervolgens op bestaande accounts voor opslag. Het volgende script worden alle opslag accounts die aanwezig zijn in uw regio gekozen:

        (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName

    >[AZURE.NOTE] Als u nodig hebt voor een nieuwe account voor opslag, maakt u eerst een accountnaam alle kleine opslag met de opdracht Nieuw AzureStorageAccount zoals in het volgende voorbeeld: **Nieuw-AzureStorageAccount - StorageAccountName "<storage account name>"-locatie $dcLocation**

1. De naam van de doel opslag toewijzen aan de **$staccount**. **Set AzureSubscription** vervolgens gebruiken voor het instellen van het abonnement en de huidige account voor opslag.

        $staccount="<storage account name>"
        Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

## <a name="select-a-sql-server-virtual-machine-image"></a>Selecteer de afbeelding van een SQL Server virtuele machine

1. Ontdek de lijst met beschikbare kopieën van SQL Server virtuele machines in de galerie. Deze afbeeldingen hebben een **ImageFamily** -eigenschap die met 'SQL begint'. De volgende query geeft de familie afbeelding beschikbaar aan u met SQL Server is geïnstalleerd.

        Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily

1. Als u de virtuele machine afbeelding-familie, kan er meerdere gepubliceerde afbeeldingen in deze groep. Het volgende script gebruiken om te zoeken naar de meest recente gepubliceerde virtuele machine afbeelding naam voor de geselecteerde afbeelding familie (zoals **SQL Server 2016 RTM Enterprise op Windows Server 2012 R2**):

        $family="<ImageFamily value>"
        $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

        echo "Selected SQL Server image name:"
        echo "   $image"

## <a name="create-the-virtual-machine"></a>De virtuele machine maken

Tot slot maakt u de virtuele machine met PowerShell:

1. Maak een cloud-service als host voor de nieuwe VM. Houd er rekening mee dat het is ook mogelijk om een bestaande cloud-service te gebruiken. Maak een nieuwe variabele **$svcname** met de korte naam van de service cloud.

        $svcname = "<cloud service name>"
        New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

2. De naam van de virtuele machine en een grootte opgeven. Zie voor meer informatie over de grootte van de virtuele machine, [Virtual Machine formaten voor Azure](virtual-machines-windows-sizes.md).

        $vmname="<machine name>"
        $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
        $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

3. De lokale administrator-account en het wachtwoord opgeven.

        $cred=Get-Credential -Message "Type the name and password of the local administrator account."
        $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

4. Voer het volgende script om de virtuele machine maken.

        New-AzureVM –ServiceName $svcname -VMs $vm1

>[AZURE.NOTE] Zie de sectie **de opdrachtset samenstellen** in [Gebruik Azure PowerShell maken en vooraf configureren van Windows-gebaseerde virtuele Machines](virtual-machines-windows-classic-create-powershell.md)voor extra uitleg en configuratieopties.

## <a name="example-powershell-script"></a>Voorbeeld van de PowerShell-script

Het volgende script bevat en voorbeeld van een volledig script waarmee u een virtuele machine voor **SQL Server 2016 RTM Enterprise op Windows Server 2012 R2** . Als u dit script gebruikt, moet u de oorspronkelijke variabelen op basis van de voorgaande stappen in dit onderwerp aanpassen.

    # Customize these variables based on your settings and requirements:
    $dcLocation = "East US"
    $subscr="mysubscription"
    $staccount="mystorageaccount"
    $family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
    $svcname = "mycloudservice"
    $vmname="myvirtualmachine"
    $vmsize="A5"

    # Set the current subscription and storage account
    # Comment out the New-AzureStorageAccount line if the account already exists
    Select-AzureSubscription -SubscriptionName $subscr –Current
    New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

    # Select the most recent VM image in this image family:
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    # Create the new cloud service; comment out this line if cloud service exists already:
    New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

    # Create the VM config:
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    # Set administrator credentials:
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    # Create the SQL Server VM:
    New-AzureVM –ServiceName $svcname -VMs $vm1


## <a name="connect-with-remote-desktop"></a>Verbinding met extern bureaublad

1. Maak de. RDP-bestanden in de Documentenmap van de huidige gebruiker aan deze virtuele machines te starten:

        $documentspath = [environment]::getfolderpath("mydocuments")
        Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"

1. Start het RDP-bestand in de map documenten. Verbinding maken met de gebruikersnaam en het wachtwoord dat eerder (bijvoorbeeld als uw gebruikersnaam VMAdmin is, '\VMAdmin' opgeven als de gebruiker en het wachtwoord op te geven).

        cd $documentspath
        .\vm1.rdp

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>Voltooi de configuratie van de SQL Server-computer voor externe toegang

Na het aanmelden bij de computer met extern bureaublad, op basis van de instructies in de [stappen voor het configureren van SQL Server-verbinding in een VM Azure](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)SQL-Server te configureren.

## <a name="next-steps"></a>Volgende stappen

Hier vindt u aanvullende instructies voor het inrichten van virtuele machines met PowerShell in de [documentatie van de virtuele machines](virtual-machines-windows-classic-create-powershell.md). Zie voor extra scripts die betrekking hebben op de SQL Server- en Premium, [Gebruik Azure Premium opslag met SQL Server op virtuele Machines](virtual-machines-windows-classic-sql-server-premium-storage.md).

In veel gevallen is de volgende stap uw databases migreren naar deze nieuwe SQL Server-VM. Zie voor richtlijnen database migratie, [migreren van een Database aan SQL Server op een Azure VM](virtual-machines-windows-migrate-sql.md).

Als u ook geïnteresseerd bent in SQL virtuele Machines maken met behulp van de portal Azure, Zie [het inrichten van een SQL Server virtuele Machine op Azure](virtual-machines-windows-portal-sql-server-provision.md). Houd er rekening mee dat de handleiding die u bij de portal begeleidt met de aanbevolen Resource Manager-model in plaats van het klassieke model gebruikt in dit onderwerp PowerShell VMs maakt.

Behalve deze bronnen, raden wij u aan [andere onderwerpen over het uitvoeren van SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md)te controleren.

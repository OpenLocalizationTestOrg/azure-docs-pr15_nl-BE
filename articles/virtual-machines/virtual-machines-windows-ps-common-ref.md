<properties 
   pageTitle="Algemene PowerShell-opdrachten voor VMs | Microsoft Azure"
   description="Algemene PowerShell-opdrachten kunt u maken en beheren van uw VMs in Azure op Windows"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="davidmu1" 
   manager="timlt" 
   editor="tysonn" 
   tags="azure-resource-manager"/>
   
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="09/27/2016"
   ms.author="davidmu" />

# <a name="common-powershell-commands-for-creating-and-managing-vms"></a>Algemene PowerShell-opdrachten voor het maken en beheren van VMs

In dit artikel komen een aantal van de Azure PowerShell-opdrachten die u kunt maken en beheren van virtuele machines in uw abonnement Azure.  Voor meer gedetailleerde hulp bij specifieke schakelopties en opties, kunt u **Get-Help** - *opdracht*.

[Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor informatie over het installeren van de nieuwste versie van Azure PowerShell, uw abonnement selecteren en aanmelden bij uw account.

## <a name="create-a-vm"></a>Een VM maken

Taak | Opdracht
-------------- | -------------------------
Maak een VM-configuratie | $vm = [New AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) - VMName "vm_name" - VMSize "vm_size"<BR></BR><BR></BR>De VM-configuratie wordt gebruikt om te definiëren of de instellingen voor de VM bijwerken. De configuratie wordt geïnitialiseerd met de naam van de VM en de [grootte](virtual-machines-windows-sizes.md).
Configuratie-instellingen toevoegen | $vm = $vm [Set AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) - VM-'computernaam' Windows - computernaam-referenties $cred - ProvisionVMAgent - EnableAutoUpdate<BR></BR><BR></BR>Instellingen van het besturingssysteem met inbegrip van de [referenties](https://technet.microsoft.com/library/hh849815.aspx) worden toegevoegd aan het configuratieobject dat u eerder hebt gemaakt met behulp van nieuwe AzureRmVMConfig.
Een netwerkinterface toevoegt | $vm = [Toevoegen AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) - VM $vm-Id $nic. ID<BR></BR><BR></BR>Een VM moet een [netwerk-interface](virtual-machines-windows-ps-create.md) om te communiceren in een virtueel netwerk hebben. U kunt ook [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) gebruiken voor het ophalen van een bestaand netwerk interface-object.
Geef een besturingssysteemkopie | $vm = [Set AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) - VM $vm - PublisherName "publisher_name"-voorstel "publisher_offer" - SKU's "product_sku"-"nieuwste" versie<BR></BR><BR></BR>[Informatie over de afbeelding](virtual-machines-windows-cli-ps-findimage.md) wordt toegevoegd aan het configuratieobject dat u eerder hebt gemaakt met behulp van nieuwe AzureRmVMConfig. Het object dat het resultaat van deze opdracht wordt alleen gebruikt wanneer u de OS-schijf om de afbeelding van een platform te gebruiken.
Set OS schijf gebruik van een besturingssysteemkopie | $vm = $vm [Set AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) - VM-naam "disk_name" - VhdUri "http://mystore1.blob.core.windows.net/vhds/disk_name.vhd" - CreateOption FromImage<BR></BR><BR></BR>De naam van de schijf en de bijbehorende locatie in de [opslag](../storage/storage-powershell-guide-full.md) wordt toegevoegd om het configuratieobject dat u eerder hebt gemaakt.
OS schijf gebruik van een algemene installatiekopie instellen | $vm = $vm set AzureRmVMOSDisk - VM-https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk"naam"disk_name"- SourceImageUri. VHD {guid}"- VhdUri"https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"- CreateOption FromImage-Windows<BR></BR><BR></BR>De naam van de schijf, de locatie van de bronafbeelding en de locatie van de schijf in de [opslag](../storage/storage-powershell-guide-full.md) wordt toegevoegd om het configuratieobject.
Set OS schijf om een speciale afbeelding gebruiken | $vm = $vm set AzureRmVMOSDisk - VM-naam "name_of_disk" - VhdUri "http://mystore1.blob.core.windows.net/vhds/" - CreateOption koppelen - Windows
Een VM maken | "Resource_group_name" [Nieuw-AzureRmVM]() - ResourceGroupName-locatie 'naam_locatie' - VM $vm<BR></BR><BR></BR>Alle resources worden in een [resourcegroep](../powershell-azure-resource-manager.md)gemaakt. De VM moet worden gemaakt op dezelfde [locatie](https://msdn.microsoft.com/library/azure/dn495177.aspx) als de resourcegroep. Voordat u deze opdracht uitvoert, worden nieuwe AzureRmVMConfig, Set AzureRmVMOperatingSystem Set AzureRmVMSourceImage, Add-AzureRmVMNetworkInterface en Set-AzureRmVMOSDisk uitgevoerd.

## <a name="get-information-about-vms"></a>Informatie over het VMs

Taak | Opdracht
-------------- | -------------------------
Lijst van VMs in een abonnement| [Get-AzureRmVM](https://msdn.microsoft.com/library/mt603718.aspx)
Lijst van VMs in een resourcegroep | Get-AzureRmVM - ResourceGroupName "resource_group_name"<BR></BR><BR></BR>Als u een lijst met resourcegroepen in uw abonnement, [Get-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt679016.aspx)te gebruiken.
Informatie opvragen over een VM | 'Resource_group_name' Get-AzureRmVM - ResourceGroupName-naam "vm_name"

## <a name="manage-vms"></a>VMs beheren

Taak | Opdracht
-------------- | -------------------------
Start een VM | [Start-AzureRmVM](https://msdn.microsoft.com/library/mt603453.aspx) - ResourceGroupName "resource_group_name"-de naam "vm_name"
Stop een VM | [Stop-AzureRmVM](https://msdn.microsoft.com/library/mt603483.aspx) - ResourceGroupName "resource_group_name"-de naam "vm_name"
Opnieuw een actieve VM | [Opnieuw AzureRmVM](https://msdn.microsoft.com/library/mt603775.aspx) - ResourceGroupName "resource_group_name"-de naam "vm_name"
Een VM verwijderen | [Verwijderen AzureRmVM](https://msdn.microsoft.com/library/mt603641.aspx) - ResourceGroupName "resource_group_name"-de naam "vm_name"
Een VM generalize | [Set AzureRmVm](https://msdn.microsoft.com/library/mt603688.aspx) - ResourceGroupName YourResourceGroup-de naam 'vm_name'-Generalized<BR></BR><BR></BR>Deze opdracht uitvoeren voordat u opslaan AzureRmVMImage uitvoeren.
Vastleggen van een VM | [Opslaan-AzureRmVMImage](https://msdn.microsoft.com/library/mt619423.aspx) - ResourceGroupName "resource_group_name" - VMName "vm_name" - DestinationContainerName "image_container" - VHDNamePrefix "image_name_prefix"-pad "C:\filepath\filename.json"<BR></BR><BR></BR>Een virtuele machine moet [afsluiten en generalized](virtual-machines-windows-generalize-vhd.md) moet worden gebruikt om een afbeelding te maken. Set AzureRmVm voordat u deze opdracht uitvoert, worden uitgevoerd.
Een VM bijwerken | [Update AzureRmVM](https://msdn.microsoft.com/library/mt603662.aspx) - ResourceGroupName "resource_group_name" - VM $vm<BR></BR><BR></BR>Get, Get-AzureRmVM met de huidige VM-configuratie, wijzigen van configuratie-instellingen op de VM-object en voer deze opdracht.
Een schijf met gegevens toevoegen aan een VM | [Toevoegen-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603673.aspx) - VM $vm-naam "disk_name" - VhdUri "https://mystore1.blob.core.windows.net/vhds/disk_name.vhd" - LUN #-ReadWrite Caching - DiskSizeinGB # - CreateOption leeg<BR></BR><BR></BR>Get-AzureRmVM gebruiken om de VM-object. Hiermee geeft u het LUN-nummer en de grootte van de schijf. Update-AzureRmVM om de configuratiewijzigingen van toepassing op de VM worden uitgevoerd. De schijf die u toevoegt, is niet geïnitialiseerd. Voor informatie over het initialiseren van schijven worden toegevoegd, Zie [beheren van Azure virtuele Machines met behulp van bronbeheer en PowerShell](virtual-machines-windows-ps-manage.md).
Een schijf met gegevens verwijderen uit een VM | [Verwijderen AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx) - VM $vm-naam "disk_name"<BR></BR><BR></BR>Get-AzureRmVM gebruiken om de VM-object. Update-AzureRmVM om de configuratiewijzigingen van toepassing op de VM worden uitgevoerd.
Een uitbreidingsmodule toevoegen aan een VM | "Resource_group_name" [Set AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) - ResourceGroupName-locatie "azure_location" - VMName "vm_name"-naam "extension_name"-Publisher "publisher_name"-Type "extension_type" - TypeHandlerVersion "##"-instellingen $Settings - ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Deze opdracht uitvoeren met de juiste [configuratie-informatie](virtual-machines-windows-extensions-configuration-samples.md) voor de extensie die u wilt installeren.
Verwijder een VM-extensie | [Verwijderen AzureRmVMExtension](https://msdn.microsoft.com/library/mt603782.aspx) - ResourceGroupName "resource_group_name"-de naam "extension_name" - VMName "vm_name"

## <a name="next-steps"></a>Volgende stappen

- Zie de basisstappen voor het maken van een virtuele machine in het [maken van een Windows VM met behulp van bronbeheer en PowerShell](virtual-machines-windows-ps-create.md).


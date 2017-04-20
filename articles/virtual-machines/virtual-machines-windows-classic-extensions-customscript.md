<properties
   pageTitle="Aangepast Script extensie op een Windows VM | Microsoft Azure"
   description="Azure VM configuratietaken automatiseren met behulp van de uitbreiding aangepast Script PowerShell scripts uitvoeren op een externe Windows-VM"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/06/2015"
   ms.author="kundanap"/>

# <a name="custom-script-extension-for-windows-virtual-machines"></a>Aangepaste Script-extensie voor Windows virtuele machines

Dit artikel geeft een overzicht van de uitbreiding aangepast Script gebruiken op Windows VMs met Azure PowerShell-cmdlets en Azure Service Management API's.

Virtual machine (VM) extensies zijn gebouwd door Microsoft en vertrouwde uitgevers van derden uit te breiden de functionaliteit van de VM. Zie voor een overzicht van de VM-extensies, [functies en uitbreidingen voor Azure VM](virtual-machines-windows-extensions-features.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Meer informatie over hoe u [deze stappen uitvoert met behulp van het model Resource Manager](virtual-machines-windows-extensions-customscript.md).

## <a name="custom-script-extension-overview"></a>Aangepast Script extensie-overzicht

Met de extensie aangepast Script voor Windows, kunt u PowerShell scripts uitvoeren op een externe VM zonder aanmelden bij deze. U kunt de scripts uitvoeren na het inrichten van de VM of op elk gewenst moment tijdens de levenscyclus van de VM zonder enige extra poorten openen. De meest voorkomende use-cases voor het uitvoeren van aangepaste Script extensie zijn uitgevoerd, installeren en configureren van extra software op de VM nadat deze ingericht.

### <a name="prerequisites-for-running-the-custom-script-extension"></a>Vereisten voor het uitvoeren van de uitbreiding aangepast Script

1. <a href="http://azure.microsoft.com/downloads" target="_blank">Azure PowerShell-cmdlets</a> versie 0.8.0 installeren of later.
2. Als u wilt dat de scripts worden uitgevoerd op een bestaande VM, moet dat op de VM VM-Agent is ingeschakeld. Als niet is geïnstalleerd, volgt u deze [stappen](virtual-machines-windows-classic-agents-and-extensions.md). Als de VM vanaf de Azure portal is gemaakt, wordt de VM Agent standaard geïnstalleerd.
3. De scripts die u wilt uitvoeren op de VM naar Azure opslag uploaden. De scripts kunnen afkomstig zijn uit een container met één of meerdere Opslagcontainers.
4. Het script moet worden geschreven, zodat de post script, dat door de uitbreiding is gestart, begint de andere scripts.

## <a name="custom-script-extension-scenarios"></a>Aangepast Script extensie scenario 's

### <a name="upload-files-to-the-default-container"></a>Bestanden uploaden naar de standaardcontainer

In het volgende voorbeeld ziet u hoe u kunt uw scripts uitvoeren op de VM als deze zich in de container voor opslag van de standaardaccount in uw abonnement. U kunt uw scripts uploaden naar ContainerName. U kunt controleren of de standaardaccount voor opslag met behulp van de **Get-AzureSubscription-standaard** opdracht.

In het volgende voorbeeld wordt een VM, maar u kunt ook hetzelfde scenario uitvoeren op een bestaande VM.

    # Create a new VM in Azure.
    $vm = New-AzureVMConfig -Name $name -InstanceSize Small -ImageName $imagename
    $vm = Add-AzureProvisioningConfig -VM $vm -Windows -AdminUsername $username -Password $password
    // Add Custom Script extension to the VM. The container name refers to the storage container that contains the file.
    $vm = Set-AzureVMCustomScriptExtension -VM $vm -ContainerName $container -FileName 'start.ps1'
    New-AzureVM -ServiceName $servicename -Location $location -VMs $vm
    #  After the VM is created, the extension downloads the script from the storage location and executes it on the VM.

    # Viewing the  script execution output.
    $vm = Get-AzureVM -ServiceName $servicename -Name $name
    # Use the position of the extension in the output as index.
    $vm.ResourceExtensionStatusList[i].ExtensionSettingStatus.SubStatusList

### <a name="upload-files-to-a-non-default-storage-container"></a>Bestanden uploaden naar een niet-standaard opslag container

Dit scenario laat zien hoe een niet-standaard opslag container binnen het abonnement hetzelfde of in een ander abonnement te gebruiken voor het uploaden van bestanden en scripts. In dit voorbeeld wordt een bestaande VM, maar de bewerkingen kunnen worden uitgevoerd wanneer u een VM maakt.

        Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileName 'file1.ps1','file2.ps1' -Run 'file.ps1' | Update-AzureVM

### <a name="upload-scripts-to-multiple-containers-across-different-storage-accounts"></a>Scripts uploaden naar meerdere containers in verschillende opslag rekeningen

  Als de scriptbestanden op meerdere containers zijn opgeslagen, moet u de volledige toegang tot de gedeelde handtekeningen (SAS)-URL voor de bestanden de scripts worden uitgevoerd.

      Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileUri $fileUrl1, $fileUrl2 -Run 'file.ps1' | Update-AzureVM


### <a name="add-the-custom-script-extension-from-the-azure-portal"></a>De uitbreiding aangepast Script toevoegen vanuit de Azure portal

Ga naar de VM in <a href="https://portal.azure.com/ " target="_blank">Azure portal</a> en de extensie toevoegen door te geven van het scriptbestand worden uitgevoerd.

  ![Geef de script-bestand][5]


### <a name="uninstall-the-custom-script-extension"></a>De extensie aangepast Script verwijderen

Met de volgende opdracht kunt u de extensie aangepast Script verwijderen uit de VM.

      get-azureVM -ServiceName KPTRDemo -Name KPTRDemo | Set-AzureVMCustomScriptExtension -Uninstall | Update-AzureVM

### <a name="use-the-custom-script-extension-with-templates"></a>Gebruik de extensie aangepast Script met sjablonen

Zie informatie over het gebruik van de extensie aangepast Script met Azure Resource Manager-sjablonen [met de extensie aangepast Script voor VMs Windows Azure Resource Manager-sjablonen](virtual-machines-windows-extensions-customscript.md).

<!--Image references-->
[5]: ./media/virtual-machines-windows-classic-extensions-customscript/addcse.png

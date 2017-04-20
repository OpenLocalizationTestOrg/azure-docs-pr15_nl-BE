<properties
   pageTitle="Gewenste status configuratie voor Azure overzicht | Microsoft Azure"
   description="Overzicht voor het gebruik van de Microsoft Azure extensie-handler voor de configuratie van de gewenste PowerShell staat. Met inbegrip van voorwaarden, architectuur, cmdlets..."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Inleiding tot de configuratie van Azure staat de gewenste extensie-handler #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Azure VM Agent en de bijbehorende extensies uitmaken deel van de infrastructuurservices van Microsoft Azure. VM-extensies zijn softwareonderdelen die de functionaliteit VM uitbreiden en diverse VM management bewerkingen te vereenvoudigen. Bijvoorbeeld, de VMAccess-extensie kan worden gebruikt om een administrator wachtwoord opnieuw instellen of de uitbreiding aangepast Script kan worden gebruikt voor het uitvoeren van een script op de VM.

In dit artikel wordt de extensie PowerShell gewenst staat configuratie (DSC) voor Azure VMs als onderdeel van de Azure PowerShell SDK geïntroduceerd. Nieuwe cmdlets kunt u uploaden en een PowerShell DSC-configuratie toe te passen op een Azure VM met de PowerShell DSC-extensie is ingeschakeld. De PowerShell DSC extension-aanroepen in PowerShell DSC op te nemen van de DSC-configuratie op de VM ontvangen. Deze functionaliteit is ook beschikbaar via de portal Azure.

## <a name="prerequisites"></a>Vereisten ##
**Lokale computer** Om te kunnen communiceren met de extensie Azure VM, moet u de Azure portal of de Azure PowerShell SDK gebruiken. 

**Gast-Agent** De Azure VM die wordt geconfigureerd door de DSC-configuratie moet een besturingssysteem die worden ondersteund door Windows Management Framework (WMF) 4.0 of 5.0. De volledige lijst van ondersteunde OS versies vindt u de [Versiegeschiedenis van DSC-extensie](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Termen en begrippen ##
Deze handleiding wordt ervan uitgegaan vertrouwdheid met de volgende principes:

Configuratie: een document DSC-configuratie. 

Node - een doel voor een DSC-configuratie. In dit document betrekking 'knooppunt' altijd op een Azure VM.

Configuratie - een .psd1 van gegevensbestanden met milieu-gegevens voor een configuratie

## <a name="architectural-overview"></a>Architectuur-overzicht ##

De Azure DSC-extensie wordt het kader voor Azure VM Agent leveren en rapporteren over de DSC-configuraties op Azure VMs vast. De DSC-extensie verwacht een ZIP-bestand met ten minste een configuratie document en een set parameters geleverd via de Azure PowerShell SDK of via de portal Azure.

Als de extensie voor het eerst wordt aangeroepen, wordt een installatieproces uitgevoerd. Dit proces wordt er een versie van het Windows Management Framework (WMF) met behulp van de volgende logica:

1. Als de Azure VM OS Windows Server 2016 is, wordt geen actie ondernomen. Windows Server 2016 al de nieuwste versie van PowerShell is geïnstalleerd.
2. Als de `wmfVersion` eigenschap is opgegeven, wordt die versie van de WMF tenzij onverenigbaar is met het besturingssysteem van de VM is geïnstalleerd.
3. Als er geen `wmfVersion` eigenschap is opgegeven, wordt de laatst geldende versie van de WMF is geïnstalleerd.

Installatie van de WMF moet opnieuw worden opgestart. Na opnieuw opstarten, de extensie downloaden van het ZIP-bestand dat is opgegeven in de `modulesUrl` eigenschap. Als deze locatie zich in Azure blob storage, SAS-token kan worden opgegeven in de `sasToken` eigenschap toegang tot het bestand. Nadat de .zip is gedownload en uitgepakt, wordt de configuratie functie gedefinieerd in `configurationFunction` voor het genereren van het MOF-bestand wordt uitgevoerd. De extensie voert `Start-DscConfiguration -Force` op het MOF-bestand gegenereerd. De extensie wordt vastgelegd, uitvoer en schrijft weer uit het kanaal Azure Status. Vanaf dit punt de LCM DSC zorgt voor controle en correctie als normaal. 

## <a name="powershell-cmdlets"></a>PowerShell-cmdlets ##

PowerShell-cmdlets kunnen worden gebruikt met ARM of ASM inpakken, publiceren en DSC-extensie implementaties te controleren. De volgende cmdlets vermeld zijn de ASM-modules, maar 'Azure' kan worden vervangen door "AzureRm" met het objectmodel van de ARM. Bijvoorbeeld `Publish-AzureVMDscConfiguration` maakt gebruik van ASM, waar `Publish-AzureRmVMDscConfiguration` ARM wordt gebruikt. 

`Publish-AzureVMDscConfiguration`wordt in een configuratiebestand voor afhankelijke bronnen van DSC wordt gescand en maakt een ZIP-bestand met de configuratie en de DSC-bronnen die nodig zijn om de configuratie vast. Maak ook het pakket lokaal met behulp van de `-ConfigurationArchivePath` parameter. Anders wordt het ZIP-bestand publiceert naar Azure blob-opslag en beveiligen met een SAS-token.

Het ZIP-bestand gemaakt met deze cmdlet heeft het configuratiescript .ps1 in de hoofdmap van de archiefmap. Bronnen hebben de module map in de archiefmap geplaatst. 

`Set-AzureVMDscExtension`de instellingen die nodig zijn voor de uitbreiding PowerShell DSC in een VM-configuratie-object kan vervolgens worden toegepast op een Azure VM met injects `Update-AzureVM`.

`Get-AzureVMDscExtension`de status van de DSC-extensie van een bepaalde VM opgehaald. 

`Get-AzureVMDscExtensionStatus`haalt de status van de DSC-configuratie door de handler DSC uitbreiding van kracht. Deze actie kan worden uitgevoerd op een enkele VM of groep van VMs.

`Remove-AzureVMDscExtension`Hiermee verwijdert u de extensie-handler vanaf een bepaalde virtuele machine. Deze cmdlet wordt **niet** verwijderen voor het configureren van de WMF verwijderen of wijzigen van de instellingen toegepast op de virtuele machine. Alleen de extensie-handler wordt verwijderd. 

**Belangrijke verschillen in ASM en ARM-cmdlets**

- ARM cmdlets worden synchroon. ASM-cmdlets zijn asynchroon.
- ResourceGroupName, VMName, ArchiveStorageAccountName, versie en locatie zijn alle nieuwe vereiste parameters.
- ArchiveResourceGroupName is een nieuwe optionele parameter voor de ARM. Met deze parameter kunt u opgeven wanneer de opslag account deel uitmaakt van een andere groep dan waar de virtuele machine wordt gemaakt.
- ConfigurationArchive heet ArchiveBlobName in het ARM
- ContainerName heet ArchiveContainerName in het ARM
- StorageEndpointSuffix heet ArchiveStorageEndpointSuffix in het ARM
- De schakeloptie AutoUpdate is toegevoegd aan de ARM om automatische updates van de handler voor uitbreiding naar de nieuwste versie als en wanneer deze beschikbaar is. Deze parameter heeft veroorzaken hoofdschema opnieuw is opgestart op de VM wanneer een nieuwe versie van de WMF wordt vrijgegeven. 


## <a name="azure-portal-functionality"></a>Azure portal functionaliteit ##
Blader naar een klassieke VM. -Onder Instellingen > Algemeen klikt u op 'Extensions'. Een nieuw deelvenster gemaakt. Klik op "Toevoegen" en selecteer PowerShell DSC.

De portal moet input.
**Configuratie van Modules of Script**: dit veld is verplicht. Vereist een met een configuratiescript .ps1-bestand of een ZIP-bestand met een configuratiescript .ps1 op de hoofdmap en alle afhankelijke bronnen in mappen binnen de ZIP-module. Kunnen worden gemaakt met de `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` opgenomen in de SDK van Azure PowerShell cmdlets. Het ZIP-bestand is geüpload naar de gebruiker blob-opslag beveiligd door een SAS-token. 

**PSD1 configuratiebestand**: dit veld is optioneel. Als uw configuratie een configuratiebestand van de gegevens in .psd1 is, gebruikt u dit veld en upload deze naar de gebruiker blob-opslag waar wordt beveiligd door een SAS-token. 
 
**Naam van configuratie Module-Qualified**: .ps1 bestanden kunnen meerdere functies van configuratie hebben. Voer de naam van het script voor de configuratie .ps1 gevolgd door een '\' en de naam van de functie van de configuratie. Bijvoorbeeld, als uw script .ps1 heeft de naam 'configuration.ps1' en de configuratie van de 'IisInstall' is, zou u:`configuration.ps1\IisInstall`

**Configuratie argumenten**: als de functie van de configuratie argumenten gebruikt worden, geeft u ze hier in de notatie `argumentName1=value1,argumentName2=value2`. Opmerking: deze indeling is een andere indeling dan hoe argumenten configuratie via PowerShell-cmdlets of bronnenbeheerder sjablonen worden geaccepteerd. 

## <a name="getting-started"></a>Aan de slag ##

De extensie Azure DSC in DSC configuratiedocumenten neemt en ze op Azure VMs enacts. Er volgt een eenvoudig voorbeeld van een configuratie. Dit lokaal opslaan als 'IisInstall.ps1':

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

Het script IisInstall.ps1 plaats op de opgegeven VM, de configuratie uitvoeren en rapporteren over de status van de volgende stappen uit.
 
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "demo.ps1.zip" -StorageContext $storageContext -ConfigurationName "runScript" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```

## <a name="logging"></a>Logboekregistratie ##

In worden logboeken geplaatst:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[versienummer]

## <a name="next-steps"></a>Volgende stappen ##

Voor meer informatie over PowerShell DSC, [gaat u naar het midden van de documentatie van PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Controleer de [sjabloon voor de DSC-extensie Azure Resource Manager](virtual-machines-windows-extensions-dsc-template.md
). 

Ga voor extra functionaliteit kunt u beheren met PowerShell DSC, [de PowerShell-galerie bladeren](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) naar meer bronnen van DSC.

Zie voor meer informatie over gevoelige parameters doorgeven in configuraties [beheren referenties veilig met de handler DSC-extensie](virtual-machines-windows-extensions-dsc-credentials.md).

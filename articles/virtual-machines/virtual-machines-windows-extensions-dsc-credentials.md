<properties
   pageTitle="Referenties worden doorgegeven aan DSC met Azure | Microsoft Azure"
   description="Overzicht op veilig referenties worden doorgegeven aan de Azure virtuele machines met PowerShell gewenst staat configuratie"
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

# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a>Referenties worden doorgegeven aan de handler Azure DSC-extensie #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Dit artikel heeft betrekking op de uitbreiding van de configuratie van de gewenste status voor Azure. Een overzicht van de handler DSC-extensie kunt u vinden op [Inleiding tot de configuratie van Azure staat de gewenste extensie handler](virtual-machines-windows-extensions-dsc-overview.md). 


## <a name="passing-in-credentials"></a>Referenties worden doorgegeven
Als onderdeel van de configuratie, moet u voor het instellen van gebruikersaccounts, toegang tot services of een programma installeren in de context van een gebruiker. Als u wilt doen, moet u referenties op te geven. 

DSC kunt geparametriseerde configuraties waarin referenties worden doorgegeven aan de configuratie en veilig opgeslagen in de MOF-bestanden. De Handler Azure extensie vereenvoudigt Referentiebeheer dankzij het automatische beheer van certificaten. 

Houd rekening met het volgende script uit DSC-configuratie die een lokale gebruikersaccount worden gemaakt met het opgegeven wachtwoord:

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

Het is belangrijk dat het *knooppunt localhost* opnemen als onderdeel van de configuratie. Als deze instructie niet aanwezig is, werken de volgende stappen niet als de extensie handler specifiek voor het knooppunt localhost-overzicht zoekt. Het is ook belangrijk dat u de typecast *[PsCredential]*als dit specifieke type de extensie activeert voor het coderen van de referentie. 

Dit script publiceren naar een blobopslag:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

De Azure DSC-extensie instellen en geef de referenties:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"
 
$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments
 
$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Hoe referenties worden beveiligd
Vraagt u deze code uitvoert om een referentie. Zodra deze beschikbaar is, wordt deze kort in het geheugen opgeslagen. Wanneer het is gepubliceerd met `Set-AzureVmDscExtension` cmdlet, het wordt verzonden via HTTPS voor VM, een waar Azure opgeslagen op schijf, met de lokale VM certificaat gecodeerd. Vervolgens wordt een korte gedecodeerd in het geheugen en opnieuw gecodeerd als u wilt doorgeven aan DSC.

Dit is anders dan het [gebruik van veilige configuraties zonder de extensie-handler](https://msdn.microsoft.com/powershell/dsc/securemof). De Azure-omgeving kunt de configuratiegegevens via certificaten veilig te verzenden. Wanneer u de extensie DSC handler gebruikt, is het niet nodig om $CertificatePath of een $CertificateID / $Thumbprint vermelding in de ConfigurationData.


## <a name="next-steps"></a>Volgende stappen ##

Zie voor meer informatie over de handler Azure DSC-extensie, [Inleiding tot de configuratie van Azure staat de gewenste extensie handler](virtual-machines-windows-extensions-dsc-overview.md). 

Controleer de [sjabloon voor de DSC-extensie Azure Resource Manager](virtual-machines-windows-extensions-dsc-template.md).

Voor meer informatie over PowerShell DSC, [gaat u naar het midden van de documentatie van PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Ga voor extra functionaliteit kunt u beheren met PowerShell DSC, [de PowerShell-galerie bladeren](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) naar meer bronnen van DSC.

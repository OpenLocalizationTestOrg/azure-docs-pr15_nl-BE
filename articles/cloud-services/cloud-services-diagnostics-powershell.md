<properties
    pageTitle="Diagnostiek in Azure Cloud Services met PowerShell inschakelen | Microsoft Azure"
    description="Informatie over het inschakelen van diagnostische gegevens voor cloud services met PowerShell"
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Diagnostiek in Azure Cloud Services met PowerShell inschakelen

U kunt diagnostische gegevens zoals toepassingslogboeken, verzamelen prestatiemeteritem enz. uit een bestand met de extensie Diagnostics Azure Cloud-Service. In dit artikel wordt beschreven hoe de Azure Diagnostics-extensie inschakelen voor een Cloud Service met PowerShell.  Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) op de vereisten die nodig zijn voor dit artikel.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Uitbreiding van de diagnostische gegevens als onderdeel van de implementatie van een Cloud-Service inschakelen

Deze benadering van goede voor continue integratie van scenario's waarin de uitbreiding van de diagnostische gegevens als onderdeel van de implementatie van de cloud-service kan worden ingeschakeld. Bij het maken van een nieuwe wolk Service implementeren, kunt u de extensie diagnostics inschakelen door doorgegeven in de parameter *ExtensionConfiguration* aan de cmdlet [New-AzureDeployment](https://msdn.microsoft.com/library/azure/mt589089.aspx) . De parameter *ExtensionConfiguration* neemt een matrix van configuraties met diagnostische gegevens die kunnen worden gemaakt met de cmdlet [New-AzureServiceDiagnosticsExtensionConfig](https://msdn.microsoft.com/library/azure/mt589168.aspx) .

In het volgende voorbeeld ziet u het inschakelen van diagnostische gegevens voor een cloud service met een WebRole en WorkerRole elk met een andere diagnostische configuratie.

    $service_name = "MyService"
    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

Als de diagnostische gegevens van het configuratiebestand een StorageAccount-element met de naam van een opslag, vervolgens in de cmdlet New-AzureServiceDiagnosticsExtensionConfig automatisch opslag account gebruiken. Dit werkt, moet de account van de opslag in het abonnement hetzelfde als de Cloud-Service wordt geïmplementeerd.

Doeluitvoer opgenomen van Azure SDK 2,6 geschieden de extensie-configuratiebestanden die door de MSBuild publiceren de naam van de opslag op basis van de configuratiereeks diagnostische gegevens in het configuratiebestand service (.cscfg). Het volgende script laat zien hoe u de configuratiebestanden van de extensie uit de resultaten publiceren doel parseren en uitbreiding van de diagnostische gegevens voor elke rol configureren bij het implementeren van de cloud-service.

    $service_name = "MyService"
    $service_package = "C:\build\output\CloudService.cspkg"
    $service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

    #Find the Extensions path based on service configuration file
    $extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

    $diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
    $diagnosticsConfigurations = @()
    foreach ($extPath in $diagnosticsExtensions)
    {
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
        {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
        }
    }
    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations

Visual Studio Online maakt gebruik van een soortgelijke benadering voor geautomatiseerde deploymnts van Cloud-Services met de extensie diagnostische gegevens. Zie [Publicatie-AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) voor een volledig voorbeeld.

Als er geen StorageAccount is opgegeven in de configuratie van diagnostische gegevens, moet u in de StorageAccountName-parameter doorgeven aan de cmdlet. Als de parameter StorageAccountName is opgegeven, klik in de cmdlet altijd opslag-account die is opgegeven in de parameter en niet de een die is opgegeven in het configuratiebestand van de diagnostische gegevens gebruiken.

Als de diagnostische gegevens opslag account zich in een ander abonnement van de Cloud-Service, moet u expliciet in de StorageAccountName en StorageAccountKey parameters doorgeven aan de cmdlet. De parameter StorageAccountKey is niet nodig als de diagnostische gegevens opslag account in het hetzelfde abonnement als de cmdlet automatisch kunt opvragen en de waarde van de sleutel instellen om de uitbreiding van de diagnostische gegevens. Echter, als de diagnostische gegevens opslag account in een ander abonnement, vervolgens de cmdlet mogelijk niet automatisch ophalen van de sleutel en moet u de sleutel tot en met de parameter StorageAccountKey expliciet op te geven.

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key


## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Uitbreiding van de diagnostische gegevens op een bestaande Cloud-Service inschakelen

U kunt de cmdlet [Set-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589140.aspx) inschakelen of bijwerken van diagnostische gegevens configureren op een Cloud-Service die al wordt uitgevoerd.


    $service_name = "MyService"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name


## <a name="get-current-diagnostics-extension-configuration"></a>Ophalen van de huidige configuratie van diagnostische extensie
De cmdlet [Get-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589204.aspx) gebruiken om de huidige configuratie van diagnostische gegevens voor een cloud-service.

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"

## <a name="remove-diagnostics-extension"></a>Uitbreiding van de diagnostische gegevens verwijderen
Diagnostische gegevens van een cloud-service uitschakelen als u wilt kunt u de [Software-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589183.aspx) -cmdlet.

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"

Als u de diagnostische gegevens van uitbreiding met behulp van *Set AzureServiceDiagnosticsExtension* of de *Nieuwe AzureServiceDiagnosticsExtensionConfig* zonder de parameter van de *functie* ingeschakeld kunt u de extensie *Verwijderen AzureServiceDiagnosticsExtension* gebruikt zonder de parameter *rol* verwijderen. Als de parameter *rol* werd gebruikt om de extensie moet vervolgens het ook worden gebruikt om de extensie te verwijderen.

De uitbreiding van de diagnostische gegevens van elke afzonderlijke rol verwijderen:

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"


## <a name="next-steps"></a>Volgende stappen

- Zie voor meer advies over het gebruik van Azure diagnostische gegevens en andere technieken voor het oplossen van problemen, [Diagnostische gegevens inschakelen in Azure Cloud Services en virtuele Machines](cloud-services-dotnet-diagnostics.md).
- Het [Schema van diagnostische gegevens configureren](https://msdn.microsoft.com/library/azure/dn782207.aspx) beschrijft de verschillende XML-configuraties opties voor de uitbreiding van de diagnostische gegevens.
- Zie informatie over het inschakelen van de uitbreiding van de diagnostische gegevens voor virtuele Machines, [een virtuele Windows-machine met bewaking en diagnostiek met Azure Resource Manager-sjabloon maken](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)  

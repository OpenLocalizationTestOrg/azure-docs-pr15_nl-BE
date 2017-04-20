<properties
    pageTitle="Met setup toepassing inzichten in een Azure PowerShell | Microsoft Azure"
    description="Automatiseren Azure diagnostische gegevens configureren naar de pipe naar inzichten van toepassing."
    services="application-insights"
    documentationCenter=".net"
    authors="sbtron"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/17/2015"
    ms.author="awills"/>

# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a>PowerShell gebruiken voor het instellen van de inzichten van toepassing voor een Azure web app

[Microsoft Azure](https://azure.com) kan worden [geconfigureerd voor het verzenden van Azure Diagnostics](app-insights-azure-diagnostics.md) voor [Visual Studio-toepassing inzichten](app-insights-overview.md). De diagnostische gegevens met betrekking tot Azure Cloud Services Azure VMs. Ze vormen een aanvulling op de telemetrie die u verzendt vanuit de app met de SDK van toepassing inzichten. Als onderdeel van het proces van het maken van nieuwe bronnen in Azure automatiseren, kunt u diagnostische gegevens met PowerShell.

## <a name="azure-template"></a>Azure sjabloon

Als de web app in Azure is en u uw bronnen met behulp van een sjabloon Azure Resource Manager maakt, kunt u toepassing inzichten door deze toe te voegen aan het knooppunt van de bronnen:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource`-een naam voor de bron van de inzichten van toepassing
* `myWebAppName`-de id van de web app.


## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Uitbreiding van de diagnostische gegevens als onderdeel van de implementatie van een Cloud-Service inschakelen

De `New-AzureDeployment` cmdlet heeft een parameter `ExtensionConfiguration`, die neemt een matrix van diagnostische configuraties. Deze kunnen worden gemaakt met de `New-AzureServiceDiagnosticsExtensionConfig` cmdlet. Bijvoorbeeld:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Uitbreiding van de diagnostische gegevens op een bestaande Cloud-Service inschakelen

Gebruik een bestaande service, `Set-AzureServiceDiagnosticsExtension`.

```ps
 
    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>Ophalen van de huidige configuratie van diagnostische extensie

```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Uitbreiding van de diagnostische gegevens verwijderen

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Als u de diagnostische gegevens van uitbreiding met ingeschakeld `Set-AzureServiceDiagnosticsExtension` of `New-AzureServiceDiagnosticsExtensionConfig` zonder de parameter rol vervolgens kunt u de uitbreiding met behulp van `Remove-AzureServiceDiagnosticsExtension` zonder de parameter rol. Als de parameter rol werd gebruikt om de extensie moet vervolgens het ook worden gebruikt om de extensie te verwijderen.

De uitbreiding van de diagnostische gegevens van elke afzonderlijke rol verwijderen:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Zie ook

* [Controleren van Azure Cloud Services apps met inzichten van toepassing](app-insights-cloudservices.md)
* [Azure diagnostische gegevens verzenden naar inzichten van toepassing](app-insights-azure-diagnostics.md)
* [Automatiseren van waarschuwingen configureren](app-insights-powershell-alerts.md)


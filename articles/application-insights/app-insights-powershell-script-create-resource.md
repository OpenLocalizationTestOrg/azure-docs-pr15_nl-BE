<properties 
    pageTitle="PowerShell script voor het maken van een toepassing inzichten bron" 
    description="Automatiseren van het maken van de inzichten van toepassing." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/19/2016" 
    ms.author="awills"/>

#  <a name="powershell-script-to-create-an-application-insights-resource"></a>PowerShell script voor het maken van een toepassing inzichten bron

*Inzichten van toepassing is in het afdrukvoorbeeld.*

Als u een nieuwe aanvraag- of een nieuwe versie van een toepassing - met [Inzichten van Visual Studio-toepassing](https://azure.microsoft.com/services/application-insights/)controleren wilt, stelt u een nieuwe resource in Microsoft Azure. Deze bron is waar de telemetriegegevens van uw app wordt geanalyseerd en wordt weergegeven. 

Het maken van een nieuwe resource kunt u automatiseren met behulp van PowerShell.

Bijvoorbeeld, als u een mobiele telefoon-app ontwikkelt, is waarschijnlijk dat, op elk gewenst moment, er verschillende gepubliceerde versies van uw app gebruikt door uw klanten worden. U wilt niet dat de telemetrie resultaten uit verschillende versies door elkaar halen. Zo krijgt u uw bouwproces voor elke build een nieuwe bron maken.

## <a name="script-to-create-an-application-insights-resource"></a>Script voor het maken van een toepassing inzichten bron

Raadpleeg de specificaties van de desbetreffende cmdlet:

* [Nieuwe AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [Nieuwe AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)


*PowerShell Script*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 
# - http://azure.microsoft.com/documentation/articles/azure-preview-portal-using-tags/
$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

#Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource

$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ Name = "AppInsightsApp"; Value = $applicationTagName} `
  -ResourceType "Microsoft.Insights/Components" `
  -Location "Central US" `
  -PropertyObject @{"Type"="ASP.NET"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


#Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Wat te doen met de iKey

Elke resource wordt geïdentificeerd door de sleutel instrumentation (iKey). De iKey is een uitvoer van het script voor het maken van de resource. Het script build dient dat de iKey de inzichten Application SDK ingesloten in uw app.

Er zijn twee manieren om de iKey beschikbaar te maken voor de SDK:
  
* In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
 * `<instrumentationkey>`*iKey*`</instrumentationkey>`
* Of in de [programmacode voor de initialisatie](app-insights-api-custom-events-metrics.md): 
 * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`



## <a name="see-also"></a>Zie ook

* [Toepassing inzichten en bronnen op het web test maken van sjablonen](app-insights-powershell.md)
* [Bewaking van Azure diagnostiek met PowerShell instellen](app-insights-powershell-azure-diagnostics.md) 
* [Waarschuwingen instellen met behulp van PowerShell](app-insights-powershell-alerts.md)

 
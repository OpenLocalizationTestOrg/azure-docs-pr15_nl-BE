<properties
    pageTitle="Azure REST API scenario Monitoring | Microsoft Azure"
    description="Hoe aanvragen te verifiëren en de Azure Monitoring REST-API gebruikt."
    authors="mcollier, rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="mcollier"/>

# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure REST API scenario controleren
In dit artikel wordt beschreven hoe u verificatie uitvoeren, zodat de code de [REST API Reference voor Microsoft Azure Monitor](https://msdn.microsoft.com/library/azure/dn931943.aspx)kunt gebruiken.         

De API Azure Monitor kunt u de beschikbare standaard metrische definities (het type metrische gegevens zoals CPU-tijd, aanvragen, enz.), granulatie en metrische waarden niet ophalen. Zodra opgehaald, kunnen de gegevens worden opgeslagen in een afzonderlijk gegevensarchief zoals Azure SQL-Database, DocumentDB of Azure gegevens Lake. Van daaruit kan zo nodig aanvullende analyse worden uitgevoerd.

Naast het werken met verschillende metrische gegevenspunten, zoals in dit artikel wordt beschreven, maakt de Monitor API het mogelijk om waarschuwingsregels weergave activiteitenlogboeken en nog veel meer. Zie de [Naslaginformatie over Microsoft Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx)voor een volledige lijst van beschikbare bewerkingen.

## <a name="authenticating-azure-monitor-requests"></a>Verificatie van Azure Monitor aanvragen

De eerste stap is voor de verificatie van de aanvraag.

Alle taken die worden uitgevoerd op de Monitor Azure API gebruiken het verificatiemodel Azure Resource Manager. Daarom moeten alle aanvragen worden geverifieerd met Azure Active Directory (AD Azure). Een methode voor het verifiëren van de clienttoepassing is een advertentie Azure service principal maken en verificatietoken (JWT) ophalen. Het volgende voorbeeldscript ziet een Azure AD-service principal via PowerShell maakt. Raadpleeg de documentatie over het [gebruik van Azure PowerShell om een service principal toegang krijgen tot bronnen maken](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password—powershell)voor een gedetailleerder overzicht. Het is ook mogelijk te [maken van een beginsel van de service via de portal Azure](../resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

De API van de Monitor Azure opvraagt, moet de clienttoepassing de eerder gemaakte service principal gebruiken om te verifiëren. In het volgende voorbeeld PowerShell script bevat één methode, met behulp van de [Active Directory-verificatie Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) de verificatie JWT token ophalen. Het token JWT wordt doorgegeven als onderdeel van een HTTP Authorization-parameter in aanvragen voor de Azure Monitor REST API.

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.windows.net/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)
 
$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values 
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Als de verificatie-instellingsstap voltooid is, kunnen query's tegen de Azure Monitor REST API worden uitgevoerd. Er zijn twee handige query's:

1. De metrische definities voor een resource weergeven

2. De metrische waarden ophalen


## <a name="retrieve-metric-definitions"></a>Metrische definities ophalen
>[AZURE.NOTE] Metrische definities Azure Monitor REST API gebruiken haalt met '01-03-2016' als de versie van de API.

```PowerShell
$apiVersion = "2016-03-01"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metricDefinitions?api-version=${apiVersion}"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -Verbose
```
Voor een App Azure logica lijkt de metrische definities vergelijkbaar met de volgende schermafdruk:

![ALT 'Weergave van metrische definitie antwoord JSON'.](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

Zie de [lijst van de metrische definities voor een resource in Azure Monitor REST API](https://msdn.microsoft.com/library/azure/mt743621.aspx) -documentatie voor meer informatie.

## <a name="retrieve-metric-values"></a>Metrische waarden ophalen
Zodra de beschikbare metrische definities zijn bekend, kan vervolgens de gerelateerde metrische waarden ophalen. De metric van de naam 'value' (niet de ' localizedValue') gebruiken voor alle verzoeken filteren (bijvoorbeeld 'CpuTime' en 'Vraagt' metrische gegevenspunten ophalen). Als er geen filters zijn opgegeven, wordt de standaard metrische gegevens geretourneerd.

>[AZURE.NOTE] Metrische om waarden te vinden met de Azure Monitor REST API, '2016 06-01-' als de API-versie te gebruiken.

**Methode**: ophalen

**Aanvraag-URI**: https://management.azure.com/subscriptions/_{abonnement-id}_/resourceGroups/_{bron groep naam}_/providers/_{bron provider naamruimte}_/_{type resource}_/_{Resourcenaam}_/providers/microsoft.insights/metrics?$filter=_{filter}_& api versie =_{apiVersion}_

Bijvoorbeeld voor het ophalen van de RunsSucceeded metrische gegevenspunten voor de opgegeven periode en voor een tijd korrel van 1 uur, zou de aanvraag zijn als volgt:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

Het resultaat zou worden weergegeven zoals in het voorbeeld screenshot te volgen:

![ALT "JSON antwoord met gemiddelde reactietijd van de metrische waarde"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

Voor het ophalen van meerdere gegevens of aggregatie punten toevoegen de namen van de definitie van metrische en aggregatietypen aan het filter, zoals in het volgende voorbeeld:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a>ARMClient gebruiken
Een alternatief voor het gebruik van PowerShell (zoals hierboven), is met [ARMClient](https://github.com/projectkudu/ARMClient) op uw Windows-computer. ARMClient verwerkt automatisch Azure AD-verificatie (en de resulterende JWT token). Gebruik van ARMClient voor het ophalen van metrische gegevens een overzicht van de volgende stappen uit:

1. Installeer [Chocolatey](https://chocolatey.org/) en [ARMClient](https://github.com/projectkudu/ARMClient).

2. Typ _armclient.exe aanmelding_in terminal-venster. Hiermee vraagt u aan te melden Azure.

3. Type _armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01_

4. Type _armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01_


!["ARMClient gebruiken om te werken met de Azure Monitoring REST API" ALT](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)


## <a name="retrieve-the-resource-id"></a>De Resource-ID ophalen
Met behulp van de REST-API kunt echt te begrijpen van de beschikbare metrische definities, granulatie en gerelateerde waarden. Deze informatie is handig als u met behulp van de [Bibliotheek van Azure Management](https://msdn.microsoft.com/library/azure/mt417623.aspx).

De resource-ID is het volledige pad naar de gewenste bron Azure voor de bovenstaande code. Als u wilt query uitvoeren op een Web App van Azure, kan de resource-ID zou zijn:

*/Subscriptions/{Subscription-id}/resourceGroups/{resource-Group-Name}/providers/Microsoft.Web/sites/{site-name}/*

De volgende lijst bevat enkele voorbeelden van indelingen voor resource-ID voor verschillende Azure resources:

* **IoT Hub** - /subscriptions/_{abonnement-id}_/resourceGroups/_{bron groep naam}_/providers/Microsoft.Devices/IotHubs/_{iot hub naam}_

* **Elastische SQL Pool** - /subscriptions/_{abonnement-id}_/resourceGroups/_{bron groep naam}_/providers/Microsoft.Sql/servers/_{db groep}_/elasticpools/_{sql groep naam}_

* **SQL-Database (v12)** - /subscriptions/_{abonnement-id}_/resourceGroups/_{bron groep naam}_/providers/Microsoft.Sql/servers/_{servernaam}_/databases/_{naam database}_

* **Service Bus** - /subscriptions/_{abonnement-id}_/resourceGroups/_{bron groep naam}_/providers/Microsoft.ServiceBus/_{naamruimte}_/_{servicebus naam}_

* **VM schaal Sets** - /subscriptions/_{abonnement-id}_/resourceGroups/_{bron groep naam}_/providers/Microsoft.Compute/virtualMachineScaleSets/_{vm-naam}_

* **VMs** - /subscriptions/_{abonnement-id}_/resourceGroups/_{bron groep naam}_/providers/Microsoft.Compute/virtualMachines/_{vm-naam}_

* **Gebeurtenis Hubs** - /subscriptions/_{abonnement-id}_/resourceGroups/_{bron groep naam}_/providers/Microsoft.EventHub/namespaces/_{eventhub} - naamruimte_


Er zijn alternatieve benaderingen voor het ophalen van de resource-ID, inclusief het gebruik van Azure Resource Explorer, bekijken van de gewenste resource in de portal Azure en via PowerShell of de CLI Azure.

### <a name="azure-resource-explorer"></a>Azure Resource Explorer
Een nuttige benadering is de [Azure Resource Explorer](https://resources.azure.com) gebruiken om de resource-ID voor de gewenste resource. Ga naar de gewenste bestemming en bekijk de ID weergegeven, zoals in de volgende schermafdruk:

![ALT 'Azure Resource Explorer'](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure portal
De resource-ID kan ook worden verkregen via de portal Azure. Hiertoe gaat u naar de gewenste bron en selecteer vervolgens Eigenschappen. De Resource-ID wordt weergegeven in het blad eigenschappen zoals in de volgende schermafdruk:

![ALT "Resource-ID weergegeven in de eigenschappen blade in Azure portal"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell
De resource-ID kan worden opgehaald met Azure PowerShell-cmdlets ook. Als u de resource-ID voor een Web App van Azure, voer uit de cmdlet Get-AzureRmWebApp, zoals in de volgende schermafdruk:

![ALT "Resource-ID verkregen via PowerShell"](./media\monitoring-rest-api-walkthrough\resourceid_powershell.png)

### <a name="azure-cli"></a>Azure CLI
Voor het ophalen van de resource-ID met behulp van de CLI Azure uitvoeren de opdracht 'azure webapp weergeven' opgeven van de '--json' optie, zoals in de volgende schermafdruk:

![ALT "Resource-ID verkregen via PowerShell"](./media\monitoring-rest-api-walkthrough\resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a>Activiteit logboekgegevens ophalen
Naast het werken met metrische definities en verwante waarden is het ook mogelijk extra interessante inzichten in verband met Azure resources ophalen. Als u bijvoorbeeld kan querygegevens [logboek voor faxactiviteit](https://msdn.microsoft.com/library/azure/dn931934.aspx) . In het volgende voorbeeld wordt getoond met de Azure Monitor REST API query activiteit logboekgegevens binnen een bepaald datumbereik voor een Azure-abonnement:

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a>Volgende stappen
* Bekijk het [overzicht van de controle](monitoring-overview.md).
* De [ondersteunde parameters met Azure Monitor](monitoring-supported-metrics.md)weergeven.
* Bekijk de [REST API-Naslaggids van Microsoft Azure Monitor](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Bekijk de [Azure Management Library](https://msdn.microsoft.com/library/azure/mt417623.aspx).

<properties
    pageTitle="App Service API Apps metagegevens voor het genereren van code voor het opsporen en API | Microsoft Azure"
    description="Informatie over hoe Apps in Azure App Service-API gebruiken voor metagegevens Swagger API voor het opsporen en code generatie."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>App Service API Apps metagegevens voor API voor het opsporen en code genereren 

Ondersteuning voor metagegevens van [Swagger 2.0](http://swagger.io/) API is ingebouwd in de App Service API Apps. U hebt geen Swagger gebruiken, maar als u deze gebruikt, kunt u profiteren van Apps API-functies die het opsporen en het verbruik vereenvoudigen.   

## <a name="swagger-endpoint"></a>Eindpunt swagger

Kunt u een eindpunt met Swagger 2.0 JSON-metagegevens voor een API-app in een eigenschap van de API-app. Het eindpunt kan ten opzichte van de basis-URL van de API-app of een absolute URL. Absolute URL's kunnen buiten de app API verwijzen. 

Veel downstream-clients (voor bijvoorbeeld Visual Studio code genereren en PowerApps "API toevoegen" stroom), de URL moet toegankelijk voor het publiek (niet beveiligd door een gebruiker of service verificatie). Dit betekent dat als u App Service verificatie en de definitie van de API van binnen uw app zelf beschikbaar maken, moet u de verificatieoptie waarmee anonieme verkeer uw API gebruiken. Zie [verificatie en machtiging van Apps in App Service API](app-service-api-authentication.md)voor meer informatie.

### <a name="portal-blade"></a>Portal blade

In [Azure portal](https://portal.azure.com/) kan de eindpunt-URL worden bekeken en gewijzigd in de **Definitie van API** -blade.

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Azure Resource Manager, eigenschap

U kunt ook de URL van de definitie van API voor een API-app met [Resource Explorer](https://resources.azure.com/) of [Azure Resource Manager-sjablonen](../resource-group-authoring-templates.md) in de opdrachtregelprogramma's zoals [Azure PowerShell](../powershell-install-configure.md) en de [CLI Azure](../xplat-cli-install.md). 

In de **Resource Explorer**gaat u naar **abonnementen > {uw abonnement} > resourceGroups > {de resourcegroep} > providers > Microsoft.Web > sites > {uw site} > config > web**, ziet u de `apiDefinition` eigenschap:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Voor een voorbeeld van een sjabloon die wordt ingesteld op Azure Resource Manager de `apiDefinition` eigenschap, opent u het [bestand azuredeploy.json in de voorbeeldtoepassing takenlijst](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Zoek de sectie van de sjabloon die eruit als de JSON-voorbeeld ziet hierboven is weergegeven.

### <a name="default-value"></a>Standaardwaarde

Als u Visual Studio voor het maken van een API-app, het eindpunt van de definitie van API automatisch ingesteld op de basis-URL van de API-app plus `/swagger/docs/v1`. Dit is de standaard-URL die gebruikmaakt van het pakket [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet dienen dynamisch gegenereerde Swagger metagegevens voor een project voor ASP.NET Web API. 

## <a name="code-generation"></a>Code genereren

Een van de voordelen van de integratie van Swagger in Azure API apps is automatische code genereren. Client gegenereerde klassen gemakkelijker code schrijven die een app API wordt aangeroepen.

U kunt clientcode voor een API-app genereren met behulp van Visual Studio of vanaf de opdrachtregel. Zie voor informatie over het genereren van client-klassen in Visual Studio voor een project voor ASP.NET Web API [aan de slag met API Apps en ASP.NET](app-service-api-dotnet-get-started.md#codegen). Zie het Leesmij-bestand van de opslagplaats [Azure/autorest](https://github.com/azure/autorest) op GitHub.com voor meer informatie over hoe u deze vanaf de opdrachtregel voor alle ondersteunde talen.
 
## <a name="next-steps"></a>Volgende stappen

Zie [aan de slag met API Apps in Azure App-Service](app-service-api-dotnet-get-started.md)voor een stapsgewijze zelfstudie waarin u helpt bij het maken, distribueren en gebruik van een API-app.

Als u beheer van Azure API API Apps gebruikt, kunt u metagegevens Swagger importeren de API in de beheer-API. Zie [informatie over het importeren van de definitie van een bewerkingen in Azure API beheer-API](../api-management/api-management-howto-import-api.md)voor meer informatie. 

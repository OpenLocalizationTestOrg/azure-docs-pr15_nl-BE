<properties
    pageTitle="Azure Resource Manager gebaseerde platforms opdrachtregelprogramma's voor Azure Web App | Microsoft Azure"
    description="Informatie over het gebruik van de nieuwe Azure Resource Manager gebaseerde platforms opdrachtregelprogramma's voor het beheren van uw Web-Apps Azure."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="aelnably"/>

# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-web-app"></a>Met behulp van Azure Resource Manager gebaseerde XPlat CLI voor Azure Web App#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Nieuwe opdrachten toegevoegd met de release van Microsoft Azure platforms opdrachtregelprogramma's versie 0.10.5. Deze opdrachten geven de gebruiker de mogelijkheid om op basis van de bronnenbeheerder Azure PowerShell-opdrachten gebruiken voor het beheren van Web Apps.

Meer informatie over het beheren van resourcegroepen, Zie [de CLI Azure Azure resources en resourcegroepen beheren gebruiken](../xplat-cli-azure-resource-manager.md). 


## <a name="managing-app-service-plans"></a>Plan App-Service beheren ##

### <a name="create-an-app-service-plan"></a>Een Service App maken ###
Als u wilt een serviceplan app maken, gebruikt u de opdracht **azure appserviceplan maken** .

Hieronder volgen beschrijvingen van de verschillende parameters:

-   **--resourcegroep**: bronnengroep met de zojuist gemaakte app serviceplan.
-   **--naam**: naam van de app serviceplan.
-   **--locatie**: plan de locatie van app.
-   **--laag**: de gewenste prijzen sku (de opties zijn: F1 (gratis). D1 (gedeeld). B1 (klein basis), B2 (voedingsbodem) en B3 (grote Basic). S1 (standaard klein) S2 (standaard Medium) en de S3 (standaard grote). (Klein Premium) P1, P2 (Premium gemiddeld), en P3 (grote Premium).)
-   **--exemplaren**: het aantal werknemers in de app service plan (de standaardwaarde is 1).

Bijvoorbeeld deze cmdlet gebruiken:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

### <a name="list-existing-app-service-plans"></a>Lijst plannen bestaande App ###

Als u de bestaande app serviceplannen, **lijst azure appserviceplan** opdracht te gebruiken.

U kunt alle app serviceplannen onder een bepaalde resourcegroep gebruiken:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

Als u een specifieke app serviceplan, gebruikt u de opdracht **appserviceplan azure weergeven** :

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Een bestaande plannen van App-Service configureren ###

Wijzig de instellingen voor een bestaande app serviceplan, gebruikt u de opdracht **azure appserviceplan config** . U kunt de sku en het aantal werknemers wijzigen 

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Schaal van een App-serviceplan ####

Als u wilt schalen op een bestaande App Service plannen, gebruik:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>Wijzigen van de SKU van een App-serviceplan ####

De sku van een bestaande App Service plannen met kunt u wijzigen:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Verwijder een bestaande App Service Plan ###

U verwijdert een bestaande app serviceplan, moeten alle toegewezen web apps worden verplaatst of verwijderd. Vervolgens met behulp van de opdracht **verwijderen azure webapp** dat kunt u het abonnement app verwijderen.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-web-apps"></a>Web-Apps App-Service beheren ##

### <a name="create-a-web-app"></a>Maken van een Web App. ###

Gebruik wilt maken van een web app, de opdracht **create azure webapp** .

Hieronder volgen beschrijvingen van de verschillende parameters:

- **--naam**: naam van de web app.
- **--plan**: naam voor het abonnement gebruikt voor het hosten van de web app.
- **--resourcegroep**: resourcegroep waarop het plan App-service.
- **--locatie**: de locatie van web app.

Bijvoorbeeld deze cmdlet gebruiken:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-web-app"></a>Verwijderen van een bestaande Web App ###

Verwijderen van een bestaande web app kunt u de opdracht **delete azure webapp** , moet u de naam van de web app en de naam van de resource opgeven.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-web-apps"></a>Lijst van bestaande Web Apps ###

Als u de bestaande web-apps, gebruikt u de opdracht **lijst azure webapp** .

U kunt alle web apps onder een bepaalde resourcegroep gebruiken:

    azure webapp list --resource-group ContosoAzureResourceGroup

Als u een specifieke web app, gebruikt u de opdracht **show azure webapp** .

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-web-app"></a>Een bestaand Web App configureren ###

Als u wilt wijzigen in de instellingen en configuraties voor een bestaand web app, gebruik de opdracht **set config azure webapp** .

Voorbeeld (1): wijzigen in de php-versie van een web app. 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

Voorbeeld (2): toevoegen of wijzigen van de instellingen van de app

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

Om te weten wat andere configuratie kan worden gewijzigd, gebruikt u de opdracht **azure webapp config -h instellen** .

### <a name="change-the-state-of-an-existing-web-app"></a>De status wijzigen van een bestaand Web App ###

#### <a name="restart-a-web-app"></a>Opnieuw starten van een web app. ####

Als u wilt starten van een web app, moet u de groep en de bron van de web app.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Een web app stoppen ####

Als u wilt stoppen met een web app, moet u de groep en de bron van de web app.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Start een web app. ####

U start een web app, moet u de groep en de bron van de web app.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Web App Publishing profielen beheren ###

Elke web app heeft een profiel voor publiceren die kan worden gebruikt voor het publiceren van uw toepassingen.

#### <a name="get-publishing-profile"></a>Krijg profiel ####

Als u het profiel publiceren voor een web app, gebruikt u:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Met deze opdracht een echo publishing profiel gebruikersnaam en wachtwoord aan de opdrachtregel.

### <a name="manage-web-app-hostnames"></a>Hostnamen Web App beheren ###

Gebruik de opdracht **azure webapp config hostnamen** bindingen hostnaam voor uw web app beheren  

#### <a name="list-hostname-bindings"></a>Lijst hostname bindingen ####

Als u de huidige hostname bindingen voor een web app, gebruikt u:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Hostname bindingen toevoegen ####

Hostname bindingen toevoegen aan een web app, gebruik:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Hostname bindingen verwijderen ####

Als u wilt verwijderen de hostnaam bindingen, gebruik:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

### <a name="next-steps"></a>Volgende stappen ###
- Zie voor meer informatie over de ondersteuning van Azure Resource Manager CLI [de Azure CLI gebruiken voor het beheren van Azure resources en resourcegroepen.](../xplat-cli-azure-resource-manager.md)
- Zie voor meer informatie over het beheren van App-Service met PowerShell [PowerShell naar Azure Web Apps beheren Using Azure Resource Manager-Based.](app-service-web-app-azure-resource-manager-powershell.md)

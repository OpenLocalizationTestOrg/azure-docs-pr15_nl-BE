<properties
    pageTitle="Azure PowerShell Resource Manager gebaseerde opdrachten voor Azure Web App | Microsoft Azure"
    description="Informatie over het gebruik van de nieuwe op basis van de bronnenbeheerder Azure PowerShell-opdrachten voor het beheren van uw Web-Apps Azure."
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

# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>Azure Resource Manager gebaseerde PowerShell gebruiken voor het beheren van Azure Web Apps#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Met Microsoft Azure PowerShell versie 1.0.0 zijn nieuwe opdrachten toegevoegd, die de gebruiker de mogelijkheid om op basis van de bronnenbeheerder Azure PowerShell-opdrachten gebruiken voor het beheren van Web Apps geven.

Zie meer informatie over het beheren van resourcegroepen, [Azure PowerShell Azure Resource Manager gebruiken](../powershell-azure-resource-manager.md). 

Zie de [Volledige Cmdlet referentie van PowerShell-Cmdlets met Web App Azure Resource Manager](https://msdn.microsoft.com/library/mt619237.aspx) voor meer informatie over de volledige lijst met opties voor de PowerShell-cmdlets en parameters

## <a name="managing-app-service-plans"></a>Plan App-Service beheren ##

### <a name="create-an-app-service-plan"></a>Een Service App maken ###
U maakt een serviceplan app, met de cmdlet **New-AzureRmAppServicePlan** .

Hieronder volgen beschrijvingen van de verschillende parameters:

-   **Naam**: naam van de app serviceplan.
-   **Locatie**: service plan locatie.
-   **ResourceGroupName**: bronnengroep met de zojuist gemaakte app serviceplan.
-   **Laag**: de gewenste prijzen laag (standaard is dit gratis, andere opties zijn gedeeld, Basic, Standard en Premium)
-   **WorkerSize**: de grootte van de werknemers (standaard is small als de laag-parameter is opgegeven als Basic, Standard of Premium. Andere opties zijn normaal en groot.)
-   **NumberofWorkers**: het aantal werknemers in de app service plan (de standaardwaarde is 1). 

Bijvoorbeeld deze cmdlet gebruiken:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Een Service App maken in een App-omgeving ###
Gebruik dezelfde opdracht voor **Nieuw AzureRmAppServicePlan** met extra parameters van de ASE naam en de naam van ASE resourcegroep opgeven om een serviceplan app maken in een app-omgeving.

Bijvoorbeeld deze cmdlet gebruiken:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Raadpleeg voor meer informatie over app-omgeving, [Inleiding tot App-omgeving](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Lijst plannen bestaande App ###

U kunt de bestaande app serviceplannen gebruiken de cmdlet **Get-AzureRmAppServicePlan** .

U kunt alle app serviceplannen onder uw abonnement gebruiken: 

    Get-AzureRmAppServicePlan

U kunt alle app serviceplannen onder een bepaalde resourcegroep gebruiken:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

Als u een specifieke app serviceplan, gebruikt u:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Een bestaande plannen van App-Service configureren ###

Wijzig de instellingen voor een bestaande app serviceplan, gebruikt u de cmdlet **Set-AzureRmAppServicePlan** . Kunt u de laag, de grootte van de werknemer en het aantal werknemers 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Schaal van een App-serviceplan ####

Als u wilt schalen op een bestaande App Service plannen, gebruik:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>Wijzigen van de grootte van de werknemer van een App Service Plan ####

Om de grootte van de werknemers in een bestaande App Service Plan, gebruikt u:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>Wijzigen van de laag van een App-serviceplan ####

Met kunt u de laag van een bestaande App Service plannen wijzigen:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Verwijder een bestaande App Service Plan ###

U verwijdert een bestaande app serviceplan, moeten alle toegewezen web apps worden verplaatst of verwijderd. Vervolgens met behulp van de **Software-AzureRmAppServicePlan** -cmdlet kunt u het abonnement app verwijderen.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Web-Apps App-Service beheren ##

### <a name="create-a-web-app"></a>Maken van een Web App. ###

U kunt een web app maken met de cmdlet **New-AzureRmWebApp** .

Hieronder volgen beschrijvingen van de verschillende parameters:

- **Naam**: naam voor het web app.
- **AppServicePlan**: naam voor het abonnement gebruikt voor het hosten van de web app.
- **ResourceGroupName**: resourcegroep waarop het plan App-service.
- **Locatie**: de locatie van web app.

Bijvoorbeeld deze cmdlet gebruiken:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Maken van een Web App in een App-omgeving ###

Voor het maken van een web app in een App Service omgeving (ASE). Gebruik dezelfde opdracht **Nieuw AzureRmWebApp** met extra parameters op te geven van de ASE-naam en de naam van de resource die de ASE behoort.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Raadpleeg voor meer informatie over app-omgeving, [Inleiding tot App-omgeving](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Verwijderen van een bestaande Web App ###

U kunt de cmdlet **Verwijderen AzureRmWebApp** te verwijderen, een bestaande web app, moet u de naam van de web app en de naam van de resource opgeven.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Lijst van bestaande Web Apps ###

Als u de bestaande web-apps, gebruikt u de cmdlet **Get-AzureRmWebApp** .

U kunt alle web apps onder uw abonnement gebruiken:

    Get-AzureRmWebApp

U kunt alle web apps onder een bepaalde resourcegroep gebruiken:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Als u een specifieke web app, gebruikt u:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Een bestaand Web App configureren ###

Met de cmdlet **Set-AzureRmWebApp** wijzigt u de instellingen en configuraties voor een bestaand web app. Controleer de [Cmdlet referentie koppeling](https://msdn.microsoft.com/library/mt652487.aspx) voor een volledige lijst met parameters.

Voorbeeld (1): Gebruik deze cmdlet verbindingsreeksen wijzigen

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Voorbeeld (2): toevoegen of wijzigen van de instellingen van de app

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


Voorbeeld (3): Stel de web app in 64-bits modus uit te voeren.

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>De status wijzigen van een bestaand Web App ###

#### <a name="restart-a-web-app"></a>Opnieuw starten van een web app. ####

Als u wilt starten van een web app, moet u de groep en de bron van de web app.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Een web app stoppen ####

Als u wilt stoppen met een web app, moet u de groep en de bron van de web app.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Start een web app. ####

U start een web app, moet u de groep en de bron van de web app.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Web App Publishing profielen beheren ###

Elke web app heeft een profiel voor publiceren die kan worden gebruikt voor het publiceren van uw apps, meerdere bewerkingen kunnen worden uitgevoerd op de publicatiepagina profielen.

#### <a name="get-publishing-profile"></a>Krijg profiel ####

Als u het profiel publiceren voor een web app, gebruikt u:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Deze opdracht geeft het profiel publiceren aan de opdrachtregel als ook uitvoer profiel voor het publiceren naar een tekstbestand.

#### <a name="reset-publishing-profile"></a>Opnieuw publiceren van profiel ####

Als u beide opnieuw wilt implementeren de publishing wachtwoord voor FTP- en web voor een web app, gebruik:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Web App certificaten beheren ###

Zie voor meer informatie over het beheren van certificaten voor web app [SSL-certificaten binding met PowerShell](app-service-web-app-powershell-ssl-binding.md)


### <a name="next-steps"></a>Volgende stappen ###
- Zie voor meer informatie over de ondersteuning van Azure Resource Manager PowerShell [Azure PowerShell met behulp van bronbeheer met Azure.](../powershell-azure-resource-manager.md)
- Zie voor meer informatie over Service-omgevingen App [App-omgeving-inleiding.](app-service-app-service-environment-intro.md)
- Zie voor meer informatie over het beheren van App Service SSL-certificaten met PowerShell [SSL-certificaten binding met PowerShell.](app-service-web-app-powershell-ssl-binding.md)
- Zie voor meer informatie over de volledige lijst van op basis van de bronnenbeheerder Azure PowerShell-cmdlets voor Azure Web Apps, [Azure Cmdlet referentie van Web Apps Azure PowerShell Cmdlets voor bronbeheer.](https://msdn.microsoft.com/library/mt619237.aspx)
- - Zie [Using Azure Resource Manager-Based XPlat CLI voor Azure Web App.](app-service-web-app-azure-resource-manager-xplat-cli.md) voor meer informatie over het beheren van App-Service met behulp van de CLI

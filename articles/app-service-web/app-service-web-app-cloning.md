<properties
    pageTitle="Web App klonen met PowerShell"
    description="Informatie over het kopiëren van uw Web Apps voor de nieuwe Web-Apps met PowerShell."
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
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure App Service App met PowerShell klonen#

Met de release van Microsoft Azure PowerShell versie 1.1.0 is een nieuwe optie toegevoegd aan nieuw-AzureRMWebApp die geven de gebruiker de mogelijkheid om te kopiëren van een bestaand Web App naar een nieuwe toepassing in dezelfde regio of in een andere regio. Hiermee schakelt u de klanten voor de implementatie van een aantal apps tussen de verschillende regio's snel en gemakkelijk.

Klonen van App wordt momenteel alleen ondersteund voor premium tier app serviceplannen. De nieuwe functie maakt gebruik van dezelfde beperkingen als Web Apps back-up functie, Zie [Back-up van een web app in Azure App-Service](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Als u wilt weten over het gebruik van Azure Resource Manager op basis van Azure PowerShell controleren cmdlets voor het beheren van uw Web-Apps [Azure Resource Manager op basis van PowerShell-opdrachten voor Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="cloning-an-existing-app"></a>Klonen van een bestaande App ##

Scenario: Een bestaand web app in de regio Zuid-centraal ons de gebruiker wil de inhoud naar een nieuw web app in de regio Noord centrale ons klonen. Dit kunt u doen door een nieuwe web app met de optie - SourceWebApp maken met behulp van de bronnenbeheerder Azure-versie van de PowerShell-cmdlet.

Weet de naam van de resource-groep met de bron web app, kunnen we u het volgende PowerShell de bron web app-gegevens (in dit geval de naam bron-webtoepassing):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Wilt maken op een nieuwe App-Service Plan, kunt de opdracht Nieuw AzureRmAppServicePlan zoals in het volgende voorbeeld gebruiken

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Met de opdracht Nieuw AzureRmWebApp we de nieuwe web app in de regio Noord centrale ons maken en deze koppelen aan een bestaande premium-tier App Service plannen, bovendien wij dezelfde resourcegroep gebruiken als de bron web app of een nieuwe groep te definiëren, de volgende aantoont dat:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

Kopie van een bestaande web app met inbegrip van alle bijbehorende implementatie-sleuven, de gebruiker moet de parameter IncludeSourceWebAppSlots gebruiken, de volgende PowerShell-opdracht laat zien hoe u deze parameter met de opdracht Nieuw AzureRmWebApp:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots $true

Klonen van een bestaand web app binnen dezelfde regio, de gebruiker moet een nieuwe bronnengroep en een nieuwe app-service maken plannen in hetzelfde gebied, en vervolgens met behulp van de volgende PowerShell-opdracht voor het klonen van de web app

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonen van een bestaande App naar een App-omgeving ##

Scenario: Een bestaand web app in de regio Zuid-centraal ons de gebruiker wilt klonen van inhoud naar een nieuw web app aan een bestaande App Service omgeving (ASE).

Weet de naam van de resource-groep met de bron web app, kunnen we u het volgende PowerShell de bron web app-gegevens (in dit geval de naam bron-webtoepassing):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Weet de naam van de ASE en de naam van de resource die de ASE behoort, de gebruiker kan de opdracht Nieuw AzureRmWebApp gebruiken voor het maken van de nieuwe web app in de bestaande ASE, de volgende aantoont dat:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

De parameter locatie om oude reden vereist is, maar bij het maken van een app in een ASE, wordt deze genegeerd. 

## <a name="cloning-an-existing-app-slot"></a>Klonen van een bestaande App-sleuf ##

Scenario: De gebruiker wil graag een bestaande Web App-sleuf met ofwel een nieuwe Web App of een nieuwe webtoepassing sleuf klonen. De nieuwe Web App kan worden in hetzelfde gebied, als de oorspronkelijke Web App-sleuf of in een andere regio.

Weet de naam van de resource-groep met de bron web app, kunnen we u het volgende PowerShell voor de bron web app sleuf informatie (in dit geval de naam bron webappslot) gekoppeld aan een bron-webtoepassing voor Web App:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

Het volgende voorbeeld toont een kloon van de bron web app aan een nieuwe webtoepassing maken:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Beheer van netwerkverkeer configureren tijdens het klonen van een App ##

Gebied met meerdere webtoepassingen maken en configureren van Azure verkeer Manager om het doorsturen van verkeer naar alle webtoepassingen, is een n belangrijke scenario om ervoor te zorgen dat klanten toepassingen maximaal beschikbaar zijn, bij het klonen van een bestaand web app hebt u de mogelijkheid voor zowel web-apps naar een nieuw profiel voor verkeer manager of een bestaande verbinding - merk op dat alleen Azure Resource Manager versie beheer van het verkeer wordt ondersteund.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Een nieuw profiel voor beheer van verkeer maken tijdens het klonen van een App ###

Scenario: De gebruiker wilt klonen van een web app op een ander gebied tijdens het configureren van een bronnenbeheerder Azure verkeer manager profiel waarin beide web apps. Het volgende voorbeeld toont een kloon van de bron web app aan een nieuwe webtoepassing maken tijdens het configureren van een nieuw verkeer Manager profiel:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Toevoegen van nieuwe Web App gekloond aan een bestaand profiel voor beheer van netwerkverkeer ###

Scenario: De gebruiker al een bronnenbeheerder Azure verkeer manager profiel hij graag beide web apps als eindpunten. Hiervoor moeten we eerst de bestaande verkeer manager profiel-id te assembleren, moeten we de abonnement-id, Resourcegroepnaam en bestaande verkeer profielnaam van de manager.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Nadat ik de id van de Manager verkeer, het volgende voorbeeld toont een kloon van de bron web app aan een nieuwe web app maken terwijl ze toe te voegen aan een bestaand profiel voor verkeer Manager:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Huidige beperkingen ##

Deze functie is momenteel in de voorvertoning, werken we aan nieuwe mogelijkheden toevoegen na verloop van tijd, in de volgende lijst worden de bekende beperkingen van de huidige versie van de app klonen:

- Instellingen voor automatisch schalen worden niet gekloond
- Back-upschema instellingen worden niet gekloond
- VNET-instellingen worden niet gekloond
- App inzichten worden niet automatisch ingesteld op de bestemming web app
- Eenvoudige verificatie-instellingen worden niet gekloond
- Kudu uitbreiding niet worden gekloond
- TiP-regels worden niet gekloond
- Database-inhoud niet worden gekloond


### <a name="references"></a>Verwijzingen ###
- [Azure Resource Manager op basis van PowerShell-opdrachten voor Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Web App klonen met Azure Portal](app-service-web-app-cloning-portal.md)
- [Back-up van een web app in Azure App-Service.](web-sites-backup.md)
- [Azure Resource Manager ondersteuning voor Azure verkeer Manager-voorbeeld](../../articles/traffic-manager/traffic-manager-powershell-arm.md)
- [Inleiding tot de App-omgeving](app-service-app-service-environment-intro.md)
- [Met behulp van Azure PowerShell met Azure Resource Manager](../powershell-azure-resource-manager.md)

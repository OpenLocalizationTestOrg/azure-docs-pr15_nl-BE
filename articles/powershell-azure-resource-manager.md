<properties 
    pageTitle="Azure PowerShell met Resource Manager | Microsoft Azure" 
    description="Inleiding tot het gebruik van Azure PowerShell implementatie van meerdere bronnen als resourcegroep naar Azure." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="tomfitz"/>

# <a name="using-azure-powershell-with-azure-resource-manager"></a>Met behulp van Azure PowerShell met Azure Resource Manager

> [AZURE.SELECTOR]
- [Portal](azure-portal/resource-group-portal.md) 
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [REST-API](resource-manager-rest-api.md)


Azure Resource Manager implementeert een moderne benadering van Azure resources lifecycle beheer. In plaats van het maken en beheren van afzonderlijke resources, eerst u een complete oplossing, zoals een blog, een fotocollage, een SharePoint-portal of een wiki imagining. Met een sjabloon--een declaratieve weergave van de oplossing, kunt u definiëren een bronnengroep met alle middelen die u nodig hebt ter ondersteuning van de oplossing. Vervolgens implementeren en beheren die bronnengroep als logische eenheid. 

In deze zelfstudie leert u hoe Azure PowerShell Azure Resource Manager gebruiken. Het helpt u bij het proces van het implementeren van een oplossing en het werken met deze oplossing. U kunt Azure PowerShell en een sjabloon Resource Manager implementeert:

- SQL server - als host voor de database
- SQL-database - opslaan van de gegevens
- Firewall-regels - de web app verbinding met de database toestaan
- App serviceplan - voor het definiëren van de mogelijkheden en de kosten van de web app.
- Website - voor het uitvoeren van de web app.
- Web-config - om de verbindingsreeks voor de database op te slaan 
- Waarschuwingsregels - voor prestaties en fouten controleren
- App inzichten - instellingen automatisch schalen

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie hebt u het volgende nodig:

- Een account Azure
  + U kunt [gratis een Azure account openen](/pricing/free-trial/?WT.mc_id=A261C142F): krijgt u credits kunt u uitproberen betaalde Azure services en zelfs nadat ze gebruikt omhoog kunt u de account en gebruik gratis Azure services, zoals Websites. Uw creditcard wordt nooit afgeschreven, tenzij u expliciet de instellingen wijzigen en ten laste van vragen.
  
  + Kunt u de [voordelen van MSDN subscriber activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): uw MSDN-abonnement biedt u credits elke maand die u voor betaalde Azure services gebruiken kunt.
- Azure PowerShell 1.0. Zie voor meer informatie over deze release en deze installeren [het installeren en configureren van Azure PowerShell](powershell-install-configure.md).

Deze zelfstudie is ontworpen voor beginners PowerShell, maar wordt ervan uitgegaan dat u de basisconcepten, zoals modules, cmdlets en sessies begrijpt.

## <a name="get-help-for-cmdlets"></a>Help voor cmdlets

Voor gedetailleerde informatie over de cmdlet die u in deze zelfstudie ziet, gebruikt u de cmdlet Get-Help. 

    Get-Help <cmdlet-name> -Detailed

Bijvoorbeeld, als u help voor de cmdlet Get-AzureRmResource, typt u:

    Get-Help Get-AzureRmResource -Detailed

Als u een lijst van cmdlets in de module Resources met een overzicht van de help, typt u: 

    Get-Command -Module AzureRM.Resources | Get-Help | Format-Table Name, Synopsis

De uitvoer ziet er ongeveer als het volgende fragment:

    Name                                   Synopsis
    ----                                   --------
    Find-AzureRmResource                   Searches for resources using the specified parameters.
    Find-AzureRmResourceGroup              Searches for resource group using the specified parameters.
    Get-AzureRmADGroup                     Filters active directory groups.
    Get-AzureRmADGroupMember               Get a group members.
    ...

Voor volledige informatie over een cmdlet, typt u een opdracht met de indeling:

    Get-Help <cmdlet-name> -Full
  
## <a name="login-to-your-azure-account"></a>Meld u aan bij uw account Azure

Voordat u aan uw oplossing werkt, moet u inloggen met uw account.

Als u wilt aanmelden bij uw account Azure, gebruikt u de cmdlet **Add-AzureRmAccount** .

    Add-AzureRmAccount

De cmdlet wordt u gevraagd de aanmeldingsreferenties voor de account Azure. Na het aanmelden, wordt het gedownload uw accountinstellingen zodat ze beschikbaar voor Azure PowerShell zijn. 

Accountinstellingen voor de is verlopen, zodat u ze af en toe vernieuwen moet. U vernieuwt de accountinstellingen, opnieuw **Toevoegen AzureRmAccount** uitvoeren. 

>[AZURE.NOTE] De bronnenbeheerder modules toevoegen AzureRmAccount vereist. Een bestand van de publicatie-instellingen is niet voldoende.     

Als u meer dan één abonnement hebt, bieden de abonnement-id die u wilt gebruiken voor de implementatie met de cmdlet **Set-AzureRmContext** .

    Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Bronnen om uw abonnement te implementeren, moet u een resourcegroep die de resources bevat. 

Maakt u een resourcegroep met de cmdlet **New-AzureRmResourceGroup** .

De opdracht gebruikt de parameter **naam** een naam voor de resourcegroep en de parameter **locatie** geeft u de locatie opgeven. Op basis van wat we in de vorige sectie ontdekt, gebruiken we "West-US" voor de locatie.

    New-AzureRmResourceGroup -Name TestRG1 -Location "West US"
    
De uitvoer is vergelijkbaar met:

    ResourceGroupName : TestRG1
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1

De resourcegroep heeft gemaakt.

## <a name="deploy-your-solution"></a>Uw oplossing implementeren

In dit onderwerp wordt niet laten zien hoe u de sjabloon maken of de structuur van de sjabloon te bespreken. Zie [sjablonen ontwerpen Azure Resource Manager](resource-group-authoring-templates.md) en [Resource Manager sjabloon scenario](resource-manager-template-walkthrough.md)voor die informatie. U kunt de vooraf gedefinieerde sjabloon [dient een Web App met een SQL-Database](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) van [Azure Quickstart sjablonen](https://azure.microsoft.com/documentation/templates/)gaat implementeren.

U hebt de resourcegroep en hebt u de sjabloon, zodat u nu gereed bent voor de implementatie van de infrastructuur van de sjabloon aan de resourcegroep. U implementeren resources met de cmdlet **New-AzureRmResourceGroupDeployment** . De sjabloon bevat veel standaardwaarden die we dus u geen hoeft waarden op te geven voor de parameters gebruiken. De basissyntaxis ziet:

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -administratorLogin exampleadmin -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

Geeft u de resourcegroep en de locatie van de sjabloon. Als uw sjabloon een lokaal bestand is, kunt u de parameter **- Sjabloonbestand** gebruiken en geef het pad naar de sjabloon. U kunt instellen de **-modus** parameter voor **Incrementeel** of **volledig**. Resource Manager wordt standaard een incrementele update uitgevoerd tijdens de implementatie; het is daarom niet noodzakelijk om in te stellen **-modus** waarop **Incrementeel**. Zie [een toepassing met Azure Resource Manager sjabloon distribueren](resource-group-template-deploy.md)inzicht in de verschillen tussen deze implementatiemodi. 

###<a name="dynamic-template-parameters"></a>Dynamische Sjabloonparameters

Als u bekend met PowerShell bent, weet u dat u door de beschikbare parameters voor een cmdlet bladeren kunt met een minteken (-) te typen en vervolgens op de TAB-toets te drukken. Deze functionaliteit werkt ook met de parameters die u in de sjabloon opgeeft. Als u typt de naam van de sjabloon, de cmdlet haalt de sjabloon parseert het en wordt de Sjabloonparameters aan de opdracht dynamisch. Hierdoor kunt heel gemakkelijk de parameterwaarden voor de sjabloon opgeven.

Wanneer u de opdracht opgeeft, wordt u gevraagd de ontbrekende verplichte parameter **administratorLoginPassword**. En als u het wachtwoord typt, de beveiligde tekenreekswaarde wordt verborgen. Deze strategie elimineert het risico van een wachtwoord in onbewerkte tekst.

    cmdlet New-AzureRmResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    administratorLoginPassword: ********

Als de sjabloon bevat een parameter met een naam die overeenkomt met een van de parameters in de opdracht voor de implementatie van de sjabloon (bijvoorbeeld ook een parameter met de naam **ResourceGroupName** in de sjabloon die hetzelfde als de parameter **ResourceGroupName** in de cmdlet [New-AzureRmResourceGroupDeployment is](https://msdn.microsoft.com/library/azure/mt679003.aspx) ), wordt u gevraagd om een waarde opgeven voor een parameter met de postfix- **FromTemplate** (bijvoorbeeld **ResourceGroupNameFromTemplate**). In het algemeen, voorkomt u deze verwarring door de naam van parameters niet met dezelfde naam als de parameters die worden gebruikt voor de implementatie.

De opdracht wordt uitgevoerd en worden berichten geretourneerd als de resources worden gemaakt. Uiteindelijk ziet u het resultaat van de implementatie.

    DeploymentName    : azuredeploy
    ResourceGroupName : TestRG1
    ProvisioningState : Succeeded
    Timestamp         : 4/11/2016 7:26:11 PM
    Mode              : Incremental
    TemplateLink      :
                Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json
                ContentVersion : 1.0.0.0
    Parameters        :
                Name             Type                       Value
                ===============  =========================  ==========
                skuName          String                     F1
                skuCapacity      Int                        1
                administratorLogin  String                  exampleadmin
                administratorLoginPassword  SecureString
                databaseName     String                     sampledb
                collation        String                     SQL_Latin1_General_CP1_CI_AS
                edition          String                     Basic
                maxSizeBytes     String                     1073741824
                requestedServiceObjectiveName  String       Basic

    Outputs           :
                Name             Type                       Value
                ===============  =========================  ==========
                siteUri          String                     websites5wdai7p2k2g4.azurewebsites.net
                sqlSvrFqdn       String                     sqlservers5wdai7p2k2g4.database.windows.net
                    
    DeploymentDebugLogLevel :

In een paar stappen we hebben gemaakt en de bronnen die nodig zijn voor een complexe website geïmplementeerd. 

### <a name="log-debug-information"></a>Logboek voor foutopsporingsinformatie

Bij het implementeren van een sjabloon, kunt u meer informatie over de aanvraag en het antwoord registreren door de parameter **- DeploymentDebugLogLevel** op te geven wanneer een **Nieuw AzureRmResourceGroupDeployment**wordt uitgevoerd. Deze informatie kunt u fouten implementatie. De standaardwaarde is **geen** wat betekent dat er geen aanvraag of antwoord inhoud wordt vastgelegd. U kunt logboekregistratie van de inhoud van een aanvraag of antwoord.  Zie voor meer informatie over het oplossen van implementaties en Foutopsporingsgegevens vastleggen [Troubleshooting resource groep implementaties met Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md). In het volgende voorbeeld registreert de inhoud van de aanvraag en de inhoud van de reactie voor de implementatie.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -DeploymentDebugLogLevel All -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

> [AZURE.NOTE] Als u de parameter DeploymentDebugLogLevel, Overweeg zorgvuldig het type informatie dat u aan tijdens de implementatie doorgeeft. Door de registratie-informatie over de aanvraag of het antwoord, kan u blootstellen gevoelige gegevens die worden opgehaald via de implementatie van bewerkingen. 


## <a name="get-information-about-your-resource-groups"></a>Informatie over de resourcegroepen

Na het maken van een resourcegroep, kunt u de in de module Resource Manager-cmdlets voor het beheren van de bronnengroepen.

- Als u een groep in uw abonnement, gebruikt u de cmdlet **Get-AzureRmResourceGroup** :

        Get-AzureRmResourceGroup -ResourceGroupName TestRG1
    
    Waar wordt de volgende informatie:

        ResourceGroupName : TestRG1
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG
        
        ...

    Als u de naam van een resource niet opgeeft, retourneert de cmdlet alle de resourcegroepen in uw abonnement.

- Als u de resources in de resourcegroep, gebruikt u de cmdlet **Zoeken naar AzureRmResource** en de parameter **ResourceGroupNameContains** . Zonder parameters haalt zoeken AzureRmResource alle resources in uw abonnement Azure.

        Find-AzureRmResource -ResourceGroupNameContains TestRG1
    
     Dat geeft als resultaat een lijst met resources indeling als volgt:
        
        Name              : sqlservers5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Sql/servers/sqlservers5wdai7p2k2g4
        ResourceName      : sqlservers5wdai7p2k2g4
        ResourceType      : Microsoft.Sql/servers
        Kind              : v2.0
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
        Tags              : {System.Collections.Hashtable}
        ...
            
- U kunt tags logisch ordenen van de resources in uw abonnement en resources met de cmdlets **Zoeken naar AzureRmResource** en **Zoek AzureRmResourceGroup** ophalen.

        Find-AzureRmResource -TagName displayName -TagValue Website

        Name              : webSites5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Web/sites/webSites5wdai7p2k2g4
        ResourceName      : webSites5wdai7p2k2g4
        ResourceType      : Microsoft.Web/sites
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
                
      There is much more you can do with tags. For more information, see [Using tags to organize your Azure resources](resource-group-using-tags.md).

## <a name="add-to-a-resource-group"></a>Toevoegen aan een resourcegroep

Als u een resource toevoegt aan de resourcegroep, kunt u de cmdlet **New-AzureRmResource** . Echter, toevoegen van een resource op deze manier kan wel verwarring toekomstige omdat de nieuwe resource niet in de sjabloon bestaat. Als u de oude sjabloon opnieuw geïmplementeerd, zou u een onvolledige oplossing implementeren. Als u vaak implementeert, zult u het gemakkelijker en betrouwbaarder de nieuwe resource toevoegen aan de sjabloon en opnieuw te implementeren.

## <a name="move-a-resource"></a>Een resource verplaatsen

U kunt bestaande resources verplaatsen naar een nieuwe resourcegroep. Zie voor voorbeelden van [Resources verplaatsen naar een nieuwe resourcegroep of abonnement](resource-group-move-resources.md).

## <a name="export-template"></a>Sjabloon exporteren

Voor een bestaande resourcegroep (geïmplementeerd via PowerShell of een van de andere methoden zoals de portal), kunt u de sjabloon Resource Manager voor de resourcegroep bekijken. De sjabloon exporteren biedt twee voordelen:

1. Omdat alle van de infrastructuur is gedefinieerd in de sjabloon kunt u eenvoudig de toekomstige implementaties van de oplossing automatiseren.

2. U kunt bekend met de sjabloonsyntaxis worden door te kijken op JavaScript Object Notation (JSON) die uw oplossing vertegenwoordigt.

Via de PowerShell, kunt u een sjabloon waarmee de huidige status van de resourcegroep genereren of ophalen van de sjabloon die is gebruikt voor een bepaalde implementatie.

Exporteren van de sjabloon voor een resourcegroep is handig als u wijzigingen hebt aangebracht in een resourcegroep en moet de JSON representatie van de huidige toestand te halen. De gegenereerde sjabloon bevat echter alleen een minimum aantal parameters en geen variabelen. De meeste van de waarden in de sjabloon zijn hardcoded. Voordat u de gegenereerde sjabloon implementeert, wilt u mogelijk meer waarden converteren naar parameters, zodat u de implementatie voor verschillende omgevingen kan aanpassen.

Exporteren van de sjabloon voor een bepaalde implementatie is handig als u nodig hebt om de werkelijke sjabloon die is gebruikt voor het implementeren van resources weer te geven. De sjabloon bevat alle parameters en variabelen die zijn gedefinieerd voor de oorspronkelijke implementatie. Echter, als iemand in uw organisatie wijzigingen in de resourcegroep aangebracht heeft buiten de waarde die is gedefinieerd in de sjabloon, deze sjabloon niet staat voor de huidige status van de resourcegroep.

> [AZURE.NOTE] De functie van de sjabloon exporteren in het voorbeeld is en niet alle typen momenteel ondersteuning voor het exporteren van een sjabloon. Bij het exporteren van een sjabloon, wordt er een fout gemeld dat sommige bronnen zijn niet geëxporteerd. Indien nodig kunt handmatig u deze resources aan uw sjabloon na het downloaden.

###<a name="export-template-from-resource-group"></a>Sjabloon exporteren uit resourcegroep

Als u wilt weergeven in de sjabloon voor een resourcegroep, de cmdlet **Export-AzureRmResourceGroup** worden uitgevoerd.

    Export-AzureRmResourceGroup -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\TestRG1.json
    
###<a name="download-template-from-deployment"></a>Sjabloon downloaden van implementatie

Downloaden van de sjabloon die wordt gebruikt voor een bepaalde implementatie van de cmdlet **Opslaan AzureRmResourceGroupDeploymentTemplate** worden uitgevoerd.

    Save-AzureRmResourceGroupDeploymentTemplate -DeploymentName azuredeploy -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\azuredeploy.json

## <a name="delete-resources-or-resource-group"></a>Resources of resourcegroepen verwijderen

- Als een resource verwijderen uit de resourcegroep, gebruikt u de cmdlet **Verwijderen AzureRmResource** . Deze cmdlet wordt verwijderd van de bron, maar verwijdert u de resourcegroep.

    Met deze opdracht verwijdert u de TestSite website uit de TestRG1.

        Remove-AzureRmResource -Name TestSite -ResourceGroupName TestRG1 -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01

- Als u wilt verwijderen van een resourcegroep, gebruikt u de cmdlet **Verwijderen AzureRmResourceGroup** . Deze cmdlet Hiermee verwijdert u de resourcegroep en de bijbehorende bronnen.

        Remove-AzureRmResourceGroup -Name TestRG1
        
    U wordt gevraagd het verwijderen te bevestigen.
        
        Confirm
        Are you sure you want to remove resource group 'TestRG1'
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y

## <a name="deployment-script"></a>Script voor implementatie

De eerdere implementatie voorbeelden in dit onderwerp bleek alleen de afzonderlijke cmdlets die nodig zijn voor resources implementeren op Azure. In het volgende voorbeeld ziet u een implementatiescript dat wordt gemaakt van de resourcegroep en implementeert u de resources.

    <#
      .SYNOPSIS
      Deploys a template to Azure
      
      .DESCRIPTION
      Deploys an Azure Resource Manager template

      .PARAMETER subscriptionId
      The subscription id where the template will be deployed.

      .PARAMETER resourceGroupName
      The resource group where the template will be deployed. Can be the name of an existing or a new resource group.

      .PARAMETER resourceGroupLocation
      Optional, a resource group location. If specified, will try to create a new resource group in this location. If not specified, assumes resource group is existing.

      .PARAMETER deploymentName
      The deployment name.

      .PARAMETER templateFilePath
      Optional, path to the template file. Defaults to template.json.

      .PARAMETER parametersFilePath
      Optional, path to the parameters file. Defaults to parameters.json. If file is not found, will prompt for parameter values based on template.
    #>

    param(
      [Parameter(Mandatory=$True)]
      [string]
      $subscriptionId,

      [Parameter(Mandatory=$True)]
      [string]
      $resourceGroupName,

      [string]
      $resourceGroupLocation,

      [Parameter(Mandatory=$True)]
      [string]
      $deploymentName,

      [string]
      $templateFilePath = "template.json",

      [string]
      $parametersFilePath = "parameters.json"
    )

    #******************************************************************************
    # Script body
    # Execution begins here 
    #******************************************************************************
    $ErrorActionPreference = "Stop"

    # sign in
    Write-Host "Logging in...";
    Add-AzureRmAccount;

    # select subscription
    Write-Host "Selecting subscription '$subscriptionId'";
    Set-AzureRmContext -SubscriptionID $subscriptionId;

    #Create or check for existing resource group
    $resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
    if(!$resourceGroup)
    {
      Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
      if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
      }
      Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
      New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
    }
    else{
      Write-Host "Using existing resource group '$resourceGroupName'";
    }

    # Start the deployment
    Write-Host "Starting deployment...";
    if(Test-Path $parametersFilePath) {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
    } else {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
    }

## <a name="next-steps"></a>Volgende stappen

- Zie meer informatie over het maken van sjablonen voor bronbeheer, [Azure Resource Manager sjablonen ontwerpen](./resource-group-authoring-templates.md).
- Zie meer informatie over het implementeren van sjablonen, [een toepassing met Azure Resource Manager sjabloon distribueren](./resource-group-template-deploy.md).
- Zie voor een gedetailleerd voorbeeld van een project te implementeren [distribueren microservices voorspelbaar in Azure](app-service-web/app-service-deploy-complex-application-predictably.md).
- Meer informatie over het oplossen van een installatie is mislukt, Zie [Troubleshooting resource groep implementaties in Azure](./resource-manager-troubleshoot-deployments-powershell.md).


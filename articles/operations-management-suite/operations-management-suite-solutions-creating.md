<properties
   pageTitle="Oplossingen maken in het Operations Management Suite (OMS) | Microsoft Azure"
   description="De functionaliteit van Operations Management Suite (OMS) uitbreiden oplossingen door middel van verpakte beheerscenario die klanten aan hun OMS-werkruimte kunnen toevoegen.  Dit artikel bevat informatie over het maken van oplossingen te gebruiken in uw eigen omgeving of beschikbaar gesteld aan uw klanten."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="creating-management-solutions-in-operations-management-suite-oms-preview"></a>Oplossingen maken in Operations Management Suite (OMS) (voorbeeld)

>[AZURE.NOTE]Dit is voorlopige documentatie voor het maken van oplossingen in OMS die momenteel in de voorvertoning. De hieronder beschreven schema kan worden gewijzigd.  

De functionaliteit van Operations Management Suite (OMS) uitbreiden oplossingen door middel van verpakte beheerscenario die klanten aan hun OMS-werkruimte kunnen toevoegen.  Dit artikel bevat informatie over het maken van uw eigen oplossingen te gebruiken in uw eigen omgeving of beschikbaar te stellen aan klanten door de Gemeenschap.

## <a name="planning-your-management-solution"></a>Oplossing voor uw planning
OMS oplossingen voor systeembeheer omvatten meerdere resources een scenario van bepaalde management ondersteunende.  Bij het plannen van uw oplossing, moet u zich richten op het scenario dat u wilt bereiken en alle vereiste resources te ondersteunen.  Elke oplossing moet worden opgenomen en definiëren van elke bron die er, zelfs als een of meer resources worden ook bepaald door andere oplossingen.  Wanneer een oplossing is geïnstalleerd, wordt elke resource gemaakt, tenzij deze al bestaat en u wat er gebeurt met resources bepalen kunt als een oplossing wordt verwijderd.  

Een oplossing kan bijvoorbeeld een [Azure automatisering runbook](../automation/automation-intro.md) waarmee gegevens naar het logboek Analytics opslagplaats met behulp van een [planning](../automation/automation-schedules.md) en een [weergave](../log-analytics/log-analytics-view-designer.md) verschillende visualisaties van de verzamelde gegevens biedt worden verzameld.  Hetzelfde schema kan worden gebruikt door een andere oplossing.  Als de auteur van de management-oplossing, zou u alle drie resources definiëren maar opgeven dat de runbook en de weergave moeten worden automatisch verwijderd als de oplossing wordt verwijderd.    U zou ook de planning definiëren maar dat het blijven moet als de oplossing voor het geval was het nog in gebruik zijn verwijderd door de oplossing van andere.

## <a name="management-solution-files"></a>Management solution-bestanden
Oplossingen worden geïmplementeerd als [sjablonen voor resourcebeheer](../resource-manager-template-walkthrough.md).  De belangrijkste taak bij het leren van de oplossingen ontwerpen is leren hoe u [de auteur van een sjabloon](../resource-group-authoring-templates.md).  Dit artikel bevat een unieke details van sjablonen voor oplossingen en het definiëren van de standaardoplossing bronnen gebruikt.

De basisstructuur van een management solution-bestand is hetzelfde als een [Sjabloon voor bronbeheer](resource-group-authoring-templates.md#template-format) als volgt is.  Elk van de volgende secties wordt beschreven met de elementen van het hoogste niveau en en hun inhoud in een oplossing.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parameters

[Parameters](../resource-group-authoring-templates.md#parameters) zijn de waarden die u nodig hebt van de gebruiker bij de installatie van de oplossing.  Er zijn standaard parameters die u alle oplossingen hebt en kunt u extra parameters toevoegen zoals vereist voor uw specifieke oplossing.  Hoe gebruikers parameterwaarden worden opgeven bij de installatie van uw oplossing wordt afhankelijk van de desbetreffende parameter en de manier waarop de oplossing wordt geïnstalleerd.


Wanneer een gebruiker uw beheeroplossing via de [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-solutions) of [Azure QuickStart sjablonen](operations-management-suite-solutions.md#finding-and-installing-solutions) installeert wordt gevraagd of u een [werkruimte OMS en automatisering account](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account)selecteren.  Deze worden gebruikt voor het vullen van de waarden van elk van de standaard parameters.  De gebruiker wordt niet gevraagd om direct waarden voor de standaard parameters, maar ze worden gevraagd waarden op te geven voor eventuele extra parameters.

Wanneer de gebruiker de oplossing [een andere methode](operations-management-suite-solutions.md#finding-and-installing-solutions)installeert, moeten zij een waarde opgeven voor alle standaard parameters en alle aanvullende parameters.

Hieronder vindt u een voorbeeld-parameter.

    "Daily Start Time": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

De volgende tabel beschrijft de kenmerken van een parameter.

| Kenmerk | Beschrijving |
|:--|:--|
| type        | Het gegevenstype voor de parameter. Het besturingselement voor tekstinvoer weergegeven voor de gebruiker, is afhankelijk van het gegevenstype.<br><br>BOOL - vervolgkeuzelijst<br>tekenreeks - tekstvak<br>int - tekstvak<br>SecureString - wachtwoordveld<br> |
| categorie    | De categorie voor de parameter is optioneel.  Parameters in dezelfde categorie worden gegroepeerd. |
| besturingselement     | Aanvullende functionaliteit voor tekenreeksparameters.<br><br>datum/tijd - datum/tijd-besturingselement wordt weergegeven.<br>GUID - Guid-waarde wordt automatisch gegenereerd en de parameter niet wordt weergegeven. |
| Beschrijving | Optionele beschrijving voor de parameter.  Weergegeven in een tekstballon met informatie naast de parameter. |


### <a name="standard-parameters"></a>Standaard parameters
De volgende tabel worden de standaardparameters voor alle oplossingen.  Deze waarden worden ingevuld voor de gebruiker vragen om ze als uw oplossing uit de Quickstart sjablonen of Azure Marketplace is geïnstalleerd.  De gebruiker moet waarden opgeven voor hen als de oplossing is geïnstalleerd met een andere methode.

>[AZURE.NOTE]De gebruikersinterface in de Marketplace Azure en Quickstart sjablonen verwacht dat de namen van de parameters in de tabel.  Als u namen van verschillende parameters wordt de gebruiker gevraagd voor hen en ze wordt niet automatisch ingevuld.


| Parameter | Type | Beschrijving |
|:--|:--|:--|
| Accountnaam       | tekenreeks |  Azure automatisering accountnaam. |
| pricingTier       | tekenreeks | Prijzen laag zowel logboek Analytics werkruimte en account Azure automatisering. |
| regionId          | tekenreeks | De regio van de automatisering van Azure-account. |
| Naam oplossing      | tekenreeks | De naam van de oplossing. |
| Werkruimtenaam     | tekenreeks | Logboeknaam Analytics werkruimte. |
| workspaceRegionId | tekenreeks | De regio van de werkruimte logboek Analytics. |





### <a name="sample"></a>Monster
Hier volgt een entiteit monster parameter voor een oplossing.  Dit omvat alle van de standaard parameters en twee extra parameters in dezelfde categorie.

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "A valid Azure Automation account name"
            }
        },
        "workspaceRegionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        },
        "jobIdGuid": {
        "type": "string",
            "metadata": {
                "description": "GUID for a runbook job",
                "control": "guid",
                "category": "Schedule"
            }
        },
        "startTime": {
            "type": "string",
            "metadata": {
                "description": "Time for starting the runbook.",
                "control": "datetime",
                "category": "Schedule"
            }
        }


U verwijst naar de waarden van de parameters in andere onderdelen van de oplossing met de syntaxis **parameters (parameternaam)**.  Bijvoorbeeld, als u de naam van de werkruimte, gebruikt u **parameters('workspaceName')** 

## <a name="variables"></a>Variabelen

Het element **variabelen** bevat waarden die u wilt gebruiken in de rest van de oplossing.  Deze waarden worden niet blootgesteld aan de gebruiker de oplossing te installeren.  Ze zijn bedoeld om de auteur van het voorzien van een enkele locatie waar ze waarden die meerdere keren kunnen worden gebruikt in de oplossing kunnen beheren. U moet waarden specifieke plaatsen aan uw oplossing in variabelen niet hardcoding ze in het element **bronnen** .  Dit maakt de code beter leesbaar en u kunt deze waarden in hogere versies gemakkelijk wijzigen.

Hier volgt een voorbeeld van een element van de **variabelen** met de typische parameters die worden gebruikt in oplossingen.

    "variables": { 
        "SolutionVersion": "1.1", 
        "SolutionPublisher": "Contoso", 
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

U verwijst naar de waarden van variabelen door middel van de oplossing met de syntaxis van **variabelen ('variabele naam')**.  Bijvoorbeeld voor toegang tot de variabele naam oplossing, gebruikt u **variables('solutionName')** 


## <a name="resources"></a>Bronnen

Het element **resources** bepaalt de verschillende bronnen in uw oplossing is opgenomen.  Dit is het grootste en meest complexe deel van de sjabloon.  Resources worden gedefinieerd met de volgende structuur.  

    "resources": [
        {
            "name": "<name-of-the-resource>",           
            "apiVersion": "<api-version-of-resource>",
            "type": "<resource-provider-namespace/resource-type-name>",     
            "location": "<location-of-resource>",
            "tags": "<name-value-pairs-for-resource-tagging>",
            "comments": "<your-reference-notes>",
            "dependsOn": [
                "<array-of-related-resource-names>"
            ],
            "properties": "<unique-settings-for-the-resource>",
            "resources": [
                "<array-of-child-resources>"
            ]
        }
    ]

### <a name="dependencies"></a>Afhankelijkheden
**DependsOn** elementen bevat een [afhankelijkheid](../resource-group-define-dependencies.md) op een ander middel.  Wanneer de oplossing is geïnstalleerd, wordt een resource wordt niet gemaakt totdat alle bijbehorende afhankelijkheden zijn gemaakt.  Bijvoorbeeld, kan uw oplossing [starten een runbook](operations-management-suite-solutions-resources-automation.md#runbooks) wordt geïnstalleerd met behulp van een [taak, resource](operations-management-suite-solutions-resources-automation.md#automation-jobs).  De resource voor de taak, zijn afhankelijk van de bron van runbook om ervoor te zorgen dat de runbook is gemaakt voordat de taak is gemaakt.

### <a name="oms-workspace-and-automation-account"></a>OMS-werkruimte en account automatisering
Oplossingen vereisen een [werkruimte OMS](../log-analytics/log-analytics-manage-access.md) weergaven bevatten en een [account voor automatisering](../automation/automation-security-overview.md#automation-account-overview) runbooks en verwante bronnen bevat.  Deze moeten beschikbaar zijn voordat de resources in de oplossing zijn gemaakt en niet in de oplossing zelf moeten worden gedefinieerd.  De gebruiker wordt [een werkruimte en account opgeven](operations-management-suite-solutions.md#oms-workspace-and-automation-account) bij het implementeren van uw oplossing, maar als de auteur moet u rekening houden met de volgende punten.


## <a name="solution-resource"></a>Bron van de oplossing
Elke oplossing moet een bron in de **bronnen** -element waarin de oplossing zelf.  Dit heeft een soort **Microsoft.OperationsManagement/solutions**.  Hier volgt een voorbeeld van een bron oplossing.  De verschillende elementen worden in de volgende secties beschreven.

    "name": "[concat(variables('SolutionName'), '[ ' ,parameters('workspacename'), ' ]')]",
    "location": "[parameters('workspaceRegionId')]",
    "tags": { },
    "type": "Microsoft.OperationsManagement/solutions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]",
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
    ]
    "properties": {
        "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
        "referencedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]"
        ],
        "containedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
        ]
    },
    "plan": {
        "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
        "Version": "[variables('SolutionVersion')]",
        "product": "AzureSQLAnalyticSolution",
        "publisher": "[variables('SolutionPublisher')]",
        "promotionCode": ""
    }

### <a name="solution-name"></a>Oplossingsnaam
De naam van de oplossing moet uniek zijn in uw abonnement Azure. De aanbevolen waarde te gebruiken is het volgende.  Dit wordt een variabele met de naam **naam oplossing** voor de naam en de parameter **Werkruimtenaam** gebruikt om ervoor te zorgen dat de naam uniek is.

    [concat(variables('SolutionName'), ' [' ,parameters('workspaceName'), ']')]

Dit zou een naam als volgt oplossen.

    My Solution Name [MyWorkspace]
 

### <a name="dependencies"></a>Afhankelijkheden
De bron van de oplossing moet een [afhankelijkheid](../resource-group-define-dependencies.md) hebben op alle andere bronnen in de oplossing omdat ze al bestaan moeten voordat de oplossing kan worden gemaakt.  Hiervoor een vermelding voor elke resource in het **dependsOn** element toe te voegen.


### <a name="properties"></a>Eigenschappen
De resource oplossing heeft de eigenschappen in de volgende tabel.  Dit omvat de bronnen wordt verwezen en die door de oplossing waarin wordt gedefinieerd hoe de resource na de installatie van de oplossing wordt beheerd.  Elke resource in de oplossing moet worden vermeld in de **referencedResources** of de eigenschap **containedResources** .

| Eigenschap | Beschrijving |
|:--|:--|
| workspaceResourceId | ID van de werkruimte OMS in de vorm * <Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<werkruimte naam\>*. |
| referencedResources   | De lijst met resources voor de oplossing die niet moet worden verwijderd als de oplossing is verwijderd. |
| containedResources   | De lijst met resources voor de oplossing die moet worden verwijderd als de oplossing is verwijderd. |

In het bovenstaande voorbeeld is voor een oplossing met een runbook, een schema en de weergave.  Het schema en de runbook zijn *waarnaar wordt verwezen* in de **Eigenschappen van** element, zodat ze niet worden verwijderd wanneer de oplossing is verwijderd.  De weergave is *opgenomen* wordt verwijderd als de oplossing wordt verwijderd.


### <a name="plan"></a>Plan
De entiteit **planning** van de resource oplossing heeft de eigenschappen in de volgende tabel. 

| Eigenschap | Beschrijving |
|:--|:--|
| naam          | De naam van de oplossing. |
| Versie       | De versie van de oplossing zoals bepaald door de auteur. |
| product       | Unieke tekenreeks voor het identificeren van de oplossing. |
| Publisher     | De uitgever van de oplossing. |


## <a name="other-resources"></a>Andere bronnen
U kunt de details en voorbeelden van bronnen die gemeenschappelijk voor oplossingen in de volgende artikelen zijn.

- [Weergaven en dashboards](operations-management-suite-solutions-resources-views.md)
- [Bronnen voor automatisering](operations-management-suite-solutions-resources-automation.md)

## <a name="testing-a-management-solution"></a>Testen van een oplossing voor beheer
Voordat u uw oplossing implementeert, is het raadzaam u testen met behulp van [Test-en AzureRmResourceGroupDeployment](../resource-group-template-deploy.md#deploy-with-powershell).  Dit zal valideren uw oplossingsbestand en help die u problemen identificeren voordat u probeert te implementeren.



## <a name="next-steps"></a>Volgende stappen

- Informatie over de details van [sjablonen ontwerpen Azure Resource Manager](../resource-group-authoring-templates.md).
- Zoeken [Azure Quickstart sjablonen](https://azure.microsoft.com/documentation/templates) voor voorbeelden van verschillende sjablonen voor Resource Manager.
- De details weergeven voor [weergaven naar een oplossing toe te voegen](operations-management-suite-solutions-resources-views.md).
- Bekijk de details voor het [toevoegen van bronnen aan een oplossing voor automatisering](operations-management-suite-solutions-resources-automation.md).
 
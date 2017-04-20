<properties
   pageTitle="Bronnen voor automatisering in OMS oplossingen | Microsoft Azure"
   description="Oplossingen in OMS bevatten meestal runbooks in Azure automatisering voor het automatiseren van processen, zoals het verzamelen en verwerken van meetgegevens.  In dit artikel wordt beschreven hoe runbooks en de bijbehorende resources opnemen in een oplossing."
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
   ms.date="10/19/2016"
   ms.author="bwren" />

# <a name="automation-resources-in-oms-solutions-preview"></a>Bronnen voor automatisering in OMS oplossingen (voorbeeld)

>[AZURE.NOTE]Dit is voorlopige documentatie voor het maken van oplossingen in OMS die momenteel in de voorvertoning. De hieronder beschreven schema kan worden gewijzigd.   

[Oplossingen in OMS](operations-management-suite-solutions.md) bevatten meestal runbooks in Azure automatisering voor het automatiseren van processen, zoals het verzamelen en verwerken van meetgegevens.  Automatisering accounts bevat naast de runbooks, activa, zoals variabelen en schema's die ondersteuning bieden voor de runbooks die wordt gebruikt in de oplossing.  In dit artikel wordt beschreven hoe runbooks en de bijbehorende resources opnemen in een oplossing.

>[AZURE.NOTE]De voorbeelden in dit artikel gebruiken parameters en variabelen die zijn vereist of gemeenschappelijke oplossingen voor beheer en wordt beschreven in de [oplossingen in het Operations Management Suite (OMS) maken](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u al bekend met het [maken van een oplossing](operations-management-suite-solutions-creating.md) en de structuur van een oplossingsbestand bent.

## <a name="samples"></a>Voorbeelden
U krijgt voorbeeldsjablonen Resource Manager voor automatisering resources uit de [QuickStart sjablonen in de GitHub](https://github.com/azureautomation/automation-packs/tree/master/101-sample-automation-resource-templates).

## <a name="automation-account"></a>Automatisering-account
Alle bronnen in Azure automatisering zijn opgenomen in een [account met automatisering](../automation/automation-security-overview.md#automation-account-overview).  Zoals beschreven in de [OMS werkruimte en account Automation](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account) de automatisering-account is niet opgenomen in de oplossing, maar moet bestaan voordat de oplossing is geïnstalleerd.  Als deze niet beschikbaar is, wordt de installatie van de oplossing niet.

De naam van de rekening van de automatisering is de naam van elke resource automatisering.  Dit is de oplossing met de parameter **accountnaam** in het volgende voorbeeld van een runbook middel.
    
    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbooks
[Azure automatisering runbook](../automation/automation-runbook-types.md) resources hebben een soort **Microsoft.Automation/automationAccounts/runbooks** en hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| runbookType | Hiermee geeft u de typen van de runbook. <br><br> Script - PowerShell-script <br>PowerShell - PowerShell workflow <br> GraphPowerShell - grafische PowerShell script runbook <br> GraphPowerShellWorkflow - runbook van PowerShell grafische workflow   |
| logProgress | Hiermee geeft u aan of [voortgang records](../automation/automation-runbook-output-and-messages.md) moeten worden gegenereerd voor de runbook. |
| logVerbose  | Hiermee geeft u aan of [uitgebreide records](../automation/automation-runbook-output-and-messages.md) moeten worden gegenereerd voor de runbook. |
| Beschrijving | Optionele beschrijving voor de runbook. |
| publishContentLink | Hiermee geeft u de inhoud van de runbook. <br><br>URI - Uri op de inhoud van de runbook.  Dit is een bestand .ps1 voor runbooks en het PowerShell Script en een bestand met geëxporteerde grafische runbook voor de runbook van een grafiek.  <br> versie - versie van de runbook voor uw eigen bijhouden. |

Een voorbeeld van een bron runbook is hieronder.  In dit geval wordt de runbook opgehaald uit de [Galerie met PowerShell](https://www.powershellgallery.com).

    "name": "[concat(parameters('accountName'), '/Start-AzureV2VMs'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]"
    ],
    "tags": { },
    "properties": {
        "runbookType": "PowerShell",
        "logProgress": "true",
        "logVerbose": "true",
        "description": "",
        "publishContentLink": {
            "uri": "https://www.powershellgallery.com/api/v2/items/psscript/package/Update-ModulesInAutomationToLatestVersion/1.0/Start-AzureV2VMs.ps1",
            "version": "1.0"
        }
    }

### <a name="starting-a-runbook"></a>Een runbook starten
Er zijn twee methoden om te beginnen een runbook in een oplossing.

- Om te beginnen de runbook wanneer de oplossing is geïnstalleerd, maakt u een [taak, resource](#automation-jobs) zoals hieronder beschreven.
- Als u de runbook op een schema, een [resource planning](#schedules) te maken zoals hieronder beschreven. 


## <a name="automation-jobs"></a>Taken automatiseren
Een runbook gestart als de oplossing is geïnstalleerd, moet u de resource voor een **taak** maken.  Project resources hebben een soort **Microsoft.Automation/automationAccounts/jobs** en hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| runbook    | **De naam** van één entiteit met de naam van de runbook.  |
| parameters | Entiteit voor elke parameter die wordt vereist door de runbook. |


De taak omvat de naam van de runbook en alle parameterwaarden worden verzonden naar de runbook.  De taak moet [afhankelijk zijn van](operations-management-suite-solutions-creating.md#resources) de runbook die het sinds de runbook wordt gestart voordat de taak moet worden gemaakt.  U kunt ook afhankelijkheden maken op andere taken voor runbooks die moeten worden voltooid voordat het huidige.

De naam van de resource voor een taak moet een GUID die gewoonlijk door een parameter toegewezen wordt bevatten.  U vindt meer informatie over GUID-parameters in [oplossingen maken in het Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md#parameters).  

Hier volgt een voorbeeld van een taak resource waarmee een runbook wordt gestart wanneer de oplossing is geïnstalleerd.  Een andere runbooks moet worden voltooid voordat dit een wordt gestart, zodat deze afhankelijk van de taken voor die runbook is.  De details van het project zijn opgenomen in de parameters en variabelen in plaats van rechtstreeks worden opgegeven.

    {
        "name": "[concat(parameters('accountName'), '/', parameters('startVmsRunbookGuid'))]",
        "type": "Microsoft.Automation/automationAccounts/jobs",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "location": "[parameters('regionId')]",
        "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]",
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/jobs/', parameters('initialPrepRunbookGuid'))]"
        ],
        "tags": { },
        "properties": {
            "runbook": {
                "name": "[variables('startVmsRunbookName')]"
            },
            "parameters": {
                "AzureConnectionAssetName": "[variables('AzureConnectionAssetName')]",
                "ResourceGroupName": "[variables('ResourceGroupName')]"
            }
        }
    }


## <a name="certificates"></a>Certificaten
[Azure automatisering certificaten](../automation/automation-certificates.md) hebben een soort **Microsoft.Automation/automationAccounts/certificates** en hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| base64Value   | Base 64-waarde voor het certificaat. |
| vingerafdruk    | Blauwdruk voor het certificaat. |

Een voorbeeld van een bron van het certificaat is hieronder.

    "name": "[concat(parameters('accountName'), '/MyCertificate')]",
    "type": "certificates",
    "apiVersion": "2015-01-01-preview",
    "location": "[parameters('regionId')]",
    "tags": {},
    "dependsOn": [
    ],
    "properties": {
        "base64Value": "MIIC1jCCAA8gAwIAVgIYJY4wXCXH/YAHMtALh7qEFzANAgkqhkiG5w0AAQUFGDAUMRIwEBYDVQQDEwlsA3NhAGevc3QqHhcNMTZwNjI5MHQxMjAsWhcNOjEwNjI5NKWwMDAwWjAURIwEAYDVQQBEwlsA2NhAGhvc3QwggEiMA0GCSqGSIA3DQEAAQUAA4IADwAwggEKAoIAAQDIyzv2A0RUg1/AAryI9W1DGAHAqqGdlFfTkUSDfv+hEZTAwKv0p8daqY6GroT8Du7ctQmrxJsy8JxIpDWxUaWwXtvv1kR9eG9Vs5dw8gqhjtOwgXvkOcFdKdQwA82PkcXoHlo+NlAiiPPgmHSELGvcL1uOgl3v+UFiiD1ro4qYqR0ITNhSlq5v2QJIPnka8FshFyPHhVtjtKfQkc9G/xDePW8dHwAhfi8VYRmVMmJAEOLCAJzRjnsgAfznP8CZ/QUczPF8LuTZ/WA/RaK1/Arj6VAo1VwHFY4AZXAolz7xs2sTuHplVO7FL8X58UvF7nlxq48W1Vu0l8oDi2HjvAgMAAAGjJDAiMAsGA1UdDwREAwIEsDATAgNVHSUEDDAKAggrAgEFNQcDATANAgkqhkiG9w0AAQUFAAOCAQEAk8ak2A5Ug4Iay2v0uXAk95qdAthJQN5qIVA13Qay8p4MG/S5+aXOVz4uMXGt18QjGds1A7Q8KDV4Slnwn95sVgA5EP7akvoGXhgAp8Dm90sac3+aSG4fo1V7Y/FYgAgpEy4C/5mKFD1ATeyyhy3PmF0+ZQRJ7aLDPAXioh98LrzMZr1ijzlAAKfJxzwZhpJamAwjZCYqiNZ54r4C4wA4QgX9sVfQKd5e/gQnUM8gTQIjQ8G2973jqxaVNw9lZnVKW3C8/QyLit20pNoqX2qQedwsqg3WCUcPRUUqZ4NpQeHL/AvKIrt158zAfU903yElAEm2Zr3oOUR4WfYQ==",
        "thumbprint": "F485CBE5569F7A5019CB68D7G6D987AC85124B4C"
    }

## <a name="credentials"></a>Referenties
[Azure automatisering referenties](../automation/automation-credentials.md) hebben een soort **Microsoft.Automation/automationAccounts/credentials** en hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Gebruikersnaam   | Gebruikersnaam voor de referenties. |
| wachtwoord   | Wachtwoord voor de referentie. |

Een voorbeeld van een bron van referentie is hieronder.

    "name": "[concat(parameters('accountName'), '/', variables('credentialName'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks/credentials",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "userName": "User01",
        "password": "password"
    }


## <a name="schedules"></a>Schema 's
[Azure automatisering schema's](../automation/automation-schedules.md) hebben een soort **Microsoft.Automation/automationAccounts/schedules** en hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Beschrijving | Optionele beschrijving voor de planning. |
| Starttijd   | Hiermee geeft u de begintijd van een schema als een DateTime-object. Een tekenreeks kan worden geleverd als een geldige datum/tijd kan worden geconverteerd. |
| isEnabled   | Hiermee geeft u aan of de planning is ingeschakeld. |
| interval    | Het type interval voor de planning.<br><br>dag<br>uur |
| frequentie   | De frequentie waarmee de planning moet worden geactiveerd in een aantal uren of dagen. |

Een voorbeeld van een resource planning is hieronder.

    "name": "[concat(parameters('accountName'), '/', variables('variableName'))]",
    "type": "microsoft.automation/automationAccounts/schedules",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Schedule for StartByResourceGroup runbook",
        "startTime": "10/30/2016 12:00:00",
        "isEnabled": "true",
        "interval": "day",
        "frequency": "1"
    }


## <a name="variables"></a>Variabelen
[Azure automatisering variabelen](../automation/automation-variables.md) hebben een soort **Microsoft.Automation/automationAccounts/variables** en hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Beschrijving | Optionele beschrijving voor de variabele. |
| isEncrypted | Hiermee geeft u aan of de variabele moet worden gecodeerd. |
| type        | Het gegevenstype van de variabele. |
| waarde       | De waarde voor de variabele. |

Een voorbeeld van een variabele bron is hieronder.

    "name": "[concat(parameters('accountName'), '/', variables('StartTargetResourceGroupsAssetName')) ]",
    "type": "microsoft.automation/automationAccounts/variables",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Description for the variable.",
        "isEncrypted": "true",
        "type": "string",
        "value": "'This is a string value.'"
    }


## <a name="modules"></a>Modules
De oplossing hoeft niet te definiëren, [globale modules](../automation/automation-integration-modules.md) gebruikt door de runbooks, omdat ze altijd beschikbaar.  U hoeft een resource voor een willekeurige andere module die wordt gebruikt door de runbooks en de runbook moet afhankelijk zijn van de bron van de module om ervoor te zorgen dat wordt gemaakt voordat de runbook.

[Integratiemodules](../automation/automation-integration-modules.md) een soort **Microsoft.Automation/automationAccounts/modules** en hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| contentLink | Hiermee geeft u de inhoud van de module. <br><br>URI - Uri op de inhoud van de runbook.  Dit is een bestand .ps1 voor runbooks en het PowerShell Script en een bestand met geëxporteerde grafische runbook voor de runbook van een grafiek.  <br> versie - versie van de runbook voor uw eigen bijhouden. |

Een voorbeeld van een module resource lager is dan.

    {       
        "name": "[concat(parameters('accountName'), '/', variables('OMSIngestionModuleName'))]",
        "type": "Microsoft.Automation/automationAccounts/modules",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "properties": {
            "contentLink": {
                "uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
            }
        }
    }

### <a name="updating-modules"></a>Modules bijwerken
Als u een oplossing met een runbook die gebruikmaakt van een planning bijwerken en de nieuwe versie van uw oplossing een nieuwe module die door deze runbook wordt gebruikt heeft, kan de runbook de oude versie van de module gebruiken.  De volgende runbooks opnemen in uw oplossing en u een taak om ze uit te voeren voordat u een andere runbooks maken.  Hierdoor weet u zeker dat alle modules die worden bijgewerkt als nodig is voordat de runbooks worden geladen.

- [Update ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) zal ervoor zorgen dat alle modules die worden gebruikt door runbooks in de oplossing de meest recente versie.  
- [MS-ReRegisterAutomationSchedule-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) opnieuw alle resources plannen om ervoor te zorgen dat de runbooks gekoppeld aan hen gebruiken de nieuwste modules registreren.


Hier volgt een voorbeeld van de vereiste elementen van een oplossing voor de ondersteuning van de module update.
    
    "parameters": {
        "ModuleImportGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        },
        "ReRegisterRunbookGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        }
    },

    "variables": {
        "ModuleImportRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ModuleImportRunbookUri": "https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/Content/Update-ModulesInAutomationToLatestVersion.ps1",
        "ModuleImportRunbookDescription": "Imports modules and also updates all Azure dependent modules that are in the same Automation Account.",
        "ReRegisterSchedulesRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ReRegisterSchedulesRunbookUri": "https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/Content/ReRegisterAutomationSchedule-MS-Mgmt.ps1",
        "ReRegisterSchedulesRunbookDescription": "Reregisters schedules to ensure that they use latest modules."
    },

    "resources": [
        {
            "name": "[concat(parameters('accountName'), '/', variables('ModuleImportRunbookName'))]",
            "type": "Microsoft.Automation/automationAccounts/runbooks",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "dependsOn": [
            ],
            "location": "[parameters('regionId')]",
            "tags": { },
            "properties": {
                "runbookType": "PowerShell",
                "logProgress": "true",
                "logVerbose": "true",
                "description": "[variables('ModuleImportRunbookDescription')]",
                "publishContentLink": {
                    "uri": "[variables('ModuleImportRunbookUri')]",
                    "version": "1.0.0.0"
                }
            }
        },
        {
            "name": "[concat(parameters('accountName'), '/', parameters('ModuleImportGuid'))]",
            "type": "Microsoft.Automation/automationAccounts/jobs",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "location": "[parameters('regionId')]",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ModuleImportRunbookName'))]"
            ],
            "tags": { },
            "properties": {
                "runbook": {
                    "name": "[variables('ModuleImportRunbookName')]"
                },
                "parameters": {
                    "ResourceGroupName": "[resourceGroup().name]",
                    "AutomationAccountName": "[parameters('accountName')]",
                    "NewModuleName": "AzureRM.Insights"
                }
            }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('ReRegisterSchedulesRunbookName'))]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "PowerShell",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('ReRegisterSchedulesRunbookDescription')]",
            "publishContentLink": {
              "uri": "[variables('ReRegisterSchedulesRunbookUri')]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', parameters('reRegisterSchedulesGuid'))]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ReRegisterSchedulesRunbookName'))]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('ReRegisterSchedulesRunbookName')]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
        }
    ]


## <a name="next-steps"></a>Volgende stappen

- [Een weergave toevoegen aan uw oplossing](operations-management-suite-solutions-resources-views.md) verzamelde gegevens visualiseren.
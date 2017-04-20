<properties
   pageTitle="Weergaven in oplossingen voor het beheer van Operations Management Suite (OMS) | Microsoft Azure"
   description="Oplossingen voor werkstroombeheer in Operations Management Suite (OMS) beschikken meestal over een of meer weergaven om gegevens.  In dit artikel wordt beschreven hoe een weergave gemaakt door de ontwerpfunctie voor weergaven exporteren en opnemen in een oplossing. "
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
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Weergaven in het Operations Management Suite (OMS) oplossingen (voorbeeld)

>[AZURE.NOTE]Dit is voorlopige documentatie voor het maken van oplossingen in OMS die momenteel in de voorvertoning. De hieronder beschreven schema kan worden gewijzigd.    

[Oplossingen voor werkstroombeheer in Operations Management Suite (OMS)](operations-management-suite-solutions.md) beschikken meestal over een of meer weergaven om gegevens.  In dit artikel wordt beschreven hoe een weergave gemaakt door de [Ontwerpfunctie voor weergaven](../log-analytics/log-analytics-view-designer.md) exporteren en opnemen in een oplossing.  

>[AZURE.NOTE]De voorbeelden in dit artikel gebruiken parameters en variabelen die zijn vereist of gemeenschappelijke oplossingen voor beheer en wordt beschreven in de [oplossingen in het Operations Management Suite (OMS) maken](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u al bekend met het [maken van een oplossing](operations-management-suite-solutions-creating.md) en de structuur van een oplossingsbestand bent.


## <a name="overview"></a>Overzicht

Om een weergave opneemt in een oplossing, maakt u een **bron** voor het in het [oplossingsbestand](operations-management-suite-solutions-creating.md).  De JSON die in detail de configuratie van de weergave beschrijft is complex wel en niet iets dat de auteur van een typische oplossing zou kunnen handmatig maken.  De meest gebruikte methode is de weergave met behulp van de [Ontwerpfunctie voor weergaven](../log-analytics/log-analytics-view-designer.md)maken, exporteren en vervolgens de gedetailleerde configuratie toevoegen aan de oplossing. 

Basisstappen voor het toevoegen van een weergave aan een oplossing zijn als volgt.  Elke stap wordt nader besproken in de onderstaande secties beschreven.

1. De weergave exporteren naar een bestand.
2. Maak de resource weergeven in de oplossing.
3. De weergavedetails toevoegen.

## <a name="export-the-view-to-a-file"></a>De weergave exporteren naar een bestand
Volg de instructies op het [Ontwerp van logboek Analytics weergeven](../log-analytics/log-analytics-view-designer.md) een weergave exporteren naar een bestand.  Het geëxporteerde bestand worden in JSON-formaat met dezelfde [elementen als het oplossingsbestand](operations-management-suite-solutions-creating.md#management-solution-files).  

Het element van de **bronnen** van het weergavebestand heeft een resource met een soort **Microsoft.OperationalInsights/workspaces** die de werkruimte OMS vertegenwoordigt.  Dit element heeft een subelement van het type **weergaven** die staat voor de weergave en de gedetailleerde configuratiegegevens bevat.  U de details van dit element kopiëren en deze vervolgens kopiëren naar uw oplossing.


## <a name="create-the-view-resource-in-the-solution"></a>De resource weergeven in de oplossing te maken
De volgende resource weergeven toevoegen aan het element van de **bronnen** van het oplossingsbestand.  Deze maakt gebruik van variabelen die hieronder worden beschreven die u ook moet toevoegen.  Merk op dat de eigenschappen van het **Dashboard** en **OverviewTile** tijdelijke aanduidingen die door de corresponderende eigenschappen van het geëxporteerde weergavebestand wordt overschreven.
 
    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile": 
        }
    }

De volgende variabelen toevoegen aan het element van de [variabelen](operations-management-suite-solutions-creating.md#variables) van het oplossingsbestand en vervang de waarden die voor uw oplossing.

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."


Houd er rekening mee dat u de bron van de gehele weergave vanuit het bestand met geëxporteerde weergave kopiëren kan, maar u moet de volgende wijzigingen te werken in de oplossing.  

- Het **type** voor de resource weergeven moet worden gewijzigd van **weergaven** in **Microsoft.OperationalInsights/workspaces**.
- De eigenschap **naam** voor de bron van de weergave moet worden gewijzigd en bevat de naam van de werkruimte.
- De afhankelijkheid van de werkruimte moet worden verwijderd omdat de bron van de werkruimte is niet gedefinieerd in de oplossing.
- De eigenschap **DisplayName** moet worden toegevoegd aan de weergave.  De **Id**, **naam**en **DisplayName** moeten alle overeenkomen.
- Parameternamen moeten worden gewijzigd zodat deze overeenkomen met de vereiste set parameters.
- Variabelen moeten worden gedefinieerd in de oplossing en gebruikt in de gewenste eigenschappen.

## <a name="add-the-view-details"></a>De weergavedetails toevoegen
De resource weergeven in het geëxporteerde weergavebestand bevat twee elementen in het **Dashboard** en **OverviewTile** genoemde **Eigenschappen** -element waarin de gedetailleerde configuratie van de weergave.  Deze twee elementen en de inhoud ervan kopiëren naar het element van de **Eigenschappen** van de bron weergeven in het oplossingsbestand. 

## <a name="example"></a>Voorbeeld
In het volgende voorbeeld ziet u bijvoorbeeld een eenvoudige oplossing-bestand met een weergave.  Weglatingstekens (...) worden weergegeven voor de inhoud van het **Dashboard** en **OverviewTile** omwille van de ruimte.


    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
        ]
    }




## <a name="next-steps"></a>Volgende stappen

- Informatie over alle details van het maken van [oplossingen](operations-management-suite-solutions-creating.md).
- [Runbooks voor automatisering in uw oplossing](operations-management-suite-solutions-resources-automation.md)zijn.
<properties
   pageTitle="Sjabloon Resource Manager voor het koppelen van resources | Microsoft Azure"
   description="Vindt u het schema Resource Manager voor het implementeren van koppelingen tussen verwante bronnen via een sjabloon."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="resource-links-template-schema"></a>Schema voor resource koppelingen

Hiermee maakt u een koppeling tussen twee bronnen. De koppeling wordt met een bron die bekend staat als de bron van de bron toegepast. De tweede bron in de koppeling staat bekend als de doelbron.

## <a name="schema-format"></a>Schema-indeling

Toevoegen in het volgende schema aan de sectie bronnen van de sjabloon wilt koppelen.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "targetId": string,
            "notes": string
        }
    }



## <a name="values"></a>Waarden

De volgende tabellen beschrijven de waarden die u nodig hebt om in te stellen in het schema.

| Naam | Waarde |
| ---- | ---- |
| type | Enum<br />Vereist<br />**{naamruimte} / {type} providers/koppelingen**<br /><br />Het brontype te maken. {De naamruimte} en {} waarden verwijzen naar het type gegevensvoorziening naamruimte en de bron van de bron van de bron. |
| apiVersion | Enum<br />Vereist<br />**01-01-2015**<br /><br />De versie van de API te gebruiken voor het maken van de resource. |  
| naam | Tekenreeks<br />Vereist<br />**{resouce}/Microsoft.Resources/{linkname}**<br /> maximaal 64 tekens bevatten en mag niet <>, %, &,?, of willekeurig control-tekens.<br /><br />Een waarde ter aanduiding van zowel de naam van de bron van de bron en een naam voor de koppeling. |
| dependsOn | Matrix<br />Optioneel<br />Een door komma's gescheiden lijst van een resource namen of unieke resource-id's.<br /><br />De collectie van de middelen hangt af van deze koppeling. Als de resources die u wilt koppelen in dezelfde sjabloon worden gebruikt, bevatten de resourcenamen in dit element om ervoor te zorgen dat zij eerst worden geïmplementeerd. | 
| Eigenschappen | Object<br />Vereist<br />[object eigenschappen](#properties)<br /><br />Een object waarmee de resource wilt koppelen, en opmerkingen over de koppeling. |  

<a id="properties" />
### <a name="properties-object"></a>object eigenschappen

| Naam | Waarde |
| ------- | ---- |
| targetId | Tekenreeks<br />Vereist<br />**{resource-id}**<br /><br />De id van de doelbron wilt koppelen. |
| notities | Tekenreeks<br />Optioneel<br />maximaal 512 tekens bevatten<br /><br />Beschrijving van de vergrendeling. |


## <a name="how-to-use-the-link-resource"></a>Het gebruik van de koppeling bron

U een koppeling tussen twee resources wanneer resources hebben een afhankelijkheid die na de implementatie blijft van toepassing. Een app kan bijvoorbeeld verbinding maken met een database in een andere bronnengroep bevinden. Door een koppeling met de database van de toepassing kunt u deze afhankelijkheid. Koppelingen kunt u de relatie tussen twee bronnen. Later, kunt u of iemand anders in uw organisatie een bron voor koppelingen om te ontdekken hoe de resource werkt met andere bronnen zoeken.

Alle gekoppelde bronnen moeten deel uitmaken van het hetzelfde abonnement. Elke resource kan worden gekoppeld aan 50 andere bronnen. Als een van de gekoppelde bronnen zijn verwijderd of verplaatst, moet de eigenaar van de koppeling de resterende koppeling opschonen.

Als u wilt werken met koppelingen rest, Zie [Gekoppelde bronnen](https://msdn.microsoft.com/library/azure/mt238499.aspx).

Gebruik de volgende Azure PowerShell-opdracht voor een overzicht van alle koppelingen in uw abonnement. U kunt andere parameters om de resultaten te beperken bieden.

    Get-AzureRmResource -ResourceType Microsoft.Resources/links -isCollection -ResourceGroupName <YourResourceGroupName>

## <a name="examples"></a>Voorbeelden

Het volgende voorbeeld wordt een alleen-lezen vergrendeling aan een web app.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "hostingPlanName": {
                "type": "string"
            }
        },
        "variables": {
            "siteName": "[concat('site',uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "type": "Microsoft.Web/serverfarms",
                "name": "[parameters('hostingPlanName')]",
                "location": "[resourceGroup().location]",
                "sku": {
                    "tier": "Free",
                    "name": "f1",
                    "capacity": 0
                },
                "properties": {
                    "numberOfWorkers": 1
                }
            },
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "dependsOn": [ "[parameters('hostingPlanName')]" ],
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                }
            },
            {
                "type": "Microsoft.Web/sites/providers/links",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
                    "notes": "This web site uses the storage account to store user information."
                }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>QuickStart-sjablonen

De volgende sjablonen voor quickstart implementeren bronnen met een koppeling.

- [Waarschuwen voor wachtrij met logica app](https://azure.microsoft.com/documentation/templates/201-alert-to-queue-with-logic-app)
- [Waarschuwing voor toegestane vertraging met logica app](https://azure.microsoft.com/documentation/templates/201-alert-to-slack-with-logic-app)
- [Inrichten van een API-app met een bestaande gateway](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-existing)
- [Inrichten van een API-app met een nieuwe gateway](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-new)
- [Een App logica plus API app met behulp van een sjabloon maken](https://azure.microsoft.com/documentation/templates/201-logic-app-api-app-create)
- [Logica app die een bericht wordt verzonden wanneer er een waarschuwing wordt gestart](https://azure.microsoft.com/documentation/templates/201-alert-to-text-message-with-logic-app)


## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over de sjabloonstructuur van de [sjablonen ontwerpen Azure Resource Manager](resource-group-authoring-templates.md).

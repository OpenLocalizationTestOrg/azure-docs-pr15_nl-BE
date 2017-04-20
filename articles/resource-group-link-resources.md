<properties 
    pageTitle="Koppelen in Azure resourcemanager resources | Microsoft Azure" 
    description="Een koppeling maken tussen verwante bronnen in verschillende resourcegroepen in Azure Resource Manager." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/01/2016" 
    ms.author="tomfitz"/>

# <a name="linking-resources-in-azure-resource-manager"></a>Koppelen in Azure resourcemanager resources

Tijdens de implementatie kunt u een bron als die van een andere bron afhankelijk markeert, maar die lifecycle eindigt bij de implementatie. Na de implementatie is er geen bekende relatie tussen afhankelijke bronnen. Resource Manager bevat een functie genaamd resource koppelt, permanente relaties tussen bronnen.

Met een resource koppelt, kunt u de relaties die zich uitstrekken over resourcegroepen documenteren. Het is bijvoorbeeld gebruikelijk een app met een verschillende levenscyclus bevinden zich in een andere bronnengroep bevinden en als u een database met een eigen levenscyclus bevinden zich in één resourcegroep. De app verbonden met de database zodat u wilt een koppeling tussen de app en de database markeren. 

Alle gekoppelde bronnen moeten deel uitmaken van het hetzelfde abonnement. Elke resource kan worden gekoppeld aan 50 andere bronnen. De enige manier om de gerelateerde bronnen zoeken is via de REST API. Als een van de gekoppelde bronnen zijn verwijderd of verplaatst, moet de eigenaar van de koppeling de resterende koppeling opschonen. U bent **niet** gewaarschuwd wanneer een bron die is gekoppeld aan andere bronnen te verwijderen.

## <a name="linking-in-templates"></a>Koppelingen in sjablonen

Als u wilt een koppeling in een sjabloon definieert, zijn u een brontype dat de naamruimte resource provider combineert en het type van de bron bron met **/providers/links**. De naam moet de naam van de bron bron bevatten. U opgeven de resource-id van de resource doel. In het volgende voorbeeld wordt een koppeling tussen een website en een account voor opslag.

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


Zie [Resource koppelingen - sjabloon schema](resource-manager-template-links.md)voor een volledige beschrijving van de sjabloonindeling.

## <a name="linking-with-rest-api"></a>Koppelen met REST API

Als u een koppeling tussen gedistribueerde bronnen definieert, worden uitgevoerd:

    PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/providers/Microsoft.Resources/links/{link-name}?api-version={api-version}

{Abonnement-id} vervangen door uw abonnements-id. Vervang {resource-groep} {provider-naamruimte, {-brontype} en {bronnaam} met de waarden die de eerste resource in de koppeling aangeven. {Naam koppeling} vervangen door de naam van de koppeling te maken. Gebruik 2015 01-01-voor de api-versie.

Opnemen in de aanvraag, een object dat de tweede bron in de koppeling wordt gedefinieerd:

    {
        "name": "{new-link-name}",
        "properties": {
            "targetId": "subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/",
            "notes": "{link-description}",
        }
    }

De eigenschappen van element bevat de id van de tweede bron.

U kunt koppelingen in uw abonnement met opvragen:

    https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Resources/links?api-version={api-version}

Zie voor meer voorbeelden, met inbegrip van het ophalen van informatie over koppelingen, [Gekoppelde bronnen](https://msdn.microsoft.com/library/azure/mt238499.aspx).

## <a name="next-steps"></a>Volgende stappen

- U kunt ook uw resources met tags organiseren. Zie meer informatie over bronnen voor tagging, [tags gebruiken voor het ordenen van uw resources](resource-group-using-tags.md).
- Zie [sjablonen ontwerpen](resource-group-authoring-templates.md)voor een beschrijving van hoe u sjablonen maken en definiëren van de bronnen kunnen worden geïmplementeerd.

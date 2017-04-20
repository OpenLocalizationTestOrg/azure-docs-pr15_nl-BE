<properties
    pageTitle="Met behulp van labels voor het ordenen van uw resources Azure | Microsoft Azure"
    description="Codes voor het indelen van bronnen voor facturering en het beheren van toepassing wordt."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="AzurePortal"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="tomfitz"/>


# <a name="using-tags-to-organize-your-azure-resources"></a>Met behulp van labels voor het ordenen van uw resources Azure

Resource Manager kunt u logisch indelen van bronnen door labels toe te passen. De codes bestaan uit sleutel/waarde-paren die resources identificeren met de eigenschappen die u definieert. Als u resources aan dezelfde categorie behoren, hetzelfde label van toepassing op deze middelen.

Wanneer u resources met een bepaalde tag bekijkt, ziet u de bronnen van alle resourcegroepen. U bent niet beperkt tot alleen de bronnen in dezelfde resourcegroep, waarmee u resources op een manier die onafhankelijk is van de implementatie van relaties te organiseren. Tags is handig als u nodig hebt voor het ordenen van bronnen voor facturering of management.

Elke code die u aan een resource of resourcegroep toevoegt wordt automatisch toegevoegd aan de taxonomie hele abonnement. U kunt ook de taxonomie vooraf invullen voor uw abonnement met de labelnamen en -waarden die u gebruiken wilt als de resources in de toekomst worden gecodeerd.

Elke resource of resourcegroep kan maximaal 15 tags hebben. De labelnaam mag maximaal 512 tekens en de waarde van de tag is beperkt tot 256 tekens.

> [AZURE.NOTE] U kunt alleen labels toepassen op bronnen die worden ondersteund door bronbeheer. Als u een virtuele Machine, Virtual Network of opslag via de klassieke implementatiemodel gemaakt (zoals via de klassieke portal), niet kunt u een tag toepast die resource. Implementeren ter ondersteuning van labels, deze bronnen via Resource Manager. Alle andere bronnen ondersteuning voor codering.

## <a name="templates"></a>Sjablonen

Als een resource tijdens de implementatie van labels, gewoon het element met **codes** toevoegen aan de resource die u wilt implementeren en bieden de label en de waarde. De naam van de code en de waarde hoeft niet vooraf aanwezig zijn in uw abonnement. U kunt maximaal 15 tags opgeven voor elke resource.

In het volgende voorbeeld ziet u een opslag-account met een label.

    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2015-06-15",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "tags": {
                "dept": "Finance"
            },
            "properties": 
            {
                "accountType": "Standard_LRS"
            }
        }
    ]

Resource Manager biedt momenteel geen ondersteuning voor verwerking van een object voor de labelnamen en -waarden. In plaats daarvan een object voor de waarden van het argument doorgeeft, maar u moet nog steeds de labelnamen opgeven zoals in het volgende voorbeeld.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "tagvalues": {
          "type": "object",
          "defaultValue": {
            "dept": "Finance",
            "project": "Test"
          }
        }
      },
      "resources": [
      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "examplestorage",
        "tags": {
          "dept": "[parameters('tagvalues').dept]",
          "project": "[parameters('tagvalues').project]"
        },
        "location": "[resourceGroup().location]",
        "properties": {
          "accountType": "Standard_LRS"
        }
      }]
    }


## <a name="portal"></a>Portal

[AZURE.INCLUDE [resource-manager-tag-resource](../includes/resource-manager-tag-resources.md)]

## <a name="powershell"></a>PowerShell

[AZURE.INCLUDE [resource-manager-tag-resources](../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Azure CLI

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="rest-api"></a>REST-API

De portal en PowerShell gebruiken beide de [Bronnenbeheerder REST API](https://msdn.microsoft.com/library/azure/dn848368.aspx) achter de schermen. Als u coderen in een andere omgeving integreren wilt, kunt u labels met een GET van de resource-id ophalen en bijwerken van de set codes met een aanroep van de PATCH.


## <a name="tags-and-billing"></a>Labels en facturering

Voor ondersteunde services kunt u codes om uw factuuradres gegevens te groeperen. Bijvoorbeeld, kunt u definiëren en toepassen van codes voor het indelen van het gebruik van facturering voor virtuele machines [virtuele Machines met Azure Resource Manager geïntegreerd](./virtual-machines/virtual-machines-windows-compare-deployment-models.md) . Als u meerdere VMs voor verschillende organisaties worden uitgevoerd, kunt u de codes aan het gebruik van de groep per kostenplaats.  
U kunt ook labels gebruiken voor het categoriseren van kosten door de runtimeomgeving. zoals het gebruik van het factuuradres voor VMs uitgevoerd in de productieomgeving.

U kunt informatie ophalen over codes via de [Azure Resourcegebruik en RateCard API's](billing-usage-rate-card-overview.md) of het gebruik door komma's gescheiden waarden (CSV) bestand. U downloaden het bestand gebruik van de [portal Azure accounts](https://account.windowsazure.com/) of [EA portal](https://ea.azure.com). Zie voor meer informatie over de programmatische toegang tot informatie over facturering, [inzicht in uw verbruik van Microsoft Azure krijgen](billing-usage-rate-card-overview.md). Zie voor REST API operations, [Azure facturering REST API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Wanneer u het gebruik van CSV voor services die codes met facturering, ondersteunen de tags worden weergegeven in de kolom **labels** downloaden. Zie voor meer details, [begrijp wat uw factuur voor Microsoft Azure](billing/billing-understand-your-bill.md).

![Zie tags in facturering](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Volgende stappen

- U kunt beperkingen en conventies over je abonnement met het aangepaste beleid toepassen. Het beleid dat u definieert kan vereisen dat alle resources een waarde voor een bepaalde tag hebben. Zie [Beleid voor netwerkbronnen beheren en toegang beheren](resource-manager-policy.md)voor meer informatie.
- Zie voor een inleiding tot het gebruik van Azure PowerShell bij het implementeren van bronnen, [Met behulp van Azure PowerShell Azure Resource Manager](./powershell-azure-resource-manager.md).
- Zie voor een inleiding tot het gebruik van Azure CLI bij het implementeren van bronnen, [met behulp van de CLI Azure voor Mac, Linux en Windows met Azure Resource Management](./xplat-cli-azure-resource-manager.md).
- Zie voor een inleiding tot het gebruik van de portal, [via de portal Azure Azure resources beheren](./azure-portal/resource-group-portal.md)  

<properties
   pageTitle="Sjabloon voor vergrendelingen resource Resource Manager | Microsoft Azure"
   description="Vindt u het schema Resource Manager voor het implementeren van bronvergrendelingen door middel van een sjabloon."
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
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="resource-locks-template-schema"></a>Schema voor resource sloten

Hiermee maakt u een vergrendeling op een resource en de onderliggende bronnen.

## <a name="schema-format"></a>Schema-indeling

Toevoegen in het volgende schema aan de sectie bronnen van de sjabloon wilt maken van vergrendeling.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "level": enum,
            "notes": string
        }
    }



## <a name="values"></a>Waarden

De volgende tabellen beschrijven de waarden die u nodig hebt om in te stellen in het schema.

| Naam | Vereist | Beschrijving |
| ---- | -------- | ----------- |
| type | Ja | Het brontype te maken.<br /><br />Voor resources:<br />**{naamruimte} / {type} vergrendelingen providers /**<br /><br/>Voor resourcegroepen:<br />**Microsoft.Authorization/locks** |
| apiVersion | Ja | De versie van de API te gebruiken voor het maken van de resource.<br /><br />Gebruik:<br />**01-01-2015**<br /><br /> |
| naam | Ja | Een waarde die de bron te vergrendelen en een naam voor de vergrendeling. Maximaal 64 tekens bestaan en mag niet <>, %, &,?, of willekeurig control-tekens.<br /><br />Voor resources:<br />**{resource}/Microsoft.Authorization/{lockname}**<br /><br />Voor resourcegroepen:<br />**{lockname}** |
| dependsOn | Nee | Een door komma's gescheiden lijst van een resource namen of unieke resource-id's.<br /><br />De collectie van de middelen hangt af van deze vergrendeling. Als de bronvergrendeling in dezelfde sjabloon wordt geïmplementeerd, bevatten de naam van die resource in dit element kunt u dat de resource eerst wordt geïmplementeerd. | 
| Eigenschappen | Ja | Een object dat het type vergrendeling en notities over de vergrendeling.<br /><br />Zie [Eigenschappen-object](#properties-object). |  

### <a name="properties-object"></a>object eigenschappen

| Naam | Vereist | Beschrijving |
| ---- | -------- | ----------- |
| niveau   | Ja | Het type vergrendeling toe te passen op het bereik.<br /><br />**CannotDelete** - gebruikers kunnen wijzigen van de bron, maar niet verwijderd.<br />**Alleen-lezen** - gebruikers kunnen lezen van een bron, maar niet verwijderen of alle acties op uitvoeren. |
| notities   | Nee | Beschrijving van de vergrendeling. Mag maximaal 512 tekens bevatten. |


## <a name="how-to-use-the-lock-resource"></a>Het gebruik van de lock-bron

U toevoegen deze resource aan uw sjabloon om te voorkomen dat de opgegeven acties op een bron. De vergrendeling is van toepassing op alle gebruikers en groepen.

Maken of verwijderen van vergrendelingen beheren, hebt u toegang tot de **Microsoft.Authorization/** * of * *Microsoft.Authorization/locks/* ** Acties. Van de ingebouwde functies, alleen **eigenaar** en **gebruiker toegang beheerder ** deze acties worden verleend. Voor meer informatie over rollen gebaseerd toegangsbeheer Zie [Toegangsbeheer Azure rolgebaseerde](./active-directory/role-based-access-control-configure.md).

De vergrendeling wordt toegepast op de opgegeven resource en eventuele onderliggende bronnen.

Kunt u een vergrendeling met de PowerShell-opdracht **Verwijderen AzureRmResourceLock** of het [verwijderen](https://msdn.microsoft.com/library/azure/mt204562.aspx) van de REST API.

## <a name="examples"></a>Voorbeelden

Het volgende voorbeeld wordt een vergrendeling kan niet verwijderen op een web app.

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
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                },
            },
            {
                "type": "Microsoft.Web/sites/providers/locks",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
             }
        ],
        "outputs": {}
    }

Een vergrendeling kan niet verwijderen geldt in het volgende voorbeeld voor de resourcegroep.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Authorization/locks",
                "apiVersion": "2015-01-01",
                "name": "MyGroupLock",
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
            }
        ],
        "outputs": {}
    }

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over de sjabloonstructuur van de [sjablonen ontwerpen Azure Resource Manager](resource-group-authoring-templates.md).
- Zie voor meer informatie over sloten [bronnen vergrendelen met Azure Resource Manager](resource-group-lock-resources.md).

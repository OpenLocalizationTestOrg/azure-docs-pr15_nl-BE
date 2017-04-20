<properties
   pageTitle="Afhankelijkheden in de Resource Manager templates | Microsoft Azure"
   description="Hierin wordt beschreven hoe één bron als die van een andere bron afhankelijk tijdens de implementatie tot de resources worden ingezet in de juiste volgorde."
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
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="defining-dependencies-in-azure-resource-manager-templates"></a>Afhankelijkheden in Azure Resource Manager sjablonen definiëren

Voor een bepaalde bron, kunnen er andere bronnen moeten bestaan voordat de resource wordt toegepast. Bijvoorbeeld moet een SQL-server bestaan voordat u de implementatie van een SQL-database. U definiëren deze relatie door één bron afhankelijk is van de andere bron te markeren. Normaal gesproken definieert u een afhankelijkheid met het element **dependsOn** , maar ook kunt u deze via de **verwijzing** . 

Resource Manager evalueert de afhankelijkheden tussen resources en implementeert u deze in de volgorde van hun afhankelijke. Wanneer de bronnen niet van elkaar afhankelijk zijn, worden deze door Resource Manager parallel implementeert.

## <a name="dependson"></a>dependsOn

Binnen uw sjabloon kunt het element dependsOn u een resource definiëren als een afhankelijk van een of meer resources. De waarde kan een door komma's gescheiden lijst met namen van de resources zijn. 

In het volgende voorbeeld ziet u een virtuele machine schaal instellen die afhankelijk is van een load balancer virtueel netwerk en een lus waarmee meerdere accounts voor opslag. Deze andere bronnen worden niet weergegeven in het volgende voorbeeld, maar moeten ze zich elders bevinden in de sjabloon.

    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "name": "[variables('namingInfix')]",
      "location": "[variables('location')]",
      "apiVersion": "2016-03-30",
      "tags": {
        "displayName": "VMScaleSet"
      },
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      ...
    }

Als u een afhankelijkheid tussen een bron en de bronnen die zijn gemaakt via een lus kopie definieert, stelt het element dependsOn op naam van de lus. Zie voor een voorbeeld, [meerdere exemplaren van de resources in Azure Resource Manager maken](resource-group-create-multiple.md).

Terwijl helling ten opzichte kan dependsOn met relaties tussen resources toewijst, is het belangrijk om te begrijpen waarom u doet dit omdat dit effect op de prestaties van uw implementatie hebben kan. Als u wilt documenteren hoe resources met elkaar zijn verbonden, is dependsOn bijvoorbeeld niet de juiste aanpak. U kan niet opvragen welke bronnen zijn gedefinieerd in het element dependsOn na de implementatie. Met behulp van dependsOn u mogelijk van invloed zijn implementatietijd omdat Resource Manager implementeert geen parallelle twee bronnen die een afhankelijkheid hebben. Document relaties tussen bronnen, in plaats daarvan gebruikt u [resource koppelen](resource-group-link-resources.md).

## <a name="child-resources"></a>Onderliggende bronnen

De eigenschap resources kunt u opgeven van de onderliggende bronnen die betrekking hebben op de resource wordt gedefinieerd. Onderliggende bronnen kunnen alleen worden gedefinieerd vijf niveaus diep. Het is belangrijk te weten dat er een impliciete afhankelijkheid niet wordt gemaakt tussen de bron van de bovenliggende en onderliggende resources. Als u de onderliggende bron worden geïmplementeerd na de bovenliggende resource, moet u die afhankelijkheid met de eigenschap dependsOn expliciet worden vermeld. 

Elke bovenliggende resource accepteert alleen bepaalde resourcetypen als onderliggende bronnen. De geaccepteerde brontypen worden opgegeven in het [schema voor](https://github.com/Azure/azure-resource-manager-schemas) de bron van de bovenliggende. De naam van het brontype kind bevat de naam van het bovenliggende resourcetype, zoals **Microsoft.Web/sites/config** en **Microsoft.Web/sites/extensions** zijn beide onderliggende bronnen van de **Microsoft.Web/sites**.

In het volgende voorbeeld ziet u een SQL-server en de SQL-database. U ziet dat een expliciete afhankelijkheid gedefinieerd tussen de SQL-database en SQL server, zelfs als de database een onderliggend element van de server is.

    "resources": [
      {
        "name": "[variables('sqlserverName')]",
        "type": "Microsoft.Sql/servers",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "SqlServer"
        },
        "apiVersion": "2014-04-01-preview",
        "properties": {
          "administratorLogin": "[parameters('administratorLogin')]",
          "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
        },
        "resources": [
          {
            "name": "[parameters('databaseName')]",
            "type": "databases",
            "location": "[resourceGroup().location]",
            "tags": {
              "displayName": "Database"
            },
            "apiVersion": "2014-04-01-preview",
            "dependsOn": [
              "[variables('sqlserverName')]"
            ],
            "properties": {
              "edition": "[parameters('edition')]",
              "collation": "[parameters('collation')]",
              "maxSizeBytes": "[parameters('maxSizeBytes')]",
              "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
            }
          }
        ]
      }
    ]


## <a name="reference-function"></a>verwijzing naar functie

De [verwijzing naar functie](resource-group-template-functions.md#reference) stelt een expressie voor het afleiden van de waarde van andere JSON-naam- en waardeparen of runtime-bronnen. Reference expressies declareren impliciet dat één bron afhankelijk van een andere is. 

    reference('resourceName').propertyPath

U kunt dit element of het element dependsOn afhankelijkheden opgeven, maar u hoeft niet te gebruiken voor deze bron afhankelijk is. Gebruik waar mogelijk een impliciete verwijzing om te voorkomen dat per ongeluk een onnodige afhankelijkheid toevoegen.

Voor meer informatie, Zie [referentie-functie](resource-group-template-functions.md#reference).

## <a name="next-steps"></a>Volgende stappen

- Zie meer informatie over het maken van Azure Resource Manager-sjablonen, [sjablonen ontwerpen](resource-group-authoring-templates.md). 
- Zie [functies van de sjabloon](resource-group-template-functions.md)voor een lijst van de beschikbare functies in een sjabloon.


<properties
   pageTitle="Implementatie van meerdere exemplaren van Resources | Microsoft Azure"
   description="Kopiëren en arrays in een sjabloon Azure Resource Manager gebruiken om meerdere malen bij de implementatie van resources."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/30/2016"
   ms.author="tomfitz"/>

# <a name="create-multiple-instances-of-resources-in-azure-resource-manager"></a>Meerdere exemplaren van de resources in Azure Resource Manager maken

In dit onderwerp wordt beschreven hoe u bladeren in uw sjabloon Azure Resource Manager voor het maken van meerdere exemplaren van een resource.

## <a name="copy-copyindex-and-length"></a>kopiëren, copyIndex en lengte

In de bron voor het maken van meerdere keren kunt u een object **kopiëren** , waarmee het aantal keren te herhalen. De kopie wordt gebruikt in de volgende notatie:

    "copy": { 
        "name": "websitescopy", 
        "count": "[parameters('count')]" 
    } 

U kunt de huidige iteratie-waarde met de functie **copyIndex()** openen, zoals hieronder wordt weergegeven binnen de functie concat.

    [concat('examplecopy-', copyIndex())]

Als u meerdere resources uit een matrix met waarden, kunt u de functie **lengte** kunt u het aantal opgeven. U opgeven de matrix als de parameter voor de functie lengte.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

## <a name="use-index-value-in-name"></a>Indexwaarde in het vak naam gebruiken

U kunt de kopie bewerking meerdere exemplaren van een bron die een unieke naam worden op basis van de stijgende index maken. U wilt een uniek nummer toevoegen aan het einde van de naam van elke resource die is geïmplementeerd. Drie websites met de naam implementeren:

- examplecopy-0
- examplecopy-1
- examplecopy-2.

Gebruik de volgende sjabloon:

    "parameters": { 
      "count": { 
        "type": "int", 
        "defaultValue": 3 
      } 
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', copyIndex())]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[parameters('count')]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          }
      } 
    ]

## <a name="offset-index-value"></a>Indexwaarde offset

U zult zien in het vorige voorbeeld is dat de indexwaarde van nul tot 2 gaat. U wilt verschuiven de indexwaarde, kunt u een waarde in de functie **copyIndex()** , zoals **copyIndex(1)**doorgeven. Het aantal herhalingen uit te voeren nog steeds is opgegeven in het element kopiëren, maar de waarde van copyIndex wordt gecompenseerd door de opgegeven waarde. Dus zou dezelfde sjabloon als het vorige voorbeeld, maar de **copyIndex(1)** geven drie websites met de naam implementeren:

- examplecopy-1
- examplecopy-2
- examplecopy-3

## <a name="use-copy-with-array"></a>Gebruik kopiëren met matrix
   
De kopieerbewerking is vooral handig bij het werken met arrays, omdat u elk element in de matrix kunt doorlopen. Drie websites met de naam implementeren:

- examplecopy Contoso
- examplecopy Fabrikam
- examplecopy Coho

Gebruik de volgende sjabloon:

    "parameters": { 
      "org": { 
         "type": "array", 
             "defaultValue": [ 
             "Contoso", 
             "Fabrikam", 
             "Coho" 
          ] 
      }
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', parameters('org')[copyIndex()])]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[length(parameters('org'))]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          } 
      } 
    ]

Natuurlijk stelt u het aantal op een andere waarde dan de lengte van de matrix. U kan bijvoorbeeld een matrix met meerdere waarden maken en vervolgens doorgeven in een parameterwaarde die aangeeft hoeveel van de array-elementen te implementeren. In dat geval stelt u het aantal zoals aangegeven in het eerste voorbeeld. 

## <a name="depending-on-resources-in-a-loop"></a>Afhankelijk van de bronnen in een lus

U kunt opgeven dat een resource na een andere resource worden geïmplementeerd met behulp van het element **dependsOn** . Wanneer u een resource die afhankelijk van de collectie van bronnen in een lus is te implementeren, kunt u de naam opgeven van de lus kopiëren in het element **dependsOn** . In het volgende voorbeeld ziet u hoe 3 opslag rekeningen implementeren voordat u de virtuele Machine. De volledige definitie van de virtuele Machine wordt niet weergegeven. Merk op dat het element kopiëren heeft een **naam** als **storagecopy** en het element **dependsOn** voor de virtuele Machines ook is ingesteld op **storagecopy**.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "resources": [
            {
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Storage/storageAccounts",
                "name": "[concat('storage', uniqueString(resourceGroup().id), copyIndex())]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "accountType": "Standard_LRS"
                 },
                "copy": { 
                    "name": "storagecopy", 
                    "count": 3 
                }
            },
           {
               "apiVersion": "2015-06-15", 
               "type": "Microsoft.Compute/virtualMachines", 
               "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
               "dependsOn": ["storagecopy"],
               ...
           }
        ],
        "outputs": {}
    }

## <a name="looping-on-a-nested-resource"></a>Op de bron van een geneste lus

Kunt u een lus kopiëren voor een geneste resource. Als u meerdere exemplaren van een bron die u normaal gesproken als genest binnen een ander middel definieert te maken, moet u in plaats daarvan de resource als een resource op het hoogste niveau maken en de relatie met de bron van de bovenliggende via de eigenschappen **type** en de **naam** .

Stel bijvoorbeeld dat u meestal een gegevensset definiëren als een geneste bron in een Data Factory.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "string"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
        "resources": [
        {
            "type": "datasets",
            "name": "[parameters('dataSetName')]",
            "dependsOn": [
                "[parameters('dataFactoryName')]"
            ],
            ...
        }
    }]
    
Als u wilt meerdere exemplaren van gegevenssets maken, moet u de sjabloon wijzigen, zoals hieronder wordt weergegeven. Aankondiging van de volledig gekwalificeerde type en de naam bevat de naam van de fabriek.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "array"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
    },
    {
        "type": "Microsoft.DataFactory/datafactories/datasets",
        "name": "[concat(parameters('dataFactoryName'), '/', parameters('dataSetName')[copyIndex()])]",
        "dependsOn": [
            "[parameters('dataFactoryName')]"
        ],
        "copy": { 
            "name": "datasetcopy", 
            "count": "[length(parameters('dataSetName'))]" 
        } 
        ...
    }]

## <a name="create-multiple-instances-when-copy-wont-work"></a>Maak meerdere exemplaren als kopie werkt niet

U kunt alleen **kopiëren** op de brontypen en niet op eigenschappen binnen een brontype. Dit kan problemen voor u maken als u wilt dat meerdere exemplaren van een proces dat deel uitmaakt van een resource te maken. Een gebruikelijk scenario is meerdere gegevensschijven voor een virtuele Machine maken. U kan **kopiëren** met de gegevensschijven gebruiken, omdat **dataDisks** een eigenschap van de virtuele Machine, niet een eigen resourcetype is. In plaats daarvan maakt u een matrix met zoveel gegevensschijven als u nodig hebt, en in het werkelijke aantal gegevensschijven maken doorgeven. In de definitie van de virtuele machine kunt u de functie **neemt** het aantal elementen die u daadwerkelijk wilt ophalen uit de matrix.

Een volledig voorbeeld van dit patroon is weergeven in de sjabloon voor het [maken van een VM met een dynamische selectie gegevensschijven](https://azure.microsoft.com/documentation/templates/201-vm-dynamic-data-disks-selection/) .

Hieronder ziet u de relevante onderdelen van de sjabloon voor de implementatie. Veel van de sjabloon is te selecteren in de secties die betrokken zijn bij het maken van een aantal gegevensschijven dynamisch verwijderd. U ziet de parameter **numDataDisks** kunt u door te geven van het aantal schijven te maken. 

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
    "numDataDisks": {
      "type": "int",
      "maxValue": 64,
      "metadata": {
        "description": "This parameter allows you to select the number of disks you want"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'dynamicdisk')]",
    "sizeOfDataDisksInGB": 100,
    "diskCaching": "ReadWrite",
    "diskArray": [
      {
        "name": "datadisk1",
        "lun": 0,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk1.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk2",
        "lun": 1,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk2.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk3",
        "lun": 2,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk3.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk4",
        "lun": 3,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk4.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      ...
      {
        "name": "datadisk63",
        "lun": 62,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk63.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk64",
        "lun": 63,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk64.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      }
    ]
  },
  "resources": [
    ...
    {
      "type": "Microsoft.Compute/virtualMachines",
      "properties": {
        ...
        "storageProfile": {
          ...
          "dataDisks": "[take(variables('diskArray'),parameters('numDataDisks'))]"
        },
        ...
      }
      ...
    }
  ]
}
```


## <a name="next-steps"></a>Volgende stappen
- Als u weten hoe de secties van een sjabloon wilt, Zie [Azure Resource Manager sjablonen ontwerpen](./resource-group-authoring-templates.md).
- Zie voor alle functies die u in een sjabloon gebruiken kunt, [Azure Resource Manager sjabloon functies](./resource-group-template-functions.md).
- Zie informatie over het implementeren van uw sjabloon, [een toepassing met Azure Resource Manager sjabloon distribueren](resource-group-template-deploy.md).

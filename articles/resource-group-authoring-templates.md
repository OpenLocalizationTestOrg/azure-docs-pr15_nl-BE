<properties
   pageTitle="Azure Resource Manager sjablonen ontwerpen | Microsoft Azure"
   description="Installeer Azure-toepassingen met behulp van declaratieve syntaxis voor JSON Azure Resource Manager-sjablonen maken."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="authoring-azure-resource-manager-templates"></a>Azure Resource Manager sjablonen ontwerpen

Dit onderwerp beschrijft de structuur van een sjabloon Azure Resource Manager. Het geeft de verschillende secties van een sjabloon en de eigenschappen die beschikbaar in deze secties zijn. De sjabloon bestaat uit JSON en expressies waarmee u waarden voor de implementatie samenstellen. 

De sjabloon voor resources die u al hebt geïmplementeerd, Zie [exporteren van een sjabloon Azure Resource Manager aan de bestaande bronnen](resource-manager-export-template.md). Zie voor instructies over het maken van een sjabloon [Resource Manager sjabloon scenario](resource-manager-template-walkthrough.md). Zie [Aanbevolen procedures voor het maken van sjablonen voor Azure Resource Manager](resource-manager-template-best-practices.md)voor aanbevelingen over het maken van sjablonen.

Een goede JSON-editor kunt u de taak van het maken van sjablonen vereenvoudigen. Zie voor meer informatie over het gebruik van Visual Studio met uw sjablonen [maken en implementeren van Azure resourcegroepen via Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). Zie [werken met Azure Resource Manager Templates in Visual Studio Code](resource-manager-vs-code.md)voor meer informatie over het gebruik van de Code van de VS.

Limiet van uw sjabloon op 1 MB en elk parameterbestand 64 kB. De limiet van 1 MB is van toepassing op de definitieve status van de sjabloon nadat is uitgebreid met iteratieve resource definities en waarden voor variabelen en parameters. 

## <a name="template-format"></a>Sjabloon

In de meest eenvoudige structuur bevat een sjabloon de volgende elementen:

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

| Elementnaam   | Vereist | Beschrijving
| :------------: | :------: | :----------
| $schema        |   Ja    | De locatie van het schemabestand JSON beschrijving van de versie van de taal van de sjabloon. Gebruik de URL in het voorgaande voorbeeld.
| contentVersion |   Ja    | De versie van de sjabloon (bijvoorbeeld 1.0.0.0). U kunt een waarde opgeven voor dit element. Als u resources met behulp van de sjabloon implementeert, kan deze waarde worden gebruikt om ervoor te zorgen dat de juiste sjabloon wordt gebruikt.
| parameters     |   Nee     | Waarden die beschikbaar zijn wanneer de implementatie voor het aanpassen van de implementatie van de resource wordt uitgevoerd.
| variabelen      |   Nee     | De waarden die worden gebruikt als JSON fragmenten in de sjabloon sjabloon language-expressies te vereenvoudigen.
| bronnen      |   Ja    | Brontypen die worden geïnstalleerd of bijgewerkt in een resourcegroep.
| uitgangen        |   Nee     | De waarden die worden geretourneerd na de implementatie.

Wij onderzoeken de gedeelten van de sjabloon nader verderop in dit onderwerp. Nu, zullen wij nagaan of sommige van de syntaxis van de sjabloon maakt.

## <a name="expressions-and-functions"></a>Expressies en functies

De basissyntaxis van de sjabloon is JSON. Expressies en functies echter uitbreiden de JSON die beschikbaar is in de sjabloon. Met expressies kunt u waarden die geen strikte letterlijke waarden maken. Expressies tussen haakjes [en] en worden geëvalueerd wanneer de sjabloon wordt geïmplementeerd. Expressies kunnen plaatsen in een string-waarde JSON en altijd een andere JSON-waarde als resultaat geven. Als u wilt gebruiken een letterlijke tekenreeks die met een beugel begint [, moet u twee haakjes [[.

Meestal gebruikt u expressies met functies voor het uitvoeren van bewerkingen voor het configureren van de implementatie. Net als in JavaScript, worden functieaanroepen opgemaakt als **functionName(arg1,arg2,arg3)**. U verwijzen naar eigenschappen met behulp van de operatoren punt en [index].

In het volgende voorbeeld ziet u hoe u met verschillende functies bij het construeren van waarden:
 
    "variables": {
       "location": "[resourceGroup().location]",
       "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
       "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

Zie [Bronbeheer Azure sjabloon functies](resource-group-template-functions.md)voor de volledige lijst van functies van de sjabloon. 


## <a name="parameters"></a>Parameters

U opgeven welke waarden u kunt bij het implementeren van de resources invoeren in het gedeelte parameters van de sjabloon. De parameterwaarden voor deze kunnen u de installatie aanpassen door waarden die zijn aangepast voor een bepaalde omgeving (zoals dev, test en productie). U hoeft niet te worden voorzien van parameters in de sjabloon, maar zonder parameters de sjabloon altijd dezelfde resources met dezelfde namen, locaties en eigenschappen wilt implementeren.

Deze parameterwaarden in de sjabloon kunt u waarden instellen voor de gebruikte bronnen. Alleen de parameters die zijn gedeclareerd in de sectie parameters kunnen worden gebruikt in andere gedeelten van de sjabloon.

U definieert de parameters met de volgende structuur:

    "parameters": {
       "<parameter-name>" : {
         "type" : "<type-of-parameter-value>",
         "defaultValue": "<default-value-of-parameter>",
         "allowedValues": [ "<array-of-allowed-values>" ],
         "minValue": <minimum-value-for-int>,
         "maxValue": <maximum-value-for-int>,
         "minLength": <minimum-length-for-string-or-array>,
         "maxLength": <maximum-length-for-string-or-array-parameters>,
         "metadata": {
             "description": "<description-of-the parameter>" 
         }
       }
    }

| Elementnaam   | Vereist | Beschrijving
| :------------: | :------: | :----------
| parameterName  |   Ja    | De naam van de parameter. Moet een geldige id in de JavaScript.
| type           |   Ja    | De waarde van de parameter type. Zie de onderstaande lijst van toegestane typen.
| Standaardwaarde   |   Nee     | De standaardwaarde voor de parameter, als er geen waarde is opgegeven voor de parameter.
| allowedValues  |   Nee     | Matrix van toegestane waarden voor de parameter om ervoor te zorgen dat de juiste waarde beschikbaar is.
| minValue       |   Nee     | De minimumwaarde voor int typeparameters, deze waarde is inclusief.
| maxValue       |   Nee     | De maximale waarde voor int typeparameters, deze waarde is inclusief.
| minLength      |   Nee     | De minimale lengte van de tekenreeks, secureString en matrix typeparameters, deze waarde is inclusief.
| maxLength      |   Nee     | De maximale lengte voor string, secureString en matrix typeparameters, deze waarde is inclusief.
| Beschrijving    |   Nee     | Beschrijving van de parameter die wordt weergegeven voor gebruikers van de sjabloon via de interface portal aangepaste sjabloon.

De toegestane typen en waarden zijn:

- **tekenreeks**
- **secureString**
- **int**
- **BOOL**
- **object** 
- **secureObject**
- **matrix**

Als u een parameter als optioneel, bieden een standaardwaarde (een lege tekenreeks kan worden). 

Als u een naam die met de parameters in de opdracht overeenkomt voor de implementatie van de sjabloon voor de parameter opgeeft, wordt u gevraagd om een waarde opgeven voor een parameter met de postfix- **FromTemplate**. Bijvoorbeeld, als u een parameter met de naam **ResourceGroupName** in de sjabloon die is hetzelfde als de parameter **ResourceGroupName** in de [Nieuwe AzureRmResourceGroupDeployment] [ deployment2cmdlet] cmdlet, moet u een waarde opgeven voor **ResourceGroupNameFromTemplate**. In het algemeen, voorkomt u deze verwarring door de naam van parameters niet met dezelfde naam als de parameters die worden gebruikt voor de implementatie.

>[AZURE.NOTE] Alle wachtwoorden, sleutels en andere geheimen te gebruiken **secureString** . Sjabloonparameters met het type secureString na de implementatie van de resource niet lezen. 

In het volgende voorbeeld ziet u hoe om parameters te definiëren:

    "parameters": {
      "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
      },
      "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
      },
      "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
      },
      "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
      }
    }

Voor de invoer van de parameterwaarden tijdens de implementatie, Zie van [een toepassing met Azure Resource Manager sjabloon distribueren](resource-group-template-deploy.md#parameter-file). 

## <a name="variables"></a>Variabelen

In de sectie variabelen kunt u waarden die kunnen worden gebruikt in de sjabloon maken. Variabelen zijn meestal gebaseerd op waarden van de parameters worden opgegeven. U hoeft geen variabelen definiëren, maar ze vaak uw sjabloon vereenvoudigen door minder complexe expressies.

U definieert de variabelen met de volgende structuur:

    "variables": {
       "<variable-name>": "<variable-value>",
       "<variable-name>": { 
           <variable-complex-type-value> 
       }
    }

In het volgende voorbeeld ziet u hoe een variabele die is samengesteld uit twee waarden van de parameters definiëren:

     "variables": {
       "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
    }

In het volgende voorbeeld ziet u een variabele die een complexType JSON en variabelen die zijn opgebouwd uit andere variabelen:

    "parameters": {
       "environmentName": {
         "type": "string",
         "allowedValues": [
           "test",
           "prod"
         ]
       }
    },
    "variables": {
       "environmentSettings": {
         "test": {
           "instancesSize": "Small",
           "instancesCount": 1
         },
         "prod": {
           "instancesSize": "Large",
           "instancesCount": 4
         }
       },
       "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
       "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
       "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
    }

## <a name="resources"></a>Bronnen

In de sectie bronnen definieert u de bronnen die worden geïnstalleerd of bijgewerkt. In deze sectie krijgt ingewikkeld omdat u dat de typen die u implementeert begrijpen moet om de juiste waarden. 

Definieert u resources met de volgende structuur:

    "resources": [
       {
         "apiVersion": "<api-version-of-resource>",
         "type": "<resource-provider-namespace/resource-type-name>",
         "name": "<name-of-the-resource>",
         "location": "<location-of-resource>",
         "tags": "<name-value-pairs-for-resource-tagging>",
         "comments": "<your-reference-notes>",
         "dependsOn": [
           "<array-of-related-resource-names>"
         ],
         "properties": "<settings-for-the-resource>",
         "copy": {
           "name": "<name-of-copy-loop>",
           "count": "<number-of-iterations>"
         }
         "resources": [
           "<array-of-child-resources>"
         ]
       }
    ]

| Elementnaam             | Vereist | Beschrijving
| :----------------------: | :------: | :----------
| apiVersion               |   Ja    | Versie van de REST-API gebruiken voor het maken van de resource.
| type                     |   Ja    | Het type van de resource. Deze waarde is een combinatie van de naamruimte van de resource-provider en het resourcetype (bijvoorbeeld **Microsoft.Storage/storageAccounts**).
| naam                     |   Ja    | De naam van de resource. De naam moet voldoen aan URI component beperkingen zijn gedefinieerd in RFC3986. Azure services die de naam van de resource voor het valideren van de naam om te controleren of deze buiten de partijen is ook niet een poging tot het vervalsen van een andere identiteit. Zie de [naam van de resource](https://msdn.microsoft.com/library/azure/mt219035.aspx).
| locatie                 |   Varieert  | Geo-locaties van de opgegeven resource ondersteund. U kunt kiezen uit de beschikbare locaties, maar meestal is het zinvol te kiezen die dicht bij uw gebruikers. Meestal is het ook verstandig om interactie met elkaar in hetzelfde gebied. De meeste brontypen vereist een locatie, maar sommige bestandstypen (zoals een roltoewijzing) hoeven niet een locatie.
| tags                     |   Nee     | Labels die gekoppeld aan de bron zijn.
| opmerkingen                 |   Nee     | Uw notities voor het documenteren van de resources in uw sjabloon
| dependsOn                |   Nee     | Bronnen die afhankelijk zijn van de resource wordt gedefinieerd. De afhankelijkheden tussen resources worden geëvalueerd en resources worden ingezet in de volgorde van hun afhankelijk. Wanneer de bronnen niet van elkaar afhankelijk zijn, worden zij parallel geïmplementeerd. De waarde kan een door komma's gescheiden lijst van een resource worden namen of unieke resource-id's.
| Eigenschappen               |   Nee     | Resource-specifieke configuratie-instellingen. De waarden voor de eigenschappen zijn hetzelfde als de waarden die u in het hoofdgedeelte van de aanvraag voor de REST API-bewerking (methode PUT opgeeft) om de bron te maken. Documentatie bij resource-schema of REST API Zie [Bronbeheer providers, regio's, API-versies en schema's](resource-manager-supported-services.md).
| kopiëren                     |   Nee     | Als meer dan één exemplaar nodig is, het aantal resources te maken. Zie [meerdere instanties maken van bronnen in Azure Resource Manager](resource-group-create-multiple.md)voor meer informatie. |
| bronnen                |   Nee     | Onderliggende bronnen die afhankelijk zijn van de resource wordt gedefinieerd. U kunt alleen de brontypen die zijn toegestaan door het schema van de bron van de bovenliggende bieden. De volledig gekwalificeerde naam van het brontype van de onderliggende bevat de bovenliggende brontype, zoals **Microsoft.Web/sites/extensions**. Afhankelijkheid van de bovenliggende resource wordt niet geïmpliceerd; die afhankelijkheid moet expliciet worden gedefinieerd. 

Weet welke waarden u moet opgeven voor **apiVersion**, is **en de **locatie** **niet onmiddellijk duidelijk. Gelukkig kunt u deze waarden via Azure PowerShell of Azure CLI bepalen.

Als u alle resources weer met **PowerShell**, gebruikt u:

    Get-AzureRmResourceProvider -ListAvailable

Uit de resulterende lijst vindt u geïnteresseerd bent in de resource-providers. Als u de resourcetypen voor de voorziening van een resource (bijvoorbeeld opslag), gebruikt:

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes

Als u de API-versies voor een brontype (dergelijke opslag rekeningen), gebruikt u:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).ApiVersions

Als u ondersteunde locaties voor een resourcetype, gebruikt u:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).Locations

Als u alle resources providers met **Azure CLI**, gebruikt u:

    azure provider list

Uit de resulterende lijst vindt u geïnteresseerd bent in de resource-providers. Als u de resourcetypen voor de voorziening van een resource (bijvoorbeeld opslag), gebruikt:

    azure provider show Microsoft.Storage

Als u ondersteunde locaties en API-versies, gebruikt u:

    azure provider show Microsoft.Storage --details --json

Zie voor meer informatie over resource-providers, [aanbieders van Resource Manager, regio's, API-versies en schema's](resource-manager-supported-services.md).

De sectie bronnen bevat een matrix van de middelen te implementeren. Binnen elke bron, ook kunt u een array van onderliggende bronnen. De sectie bronnen kan daarom een structuur zoals hebben:

    "resources": [
       {
           "name": "resourceA",
       },
       {
           "name": "resourceB",
           "resources": [
               {
                   "name": "firstChildResourceB",
               },
               {   
                   "name": "secondChildResourceB",
               }
           ]
       },
       {
           "name": "resourceC",
       }
    ]


In het volgende voorbeeld ziet u een resource **Microsoft.Web/serverfarms** en een **Microsoft.Web/sites** middel met een onderliggende bron **extensies** . U ziet dat de site is gemarkeerd als afhankelijk is van de server-farm, omdat de server-farm moet bestaan voordat de site kan worden geïmplementeerd. Te merken dat de resource **extensies** een onderliggend element van de site is.

    "resources": [
      {
        "apiVersion": "2015-08-01",
        "name": "[parameters('hostingPlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "HostingPlan"
        },
        "sku": {
          "name": "[parameters('skuName')]",
          "capacity": "[parameters('skuCapacity')]"
        },
        "properties": {
          "name": "[parameters('hostingPlanName')]",
          "numberOfWorkers": 1
        }
      },
      {
        "apiVersion": "2015-08-01",
        "type": "Microsoft.Web/sites",
        "name": "[parameters('siteName')]",
        "location": "[resourceGroup().location]",
        "tags": {
          "environment": "test",
          "team": "Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Web/serverFarms/', parameters('hostingPlanName'))]"
        ],
        "properties": {
          "name": "[parameters('siteName')]",
          "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
        },
        "resources": [
          {
            "apiVersion": "2015-08-01",
            "type": "extensions",
            "name": "MSDeploy",
            "dependsOn": [
              "[concat('Microsoft.Web/sites/', parameters('siteName'))]"
            ],
            "properties": {
              "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
              "dbType": "None",
              "connectionString": "",
              "setParameters": {
                "Application Path": "[parameters('siteName')]"
              }
            }
          }
        ]
      }
    ]


## <a name="outputs"></a>Uitgangen

In het gedeelte uitvoer geeft u waarden die worden geretourneerd van de implementatie. U kan bijvoorbeeld de URI voor toegang tot een bron geïmplementeerde terug.

In het volgende voorbeeld ziet u de structuur van een definitie van de uitvoer:

    "outputs": {
       "<outputName>" : {
         "type" : "<type-of-output-value>",
         "value": "<output-value-expression>"
       }
    }

| Elementnaam   | Vereist | Beschrijving
| :------------: | :------: | :----------
| outputName     |   Ja    | De naam van de uitvoerwaarde. Moet een geldige id in de JavaScript.
| type           |   Ja    | Het type van de uitvoerwaarde. Uitvoerwaarden ondersteuning van hetzelfde type als de sjabloonparameters als invoerparameters.
| waarde          |   Ja    | Sjabloon voor taal-expressie die wordt geëvalueerd en wordt geretourneerd als waarde voor uitvoer.


In het volgende voorbeeld ziet u een waarde die wordt geretourneerd in het gedeelte uitvoer.

    "outputs": {
       "siteUri" : {
         "type" : "string",
         "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
       }
    }

Zie voor meer informatie over het werken met uitvoer [delen staat in Azure Resource Manager-sjablonen](best-practices-resource-manager-state.md).

## <a name="next-steps"></a>Volgende stappen
- Zie de [Azure Quickstart sjablonen](https://azure.microsoft.com/documentation/templates/)volledige sjablonen voor verschillende soorten oplossingen.
- Zie voor meer informatie over de functies die u uit binnen een sjabloon kunt [Azure Resource Manager sjabloon functies](resource-group-template-functions.md).
- Zie [werken met gekoppelde sjablonen met Azure Resource Manager](resource-group-linked-templates.md)wilt combineren meerdere sjablonen tijdens de implementatie.
- U wilt resources in een resourcegroep gebruiken. In dit scenario wordt gebruikt bij het werken met accounts van opslag of virtuele netwerken die worden gedeeld door meerdere bronnengroepen. Voor meer informatie, Zie de [functie resourceId](resource-group-template-functions.md#resourceid).


[deployment2cmdlet]: https://msdn.microsoft.com/library/mt740620(v=azure.200).aspx

<properties
    pageTitle="Hoge dichtheid van Azure App Service hosting | Microsoft Azure"
    description="Hoge dichtheid hosting op Azure App Service"
    authors="btardif"
    manager="wpickett"
    editor=""
    services="app-service\web"
    documentationCenter=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"/>

# <a name="high-density-hosting-on-azure-app-service"></a>Hoge dichtheid hosting op Azure App Service

Bij het gebruik van App-Service, wordt uw toepassing ontkoppeld van de capaciteit is toegewezen door twee begrippen:

- **De toepassing:** Hiermee geeft u de app en runtime configuratie. Het bevat bijvoorbeeld de versie van .NET, dat de runtime laden moet, de app instellingen, enz.

- **Het abonnement App:** Hiermee definieert u de kenmerken van de capaciteit, beschikbare functies en de plaats van de toepassing. Kenmerken zijn mogelijk grote (vier cores) machine, vier exemplaren, Premium-functies in Oost-VS.

Een app is altijd gekoppeld aan een serviceplan App, maar een App serviceplan capaciteit van een of meer toepassingen kan bieden.

Het platform biedt daardoor de flexibiliteit om te isoleren van een app enkele of meerdere apps bronnen te delen door het delen van een App serviceplan hebt.

Echter wanneer er meerdere apps delen een serviceplan App, wordt een exemplaar van deze app in elk exemplaar van dat plan App-Service.

## <a name="per-app-scaling"></a>Per app schalen
*Per app schalen* is een functie die kan worden ingeschakeld op het niveau van de App Service plan en vervolgens per toepassing worden gebruikt.

Per app schaalt schaal toepassing onafhankelijk van de App serviceplan die als host fungeert. Op deze manier een serviceplan App kan worden geconfigureerd voor het leveren van 10 exemplaren, maar een app voor slechts 5 kan worden ingesteld.

De volgende Azure Resource Manager-sjabloon maakt een App serviceplan schaalvergroting 10 exemplaren en een app die geconfigureerd voor gebruik per app schalen en schalen tot slechts 5 exemplaren.

Het abonnement App is de eigenschap **scaling per site** instellen op true ( `"perSiteScaling": true`). De app is als het **aantal werknemers** met 5 (`"properties": { "numberOfWorkers": "5" }`).

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters":{
            "appServicePlanName": { "type": "string" },
            "appName": { "type": "string" }
         },
        "resources": [
        {
            "comments": "App Service Plan with per site perSiteScaling = true",
            "type": "Microsoft.Web/serverFarms",
            "sku": {
                "name": "S1",
                "tier": "Standard",
                "size": "S1",
                "family": "S",
                "capacity": 10
                },
            "name": "[parameters('appServicePlanName')]",
            "apiVersion": "2015-08-01",
            "location": "West US",
            "properties": {
                "name": "[parameters('appServicePlanName')]",
                "perSiteScaling": true
            }
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[parameters('appName')]",
            "apiVersion": "2015-08-01-preview",
            "location": "West US",
            "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
            "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
            "resources": [ {
                    "comments": "",
                    "type": "config",
                    "name": "web",
                    "apiVersion": "2015-08-01",
                    "location": "West US",
                    "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                    "properties": { "numberOfWorkers": "5" }
             } ]
         }]
    }


## <a name="recommended-configuration-for-high-density-hosting"></a>Aanbevolen configuratie voor het hosten van hoge dichtheid

Per app schalen is een functie die in openbare Azure gebieden en omgevingen met App-Service is ingeschakeld. De aanbevolen strategie is echter App Service-omgevingen gebruiken om gebruik te maken van hun geavanceerde functies en de grotere capaciteit van toepassingen.  

Ga als volgt te werk om met hoge dichtheid die als host fungeert voor uw toepassingen:

1. De omgeving van App-Service configureren en kies een groep werknemers die op het scenario voor de host met hoge dichtheid gericht is.

1. Een enkel App Service plan maken en dit gebruik van de beschikbare capaciteit op de toepassingen van de werknemer.

1. De schaal vlag per site ingesteld op true op het plan App-Service.

1. Nieuwe sites zijn gemaakt en toegewezen aan dat plan App-Service met de eigenschap **numberOfWorkers** is ingesteld op **1**. Met deze configuratie levert de hoogste dichtheid mogelijk op deze werknemer van toepassingen.

1. Het aantal werknemers kan afzonderlijk worden geconfigureerd per site aanvullende bronnen verlenen indien nodig. Zo kan een site intensief gebruik **numberOfWorkers** ingesteld op **3** om meer verwerkingscapaciteit voor deze app, terwijl het lage gebruik sites zou **numberOfWorkers** ingesteld op **1**.

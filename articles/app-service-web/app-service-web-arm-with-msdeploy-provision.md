<properties
    pageTitle="Implementeren van een web app met behulp van MSDeploy met een hostnaam en een ssl-certificaat"
    description="Een sjabloon Azure Resource Manager gebruiken voor de implementatie van een web app met behulp van MSDeploy en het instellen van aangepaste hostnaam en een SSL-certificaat"
    services="app-service\web"
    manager="erikre"
    documentationCenter=""
    authors="jodehavi"
    />

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="john.dehavilland"/>

# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>Implementeren van een web app met MSDeploy, aangepaste hostname en SSL-certificaat.

Deze handleiding helpt bij het maken van een end-to-end implementatie voor een Azure Web App, gebruik te maken van MSDeploy, alsmede een aangepaste hostnaam en een SSL-certificaat toe te voegen aan de sjabloon ARM.

Zie voor meer informatie over het maken van sjablonen [Azure Resource Manager sjablonen ontwerpen](../resource-group-authoring-templates.md).

###<a name="create-sample-application"></a>Voorbeeldtoepassing maken

U implementeren een ASP.NET-webtoepassing. De eerste stap is het maken van een eenvoudige webtoepassing (of kunt u gebruik een bestaande - in dat geval kunt u deze stap overslaan).

Open de Visual Studio 2015 en kies Bestand > Nieuw Project. Kies in het dialoogvenster dat wordt weergegeven Web > ASP.NET-webtoepassingen. Kies Web onder sjablonen en kies de sjabloon MVC. Selecteer _wijziging verificatietype_ _Geen_verificatie. Dit is het net zo eenvoudig mogelijk maken van de voorbeeldtoepassing.

Op dit punt hebt u een basic ASP.Net-webtoepassing gereed voor gebruik als onderdeel van het implementatieproces.

###<a name="create-msdeploy-package"></a>MSDeploy-pakket maken

Volgende stap is het maken van het pakket voor de implementatie van de web app met Azure. U doet dit door uw project opslaan en vervolgens de volgende uitvoeren vanaf de opdrachtregel:

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

Hiermee maakt u een ZIP-pakket in de map PackageLocation. De toepassing is nu klaar om te worden geïmplementeerd, die u nu kunt samenstellen uit een sjabloon Azure Resource Manager doen.

###<a name="create-arm-template"></a>ARM-sjabloon maken
Eerst laten we beginnen met een ARM standaardsjabloon waarop het maken van een webtoepassing en een hosting plan (noot parameters en variabelen niet voor kort te houden weergegeven worden).

    {
        "name": "[parameters('appServicePlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "apiVersion": "2014-06-01",
        "dependsOn": [ ],
        "tags": {
            "displayName": "appServicePlan"
        },
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "sku": "[parameters('appServicePlanSKU')]",
            "workerSize": "[parameters('appServicePlanWorkerSize')]",
            "numberOfWorkers": 1
        }
    },
    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        }
    }

Vervolgens moet u de resource web app krijgen een geneste MSDeploy bron wijzigen. Hierdoor kunt u verwijzen naar het pakket eerder hebt gemaakt en Azure Resource Manager MSDeploy gebruiken voor het implementeren van het pakket op de Azure WebApp Vertel. Hier ziet u de bron van de Microsoft.Web/sites met de geneste MSDeploy bron:

    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        },
        "resources": [
            {
                "name": "MSDeploy",
                "type": "extensions",
                "location": "[resourceGroup().location]",
                "apiVersion": "2015-08-01",
                "dependsOn": [
                    "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
                ],
                "tags": {
                    "displayName": "webDeploy"
                },
                "properties": {
                    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                        "IIS Web Application Name": "[variables('webAppName')]"
                    }
                }
            }
        ]
    }

Nu zult u zien dat de resource MSDeploy neemt een **packageUri** -eigenschap die wordt als volgt gedefinieerd:

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

Deze **packageUri** wordt de opslag account uri die wijst naar de opslag account waar u uploadt uw zip pakket aan. De bronnenbeheerder Azure maken gebruik van [Gedeelde toegang handtekeningen](../storage/storage-dotnet-shared-access-signature-part-1.md) te halen het pakket lokaal van de opslag-account wanneer u de sjabloon implementeert. Dit proces wordt uitgevoerd via een PowerShell script die het pakket uploaden en roept de Azure Management API om de sleutels te maken nodig is en die in de sjabloon doorgeven als parameters (*_artifactsLocation* en *_artifactsLocationSasToken*). Moet u de parameters definiëren voor de map en bestandsnaam van het pakket is geüpload naar onder de container opslag.

Vervolgens moet u in een andere geneste resource voor het instellen van de bindingen hostnaam om te profiteren van een aangepast domein toe te voegen. Eerst moet u ervoor zorgen dat u eigenaar bent van de hostnaam en ingesteld kan worden gecontroleerd door Azure dat u bezit - Zie [een aangepaste domeinnaam in Azure App-Service configureren](web-sites-custom-domain-name.md). Zodra u dat hebt gedaan, kunt u de volgende toevoegen aan uw sjabloon in de sectie Microsoft.Web/sites resource:

    {
        "apiVersion": "2015-08-01",
        "type": "hostNameBindings",
        "name": "www.yourcustomdomain.com",
        "dependsOn": [
            "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
        ],
        "properties": {
            "domainId": null,
            "hostNameType": "Verified",
            "siteName": "variables('webAppName')"
        }
    }

Ten slotte moet u nog een bovenste niveau resource, Microsoft.Web/certificates. Deze bron bevat uw SSL-certificaat en op hetzelfde niveau als uw web app en hosting plan zal bestaan.

    {
        "name": "[parameters('certificateName')]",
        "apiVersion": "2014-04-01",
        "type": "Microsoft.Web/certificates",
        "location": "[resourceGroup().location]",
        "properties": {
            "pfxBlob": "pfx base64 blob",
            "password": "some pass"
        }
    }

U moet een geldig SSL-certificaat hebben om deze resource instellen. Als u eenmaal die geldig certificaat hebt moet u het pfx-bytes ophalen als een base64-tekenreeks. Een optie om aan te pakken dit is de volgende PowerShell-opdracht gebruiken:

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

U kan vervolgens dit als parameter doorgeven aan uw sjabloon ARM implementatie.

Op dit moment is de ARM-sjabloon klaar.

###<a name="deploy-template"></a>Sjabloon distribueren

De laatste stappen zijn om dit allemaal samen in een volledige end-to-end implementatie. Voor een eenvoudiger implementatie dat kunt u gebruikmaken van het **Distribueren AzureResourceGroup.ps1** PowerShell-script dat bij het maken van een resourcegroep Azure-project in Visual Studio om te helpen bij het uploaden van alle artefacten vereist in de sjabloon wordt toegevoegd. Deze moet u een opslag account die u wilt gebruiken voor de tijd hebt gemaakt. In dit voorbeeld moet ik een account voor gedeelde opslag voor de package.zip te uploaden gemaakt. Het script maken gebruik van AzCopy om het pakket te uploaden naar de rekening voor opslag. U doorgeven op de locatie van de map artefact en het script zal alle bestanden in die map automatisch uploaden naar de container met de naam storage. U hebt na het aanroepen van distribueren AzureResourceGroup.ps1 werk vervolgens de bindingen SSL toewijzen van de hostnaam aangepast met uw SSL-certificaat.

De volgende PowerShell ziet u de volledige implementatie roepen de implementeren-AzureResourceGroup.ps1:

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script to deploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app to bind ssl certificate to hostname. This has to be done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

Op dit moment uw toepassing moet zijn geïmplementeerd en u moet mogelijk bladeren aan via https://www.yourcustomdomain.com

<properties
   pageTitle="Hadoop Windows-gebaseerde clusters in HDInsight met behulp van bronbeheer Azure sjablonen maken | Microsoft Azure"
    description="Informatie over het maken van clusters voor Azure HDInsight met behulp van bronbeheer Azure-sjablonen."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-resource-manager-templates"></a>Hadoop Windows-gebaseerde clusters in HDInsight met behulp van bronbeheer Azure sjablonen maken

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Informatie over het HDInsight clusters met behulp van bronbeheer Azure sjablonen maken. Zie [een toepassing met Azure Resource Manager sjabloon distribueren](../resource-group-template-deploy.md)voor meer informatie. Maken van het cluster andere hulpprogramma's en functies klikt u op het tabblad selecteren boven aan deze pagina of voor [methoden voor het Cluster maken](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Voorwaarden:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Voordat u de instructies in dit artikel, hebt u het volgende:

- [Azure-abonnement](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell of Azure CLI

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

### <a name="access-control-requirements"></a>Access controle-eisen

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="resource-manager-templates"></a>Sjablonen voor bronbeheer

Resource Manager sjabloon gemakkelijk voor het maken van clusters van HDInsight, hun afhankelijke bronnen (zoals de standaard opslag-account) en andere bronnen (zoals Azure SQL-Database te gebruiken van Apache Sqoop) voor de toepassing in een enkele, gecoördineerde werking. In de sjabloon voor de resources die nodig zijn voor de toepassing definiëren en implementatie-parameters voor het invoeren van waarden voor verschillende omgevingen opgeven. De sjabloon bestaat uit JSON en expressies waarmee u waarden voor de implementatie samenstellen.

Een sjabloon Resource Manager voor het maken van een cluster van HDInsight en de afhankelijke Azure opslag account vindt u in [Bijlage A](#appx-a-arm-template). Gebruik een teksteditor om de sjabloon opslaan in een bestand op uw werkstation. Hier leert u hoe de sjabloon met behulp van verschillende hulpprogramma's.

Zie voor meer informatie over de sjabloon Resource Manager

- [Auteur Azure Resource Manager-sjablonen](../resource-group-authoring-templates.md)
- [Een toepassing met de sjabloon Azure Resource Manager](../resource-group-template-deploy.md)


## <a name="deploy-with-powershell"></a>Met PowerShell implementeren

De volgende procedure maakt een cluster HDInsight.

**Implementatie van een cluster met behulp van bronbeheer sjabloon**

1. Sla het bestand json in [bijlage A](#appx-a-arm-template) bij uw werkstation.
2. De parameters instellen als nodig is.
3. De sjabloon met behulp van de volgende PowerShell script uitvoeren:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>" 
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and varialbes
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage accounge
        $parameters = @{clusterName="$hdinsightClusterName";clusterStorageAccountName="$defaultStorageAccountName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName 

    De PowerShell-script configureert alleen de naam van het cluster en de naam van de opslag.  U kunt andere waarden in de sjabloon voor bronbeheer instellen. 
    
Zie [met PowerShell distribueren](../resource-group-template-deploy.md#deploy-with-powershell)voor meer informatie.

## <a name="deploy-with-azure-cli"></a>Implementatie van CLI Azure

In het volgende voorbeeld wordt een cluster en de account voor opslag afhankelijk en container gemaakt door het aanroepen van een Resource Manager-sjabloon:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US 2"
    azure group deployment create "hdi1229rg" "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json" -p "{\"clusterName\":{\"value\":\"hdi1229win\"},\"clusterStorageAccountName\":{\"value\":\"hdi1229store\"},\"location\":{\"value\":\"East US 2\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"}}"





## <a name="deploy-with-rest-api"></a>Met REST API implementeren

Zie [implementeren met de REST API](../resource-group-template-deploy.md#deploy-with-the-rest-api).

## <a name="deploy-with-visual-studio"></a>Implementeren met Visual Studio

Met Visual Studio kunt u een groep resourceproject maken en deze implementeren in Azure via de gebruikersinterface. Selecteert u het type van de resources in uw project wilt opnemen en deze resources automatisch worden toegevoegd aan de sjabloon Resource Manager. Het project biedt tevens een PowerShell script voor de implementatie van de sjabloon.

Zie voor een inleiding tot het gebruik van Visual Studio met resourcegroepen, [maken en implementeren van Azure resourcegroepen via Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

##<a name="next-steps"></a>Volgende stappen
In dit artikel, kunt u op verschillende manieren voor het maken van een HDInsight-cluster hebt geleerd. Voor meer informatie, Zie de volgende artikelen:


- Zie [bronnen implementeren met behulp van .NET bibliotheken en een sjabloon](../virtual-machines/virtual-machines-windows-csharp-template.md)voor een voorbeeld van de implementatie van bronnen via de .NET client library.
- Zie voor een uitgebreidere voorbeeld van de implementatie van een toepassing [bepaling en microservices voorspelbaar in Azure geïmplementeerd](../app-service-web/app-service-deploy-complex-application-predictably.md).
- Zie [ontwikkel- en testomgevingen in Microsoft Azure](../solution-dev-test-environments.md)voor instructies over het implementeren van uw oplossing in verschillende omgevingen.
- Meer informatie over de gedeelten van de sjabloon Azure Resource Manager, Zie [sjablonen ontwerpen](../resource-group-authoring-templates.md).
- Zie [functies van de sjabloon](../resource-group-template-functions.md)voor een lijst van de functies die u in een sjabloon Azure Resource Manager gebruiken kunt.



##<a name="appx-a-resource-manager-template"></a>Sjabloon toepassingX A: Resource Manager

De volgende Azure bronnenbeheerder doorgegeven sjabloon maakt een Hadoop Windows gebaseerde cluster met de afhankelijke Azure opslag account.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
        "type": "string",
        "defaultValue": "East US 2",
        "allowedValues": [
            "Central US",
            "North Europe",
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Brizil South",
            "Australia East",
            "Australia Southeast",
            "Central India"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password for the cluster login."
        }
        },
        "clusterStorageAccountName": {
        "type": "string",
        "metadata": {
            "description": "The name of the storage account to be created and be used as the cluster's storage."
        }
        },
        "clusterStorageType": {
        "type": "string",
        "defaultValue": "Standard_LRS",
        "allowedValues": [
            "Standard_LRS",
            "Standard_GRS",
            "Standard_ZRS"
        ]
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 4,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
        "variables": {},
        "resources": [
            {
            "name": "[parameters('clusterStorageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2015-05-01-preview",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "accountType": "[parameters('clusterStorageType')]"
            }
            },
            {
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"
            ],
            "tags": {},
            "properties": {
                "clusterVersion": "3.2",
                "osType": "Windows",
                "clusterDefinition": {
                "kind": "hadoop",
                "configurations": {
                    "gateway": {
                    "restAuthCredential.isEnabled": true,
                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                    }
                }
                },
                "storageProfile": {
                "storageaccounts": [
                    {
                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                    "isDefault": true,
                    "container": "[parameters('clusterName')]",
                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                    }
                ]
                },
                "computeProfile": {
                "roles": [
                    {
                    "name": "headnode",
                    "targetInstanceCount": "1",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    },
                    {
                    "name": "workernode",
                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    }
                ]
                }
            }
            }
        ],
        "outputs": {
            "cluster": {
            "type": "object",
            "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
            }
        }
    }


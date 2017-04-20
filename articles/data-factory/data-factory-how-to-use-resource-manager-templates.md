<properties 
    pageTitle="Sjablonen voor bronbeheer gebruiken in Data Factory | Microsoft Azure" 
    description="Informatie over het maken en gebruiken van sjablonen Azure Resource Manager Data Factory-entiteiten maken." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="shlo"/>

# <a name="use-templates-to-create-azure-data-factory-entities"></a>Azure Data Factory-entiteiten maken met behulp van sjablonen

## <a name="overview"></a>Overzicht
Terwijl Azure Data Factory voor uw behoeften integratie van gegevens, u zult zien dat hetzelfde patroon opnieuw te gebruiken in verschillende omgevingen of in dezelfde taak wanneer binnen dezelfde oplossing implementeren. Sjablonen helpen u implementeren en beheren van deze scenario's op eenvoudige wijze. Sjablonen in Azure Data Factory zijn ideaal voor scenario's voor hergebruik en herhaling.
 
Neem de situatie waarbij een organisatie fabrieken in 10 over de hele wereld heeft. De logboeken van de planten worden opgeslagen in een afzonderlijke op lokalen SQL Server-database. Het bedrijf wil een gegevensmagazijn enkele in de cloud voor ad-hoc analytics bouwen. Ook wil het dezelfde logica, maar verschillende configuraties voor ontwikkeling, test en productie-omgevingen. 

In dit geval moet een taak worden herhaald in dezelfde omgeving, maar met verschillende waarden voor de fabrieken 10 gegevens voor iedere fabriek. **Herhaling** is in feite aanwezig. Templating kunnen de onttrekking van deze algemene overdracht (dat wil zeggen, pijpleidingen met dezelfde activiteiten in elke data factory genoemd), maar maakt gebruik van een afzonderlijke parameterbestand voor iedere fabriek.

Bovendien als de organisatie wil implementeren van deze fabrieken 10 gegevens meerdere keren in verschillende omgevingen, kunt sjablonen dit **hergebruik** door het gebruik van afzonderlijke parameterbestanden voor ontwikkeling, test en productie-omgevingen.

## <a name="templating-with-azure-resource-manager"></a>Templating met Azure Resource Manager
[Azure Resource Manager sjablonen](../azure-resource-manager/resource-group-overview.md#template-deployment) zijn een uitstekende manier om te bereiken templating in Azure Data Factory. Resource Manager sjablonen definiëren de infrastructuur en de configuratie van uw Azure oplossing via een JSON-bestand. Omdat Azure Resource Manager sjablonen met alle/meeste Azure services werken, kan het veel gemakkelijk beheren van alle bronnen van uw activa Azure gebruikt. Zie [sjablonen ontwerpen Azure Resource Manager](../resource-group-authoring-templates.md) voor meer informatie over de Resource Manager-sjablonen in het algemeen. 

## <a name="tutorials"></a>Zelfstudies
Zie de volgende zelfstudies voor stapsgewijze instructies voor het Data Factory-entiteiten maken met behulp van bronbeheer sjablonen:

- [Zelfstudie: Een pijpleiding om gegevens te kopiëren met behulp van bronbeheer Azure-sjabloon maken](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [Zelfstudie: Een pijpleiding om gegevens te verwerken met behulp van bronbeheer Azure-sjabloon maken](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Data Factory-sjablonen op Github
Bekijk de volgende sjablonen Azure snel starten op Github: 

- [Een Data factory om gegevens te kopiëren van Azure Blob-opslag met Azure SQL-Database maken](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
- [Maak een Data factory met component activiteit op Azure HDInsight cluster](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
- [Maak een Data factory om gegevens te kopiëren van televergaderingen naar Azure BLOB 's](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)

Gerust uw sjablonen Azure Data Factory op [Azure Quick start](https://azure.microsoft.com/documentation/templates/). Raadpleeg de [handleiding van de bijdrage](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) tijdens het ontwikkelen van sjablonen die kunnen worden gedeeld via deze opslagplaats. 

De volgende secties geven informatie over het definiëren van Data Factory-bronnen in een sjabloon Resource Manager. 

## <a name="defining-data-factory-resources-in-templates"></a>Data Factory resources definiëren in sjablonen
De sjabloon op het hoogste niveau voor het definiëren van een fabriek van gegevens is:

    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
    {
        "name": "[parameters('dataFactoryName')]",
        "apiVersion": "[variables('apiVersion')]",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "westus",
        "resources": [
        { "type": "linkedservices",
            ...
        },
        {"type": "datasets",
            ...
        },
        {"type": "dataPipelines",
            ...
        }
    }

### <a name="define-data-factory"></a>Definieer data factory

U definieert een fabriek van gegevens in de sjabloon Resource Manager zoals in het volgende voorbeeld:

    "resources": [
    {
        "name": "[variables('<mydataFactoryName>')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "East US"
    }

De dataFactoryName wordt in de "variabelen" gedefinieerd als:

    "dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",

### <a name="define-linked-services"></a>Gekoppelde services definiëren 
    
    "type": "linkedservices",
    "name": "[variables('<LinkedServiceName>')]",
    "apiVersion": "2015-10-01",
    "dependsOn": [ "[variables('<dataFactoryName>')]" ],
    "properties": {
        ...
    }


Zie [Gekoppelde Storage-Service](data-factory-azure-blob-connector.md#azure-storage-linked-service) of de [Gekoppelde Services berekenen](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie over de JSON-eigenschappen voor de specifieke gekoppelde service die u wilt implementeren. De parameter 'dependsOn' geeft de naam van de bijbehorende data factory genoemd. In de volgende definitie van JSON ziet u een voorbeeld van een gekoppelde service voor de opslag van Azure definiëren:

### <a name="define-datasets"></a>Datasets bepalen

    "type": "datasets",
    "name": "[variables('<myDatasetName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<myDatasetLinkedServiceName>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        ...
    }

Raadpleeg de [opgeslagen gegevens wordt ondersteund](data-factory-data-movement-activities.md#supported-data-stores-and-formats) voor meer informatie over de JSON-eigenschappen voor het specifiek dataset-type dat u wilt implementeren. Opmerking dat de parameter 'dependsOn' bevat de naam van de bijbehorende data factory en opslag service gekoppeld. In de volgende definitie van JSON ziet u een voorbeeld van het type dataset Azure blob-opslag definiëren:

    "type": "datasets",
    "name": "[variables('storageDataset')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('storageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "[variables('storageLinkedServiceName')]",
    "typeProperties": {
        "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
        "fileName": "[parameters('sourceBlobName')]",
        "format": {
            "type": "TextFormat"
        }
    },
    "availability": {
        "frequency": "Hour",
        "interval": 1
    }

### <a name="define-pipelines"></a>Definiëren van pijpleidingen

    "type": "dataPipelines",
    "name": "[variables('<mypipelineName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<inputDatasetLinkedServiceName>')]",
        "[variables('<outputDatasetLinkedServiceName>')]",
        "[variables('<inputDataset>')]",
        "[variables('<outputDataset>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        activities: {
            ...
        }
    }

Verwijzen naar de [pijpleidingen te definiëren](data-factory-create-pipelines.md#pipeline-json) voor meer informatie over de JSON-eigenschappen voor het definiëren van de specifieke pipeline en de activiteiten die u wilt implementeren. Opmerking de parameter 'dependsOn' geeft de naam aan van de fabriek van de gegevens en de bijbehorende gekoppelde services of datasets. Een voorbeeld van een pijpleiding die gegevens van Azure Blob-opslag met Azure SQL-Database kopieert wordt weergegeven in het volgende fragment van JSON:

    "type": "datapipelines",
    "name": "[variables('pipelineName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]",
        "[variables('azureSqlLinkedServiceName')]",
        "[variables('blobInputDatasetName')]",
        "[variables('sqlOutputDatasetName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "activities": [
        {
            "name": "CopyFromAzureBlobToAzureSQL",
            "description": "Copy data frm Azure blob to Azure SQL",
            "type": "Copy",
            "inputs": [
                {
                    "name": "[variables('blobInputDatasetName')]"
                }
            ],
            "outputs": [
                {
                    "name": "[variables('sqlOutputDatasetName')]"
                }
            ],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "SqlSink",
                    "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                },
                "translator": {
                    "type": "TabularTranslator",
                    "columnMappings": "Column0:FirstName,Column1:LastName"
                }
            },
            "Policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 3,
                "timeout": "01:00:00"
            }
        }
        ],
        "start": "2016-10-03T00:00:00Z",
        "end": "2016-10-04T00:00:00Z"

## <a name="parameterizing-data-factory-template"></a>Data Factory sjabloon parameters voorzien
Voor procedures van parameters voorzien, Zie [Aanbevolen procedures voor het maken van sjablonen voor Azure Resource Manager](../resource-manager-template-best-practices.md#parameters) artikel. In het algemeen moet parametergebruik worden geminimaliseerd, vooral als variabelen in plaats daarvan kunnen worden gebruikt. Alleen de parameters in de volgende scenario's bieden:

- Instellingen is afhankelijk van de omgeving (voorbeeld: ontwikkeling, test en productie)
- Geheimen (zoals wachtwoorden)

Als u halen de geheimen uit [Azure sleutel kluis wilt](../key-vault/key-vault-get-started.md) bij het implementeren van Azure Data Factory entiteiten met behulp van sjablonen, geeft u de **sleutel kluis** en **geheime naam** zoals in het volgende voorbeeld:

    "parameters": {
        "storageAccountKey": { 
            "reference": {
                "keyVault": {
                    "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
                },
                "secretName": "<secretName>"
            }, 
        },
        ...
    }

> [AZURE.NOTE] Sjablonen voor fabrieken van bestaande gegevens exporteren is momenteel nog niet ondersteund, is maar in de werken. 



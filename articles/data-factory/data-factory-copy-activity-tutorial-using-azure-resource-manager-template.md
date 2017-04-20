<properties
    pageTitle="Zelfstudie: Een pijpleiding met behulp van bronbeheer sjabloon maken | Microsoft Azure"
    description="In deze zelfstudie maakt u een pijpleiding Azure Data Factory met een activiteit kopiëren met behulp van bronbeheer Azure sjabloon."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-resource-manager-template"></a>Zelfstudie: Een pijpleiding met kopie activiteit met behulp van bronbeheer Azure-sjabloon maken
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md)
- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Sjabloon voor Azure Resource Manager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


In deze zelfstudie wordt beschreven hoe u maken en een Azure data factory met behulp van een sjabloon Azure Resource Manager controleren. De pijpleiding die in de fabriek gegevens kopieert gegevens van Azure Blob-opslag met Azure SQL-Database.

## <a name="prerequisites"></a>Vereisten
- Ga via [zelfstudie overzicht en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) en de **vereiste** stappen.
- Volg de instructies in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) artikel naar de meest recente versie van Azure PowerShell op uw computer installeren. In deze zelfstudie gebruikt u PowerShell implementatie van Data Factory entiteiten. 
- (optioneel) Zie [Authoring Azure Resource Manager Templates](../resource-group-authoring-templates.md) voor meer informatie over sjablonen voor Azure Resource Manager.


## <a name="in-this-tutorial"></a>In deze zelfstudie

In deze zelfstudie kunt u een data factory maken met de volgende Data Factory-entiteiten:

Entiteit | Beschrijving  
------ | ----------- 
Azure gekoppeld opslagservice | Uw account Azure opslag koppelt aan de fabriek van gegevens. Azure opslag is de bron data store en Azure SQL-database is de sink gegevensopslag voor de activiteit van de kopie in de zelfstudie. Deze geeft de opslag rekening met de gegevens die zijn ingevoerd voor de activiteit kopiëren. 
Azure SQL-Database gekoppeld service| De Azure SQL-database wordt gekoppeld aan de data factory genoemd. Deze geeft de Azure SQL-database waarin de uitvoergegevens voor de activiteit kopiëren. 
Azure Blob invoer dataset | Verwijst naar de gekoppelde Azure-Storage-service. De gekoppelde service verwijst naar een account Azure opslag en de dataset Azure Blob Hiermee geeft u de container, map en bestandsnaam in de opslag die in het bezit van de ingevoerde gegevens. 
Azure SQL output dataset | Verwijst naar de gekoppelde Azure SQL service. De gekoppelde Azure SQL service verwijst naar een Azure SQL server en SQL Azure dataset bevat de naam van de tabel waarin de gegevens over de output. 
Data pijpleiding | De pijplijn is een activiteit van het type kopiëren wordt de dataset Azure blob als input als output van een dataset Azure SQL. De activiteit kopiëren kopieert u gegevens uit een Azure blob aan een tabel in de Azure SQL-database.  

Een data factory genoemd kan een of meer pijpleidingen hebben. Een pijpleiding kan één of meer activiteiten erin hebben. Er zijn twee soorten activiteiten: [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) en [data transformation activiteiten](data-factory-data-transformation-activities.md). In deze zelfstudie maakt u een pijpleiding met één activiteit (kopie).

![Azure Blob met Azure SQL-Database kopiëren](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

De volgende sectie bevat de volledige bronnenbeheerder sjabloon voor het definiëren van Data Factory entiteiten die snel uitgevoerd door de zelfstudie en de sjabloon testen. Als u wilt begrijpen hoe elke entiteit Data Factory is gedefinieerd, Zie de sectie [Data Factory-entiteiten in de sjabloon](#data-factory-entities-in-the-template) .

## <a name="data-factory-json-template"></a>Data Factory JSON-sjabloon
De sjabloon die op het hoogste niveau Resource Manager voor het definiëren van een fabriek van gegevens is: 

    {
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
                    { ... },
                    { ... },
                    { ... },
                    { ... }
                ]
            }
        ]
    }

Maak een JSON-bestand met de naam **ADFCopyTutorialARM.json** in de map **C:\ADFGetStarted** met de volgende inhoud:


    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
          "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the data to be copied." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
          "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
          "sourceBlobName": { "type": "string", "metadata": { "description": "Name of the blob in the container that has the data to be copied to Azure SQL Database table" } },
          "sqlServerName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Server that will hold the output/copied data." } },
          "databaseName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Database in the Azure SQL server." } },
          "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of the user that has access to the Azure SQL server." } },
          "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for the user." } },
          "targetSQLTable": { "type": "string", "metadata": { "description": "Table in the Azure SQL Database that will hold the copied data." } 
          } 
        },
        "variables": {
          "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
          "azureSqlLinkedServiceName": "AzureSqlLinkedService",
          "azureStorageLinkedServiceName": "AzureStorageLinkedService",
          "blobInputDatasetName": "BlobInputDataset",
          "sqlOutputDatasetName": "SQLOutputDataset",
          "pipelineName": "Blob2SQLPipeline"
        },
        "resources": [
          {
            "name": "[variables('dataFactoryName')]",
            "apiVersion": "2015-10-01",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "West US",
            "resources": [
              {
                "type": "linkedservices",
                "name": "[variables('azureStorageLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureStorage",
                  "description": "Azure Storage linked service",
                  "typeProperties": {
                    "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
                  }
                }
              },
              {
                "type": "linkedservices",
                "name": "[variables('azureSqlLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlDatabase",
                  "description": "Azure SQL linked service",
                  "typeProperties": {
                    "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
                  }
                }
              },
              {
                "type": "datasets",
                "name": "[variables('blobInputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureBlob",
                  "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "Column0",
                      "type": "String"
                    },
                    {
                      "name": "Column1",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                    "fileName": "[parameters('sourceBlobName')]",
                    "format": {
                      "type": "TextFormat",
                      "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  },
                  "external": true
                }
              },
              {
                "type": "datasets",
                "name": "[variables('sqlOutputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureSqlLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlTable",
                  "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "FirstName",
                      "type": "String"
                    },
                    {
                      "name": "LastName",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "tableName": "[parameters('targetSQLTable')]"
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  }
                }
              },
              {
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
                  "start": "2016-10-02T00:00:00Z",
                  "end": "2016-10-03T00:00:00Z"
                }
              }
            ]
          }
        ]
      }

## <a name="parameters-json"></a>JSON-parameters 
Maak een JSON-bestand met de naam **ADFCopyTutorialARM Parameters.json** , waarin de parameters voor de sjabloon Azure Resource Manager. 

> [AZURE.IMPORTANT] Geef de naam en de sleutel van uw account Azure opslag voor de parameters **storageAccountName** en **storageAccountKey** .  

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { 
            "storageAccountName": { "value": "<Name of the Azure storage account>"  },
            "storageAccountKey": {
                "value": "<Key for the Azure storage account>"
            },
            "sourceBlobContainer": { "value": "adftutorial" },
            "sourceBlobName": { "value": "emp.txt" },
            "sqlServerName": { "value": "<Name of the Azure SQL server>" },
            "databaseName": { "value": "<Name of the Azure SQL database>" },
            "sqlServerUserName": { "value": "<Name of the user who has access to the Azure SQL database>" },
            "sqlServerPassword": { "value": "<password for the user>" },
            "targetSQLTable": { "value": "emp" }
        }
    }

> [AZURE.IMPORTANT] Mogelijk hebt u afzonderlijke parameter JSON bestanden voor het ontwikkelen, testen en productieomgevingen kunt u met dezelfde gegevens fabriek JSON-sjabloon. Met behulp van een Power Shell-script, kunt u automatiseren implementeren Data Factory entiteiten in deze omgevingen.  

## <a name="create-data-factory"></a>Data factory maken
1. **Azure PowerShell** Start en voer de volgende opdracht uit:
    - Uitvoeren van `Login-AzureRmAccount` en voer de gebruikersnaam en het wachtwoord die u gebruikt voor aanmelding bij de Azure portal.  
    - Uitvoeren van `Get-AzureRmSubscription` de abonnementen voor deze account bekijken.
    - Uitvoeren van `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` te selecteren van het abonnement dat u wilt werken. 
2. Voer de volgende opdracht te implementeren met behulp van de sjabloon Resource Manager Data Factory-entiteiten die u in stap 1 hebt gemaakt.

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Monitor-pipeline
1. Log in op de [Azure portal](https://portal.azure.com) met uw account Azure.
2. **Fabrieken van gegevens** in het menu links (of) klikt u op **meer services** en klik op **gegevens fabrieken** onder categorie **INTELLIGENCE + ANALYTICS** .

    ![Menu Data-fabrieken](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. Op de pagina **gegevens fabrieken** zoeken en weergeven van uw data factory genoemd. 

    ![Zoeken naar data factory](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Klik op de Azure data factory genoemd. U ziet de introductiepagina voor de fabriek van gegevens.

    ![Introductiepagina voor data factory](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
5. Klik op de tegel **Diagram** om het diagram van de fabriek gegevens bekijken.

    ![De weergave Netwerkdiagram van data factory](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-diagram-view.png)
6. Dubbelklik in de weergave Netwerkdiagram op de dataset **SQLOutputDataset**. U ziet dat de status van het segment. Wanneer de bewerking voor het kopiëren is voltooid, stelt u de status op **Gereed**.

    ![Output segment in de status gereed](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/output-slice-ready.png)
7. Wanneer het segment in de status **Gereed** is, zorg ervoor dat de gegevens worden gekopieerd naar de tabel **emp** in de Azure SQL-database.

Zie [Monitor datasets en pipeline](data-factory-monitor-manage-pipelines.md) voor instructies over het gebruik van de portal Azure blades de pijpleiding en datasets dat u in deze zelfstudie hebt gemaakt.

U kunt ook controleren en beheren van App voor het controleren van uw gegevens pijpleidingen. Zie [controleren en beheren van Azure Data Factory pijpleidingen Monitoring App met](data-factory-monitor-manage-app.md) voor meer informatie over het gebruik van de toepassing.


## <a name="data-factory-entities-in-the-template"></a>Data Factory-entiteiten in de sjabloon

### <a name="define-data-factory"></a>Definieer data factory
U definieert een fabriek van de gegevens in de resource manager sjabloon zoals in het volgende voorbeeld:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

De dataFactoryName wordt gedefinieerd als: 
      
    "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"

Dit is een unieke tekenreeks, op basis van de resource-ID.  

### <a name="defining-data-factory-entities"></a>Data Factory-entiteiten definiëren
De volgende Data Factory entiteiten worden gedefinieerd in de JSON-sjabloon: 

1. [Azure gekoppeld opslagservice](#azure-storage-linked-service)
2. [Azure gekoppelde SQL-service](#azure-sql-database-linked-service)
3. [Azure blob dataset](#azure-blob-dataset)
4. [Azure SQL dataset](#azure-sql-dataset)
5. [De pijpleiding gegevens met een activiteit kopiëren](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Azure gekoppeld opslagservice
Geeft u de naam en sleutel van uw account Azure opslag in deze sectie. Zie [opslag Azure service gekoppeld](data-factory-azure-blob-connector.md#azure-storage-linked-service) voor meer informatie over JSON eigenschappen gebruikt voor het definiëren van een service gekoppeld Azure-opslag. 

    {
        "type": "linkedservices",
        "name": "[variables('azureStorageLinkedServiceName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureStorage",
            "description": "Azure Storage linked service",
            "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
            }
        }
    }

De connectionString wordt gebruikt voor de parameters storageAccountName en storageAccountKey. De waarden voor deze parameters die worden doorgegeven met behulp van een configuratiebestand. De definitie ook variabelen worden gebruikt: azureStroageLinkedService en dataFactoryName die zijn gedefinieerd in de sjabloon. 
    
#### <a name="azure-sql-database-linked-service"></a>Azure SQL-Database gekoppeld service
U opgeven de Azure SQL servernaam, databasenaam, gebruikersnaam en wachtwoord in deze sectie. Zie [SQL Azure gekoppeld service](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) voor meer informatie over JSON eigenschappen gebruikt voor het definiëren van een gekoppelde Azure SQL service.  

    {
        "type": "linkedservices",
        "name": "[variables('azureSqlLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "Azure SQL linked service",
            "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
            }
        }
    }

De connectionString gebruikt naamsqlserver, databasenaam, sqlServerUserName en sqlServerPassword parameters waarvan de waarden worden doorgegeven met behulp van een configuratiebestand. De volgende variabelen in de sjabloon wordt ook gebruikgemaakt van de definitie: azureSqlLinkedServiceName, dataFactoryName.

#### <a name="azure-blob-dataset"></a>Azure blob dataset
U geeft de namen van de blob-container, map en bestand met de ingevoerde gegevens. Zie [Eigenschappen van Azure Blob dataset](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) voor meer informatie over JSON eigenschappen gebruikt voor het definiëren van een dataset Azure Blob. 


    {
        "type": "datasets",
        "name": "[variables('blobInputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
            "structure": [
            {
                "name": "Column0",
                "type": "String"
            },
            {
                "name": "Column1",
                "type": "String"
            }
            ],
            "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            },
            "external": true
        }
    }

#### <a name="azure-sql-dataset"></a>Azure SQL dataset
U kunt de naam van de tabel opgeven in de Azure SQL-database waarin u de gekopieerde gegevens uit de Azure Blob-opslag. Zie [Eigenschappen van SQL Azure dataset](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties) voor meer informatie over JSON eigenschappen gebruikt voor het definiëren van een dataset Azure SQL. 

    {
        "type": "datasets",
        "name": "[variables('sqlOutputDatasetName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureSqlLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
            "structure": [
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
            ],
            "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

#### <a name="data-pipeline"></a>Data pijpleiding
U definieert een pijpleiding die gegevens opgehaald uit de dataset Azure blob in de dataset Azure SQL. [Pijpleiding JSON](data-factory-create-pipelines.md#pipeline-json) Zie voor beschrijvingen van de JSON-elementen gebruikt voor het definiëren van een pijpleiding in dit voorbeeld. 

    {
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
            "start": "2016-10-02T00:00:00Z",
            "end": "2016-10-03T00:00:00Z"
        }
    }

## <a name="reuse-the-template"></a>De sjabloon opnieuw gebruiken 
In de zelfstudie, kunt u een sjabloon voor het definiëren van Data Factory entiteiten en een sjabloon voor het doorgeven van waarden voor de parameters gemaakt. De pijpleiding kopieert gegevens uit een account Azure opslag naar een Azure SQL-database die is opgegeven via de parameters. Als u dezelfde sjabloon wilt implementeren van Data Factory entiteiten in verschillende omgevingen, een parameterbestand maken voor elke omgeving en deze gebruiken bij het implementeren van deze omgeving.     

Voorbeeld:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json

U ziet dat de eerste opdracht parameterbestand voor de ontwikkelomgeving, tweede één voor de testomgeving en de derde is voor de productie-omgeving gebruikt.  

U kunt de sjabloon voor herhaalde taken ook hergebruiken. Bijvoorbeeld, moet u veel gegevens fabrieken maken met een of meer pijpleidingen die dezelfde logica te implementeren maar elke fabriek gegevens gebruikt verschillende Azure opslag en rekeningen Azure SQL-Database. In dit scenario kunt u dezelfde sjabloon in dezelfde omgeving (dev-, test- of productieorder) met verschillende bestanden gegevens fabrieken maken.   


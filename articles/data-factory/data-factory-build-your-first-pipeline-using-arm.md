<properties
    pageTitle="De eerste data factory (Resource Manager sjabloon) maken | Microsoft Azure"
    description="In deze zelfstudie maakt u een monster Azure Data Factory pijpleiding met behulp van een sjabloon Azure Resource Manager."
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
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a>Zelfstudie: De eerste gegevens Azure fabriek met behulp van bronbeheer Azure-sjabloon maken
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-build-your-first-pipeline.md)
- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Sjabloon voor bronbeheer](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)

In dit artikel wordt u een sjabloon Azure Resource Manager maakt uw eerste Azure data factory genoemd.

## <a name="prerequisites"></a>Vereisten
- Lees artikel [Overzicht zelfstudie](data-factory-build-your-first-pipeline.md) en de **vereiste** stappen.
- Volg de instructies in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) artikel naar de meest recente versie van Azure PowerShell op uw computer installeren.
- Zie [Authoring Azure Resource Manager Templates](../resource-group-authoring-templates.md) voor meer informatie over sjablonen voor Azure Resource Manager. 

## <a name="in-this-tutorial"></a>In deze zelfstudie
Entiteit | Beschrijving  
------ | ----------- 
Azure gekoppeld opslagservice | Uw account Azure opslag koppelt aan de fabriek van gegevens. De opslag van Azure-account bevat de gegevens invoer en uitvoer voor de pijplijn in dit voorbeeld. 
HDInsight op verzoek gekoppelde service| De data factory cluster koppelingen een HDInsight op aanvraag. Het cluster automatisch voor u gemaakt om gegevens te verwerken en wordt verwijderd nadat de verwerking is voltooid.
Azure Blob invoer dataset | Verwijst naar de gekoppelde Azure-Storage-service. De gekoppelde service verwijst naar een account Azure opslag en de dataset Azure Blob Hiermee geeft u de container, map en bestandsnaam in de opslag die in het bezit van de ingevoerde gegevens. 
Azure Blob uitvoer dataset | Verwijst naar de gekoppelde Azure-Storage-service. De gekoppelde service verwijst naar een account Azure opslag en de dataset Azure Blob Hiermee geeft u de container, map en bestandsnaam in de opslag die in het bezit van de uitvoergegevens. 
Data pijpleiding | De pijplijn is een activiteit van het type HDInsightHive verbruikt de dataset invoer en uitvoer dataset produceert.   

Een data factory genoemd kan een of meer pijpleidingen hebben. Een pijpleiding kan één of meer activiteiten erin hebben. Er zijn twee soorten activiteiten: [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) en [data transformation activiteiten](data-factory-data-transformation-activities.md). In deze zelfstudie maakt u een pijpleiding met één activiteit (kopie).

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

Maak een JSON-bestand met de naam **ADFTutorialARM.json** in de map **C:\ADFGetStarted** met de volgende inhoud:

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
            "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the input/output data." } },
            "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
            "blobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
            "inputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that has the input file." } },
            "inputBlobName": { "type": "string", "metadata": { "description": "Name of the input file/blob." } },
            "outputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that will hold the transformed data." } },
            "hiveScriptFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that contains the Hive query file." } },
            "hiveScriptFile": { "type": "string", "metadata": { "description": "Name of the hive query (HQL) file." } }
        },
        "variables": {
            "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
            "azureStorageLinkedServiceName": "AzureStorageLinkedService",
            "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
            "blobInputDatasetName": "AzureBlobInput",
            "blobOutputDatasetName": "AzureBlobOutput",
            "pipelineName": "HiveTransformPipeline"
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
                "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "HDInsightOnDemand",
                    "typeProperties": {
                        "clusterSize": 1,
                        "version": "3.2",
                        "timeToLive": "00:05:00",
                        "osType": "windows",
                        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
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
                    "typeProperties": {
                        "fileName": "[parameters('inputBlobName')]",
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "external": true
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobOutputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
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
                    "[variables('hdInsightOnDemandLinkedServiceName')]",
                    "[variables('blobInputDatasetName')]",
                    "[variables('blobOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "description": "Pipeline that transforms data using Hive script.",
                    "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                            "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                            "defines": {
                                "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                                "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                            }
                        },
                        "inputs": [
                            {
                                "name": "[variables('blobInputDatasetName')]"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "[variables('blobOutputDatasetName')]"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
                    }
                    ],
                    "start": "2016-10-01T00:00:00Z",
                    "end": "2016-10-02T00:00:00Z",
                    "isPaused": false
                }
            }
            ]
        }
        ]
    }

> [AZURE.NOTE] Vindt u een voorbeeld van bronbeheer sjabloon voor het maken van een fabriek Azure gegevens op [Zelfstudie: maken van een pijplijn met kopie activiteit met behulp van een sjabloon Azure Resource Manager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).  

## <a name="parameters-json"></a>JSON-parameters 
Maak een JSON-bestand met de naam **ADFTutorialARM Parameters.json** , waarin de parameters voor de sjabloon Azure Resource Manager.  

> [AZURE.IMPORTANT] Geef de naam en de sleutel van uw account Azure opslag voor de parameters **storageAccountName** en **storageAccountKey** in dit parameterbestand. 

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "storageAccountName": {
                "value": "<Name of your Azure Storage account>"
            },
            "storageAccountKey": {
                "value": "<Key of your Azure Storage account>"
            },
            "blobContainer": {
                "value": "adfgetstarted"
            },
            "inputBlobFolder": {
                "value": "inputdata"
            },
            "inputBlobName": {
                "value": "input.log"
            },
            "outputBlobFolder": {
                "value": "partitioneddata"
            },
            "hiveScriptFolder": {
                "value": "script"
            },
            "hiveScriptFile": {
                "value": "partitionweblogs.hql"
            }
        }
    }

> [AZURE.IMPORTANT] Mogelijk hebt u afzonderlijke parameter JSON bestanden voor het ontwikkelen, testen en productieomgevingen kunt u met dezelfde gegevens fabriek JSON-sjabloon. Met behulp van een Power Shell-script, kunt u automatiseren implementeren Data Factory entiteiten in deze omgevingen. 

## <a name="create-data-factory"></a>Data factory maken

1. **Azure PowerShell** Start en voer de volgende opdracht uit: 
    - Uitvoeren van `Login-AzureRmAccount` en voer de gebruikersnaam en het wachtwoord die u gebruikt voor aanmelding bij de Azure portal.  
    - Uitvoeren van `Get-AzureRmSubscription` de abonnementen voor deze account bekijken.
    - Uitvoeren van `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` te selecteren van het abonnement dat u wilt werken. Dit abonnement moet hetzelfde zijn als die u in de portal Azure.
1. Voer de volgende opdracht te implementeren met behulp van de sjabloon Resource Manager Data Factory-entiteiten die u in stap 1 hebt gemaakt. 

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Monitor-pipeline
 
1.  Na het aanmelden op de [portal Azure](https://portal.azure.com/), klikt u op **Bladeren** en selecteer **gegevens fabrieken**.
        ![Bladeren -> gegevens fabrieken](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2.  Klik op de data factory (**TutorialFactoryARM**) die u hebt gemaakt in het blad **Gegevens fabrieken** .   
2.  In het blad **Data Factory** voor de fabriek van gegevens, klikt u op het **Diagram**.
        ![Diagram naast elkaar](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4.  In de **Weergave Netwerkdiagram**ziet u een overzicht van de pijpleidingen en datasets die in deze zelfstudie wordt gebruikt.
    
    ![Diagram weergeven](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
8. Dubbelklik in de weergave Netwerkdiagram op de dataset **AzureBlobOutput**. U ziet dat het segment dat momenteel wordt verwerkt.

    ![DataSet](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
9. Wanneer de verwerking is voltooid, ziet u het segment in de status **Gereed** . Maken van een cluster op verzoek HDInsight duurt meestal ergens (ongeveer 20 minuten). Daarom verwachten de pijpleiding om **ongeveer 30 minuten** voor het verwerken van het segment.

    ![DataSet](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png) 
10. Als het segment **gereed is** , controleert u de map **partitioneddata** in de container **adfgetstarted** in de blob-opslag voor de uitvoergegevens.  

Zie [Monitor datasets en pipeline](data-factory-monitor-manage-pipelines.md) voor instructies over het gebruik van de portal Azure blades de pijpleiding en datasets dat u in deze zelfstudie hebt gemaakt.

U kunt ook controleren en beheren van App voor het controleren van uw gegevens pijpleidingen. Zie [controleren en beheren van Azure Data Factory pijpleidingen Monitoring App met](data-factory-monitor-manage-app.md) voor meer informatie over het gebruik van de toepassing. 

> [AZURE.IMPORTANT] Het invoerbestand wordt verwijderd wanneer het segment wordt verwerkt. Dus als u wilt dat het segment opnieuw uitvoeren of de zelfstudie opnieuw doen, upload het invoerbestand (input.log) naar de map inputdata van de container adfgetstarted.

## <a name="data-factory-entities-in-the-template"></a>Data Factory-entiteiten in de sjabloon
### <a name="define-data-factory"></a>Definieer data factory
U definieert een fabriek van gegevens in de sjabloon Resource Manager zoals in het volgende voorbeeld:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

De dataFactoryName wordt gedefinieerd als: 
      
      "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",

Het is een unieke tekenreeks, op basis van de resource-ID.  

### <a name="defining-data-factory-entities"></a>Data Factory-entiteiten definiëren
De volgende Data Factory entiteiten worden gedefinieerd in de JSON-sjabloon: 

- [Azure gekoppeld opslagservice](#azure-storage-linked-service)
- [HDInsight op verzoek gekoppelde service](#hdinsight-on-demand-linked-service)
- [Invoer dataset Azure blob](#azure-blob-input-dataset)
- [Azure blob uitvoer dataset](#azure-blob-output-dataset)
- [De pijpleiding gegevens met een activiteit kopiëren](#data-pipeline)

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

De **connectionString** wordt gebruikt voor de parameters storageAccountName en storageAccountKey. De waarden voor deze parameters die worden doorgegeven met behulp van een configuratiebestand. De definitie ook variabelen worden gebruikt: azureStroageLinkedService en dataFactoryName die zijn gedefinieerd in de sjabloon. 
    
#### <a name="hdinsight-on-demand-linked-service"></a>HDInsight op verzoek gekoppelde service
Zie artikel [berekenen gekoppelde services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie over JSON eigenschappen gebruikt voor het definiëren van een HDInsight op verzoek gekoppelde service.  

      {
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "HDInsightOnDemand",
          "typeProperties": {
            "clusterSize": 1,
            "version": "3.2",
            "timeToLive": "00:05:00",
            "osType": "windows",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
          }
        }
      }

Houd rekening met de volgende punten: 

- De Data Factory gemaakt een HDInsight **Windows-gebaseerde** clusters met de bovenstaande JSON. U kunt ook het maken van een HDInsight **op basis van Linux** cluster hebben. Zie [Gekoppelde HDInsight-Service op verzoek](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie. 
- In plaats van een cluster op verzoek HDInsight kunt u **uw eigen cluster HDInsight** . Zie [Gekoppelde HDInsight-Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) voor meer informatie.
- Het cluster HDInsight maakt een **standaardcontainer** in de blobopslag die u hebt opgegeven in de JSON (**linkedServiceName**). HDInsight, verwijdert deze container wanneer het cluster wordt verwijderd. Dit gedrag is inherent aan het ontwerp. Op verzoek HDInsight gekoppeld service, een cluster wordt gemaakt telkens wanneer een segment worden verwerkt moet, tenzij er een bestaande HDInsight live cluster (**timeToLive**) en wordt verwijderd wanneer de verwerking is voltooid.

    Als u meer segmenten worden verwerkt, ziet u veel containers in Azure blobopslag. Als u niet hen hoeft voor het oplossen van de taken, kunt u deze om de kosten voor opslag te verwijderen. De namen van deze containers een patroon volgen: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Met hulpmiddelen zoals [Microsoft Opslagverkenner](http://storageexplorer.com/) containers in Azure blobopslag verwijderen.

Zie [Gekoppelde HDInsight-Service op verzoek](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie.



#### <a name="azure-blob-input-dataset"></a>Invoer dataset Azure blob
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
          "typeProperties": {
            "fileName": "[parameters('inputBlobName')]",
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          },
          "external": true
        }
      }

Deze definitie worden de volgende parameters die zijn gedefinieerd in de sjabloon parameter gebruikt: blobContainer, inputBlobFolder, en inputBlobName. 

#### <a name="azure-blob-output-dataset"></a>Azure Blob uitvoer dataset
U geeft de namen van de blob-container en de map waarin de gegevens over de output. Zie [Eigenschappen van Azure Blob dataset](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) voor meer informatie over JSON eigenschappen gebruikt voor het definiëren van een dataset Azure Blob.  

      {
        "type": "datasets",
        "name": "[variables('blobOutputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          }
        }
      }

Deze definitie maakt gebruik van de volgende parameters die zijn gedefinieerd in de sjabloon parameter: blobContainer en outputBlobFolder. 

#### <a name="data-pipeline"></a>Data pijpleiding
U definieert een pijpleiding omzetten van gegevens met een component-script op een cluster op verzoek Azure HDInsight. [Pijpleiding JSON](data-factory-create-pipelines.md#pipeline-json) Zie voor beschrijvingen van de JSON-elementen gebruikt voor het definiëren van een pijpleiding in dit voorbeeld. 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]",
          "[variables('hdInsightOnDemandLinkedServiceName')]",
          "[variables('blobInputDatasetName')]",
          "[variables('blobOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "description": "Pipeline that transforms data using Hive script.",
          "activities": [
            {
              "type": "HDInsightHive",
              "typeProperties": {
                "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                "defines": {
                  "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                  "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                }
              },
              "inputs": [
                {
                  "name": "[variables('blobInputDatasetName')]"
                }
              ],
              "outputs": [
                {
                  "name": "[variables('blobOutputDatasetName')]"
                }
              ],
              "policy": {
                "concurrency": 1,
                "retry": 3
              },
              "scheduler": {
                "frequency": "Month",
                "interval": 1
              },
              "name": "RunSampleHiveActivity",
              "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
            }
          ],
          "start": "2016-10-01T00:00:00Z",
          "end": "2016-10-02T00:00:00Z",
          "isPaused": false
        }
      }

## <a name="reuse-the-template"></a>De sjabloon opnieuw gebruiken 
In de zelfstudie, kunt u een sjabloon voor het definiëren van Data Factory entiteiten en een sjabloon voor het doorgeven van waarden voor de parameters gemaakt. Als u dezelfde sjabloon wilt implementeren van Data Factory entiteiten in verschillende omgevingen, een parameterbestand maken voor elke omgeving en deze gebruiken bij het implementeren van deze omgeving.     

Voorbeeld:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json

U ziet dat de eerste opdracht parameterbestand voor de ontwikkelomgeving, tweede één voor de testomgeving en de derde is voor de productie-omgeving gebruikt.  

U kunt de sjabloon voor herhaalde taken ook hergebruiken. Bijvoorbeeld, moet u veel gegevens fabrieken maken met een of meer pijpleidingen die dezelfde logica te implementeren maar elke fabriek gegevens gebruikt verschillende Azure opslag en rekeningen Azure SQL-Database. In dit scenario kunt u dezelfde sjabloon in dezelfde omgeving (dev-, test- of productieorder) met verschillende bestanden gegevens fabrieken maken. 

## <a name="resource-manager-template-for-creating-a-gateway"></a>Sjabloon voor het maken van een gateway Resource Manager
Hier volgt een voorbeeldsjabloon Resource Manager voor het maken van een logische gateway aan de achterkant. Een gateway installeren op uw computer op gebouwen of Azure IaaS VM en registreren van de gateway met Data Factory service met behulp van een sleutel. Zie het [verplaatsen van gegevens tussen op gebouwen en cloud](data-factory-move-data-between-onprem-and-cloud.md) voor meer informatie.

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "GatewayUsingArmDF",
            "apiVersion": "2015-10-01",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "eastus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "gateways",
                        "apiVersion": "[variables('apiVersion')]",
                        "name": "GatewayUsingARM",
                        "properties": {
                            "description": "my gateway"
                        }
                    }            
                ]
            }
        ]
    }

Met deze sjabloon maakt u een data factory met de naam GatewayUsingArmDF met een gateway met de naam: GatewayUsingARM. 

## <a name="see-also"></a>Zie ook
| Onderwerp | Beschrijving |
| :---- | :---- |
| [Data Transformation activiteiten](data-factory-data-transformation-activities.md) | Dit artikel bevat een lijst met data transformation activiteiten (zoals component HDInsight-transformatie die u in deze zelfstudie wordt gebruikt) door Azure Data Factory ondersteund. |
| [Planning en uitvoering](data-factory-scheduling-and-execution.md) | In dit artikel wordt uitgelegd dat de planning en uitvoering van aspecten van Azure Data Factory toepassingsmodel. |
| [Pijpleidingen](data-factory-create-pipelines.md) | Dit artikel helpt u bij het begrijpen van pijpleidingen en activiteiten in Azure Data Factory en het gebruik maken van end-to-end gegevensgestuurde werkstromen voor uw scenario of bedrijf. |
| [DataSets](data-factory-create-datasets.md) | Dit artikel helpt u bij het begrijpen van datasets in Azure Data Factory.
| [Bewaken en beheren van Monitoring App via pijpleidingen](data-factory-monitor-manage-app.md) | In dit artikel wordt beschreven hoe bewaken, beheren en foutopsporing met de Monitoring en beheer App pijpleidingen. 

  


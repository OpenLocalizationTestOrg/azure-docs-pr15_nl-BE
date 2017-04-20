<properties 
    pageTitle="Gebruik van Machine Learning activiteiten | Microsoft Azure" 
    description="Beschrijft hoe u voorspellende pijpleidingen met Azure Data Factory en Azure Machine Learning maken" 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="create-predictive-pipelines-using-azure-machine-learning-activities"></a>Voorspellende pijpleidingen met Azure Machine Learning-activiteiten maken   
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Streaming Hadoop](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics U SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Inleiding

[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) kunt u bouwen, testen en implementeren van anticiperende analytische oplossingen. Uit een oogpunt van de high-level dit gebeurt in drie stappen: 

1. **Een training experiment maken**. U doen deze stap met behulp van de Studio Azure ML. De ML studio is een ontwikkelomgeving voor het gezamenlijke visuele met trainen en testen van een anticiperende analytische model met trainingsgegevens.
2. **Converteren naar een voorspellende experiment**. Zodra uw model is getraind met bestaande gegevens en u klaar om te gebruiken bent voor het verkrijgen van nieuwe gegevens, kunt u voorbereiden en stroomlijnen van uw experiment voor het scoren.
3. **Als een webservice te implementeren**. U kunt uw score experiment publiceren als een webservice Azure. U kunt gegevens verzenden naar uw model via dit web-service-eindpunt en ontvangen resultaat voorspellingen van het model.  

Azure Data Factory kunt u gemakkelijk maken pijpleidingen die gebruikmaken van een gepubliceerde [Azure Machine Learning] [ azure-machine-learning] voor anticiperende analytics-webservice. Zie [Inleiding tot Azure Data Factory](data-factory-introduction.md) en [bouwen van uw eerste pijpleiding](data-factory-build-your-first-pipeline.md) artikelen om snel aan de slag met de Azure Data Factory-service. 

Met de **Batchverwerking uitvoeren van activiteiten** in een pijpleiding Azure Data Factory, kunt u een webservice Azure ML om voorspellingen van de gegevens in de batch aanroepen. Zie sectie [roepen een Azure ML webservice met behulp van de Batch-activiteit kan worden uitgevoerd](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) voor meer informatie.

Na verloop van tijd moeten de voorspellende modellen in de Azure ML scoren experimenten met nieuwe invoer datasets worden retrained. U kunt een model Azure ML van een pijpleiding Data Factory retrain volgt u de volgende stappen uit: 

1. Het experiment training (geen voorspellende experiment) publiceren als een webservice. U kunt deze stap in de Studio Azure ML doen als hiervoor toen u voorspellende experiment worden blootgesteld als een webservice in het vorige scenario.
2. Azure ML Batch uitvoering activiteit aan te roepen van de webservice voor het experiment training gebruiken. In principe kunt u de uitvoering van Azure ML Batch-activiteit aan te roepen van zowel opleiding webservice en webservice score. 
  
Nadat u klaar bent met de bijscholing, wilt u score webservice (weergegeven als een webservice voorspellende experiment) bijwerken met de nieuwe opgeleide model. Hier zijn de stappen: 

1. Het eindpunt van een niet-standaard aan de score webservice toevoegen. Het standaardeindpunt van de web-service kan niet worden bijgewerkt, dus moet u een niet-standaard-eindpunt met de Azure portal maken. Zie het artikel [Eindpunten maken](../machine-learning/machine-learning-create-endpoint.md) voor algemene informatie en procedurele stappen.
2. Bijwerken van bestaande Azure ML gekoppelde services voor het gebruik van het eindpunt van de niet-standaard scoren. Start met het gebruik van de webservice die is bijgewerkt met behulp van het nieuwe endpoint.
3. Gebruik de **Azure ML-Resource activiteit** naar de webservice bijwerken met het nieuwe opgeleide model.  

Zie de sectie [bijwerken Azure ML modellen met behulp van de activiteit van de bron bijwerken](#updating-azure-ml-models-using-the-update-resource-activity) voor meer informatie. 

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Aanroepen van een webservice met behulp van Batch tot het uitvoeren van activiteiten

U gebruik Azure Data Factory goedkeuringen door verplaatsing van gegevens en van verwerking en vervolgens uitvoeren batch worden uitgevoerd met behulp van Azure Machine Learning. Hier zijn de stappen op het hoogste niveau:

1. Maak een service Azure Machine Learning gekoppeld. U hebt het volgende nodig:
    1. De **URI van de aanvraag** voor het uitvoeren van Batch API. De aanvraag-URI kunt u vinden door te klikken op de koppeling **BATCH worden uitgevoerd** in de pagina web services.
    1. **API-sleutel** voor de gepubliceerde Azure Machine Learning webservice. De API-sleutel kunt u vinden door te klikken op de web-service die u hebt gepubliceerd. 
 2. Gebruik de **AzureMLBatchExecution** -activiteit.

    ![Machine Learning Dashboard](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

    ![Batch URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)


### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Scenario: Proeven met Web service-ingangen /-uitgangen die naar gegevens in Azure Blob-opslag verwijzen
In dit scenario wordt de webservice Azure Machine Learning maakt voorspellingen met gegevens uit een bestand in een Azure blobopslag en slaat de resultaten voorspelling in de blobopslag. De volgende JSON definieert een pijpleiding Data Factory met een AzureMLBatchExecution activiteit. De activiteit is de dataset **DecisionTreeInputBlob** als invoer en **DecisionTreeResultBlob** als de uitvoer. De **DecisionTreeInputBlob** wordt doorgegeven als invoer voor de webservice met behulp van de eigenschap **webServiceInput** JSON. De **DecisionTreeResultBlob** wordt doorgegeven als output van een webservice met behulp van de eigenschap **webServiceOutputs** JSON.  

> [AZURE.IMPORTANT] 
> Als de webservice meerdere ingangen heeft, gebruikt u de eigenschap **webServiceInputs** in plaats van **webServiceInput**. Zie de sectie [meerdere ingangen is vereist](#web-service-requires-multiple-inputs) voor een voorbeeld van de eigenschap webServiceInputs.
>  
> DataSets waarnaar wordt verwezen door de **webServiceInput**/**webServiceInputs** en **webServiceOutputs** eigenschappen (in **typeProperties**) moeten ook worden opgenomen in de activiteit, **invoer** en **uitvoer**.
> 
> In het experiment Azure ML hebben web service input en output poorten en globale parameters standaardnamen ("input1", "input2") die u kunt aanpassen. De namen die u voor de webServiceInputs, webServiceOutputs en globalParameters-instellingen moeten exact overeenkomen met de namen in de experimenten. U kunt de nettolading monster aanvraag bekijken op de pagina Help voor Batch worden uitgevoerd voor uw Azure ML eindpunt om te controleren of de verwachte toewijzing. 


    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "DecisionTreeInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "DecisionTreeResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties":
            {
                "webServiceInput": "DecisionTreeInputBlob",
                "webServiceOutputs": {
                    "output1": "DecisionTreeResultBlob"
                }                
            },
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

> [AZURE.NOTE] Enige ingangen en uitgangen van de activiteit van de AzureMLBatchExecution kunnen als parameters worden doorgegeven aan de webservice. In het bovenstaande fragment JSON is DecisionTreeInputBlob bijvoorbeeld een invoer voor de AzureMLBatchExecution-activiteit, die via de webServiceInput-parameter wordt doorgegeven als invoer voor de webservice.   

### <a name="example"></a>Voorbeeld

In dit voorbeeld wordt de Azure opslag voor de gegevens invoer en uitvoer. 

Het is raadzaam dat u [uw eerste pijpleiding met Data Factory bouwt] doorlopen[ adf-build-1st-pipeline] zelfstudie voordat u verdergaat met het volgende voorbeeld. De Data Factory-Editor gebruiken om Data Factory artefacten (gekoppelde services, datasets, pijpleiding) maken in het volgende voorbeeld.   
 

1. Maak een **gekoppelde service** voor uw **Opslag Azure**. Als de bestanden voor invoer en uitvoer in opslag voor verschillende accounts, moet u twee gekoppelde services. Hier volgt een voorbeeld van JSON:

        {
          "name": "StorageLinkedService",
          "properties": {
            "type": "AzureStorage",
            "typeProperties": {
              "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
            }
          }
        }

2. Maak de **invoer** Azure Data Factory **dataset**. In tegenstelling tot sommige andere Data Factory datasets moet deze datasets waarden **mappad** en de **bestandsnaam** bevatten. U kunt partitioneren van elke batch worden uitgevoerd (elk segment) te verwerken of produceren unieke invoer en uitvoer van bestanden. Wellicht moet u enkele upstream activiteiten om te zetten van de invoer in de CSV-indeling en plaats deze op de rekening van de opslag voor elk segment zijn. In dat geval neemt u niet de instellingen van de **externe** en **externalData** in het volgende voorbeeld wordt getoond, en uw DecisionTreeInputBlob de dataset uitvoer van een andere activiteit zou zijn.

        {
          "name": "DecisionTreeInputBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/input",
              "fileName": "in.csv",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Day",
              "interval": 1
            },
            "policy": {
              "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
              }
            }
          }
        }
    
    Uw input CSV-bestand, moet de rij voor kolomkoppen hebben. Als u de **Activiteit van de kopie** naar de csv in de blobopslag maken/verplaatsen, moet u de sink eigenschap **blobWriterAddHeader** ingesteld op **true**. Bijvoorbeeld:
    
         sink: 
         {
             "type": "BlobSink",     
             "blobWriterAddHeader": true 
         }
     
    Als het CSV-bestand geen rij met kolomkoppen bevat, ziet u mogelijk de volgende fout: fout in activiteit **: fout bij het lezen van de tekenreeks. Onverwachte token: StartObject. Pad '', regel 1, positie 1**.
3. De **uitvoer** van Azure Data Factory **dataset**maken. In dit voorbeeld wordt gepartitioneerd voor het maken van een unieke output pad voor elk segment kan worden uitgevoerd. De activiteit zou het bestand overschrijven zonder de partities.

        {
          "name": "DecisionTreeResultBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/scored/{folderpart}/",
              "fileName": "{filepart}result.csv",
              "partitionedBy": [
                {
                  "name": "folderpart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "yyyyMMdd"
                  }
                },
                {
                  "name": "filepart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HHmmss"
                  }
                }
              ],
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Day",
              "interval": 15
            }
          }
        }

4. Maak een **gekoppelde service** van het type: **AzureMLLinkedService**, die de API key en batch uitvoering URL model.
        
        {
          "name": "MyAzureMLLinkedService",
          "properties": {
            "type": "AzureML",
            "typeProperties": {
              "mlEndpoint": "https://[batch execution endpoint]/jobs",
              "apiKey": "[apikey]"
            }
          }
        }
5. Ten slotte auteur een pijpleiding met een **AzureMLBatchExecution** activiteit. Pijpleiding voert tijdens runtime, de volgende stappen uit:
    1. Met deze eigenschap wordt de locatie van het invoerbestand van uw invoer datasets.
    2. Hiermee wordt de uitvoering in Azure Machine Learning batch API
    3. De uitvoering batchuitvoer gekopieerd naar de blob in de dataset uitvoer gegeven. 

    > [AZURE.NOTE] AzureMLBatchExecution activiteit kan nul of meer ingangen en uitgangen van een of meer hebben.

        {
          "name": "PredictivePipeline",
          "properties": {
            "description": "use AzureML model",
            "activities": [
              {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                  {
                    "name": "DecisionTreeInputBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "DecisionTreeResultBlob"
                  }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                  "concurrency": 3,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 1,
                  "timeout": "02:00:00"
                }
              }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
          }
        }

    **Begin** en **einde** datum/tijd moet in de [ISO-notatie](http://en.wikipedia.org/wiki/ISO_8601)zijn. Bijvoorbeeld: 2014-10-14T16:32:41Z. **De eindtijd** is optioneel. Als u geen waarde voor de eigenschap **end** opgeeft, wordt berekend als "**start + 48 uur.**" Geef de pijplijn voor onbepaalde tijd uitvoeren, **9999-09-09** als de waarde voor de eigenschap **end** . Zie [Scripting JSON-naslaginformatie](https://msdn.microsoft.com/library/dn835050.aspx) voor meer informatie over eigenschappen van JSON.

    > [AZURE.NOTE] Invoer voor de AzureMLBatchExecution is activiteit optioneel. 

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Scenario: Proeven met Reader/Writer-Modules om te verwijzen naar gegevens in verschillende archieven

Een andere gangbare scenario bij het maken van Azure ML experimenten is het gebruik van modules lezer en schrijver. De module reader wordt gebruikt om gegevens te laden in een experiment en de schrijver module kan gegevens van de experimenten worden opgeslagen. Zie voor meer informatie over lezer-schrijver en [lezer](https://msdn.microsoft.com/library/azure/dn905997.aspx) en [schrijver](https://msdn.microsoft.com/library/azure/dn905984.aspx) onderwerpen op de MSDN-bibliotheek.     

Wanneer u de lezer en schrijver modules, is het verstandig een webserviceparameter gebruiken voor elke eigenschap van deze modules lezer/schrijver. Deze web-parameters kunnen u de waarden configureren tijdens runtime. Zo kunt u een experiment met een reader-module die gebruikmaakt van een Azure SQL-Database: XXX.database.windows.net. Nadat de webservice is geïmplementeerd, die u wilt inschakelen van de consumenten van de webservice te geven een andere Azure SQL Server YYY.database.windows.net genoemd. U kunt een webserviceparameter toestaan deze waarde moet worden geconfigureerd.

> [AZURE.NOTE] Web service invoer en uitvoer verschillen van de parameters van de Web service. In het eerste scenario hebt u gezien hoe de input en de output kunnen worden opgegeven voor een webservice Azure ML. In dit scenario kunt u parameters voor een webservice die overeenkomen met de eigenschappen van de lezer/schrijver modules doorgeven. 

We bekijken een scenario voor het gebruik van de parameters van de Web service. U hebt een geïmplementeerde Azure Machine Learning-webservice die gebruikmaakt van een module reader voor het lezen van gegevens uit een van de gegevensbronnen die worden ondersteund door Azure Machine Learning (bijvoorbeeld: Azure SQL-Database). Nadat de uitvoering van de batchverwerking is uitgevoerd, worden de resultaten geschreven met behulp van een module schrijver (Azure SQL-Database).  Geen web service ingangen en uitgangen worden gedefinieerd in de experimenten. In dit geval is het raadzaam dat u relevante web service-parameters voor de lezer en schrijver modules configureren. Deze configuratie kan de lezer/schrijver modules worden geconfigureerd wanneer u de AzureMLBatchExecution-activiteit. U opgeven parameters voor Web-service als volgt in de sectie **globalParameters** van de activiteit JSON. 


    "typeProperties": {
        "globalParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }

U kunt ook [Data Factory-functies](https://msdn.microsoft.com/library/dn835056.aspx) in het doorgeven van waarden voor de parameters van de webservice zoals weergegeven in het volgende voorbeeld:

    "typeProperties": {
        "globalParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Parameters van de webservice zijn hoofdlettergevoelig, dus zorg ervoor dat de namen die u opgeeft in de activiteit JSON overeenkomen met de gegevenstypen die door de webservice weergegeven. 

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>Een Reader-module gebruiken om gegevens te lezen van meerdere bestanden in Azure Blob
Grote gegevens pijpleidingen met activiteiten zoals varkens en component kan leiden tot een of meer bestanden zonder extensie uitvoer. Bijvoorbeeld wanneer u een tabel in een externe component opgeeft, kan de gegevens voor de externe component-tabel kunnen worden opgeslagen in Azure blobopslag met de volgende naam 000000_0. U kunt de module reader gebruiken in een experiment om meerdere bestanden te lezen en ze te gebruiken voor voorspellingen. 

Wanneer u de module lezer in een experiment Azure Machine Learning, kunt u de Azure Blob opgeven als invoer. De bestanden in de Azure blobopslag kunnen de uitvoerbestanden zijn (voorbeeld: 000000_0) die worden geproduceerd door een varken en component script dat wordt uitgevoerd op HDInsight. De reader-module kunt u configureren voor het lezen van bestanden (zonder extensie) de **pad naar de container directory blob/**. Het **pad naar de container** verwijst naar de container en de **directory/blob** verwijst naar de map die de bestanden bevat, zoals in de volgende afbeelding. Het sterretje, \*) **geeft aan dat alle bestanden in de container of de map (dat wil zeggen gegevens/aggregateddata/jaar = maand-2014-6 /\*)** als onderdeel van het experiment worden gelezen.

![Azure Blob-eigenschappen](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)


### <a name="example"></a>Voorbeeld 
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Pijplijn met AzureMLBatchExecution activiteit met de Parameters van de Web Service

    {
      "name": "MLWithSqlReaderSqlWriter",
      "properties": {
        "description": "Azure ML model with sql azure reader/writer",
        "activities": [
          {
            "name": "MLSqlReaderSqlWriterActivity",
            "type": "AzureMLBatchExecution",
            "description": "test",
            "inputs": [
              {
                "name": "MLSqlInput"
              }
            ],
            "outputs": [
              {
                "name": "MLSqlOutput"
              }
            ],
            "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
            "typeProperties":
            {
                "webServiceInput": "MLSqlInput",
                "webServiceOutputs": {
                    "output1": "MLSqlOutput"
                }
                "globalParameters": {
                    "Database server name": "<myserver>.database.windows.net",
                    "Database name": "<database>",
                    "Server user account name": "<user name>",
                    "Server user account password": "<password>"
                }              
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            },
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }
 
In het bovenstaande voorbeeld van JSON:

- De geïmplementeerde Azure Machine Learning webservice gebruikt een lezer en een schrijver module voor lezen/schrijven van/naar een Azure SQL-Database. Deze webservice beschrijft de volgende vier parameters: servernaam, databasenaam Server gebruikersnaam en wachtwoord van Server-Database.  
- **Begin** en **einde** datum/tijd moet in de [ISO-notatie](http://en.wikipedia.org/wiki/ISO_8601)zijn. Bijvoorbeeld: 2014-10-14T16:32:41Z. **De eindtijd** is optioneel. Als u geen waarde voor de eigenschap **end** opgeeft, wordt berekend als "**start + 48 uur.**" Geef de pijplijn voor onbepaalde tijd uitvoeren, **9999-09-09** als de waarde voor de eigenschap **end** . Zie [Scripting JSON-naslaginformatie](https://msdn.microsoft.com/library/dn835050.aspx) voor meer informatie over eigenschappen van JSON.

### <a name="other-scenarios"></a>Andere scenario 's

#### <a name="web-service-requires-multiple-inputs"></a>Meerdere ingangen is vereist
Als de webservice meerdere ingangen heeft, gebruikt u de eigenschap **webServiceInputs** in plaats van **webServiceInput**. DataSets waarnaar wordt verwezen door de **webServiceInputs** moet ook worden opgenomen in de activiteit- **ingangen**.
 
In het experiment Azure ML hebben web service input en output poorten en globale parameters standaardnamen ("input1", "input2") die u kunt aanpassen. De namen die u voor de webServiceInputs, webServiceOutputs en globalParameters-instellingen moeten exact overeenkomen met de namen in de experimenten. U kunt de nettolading monster aanvraag bekijken op de pagina Help voor Batch worden uitgevoerd voor uw Azure ML eindpunt om te controleren of de verwachte toewijzing.


    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [{
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [{
                    "name": "inputDataset1"
                }, {
                    "name": "inputDataset2"
                }],
                "outputs": [{
                    "name": "outputDataset"
                }],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties": {
                    "webServiceInputs": {
                        "input1": "inputDataset1",
                        "input2": "inputDataset2"
                    },
                    "webServiceOutputs": {
                        "output1": "outputDataset"
                    }
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }

#### <a name="web-service-does-not-require-an-input"></a>Web-Service vereist geen invoer

Azure ML batch uitvoering van webservices kunnen worden gebruikt voor het uitvoeren van de werkstromen voor voorbeeld R of Python scripts, waardoor mogelijk niet alle ingangen. Of het experiment kan worden geconfigureerd met een Reader-module die niet wordt weergegeven door een GlobalParameters. In dat geval wordt de activiteit van de AzureMLBatchExecution zou als volgt geconfigureerd:

    {
        "name": "scoring service",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "myBlob"
            }
        ],
        "typeProperties": {
            "webServiceOutputs": {
                "output1": "myBlob"
            }              
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },
   

#### <a name="web-service-does-not-require-an-inputoutput"></a>Web-Service hoeft niet een invoer/uitvoer
De webservice voor de uitvoering van Azure ML mogelijk geen uitvoer Web-Service geconfigureerd. In dit voorbeeld is er een Web Service input of output, noch een GlobalParameters zijn geconfigureerd. Er is nog steeds een uitvoer die is geconfigureerd op de activiteit zelf, maar er geen is opgegeven als een webServiceOutput.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "placeholderOutputDataset"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Web Service gebruikt lezers en schrijvers en de activiteit wordt uitgevoerd alleen als andere activiteiten zijn geslaagd.

Azure ML web service lezer en schrijver modules kunnen worden geconfigureerd met of zonder een GlobalParameters. U kunt echter serviceoproepen insluiten in een pijpleiding die dataset afhankelijkheden gebruikt voor het aanroepen van de service alleen als sommige upstream verwerking is voltooid. U kunt ook een andere actie activeren nadat de uitvoering van de batchverwerking is voltooid met behulp van deze aanpak. In dat geval kunt u de activiteit, ingangen en uitgangen, zonder deze webservice inputs of outputs met afhankelijkheden uitdrukken.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "inputs": [
            {
                "name": "upstreamData1"
            },
            {
                "name": "upstreamData2"
            }
        ],
        "outputs": [
            {
                "name": "downstreamData"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

De **takeaways** zijn:

-   Als uw experiment eindpunt een webServiceInput gebruikt: het wordt vertegenwoordigd door een dataset blob en is opgenomen in de ingangen van de activiteit en de eigenschap webServiceInput. Anders wordt wordt de eigenschap webServiceInput weggelaten. 
-   Als uw experiment eindpunt webServiceOutput(s) gebruikt: ze worden vertegenwoordigd door een blob datasets en zijn opgenomen in de resultaten van de activiteit en in de eigenschap webServiceOutputs. Hiermee kunt u de activiteit en webServiceOutputs worden toegewezen door de naam van elke uitvoer in het experiment. Anders wordt wordt de eigenschap webServiceOutputs weggelaten.
-   Als uw eindpunt experiment globalParameter(s) biedt, zijn zij voorkomen in de eigenschap globalParameters voor de activiteit als sleutel-waardeparen. Anders wordt wordt de eigenschap globalParameters weggelaten. De toetsen zijn hoofdlettergevoelig. [Azure Data Factory-functies](data-factory-scheduling-and-execution.md#data-factory-functions-reference) kunnen worden gebruikt in de waarden. 
- Extra datasets mogen worden opgenomen in de activiteit-eigenschappen van ingangen en uitgangen, zonder waarnaar wordt verwezen in de activiteit typeProperties. Deze datasets worden uitgevoerd met behulp van afhankelijkheden segment bepalen maar anders worden genegeerd door de AzureMLBatchExecution-activiteit. 


## <a name="updating-models-using-update-resource-activity"></a>Modellen met Update Resource activiteit bijwerken
Na verloop van tijd moeten de voorspellende modellen in de Azure ML scoren experimenten met nieuwe invoer datasets worden retrained. Nadat u klaar bent met de bijscholing, die u wilt bijwerken de score webservice met het retrained ML model. De gebruikelijke stappen opnieuw opleiden en updating Azure ML modellen via webservices inschakelen zijn: 

1. Maak een experiment in [Azure ML Studio](https://studio.azureml.net). 
2. Wanneer u tevreden met het model bent, Azure ML Studio gebruiken voor het publiceren van webservices voor zowel de **training experimenteren** en scoren /**voorspellende experiment**.

De volgende tabel beschrijft de webservices gebruikt in dit voorbeeld.  Zie [Machine Learning Retrain modellen via programmacode](../machine-learning/machine-learning-retrain-models-programmatically.md) voor meer informatie.

| Type van webservice | Beschrijving 
| :------------------ | :---------- 
| **Opleiding webservice** | Trainingsgegevens ontvangt en opgeleide modellen produceert. De uitvoer van de bijscholing is een .ilearner bestand in een Azure Blob-opslag.  Het **standaardeindpunt** wordt automatisch voor u gemaakt wanneer u het experiment training als een webservice publiceert. U kunt meer eindpunten maken maar het voorbeeld wordt alleen het standaardeindpunt |
| **Score van webservice** | Voorbeelden van gegevens zonder label ontvangt en voorspellingen maakt. De uitvoer van voorspelling kan hebben verschillende vormen, zoals een CSV-bestand of de rijen in een Azure SQL-database, afhankelijk van de configuratie van het experiment. Het standaardeindpunt wordt automatisch voor u gemaakt wanneer u de voorspellende experiment als een webservice publiceert. Het tweede **eindpunt voor niet-standaard en bij te werken** via de [Azure portal](https://manage.windowsazure.com)maken. In dit voorbeeld wordt slechts één eindpunt voor niet-standaard-bij te werken, maar kunt u meer eindpunten. Zie het artikel [Eindpunten maken](../machine-learning/machine-learning-create-endpoint.md) voor stapsgewijze instructies.       
 
In de volgende afbeelding ziet u de relatie tussen opleiding en eindpunten in Azure ML scoren. 

![Webservices](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)


U kunt de **training webservice** aanroepen met behulp van de **Activiteit van Azure ML Batch worden uitgevoerd**. Het aanroepen van een webservice training is gelijk aan het aanroepen van een webservice voor Azure ML (webservice scoren) score gegevens. De voorgaande secties wordt beschreven hoe aanroepen van een webservice Azure ML van een pijpleiding Azure Data Factory in detail. 
  
U kunt voor het **scoren van webservice** aanroepen met behulp van de **Azure ML-Resource activiteit** naar de webservice bijwerken met het nieuwe opgeleide model. Zoals vermeld in de bovenstaande tabel, moet u het maken en gebruiken van het eindpunt van niet-standaard-bij te werken. Eventuele bestaande gekoppelde services in uw fabriek gegevens en het eindpunt van de niet-standaard gebruiken zodat gebruik altijd de meest recente retrained model ook bijwerken. 

In het volgende scenario geeft meer details. Een voorbeeld van her- en bijscholingscursussen Azure ML modellen van een pijpleiding Azure Data Factory heeft. 
 
### <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Scenario: her- en bijscholingscursussen een model Azure ML
Deze sectie bevat een pijpleiding monster met de **uitvoering van Azure ML Batch activiteit** retrain een model. De pijpleiding wordt de **Azure ML bijwerken Resource activiteit** ook het model in de webservice score bijwerken. De sectie biedt ook JSON fragmenten voor de gekoppelde services, datasets en pijpleiding in het voorbeeld. 

Hier wordt de diagramweergave van de pijpleiding monster. Zoals u ziet, de Azure ML Batch uitvoering haalt de invoer voor training en produceert de uitvoer van een opleiding (iLearner-bestand). De Azure ML Update Resource activiteit duurt deze training-uitvoer en werkt het model in het scorepatroon web-service-eindpunt. De activiteit van de Resource-Update niet er geen uitvoer geproduceerd. De placeholderBlob is een dummy uitvoer dataset is vereist door de Azure Data Factory service voor het uitvoeren van de pijpleiding. 

![Pipeline-diagram](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


#### <a name="azure-blob-storage-linked-service"></a>Azure Blob storage service gekoppeld:
De opslag van Azure bevat de volgende gegevens:

- trainingsgegevens. De ingevoerde gegevens voor de webservice Azure ML training.  
- het bestand iLearner. De uitvoer van de webservice Azure ML training. Dit bestand is ook de invoer voor de activiteit van de bron van de Update.  
   
Hier is de steekproef JSON-definitie van de gekoppelde service: 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
            }
        }
    }


#### <a name="training-input-dataset"></a>Invoer dataset training:
De volgende dataset vertegenwoordigt de invoer trainingsgegevens voor de webservice Azure ML training. De uitvoering van Azure ML Batch-activiteit duurt deze dataset als invoer. 

    {
        "name": "trainingData",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "labeledexamples",
                "fileName": "labeledexamples.arff",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "policy": {          
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

#### <a name="training-output-dataset"></a>Training uitvoer dataset:
De volgende dataset vertegenwoordigt het uitvoerbestand van het iLearner van de webservice Azure ML training. De uitvoering van Azure ML Batch-activiteit produceert deze dataset. Deze dataset is ook de invoer voor de activiteit Azure ML Update Resource.

    {
        "name": "trainedModelBlob",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "trainingoutput",
                "fileName": "model.ilearner",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            }
        }
    }

#### <a name="linked-service-for-azure-ml-training-endpoint"></a>Gekoppelde service voor Azure ML training eindpunt 
In het volgende fragment van JSON definieert een service Azure Machine Learning gekoppeld die naar het standaardeindpunt van de opleiding webservice verwijst. 

    {   
        "name": "trainingEndpoint",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
                "apiKey": "myKey"
            }
        }
    }

Doe het volgende als u de waarden voor **mlEndpoint** en **apiKey**in **Azure ML Studio**:

1. Klik op **WEB SERVICES** in het menu links.
2. Klik op de **opleiding web-service** in de lijst met webservices. 
3. Klik op kopiëren naast het tekstvak **API-sleutel** . De sleutel in het Klembord plakken in de editor Data Factory JSON.
4. Klik op koppeling **BATCH EXECUTION** in **Azure ML studio**.
5. De **Aanvraag-URI** van de sectie **aanvragen** kopiëren en plakken in de editor Data Factory JSON.   


#### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Gekoppelde Service voor Azure ML bijgewerkt score eindpunt:
In het volgende fragment van JSON definieert een service Azure Machine Learning gekoppeld die naar het niet-standaard bijgewerkt eindpunt van de score van de webservice verwijst.  

    {
        "name": "updatableScoringEndpoint2",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
                "apiKey": "endpoint2Key",
                "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
            }
        }
    }


Voordat gekoppeld service maken en implementeren van een ML Azure, volg de stappen in artikel [Eindpunten maken](../machine-learning/machine-learning-create-endpoint.md) om een tweede (niet-standaard en kan worden bijgewerkt) eindpunt voor de score van de webservice te maken.

Nadat u het eindpunt van niet-standaard-bij te werken, het volgende doen:

- Klik op **BATCH worden uitgevoerd** om de URI-waarde voor de eigenschap **mlEndpoint** JSON.
- Klik op de koppeling **Bron bijwerken** om de URI-waarde voor de eigenschap **updateResourceEndpoint** JSON. De API-sleutel is op het eindpunt pagina zelf (in de rechterbenedenhoek). 

![eindpunt bij te werken](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

 
#### <a name="placeholder-output-dataset"></a>Tijdelijke uitvoer dataset:
De activiteit van de Resource van Azure ML Update genereert geen uitvoer. Azure Data Factory vereist echter een dataset uitvoer om de planning van een pijpleiding. Daarom gebruiken we een dataset pop/tijdelijke aanduiding in het volgende voorbeeld.  

    {
        "name": "placeholderBlob",
        "properties": {
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "any",
                "format": {
                    "type": "TextFormat"
                }
            }
        }
    }


#### <a name="pipeline"></a>Pijpleiding
De pijplijn heeft twee activiteiten: **AzureMLBatchExecution** en **AzureMLUpdateResource**. De activiteit Azure ML Batch Execution neemt de trainingsgegevens als invoer en een iLearner-bestand als de output van een produceert. De activiteit roept de webservice training (training experiment als webservice beschikbaar) met de gegevens van de invoer van opleiding en ontvangt het bestand ilearner van de webservice. De placeholderBlob is een dummy uitvoer dataset is vereist door de Azure Data Factory service voor het uitvoeren van de pijpleiding. 

![Pipeline-diagram](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


    {
        "name": "pipeline",
        "properties": {
            "activities": [
                {
                    "name": "retraining",
                    "type": "AzureMLBatchExecution",
                    "inputs": [
                        {
                            "name": "trainingData"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "typeProperties": {
                        "webServiceInput": "trainingData",
                        "webServiceOutputs": {
                            "output1": "trainedModelBlob"
                        }              
                     },
                    "linkedServiceName": "trainingEndpoint",
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "02:00:00"
                    }
                },
                {
                    "type": "AzureMLUpdateResource",
                    "typeProperties": {
                        "trainedModelName": "Training Exp for ADF ML [trained model]",
                        "trainedModelDatasetName" :  "trainedModelBlob"
                    },
                    "inputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "placeholderBlob"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "name": "AzureML Update Resource",
                    "linkedServiceName": "updatableScoringEndpoint2"
                }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }


### <a name="reader-and-writer-modules"></a>Lezer en schrijver Modules

Een algemeen scenario voor het gebruik van de parameters van de Web service is het gebruik van Azure SQL lezers en schrijvers. De module reader wordt gebruikt om gegevens te laden in een experiment van data management-services buiten Azure Machine Learning Studio. De module schrijver is gegevens van de experimenten in het beheer van gegevens buiten de Azure Machine Learning Studio opslaan.  

Zie voor meer informatie over Azure Blob/Azure SQL-lezer/schrijver, [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) en [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) onderwerpen op de MSDN-bibliotheek. In het voorbeeld in de vorige sectie, gebruikt de Azure Blob-lezer en schrijver Azure Blob. In deze sectie wordt beschreven met behulp van SQL Azure lezer en schrijver Azure SQL.


## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Q:** Ik heb meerdere bestanden die worden gegenereerd door mijn grote gegevens pijpleidingen. Kan ik de AzureMLBatchExecution activiteit voor alle bestanden gebruiken?

**A:** Ja. Zie de sectie **een Reader-module gebruiken om te lezen van gegevens uit meerdere bestanden in Azure Blob** voor meer informatie. 

## <a name="azure-ml-batch-scoring-activity"></a>Azure ML Batch scoren activiteit
Als u de **AzureMLBatchScoring** -activiteit te integreren met Azure Machine Learning, wordt u aangeraden de laatste activiteit van de **AzureMLBatchExecution** te gebruiken. 

De AzureMLBatchExecution activiteit is geïntroduceerd in de release augustus 2015 van Azure SDK en Azure PowerShell.

Als u doorgaan met de AzureMLBatchScoring-activiteit wilt, gaan via deze sectie.  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Azure ML Batch scoren activiteit met Azure opslag voor invoer/uitvoer 

    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchScoring",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "ScoringInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "ScoringResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

### <a name="web-service-parameters"></a>Parameters van de Web Service
Toevoegen als u waarden voor de parameters van de Web service, een sectie **typeProperties** aan de sectie **AzureMLBatchScoringActivty** van de pijpleiding JSON zoals in het volgende voorbeeld: 

    "typeProperties": {
        "webServiceParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }


U kunt ook [Data Factory-functies](https://msdn.microsoft.com/library/dn835056.aspx) in het doorgeven van waarden voor de parameters van de webservice zoals weergegeven in het volgende voorbeeld:

    "typeProperties": {
        "webServiceParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] Parameters van de webservice zijn hoofdlettergevoelig, dus zorg ervoor dat de namen die u opgeeft in de activiteit JSON overeenkomen met de gegevenstypen die door de webservice weergegeven. 

## <a name="see-also"></a>Zie ook

- [Azure blogbericht: aan de slag met Azure Data Factory en Azure Machine leren](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)





[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/


 

<properties 
    pageTitle="MapReduce-programma van Azure Data Factory aanroepen" 
    description="Informatie over het verwerken van gegevens door MapReduce programma's uitvoeren op een cluster Azure HDInsight van een fabriek Azure gegevens." 
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
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="invoke-mapreduce-programs-from-data-factory"></a>Data Factory MapReduce programma's aanroepen
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Streaming Hadoop](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics U SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)

De activiteit HDInsight MapReduce in een Data Factory- [pijpleiding](data-factory-create-pipelines.md) voert MapReduce programma's op [uw eigen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [op verzoek](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) van Windows, Linux-gebaseerde HDInsight cluster. Dit artikel is gebaseerd op het artikel [data transformation activiteiten](data-factory-data-transformation-activities.md) , dat een algemeen overzicht van de gegevenstransformatie van en de transformatie van ondersteunde activiteiten geeft.

## <a name="introduction"></a>Inleiding 
Gegevens in gekoppelde storage-services verwerkt een pijpleiding in een fabriek Azure gegevens door middel van gekoppelde compute-services. Het bevat een reeks activiteiten waarbij elke activiteit een bepaalde verwerking uitvoert. In dit artikel wordt beschreven hoe de HDInsight MapReduce activiteit.
 
Zie [varken](data-factory-pig-activity.md) en [component](data-factory-hive-activity.md) voor meer informatie over het uitvoeren van scripts varken/component op een cluster met Windows, Linux-gebaseerde HDInsight van een pijpleiding via component-en HDInsight varkens. 

## <a name="json-for-hdinsight-mapreduce-activity"></a>JSON voor HDInsight MapReduce activiteit 

In de JSON-definitie voor de activiteit van HDInsight: 
 
1. Het **type** van de **activiteit** wordt ingesteld op **HDInsight**.
3. Geef de naam van de klasse van de eigenschap **className** .
4. Geef het pad naar het JAR-bestand met de naam van de eigenschap **jarFilePath** .
5. Geef de gekoppelde service die verwijst naar de Azure Blob-opslag met het JAR-bestand voor de eigenschap **jarLinkedService** .   
6. Argumenten voor het programma MapReduce in de sectie **argumenten** opgeven. Tijdens runtime, ziet u een paar extra argumenten (bijvoorbeeld: mapreduce.job.tags) in het kader van MapReduce. Als u wilt uw argumenten met de argumenten MapReduce onderscheiden, kunt u overwegen optie en de waarde als argumenten zoals in het volgende voorbeeld (- s,--input,--output enz., zijn opties die onmiddellijk gevolgd door hun waarden).

        {
            "name": "MahoutMapReduceSamplePipeline",
            "properties": {
                "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
                "activities": [
                    {
                        "type": "HDInsightMapReduce",
                        "typeProperties": {
                            "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                            "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                            "jarLinkedService": "StorageLinkedService",
                            "arguments": [
                                "-s",
                                "SIMILARITY_LOGLIKELIHOOD",
                                "--input",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                                "--output",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                                "--maxSimilaritiesPerItem",
                                "500",
                                "--tempDir",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                            ]
                        },
                        "inputs": [
                            {
                                "name": "MahoutInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "MahoutOutput"
                            }
                        ],
                        "policy": {
                            "timeout": "01:00:00",
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "MahoutActivity",
                        "description": "Custom Map Reduce to generate Mahout result",
                        "linkedServiceName": "HDInsightLinkedService"
                    }
                ],
                "start": "2014-01-03T00:00:00Z",
                "end": "2014-01-04T00:00:00Z",
                "isPaused": false,
                "hubName": "mrfactory_hub",
                "pipelineMode": "Scheduled"
            }
        }
    
    

U kunt de activiteit HDInsight MapReduce MapReduce jar bestanden op een HDInsight-cluster worden uitgevoerd. In het volgende voorbeeld JSON-definitie van een pijpleiding, is de HDInsight activiteit geconfigureerd een Mahout JAR-bestand.

## <a name="sample-on-github"></a>Monster op GitHub
Kunt u een voorbeeld voor het gebruik van de HDInsight MapReduce activiteit in: [Fabriek voorbeelden op GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-the-word-count-program"></a>Het programma Word Count
De pijpleiding die in dit voorbeeld voert het programma Word Count kaart/verminderen op het cluster Azure HDInsight.   

### <a name="linked-services"></a>Gekoppelde Services
U maakt eerst een gekoppelde service de Azure opslag die wordt gebruikt door het cluster Azure HDInsight fabriek Azure gegevens koppelen. Vergeet niet te vervangen door de naam en de sleutel van uw opslag Azure **accountnaam** en **key account** als kopiëren en plakken van de volgende code. 

#### <a name="azure-storage-linked-service"></a>Azure gekoppeld opslagservice

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
            }
        }
    }

#### <a name="azure-hdinsight-linked-service"></a>Azure gekoppeld HDInsight service
Vervolgens maakt u een gekoppelde service als u wilt uw cluster Azure HDInsight fabriek Azure gegevens koppelen. Kopiëren en plakken van de volgende code vervangen door de naam van het cluster HDInsight **HDInsight clusternaam** als waarden van gebruikersnaam en wachtwoord te wijzigen.   

    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }

### <a name="datasets"></a>DataSets

#### <a name="output-dataset"></a>Dataset-uitvoer
De pijpleiding die in dit voorbeeld vindt niet alle ingangen. U opgeven een dataset uitvoer voor de HDInsight MapReduce activiteit. Deze dataset is gewoon een dummy dataset is vereist om de planning van de pijpleiding.  

    {
        "name": "MROutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "WordCountOutput1.txt",
                "folderPath": "example/data/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Pijpleiding
De pijpleiding die in dit voorbeeld is slechts één activiteit van het type: HDInsightMapReduce. Enkele van de belangrijke eigenschappen in de JSON zijn: 

Eigenschap | Notities
:-------- | :-----
type | Het type moet worden ingesteld op **HDInsightMapReduce**. 
Klassenaam | De naam van de klasse: **wordcount**
jarFilePath | Het pad naar het jar-bestand met de klasse. Vergeet niet de naam van het cluster wijzigen als kopiëren en plakken van de volgende code. 
jarLinkedService | Azure Storage service met het jar-bestand gekoppeld. Deze service gekoppelde verwijst naar de opslag die is gekoppeld aan het cluster HDInsight. 
argumenten | Het programma wordcount heeft twee argumenten, invoer en uitvoer. Het invoerbestand is het bestand davinci.txt.
frequentie-interval | De waarden voor deze eigenschappen overeen met de uitvoer dataset. 
linkedServiceName | verwijst naar de HDInsight gekoppeld service die u eerder had gemaakt.   

    {
        "name": "MRSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run the Word Count Program",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "wordcount",
                        "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "/example/data/gutenberg/davinci.txt",
                            "/example/data/WordCountOutput1"
                        ]
                    },
                    "outputs": [
                        {
                            "name": "MROutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "MRActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-03T00:00:00Z",
            "end": "2014-01-04T00:00:00Z"
        }
    }

## <a name="run-spark-programs"></a>Spark-programma's uitvoeren
U kunt MapReduce activiteit Spark programma's uitvoert op uw cluster HDInsight Spark. Zie [programma's aanroepen Spark van Azure Data Factory](data-factory-spark.md) voor meer informatie.  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com
 
## <a name="see-also"></a>Zie ook
- [Activiteit van de component](data-factory-hive-activity.md)
- [Varken activiteit](data-factory-pig-activity.md)
- [Hadoop Streaming activiteit](data-factory-hadoop-streaming-activity.md)
- [Spark programma's aanroepen](data-factory-spark.md)
- [R-scripts starten](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

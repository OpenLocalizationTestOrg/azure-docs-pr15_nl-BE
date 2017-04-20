<properties 
    pageTitle="Hadoop Streaming activiteit" 
    description="Informatie over hoe u kunt de activiteit Hadoop Streaming in een fabriek Azure gegevens Hadoop Streaming programma's uitvoert op een op vraag/uw eigen HDInsight cluster." 
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
    ms.date="09/20/2016" 
    ms.author="shlo"/>

# <a name="hadoop-streaming-activity"></a>Hadoop Streaming activiteit
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Streaming Hadoop](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics U SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)

U kunt de HDInsightStreamingActivity-activiteit roept een taak Hadoop Streaming van een pijpleiding Azure Data Factory. In het volgende fragment van JSON ziet u de syntaxis voor het gebruik van de HDInsightStreamingActivity in een pijpleiding JSON-bestand. 

HDInsight Streaming activiteit in een Data Factory- [pijpleiding](data-factory-create-pipelines.md) voert Hadoop Streaming programma's op [uw eigen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [op verzoek](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) van Windows, Linux-gebaseerde HDInsight cluster. Dit artikel is gebaseerd op het artikel [data transformation activiteiten](data-factory-data-transformation-activities.md) , dat een algemeen overzicht van de gegevenstransformatie van en de transformatie van ondersteunde activiteiten geeft.

## <a name="json-sample"></a>JSON-voorbeeld
Het cluster HDInsight wordt automatisch gevuld met de voorbeeld-programma's (wc.exe en cat.exe) en gegevens (davinci.txt). Naam van de container die wordt gebruikt door het cluster HDInsight is de naam van het cluster zelf. Als de naam van het cluster myhdicluster is, zou de naam van de blob-container die myhdicluster zijn. 

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<nameofthecluster>/example/apps/wc.exe",
                            "<nameofthecluster>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService",
                        "getDebugInfo": "Failure"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

Houd rekening met de volgende punten:

1. De **linkedServiceName** instellen op de naam van de gekoppelde service die verwijst naar de HDInsight cluster waarop de streaming mapreduce-taak wordt uitgevoerd.
2. Het type van de activiteit wordt ingesteld op **HDInsightStreaming**.
3. Geef de naam van het uitvoerbare bestand mapper voor de eigenschap **mapper** . In het voorbeeld is cat.exe de mapper uitvoerbaar.
4. Geef de naam van het uitvoerbare bestand reducer voor de eigenschap **reducer** . In het voorbeeld is wc.exe de uitvoerbare reducer.
5. De eigenschap **input** type geeft u het invoerbestand (met inbegrip van de locatie) voor de toewijzing. In het voorbeeld: "wasb://adfsample@ <account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt ": adfsample de blob-container, gegevens-voorbeeld/Gutenberg is de map, en davinci.txt is de blob.
6. De eigenschap van het type **uitvoer** geeft u het uitvoerbestand (met inbegrip van de locatie) voor de reducer. De uitvoer van de taak Hadoop Streaming wordt weggeschreven naar de locatie die is opgegeven voor deze eigenschap.
7. Geef de paden voor de toewijzing en reducer uitvoerbare bestanden in de sectie **filePaths** . In het voorbeeld: 'adfsample/example/apps/wc.exe', adfsample is de container blob voorbeeld/apps is de map en wc.exe is het uitvoerbare bestand.
8. Voor de eigenschap **fileLinkedService** geeft u de service Azure opslag gekoppeld waarmee de Azure opslag met de bestanden die zijn opgegeven in de sectie filePaths.
9. Voor de eigenschap **argumenten** waarden opgeven voor de taak voor streaming.
10. De eigenschap **getDebugInfo** is een optioneel element. Wanneer deze is ingesteld op mislukt, worden de logboeken alleen op storing gedownload. Wanneer deze is ingesteld op alle, worden logboeken altijd gedownload ongeacht de uitvoeringsstatus.

> [AZURE.NOTE] In het voorbeeld ziet u een dataset uitvoer voor de activiteit Hadoop Streaming voor de eigenschap **uitvoer** . Deze dataset is gewoon een dummy dataset is vereist om de planning van de pijpleiding. U hoeft niet op te geven van een dataset input voor de activiteit voor de eigenschap **ingangen** .  

    
## <a name="example"></a>Voorbeeld
De pijpleiding die in dit scenario voert het programma voor streaming kaart/verminderen van woorden tellen op de cluster HDInsight Azure. 

### <a name="linked-services"></a>Gekoppelde services

#### <a name="azure-storage-linked-service"></a>Azure gekoppeld opslagservice
U maakt eerst een gekoppelde service de Azure opslag die wordt gebruikt door het cluster Azure HDInsight fabriek Azure gegevens koppelen. Vergeet niet te vervangen door de naam en de sleutel van uw opslag Azure accountnaam en key account als kopiëren en plakken van de volgende code. 

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
Vervolgens maakt u een gekoppelde service als u wilt uw cluster Azure HDInsight fabriek Azure gegevens koppelen. Kopiëren en plakken van de volgende code vervangen door de naam van het cluster HDInsight HDInsight clusternaam als waarden van gebruikersnaam en wachtwoord te wijzigen. 
    
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
De pijpleiding die in dit voorbeeld vindt niet alle ingangen. U geeft een dataset uitvoer voor de activiteit HDInsight Streaming. Deze dataset is gewoon een dummy dataset is vereist om de planning van de pijpleiding. 

    {
        "name": "StreamingOutputDataset",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/streamingdata/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                },
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Pijpleiding

De pijpleiding die in dit voorbeeld is slechts één activiteit van het type: **HDInsightStreaming**. 

Het cluster HDInsight wordt automatisch gevuld met de voorbeeld-programma's (wc.exe en cat.exe) en gegevens (davinci.txt). Naam van de container die wordt gebruikt door het cluster HDInsight is de naam van het cluster zelf. Als de naam van het cluster myhdicluster is, zou de naam van de blob-container die myhdicluster zijn.  

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<blobcontainer>/example/apps/wc.exe",
                            "<blobcontainer>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

## <a name="see-also"></a>Zie ook
- [Activiteit van de component](data-factory-hive-activity.md)
- [Varken activiteit](data-factory-pig-activity.md)
- [MapReduce-activiteit](data-factory-map-reduce.md)
- [Spark programma's aanroepen](data-factory-spark.md)
- [R-scripts starten](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


<properties 
    pageTitle="Varken activiteit" 
    description="Informatie over hoe u kunt de activiteit van het varken in een fabriek Azure gegevens varken scripts uitvoeren op een op vraag/uw eigen HDInsight cluster." 
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
    ms.date="08/31/2016" 
    ms.author="shlo"/>

# <a name="pig-activity"></a>Varken activiteit
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Streaming Hadoop](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics U SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)

De activiteit HDInsight varkens in een Data Factory- [pijpleiding](data-factory-create-pipelines.md) voert varken query's op [uw eigen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [op verzoek](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) van Windows, Linux-gebaseerde HDInsight cluster. Dit artikel is gebaseerd op het artikel [data transformation activiteiten](data-factory-data-transformation-activities.md) , dat een algemeen overzicht van de gegevenstransformatie van en de transformatie van ondersteunde activiteiten geeft.

## <a name="syntax"></a>Syntaxis

    {
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "Pig Activity",
                "description": "description",
                "type": "HDInsightPig",
                "inputs": [
                    {
                        "name": "input tables"
                    }
                ],
                "outputs": [
                    {
                        "name": "output tables"
                    }
                ],
                "linkedServiceName": "MyHDInsightLinkedService",
                "typeProperties": {
                    "script": "Pig script",
                    "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                    "defines": {
                        "param1": "param1Value"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
            }
        ]
      }
    }

## <a name="syntax-details"></a>Details van de syntaxis

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
naam | Naam van de activiteit | Ja
Beschrijving | Tekst met een beschrijving van wat de activiteit wordt gebruikt | Nee
type | HDinsightPig | Ja
ingangen | Een of meer verbruikte "inputs" door de activiteit van het varken | Nee
uitgangen | Een of meer uitgangen, geproduceerd door de activiteit van het varken | Ja
linkedServiceName | Verwijzing naar het cluster HDInsight geregistreerd als een gekoppelde service in Data Factory | Ja
script | Geef het script varken inline | Nee
pad naar script | Het varken script opslaan in een Azure blobopslag en het pad naar het bestand. Gebruik de eigenschap 'script' of 'scriptPath'. Beide kunnen niet samen worden gebruikt. De naam is hoofdlettergevoelig. | Nee
Hiermee definieert u | Parameters opgeven als sleutel/waarde-paren voor het verwijzen naar binnen het script varken | Nee

## <a name="example"></a>Voorbeeld

Laten we eens een voorbeeld van het spel logboeken analytics waar u de tijd besteed door spelers spelen van games die door uw bedrijf identificeren.
 
Het volgende voorbeeld spel logboek is een bestand met door komma's (,) van elkaar gescheiden. Het bevat de volgende velden: profiel-id, SessionStart, SrcIPAddress, duur en GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

Het **varken script** om deze gegevens te verwerken:

    PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    
    GroupProfile = Group PigSampleIn all;
    
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    
    Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');

Voor het uitvoeren van dit script varkens in een pijplijn Data Factory, het volgende doen:

1. Maak een gekoppelde service Registreer [uw eigen HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [HDInsight op verzoek compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)configureren. We gaan deze gekoppelde service **HDInsightLinkedService**aanroepen.
2.  Maak een [gekoppelde service](data-factory-azure-blob-connector.md) om de verbinding met de gegevens hosten Azure Blob-opslag configureren. We gaan deze gekoppelde service **StorageLinkedService**aanroepen.
3.  Maak [datasets](data-factory-create-datasets.md) invoer- en uitvoergegevens aan te wijzen. We roepen de invoer dataset **PigSampleIn** en de uitvoer dataset **PigSampleOut**.
4.  De varkens query kopiëren in een bestand met de Azure Blob-opslag geconfigureerd in stap #2. Als de Azure opslag die fungeert als host voor de gegevens van die fungeert als host voor het query-bestand verschilt, maakt u een aparte opslag van Azure gekoppeld service. Verwijzen naar de gekoppelde service in de configuratie van de activiteit. Met **scriptPath **geeft u het pad naar het scriptbestand varken en **scriptLinkedService**. 
    
    > [AZURE.NOTE] U kunt ook de inline varken script in de definitie van de activiteit met behulp van de eigenschap **script** geven. Maar we raden deze aanpak niet alle speciale tekens in het script moet worden voorafgegaan en foutopsporing problemen kan veroorzaken. De beste manier is om stap #4.
5. De pijplijn maken met de HDInsightPig-activiteit. Deze activiteit worden de ingevoerde gegevens met een varken script op HDInsight cluster verwerkt.

        {
          "name": "PigActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                  {
                    "name": "PigSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "PigSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
              }
            ]
          }
        } 
6. Implementeren van de pijpleiding. Zie [Creating pijpleidingen](data-factory-create-pipelines.md) -artikel voor meer informatie. 
7. De pijplijn met behulp van de fabriek energiebewaking en -weergaven van gegevens controleren. Zie [controleren en beheren van Data Factory pijpleidingen](data-factory-monitor-manage-pipelines.md) artikel voor meer informatie.

## <a name="specifying-parameters-for-a-pig-script"></a>Opgeven van parameters voor het script van een varken 

Bekijk het volgende voorbeeld: spel logboeken worden dagelijks ingenomen in Azure Blob-opslag en opgeslagen in een map gepartitioneerde op datum en tijd. Wilt u het script varken voorzien en dynamisch geeft u de locatie van de invoer tijdens de runtime en produceren ook de uitvoer gepartitioneerd met datum en tijd.
 
Geparametriseerde varken als script wilt gebruiken, het volgende doen:

- Definieer de parameters in **definieert**.

        {
            "name": "PigActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "PigActivitySample",
                    "type": "HDInsightPig",
                    "inputs": [
                        {
                            "name": "PigSampleIn"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "PigSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0: %M}/dayno={0: %d}/',SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    }
                }
            ]
          }
        }  
      
- In het Script varken verwijzen naar de parameters met '**$parameterName**', zoals in het volgende voorbeeld wordt getoond:

        PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);   
        GroupProfile = Group PigSampleIn all;       
        PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);      
        Store PigSampleOut into '$Output' USING PigStorage (','); 


## <a name="see-also"></a>Zie ook
- [Activiteit van de component](data-factory-hive-activity.md)
- [MapReduce-activiteit](data-factory-map-reduce.md)
- [Hadoop Streaming activiteit](data-factory-hadoop-streaming-activity.md)
- [Spark programma's aanroepen](data-factory-spark.md)
- [R-scripts starten](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)



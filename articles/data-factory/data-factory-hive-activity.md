<properties 
    pageTitle="Activiteit van de component" 
    description="Informatie over hoe u kunt de component activiteit in een fabriek Azure gegevens component query's uitvoeren op een op vraag/uw eigen HDInsight cluster." 
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
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="hive-activity"></a>Activiteit van de component
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Streaming Hadoop](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics U SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)

De component HDInsight activiteit in een Data Factory- [pijpleiding](data-factory-create-pipelines.md) voert component query's op [uw eigen](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [op verzoek](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) cluster van Windows, Linux-gebaseerde HDInsight. Dit artikel is gebaseerd op het artikel [data transformation activiteiten](data-factory-data-transformation-activities.md) , dat een algemeen overzicht van de gegevenstransformatie van en de transformatie van ondersteunde activiteiten geeft.

## <a name="syntax"></a>Syntaxis

    {
        "name": "Hive Activity",
        "description": "description",
        "type": "HDInsightHive",
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
          "script": "Hive script",
          "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
       "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
    }
    
## <a name="syntax-details"></a>Details van de syntaxis

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
naam | Naam van de activiteit | Ja
Beschrijving | Tekst met een beschrijving van wat de activiteit wordt gebruikt | Nee
type | HDinsightHive | Ja
ingangen | Door de activiteit van de component verbruikte "inputs" | Nee
uitgangen | Geproduceerd door de activiteit van de component uitgangen | Ja 
linkedServiceName | Verwijzing naar het cluster HDInsight geregistreerd als een gekoppelde service in Data Factory | Ja 
script | Geef de component script inline | Nee
pad naar script | Het onderdeel script opslaan in een Azure blobopslag en het pad naar het bestand. Gebruik de eigenschap 'script' of 'scriptPath'. Beide kunnen niet samen worden gebruikt. De naam is hoofdlettergevoelig. | Nee 
Hiermee definieert u | Parameters opgeven als sleutel/waarde-paren voor het verwijzen naar in het onderdeel script met behulp van 'hiveconf'  | Nee

## <a name="example"></a>Voorbeeld

Laten we eens een voorbeeld van het spel logboeken analytics waar u de tijd besteed aan het spelen van games die door uw bedrijf gebruikers identificeren. 

Het volgende logboek is een monster game logboek komma (`,`) gescheiden en bevat de volgende velden: profiel-id, SessionStart, SrcIPAddress, duur en GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

De **component script** om deze gegevens te verwerken:

    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID       string, 
        SessionStart    string, 
        Duration        int, 
        SrcIPAddress    string, 
        GameType        string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 
    
    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (   
        ProfileID   string, 
        Duration    int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';
    
    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID

Voor het uitvoeren van dit script component in een pijplijn Data Factory, moet u het volgende doen

1. Maak een gekoppelde service Registreer [uw eigen HDInsight compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) of [HDInsight op verzoek compute cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)configureren. We gaan deze gekoppelde service 'HDInsightLinkedService' aanroepen.
2. Maak een [gekoppelde service](data-factory-azure-blob-connector.md) om de verbinding met de gegevens hosten Azure Blob-opslag configureren. We gaan deze gekoppelde service 'StorageLinkedService' aanroepen
3. Maak [datasets](data-factory-create-datasets.md) invoer- en uitvoergegevens aan te wijzen. We bellen de invoer dataset 'HiveSampleIn' en de dataset uitvoer "HiveSampleOut"
4. Kopie van de query component als een bestand naar Azure blobopslag geconfigureerd in stap #2. Als de opslag voor het hosten van de gegevens van deze query-bestand, een aparte opslag van Azure gekoppeld service maken en in de activiteit verwijzen. Met **scriptPath **geeft u het pad naar componentbestand query en **scriptLinkedService** het Azure opslag met het scriptbestand opgeven. 

    > [AZURE.NOTE] U kunt ook het onderdeel script in line in de definitie van de activiteit met behulp van de eigenschap **script** geven. We raden deze aanpak niet alle speciale tekens in het script in de JSON-document moet worden voorafgegaan en foutopsporing problemen kan veroorzaken. De beste manier is om stap #4.
5.  Een pijpleiding maken met de HDInsightHive-activiteit. De activiteit processen/transformaties de gegevens.

        {
          "name": "HiveActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                  {
                    "name": "HiveSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "HiveSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
              }
            ]
          }
        }

6.  Implementeren van de pijpleiding. Zie [Creating pijpleidingen](data-factory-create-pipelines.md) -artikel voor meer informatie. 
7.  De pijplijn met behulp van de fabriek energiebewaking en -weergaven van gegevens controleren. Zie [controleren en beheren van Data Factory pijpleidingen](data-factory-monitor-manage-pipelines.md) artikel voor meer informatie. 


## <a name="specifying-parameters-for-a-hive-script"></a>Parameters voor het script van een component geven  
In dit voorbeeld spel dagelijks worden geconsumeerd in Azure Blob-opslag en worden opgeslagen in een map is gepartitioneerd met datum en tijd. Wilt u het script component voorzien en dynamisch geeft u de locatie van de invoer tijdens de runtime en produceren ook de uitvoer gepartitioneerd met datum en tijd.

Geparametriseerde component als script wilt gebruiken, voert u de volgende

- Definieer de parameters in **definieert**.

        {
            "name": "HiveActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "HiveActivitySample",
                    "type": "HDInsightHive",
                    "inputs": [
                        {
                            "name": "HiveSampleIn"
                          }
                    ],
                    "outputs": [
                        {
                            "name": "HiveSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        }
                    }
                }
            ]
          }
        }

- In het Script component, verwijzen naar de parameter met **${hiveconf:parameterName}**. 

        DROP TABLE IF EXISTS HiveSampleIn; 
        CREATE EXTERNAL TABLE HiveSampleIn 
        (
            ProfileID   string, 
            SessionStart    string, 
            Duration    int, 
            SrcIPAddress    string, 
            GameType    string
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 
        
        DROP TABLE IF EXISTS HiveSampleOut; 
        CREATE EXTERNAL TABLE HiveSampleOut 
        (
            ProfileID   string, 
            Duration    int
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';
        
        INSERT OVERWRITE TABLE HiveSampleOut
        Select 
            ProfileID,
            SUM(Duration)
        FROM HiveSampleIn Group by ProfileID


## <a name="see-also"></a>Zie ook
- [Varken activiteit](data-factory-pig-activity.md)
- [MapReduce-activiteit](data-factory-map-reduce.md)
- [Hadoop Streaming activiteit](data-factory-hadoop-streaming-activity.md)
- [Spark programma's aanroepen](data-factory-spark.md)
- [R-scripts starten](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)










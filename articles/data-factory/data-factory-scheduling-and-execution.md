<properties
    pageTitle="Planning en uitvoering met Data Factory | Microsoft Azure"
    description="Informatie over de planning en uitvoering van aspecten van Azure Data Factory toepassingsmodel."
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
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="spelluru"/>

# <a name="data-factory-scheduling-and-execution"></a>Data Factory-planning en uitvoering
In dit artikel wordt uitgelegd dat de planning en uitvoering van aspecten van de toepassing Azure Data Factory model. 

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u de basisprincipes van Data Factory toepassing model concepten begrijpt, met inbegrip van de activiteit, pijpleidingen, gekoppelde services en datasets. Voor de basisconcepten van Azure Data Factory, Zie de volgende artikelen:

- [Data Factory-Inleiding](data-factory-introduction.md)
- [Pijpleidingen](data-factory-create-pipelines.md)
- [DataSets](data-factory-create-datasets.md) 

## <a name="schedule-an-activity"></a>Een activiteit plannen

Met de Taakplanner-sectie van de JSON-activiteit, kunt u een terugkerend schema voor een activiteit. Zo kunt u een activiteit plannen om het uur als volgt:

    "scheduler": {
        "frequency": "Hour",
        "interval": 1
    },  

![Voorbeeld van de planner](./media/data-factory-scheduling-and-execution/scheduler-example.png)

Zoals in het diagram weergegeven, maakt geven een planning voor de activiteit een reeks gewor windows. Windows gewor zijn een reeks vaste grootte, niet-overlappende, aaneengesloten tijdsintervallen. Deze logische gewor vensters voor de activiteit, *activiteit windows*worden genoemd.

Voor het venster momenteel uitgevoerde activiteit, kunt u het interval dat is gekoppeld aan het activiteitvenster met [WindowStart](data-factory-functions-variables.md#data-factory-system-variables) en [WindowEnd](data-factory-functions-variables.md#data-factory-system-variables) systeemvariabelen in JSON activiteit openen. U kunt deze variabelen voor verschillende doeleinden gebruiken in uw activiteit JSON. Zo kunt u gegevens selecteren uit de invoer en uitvoer datasets die tijd reeksgegevens.

De eigenschap **planner** ondersteunt de subeigenschappen hetzelfde als de eigenschap **beschikbaarheid** in een dataset. Zie [beschikbaarheid Dataset](data-factory-create-datasets.md#Availability) voor meer informatie. Voorbeelden: plannen op een bepaald tijdstip offset of de modus voor verwerking aan het begin of einde van het interval voor het activiteitvenster uitlijnen instellen.

U kunt **Taakplanner** eigenschappen opgeven voor een activiteit, maar deze eigenschap is **optioneel**. Als u een eigenschap opgeeft, moet overeenkomen met de cadence die u in de definitie van de dataset uitvoer opgeeft. Uitvoer dataset is momenteel wat de planning, stations, dus u een output-gegevensset maken moet zelfs als de activiteit niet er geen uitvoer geproduceerd. Als de activiteit niet elke input, kunt u het maken van de dataset invoer overslaan.

## <a name="time-series-datasets-and-data-slices"></a>Reeks gegevens en gegevenssets tijdsegmenten

Wanneer reeksgegevens is een doorlopende reeks gegevenspunten die meestal uit opeenvolgende metingen gedurende een tijdsinterval bestaat. Veelvoorkomende voorbeelden van time series data zijn sensor en telemetriegegevens van toepassing.

Met Data Factory, kunt u verwerken als reeksgegevens in een batch met de activiteit wordt uitgevoerd. Meestal is het een terugkerende cadence die invoergegevens binnenkomt en uitvoer van gegevens moet worden geproduceerd. Deze cadence is gemodelleerd door **beschikbaarheid** in de dataset als volgt:

    "availability": {
      "frequency": "Hour",
      "interval": 1
    },

Elke gegevenseenheid verbruikt en geproduceerd door het uitvoeren van een activiteit, een segment wordt genoemd. In het volgende diagram ziet u een voorbeeld van een activiteit met een dataset voor invoer en uitvoer van een dataset. Deze datasets hebben **beschikbaarheid** ingesteld op een frequentie per uur.

![Beschikbaarheid van Taakplanner](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

Het voorgaande diagram geeft de per uur segmenten gegevens voor de dataset invoer en uitvoer. Het diagram toont de drie input segmenten die gereed voor verwerking zijn. De 10-11 AM activiteit wordt uitgevoerd, het 10-11 AM output segment produceren.

Toegang tot het interval dat is gekoppeld aan het huidige segment in de dataset JSON met variabelen [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) en [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables)wordt geproduceerd.

Data Factory is momenteel vereist dat het schema dat is opgegeven in de activiteit precies overeenkomt met het schema dat in de **beschikbaarheid** van de dataset uitvoer. Daarom, **WindowStart**, **WindowEnd**, **SliceStart**en **SliceEnd** worden altijd toegewezen aan dezelfde periode en een segment met één output.

Zie voor meer informatie over andere eigenschappen die beschikbaar zijn voor de sectie beschikbaarheid, [gegevenssets maken](data-factory-create-datasets.md).

## <a name="move-data-from-sql-database-to-blob-storage"></a>Gegevens uit een SQL-Database verplaatsen naar een Blob-opslag

We dingen in actie en samen met het maken van een pijpleiding die gegevens uit een Azure SQL-Database tabel gekopieerd naar Azure Blob-opslag elk uur.

**Input: Azure SQL-Database dataset**

    {
        "name": "AzureSqlInput",
        "properties": {
            "published": false,
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


**Frequentie** is ingesteld op het **uur** en het **interval** is ingesteld op **1** in de sectie beschikbaarheid.

**Output: Azure Blob storage dataset**

    {
        "name": "AzureBlobOutput",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
                "format": {
                    "type": "TextFormat"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Frequentie** is ingesteld op het **uur** en het **interval** is ingesteld op **1** in de sectie beschikbaarheid.



**Activiteit: Kopie activiteit**

    {
        "name": "SamplePipeline",
        "properties": {
            "description": "copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "name": "AzureSQLtoBlob",
                    "description": "copy activity",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 100000,
                            "writeBatchTimeout": "00:05:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureSQLInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            ],
            "start": "2015-01-01T08:00:00Z",
            "end": "2015-01-01T11:00:00Z"
        }
    }


In het voorbeeld ziet de activiteitenplanning en dataset beschikbaarheid secties ingesteld op een frequentie per uur. In het voorbeeld ziet hoe u kunt **WindowStart** en **WindowEnd** om relevante gegevens voor een activiteit te selecteren, uitvoeren en kopieer deze naar een blob met de juiste **folderPath**. Het **mappad** met parameters als u wilt dat een aparte map voor elk uur.

Als drie segmenten tussen 8: 11 AM uitvoert, is de data in Azure SQL-Database als volgt:

![Monster input](./media/data-factory-scheduling-and-execution/sample-input-data.png)

Nadat de pijpleiding implementeert, wordt de Azure blob als volgt ingevuld:

-   2015/mypath/1/8/1/Data-bestand. &lt;Guid&gt;.txt met gegevens

            10002345,334,2,2015-01-01 08:24:00.3130000
            10002345,347,15,2015-01-01 08:24:00.6570000
            10991568,2,7,2015-01-01 08:56:34.5300000

    > [AZURE.NOTE] &lt;GUID&gt; wordt vervangen door de feitelijke guid. Voorbeeld van de naam: Data.bcde1348-7620-4f93-bb89-0eed3455890b.txt
-   2015/mypath/1/9/1/Data-bestand. &lt;Guid&gt;.txt met gegevens:

            10002345,334,1,2015-01-01 09:13:00.3900000
            24379245,569,23,2015-01-01 09:25:00.3130000
            16777799,21,115,2015-01-01 09:47:34.3130000
-   2015/mypath/1/10/1/Data-bestand. &lt;Guid&gt;.txt zonder gegevens.


## <a name="active-period-for-pipeline"></a>Actieve periode voor pijpleiding

[Pijpleidingen maken](data-factory-create-pipelines.md) het concept van een actieve periode voor een pijpleiding die is opgegeven door de eigenschappen **start** en **end** geïntroduceerd.

U kunt de begindatum voor de actieve pipeline-periode instellen in het verleden. Data Factory wordt automatisch berekend (vorige vullingen) alle segmenten van de gegevens in het verleden en ze worden verwerkt.

## <a name="parallel-processing-of-data-slices"></a>Parallelle verwerking van data-segmenten
Kunt u segmenten parallel worden uitgevoerd door de eigenschap **gelijktijdigheid** in de beleidssectie van de activiteit JSON gegevens weer gevuld. Zie voor meer informatie over deze eigenschap [pijpleidingen maken](data-factory-create-pipelines.md).

## <a name="rerun-a-failed-data-slice"></a>Een mislukte segment opnieuw uitvoeren 
Uitvoering van de segmenten kunt u in een rijke, visuele manier controleren. Zie [controleren en beheren van pijpleidingen met Azure portal blades](data-factory-monitor-manage-pipelines.md) of [app voor Monitoring en beheer](data-factory-monitor-manage-app.md) voor meer informatie.

Bekijk het volgende voorbeeld, waarin twee activiteiten. Activity1 produceert een dataset time series met segmenten als uitvoer die door de Activity2 voor de productie van de uiteindelijke uitvoer tijd reeks dataset wordt gebruikt als invoer.

![Mislukte segment](./media/data-factory-scheduling-and-execution/failed-slice.png)

Het diagram toont die uit drie segmenten recente, er is een fout opgetreden bij het produceren van het segment 9-10 uur voor Dataset2. Data Factory afhankelijkheid voor de reeks dataset tijd automatisch worden bijgehouden. Hierdoor wordt de activiteit uitvoeren voor de 9-10 uur downstream-segment niet worden gestart.

Gegevens fabriek monitoring en beheer hulpprogramma's kunt u inzoomen in de diagnostische logboeken voor het segment is mislukt om eenvoudig de oorzaak voor het probleem te vinden en op te lossen. Nadat u het probleem hebt opgelost, kunt u gemakkelijk de activiteit uitvoeren om de mislukte segment produceren starten. Zie voor meer informatie over het opnieuw uitvoeren en overgangen staat voor gegevens segmenten te begrijpen, [controleren en beheren van pijpleidingen met Azure portal blades](data-factory-monitor-manage-pipelines.md) of [controle- en app](data-factory-monitor-manage-app.md).

Nadat u het segment 9-10 uur voor de **Dataset2**opnieuw uit te voeren, begint Data Factory uitvoeren voor het afhankelijke segment 9-10 uur op de uiteindelijke dataset.

![Mislukte segment opnieuw uitvoeren](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="run-activities-in-a-sequence"></a>Uitvoeren van activiteiten in een reeks
U kunt twee activiteiten (een activiteit wordt uitgevoerd na de andere) koppelen door de dataset uitvoer van één activiteit de dataset input van de andere activiteit. De activiteiten kunnen zich in dezelfde pijpleiding of andere pijpleidingen. De tweede activiteit wordt uitgevoerd alleen als eerste is voltooid.

Neem bijvoorbeeld het volgende geval:

1.  Pijpleiding P1 heeft activiteit A1 die externe invoer DataSet-objecten nodig D1 en produceert uitvoer dataset D2.
2.  Pijpleiding P2 heeft activiteit A2 is invoer van de dataset D2 en produceert uitvoer dataset D3.

In dit scenario zijn activiteiten A1 en A2 andere pijpleidingen. De activiteit A1 wordt uitgevoerd als de externe gegevens beschikbaar zijn en de frequentie van de geplande beschikbaarheid wordt bereikt. De activiteit A2 wordt uitgevoerd wanneer de geplande segmenten van D2 beschikbaar komen en de frequentie van de geplande beschikbaarheid wordt bereikt. Als er een fout in een van de segmenten in de dataset D2, A2 voor dat segment pas uitgevoerd dit beschikbaar komt.

De weergave Netwerkdiagram eruit als het volgende diagram:

![Chaining activiteiten in twee pijpleidingen](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Zoals eerder vermeld, kunnen de activiteiten zich in dezelfde pijpleiding. De weergave Netwerkdiagram met beide activiteiten in dezelfde pijpleiding eruit als het volgende diagram:

![Activiteiten in dezelfde pijpleiding Chaining](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

### <a name="copy-sequentially"></a>Sequentieel kopiëren
Het is mogelijk om meerdere kopieerbewerkingen achter elkaar wilt uitvoeren op een wijze die opeenvolgende/besteld. Bijvoorbeeld, wellicht twee activiteiten van de kopie in een pijpleiding (CopyActivity1 en CopyActivity2) met de volgende invoer uitvoer datasets:   

CopyActivity1

Input: Dataset. Output: Dataset2.

CopyActivity2

Input: Dataset2.  Output: Dataset3.

CopyActivity2 zou worden alleen uitgevoerd als de CopyActivity1 met succes is uitgevoerd en Dataset2 beschikbaar is.

Hier wordt de pijpleiding monster JSON:

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob1ToBlob2",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset3"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob2ToBlob3",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2016-08-25T01:00:00Z",
            "end": "2016-08-25T01:00:00Z",
            "isPaused": false
        }
    }

U ziet dat de dataset output van de eerste kopie activiteit (Dataset2) in het voorbeeld wordt als invoer voor de tweede activiteit is opgegeven. Daarom de tweede activiteit wordt alleen uitgevoerd als de dataset output van de eerste activiteit gereed is.  

In het voorbeeld kan CopyActivity2 een andere invoer, zoals Dataset3, maar u Dataset2 opgeven als invoer voor CopyActivity2, zodat de activiteit niet wordt uitgevoerd totdat de CopyActivity1 is voltooid. Bijvoorbeeld:

CopyActivity1

Input: Dataset1. Output: Dataset2.

CopyActivity2

Ingangen: Dataset3, Dataset2. Output: Dataset4.

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlobToBlob",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset3"
                        },
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset4"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob3ToBlob4",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2017-04-25T01:00:00Z",
            "end": "2017-04-25T01:00:00Z",
            "isPaused": false
        }
    }


U ziet in het voorbeeld worden twee input datasets opgegeven voor de tweede kopie activiteit. Wanneer meerdere ingangen zijn opgegeven, wordt alleen de eerste invoer dataset wordt gebruikt voor het kopiëren van gegevens, maar andere datasets worden gebruikt als afhankelijkheden. CopyActivity2 kan pas beginnen nadat de volgende voorwaarden wordt voldaan:

- CopyActivity1 met succes is voltooid en de Dataset2 is beschikbaar. Deze dataset wordt niet gebruikt bij het kopiëren van gegevens naar Dataset4. Werkt uitsluitend als de afhankelijkheid van een planning voor CopyActivity2.   
- Dataset3 is beschikbaar. Deze dataset vertegenwoordigt de gegevens die worden gekopieerd naar de bestemming.  



## <a name="model-datasets-with-different-frequencies"></a>Model gegevenssets met verschillende frequenties

In de metingen zijn de frequenties voor invoer en uitvoer datasets en het venster activiteit plannen hetzelfde. Sommige scenario's moet de mogelijkheid om de uitvoer op een andere dan de frequenties van een of meer Input frequentie te produceren. Data Factory ondersteunt deze scenario's modelleren.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Voorbeeld 1: Een dagelijks rapport uitvoer voor invoergegevens die beschikbaar is om het uur produceren

Neem een scenario waarin u hebt ingevoerd meetgegevens van sensoren beschikbaar om het uur in Azure Blob-opslag. Wilt u een dagelijkse statistische rapport met statistieken, zoals gemiddelde, maximum en minimum voor de dag met [Data Factory component activiteit](data-factory-hive-activity.md)te produceren.

Hier ziet u hoe u kunt een model van dit scenario met Data Factory:

**Invoer dataset**

De invoerbestanden per uur worden neergezet in de map voor de desbetreffende dag. Beschikbaar voor invoer is ingesteld op het **uur** (frequentie: uur, interval: 1).

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Dataset-uitvoer**

Elke dag een uitvoerbestand gemaakt in de map van de dag. Beschikbaarheid van uitvoer wordt ingesteld op de **dag** (frequentie: dag en interval: 1).


    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Activiteit: activiteit in een pijpleiding component**

Het onderdeel script krijgt de juiste *datum/tijd* -gegevens als parameters die in de variabele **WindowStart** gebruiken, zoals in het volgende fragment. Deze variabele de component-script gebruikt voor het laden van de gegevens in de juiste map voor de dag en de aggregatie voor het genereren van de output uitvoeren.

        {  
            "name":"SamplePipeline",
            "properties":{  
            "start":"2015-01-01T08:00:00",
            "end":"2015-01-01T11:00:00",
            "description":"hive activity",
            "activities": [
                {
                    "name": "SampleHiveActivity",
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adftutorial\\hivequery.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                            "Month": "$$Text.Format('{0:%M}',WindowStart)",
                            "Day": "$$Text.Format('{0:%d}',WindowStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },          
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 2,
                        "timeout": "01:00:00"
                    }
                 }
             ]
           }
        }

In het volgende diagram wordt het scenario uit een oogpunt van gegevens afhankelijkheid.

![Gegevensafhankelijkheid](./media/data-factory-scheduling-and-execution/data-dependency.png)

24 uur segmenten uit een dataset invoer is afhankelijk van het segment van de uitvoer voor elke dag. Data Factory berekent de afhankelijkheden automatisch door na te gaan van de invoergegevens segmenten die in dezelfde periode als het segment uitvoer vallen te produceren. Als een van de 24 invoer segmenten niet beschikbaar is, wacht Data Factory de invoer segment klaar voordat u begint met de dagelijkse werkzaamheden uitvoeren.


### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Voorbeeld 2: Geef afhankelijkheid met expressies en functies van Data Factory

Laten we eens een ander scenario. Stel dat u hebt een component activiteit die twee datasets van invoer worden verwerkt. Een van hen heeft dagelijks nieuwe gegevens, maar een van deze nieuwe gegevens wordt elke week. Stel dat u wilt doen van een join tussen de twee ingangen en produceert uitvoer elke dag.

De eenvoudige benadering in de fabriek van welke gegevens automatisch cijfers uit het recht invoer segmenten worden verwerkt door het uitlijnen van de uitvoer van de gegevens segment tijd periode niet werkt.

U moet opgeven dat voor elke activiteit uitvoeren, de Data Factory segment van de laatste week voor de wekelijkse invoer dataset gebruiken moet. U Azure Data Factory functies gebruiken zoals in het volgende fragment wordt getoond voor de uitvoering van dit probleem.

**Input1: Azure blob**

De eerste invoer is de Azure blob wordt dagelijks bijgewerkt.

    {
      "name": "AzureBlobInputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Input2: Azure blob**

Input2 is de Azure blob wordt wekelijks bijgewerkt.

    {
      "name": "AzureBlobInputWeekly",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 7
        }
      }
    }

**Uitvoer: Azure blob**

Een uitvoerbestand wordt elke dag gemaakt in de map voor de dag. Beschikbaarheid van uitvoer wordt ingesteld op **dag** (frequentie: dag, interval: 1).

    {
      "name": "AzureBlobOutputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Activiteit: activiteit in een pijpleiding component**

De activiteit van de component neemt de twee ingangen en produceert een segment van de uitvoer van elke dag. U kunt elke dag van uitvoer segment afhankelijk van invoer delige wekelijkse invoer van de vorige week als volgt opgeven.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-01-01T08:00:00",
        "end":"2015-01-01T11:00:00",
        "description":"hive activity",
        "activities": [
          {
            "name": "SampleHiveActivity",
            "inputs": [
              {
                "name": "AzureBlobInputDaily"
              },
              {
                "name": "AzureBlobInputWeekly",
                "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
                "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutputDaily"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
              "scriptPath": "adftutorial\\hivequery.hql",
              "scriptLinkedService": "StorageLinkedService",
              "defines": {
                "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                "Month": "$$Text.Format('{0:%M}',WindowStart)",
                "Day": "$$Text.Format('{0:%d}',WindowStart)"
              }
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            },          
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 2,  
              "timeout": "01:00:00"
            }
           }
         ]
       }
    }


## <a name="data-factory-functions-and-system-variables"></a>Data Factory-functies en variabelen   

Zie [Data Factory-functies en variabelen](data-factory-functions-variables.md) voor een lijst met functies en variabelen die Data Factory ondersteunt.

## <a name="data-dependency-deep-dive"></a>Gegevens afhankelijkheid diep Kennismaking

Voor het genereren van een dataset-segment door het uitvoeren van een activiteit, wordt Data Factory de volgende *afhankelijkheid model* de relaties tussen datasets verbruikt en geproduceerd door een activiteit.

Het tijdsbereik van de invoer datasets vereist voor het genereren van het segment van de dataset uitvoer, wordt de *afhankelijkheid periode*genoemd.

Het uitvoeren van een activiteit genereert een dataset segment nadat gegevens segmenten van invoer datasets gedurende de afhankelijkheid beschikbaar zijn. Met andere woorden, uitvoeren de invoer segmenten die bestaat uit de periode afhankelijkheid in **Gereed voor de activiteit moet** voor de productie van een segment van de dataset uitvoer.

Voor het genereren van het segment dataset [**begin**, **einde**] moet een functie de dataset segment toewijzen aan de afhankelijkheid is verstreken. Deze functie is in wezen een formule die het begin en einde van het segment dataset naar het begin en einde van de periode voor de afhankelijkheid converteert. Meer formeel gesteld:

    DatasetSlice = [start, end]
    DependecyPeriod = [f(start, end), g(start, end)]

**F** en **g** , zijn functies die het begin en einde van de periode van de afhankelijkheid voor elke activiteit invoer berekenen toewijzing.

Zoals gezien in de monsters, komt de afhankelijkheid periode overeen met de periode voor het segment dat is geproduceerd. In deze gevallen berekent Data Factory automatisch de invoer segmenten die in de periode van afhankelijkheid vallen.  

In de steekproef aggregatie waar uitvoer geproduceerd dagelijks en invoergegevens elk uur beschikbaar is, is de periode segment bijvoorbeeld 24 uur. Data Factory vindt u de desbetreffende invoer per uur voor deze periode segmenten en maakt u het segment uitvoer afhankelijk van het segment van de invoer.

U kunt ook uw eigen toewijzing voor de periode van de afhankelijkheid in het monster, waarbij een van de ingangen wekelijks en de output segment is zoals dagelijks wordt geproduceerd bieden.

## <a name="data-dependency-and-validation"></a>Gegevensafhankelijkheid en validatie

Een gegevensset kan hebben een beleid voor validatie gedefinieerd die hoe de gegevens die zijn gegenereerd door de uitvoering van een segment worden gevalideerd aangeeft voordat deze gereed voor consumptie is. Zie [gegevenssets maken](data-factory-create-datasets.md) voor meer informatie.

In dergelijke gevallen nadat het segment is uitgevoerd, de output segment status wordt gewijzigd om te **wachten** met een substatus van **validatie**. Nadat de segmenten worden gevalideerd, verandert de status segment op **Gereed**.

Als een segment is geproduceerd, maar zijn niet worden gevalideerd, worden activiteiten uitgevoerd voor downstream segmenten die afhankelijk van dit segment zijn niet verwerkt.

[Controleren en beheren van pijpleidingen](data-factory-monitor-manage-pipelines.md) heeft betrekking op de verschillende segmenten van de gegevens in een Data Factory-Staten.

## <a name="external-data"></a>Externe gegevens

Een gegevensset kan worden gemarkeerd als extern (zoals in het volgende fragment van JSON), wat dat niet is gegenereerd met Data Factory. In dat geval het beleid Dataset hebben een extra set parameters voor validatie en het beleid voor de gegevensset opnieuw. Zie [pijpleidingen maken](data-factory-create-pipelines.md) voor een beschrijving van alle eigenschappen.

Net als datasets die worden geproduceerd door Data Factory, de data-segmenten voor externe gegevens moeten klaar zijn voordat de afhankelijke segmenten kunnen worden verwerkt.

    {
        "name": "AzureSqlInput",
        "properties":
        {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties":
            {
                "tableName": "MyTable"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1     
            },
            "external": true,
            "policy":
            {
                "externalData":
                {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }  
        }
    }


## <a name="onetime-pipeline"></a>Eenmalige pijpleiding
U kunt maken en plannen van een pijpleiding worden regelmatig uitgevoerd (bijvoorbeeld: elk uur of dagelijks) binnen de begin- en eindtijden die u in de definitie van de pijpleiding opgeeft. Zie [planning activiteiten](#scheduling-and-execution) voor meer informatie. Ook kunt u een pijpleiding die slechts eenmaal wordt uitgevoerd. Doet u de eigenschap **pipelineMode** instellen in de definitie van de pijpleiding **eenmalige** zoals in het volgende voorbeeld van JSON. De standaardwaarde voor deze eigenschap is **gepland**.

    {
        "name": "CopyPipeline",
        "properties": {
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": false
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ]
                    "name": "CopyActivity-0"
                }
            ]
            "pipelineMode": "OneTime"
        }
    }

Let op het volgende:

- ** **Start** - en** eindtijden voor de pijpleiding zijn niet opgegeven.
- **Beschikbaarheid** van invoer en uitvoer datasets is opgegeven (**frequentie** en **interval**), hoewel Data Factory maakt geen gebruik van de waarden.  
- Weergave Netwerkdiagram weergegeven eenmalige pijpleidingen niet. Dit gedrag is inherent aan het ontwerp.
- Eenmalige pijpleidingen kunnen niet worden bijgewerkt. U kunt een eenmalige pijpleiding klonen, wijzigt u de naam eigenschappen bijwerken en implementeren om te maken van een ander.

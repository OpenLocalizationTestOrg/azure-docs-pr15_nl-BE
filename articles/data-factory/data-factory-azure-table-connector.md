<properties 
    pageTitle="Gegevens verplaatsen in/uit tabel Azure | Microsoft Azure" 
    description="Informatie over het verplaatsen van gegevens van Azure tabelopslag met Azure Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Gegevens verplaatsen en naar Azure tabel met Azure Data Factory

In dit artikel wordt beschreven hoe u de activiteit van de kopie in een fabriek Azure gegevens kunt gebruiken om gegevens te verplaatsen van de tabel Azure van/naar een ander gegevensarchief. Dit artikel is gebaseerd op het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) een algemeen overzicht van de verplaatsing van gegevens en gegevens ondersteunde archief combinaties activiteit kopiëren biedt.

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De gemakkelijkste manier voor het maken van een pijpleiding die gegevens van Azure tabelopslag kopieert is de wizard kopiëren gebruiken. Zie [Zelfstudie: maken van een pijplijn met behulp van de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met behulp van de wizard kopiëren. 

De volgende voorbeelden geven monster JSON definities die u gebruiken kunt voor het maken van een pijplijn met [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Deze weergegeven gegevens kopiëren en naar Azure tabelopslag en Azure Blob Database. Gegevens kunnen wel gekopieerde **rechtstreeks** vanuit een van de bronnen naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.

## <a name="sample-copy-data-from-azure-table-to-azure-blob"></a>Voorbeeld: Gegevens kopieert uit de tabel Azure Azure Blob

In het volgende voorbeeld ziet:

1.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (gebruikt voor blob & tabel).
2.  Een invoer [dataset](data-factory-create-datasets.md) van het type [AzureTable](#azure-table-dataset-type-properties).
3.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
3.  De [pijplijn](data-factory-create-pipelines.md) met kopie activiteit die gebruikmaakt van [AzureTableSource](#azure-table-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

De steekproef die deel uitmaken van de standaardpartitie in een tabel een blob Azure elk uur gegevens opgehaald. De JSON-eigenschappen in deze voorbeelden gebruikt worden in voorbeelden van de volgende secties beschreven.

**Azure storage service gekoppeld:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure Data Factory ondersteunt twee soorten Azure gekoppeld opslagservices: **AzureStorage** en **AzureStorageSas**. U de verbindingsreeks die de toets met het account opgeven voor de eerste, en voor de latere, geeft u de Uri gedeelde toegang handtekening (SAS). Zie de sectie [Gekoppelde Services](#linked-services) voor meer informatie.  

**Azure tabel invoer dataset:**

In het voorbeeld wordt ervan uitgegaan dat u een tabel "MyTable" in Azure tabel hebt gemaakt.
 
Instelling 'externe': 'true' informeert de Data Factory-service dat de dataset buiten de fabriek gegevens is en niet wordt geproduceerd door een activiteit in de fabriek van gegevens.

    {
      "name": "AzureTableInput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
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

**Azure Blob uitvoer dataset:**

Gegevens worden weggeschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het pad naar de map wordt gebruikt voor jaar, maand, dag en uur delen van de begintijd. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Pipeline-activiteit van de kopie:**

De pijplijn bevat een kopie activiteit die is geconfigureerd voor het gebruik van de invoer en uitvoer datasets en elk uur is gepland. **Het type** is ingesteld op **AzureTableSource** in de pijplijn JSON definitie en **sink** -type is ingesteld op **BlobSink**. De SQL-query die is opgegeven met de eigenschap **AzureTableSourceQuery** selecteert de gegevens uit de standaardpartitie elk uur te kopiëren.

    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline for copy activity",
            "activities":[  
                {
                    "name": "AzureTabletoBlob",
                    "description": "copy activity",
                    "type": "Copy",
                    "inputs": [
                        {
                            "name": "AzureTableInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "AzureTableSource",
                            "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },              
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 0,
                        "timeout": "01:00:00"
                    }
                }
             ]  
        }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-table"></a>Voorbeeld: Gegevens kopiëren vanuit Azure Blob Azure tabel

In het volgende voorbeeld ziet:

1.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (gebruikt voor blob & tabel)
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureTable](#azure-table-dataset-type-properties). 
4.  De [pijplijn](data-factory-create-pipelines.md) met kopie activiteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) en [AzureTableSink](#azure-table-copy-activity-type-properties). 


Het monster kopieën tijdreeks gegevens uit een Azure blob-naar een Azure tabel per uur. De JSON-eigenschappen in deze voorbeelden gebruikt worden in voorbeelden van de volgende secties beschreven.

**Azure opslag (voor Azure tabel & Blob) gekoppeld service:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure Data Factory ondersteunt twee soorten Azure gekoppeld opslagservices: **AzureStorage** en **AzureStorageSas**. U de verbindingsreeks die de toets met het account opgeven voor de eerste, en voor de latere, geeft u de Uri gedeelde toegang handtekening (SAS). Zie de sectie [Gekoppelde Services](#linked-services) voor meer informatie. 

**Azure Blob invoer dataset:**

Gegevens wordt opgehaald uit een nieuwe blob elk uur (frequentie: uur, interval: 1). Naam van het pad en de bestandsnaam voor de blob dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het pad naar de map gebruikt, jaar, maand en dag deel uitmaken van de begintijd en bestandsnaam gebruikt de uur-deel van de begintijd. 'externe': 'true' instelling informeert de Data Factory service dat de dataset buiten de fabriek gegevens is en niet wordt geproduceerd door een activiteit in de fabriek van de gegevens.
    
    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
          "fileName": "{Hour}.csv",
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
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
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

**Azure tabel uitvoer dataset:**

In het voorbeeld worden de gegevens gekopieerd naar een tabel met de naam "MyTable in de tabel Azure. Een Azure tabel maken met hetzelfde aantal kolommen als u verwacht dat de Blob-CSV-bestand bevat. Nieuwe rijen worden toegevoegd aan de tabel elk uur. 

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Pipeline-activiteit van de kopie:**

De pijplijn bevat een kopie activiteit die is geconfigureerd voor het gebruik van de invoer en uitvoer datasets en elk uur is gepland. **Het type** is ingesteld op **BlobSource** in de pijplijn JSON definitie en **sink** -type is ingesteld op **AzureTableSink**. 


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureTableOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "AzureTableSink",
                "writeBatchSize": 100,
                "writeBatchTimeout": "01:00:00"
              }
            },
            "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },                      
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }

## <a name="linked-services"></a>Gekoppelde Services
Er zijn twee soorten gekoppelde services die kunt u een Azure blobopslag koppelen aan een Azure data factory genoemd. Ze zijn: **AzureStorage** en **AzureStorageSas** gekoppeld gekoppeld. De fabriek gegevens met wereldwijde toegang tot biedt de Azure opslag gekoppeld-service voor de opslag van Azure. Overwegende dat de Azure opslag SAS (gedeelde toegang handtekening) gekoppeld biedt service de fabriek gegevens toegang beperkt/tijd-gebonden voor de opslag van Azure. Er zijn geen verschillen tussen deze twee gekoppelde services. Kies de gekoppelde service die aan uw behoeften voldoet. In de volgende secties bevatten meer informatie over deze twee gekoppelde services.

[AZURE.INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="azure-table-dataset-type-properties"></a>Eigenschappen van tabel Dataset Azure

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van datasets. Secties zoals structuur, beschikbaarheid en het beleid van een dataset JSON zijn vergelijkbaar voor alle typen van dataset (Azure SQL Azure blob, Azure tabel, enz.).

De sectie typeProperties verschilt voor elk type dataset en vindt u informatie over de locatie van de gegevens in het gegevensarchief. De sectie **typeProperties** voor de gegevensset van het type **AzureTable** heeft de volgende eigenschappen.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| Tabelnaam | De naam van de tabel in de Azure tabel instantie die gekoppelde service naar verwijst. | Ja. Wanneer u een tabelnaam is opgegeven zonder een azureTableSourceQuery, worden alle records uit de tabel gekopieerd naar de bestemming. Als ook een azureTableSourceQuery is opgegeven, worden records uit de tabel die voldoet aan de query worden gekopieerd naar de bestemming. |

### <a name="schema-by-data-factory"></a>Schema door Data Factory
Bij winkels zoals Azure tabel gegevens schema afleidt de Data Factory service het schema in een van de volgende manieren:

1.  Als u de structuur van gegevens met behulp van de eigenschap **structuur** in de definitie van de dataset, respecteert de Data Factory-service deze structuur als het schema. In dit geval als een rij geen waarde voor een kolom is een null-waarde bepaald.
2. Als u de structuur van gegevens met behulp van de eigenschap **structuur** in de definitie van de dataset, afleidt Data Factory het schema met behulp van de eerste rij in de gegevens. Als de eerste rij geen het volledige schema bevat, worden in dit geval sommige kolommen in het resultaat van de kopieeropdracht gemist.

Daarom voor schema-vrij-gegevensbronnen, de beste manier is om op te geven van de structuur van gegevens met behulp van de eigenschap **structuur** .

## <a name="azure-table-copy-activity-type-properties"></a>Eigenschappen van tabel kopiëren activiteit Azure

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen van de naam, beschrijving, invoer en uitvoer datasets en beleidsregels zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren aan de andere kant met elk activiteitstype. Activiteit kopiëren verschillen ze afhankelijk van de typen bronnen en putten.

**AzureTableSource** ondersteunt de volgende eigenschappen in de sectie typeProperties:

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | -------- 
azureTableSourceQuery | De aangepaste query gebruiken om gegevens te lezen. | Azure tabel query-tekenreeks. Zie de voorbeelden in de volgende sectie. | Nr. Wanneer u een tabelnaam is opgegeven zonder een azureTableSourceQuery, worden alle records uit de tabel gekopieerd naar de bestemming. Als ook een azureTableSourceQuery is opgegeven, worden records uit de tabel die voldoet aan de query worden gekopieerd naar de bestemming.
azureTableSourceIgnoreTableNotFound | Geef aan of de uitzondering van tabel slikken bestaat niet. | TRUE<br/>FALSE | Nee |

### <a name="azuretablesourcequery-examples"></a>Voorbeelden van azureTableSourceQuery

Als Azure kolom van het tekenreekstype: 

    azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"

Als Azure kolom van het type datum/tijd: 

    "azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"


**AzureTableSink** ondersteunt de volgende eigenschappen in de sectie typeProperties:


Eigenschap | Beschrijving | Toegestane waarden | Vereist  
-------- | ----------- | -------------- | -------- 
azureTableDefaultPartitionKeyValue | Standaard partitie sleutelwaarde die kan worden gebruikt door de sink. | Een string-waarde. | Nee 
azureTablePartitionKeyName | Geef de naam van de kolom waarvan de waarden worden gebruikt als de partitiesleutels. Als u niet opgeeft, wordt AzureTableDefaultPartitionKeyValue als de partitiesleutel gebruikt. | De naam van een kolom. | Nee |
azureTableRowKeyName | Geef de naam van de kolom waarvan u de kolomwaarden worden gebruikt als rijsleutel. Als u niet opgeeft, wordt een GUID gebruiken voor elke rij. | De naam van een kolom. | Nee  
azureTableInsertType | De modus data in Azure tabel invoegen.<br/><br/>Deze eigenschap bepaalt of bestaande rijen in de uitvoertabel met overeenkomende rij- en partitie-toetsen hebben hun waarden vervangen of samengevoegd. <br/><br/>Meer informatie over deze instellingen (samenvoegen en vervangen), Zie [invoegen of entiteit samenvoegen](https://msdn.microsoft.com/library/azure/hh452241.aspx) en [entiteit vervangen of invoegen](https://msdn.microsoft.com/library/azure/hh452242.aspx) . <br/><br> Deze instelling is van toepassing op het rijniveau van, niet de tabelniveau, en geen van beide optie verwijdert u de rijen die niet bestaan in de invoer in de uitvoertabel. | samenvoegen (standaard)<br/>vervangen | Nee 
writeBatchSize | Voegt de gegevens in de tabel met Azure als de writeBatchSize of writeBatchTimeout is geraakt. | Geheel getal (aantal rijen)| Nee (standaard: 10000) 
writeBatchTimeout | Voegt de gegevens in de tabel met Azure als de writeBatchSize of writeBatchTimeout is geraakt. | TimeSpan<br/><br/>Voorbeeld: "00: 20:00" (20 minuten) | Nee (standaard opslag client standaard time-out waarde 90 sec.)

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Een bronkolom toewijzen aan een doelkolom JSON-eigenschap van de vertaler te gebruiken voordat u de doelkolom als de azureTablePartitionKeyName gebruiken kunt.

In het volgende voorbeeld wordt de bronkolom DivisionID is toegewezen aan de doelkolom: DivisionID.  

    "translator": {
        "type": "TabularTranslator",
        "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
    } 

De DivisionID wordt opgegeven als de partitiesleutel. 

    "sink": {
        "type": "AzureTableSink",
        "azureTablePartitionKeyName": "DivisionID",
        "writeBatchSize": 100,
        "writeBatchTimeout": "01:00:00"
    }


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-azure-table"></a>Toewijzing van het type tabel Azure

Zoals vermeld in het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) , voert de activiteit kopie automatische conversie van brontypen typen met de volgende twee stappen benadering opvangen.

1. Converteren van native brontypen naar .NET-type
2. Van het type .NET converteren naar native sink-type

Bij het verplaatsen van gegevens & Azure tabel, worden de volgende [toewijzingen die zijn gedefinieerd door de service Azure tabel](https://msdn.microsoft.com/library/azure/dd179338.aspx) uit Azure tabel OData typen als het type .NET en omgekeerd gebruikt. 

| OData-gegevenstype | .NET-type | Details |
| --------------- | --------- | ------- |
| Edm.Binary | byte] | Een matrix van bytes tot 64 KB. |
| Edm.Boolean | BOOL | Een Booleaanse waarde. |
| Edm.DateTime | Datum/tijd | Een 64-bits waarde uitgedrukt als Coordinated Universal Time (UTC). De ondersteunde DateTime bereik begint vanaf 1 januari 1601 A.D. 12:00 uur (C. E.), UTC. Het bereik eindigt op 31 December 9999. |
| Edm.Double | dubbel | Een 64-bits drijvende-kommawaarde. |
| Edm.Guid | GUID | Een 128-bits globally unique identifier. |
| Edm.Int32 | Int32 of int | Een 32-bits geheel getal. |
| Edm.Int64 | Int64 of long | Een 64-bits geheel getal. |
| Edm.String | Tekenreeks | Een waarde van UTF-16-codering. String-waarden mogelijk tot 64 KB. |

### <a name="type-conversion-sample"></a>Type conversie monster

In het volgende voorbeeld wordt voor het kopiëren van gegevens uit een Azure Blob Azure tabel met de conversie. 

Stel dat u de dataset Blob in CSV-indeling en bevat drie kolommen. Een van hen is een datum/tijd-kolom met een aangepaste datum/tijd-notatie met Franse afkortingen voor de dag van de week. 

De dataset Blob bron als volgt samen met de definities voor de kolommen definiëren.
    
    {
        "name": " AzureBlobInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1,
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

Uitgaande van de toewijzing van Azure tabel OData-type type .NET, zou u de tabel in Azure tabel definiëren met het volgende schema. 

**Azure tabelschema:**

Naam van de kolom | Type
----------- | --------
gebruikers-id | Edm.Int64
naam | Edm.String 
lastlogindate | Edm.DateTime

Definieer vervolgens de dataset Azure tabel als volgt. U hoeft niet "structuur" sectie met de type-informatie opgeven omdat de type-informatie al in de onderliggende gegevensopslag opgegeven is.

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

In dit geval Data Factory automatisch van het type conversie met inbegrip van het datum/tijd-veld met de aangepaste datum/tijd-indeling met behulp van de "fr-fr" cultuur bij het verplaatsen van gegevens uit Blob Azure tabel.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Prestatie en afstemming  
Zie meer informatie over de belangrijkste factoren die prestaties gevolgen van verplaatsing van gegevens (kopiëren activiteit) in Azure Data Factory en verschillende manieren optimaliseren, [uitvoering van de activiteit & Tuning Guide](data-factory-copy-activity-performance.md).








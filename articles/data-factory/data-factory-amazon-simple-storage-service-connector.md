<properties 
    pageTitle="Gegevens verplaatsen van Amazon Simple Storage Service met behulp van Data Factory | Microsoft Azure" 
    description="Meer informatie over het verplaatsen van gegevens van Amazon Simple Storage Service (S3 haalt) met behulp van Azure Data Factory." 
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
    ms.date="10/24/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Gegevens van Amazon Simple Storage-Service via Azure Data Factory

In dit artikel wordt beschreven hoe u de activiteit kopiëren in een fabriek Azure gegevens kunt gebruiken om gegevens op te slaan van Amazon Simple Storage-Service (S3) naar een andere gegevens te verplaatsen. Dit artikel is gebaseerd op het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) die kopie activiteit een algemeen overzicht van de verplaatsing van gegevens en een lijst met ondersteunde bron/sink-gegevensarchieven biedt.  

Data factory ondersteunt momenteel alleen verplaatst gegevens van Amazon S3 naar andere opgeslagen gegevens, maar niet voor het verplaatsen van gegevens uit andere opgeslagen gegevens naar Amazon S3.

## <a name="required-permissions"></a>Vereiste machtigingen

Om gegevens te kopiëren van Amazon S3, moet dat u onderstaande machtigingen zijn toegekend:

- **S3:GetObject** en **s3:GetObjectVersion** voor Amazon S3 Object bewerkingen
- **S3:ListBucket** en **s3:ListAllMyBuckets** (gebruikt in de wizard voor kopiëren alleen) voor Amazon S3 Bucket bewerkingen 

U vindt de volledige lijst van Amazon S3 machtigingen met details van [Machtigingen op te geven in een beleid](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De gemakkelijkste manier voor het maken van een pijpleiding die gegevens van Amazon S3 kopieert is de wizard kopiëren gebruiken. Zie [Zelfstudie: maken van een pijplijn met behulp van de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met behulp van de wizard kopiëren. 

Het volgende voorbeeld wordt een monster JSON definities die u gebruiken kunt voor het maken van een pijplijn met [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). U ziet hoe u gegevens kopiëren van Amazon S3 naar Azure Blob-opslag. Gegevens kunnen echter worden gekopieerd naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-s3-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren van Amazon S3 Azure Blob
In dit voorbeeld ziet u hoe gegevens van een Amazon S3 kopiëren naar een Azure Blob-opslag. Gegevens kunnen wel gekopieerde **rechtstreeks** naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.  
 
Het monster heeft de volgende data factory entiteiten:

- Een gekoppelde service van het type [AwsAccessKey](#linked-service-properties).
- Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Een invoer [dataset](data-factory-create-datasets.md) van het type [AmazonS3](#dataset-type-properties).
- Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Een [pijpleiding](data-factory-create-pipelines.md) met kopie activiteit die gebruikmaakt van [FileSystemSource](#copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Het monster kopieert gegevens van Amazon S3 op een Azure blob elk uur. De JSON-eigenschappen in deze voorbeelden gebruikt worden in voorbeelden van de volgende secties beschreven. 

**Amazon S3 gekoppeld service**

    {
        "name": "AmazonS3LinkedService",
        "properties": {
            "type": "AwsAccessKey",
            "typeProperties": {
                "accessKeyId": "<access key id>",
                "secretAccessKey": "<secret access key>"
            }
        }
    }

**Azure gekoppeld opslagservice**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Amazon S3 invoer dataset**

Het instellen van **'externe': true** de Data Factory service meldt dat de dataset buiten de fabriek gegevens is en niet wordt geproduceerd door een activiteit in de fabriek van gegevens. Deze eigenschap instellen op true voor een input-gegevensset die niet wordt geproduceerd door een activiteit in de pijplijn.

    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }



**Azure Blob uitvoer dataset**

Gegevens worden weggeschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het pad naar de map wordt gebruikt voor jaar, maand, dag en uur delen van de begintijd.

    {
        "name": "AzureBlobOutputDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
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
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Pipeline-exemplaar activiteit**

De pijplijn bevat een kopie activiteit die is geconfigureerd voor het gebruik van de invoer en uitvoer datasets en elk uur is gepland. **Het type** is ingesteld op **FileSystemSource** in de pijplijn JSON definitie en **sink** -type is ingesteld op **BlobSink**. 
    
    {
        "name": "CopyAmazonS3ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "FileSystemSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonS3InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutputDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "AmazonS3ToBlob"
                }
            ],
            "start": "2014-08-08T18:00:00Z",
            "end": "2014-08-08T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Eigenschappen van gekoppelde service

De volgende tabel bevat een beschrijving van specifieke JSON-elementen aan Amazon S3 (**AwsAccessKey**) service gekoppeld.

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------- | ------- |  
| accessKeyID | De ID van de van geheime toegangssleutel. | tekenreeks | Ja |
| secretAccessKey | De van geheime toegangssleutel zelf. | Gecodeerde geheime tekenreeks | Ja | 


## <a name="dataset-type-properties"></a>DataSet-eigenschappen

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van datasets. Secties zoals structuur, beschikbaarheid en beleid zijn vergelijkbaar voor alle typen van dataset (Azure SQL Azure blob, Azure tabel, enz.).

De sectie **typeProperties** verschilt voor elk type dataset en vindt u informatie over de locatie van de gegevens in het gegevensarchief. De sectie typeProperties voor dataset van het type **AmazonS3** (waaronder Amazon S3 dataset) heeft de volgende eigenschappen

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------- | ------ | 
| bucketName | De naam van de S3 bucket. | Tekenreeks | Ja |
| sleutel | De sleutel van de S3-object. | Tekenreeks | Nee | 
| voorvoegsel | Het voorvoegsel voor de S3-object. Objecten waarvan sleutels met dit voorvoegsel beginnen geselecteerd. Geldt alleen als sleutel is leeg. | Tekenreeks | Nee | 
| Versie | De versie van de S3-object als de S3 versiebeheer is ingeschakeld. | Tekenreeks | Nee |  
| indeling | De volgende notatietypen worden ondersteund: **tekstopmaak**, **AvroFormat**, **JsonFormat**, **OrcFormat**, **ParquetFormat**. Stel de eigenschap **type** onder notatie op een van deze waarden. Zie [Tekstopmaak opgeven](#specifying-textformat), [AvroFormat geven](#specifying-avroformat) [JsonFormat geven](#specifying-jsonformat), [Geven OrcFormat](#specifying-orcformat)en [ParquetFormat geven](#specifying-parquetformat) secties voor meer informatie. Als u bestanden wilt kopiëren als-is tussen bestand gebaseerde winkels (binaire kopie), kunt u de opmaaksectie in beide definities van de invoer en uitvoer dataset overslaan.| Nee
| compressie | Geef het type en de mate van compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, en **BZip2** en ondersteunde niveaus zijn: **optimale** en **snelst**. De compressie-instellingen worden momenteel niet ondersteund voor gegevens in **AvroFormat** of **OrcFormat**. Voor meer informatie, Zie de sectie [compressie-ondersteuning](#compression-support) .  | Nee |

> [AZURE.NOTE] Hiermee geeft u de locatie van het object S3 bucket is de basiscontainer voor S3 objecten waarbij sleutel is het volledige pad naar object S3 bucketName + toets.

### <a name="sample-dataset-with-prefix"></a>Monster dataset met voorvoegsel

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "prefix": "testFolder/test",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }

### <a name="sample-data-set-with-version"></a>Verzameling voorbeeldgegevens (met versie)

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "version": "WBeMIxQkJczm0CJajYkHf0_k6LhBmkcL",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


### <a name="dynamic-paths-for-s3"></a>Dynamische paden voor S3

In het voorbeeld gebruiken we vaste waarden voor sleutel- en bucketName-eigenschappen in de dataset Amazon S3. 

    "key": "testFolder/test.orc",
    "bucketName": "testbucket",

U kunt Data Factory de sleutel en het bucketName tijdens runtime dynamisch berekend met behulp van variabelen zoals SliceStart hebben.

    "key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
    "bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"

U kunt voor de eigenschap prefix van een dataset Amazon S3 hetzelfde doen. Zie [Data Factory-functies en variabelen](data-factory-functions-variables.md) voor een lijst met ondersteunde functies en variabelen. 


[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## <a name="copy-activity-type-properties"></a>Activiteit eigenschappen kopiëren

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen van de naam, beschrijving, input en output-tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie **typeProperties** van de activiteit variëren aan de andere kant met elk activiteitstype. Activiteit kopiëren verschillen ze afhankelijk van de typen bronnen en putten.

Als de bron van de activiteit van de kopie van het type **FileSystemSource** (waaronder Amazon S3), is de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------------- | -------- | 
| recursieve | Hiermee geeft u aan of S3 naar recursief lijst objecten in de directory. | waar/onwaar | Nee | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Prestatie en afstemming  
Zie de [uitvoering van de activiteit & Tuning Guide](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties gevolgen van verplaatsing van gegevens (kopiëren activiteit) in Azure Data Factory en verschillende manieren optimaliseren.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende artikelen: 
- [Zelfstudie kopiëren activiteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een activiteit kopiëren. 

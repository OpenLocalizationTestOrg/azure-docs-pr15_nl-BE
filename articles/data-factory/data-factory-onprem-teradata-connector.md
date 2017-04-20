<properties 
    pageTitle="Verplaatsen van gegevens uit de Teradata | Azure Data Factory" 
    description="Meer informatie over Teradata-Connector voor de Data Factory service waarmee u gegevens uit de Teradata-Database verplaatsen" 
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
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-teradata-using-azure-data-factory"></a>Gegevens verplaatsen van Teradata met Azure Data Factory

In dit artikel wordt beschreven hoe u de activiteit kopiëren in een fabriek Azure gegevens kunt gebruiken om gegevens op te slaan van Teradata met een andere gegevens te verplaatsen. Dit artikel is gebaseerd op het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) activiteit kopiëren en gegevens ondersteunde archief combinaties een algemeen overzicht van de verplaatsing van gegevens biedt.

Data factory ondersteunt verbindingen met voor bedrijfsruimten Teradata bronnen via de Gateway Data Management. Zie artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies over het instellen van de gateway [verplaatst gegevens tussen de locaties op gebouwen en wolken](data-factory-move-data-between-onprem-and-cloud.md) . 

> [AZURE.NOTE]
Gateway is vereist, zelfs als de Teradata wordt gehost in een IaaS Azure VM. U kunt de gateway installeren op dezelfde IaaS VM als gegevensarchief of een andere VM als de gateway verbinding met de database maken kan. 

Data factory ondersteunt alleen verplaatst gegevens van Teradata naar andere opgeslagen gegevens, niet vanuit andere opgeslagen gegevens naar Teradata.

## <a name="installation"></a>Installatie 

Voor Data Management Gateway verbinding maken met de Teradata-Database, moet u de [.NET Data Provider voor Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) op hetzelfde systeem als de Data Management Gateway hebt geïnstalleerd.

> [AZURE.NOTE] Zie [problemen oplossen gateway](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor tips over het oplossen van verbinding/gateway verband houdende vraagstukken. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om een pijpleiding die gegevens uit de Teradata worden gekopieerd naar een van de ondersteunde sink gegevensarchieven maken is de wizard kopiëren gebruiken. Zie [Zelfstudie: maken van een pijplijn met behulp van de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met behulp van de wizard kopiëren. 

Het volgende voorbeeld wordt een monster JSON definities die u gebruiken kunt voor het maken van een pijplijn met [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe gegevens van Teradata kopiëren naar Azure Blob-opslag. Gegevens kunnen echter worden gekopieerd naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.   

### <a name="sample-copy-data-from-teradata-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren vanuit Teradata Azure Blob

In dit voorbeeld ziet u hoe gegevens uit een database van Teradata kopiëren naar een Azure Blob-opslag. Gegevens kunnen wel gekopieerde **rechtstreeks** naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.  
 
Het monster heeft de volgende data factory entiteiten:

1.  Een gekoppelde service van het type [OnPremisesTeradata](data-factory-onprem-teradata-connector.md#teradata-linked-service-properties).
2.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [RelationalTable](data-factory-onprem-teradata-connector.md#teradata-dataset-type-properties).
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
4.  De [pijplijn](data-factory-create-pipelines.md) met kopie activiteit die gebruikmaakt van [RelationalSource](data-factory-onprem-teradata-connector.md#teradata-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Het monster kopieert gegevens van resultaten van een query in Teradata-database naar een blob elk uur. De JSON-eigenschappen in deze voorbeelden gebruikt worden in voorbeelden van de volgende secties beschreven. 

Instellen als eerste stap de data management gateway. De instructies zijn in het artikel [verplaatst gegevens tussen de locaties op gebouwen en wolken](data-factory-move-data-between-onprem-and-cloud.md) .

**Gekoppeld Teradata-service:**

    {
        "name": "OnPremTeradataLinkedService",
        "properties": {
            "type": "OnPremisesTeradata",
            "typeProperties": {
                "server": "<server>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }

**Azure Blob storage service gekoppeld:**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorageLinkedService",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
            }
        }
    }


**Teradata invoer dataset:**

In het voorbeeld wordt ervan uitgegaan dat u een tabel hebt gemaakt "MyTable" in Teradata en een kolom met de naam 'tijdstempel' voor een reeks tijdgegevens bevat.

'Externe' instellen: waar de Data Factory service meldt dat de tabel de fabriek gegevens buiten en niet wordt geproduceerd door een activiteit in de fabriek van gegevens.

    {
        "name": "TeradataDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremTeradataLinkedService",
            "typeProperties": {
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
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
        "name": "AzureBlobTeradataDataSet",
        "properties": {
            "published": false,
            "location": {
                "type": "AzureBlobLocation",
                "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                ],
                "linkedServiceName": "AzureStorageLinkedService"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Pipeline-exemplaar activiteit:**

De pijplijn bevat een kopie activiteit die is geconfigureerd voor het gebruik van de invoer en uitvoer datasets en elk uur wordt uitgevoerd. **Het type** is ingesteld op **RelationalSource** in de pijplijn JSON definitie en **sink** -type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de eigenschap **query** selecteert de gegevens in het afgelopen uur te kopiëren.

    {
        "name": "CopyTeradataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "TeradataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobTeradataDataSet"
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
                    "name": "TeradataToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z",
            "isPaused": false
        }
    }


## <a name="teradata-linked-service-properties"></a>Eigenschappen van gekoppelde Teradata-service

De volgende tabel bevat een beschrijving van JSON-elementen die specifiek zijn voor de service Teradata gekoppeld. 

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
type | De eigenschap type moet worden ingesteld op: **OnPremisesTeradata** | Ja
Server | De naam van de server Teradata. | Ja
authenticationType | Type verificatie dat wordt gebruikt om verbinding met de Teradata-database. Mogelijke waarden zijn: anoniem, basisverificatie en Windows. | Ja
gebruikersnaam | Geef de gebruikersnaam als u basisverificatie of Windows-verificatie gebruikt. | Nee 
wachtwoord | Geef het wachtwoord voor de gebruikersaccount die u hebt opgegeven voor de gebruikersnaam. | Nee 
gatewayName | De naam van de gateway die de Data Factory-service gebruiken moet om verbinding met de database op gebouwen Teradata. | Ja

Zie [referenties voor instelling en beveiliging](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) voor meer informatie over het instellen van referenties voor een gegevensbron op gebouwen Teradata.

## <a name="teradata-dataset-type-properties"></a>Eigenschappen van dataset Teradata

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van datasets. Secties zoals structuur, beschikbaarheid en het beleid van een dataset JSON zijn vergelijkbaar voor alle typen van dataset (Azure SQL Azure blob, Azure tabel, enz.).

De sectie **typeProperties** verschilt voor elk type dataset en vindt u informatie over de locatie van de gegevens in het gegevensarchief. Er zijn momenteel geen eigenschappen voor de dataset Teradata ondersteund. 


## <a name="teradata-copy-activity-type-properties"></a>Teradata eigenschappen activiteit kopiëren

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen van de naam, beschrijving, input en output-tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren aan de andere kant met elk activiteitstype. Activiteit kopiëren verschillen ze afhankelijk van de typen bronnen en putten.

Wanneer de bron van het type **RelationalSource** (waaronder Teradata) is de volgende eigenschappen zijn beschikbaar in de sectie **typeProperties** :

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
query | De aangepaste query gebruiken om gegevens te lezen. | SQL-queryreeks. Bijvoorbeeld: Selecteer * from MijnTabel. | Ja

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-teradata"></a>Toewijzing van het type voor Teradata

Zoals vermeld in het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) , voert de activiteit kopie automatische conversie van brontypen opvangen typen met de volgende 2-stap benadering:

1. Converteren van native brontypen naar .NET-type
2. Van het type .NET converteren naar native sink-type

Bij het verplaatsen van gegevens naar Teradata, worden de volgende toewijzingen van Teradata type type .NET gebruikt.

Type Teradata-Database | .NET framework-type
----------------- | ---------------------------
CHAR | Tekenreeks
CLOB | Tekenreeks
Afbeelding | Tekenreeks
VarChar | Tekenreeks
VarGraphic | Tekenreeks
BLOB | Byte]
Byte | Byte]
VarByte | Byte]
BigInt | Int64
ByteInt | Int16
Decimaal | Decimaal
Getal met dubbele precisie | Getal met dubbele precisie
Geheel getal | Int32
Nummer | Getal met dubbele precisie
SmallInt | Int16
Datum | Datum/tijd
Tijd | TimeSpan
Tijd met Time Zone | Tekenreeks
Tijdstempel | Datum/tijd
Tijdstempel met Time Zone | DateTimeOffset
Interval-dag | TimeSpan
Interval dag uur | TimeSpan
Interval-dag op minuut | TimeSpan
Interval voor tweede dag | TimeSpan
Interval-uur | TimeSpan
Interval uur minuut | TimeSpan
Interval voor tweede uur | TimeSpan
Interval minuut | TimeSpan
Minuut interval voor tweede | TimeSpan
Interval voor tweede | TimeSpan
Interval-jaar | Tekenreeks
Jaar maand interval | Tekenreeks
Maand interval | Tekenreeks
Period(date) | Tekenreeks
Period(Time) | Tekenreeks
Periode (tijd met Time Zone) | Tekenreeks
Period(timestamp) | Tekenreeks
Periode (tijdstempel met Time Zone) | Tekenreeks
XML | Tekenreeks

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Prestatie en afstemming  
Zie de [uitvoering van de activiteit & Tuning Guide](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties gevolgen van verplaatsing van gegevens (kopiëren activiteit) in Azure Data Factory en verschillende manieren optimaliseren.
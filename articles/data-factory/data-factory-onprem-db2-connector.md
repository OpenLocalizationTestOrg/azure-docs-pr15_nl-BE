<properties 
    pageTitle="Gegevens verplaatsen van DB2 | Azure Data Factory" 
    description="Meer informatie over het verplaatsen van gegevens uit de DB2-Database met behulp van Azure Data Factory" 
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
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-db2-using-azure-data-factory"></a>Verplaatsen van gegevens uit de DB2 met Azure Data Factory
In dit artikel wordt beschreven hoe u kunt de kopie activiteit in een fabriek Azure gegevens verplaatsen van gegevens voor het opslaan van DB2 naar een andere data. Dit artikel is gebaseerd op het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) activiteit kopiëren en gegevens ondersteunde archief combinaties een algemeen overzicht van de verplaatsing van gegevens biedt.

Data factory ondersteunt verbindingen met voor bedrijfsruimten DB2 bronnen met behulp van de Data Management Gateway. Zie artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies over het instellen van de gateway [verplaatst gegevens tussen de locaties op gebouwen en wolken](data-factory-move-data-between-onprem-and-cloud.md) . 

> [AZURE.NOTE]
Met de gateway verbinding maken met DB2, zelfs als deze wordt gehost in Azure IaaS VMs. Als u probeert verbinding te maken met een exemplaar van DB2 gehost in de wolk, kunt u ook de gateway-instantie in IaaS VM installeren.

Data factory ondersteunt momenteel alleen verplaatst gegevens van DB2 naar andere opgeslagen gegevens, niet vanuit andere opgeslagen gegevens naar de DB2. 

## <a name="installation"></a>Installatie 

De Data Management Gateway bevat een ingebouwd stuurprogramma voor DB2 die de volgende ondersteunt: 

- SQLAM 9 / 10 / 11
- DB2 voor LUW (Linux, Unix, Windows)
- DB2 z/OS en DB2 voor ik (ook bekend als / 400)

Daarom moet u niet meer handmatig de stuurprogramma's installeren als u de gegevens in de DB2.

> [AZURE.NOTE] Zie [problemen oplossen gateway](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor tips over het oplossen van verbinding/gateway verband houdende vraagstukken. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De gemakkelijkste manier voor het maken van een pijpleiding die gegevens opgehaald uit een DB2-Database is de wizard kopiëren gebruiken. Zie [Zelfstudie: maken van een pijplijn met behulp van de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met behulp van de wizard kopiëren. 

De volgende voorbeelden geven monster JSON definities die u gebruiken kunt voor het maken van een pijplijn met [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe gegevens kopiëren uit een DB2-database en Azure Blob-opslag. Gegevens kunnen echter worden gekopieerd naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.

## <a name="sample-copy-data-from-db2-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren vanuit DB2 Azure Blob

In dit voorbeeld ziet u hoe gegevens van een DB2-database op het bedrijf kopiëren naar een Azure Blob-opslag. Gegevens kunnen wel gekopieerde **rechtstreeks** naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.  
 
Het monster heeft de volgende data factory entiteiten:

1.  Een gekoppelde service van het type [OnPremisesDb2](data-factory-onprem-db2-connector.md#db2-linked-service-properties).
2.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [RelationalTable](data-factory-onprem-db2-connector.md#db2-dataset-type-properties).
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
5.  Een [pijpleiding](data-factory-create-pipelines.md) met kopie activiteit die gebruikmaakt van [RelationalSource](data-factory-onprem-db2-connector.md#db2-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

Het monster kopieert gegevens van resultaten van een query in een DB2-database naar een Azure blob per uur. De JSON-eigenschappen in deze voorbeelden gebruikt worden in voorbeelden van de volgende secties beschreven. 

Als eerste stap installeren en configureren van een gateway data management. Instructies vindt u in het artikel [verplaatst gegevens tussen de locaties op gebouwen en wolken](data-factory-move-data-between-onprem-and-cloud.md) .

**Gekoppelde DB2-service:**

    {
        "name": "OnPremDb2LinkedService",
        "properties": {
            "type": "OnPremisesDb2",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
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

**DB2 invoer dataset:**

In het voorbeeld wordt ervan uitgegaan dat u een tabel hebt gemaakt "MyTable" in de DB2 en bevat een kolom met de naam 'tijdstempel' tijd reeks gegevens.

'Externe' instellen: waar de Data Factory service meldt dat deze dataset de fabriek gegevens buiten en niet door een activiteit in de fabriek van de gegevens wordt geproduceerd. U ziet dat het **type** is ingesteld op **RelationalTable**. 

    {
        "name": "Db2DataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremDb2LinkedService",
            "typeProperties": {},
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
        "name": "AzureBlobDb2DataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Pipeline-exemplaar activiteit:**

De pijplijn bevat een kopie activiteit die is geconfigureerd voor het gebruik van de invoer en uitvoer datasets en elk uur is gepland. **Het type** is ingesteld op **RelationalSource** in de pijplijn JSON definitie en **sink** -type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de eigenschap **query** selecteert de gegevens uit de tabel Orders.

    {
        "name": "CopyDb2ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from \"Orders\""
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Db2DataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobDb2DataSet"
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
                    "name": "Db2ToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="db2-linked-service-properties"></a>Eigenschappen van gekoppelde DB2-service

De volgende tabel bevat een beschrijving van JSON-elementen die specifiek zijn voor gekoppelde DB2-service. 

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- | 
| type | De eigenschap type moet worden ingesteld op: **OnPremisesDB2** | Ja |
| Server | De naam van de server DB2. | Ja |
| database | De naam van de DB2-database. | Ja |
| schema | De naam van het schema in de database. Naam van het schema is hoofdlettergevoelig. | Nee |
| authenticationType | Type verificatie dat wordt gebruikt voor verbinding met de DB2-database. Mogelijke waarden zijn: anoniem, basisverificatie en Windows. | Ja |
| gebruikersnaam | Geef de gebruikersnaam als u basisverificatie of Windows-verificatie gebruikt. | Nee |
| wachtwoord | Geef het wachtwoord voor de gebruikersaccount die u hebt opgegeven voor de gebruikersnaam. | Nee |
| gatewayName | De naam van de gateway die de Data Factory service verbinding maken met de DB2-database op de ruimten moet gebruiken. | Ja |

Zie [referenties voor instelling en beveiliging](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) voor meer informatie over het instellen van referenties voor een gegevensbron op ruimten DB2. 


## <a name="db2-dataset-type-properties"></a>Eigenschappen voor DB2 dataset

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van datasets. Secties zoals structuur, beschikbaarheid en het beleid van een dataset JSON zijn vergelijkbaar voor alle typen van dataset (Azure SQL Azure blob, Azure tabel, enz.).

De sectie typeProperties verschilt voor elk type dataset en vindt u informatie over de locatie van de gegevens in het gegevensarchief. De sectie typeProperties voor dataset van het type RelationalTable (waaronder DB2 dataset) heeft de volgende eigenschappen.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- | 
| Tabelnaam | De naam van de tabel in de DB2-Database-instantie die gekoppelde service naar verwijst. De tabelnaam is hoofdlettergevoelig. | Nee (als de **query** van de **RelationalSource** is opgegeven) |

## <a name="db2-copy-activity-type-properties"></a>Eigenschappen voor DB2 kopie activiteit

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen van de naam, beschrijving, input en output-tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren aan de andere kant met elk activiteitstype. Activiteit kopiëren verschillen ze afhankelijk van de typen bronnen en putten.

Voor activiteit kopiëren, wanneer de bron van het type **RelationalSource** (waaronder DB2) zijn de volgende eigenschappen beschikbaar in de sectie typeProperties:


| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------- | -------------- |
| query | De aangepaste query gebruiken om gegevens te lezen. | SQL-queryreeks. Bijvoorbeeld: `"query": "select * from "MySchema"."MyTable""`. | Nee (als de **tabelnaam** van **dataset** is opgegeven)|

> [AZURE.NOTE] Schema- en tabelnamen zijn hoofdlettergevoelig. Plaatst u de namen in "" (dubbele aanhalingstekens) in de query.  

**Voorbeeld:**

    "query": "select * from "DB2ADMIN"."Customers""


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-db2"></a>Toewijzing voor DB2
Zoals vermeld in het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) , voert de activiteit kopie automatische conversie van brontypen opvangen typen met de volgende 2-stap benadering:

1. Converteren van native brontypen naar .NET-type
2. Van het type .NET converteren naar native sink-type

Bij het verplaatsen van gegevens naar de DB2, worden de volgende toewijzingen van DB2 type type .NET gebruikt.

DB2-Database type | .NET framework-type 
----------------- | ------------------- 
SmallInt | Int16
Geheel getal | Int32
BigInt | Int64
Real | Enkele
Getal met dubbele precisie | Getal met dubbele precisie
Float | Getal met dubbele precisie
Decimaal | Decimaal
DecimalFloat | Decimaal
Numerieke | Decimaal
Datum | Datum/tijd
Tijd | TimeSpan
Tijdstempel | Datum/tijd
XML | Byte]
CHAR | Tekenreeks
VarChar | Tekenreeks
LongVarChar | Tekenreeks
DB2DynArray | Tekenreeks
Binaire | Byte]
VarBinary | Byte]
LongVarBinary | Byte]
Afbeelding | Tekenreeks
VarGraphic | Tekenreeks
LongVarGraphic | Tekenreeks
CLOB | Tekenreeks
BLOB | Byte]
DbClob | Tekenreeks
SmallInt | Int16
Geheel getal | Int32
BigInt | Int64
Real | Enkele
Getal met dubbele precisie | Getal met dubbele precisie
Float | Getal met dubbele precisie
Decimaal | Decimaal
DecimalFloat | Decimaal
Numerieke | Decimaal
Datum | Datum/tijd
Tijd | TimeSpan
Tijdstempel | Datum/tijd
XML | Byte]
CHAR | Tekenreeks


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Prestatie en afstemming  
Zie de [uitvoering van de activiteit & Tuning Guide](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties gevolgen van verplaatsing van gegevens (kopiëren activiteit) in Azure Data Factory en verschillende manieren optimaliseren.



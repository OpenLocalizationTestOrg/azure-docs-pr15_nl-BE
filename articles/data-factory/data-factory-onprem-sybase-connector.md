<properties 
    pageTitle="Gegevens verplaatsen van Sybase | Azure Data Factory" 
    description="Informatie over het verplaatsen van gegevens uit de Database van Sybase met Azure Data Factory." 
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

# <a name="move-data-from-sybase-using-azure-data-factory"></a>Gegevens verplaatsen van Sybase met Azure Data Factory 

In dit artikel wordt beschreven hoe u kunt de kopie activiteit in een fabriek Azure gegevens gegevens van Sybase verplaatsen naar een ander gegevensarchief. Dit artikel is gebaseerd op het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) activiteit kopiëren en gegevens ondersteunde archief combinaties een algemeen overzicht van de verplaatsing van gegevens biedt.

Data Factory-service ondersteunt verbindingen met voor bedrijfsruimten Sybase bronnen met behulp van de Data Management Gateway. Zie artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies over het instellen van de gateway [verplaatst gegevens tussen de locaties op gebouwen en wolken](data-factory-move-data-between-onprem-and-cloud.md) . 

> [AZURE.NOTE]
> Gateway is vereist, zelfs als de Sybase-database wordt gehost in een IaaS Azure VM. U kunt de gateway installeren op dezelfde IaaS VM als gegevensarchief of een andere VM als de gateway verbinding met de database maken kan. 

Data factory ondersteunt momenteel alleen verplaatst gegevens van Sybase naar andere opgeslagen gegevens, niet vanuit andere opgeslagen gegevens voor Sybase.

## <a name="installation"></a>Installatie

Voor Data Management Gateway verbinding maken met de Sybase-Database, moet u de [data provider voor Sybase](http://go.microsoft.com/fwlink/?linkid=324846) op hetzelfde systeem als de Data Management Gateway hebt geïnstalleerd.

> [AZURE.NOTE] Zie [problemen oplossen gateway](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor tips over het oplossen van verbinding/gateway verband houdende vraagstukken. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om een pijpleiding die gegevens uit een database van Sybase gekopieerd naar een van de ondersteunde sink gegevensarchieven maken is de wizard kopiëren gebruiken. Zie [Zelfstudie: maken van een pijplijn met behulp van de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met behulp van de wizard kopiëren. 

Het volgende voorbeeld wordt een monster JSON definities die u gebruiken kunt voor het maken van een pijplijn met [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe gegevens van Sybase-database kopiëren naar Azure Blob-opslag. Gegevens kunnen echter worden gekopieerd naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.   

## <a name="sample-copy-data-from-sybase-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren vanuit Sybase Azure Blob
In dit voorbeeld ziet u hoe gegevens van een Sybase-database kopiëren naar een Azure Blob-opslag. Gegevens kunnen wel gekopieerde **rechtstreeks** naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.  
 
Het monster heeft de volgende data factory entiteiten:

1.  Een gekoppelde service van het type [OnPremisesSybase](data-factory-onprem-sybase-connector.md#sybase-linked-service-properties).
2.  Een liked service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [RelationalTable](data-factory-onprem-sybase-connector.md#sybase-dataset-type-properties).
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  De [pijplijn](data-factory-create-pipelines.md) met kopie activiteit die gebruikmaakt van [RelationalSource](data-factory-onprem-sybase-connector.md#sybase-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Het monster kopieert gegevens van resultaten van een query in de Sybase database naar een blob elk uur. De JSON-eigenschappen in deze voorbeelden gebruikt worden in voorbeelden van de volgende secties beschreven. 

Instellen als eerste stap de data management gateway. De instructies zijn in het artikel [verplaatst gegevens tussen de locaties op gebouwen en wolken](data-factory-move-data-between-onprem-and-cloud.md) .

**Sybase gekoppeld service:**

    {
        "name": "OnPremSybaseLinkedService",
        "properties": {
            "type": "OnPremisesSybase",
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


**Sybase invoer dataset:**

In het voorbeeld wordt ervan uitgegaan dat u een tabel hebt gemaakt "MyTable" in Sybase en bevat een kolom met de naam 'tijdstempel' tijd reeks gegevens.

'Externe' instellen: waar de Data Factory service meldt dat deze dataset de fabriek gegevens buiten en niet door een activiteit in de fabriek van de gegevens wordt geproduceerd. Merk op dat het **type** van de gekoppelde service is ingesteld op: **RelationalTable**. 
    
    {
        "name": "SybaseDataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremSybaseLinkedService",
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
        "name": "AzureBlobSybaseDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

De pijplijn bevat een kopie activiteit die is geconfigureerd voor het gebruik van de invoer en uitvoer datasets en elk uur wordt uitgevoerd. **Het type** is ingesteld op **RelationalSource** in de pijplijn JSON definitie en **sink** -type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de eigenschap **query** selecteert de gegevens uit de DBA. De ordertabel in de database.


    {
        "name": "CopySybaseToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from DBA.Orders"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "SybaseDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobSybaseDataSet"
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
                    "name": "SybaseToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="sybase-linked-service-properties"></a>Eigenschappen van gekoppelde Sybase-service

De volgende tabel bevat een beschrijving van JSON-elementen die specifiek zijn voor Sybase gekoppeld service.

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
type | De eigenschap type moet worden ingesteld op: **OnPremisesSybase** | Ja
Server | De naam van de server Sybase. | Ja
database | De naam van de Sybase-database. | Ja 
schema  | De naam van het schema in de database. | Nee
authenticationType | Type verificatie dat wordt gebruikt voor verbinding met de Sybase-database. Mogelijke waarden zijn: anoniem, basisverificatie en Windows. | Ja
gebruikersnaam | Geef de gebruikersnaam als u basisverificatie of Windows-verificatie gebruikt. | Nee
wachtwoord | Geef het wachtwoord voor de gebruikersaccount die u hebt opgegeven voor de gebruikersnaam. |  Nee
gatewayName | De naam van de gateway die de Data Factory-service gebruiken moet om verbinding met de database op ruimten Sybase. | Ja 

Zie [referenties voor instelling en beveiliging](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) voor meer informatie over het instellen van referenties voor een gegevensbron op ruimten Sybase.

## <a name="sybase-dataset-type-properties"></a>Sybase dataset-eigenschappen

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van datasets. Secties zoals structuur, beschikbaarheid en het beleid van een dataset JSON zijn vergelijkbaar voor alle typen van dataset (Azure SQL Azure blob, Azure tabel, enz.).

De sectie typeProperties verschilt voor elk type dataset en vindt u informatie over de locatie van de gegevens in het gegevensarchief. De sectie **typeProperties** voor dataset van het type **RelationalTable** (waaronder Sybase dataset) heeft de volgende eigenschappen:

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
Tabelnaam | De naam van de tabel in de Sybase Database-instantie die gekoppelde service naar verwijst. | Nee (als de **query** van de **RelationalSource** is opgegeven)

## <a name="sybase-copy-activity-type-properties"></a>Sybase eigenschappen activiteit kopiëren 

Voor een volledige lijst met secties & eigenschappen beschikbaar voor het definiëren van activiteiten, Zie [pijpleidingen maken](data-factory-create-pipelines.md) artikel. Eigenschappen van de naam, beschrijving, input en output-tabellen en -beleid zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren aan de andere kant met elk activiteitstype. Activiteit kopiëren verschillen ze afhankelijk van de typen bronnen en putten.

Wanneer de bron van het type **RelationalSource** (waaronder Sybase) is de volgende eigenschappen zijn beschikbaar in de sectie **typeProperties** :

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
query | De aangepaste query gebruiken om gegevens te lezen. | SQL-queryreeks. Bijvoorbeeld: Selecteer * from MijnTabel. | Nee (als de **tabelnaam** van **dataset** is opgegeven)

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-sybase"></a>Toewijzing voor Sybase

Zoals vermeld in het artikel [Gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) , voert de activiteit kopie automatische conversie van brontypen opvangen typen met de volgende 2-stap benadering:

1. Converteren van native brontypen naar .NET-type
2. Van het type .NET converteren naar native sink-type

Sybase ondersteunt T-SQL en T-SQL-typen. Zie voor een tabel met toewijzingen van sql-typen type .NET, [Azure SQL Connector](data-factory-azure-sql-connector.md) artikel.

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Prestatie en afstemming  
Zie de [uitvoering van de activiteit & Tuning Guide](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties gevolgen van verplaatsing van gegevens (kopiëren activiteit) in Azure Data Factory en verschillende manieren optimaliseren.
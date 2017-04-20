<properties 
    pageTitle="Gegevens verplaatsen van Amazon Redshift Data Factory met | Microsoft Azure" 
    description="Informatie over het verplaatsen van gegevens van Amazon Redshift met Azure Data Factory." 
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
    ms.date="08/23/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Gegevens van Amazon Redshift, met Azure Data Factory

In dit artikel wordt beschreven hoe u de activiteit kopiëren in een fabriek Azure gegevens kunt gebruiken om gegevens op te slaan van Amazon Redshift naar een andere gegevens te verplaatsen. Dit artikel is gebaseerd op het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) activiteit kopiëren en een lijst van bron/sink-gegevensarchieven een algemeen overzicht van de verplaatsing van gegevens biedt.  

Data factory ondersteunt momenteel alleen verplaatst gegevens van Amazon Redshift naar andere opgeslagen gegevens, maar niet voor het verplaatsen van gegevens uit andere opgeslagen gegevens naar Amazon Redshift.

## <a name="prerequisites"></a>Vereisten

- Als u gegevens naar een gegevensarchief voor gebouwen verplaatsen wilt, Data Management-Gateway (IP-adres gebruiken van de computer) de toegang verlenen tot Amazon Redshift cluster. Zie [toegang tot het cluster machtigen](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) voor instructies. 
- Als u gegevens naar een gegevensarchief Azure verplaatsen wilt, Zie [IP-adresbereiken van Azure Data Center](https://www.microsoft.com/download/details.aspx?id=41653) voor het berekenen IP-adresbereiken (met inbegrip van SQL-bereik) gebruikt door de Microsoft Azure datacenters. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om een pijpleiding die gegevens van Amazon Redshift kopieert maken is de wizard kopiëren gebruiken. Zie [Zelfstudie: maken van een pijplijn met behulp van de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met behulp van de wizard kopiëren. 

Het volgende voorbeeld wordt een monster JSON definities die u gebruiken kunt voor het maken van een pijplijn met [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). U ziet hoe u gegevens kopiëren van Amazon Redshift naar Azure Blob-opslag. Gegevens kunnen echter worden gekopieerd naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-redshift-to-azure-blob"></a>Voorbeeld: Gegevens van Amazon Redshift kopiëren naar Azure Blob
In dit voorbeeld ziet u hoe gegevens uit een database van Amazon Redshift kopiëren naar een Azure Blob-opslag. Gegevens kunnen wel gekopieerde **rechtstreeks** naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.  
 
Het monster heeft de volgende data factory entiteiten:

- Een gekoppelde service van het type [AmazonRedshift](#linked-service-properties).
- Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Een invoer [dataset](data-factory-create-datasets.md) van het type [RelationalTable](#dataset-type-properties).
- Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Een [pijpleiding](data-factory-create-pipelines.md) met kopie activiteit die gebruikmaakt van [RelationalSource](#copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Het monster kopieert gegevens van resultaten van een query in Amazon Redshift naar een blob elk uur. De JSON-eigenschappen in deze voorbeelden gebruikt worden in voorbeelden van de volgende secties beschreven. 

**Amazon Redshift gekoppeld service**

    {
        "name": "AmazonRedshiftLinkedService",
        "properties":
        {
            "type": "AmazonRedshift",
            "typeProperties":
            {
                "server": "< The IP address or host name of the Amazon Redshift server >",
                "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
                "database": "<The database name of the Amazon Redshift database>",
                "username": "<username>",
                "password": "<password>"
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

**Invoer dataset Amazon Redshift**

Het instellen van **'externe': true** de Data Factory service meldt dat de dataset buiten de fabriek gegevens is en niet wordt geproduceerd door een activiteit in de fabriek van gegevens. Deze eigenschap instellen op true voor een input-gegevensset die niet wordt geproduceerd door een activiteit in de pijplijn.

    {
        "name": "AmazonRedshiftInputDataset",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "AmazonRedshiftLinkedService",
            "typeProperties": {
                "tableName": "<Table name>"
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
                "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

De pijplijn bevat een kopie activiteit die is geconfigureerd voor het gebruik van de invoer en uitvoer datasets en elk uur is gepland. **Het type** is ingesteld op **RelationalSource** in de pijplijn JSON definitie en **sink** -type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de eigenschap **query** selecteert de gegevens in het afgelopen uur te kopiëren.
    
    {
        "name": "CopyAmazonRedshiftToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonRedshiftInputDataset"
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
                    "name": "AmazonRedshiftToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Eigenschappen van gekoppelde service

De volgende tabel bevat een beschrijving van JSON-elementen die specifiek zijn voor de service Amazon Redshift gekoppeld.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- | 
| type | De eigenschap type moet worden ingesteld op: **AmazonRedshift**. | Ja |
| Server | IP-adres of de hostnaam de naam van de server Amazon Redshift. | Ja |
| poort | Het nummer van de TCP-poort waarop de server Amazon Redshift luistert voor clientverbindingen. | Nee, de standaardwaarde: 5439 |
| database | De naam van de database Amazon Redshift. | Ja |
| gebruikersnaam | De naam van de gebruiker die toegang tot de database heeft.| Ja |
| wachtwoord | Het wachtwoord voor de gebruikersaccount.| Ja |


## <a name="dataset-type-properties"></a>DataSet-eigenschappen

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van datasets. Secties zoals structuur, beschikbaarheid en beleid zijn vergelijkbaar voor alle typen van dataset (Azure SQL Azure blob, Azure tabel, enz.).

De sectie **typeProperties** verschilt voor elk type dataset en vindt u informatie over de locatie van de gegevens in het gegevensarchief. De sectie typeProperties voor dataset van het type **RelationalTable** (waaronder Amazon Redshift dataset) heeft de volgende eigenschappen

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| Tabelnaam | De naam van de tabel in de gekoppelde service naar verwijst Amazon Redshift database. | Nee (als de **query** van de **RelationalSource** is opgegeven) | 

## <a name="copy-activity-type-properties"></a>Activiteit eigenschappen kopiëren

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen van de naam, beschrijving, input en output-tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie **typeProperties** van de activiteit variëren aan de andere kant met elk activiteitstype. Activiteit kopiëren verschillen ze afhankelijk van de typen bronnen en putten.

Wanneer de bron van de activiteit van de kopie is van het type **RelationalSource** (waaronder Amazon Redshift) is de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------------- | -------- |
| query | De aangepaste query gebruiken om gegevens te lezen. | SQL-queryreeks. Bijvoorbeeld: Selecteer * from MijnTabel. | Nee (als de **tabelnaam** van **dataset** is opgegeven) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-amazon-redshift"></a>Toewijzing voor Amazon Redshift

Zoals vermeld in het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) , voert de activiteit kopiëren automatische conversie van brontypen opvangen typen met de benadering van de volgende twee stappen:

1. Converteren van native brontypen naar .NET-type
2. Van het type .NET converteren naar native sink-type

Bij het verplaatsen van gegevens op Amazon Redshift, worden de volgende toewijzingen voor .NET van Amazon Redshift typen gebruikt.

Amazon Redshift Type | .NET op basis van Type
-------------------- | ---------------
SMALLINT | Int16
GEHEEL GETAL | Int32
BIGINT | Int64
DECIMAAL | Decimaal
REAL | Enkele
DUBBELE PRECISIE | Getal met dubbele precisie
BOOLE-WAARDE | Tekenreeks
CHAR | Tekenreeks
VARCHAR | Tekenreeks
DATUM | Datum/tijd
TIJDSTEMPEL | Datum/tijd
TEKST | Tekenreeks



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Prestatie en afstemming  
Zie de [uitvoering van de activiteit & Tuning Guide](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties gevolgen van verplaatsing van gegevens (kopiëren activiteit) in Azure Data Factory en verschillende manieren optimaliseren.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende artikelen: 
- [Zelfstudie kopiëren activiteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een activiteit kopiëren. 
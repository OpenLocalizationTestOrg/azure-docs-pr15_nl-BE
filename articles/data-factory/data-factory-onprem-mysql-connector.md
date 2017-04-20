<properties 
    pageTitle="Gegevens verplaatsen van MySQL | Azure Data Factory" 
    description="Informatie over het verplaatsen van gegevens van de MySQL-database met behulp van Azure Data Factory." 
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

# <a name="move-data-from-mysql-using-azure-data-factory"></a>Verplaatsen van gegevens in MySQL met behulp van Azure Data Factory

In dit artikel wordt beschreven hoe u de activiteit kopiëren in een fabriek Azure gegevens kunt gebruiken om gegevens op te slaan van MySQL aan een andere gegevens te verplaatsen. Dit artikel is gebaseerd op het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) activiteit kopiëren en gegevens ondersteunde archief combinaties een algemeen overzicht van de verplaatsing van gegevens biedt.

Data Factory-service ondersteunt verbindingen met voor bedrijfsruimten MySQL bronnen met behulp van de Data Management Gateway. Zie artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies over het instellen van de gateway [verplaatst gegevens tussen de locaties op gebouwen en wolken](data-factory-move-data-between-onprem-and-cloud.md) . 

> [AZURE.NOTE] Gateway is vereist, zelfs als de MySQL-database wordt gehost in een IaaS Azure VM (virtual machine). U kunt de gateway installeren op dezelfde VM als gegevensarchief of een andere VM als de gateway verbinding met de database maken kan.  

Data factory ondersteunt momenteel alleen verplaatst gegevens van MySQL aan andere opgeslagen gegevens, maar niet voor het verplaatsen van gegevens uit andere opgeslagen gegevens naar MySQL.

## <a name="installation"></a>Installatie 
Voor Data Management-Gateway verbinding kunnen maken met de MySQL-Database, moet u de [MySQL Connector/Net 6.6.5 voor Microsoft Windows](http://go.microsoft.com/fwlink/?LinkId=278885) installeert op hetzelfde systeem als de Gateway Data Management.

> [AZURE.NOTE] Zie [problemen oplossen gateway](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor tips over het oplossen van verbinding/gateway verband houdende vraagstukken. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om een pijpleiding die gegevens opgehaald uit een MySQL-database op een van de ondersteunde sink gegevensarchieven maken is de wizard kopiëren gebruiken. Zie [Zelfstudie: maken van een pijplijn met behulp van de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met behulp van de wizard kopiëren. 

Het volgende voorbeeld wordt een monster JSON definities die u gebruiken kunt voor het maken van een pijpleiding. Zie voor een compleet overzicht met stapsgewijze instructies voor het artikel [verplaatst gegevens tussen de locaties op gebouwen en wolken](data-factory-move-data-between-onprem-and-cloud.md) . Gegevens kunnen worden gekopieerd naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.   

## <a name="sample-copy-data-from-mysql-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren vanuit MySQL Azure Blob
In dit voorbeeld ziet u hoe gegevens van een MySQL-database op het bedrijf kopiëren naar een Azure Blob-opslag. Gegevens kunnen wel gekopieerde **rechtstreeks** naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.  

> [AZURE.IMPORTANT] In dit voorbeeld worden de JSON-fragmenten. Bevat geen stapsgewijze instructies voor het maken van de data factory genoemd. Zie artikel [verplaatst gegevens tussen de locaties op gebouwen en wolken](data-factory-move-data-between-onprem-and-cloud.md) voor stapsgewijze instructies. 
 
Het monster heeft de volgende data factory entiteiten:

1.  Een gekoppelde service van het type [OnPremisesMySql](data-factory-onprem-mysql-connector.md#mysql-linked-service-properties).
2.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [RelationalTable](data-factory-onprem-mysql-connector.md#mysql-dataset-type-properties).
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Een [pijpleiding](data-factory-create-pipelines.md) met kopie activiteit die gebruikmaakt van [RelationalSource](data-factory-onprem-mysql-connector.md#mysql-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Het monster kopieert gegevens van resultaten van een query in een MySQL-database naar een blob per uur. De JSON-eigenschappen in deze voorbeelden gebruikt worden in voorbeelden van de volgende secties beschreven. 

Instellen als eerste stap de data management gateway. De instructies zijn in het artikel [verplaatst gegevens tussen de locaties op gebouwen en wolken](data-factory-move-data-between-onprem-and-cloud.md) . 

**MySQL gekoppeld service**

    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
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

**MySQL invoer dataset**

In het voorbeeld wordt ervan uitgegaan dat u een tabel hebt gemaakt 'MyTable' in MySQL en een kolom met de naam 'timestampcolumn' voor een reeks tijdgegevens bevat.

Instelling 'externe': 'true' informeert de Data Factory-service dat de tabel de fabriek gegevens buiten en niet wordt geproduceerd door een activiteit in de fabriek van gegevens.
    
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
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



**Azure Blob uitvoer dataset**

Gegevens worden weggeschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het pad naar de map wordt gebruikt voor jaar, maand, dag en uur delen van de begintijd.

    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
        "name": "CopyMySqlToBlob",
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
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="mysql-linked-service-properties"></a>Eigenschappen van gekoppelde MySQL-service

De volgende tabel bevat een beschrijving van JSON-elementen die specifiek zijn voor gekoppelde MySQL service.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- | 
| type | De eigenschap type moet worden ingesteld op: **OnPremisesMySql** | Ja |
| Server | De naam van de server MySQL. | Ja |
| database | De naam van de MySQL-database. | Ja | 
| schema  | De naam van het schema in de database. | Nee | 
| authenticationType | Type verificatie dat wordt gebruikt om verbinding met de MySQL-database. Mogelijke waarden zijn: anoniem, basisverificatie en Windows. | Ja | 
| gebruikersnaam | Geef de gebruikersnaam als u basisverificatie of Windows-verificatie gebruikt. | Nee | 
| wachtwoord | Geef het wachtwoord voor de gebruikersaccount die u hebt opgegeven voor de gebruikersnaam. | Nee | 
| gatewayName | De naam van de gateway die de Data Factory service verbinding maken met de MySQL-database op de ruimten moet gebruiken. | Ja |

Zie [referenties voor instelling en beveiliging](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) voor meer informatie over het instellen van referenties voor een gegevensbron op ruimten MySQL.

## <a name="mysql-dataset-type-properties"></a>Eigenschappen van MySQL dataset

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van datasets. Secties zoals structuur, beschikbaarheid en het beleid van een dataset JSON zijn vergelijkbaar voor alle typen van dataset (Azure SQL Azure blob, Azure tabel, enz.).

De sectie **typeProperties** verschilt voor elk type dataset en vindt u informatie over de locatie van de gegevens in het gegevensarchief. De sectie typeProperties voor dataset van het type **RelationalTable** (waaronder MySQL dataset) heeft de volgende eigenschappen

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| Tabelnaam | De naam van de tabel in de MySQL-Database-instantie die gekoppelde service naar verwijst. | Nee (als de **query** van de **RelationalSource** is opgegeven) | 

## <a name="mysql-copy-activity-type-properties"></a>Eigenschappen van MySQL kopie activiteit

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen van de naam, beschrijving, invoer en uitvoer-tabellen zijn beleid beschikbaar zijn voor alle soorten activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie **typeProperties** van de activiteit variëren aan de andere kant met elk activiteitstype. Activiteit kopiëren verschillen ze afhankelijk van de typen bronnen en putten.

Wanneer de bron van de activiteit van de kopie van het type **RelationalSource** (waaronder MySQL) is de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------------- | -------- |
| query | De aangepaste query gebruiken om gegevens te lezen. | SQL-queryreeks. Bijvoorbeeld: Selecteer * from MijnTabel. | Nee (als de **tabelnaam** van **dataset** is opgegeven) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-mysql"></a>Toewijzing voor MySQL

Zoals vermeld in het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) , voert de activiteit kopiëren automatische conversie van brontypen opvangen typen met de benadering van de volgende twee stappen:

1. Converteren van native brontypen naar .NET-type
2. Van het type .NET converteren naar native sink-type

Als u gegevens naar MySQL, worden de volgende toewijzingen van MySQL typen gebruikt voor .NET typen.

| Type MySQL-Database | .NET framework-type |
| ------------------- | ------------------- | 
| niet-ondertekende bigint | Decimaal |
| bigint | Int64 |
| bits | Decimaal |
| BLOB | Byte] |
| BOOL |  Boole-waarde | 
| CHAR | Tekenreeks |
| datum | Datum/tijd |
| datum/tijd | Datum/tijd |
| decimaal | Decimaal |
| dubbele precisie | Getal met dubbele precisie |
| dubbel | Getal met dubbele precisie |
| Enum | Tekenreeks |
| Float | Enkele |
| niet-ondertekende integer | Int64 |
| int | Int32 |
| niet-ondertekende integer | Int64 |
| geheel getal | Int32 | 
| lange varbinary | Byte] |
| lange varchar | Tekenreeks |
| longblob | Byte] |
| LONGTEXT | Tekenreeks | 
| mediumblob |  Byte] | 
| niet-ondertekende mediumint | Int64 |
| mediumint | Int32 | 
| mediumtext | Tekenreeks |
| numerieke | Decimaal |
| real |  Getal met dubbele precisie |
| instellen | Tekenreeks |
| niet-ondertekende smallint | Int32 |
| smallint | Int16 |
| tekst | Tekenreeks |
| tijd | TimeSpan |
| tijdstempel | Datum/tijd |
| tinyblob | Byte] |
| niet-ondertekende tinyint |  Int16 | 
| tinyint | Int16 |
| tinytext | Tekenreeks | 
| varchar | Tekenreeks | 
| jaar | Int | 


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Prestatie en afstemming  
Zie de [uitvoering van de activiteit & Tuning Guide](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties gevolgen van verplaatsing van gegevens (kopiëren activiteit) in Azure Data Factory en verschillende manieren optimaliseren.


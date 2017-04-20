<properties 
    pageTitle="Gegevens verplaatsen naar/van Oracle Data Factory met | Microsoft Azure" 
    description="Informatie over het verplaatsen van gegevens uit Oracle-database die op-ruimten met behulp van Azure Data Factory." 
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

# <a name="move-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Gegevens verplaatsen van Oracle van voor ruimten met Azure Data Factory 

In dit artikel wordt beschreven hoe u data factory kopie activiteit kunt gebruiken om gegevens te verplaatsen van Oracle van/naar een andere gegevens opslaan. Dit artikel is gebaseerd op het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) activiteit kopiëren en gegevens ondersteunde archief combinaties een algemeen overzicht van de verplaatsing van gegevens biedt.

## <a name="installation"></a>Installatie 
Voor de Azure Data Factory service kunnen verbinding maken met de Oracle-database op de gebouwen, moet u de volgende onderdelen: 

- Data Management Gateway op dezelfde computer waarop de database of op een afzonderlijke computer om te voorkomen dat meedingt naar resources met de database. Data Management Gateway is een client agent met gegevensbronnen op ruimten cloud services zodanig beveiligd en beheerd. Zie artikel [verplaatst gegevens tussen op gebouwen en cloud](data-factory-move-data-between-onprem-and-cloud.md) voor meer informatie over Data Management Gateway. 
- Oracle Data Provider voor .NET. Dit onderdeel is opgenomen in [Oracle Data Access Components for Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Installeer de juiste versie (32/64 bits) op de hostcomputer waarop de gateway is geïnstalleerd. [Oracle Data Provider .NET 12,1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) toegang tot Oracle database 10 g Release 2 of hoger.

    Als u 'XCopy installatie' kiest, voert u de stappen in de readme.htm. Wij raden u kiest de installateur met de gebruikersinterface (niet-XCopy een) aan. 
 
    Na de installatie van de provider, start u de service Data Management Gateway-Host op de computer met Services-applet (of) Data Management Gateway Configuration Manager.  

> [AZURE.NOTE] Zie [problemen oplossen gateway](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor tips over het oplossen van verbinding/gateway verband houdende vraagstukken. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om een pijpleiding die gegevens van/naar een Oracle-database worden gekopieerd naar een van de ondersteunde sink gegevensarchieven maken is de wizard kopiëren gebruiken. Zie [Zelfstudie: maken van een pijplijn met behulp van de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met behulp van de wizard kopiëren. 

Het volgende voorbeeld wordt een monster JSON definities die u gebruiken kunt voor het maken van een pijplijn met [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe gegevens van/naar een Oracle-database van Azure Blob-opslag te kopiëren. Gegevens kunnen echter worden gekopieerd naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.   

## <a name="sample-copy-data-from-oracle-to-azure-blob"></a>Voorbeeld: Gegevens van Oracle naar Azure Blob kopiëren
In dit voorbeeld ziet u hoe gegevens uit een Oracle-database op het bedrijf kopiëren naar een Azure Blob-opslag. Gegevens kunnen wel gekopieerde **rechtstreeks** naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.  
 
Het monster heeft de volgende data factory entiteiten:

1.  Een gekoppelde service van het type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5.  Een [pijpleiding](data-factory-create-pipelines.md) met kopie-activiteit die wordt [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) als bron- en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) als sink gebruikt.

Het monster kopieert gegevens uit een tabel in een Oracle-database op het bedrijf naar een blob per uur. Zie de documentatie in de voorbeelden van de volgende secties voor meer informatie over de verschillende eigenschappen die in het monster wordt gebruikt.

**Gekoppelde Oracle-service:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure Blob storage service gekoppeld:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Oracle invoer dataset:**

In het voorbeeld wordt ervan uitgegaan dat u een tabel hebt gemaakt "MyTable" in Oracle en een kolom met de naam 'timestampcolumn' voor een reeks tijdgegevens bevat. 

Instelling 'externe': 'true' informeert de Data Factory-service dat de dataset buiten de fabriek gegevens is en niet wordt geproduceerd door een activiteit in de fabriek van gegevens.

    {
        "name": "OracleInput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
               "external": true,
            "availability": {
                "offset": "01:00:00",
                "interval": "1",
                "anchorDateTime": "2014-02-27T12:00:00",
                "frequency": "Hour"
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

Gegevens worden weggeschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Naam van het pad en de bestandsnaam voor de blob dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het pad naar de map wordt gebruikt voor jaar, maand, dag en uur delen van de begintijd.
    
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


**Pipeline-exemplaar activiteit:**

De pijplijn bevat een kopie activiteit die is geconfigureerd voor het gebruik van de invoer en uitvoer datasets en elk uur wordt uitgevoerd. **Het type** is ingesteld op **OracleSource** in de pijplijn JSON definitie en **sink** -type is ingesteld op **BlobSink**.  De SQL-query die is opgegeven met de eigenschap **oracleReaderQuery** wordt de gegevens in het afgelopen uur kopiëren geselecteerd.

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": " OracleInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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


U moet de query-tekenreeks op basis van hoe datums worden geconfigureerd in uw Oracle-database aanpassen. Als u het volgende foutbericht weergegeven: 

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

Wellicht moet u de query wijzigen, zoals in het volgende voorbeeld (met de functie to_date):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"

## <a name="sample-copy-data-from-azure-blob-to-oracle"></a>Voorbeeld: Gegevens kopiëren vanuit Azure Blob Oracle
In dit voorbeeld ziet u hoe u gegevens kopiëren uit een Azure Blob-opslag met een Oracle-database op gebouwen. Gegevens kunnen wel gekopieerde **rechtstreeks** vanuit een van de bronnen vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.  
 
Het monster heeft de volgende data factory entiteiten:

1.  Een gekoppelde service van het type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
5.  Een [pijpleiding](data-factory-create-pipelines.md) met kopie activiteit die [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) als [OracleSink](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) als sink bron gebruikt.

Het monster kopieert gegevens van een blob aan een tabel in een Oracle-database op ruimten elk uur. Zie de documentatie in de voorbeelden van de volgende secties voor meer informatie over de verschillende eigenschappen die in het monster wordt gebruikt.

**Gekoppelde Oracle-service:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure Blob storage service gekoppeld:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Azure Blob invoer dataset**

Gegevens wordt opgehaald uit een nieuwe blob elk uur (frequentie: uur, interval: 1). Naam van het pad en de bestandsnaam voor de blob dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het pad naar de map gebruikt, jaar, maand en dag deel uitmaken van de begintijd en bestandsnaam gebruikt de uur-deel van de begintijd. 'externe': 'true' instelling informeert de Data Factory service dat deze tabel buiten de fabriek gegevens is en niet wordt geproduceerd door een activiteit in de fabriek van gegevens.

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

**Oracle uitvoer dataset:**

In het voorbeeld wordt ervan uitgegaan dat u een tabel "MyTable" in Oracle hebt gemaakt. De tabel maken in Oracle met hetzelfde aantal kolommen als u verwacht dat de Blob-CSV-bestand bevat. Nieuwe rijen worden toegevoegd aan de tabel elk uur.

    {
        "name": "OracleOutput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": "1"
            }
        }
    }


**Pipeline-exemplaar activiteit:**

De pijplijn bevat een kopie activiteit die is geconfigureerd voor het gebruik van de invoer en uitvoer datasets en elk uur is gepland. In de pipeline JSON-definitie, **het type** is ingesteld op **BlobSource** en de **sink** -type is ingesteld op **OracleSink**.  

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "OracleOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "OracleSink"
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


## <a name="oracle-linked-service-properties"></a>Eigenschappen van gekoppelde Oracle-service

De volgende tabel bevat een beschrijving van JSON-elementen die specifiek zijn voor gekoppelde Oracle-service. 

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
type | De eigenschap type moet worden ingesteld op: **OnPremisesOracle** | Ja
connectionString | Geef de gegevens die nodig zijn voor de verbinding met de Oracle-Database-instantie voor de eigenschap connectionString. | Ja 
gatewayName | De naam van de gateway die wordt gebruikt voor verbinding met de Oracle-server op locatie | Ja

Zie [referenties voor instelling en beveiliging](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) voor meer informatie over het instellen van referenties voor een Oracle-gegevensbron op gebouwen.
## <a name="oracle-dataset-type-properties"></a>Eigenschappen van Oracle dataset

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van datasets. Secties zoals structuur, beschikbaarheid en het beleid van een dataset JSON zijn vergelijkbaar voor alle typen van dataset (Oracle, Azure blob, Azure tabel, enz.).
 
De sectie typeProperties verschilt voor elk type dataset en vindt u informatie over de locatie van de gegevens in het gegevensarchief. De sectie typeProperties voor de gegevensset van het type OracleTable heeft de volgende eigenschappen:

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
Tabelnaam | De naam van de tabel in de Oracle-Database die de gekoppelde service naar verwijst. | Nee (als **oracleReaderQuery** van **OracleSource** is opgegeven)

## <a name="oracle-copy-activity-type-properties"></a>Eigenschappen van Oracle kopie activiteit

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen van de naam, beschrijving, input en output-tabellen en -beleid zijn beschikbaar voor alle typen activiteiten. 

> [AZURE.NOTE]
De activiteit kopiëren haalt slechts één invoer en uitvoer slechts één produceert.

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren aan de andere kant met elk activiteitstype. Activiteit kopiëren verschillen ze afhankelijk van de typen bronnen en putten.

### <a name="oraclesource"></a>OracleSource
Kopie activiteit, wanneer de bron van het type **OracleSource** zijn de volgende eigenschappen beschikbaar in de sectie **typeProperties** :

Eigenschap | Beschrijving |Toegestane waarden | Vereist
-------- | ----------- | ------------- | --------
oracleReaderQuery | De aangepaste query gebruiken om gegevens te lezen. | SQL-queryreeks. Bijvoorbeeld: Selecteer *from MijnTabel <br/> <br/>als niet wordt opgegeven, de SQL-instructie die wordt uitgevoerd: Selecteer* from MijnTabel | Nee (als de **tabelnaam** van **dataset** is opgegeven)

### <a name="oraclesink"></a>OracleSink
**OracleSink** ondersteunt de volgende eigenschappen:

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
writeBatchTimeout | De wachttijd voor de insert batchbewerking uitvoeren voordat er een optreedt time-out. | TimeSpan<br/><br/> Bijvoorbeeld: 00:30:00 (30 minuten). | Nee
writeBatchSize | Voegt de gegevens in de SQL-tabel wanneer de grootte van de writeBatchSize bereikt.   | Geheel getal (aantal rijen)| Nee (standaard: 10000)  
sqlWriterCleanupScript | Een query opgeeft voor kopie activiteit uit te voeren, dat gegevens van een specifiek segment wordt opgeruimd. | Een queryinstructie. | Nee
sliceIdentifierColumnName | Kolomnaam voor de kopie activiteit te vullen met een automatisch gegenereerd segment-id, die wordt gebruikt voor het opschonen van gegevens van een specifiek segment wanneer opnieuw opgeven. | De kolomnaam van een kolom met het gegevenstype van de binary(32). | Nee


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-oracle"></a>Toewijzing voor Oracle

Zoals vermeld in voert de activiteit [data verkeer activiteiten](data-factory-data-movement-activities.md) artikel kopiëren automatische conversie van brontypen opvangen typen met de volgende 2-stap benadering:

1. Converteren van native brontypen naar .NET-type
2. Van het type .NET converteren naar native sink-type

Bij het verplaatsen van gegevens uit Oracle, gebruikt de volgende toewijzingen van Oracle-gegevenstype naar type .NET en vice versa.

Oracle-gegevenstype | .NET framework-gegevenstype
---------------- | ------------------------
BBESTANDSGEGEVENS | Byte]
BLOB | Byte]
CHAR | Tekenreeks
CLOB | Tekenreeks
DATUM | Datum/tijd
FLOAT | Decimaal
GEHEEL GETAL | Decimaal
JAAR MAAND INTERVAL | Int32
INTERVAL VOOR TWEEDE DAG | TimeSpan
LANGE | Tekenreeks
LANGE RUWE | Byte]
NCHAR | Tekenreeks
NCLOB | Tekenreeks
NUMMER | Decimaal
NVARCHAR2 | Tekenreeks
RAW | Byte]
RIJ-ID | Tekenreeks
TIJDSTEMPEL | Datum/tijd
TIJDSTEMPEL MET PLAATSELIJKE TIJD | Datum/tijd
TIJDSTEMPEL MET TIME ZONE | Datum/tijd
NIET-ONDERTEKENDE INTEGER | Nummer
VARCHAR2 | Tekenreeks
XML | Tekenreeks

## <a name="troubleshooting-tips"></a>Tips voor probleemoplossing

**Probleem:** U ziet het volgende **foutbericht weergegeven**: kopie activiteit voldaan ongeldige parameters: UnknownParameterName, gedetailleerd bericht: kan vinden van de aangevraagde .net Framework Data Provider. Het kan niet worden geïnstalleerd'.  

**Mogelijke oorzaken:**

1. .NET Framework Data Provider voor Oracle is niet geïnstalleerd.
2. .NET Framework Data Provider voor Oracle op .NET Framework 2.0 is geïnstalleerd en is niet gevonden in de mappen van het .NET Framework 4.0. 

**Resolutie/tijdelijke oplossing:**

1. Als u de .NET Provider voor Oracle, [installeert u deze](http://www.oracle.com/technetwork/topics/dotnet/downloads/) nog niet hebt geïnstalleerd en probeer het scenario. 
2. Als u het foutbericht krijgt na het installeren van de provider, voert u de volgende stappen uit: 
    1. Configuratie van .NET 2.0 machine openen vanuit de map: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
    2. Zoeken voor **Oracle Data Provider voor .NET**en u moeten kunnen vinden van een vermelding zoals in het volgende voorbeeld unwn in het volgende voorbeeld van een under **system.data** -> **DbProviderFactories**:
            “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
2.  Dit item kopiëren naar het machine.config-bestand in de map v4.0: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config en de 4.xxx.x.x-versie wijzigen.
3.  "< Pad ODP.NET Installed > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" in de globale assembly-cache (GAC) installeren door het uitvoeren van `gacutil /i [provider path]`.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


## <a name="performance-and-tuning"></a>Prestatie en afstemming  
Zie de [uitvoering van de activiteit & Tuning Guide](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties gevolgen van verplaatsing van gegevens (kopiëren activiteit) in Azure Data Factory en verschillende manieren optimaliseren.

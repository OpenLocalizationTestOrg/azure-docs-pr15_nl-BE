<properties 
    pageTitle="Gegevens verplaatsen van HDFS voor gebouwen | Azure Data Factory" 
    description="Informatie over het verplaatsen van gegevens uit voor bedrijfsruimten HDFS met Azure Data Factory." 
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

# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Gegevens verplaatsen uit voor bedrijfsruimten HDFS met Azure Data Factory
In dit artikel wordt beschreven hoe u de activiteit van de kopie in een fabriek Azure gegevens kunt gebruiken om gegevens te verplaatsen vanuit een HDFS in ruimten met een ander gegevensarchief. Dit artikel is gebaseerd op het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) die kopie activiteit en combinaties van gegevens ondersteunde winkel een algemeen overzicht van de verplaatsing van gegevens biedt.

Data factory ondersteunt momenteel alleen verplaatsen gegevens uit een HDFS op ruimten naar andere opgeslagen gegevens, maar niet voor het verplaatsen van gegevens uit andere opgeslagen gegevens naar een HDFS op gebouwen.


## <a name="enabling-connectivity"></a>Verbindingen inschakelen
Data Factory-service ondersteunt verbindingen met voor bedrijfsruimten HDFS met behulp van de Data Management Gateway. Zie artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies over het instellen van de gateway [verplaatst gegevens tussen de locaties op gebouwen en wolken](data-factory-move-data-between-onprem-and-cloud.md) . Met de gateway verbinding maken met HDFS, zelfs als deze wordt gehost in een IaaS Azure VM. 

Terwijl u gateway op de machine op locatie of de Azure VM als de HDFS installeren kunt, wordt u aangeraden de gateway te installeren op een afzonderlijke computer/Azure VM IaaS. Gateway op een afzonderlijke computer hoeft minder bronconflicten en verbetert de prestaties. Wanneer u de gateway hebt geïnstalleerd op een afzonderlijke computer, wordt de computer moet toegang kunnen krijgen tot de computer met de HDFS. 


## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om een pijpleiding die gegevens opgehaald uit HDFS voor gebouwen maken is de wizard kopiëren gebruiken. Zie [Zelfstudie: maken van een pijplijn met behulp van de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met behulp van de wizard kopiëren. 

De volgende voorbeelden geven monster JSON definities die u gebruiken kunt voor het maken van een pijplijn met [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe gegevens van een HDFS op locatie kopiëren naar een Azure Blob-opslag. Gegevens kunnen echter worden gekopieerd naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.

## <a name="sample-copy-data-from-on-premises-hdfs-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren vanuit HDFS voor bedrijfsruimten Azure Blob

In dit voorbeeld ziet u hoe u gegevens kopiëren uit een HDFS op ruimten naar Azure Blob-opslag. Gegevens kunnen wel gekopieerde **rechtstreeks** naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.  
 
Het monster heeft de volgende data factory entiteiten:

1.  Een gekoppelde service van het type [OnPremisesHdfs](#hdfs-linked-service-properties).
2.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [bestandsshare](#hdfs-dataset-type-properties).
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Een [pijpleiding](data-factory-create-pipelines.md) met kopie activiteit die gebruikmaakt van [FileSystemSource](#hdfs-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Het monster kopieert gegevens van een HDFS in ruimten met een Azure blob elk uur. De JSON-eigenschappen in deze voorbeelden gebruikt worden in voorbeelden van de volgende secties beschreven. 

Als eerste stap, de data management gateway instellen. De instructies in het artikel [verplaatst gegevens tussen de locaties op gebouwen en wolken](data-factory-move-data-between-onprem-and-cloud.md) . 

**HDFS gekoppeld service** In dit voorbeeld wordt de Windows-verificatie. Zie sectie [HDFS gekoppelde service](#hdfs-linked-service-properties) voor verschillende soorten verificatie die u kunt gebruiken. 

    {
        "name": "HDFSLinkedService",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
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

**HDFS invoer dataset** Deze dataset verwijst naar de map HDFS DataTransfer/UnitTest /. De pijpleiding kopieert alle bestanden in deze map naar de bestemming. 

Instelling 'externe': 'true' informeert de Data Factory-service dat de dataset buiten de fabriek gegevens is en niet wordt geproduceerd door een activiteit in de fabriek van gegevens.
    
    {
        "name": "InputDataset",
        "properties": {
            "type": "FileShare",
            "linkedServiceName": "HDFSLinkedService",
            "typeProperties": {
                "folderPath": "DataTransfer/UnitTest/"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }




**Azure Blob uitvoer dataset**

Gegevens worden weggeschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het pad naar de map wordt gebruikt voor jaar, maand, dag en uur delen van de begintijd.

    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

De pijplijn bevat een kopie activiteit die is geconfigureerd voor het gebruik van deze invoer en uitvoer datasets en elk uur is gepland. **Het type** is ingesteld op **FileSystemSource** in de pijplijn JSON definitie en **sink** -type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de eigenschap **query** selecteert de gegevens in het afgelopen uur te kopiëren.
    
    {
        "name": "pipeline",
        "properties":
        {
            "activities":
            [
                {
                    "name": "HdfsToBlobCopy",
                    "inputs": [ {"name": "InputDataset"} ],
                    "outputs": [ {"name": "OutputDataset"} ],
                    "type": "Copy",
                    "typeProperties":
                    {
                        "source":
                        {
                            "type": "FileSystemSource"
                        },
                        "sink":
                        {
                            "type": "BlobSink"
                        }
                    },
                    "policy":
                    {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "00:05:00"
                    }
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="hdfs-linked-service-properties"></a>Eigenschappen van gekoppelde Service HDFS

De volgende tabel bevat de omschrijving voor specifieke HDFS JSON-elementen die service is gekoppeld.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- | 
| type | De eigenschap type moet worden ingesteld op: **Hdfs** | Ja | 
| URL | URL naar de HDFS | Ja |
| encryptedCredential | [Nieuw AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) uitvoer van de access-referentie. | Nee |
| Gebruikersnaam | Gebruikersnaam voor Windows-verificatie. | Ja (voor Windows-verificatie)
| wachtwoord | Het wachtwoord voor Windows-verificatie. | Ja (voor Windows-verificatie)
| authenticationType | Windows, of anoniem. | Ja |
| gatewayName | De naam van de gateway die de Data Factory-service gebruiken moet om te verbinden met de HDFS. | Ja |   

Zie [referenties voor instelling en beveiliging](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) voor meer informatie over het instellen van referenties voor HDFS voor gebouwen.

### <a name="using-anonymous-authentication"></a>Anonieme verificatie gebruikt

    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "userName": "hadoop",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }


### <a name="using-windows-authentication"></a>Met behulp van Windows-verificatie
    
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }
 


## <a name="hdfs-dataset-type-properties"></a>Eigenschappen van de Dataset HDFS

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van datasets. Secties zoals structuur, beschikbaarheid en het beleid van een dataset JSON zijn vergelijkbaar voor alle typen van dataset (Azure SQL Azure blob, Azure tabel, enz.).

De sectie **typeProperties** verschilt voor elk type dataset en vindt u informatie over de locatie van de gegevens in het gegevensarchief. De sectie typeProperties van de dataset van het type **bestandsshare** (waaronder dataset HDFS) heeft de volgende eigenschappen

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
folderPath | Het pad naar de map. Voorbeeld:`myfolder`<br/><br/>Gebruik de escape-teken ' \ ' voor speciale tekens in de tekenreeks. Bijvoorbeeld: map opgeven voor folder\subfolder,\\\\submap en geef d: voor d:\samplefolder,\\\\Voorbeeldmap.<br/><br/>U kunt deze eigenschap in combinatie met **partitionBy** mappaden op basis van een segment zijn begin/einde-datums en tijden. | Ja
Bestandsnaam | Geef de naam van het bestand in het **mappad** als u wilt dat de tabel om te verwijzen naar een bepaald bestand in de map. Als u een waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Als de bestandsnaam is niet opgegeven voor een dataset uitvoer, de naam van het gegenereerde bestand in de volgende zou zijn deze indeling: <br/><br/>Gegevens. <Guid>.txt (bijvoorbeeld:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Nee
partitionedBy | partitionedBy kan worden gebruikt om op te geven van een dynamische folderPath, bestandsnaam voor de reeks tijdgegevens. Voorbeeld: folderPath parameters voor elk uur van de gegevens. | Nee
fileFilter | Een filter moet worden gebruikt voor het selecteren van een subset van bestanden in het mappad in plaats van alle bestanden opgeven. <br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (enkel teken).<br/><br/>Voorbeeld 1:`"fileFilter": "*.log"`<br/>Voorbeeld 2:`"fileFilter": 2014-1-?.txt"`<br/><br/>**Opmerking**: fileFilter is van toepassing op een bestandsshare-gegevensset invoer | Nee
| indeling | De volgende notatietypen worden ondersteund: **tekstopmaak**, **AvroFormat**, **JsonFormat**, **OrcFormat**en **ParquetFormat**. Stel de eigenschap **type** onder notatie op een van deze waarden. Zie [Tekstopmaak opgeven](#specifying-textformat), [AvroFormat geven](#specifying-avroformat) [JsonFormat geven](#specifying-jsonformat), [Geven OrcFormat](#specifying-orcformat)en [ParquetFormat geven](#specifying-parquetformat) secties voor meer informatie. Als u bestanden wilt kopiëren als-is tussen bestand gebaseerde winkels (binaire kopie), kunt u de opmaaksectie in beide definities van de invoer en uitvoer dataset overslaan. | Nee 
| compressie | Geef het type en de mate van compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, en **BZip2** en ondersteunde niveaus zijn: **optimale** en **snelst**. Compressie-instellingen worden momenteel niet ondersteund voor gegevens in **AvroFormat** of **OrcFormat**. Voor meer informatie, Zie de sectie [compressie-ondersteuning](#compression-support) .  | Nee |



> [AZURE.NOTE] bestandsnaam en fileFilter worden niet tegelijkertijd gebruikt.


### <a name="using-partionedby-property"></a>PartionedBy, eigenschap

Zoals vermeld in het vorige gedeelte, kunt u een dynamische folderPath, bestandsnaam voor de reeks tijdgegevens met partitionedBy. U kunt dit doen met de Data Factory-macro's en de systeemvariabele SliceStart, SliceEnd, die de logische periode voor een bepaald segment aangeven. 

Voor meer informatie over time series datasets, raadpleegt plannen en segmenten, u [Gegevenssets maken](data-factory-create-datasets.md), [planning en uitvoering](data-factory-scheduling-and-execution.md)en [Pijpleidingen maken van](data-factory-create-pipelines.md) artikelen. 

#### <a name="sample-1"></a>Voorbeeld 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

In dit voorbeeld {segment} wordt vervangen door de waarde van Data Factory systeemvariabele SliceStart in de notatie (YYYYMMDDHH). De SliceStart verwijst naar de begintijd van het segment. Het mappad verschilt voor elk segment. Bijvoorbeeld: wikisampledataout-wikidatagateway/2014100103 of wikisampledataout-wikidatagateway/2014100104.

#### <a name="sample-2"></a>Voorbeeld 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

Jaar, maand, dag en tijd van de SliceStart in dit voorbeeld worden uitgepakt naar afzonderlijke variabelen die worden gebruikt door de eigenschappen van het mappad en de bestandsnaam.

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="hdfs-copy-activity-type-properties"></a>Eigenschappen van HDFS kopie activiteit

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen van de naam, beschrijving, input en output-tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren aan de andere kant met elk activiteitstype. Activiteit kopiëren verschillen ze afhankelijk van de typen bronnen en putten.

Voor activiteit kopiëren, wanneer de bron van het type **FileSystemSource** zijn de volgende eigenschappen beschikbaar in de sectie typeProperties:

**FileSystemSource** ondersteunt de volgende eigenschappen:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------------- | -------- |
| recursieve | Geeft aan of de gegevens worden gelezen recursief uit de submappen of alleen uit de opgegeven map. | True, False (standaard)| Nee |

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Prestatie en afstemming  
Zie de [uitvoering van de activiteit & Tuning Guide](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties gevolgen van verplaatsing van gegevens (kopiëren activiteit) in Azure Data Factory en verschillende manieren optimaliseren.


<properties 
    pageTitle="Gegevenssets maken in Azure Data Factory | Microsoft Azure" 
    description="Informatie over datasets in Azure Data Factory met voorbeelden van eigenschappen van de verschuiving en anchorDateTime maken."
    keywords="Voorbeeld van de dataset dataset maken, voorbeeld van de offset"
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="shlo"/>

# <a name="datasets-in-azure-data-factory"></a>DataSets in Azure Data Factory
In dit artikel datasets in Azure Data Factory beschrijving en voorbeelden zoals verschuiving, anchorDateTime en offset/stijl databases.

Wanneer u een gegevensset maakt, maakt u een verwijzing naar de gegevens die u wilt verwerken. Gegevens worden verwerkt (input/output) in een activiteit en een activiteit deel uitmaakt van een pijpleiding. Een dataset invoer vertegenwoordigt de invoer voor een activiteit in de pijplijn en een dataset output de output voor de activiteit.

DataSets bepalen gegevens in andere gegevensarchieven, zoals tabellen, bestanden, mappen en documenten. Nadat u een gegevensset maakt, kunt u met de activiteiten in een pijpleiding. Een gegevensset kan bijvoorbeeld een dataset invoer/uitvoer van een kopie of een activiteit HDInsightHive. De Azure portal biedt een visuele indeling van alle pijpleidingen en gegevens ingangen en uitgangen. In één oogopslag ziet u de relaties en afhankelijkheden van de pijpleidingen over alle bronnen voor uw zodat u altijd weet waar data vandaan komt en waar deze naartoe gaat.

In Azure Data Factory, kunt u gegevens ophalen uit een dataset kopiëren door activiteit te gebruiken in een pijpleiding.

> [AZURE.NOTE] Bent u nieuw in Azure Data Factory, Zie [Azure Data Factory-inleiding](data-factory-introduction.md) voor een overzicht van Azure Data Factory service. Zie [de eerste gegevens fabriek bouwen](data-factory-build-your-first-pipeline.md) voor een zelfstudie voor het maken van uw eerste data factory genoemd. Deze twee artikelen bevatten achtergrondinformatie hebt moet u dit artikel beter begrijpt. 

## <a name="define-datasets"></a>Datasets bepalen
Een gegevensset in Azure Data Factory is als volgt gedefinieerd: 


    {
        "name": "<name of dataset>",
        "properties": {
            "type": "<type of dataset: AzureBlob, AzureSql etc...>",
            "external": <boolean flag to indicate external data. only for input datasets>,
            "linkedServiceName": "<Name of the linked service that refers to a data store.>",
            "structure": [
                {
                    "name": "<Name of the column>",
                    "type": "<Name of the type>"
                }
            ],
            "typeProperties": {
                "<type specific property>": "<value>",
                "<type specific property 2>": "<value 2>",
            },
            "availability": {
                "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
                "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
            },
           "policy": 
            {      
            }
        }
    }

De volgende tabel worden de eigenschappen in de bovenstaande JSON beschreven:   

| Eigenschap | Beschrijving | Vereist | Standaard |
| -------- | ----------- | -------- | ------- |
| naam | De naam van de gegevensset. Zie [Azure Data Factory - regels voor naamgeving](data-factory-naming-rules.md) voor naamgevingsregels. | Ja | NB |
| type | Het type van de dataset. Een van de typen die worden ondersteund door Azure Data Factory opgeven (bijvoorbeeld: AzureBlob, AzureSqlTable). <br/><br/>Zie [Dataset Type](#Type) voor meer informatie. | Ja | NB |
| structuur | Schema van de dataset<br/><br/>Zie voor details de [Dataset structuur](#Structure) sectie. | Nr. | NB |
| typeProperties | Eigenschappen voor het geselecteerde bestandstype. Zie de sectie [Dataset Type](#Type) voor meer informatie over de ondersteunde typen en de bijbehorende eigenschappen. | Ja | NB |
| externe | Boole-vlag om aan te geven of een dataset expliciet wordt geproduceerd door een fabriek data pijpleiding of niet.  | Nee | False | 
| beschikbaarheid | Geeft het venster of de segmenteringshulplijnen model voor de productie van de dataset. <br/><br/>Zie voor meer informatie, [Beschikbaarheid van de Dataset](#Availability) sectie. <br/><br/>Voor meer informatie over de dataset segmentering model, Zie [planning en uitvoering van](data-factory-scheduling-and-execution.md) het artikel. | Ja | NB
| beleid | Definieert de criteria of de toestand waarin de segmenten van de dataset moeten voldoen. <br/><br/>Zie [Beleid voor gegevensverzameling](#Policy) sectie. | Nee | NB |

## <a name="dataset-example"></a>Voorbeeld van de DataSet
De dataset vertegenwoordigt in het volgende voorbeeld wordt een tabel met de naam **MyTable** in een **Azure SQL-database**. 

    {
        "name": "DatasetSample",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": 
            {
                "tableName": "MyTable"
            },
            "availability": 
            {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

Houd rekening met de volgende punten: 

- type is ingesteld op AzureSqlTable.
- tableName type, eigenschap (specifiek voor AzureSqlTable) is ingesteld op MyTable.
- linkedServiceName verwijst naar een gekoppelde service van het type AzureSqlDatabase. Zie de definitie van de volgende gekoppelde service. 
- beschikbaarheid frequentie is ingesteld op de dag en het interval is ingesteld op 1, wat betekent dat het segment dagelijks is geproduceerd.  

AzureSqlLinkedService wordt als volgt gedefinieerd:

    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "",
            "typeProperties": {
                "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
            }
        }
    }

In de bovenstaande JSON: 

- type is ingesteld op AzureSqlDatabase
- de eigenschap connectionString type geeft informatie verbinding maken met een Azure SQL-database.  


Zoals u ziet, wordt de gekoppelde service verbinding maken met een Azure SQL-database. De dataset wordt gedefinieerd welke tabel wordt gebruikt als een invoer/uitvoer voor de activiteit van een pijpleiding. Het gedeelte van de activiteit in de [pijplijn](data-factory-create-pipelines.md) JSON geeft aan of de dataset wordt gebruikt als een invoer- of dataset.


> [AZURE.IMPORTANT] Tenzij een dataset wordt geproduceerd door Azure Data Factory, moet het worden gemarkeerd als **extern**. Deze instelling is algemeen van toepassing op de invoer van de eerste activiteit in een pijpleiding.   

## <a name="Type"></a>Type DataSet
De ondersteunde gegevensbronnen en de dataset typen worden uitgelijnd. Zie de onderwerpen waarnaar wordt verwezen in het artikel [Gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md#supported-data-stores) voor meer informatie over de types en configuratie van datasets. Bijvoorbeeld als u gegevens uit een Azure SQL-database, klikt u op Azure SQL-Database in de lijst met ondersteunde gegevensarchieven om gedetailleerde informatie te bekijken.  

## <a name="Structure"></a>DataSet-structuur
De sectie **structuur** definieert het schema van de dataset. Bevat een verzameling van namen en de gegevenstypen van kolommen.  In het volgende voorbeeld wordt de dataset bevat drie kolommen slicetimestamp, projectnaam en pageviews en ze zijn van het type: String, String en decimale respectievelijk.

    structure:  
    [ 
        { "name": "slicetimestamp", "type": "String"},
        { "name": "projectname", "type": "String"},
        { "name": "pageviews", "type": "Decimal"}
    ]

## <a name="Availability"></a>Beschikbaarheid van de DataSet
De sectie **beschikbaarheid** in een dataset definieert het venster (per uur, dagelijks, wekelijks, enz.) of de segmenteringshulplijnen model voor de gegevensset. Zie [planning en uitvoering van](data-factory-scheduling-and-execution.md) artikel voor meer informatie over de dataset segmenteren en afhankelijkheid model. 

De volgende sectie beschikbaarheid geeft aan dat de dataset uitvoer geproduceerd per uur (of) input dataset per uur beschikbaar is:

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 1   
    }

De volgende tabel beschrijft de eigenschappen die u in de sectie beschikbaarheid gebruiken kunt: 

| Eigenschap | Beschrijving | Vereist | Standaard |
| -------- | ----------- | -------- | ------- |
| frequentie | Hiermee geeft u de tijdseenheid voor de productie van dataset segment.<br/><br/>**Ondersteunde frequentie**: minuut, uur, dag, Week, maand | Ja | NB |
| interval | Een vermenigvuldigingsfactor voor frequentie<br/><br/>"X frequentie" wordt bepaald hoe vaak het segment wordt geproduceerd.<br/><br/>Als u nodig hebt om te worden gesneden op uurbasis dataset, stelt u de **frequentie** in **uur**- en **interval** op **1**.<br/><br/>**Opmerking:** Als u een frequentie als minuut opgeven, raden we aan dat u het interval op niet minder dan 15 instellen | Ja | NB |
| stijl | Hiermee geeft u op of het segment aan het begin/einde van het interval moet worden geproduceerd.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Als frequentie is ingesteld op maand en stijl is ingesteld op EndOfInterval, wordt het segment wordt geproduceerd op de laatste dag van de maand. Als de stijl is ingesteld op StartOfInterval, wordt het segment wordt geproduceerd op de eerste dag van de maand.<br/><br/>Als frequentie is ingesteld op de dag en de stijl is ingesteld op EndOfInterval, wordt het segment wordt geproduceerd in het laatste uur van de dag.<br/><br/>Als frequentie is ingesteld op het uur en de stijl is ingesteld op EndOfInterval, wordt het segment aan het einde van het uur geproduceerd. Voor een segment voor de periode van 1-2 uur, het segment geproduceerd om 2 uur. | Nee | EndOfInterval |
| anchorDateTime | Hiermee definieert u de absolute positie in de tijd gebruikt door Taakplanner voor het berekenen van de dataset segmentgrenzen. <br/><br/>**Opmerking:** De meer gedetailleerde delen worden genegeerd als de AnchorDateTime datumonderdelen met meer detail dan de frequentie. <br/><br/>Als het **interval** per **uur** wordt bijvoorbeeld (frequentie: uur en interval: 1) en de **AnchorDateTime** bevat de **minuten en seconden**, wordt de **minuten en seconden** delen van de AnchorDateTime worden genegeerd. | Nee | 01-01/0001 |
| verschuiving | Het interval waarmee het begin en einde van alle segmenten van de dataset worden verschoven. <br/><br/>**Opmerking:** Als zowel anchorDateTime als offset wordt opgegeven, is het resultaat de gecombineerde ploeg. | Nee | NB |

### <a name="offset-example"></a>Voorbeeld van de marge

Dagelijks segmenten die beginnen om 6 uur in plaats van de standaard middernacht. 

    "availability":
    {
        "frequency": "Day",
        "interval": 1,
        "offset": "06:00:00"
    }

De **frequentie** is ingesteld op de **dag** en het **interval** is ingesteld op **1** (eenmaal per dag): als u wilt dat het segment moet worden geproduceerd om 6 uur in plaats van op het moment dat standaard: 12 AM. Houd er rekening mee dat dit een UTC-tijd is. 

## <a name="anchordatetime-example"></a>Voorbeeld van de anchorDateTime

**Voorbeeld:** 23 uur dataset segmenten op 2007 beginnen-04-19T08:00:00

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 23, 
        "anchorDateTime":"2007-04-19T08:00:00"  
    }

## <a name="offsetstyle-example"></a>Voorbeeld van de verschuiving /-stijl

Als u op een bepaalde datum en tijd dataset op maandelijkse basis nodig (Stel 3de van elke maand om 8:00 AM), gebruiken de tag **verschuiving van** de datum en tijd het moet worden uitgevoerd. 

    {
      "name": "MyDataset",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "availability": {
          "frequency": "Month",
          "interval": 1,
          "offset": "3.08:10:00",
          "style": "StartOfInterval"
        }
      }
    }


## <a name="Policy"></a>Beleid voor gegevensverzameling

De **beleid** -sectie in de dataset definitie definieert de criteria of de toestand waarin de segmenten van de dataset moeten voldoen.

### <a name="validation-policies"></a>Beleid voor validatie

| Naam van beleid | Beschrijving | Toegepast op | Vereist | Standaard |
| ----------- | ----------- | ---------- | -------- | ------- |
| minimumSizeMB | Valideert de gegevens in een **blob-Azure** voldoet aan de eisen van de minimale grootte (in megabytes). | Azure Blob | Nee | NB |
|minimumRows | Controleert of de gegevens in een **Azure SQL-database** of een **tabel Azure** het minimum aantal rijen bevat. | <ul><li>Azure SQL-Database</li><li>Azure tabel</li></ul> | Nee | NB

#### <a name="examples"></a>Voorbeelden

**minimumSizeMB:**

    "policy":
    
    {
        "validation":
        {
            "minimumSizeMB": 10.0
        }
    }

**minimumRows**

    "policy":
    {
        "validation":
        {
            "minimumRows": 100
        }
    }

### <a name="external-datasets"></a>Externe datasets

Externe datasets zijn die niet worden geproduceerd door een pijpleiding uitgevoerd in de fabriek van gegevens. Als de dataset is gemarkeerd als **extern**, kan het **ExternalData** beleid beïnvloeden van het gedrag van de beschikbaarheid van dataset segment worden gedefinieerd. 

Tenzij een dataset wordt geproduceerd door Azure Data Factory, moet het worden gemarkeerd als **extern**. Deze instelling Algemeen van toepassing op de invoer van de eerste activiteit in een pijpleiding tenzij activiteit of pijpleiding chaining wordt gebruikt. 

| Naam | Beschrijving | Vereist | Standaardwaarde  |
| ---- | ----------- | -------- | -------------- |
| dataDelay | Tijd om de controle van de beschikbaarheid van de externe gegevens voor het opgegeven segment. Bijvoorbeeld, als de gegevens beschikbaar zijn per uur moeten, kan de controleert u de externe gegevens beschikbaar is en het bijbehorende segment gereed oplopen met behulp van dataDelay.<br/><br/>Alleen van toepassing op dit moment.  Bijvoorbeeld, als het nu om 1:00 PM en deze waarde 10 minuten is, de validatie wordt gestart om 13:10 uur.<br/><br/>Deze instelling heeft geen invloed op segmenten in het verleden (segmenten met segment eindtijd + dataDelay < nu) worden verwerkt zonder enige vertraging.<br/><br/>De tijd is groter dan 23:59 uur moeten opgegeven met de indeling day.hours:minutes:seconds. Bijvoorbeeld, als u 24 uur, hoeft u 24:00:00; in plaats daarvan 1.00:00:00 gebruiken. Als u 24:00:00 uur, wordt deze behandeld als 24 dagen (24.00:00:00). Geef de 1:04:00:00 voor 1 dag en 4 uur. | Nee | 0 |
| retryInterval | De wachttijd tussen een storing en de volgende poging opnieuw. Van toepassing is op dit moment; Als de eerdere mislukte, wachten we dit lang na de laatste poging. <br/><br/>Als het nu om 1:00 pm, beginnen we de eerste poging. Als de duur van de eerste validatiecontrole is 1 minuut en de bewerking is mislukt, wordt de volgende poging is op 1:00 + 1 min (duur) + 1min (interval voor opnieuw proberen) = 1:02 pm. <br/><br/>Er is geen vertraging voor segmenten in het verleden. De nieuwe poging gebeurt onmiddellijk. | Nee | 00:01:00 (1 minuut) | 
| retryTimeout | De time-out voor nieuwe pogingen.<br/><br/>Als deze eigenschap is ingesteld op 10 minuten, moet de validatie worden voltooid binnen 10 minuten. Als het duurt langer dan 10 minuten voor het uitvoeren van de validatie, het opnieuw geblokkeerd.<br/><br/>Als alle pogingen voor de validatie een time-out optreedt, wordt het segment gemarkeerd als time-out. | Nee | 00:10:00 (10 minuten) |
| maximumRetry | Aantal keren om de beschikbaarheid van de externe gegevens te controleren. De maximaal toegestane waarde is 10. | Nee | 3 | 

## <a name="scoped-datasets"></a>Bereik datasets
Kunt u gegevenssets die zijn binnen het bereik van een pijpleiding door de eigenschap **datasets** . Deze datasets kan alleen worden gebruikt door de activiteiten binnen deze pijpleiding, maar niet door activiteiten in andere pijpleidingen. In het volgende voorbeeld definieert een pijpleiding met twee datasets - InputDataset rdc en OutputDataset-rdc - dat in de pijpleiding wordt gebruikt:  

> [AZURE.IMPORTANT] Bereik datasets worden alleen ondersteund met eenmalige pijpleidingen (**pipelineMode** ingesteld op **OneTime**). Zie [Onetime pipeline](data-factory-scheduling-and-execution.md#onetime-pipeline) voor meer informatie.

    {
        "name": "CopyPipeline-rdc",
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
                            "name": "InputDataset-rdc"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset-rdc"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1,
                        "style": "StartOfInterval"
                    },
                    "name": "CopyActivity-0"
                }
            ],
            "start": "2016-02-28T00:00:00Z",
            "end": "2016-02-28T00:00:00Z",
            "isPaused": false,
            "pipelineMode": "OneTime",
            "expirationTime": "15.00:00:00",
            "datasets": [
                {
                    "name": "InputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "InputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/input",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": true,
                        "policy": {}
                    }
                },
                {
                    "name": "OutputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "OutputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/output",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": false,
                        "policy": {}
                    }
                }
            ]
        }
    }
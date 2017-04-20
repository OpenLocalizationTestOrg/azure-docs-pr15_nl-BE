<properties 
    pageTitle="Gegevens verplaatsen van Data Factory met Cassandra | Microsoft Azure" 
    description="Informatie over het verplaatsen van gegevens uit een op gebouwen Cassandra database met Azure Data Factory." 
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
    ms.date="09/07/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Gegevens verplaatsen uit een op gebouwen Cassandra database met Azure Data Factory
In dit artikel wordt beschreven hoe u kunt de kopie activiteit in een fabriek Azure gegevens gegevens uit een database op gebouwen Cassandra kopiëren naar een gegevensarchief vermeld onder Sink-kolom in de sectie [ondersteunde bronnen en putten](data-factory-data-movement-activities.md#supported-data-stores) . Dit artikel is gebaseerd op het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) activiteit kopiëren en gegevens ondersteunde archief combinaties een algemeen overzicht van de verplaatsing van gegevens biedt.

Data factory ondersteunt momenteel alleen verplaatst gegevens uit een database Cassandra [sink gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores)ondersteund, maar niet verplaatst gegevens uit andere opgeslagen gegevens naar een database Cassandra.

## <a name="prerequisites"></a>Vereisten
Voor de Azure Data Factory service kunnen verbinding maken met uw database op gebouwen Cassandra, moet u het volgende: 

- Data Management Gateway 2.0 of hoger op dezelfde computer waarop de database of op een afzonderlijke computer om te voorkomen dat meedingt naar resources met de database. Data Management Gateway is een software die gegevensbronnen op ruimten met cloud services zodanig beveiligd en beheerd verbonden. Zie artikel [verplaatst gegevens tussen op gebouwen en cloud](data-factory-move-data-between-onprem-and-cloud.md) voor meer informatie over Data Management Gateway.
  
    Wanneer u de gateway hebt geïnstalleerd, installeert deze automatisch een Microsoft Cassandra ODBC-stuurprogramma verbinding maken met database Cassandra gebruikt. 

> [AZURE.NOTE] Zie [problemen oplossen gateway](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor tips over het oplossen van verbinding/gateway verband houdende vraagstukken. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om een pijpleiding die gegevens uit een database Cassandra naar een van de ondersteunde sink gegevensarchieven kopieert maken is de wizard kopiëren gebruiken. Zie [Zelfstudie: maken van een pijplijn met behulp van de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met behulp van de wizard kopiëren. 

Het volgende voorbeeld wordt een monster JSON definities die u gebruiken kunt voor het maken van een pijplijn met [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe gegevens van Cassandra database kopiëren naar Azure Blob-opslag. Gegevens kunnen echter worden gekopieerd naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.   


## <a name="sample-copy-data-from-cassandra-to-blob"></a>Voorbeeld: Gegevens kopiëren vanuit Cassandra Blob
Het monster kopieert gegevens uit een database Cassandra naar een Azure blob elk uur. De JSON-eigenschappen in deze voorbeelden gebruikt worden in voorbeelden van de volgende secties beschreven. Gegevens kunnen rechtstreeks naar de PUT vermeld in het artikel [Gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory worden gekopieerd. 

- Een gekoppelde service van het type [OnPremisesCassandra](#onpremisescassandra-linked-service-properties).
- Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Een invoer [dataset](data-factory-create-datasets.md) van het type [CassandraTable](#cassandratable-properties).
- Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Een [pijpleiding](data-factory-create-pipelines.md) met kopie activiteit die gebruikmaakt van [CassandraSource](#cassandrasource-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

**Cassandra gekoppeld service**

In dit voorbeeld wordt de service **Cassandra** gekoppeld. Zie sectie [Cassandra gekoppelde service](#onpremisescassandra-linked-service-properties) voor de eigenschappen die worden ondersteund door deze gekoppelde service.  

    {
        "name": "CassandraLinkedService",
        "properties":
        {
            "type": "OnPremisesCassandra",
            "typeProperties":
            {
                "authenticationType": "Basic",
                "host": "mycassandraserver",
                "port": 9042,
                "username": "user",
                "password": "password",
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

**Cassandra invoer dataset**

    {
        "name": "CassandraInput",
        "properties": {
            "linkedServiceName": "CassandraLinkedService",
            "type": "CassandraTable",
            "typeProperties": {
                "tableName": "mytable",
                "keySpace": "mykeyspace" 
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

De Data Factory-service **externe** instelt op **true** te informeren dat de dataset de fabriek gegevens buiten en niet wordt geproduceerd door een activiteit in de fabriek van gegevens.

**Azure Blob uitvoer dataset**

Gegevens worden weggeschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). 

    {
        "name": "AzureBlobOutput",
        "properties":
        {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties":
            {
                "folderPath": "adfgetstarted/fromcassandra"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Pipeline-exemplaar activiteit**

De pijplijn bevat een kopie activiteit die is geconfigureerd voor het gebruik van de invoer en uitvoer datasets en elk uur is gepland. **Het type** is ingesteld op **CassandraSource** in de pijplijn JSON definitie en **sink** -type is ingesteld op **BlobSink**. 

Zie [Eigenschappen van het RelationalSource](#cassandrasource-type-properties) voor de lijst met eigenschappen die worden ondersteund door de RelationalSource. 
    
    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2016-06-01T18:00:00",
            "end":"2016-06-01T19:00:00",
            "description":"pipeline with copy activity",
            "activities":[  
            {
                "name": "CassandraToAzureBlob",
                "description": "Copy from Cassandra to an Azure blob",
                "type": "Copy",
                "inputs": [
                {
                    "name": "CassandraInput"
                }
                ],
                "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "CassandraSource",
                        "query": "select id, firstname, lastname from mykeyspace.mytable"
        
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
## <a name="onpremisescassandra-linked-service-properties"></a>Eigenschappen van gekoppelde OnPremisesCassandra service

De volgende tabel bevat een beschrijving van JSON-elementen die specifiek zijn voor de service Cassandra gekoppeld.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- | 
| type | De eigenschap type moet worden ingesteld op: **OnPremisesCassandra** | Ja |
| host | Een of meer IP-adressen of hostnamen van de servers van Cassandra.<br/><br/>Een door komma's gescheiden lijst met IP-adressen of hostnamen gelijktijdig verbinding maken met alle servers opgeven. | Ja | 
| poort | De TCP-poort waarop de server Cassandra luistert voor clientverbindingen. | Nee, de standaardwaarde: 9042 |
| authenticationType | Standaard of anonieme | Ja |
| gebruikersnaam |De gebruikersnaam voor de gebruikersaccount opgeven. | Ja, als authenticationType is ingesteld op Basic. |
| wachtwoord | Wachtwoord voor de gebruikersaccount opgeven.  | Ja, als authenticationType is ingesteld op Basic. |
| gatewayName | De naam van de gateway die wordt gebruikt voor verbinding met de database op gebouwen Cassandra. | Ja |
| encryptedCredential | De referentie wordt gecodeerd met behulp van de gateway. | Nee | 

## <a name="cassandratable-properties"></a>CassandraTable eigenschappen

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van datasets. Secties zoals structuur, beschikbaarheid en het beleid van een dataset JSON zijn vergelijkbaar voor alle typen van dataset (Azure SQL Azure blob, Azure tabel, enz.).

De sectie **typeProperties** verschilt voor elk type dataset en vindt u informatie over de locatie van de gegevens in het gegevensarchief. De sectie typeProperties van de dataset van het type **CassandraTable** heeft de volgende eigenschappen

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| keyspace | De naam van de keyspace of Cassandra database-schema. | Ja (als de **query** voor **CassandraSource** niet is gedefinieerd). |
| Tabelnaam | De naam van de tabel in de database Cassandra. | Ja (als de **query** voor **CassandraSource** niet is gedefinieerd). |


## <a name="cassandrasource-type-properties"></a>CassandraSource eigenschappen
Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen van de naam, beschrijving, input en output-tabellen en -beleid zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren aan de andere kant met elk activiteitstype. Activiteit kopiëren verschillen ze afhankelijk van de typen bronnen en putten.

Als bron van het type **CassandraSource**, is de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------------- | -------- |
| query | De aangepaste query gebruiken om gegevens te lezen. | SQL-92-query of query CQL. Zie [Naslaginformatie voor CQL](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Bij het gebruik van SQL-query opgeven **keyspace name.table naam** voor de tabel die u wilt opvragen. | Nee (als de tabelnaam en keyspace op dataset zijn gedefinieerd).  |
| consistencyLevel | Het niveau van de consistentie geeft aan hoeveel replica's moeten reageren op een aanvraag tot lezen voor het retourneren van gegevens naar de clienttoepassing. Cassandra controleert het opgegeven aantal replica's voor gegevens die voldoen aan de leesaanvraag. | EEN, TWEE, DRIE, QUORUM, ALLE, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE. Zie de [consistentie van de gegevens configureren](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) voor meer informatie. | Nr. Standaardwaarde is één. |  


### <a name="type-mapping-for-cassandra"></a>Toewijzing van het type Cassandra
Type Cassandra | .NET op basis van Type
--------------- | ---------------
ASCII | Tekenreeks 
BIGINT | Int64
BLOB | Byte]
BOOLE-WAARDE | Boole-waarde
DECIMAAL | Decimaal
GETAL MET DUBBELE PRECISIE | Getal met dubbele precisie
FLOAT | Enkele
INET | Tekenreeks
INT | Int32
TEKST | Tekenreeks
TIJDSTEMPEL | Datum/tijd
TIMEUUID | GUID
UUID | GUID
VARCHAR | Tekenreeks
VARINT | Decimaal

> [AZURE.NOTE]  
> Zie [werken met Cassandra typen met behulp van virtuele tabel](#work-with-collections-using-virtual-table) sectie voor collectie-typen (kaart, set, lijst, enz.) 
> 
> Door de gebruiker gedefinieerde typen worden niet ondersteund.
> 
> De lengte van de binaire kolom en String lengte kan niet groter zijn dan 4000. 

## <a name="work-with-collections-using-virtual-table"></a>Werken met virtuele tabel-collecties
Azure Data Factory gebruikt een ingebouwd ODBC-stuurprogramma verbinding maken met en gegevens uit de database Cassandra kopiëren. Voor de collectie vormen, met inbegrip van de toewijzing, set en lijst, Normaliseert het stuurprogramma opnieuw de gegevens in de bijbehorende virtuele tabellen. Als een tabel een verzameling kolommen bevat, genereert het stuurprogramma met name de volgende virtuele tabellen:

-   Een **tabel**met dezelfde gegevens als de bestaande tabel, met uitzondering van de collectie kolommen. De basistabel wordt gebruikt dezelfde naam als de echte tabel vertegenwoordigt.
-   Een **virtuele tabel** voor elke kolom collectie de geneste gegevens breidt. De virtuele tabellen met daarin verzamelingen met als naam de naam van de bestaande tabel, een scheidingsteken "_vt_" en de naam van de kolom.

Virtuele tabellen verwijzen naar de gegevens in de bestaande tabel, waardoor het stuurprogramma voor de Gedenormaliseerde gegevens. Zie de sectie Voorbeeld voor meer informatie. Toegang tot de inhoud van Cassandra collecties door te zoeken en koppelen van de virtuele tabellen.

U kunt gebruikmaken van de [Wizard Copy](data-factory-data-movement-activities.md#data-factory-copy-wizard) intuïtief de lijst wilt weergeven van tabellen in de database van Cassandra met inbegrip van de virtuele tabellen en een voorbeeld van de gegevens in. U kunt ook samenstellen van een query in de Wizard kopiëren en valideren om het resultaat te bekijken.

### <a name="example"></a>Voorbeeld
De volgende 'ExampleTable' is bijvoorbeeld een databasetabel Cassandra met een gehele kolom primaire sleutel met de naam 'pk_int', een kolom met de naam value een lijstkolom, een kolom met hyperlinks en een set kolom (met de naam "StringSet").

pk_int | Waarde | Lijst | Kaart |   StringSet
------ | ----- | ---- | --- | --------
1 | "sample waarde 1" | ["1", "2", "3"]  | {"S1": "a", "S2": "b"} | {"A", "B", "C"}
3 | "sample waarde 3" | ['100', '101', '102', '105'] | {"S1": "t"} | {"A", "E"}

Meerdere virtuele tabellen voor dit één tabel gegenereerd door het stuurprogramma. De externe-sleutelkolommen in de virtuele tabellen verwijzen naar de primaire-sleutelkolommen in de bestaande tabel en aangeven welke echte tabelrij die correspondeert met de virtuele tabelrij. 

De eerste virtuele tabel is de tabel met de naam 'ExampleTable' wordt weergegeven in de volgende tabel. De tabel bevat dezelfde gegevens als de oorspronkelijke databasetabel, met uitzondering van de collecties die zijn weggelaten uit deze tabel en uitgevouwen in andere virtuele tabellen.

pk_int | Waarde
------ | -----
1 | "sample waarde 1"
3 | "sample waarde 3"

De volgende tabellen tonen de virtuele tabellen die de gegevens uit de kolommen van de lijst met hyperlinks en StringSet renormalize. De kolommen met namen die op '_index' of '_key eindigen' geeft de positie van de gegevens in de oorspronkelijke lijst of map aan. De kolommen met namen die op "_Waarde eindigen" de uitgebreide gegevens bevatten uit de collectie.

#### <a name="table-exampletablevtlist"></a>Tabel 'ExampleTable_vt_List':
pk_int | List_index | List_value
------ | ---------- | ----------
1 | 0 | 1
1 | 1 | 2
1 | 2 | 3
3 | 0 | 100
3 | 1 | 101
3 | 2 | 102
3 | 3 | 103

#### <a name="table-exampletablevtmap"></a>Tabel 'ExampleTable_vt_Map':
pk_int | Map_key | Map_value
------ | ------- | ---------
1 | S1 | A
1 | S2 | b
3 | S1 | t

#### <a name="table-exampletablevtstringset"></a>Tabel 'ExampleTable_vt_StringSet':
pk_int | StringSet_value
------ | ---------------
1 | A
1 | B
1 | C
3 | A
3 | E





[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]
[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Prestatie en afstemming  
Zie de [uitvoering van de activiteit & Tuning Guide](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties gevolgen van verplaatsing van gegevens (kopiëren activiteit) in Azure Data Factory en verschillende manieren optimaliseren.

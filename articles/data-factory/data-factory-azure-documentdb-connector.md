<properties 
    pageTitle="Gegevens verplaatsen/DocumentDB | Microsoft Azure" 
    description="Meer informatie over hoe gegevens verplaatsen/Azure DocumentDB collectie met Azure Data Factory" 
    services="data-factory, documentdb" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="multiple" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-documentdb-using-azure-data-factory"></a>Gegevens verplaatsen naar en van de DocumentDB met Azure Data Factory

In dit artikel wordt beschreven hoe u kunt de kopie activiteit in een fabriek Azure gegevens verplaatsen Azure DocumentDB gegevens uit een andere gegevens opslaan en gegevens van DocumentDB met een ander gegevensarchief. Dit artikel is gebaseerd op het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) activiteit kopiëren en gegevens ondersteunde archief combinaties een algemeen overzicht van de verplaatsing van gegevens biedt.

De volgende voorbeelden wordt aangegeven hoe gegevens te kopiëren en naar Azure DocumentDB en Azure Blob-opslag. Gegevens kunnen wel gekopieerde **rechtstreeks** vanuit een van de bronnen naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.  

> [AZURE.NOTE] Kopiëren van gegevens van voor-ruimten/Azure IaaS gegevens worden opgeslagen naar Azure DocumentDB en omgekeerd worden ondersteund met Data Management Gateway versie 2.1 en hoger.

## <a name="sample-copy-data-from-documentdb-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren vanuit DocumentDB Azure Blob

In het onderstaande voorbeeld ziet:

1. Een gekoppelde service van het type [DocumentDb](#azure-documentdb-linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Een invoer [dataset](data-factory-create-datasets.md) van het type [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Een [pijpleiding](data-factory-create-pipelines.md) met kopie activiteit die gebruikmaakt van [DocumentDbCollectionSource](#azure-documentdb-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Het monster worden gegevens in Azure DocumentDB gekopieerd naar Azure Blob. De JSON-eigenschappen in deze voorbeelden gebruikt worden in voorbeelden van de volgende secties beschreven.

**Azure gekoppeld DocumentDB-service:**

    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure Blob storage service gekoppeld:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure Document DB dataset ingevoerd:**

Het monster wordt ervan uitgegaan dat u beschikt over een **persoon** met de naam in een database Azure DocumentDB-collectie.
 
Instelling 'externe': 'true' en externalData informatie over de Azure Data Factory-service dat de tabel de fabriek gegevens buiten en niet geproduceerd door een activiteit in de fabriek van de gegevens op te geven.

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }


**Azure Blob uitvoer dataset:**

Gegevens worden gekopieerd naar een nieuwe blob elk uur met het pad voor de blob met granulatie uur als gevolg van de specifieke datum/tijd.

    {
      "name": "PersonBlobTableOut",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

In de collectie van de persoon in een database DocumentDB monster JSON-document: 

    {
      "PersonId": 2,
      "Name": {
        "First": "Jane",
        "Middle": "",
        "Last": "Doe"
      }
    }

DocumentDB documenten opvragen met behulp van een SQL zoals syntaxis over hiërarchische JSON-documenten. 

Voorbeeld: Person.Name.Middle Person.PersonId, Person.Name.First, zoals voornaam, als Person.Name.Last als Achternaam MiddleName selecteren van persoon

De volgende pijpleiding worden gegevens uit de collectie van de persoon in de database DocumentDB gekopieerd naar een Azure blob. Als onderdeel van de activiteit kopiëren de invoer en uitvoer zijn datasets opgegeven.  
    
    {
      "name": "DocDbToBlobPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                "nestingSeparator": "."
              },
              "sink": {
                "type": "BlobSink",
                "blobWriterAddHeader": true,
                "writeBatchSize": 1000,
                "writeBatchTimeout": "00:00:59"
              }
            },
            "inputs": [
              {
                "name": "PersonDocumentDbTable"
              }
            ],
            "outputs": [
              {
                "name": "PersonBlobTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromDocDbToBlob"
          }
        ],
        "start": "2015-04-01T00:00:00Z",
        "end": "2015-04-02T00:00:00Z"
      }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-documentdb"></a>Voorbeeld: Gegevens kopiëren vanuit Azure Blob Azure DocumentDB

In het onderstaande voorbeeld ziet:

1. Een gekoppelde service van het type [DocumentDb](#azure-documentdb-linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3. Een invoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Een uitvoer [dataset](data-factory-create-datasets.md) van het type [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Een [pijpleiding](data-factory-create-pipelines.md) met kopie activiteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) en [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).


Het monster kopieert gegevens van Azure blob met Azure DocumentDB. De JSON-eigenschappen in deze voorbeelden gebruikt worden in voorbeelden van de volgende secties beschreven.

**Azure Blob storage service gekoppeld:**
    
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure gekoppeld DocumentDB-service:**
    
    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure Blob invoer dataset:**

    {
      "name": "PersonBlobTableIn",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "MiddleName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "fileName": "input.csv",
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Azure DocumentDB uitvoer dataset:**

Het monster worden de gegevens gekopieerd naar een collectie met de naam "Persoon".

    {
      "name": "PersonDocumentDbTableOut",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "Name.First",
            "type": "String"
          },
          {
            "name": "Name.Middle",
            "type": "String"
          },
          {
            "name": "Name.Last",
            "type": "String"
          }
        ],
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

De volgende pijpleiding kopieert gegevens van Azure Blob aan de collectie van de persoon in de DocumentDB. Als onderdeel van de activiteit kopiëren de invoer en uitvoer zijn datasets opgegeven. 
    
    {
      "name": "BlobToDocDbPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "DocumentDbCollectionSink",
                "nestingSeparator": ".",
                "writeBatchSize": 2,
                "writeBatchTimeout": "00:00:00"
              }
              "translator": {
                  "type": "TabularTranslator",
                  "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
              }
            },
            "inputs": [
              {
                "name": "PersonBlobTableIn"
              }
            ],
            "outputs": [
              {
                "name": "PersonDocumentDbTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromBlobToDocDb"
          }
        ],
        "start": "2015-04-14T00:00:00Z",
        "end": "2015-04-15T00:00:00Z"
      }
    }
 
Als het monster blob-invoer als 

    1,John,,Doe

Vervolgens worden de uitvoer JSON in DocumentDB als:

    {
      "Id": 1,
      "Name": {
        "First": "John",
        "Middle": null,
        "Last": "Doe"
      },
      "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
    }
    
DocumentDB is een winkel NoSQL voor JSON-documenten waarin geneste structuren zijn toegestaan. Azure Data Factory kan de gebruiker om aan te geven hiërarchie via **nestingSeparator**, die is "." in dit voorbeeld. Met het scheidingsteken, de kopie activiteit genereert het object "Naam" met drie onderliggende elementen eerste, middelste en laatste, volgens de "Name.First", "Name.Middle" en "Name.Last" in de definitie van de tabel.

## <a name="azure-documentdb-linked-service-properties"></a>Azure DocumentDB gekoppelde Service-eigenschappen

De volgende tabel bevat een beschrijving van JSON-elementen die specifiek zijn voor de service Azure DocumentDB gekoppeld. 

| **Eigenschap** | **Beschrijving** | **Vereist** |
| -------- | ----------- | --------- |
| type | De eigenschap type moet worden ingesteld op: **DocumentDb** | Ja |
| connectionString | Geef de gegevens die nodig zijn voor de verbinding met de database DocumentDB Azure. | Ja |

## <a name="azure-documentdb-dataset-type-properties"></a>Azure DocumentDB Dataset eigenschappen

Raadpleeg het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst met secties & eigenschappen beschikbaar voor het definiëren van datasets. Secties als structuur, beschikbaarheid en het beleid van een dataset JSON zijn vergelijkbaar voor alle typen van dataset (Azure SQL Azure blob, Azure tabel, enz.).
 
De sectie typeProperties verschilt voor elk type dataset en vindt u informatie over de locatie van de gegevens in het gegevensarchief. De sectie typeProperties voor de gegevensset van het type **DocumentDbCollection** heeft de volgende eigenschappen.

| **Eigenschap** | **Beschrijving** | **Vereist** |
| -------- | ----------- | -------- |
| NaamVerzameling | De naam van de collectie van het document DocumentDB. | Ja |


Voorbeeld:

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

### <a name="schema-by-data-factory"></a>Schema door Data Factory
Voor winkels schema gegevens zoals DocumentDB afleidt de Data Factory service het schema in een van de volgende manieren:  

1.  Als u de structuur van gegevens met behulp van de eigenschap **structuur** in de definitie van de dataset, respecteert de Data Factory-service deze structuur als het schema. In dit geval als een rij geen waarde voor een kolom een null-waarde krijgt voor deze.
2.  Als u de structuur van gegevens met behulp van de eigenschap **structuur** in de definitie van de dataset niet opgeeft, wordt in de Data Factory service het schema afleidt met behulp van de eerste rij in de gegevens. In dit geval als de eerste rij bevat geen volledig schema, sommige kolommen niet ontbreekt in het resultaat van de bewerking voor het kopiëren.

Daarom voor schema-vrij-gegevensbronnen, de beste manier is om op te geven van de structuur van gegevens met behulp van de eigenschap **structuur** .

## <a name="azure-documentdb-copy-activity-type-properties"></a>Eigenschappen van Azure DocumentDB kopie activiteit

Raadpleeg het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst met secties & eigenschappen beschikbaar voor het definiëren van activiteiten. Eigenschappen van de naam, beschrijving, input en output-tabellen en -beleid zijn beschikbaar voor alle typen activiteiten.
 
**Opmerking:** De activiteit kopiëren haalt slechts één invoer en uitvoer slechts één produceert.

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit aan de andere kant variëren met elk activiteitstype en bij een kopie activiteit afhankelijk van de typen bronnen en putten.

Bij een activiteit kopiëren wanneer de bron van het type **DocumentDbCollectionSource** zijn de volgende eigenschappen beschikbaar in de sectie **typeProperties** :

| **Eigenschap** | **Beschrijving** | **Toegestane waarden** | **Vereist** |
| ------------ | --------------- | ------------------ | ------------ |
| query | De query opgeven om te lezen van gegevens. | De queryreeks die door DocumentDB worden ondersteund. <br/><br/>Voorbeeld:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` | Nee <br/><br/>Als u niet opgeeft, de SQL-instructie die wordt uitgevoerd:`select <columns defined in structure> from mycollection` 
| nestingSeparator | Speciaal teken om aan te geven dat het document is genest | Een willekeurig teken. <br/><br/>DocumentDB is een winkel NoSQL voor JSON-documenten waarin geneste structuren zijn toegestaan. Azure Data Factory kan de gebruiker om aan te geven hiërarchie via nestingSeparator, die is "." in de bovenstaande voorbeelden. Met het scheidingsteken, de kopie activiteit genereert het object "Naam" met drie onderliggende elementen eerste, middelste en laatste, volgens de "Name.First", "Name.Middle" en "Name.Last" in de definitie van de tabel. | Nee

**DocumentDbCollectionSink** ondersteunt de volgende eigenschappen:

| **Eigenschap** | **Beschrijving** | **Toegestane waarden** | **Vereist** |
| -------- | ----------- | -------------- | -------- |
| nestingSeparator | Er is een speciaal teken in de kolomnaam om aan te geven dat geneste document nodig. <br/><br/>Zoals hierboven: `Name.First` in de uitvoer produceert de tabel de volgende JSON-structuur in het document DocumentDB:<br/><br/>'Naam': {<br/>  "Eerste": "Jan"<br/>}, | Teken dat wordt gebruikt voor het scheiden van geneste niveaus.<br/><br/>Dit is de `.` (punt). | Teken dat wordt gebruikt voor het scheiden van geneste niveaus. <br/><br/>Dit is de `.` (punt). | Nee | 
| writeBatchSize | Aantal parallelle aanvragen naar DocumentDB-service documenten te maken.<br/><br/>Bij het kopiëren van gegevens uit DocumentDB met behulp van deze eigenschap kunt u de prestaties aanpassen. U kunt een betere prestaties verwachten wanneer u de writeBatchSize vergroot omdat meer parallel aanvragen voor DocumentDB worden verzonden. Moet u echter wel om te voorkomen dat die beperking kunnen genereren het foutbericht: 'Laten tarief is groot'.<br/><br/>Bandbreedtebeperking is besloten door een aantal factoren, met inbegrip van de grootte van documenten, het aantal termen in de documenten, het indexeren van beleid van de doel-collectie, enz. Voor het kopiëren van, kunt u een betere collectie (bv. S3) hebben de meeste doorvoer beschikbaar (2.500 aanvraag eenheden per seconde). | Geheel getal | Nee (standaard: 10000) |
| writeBatchTimeout | De wachttijd voor de bewerking is voltooid voordat er een optreedt time-out. | TimeSpan<br/><br/> Voorbeeld: "00: 30:00" (30 minuten). | Nee |
 
## <a name="appendix"></a>Aanhangsel
1. **Vraag:**  
    heeft de update voor de kopie activiteit ondersteuning van bestaande records?

    **Antwoord:**  
    nr.

2. **Vraag:**  
    hoe al doet een nieuwe poging van een kopie aan de deal met DocumentDB records opgehaald?

    **Antwoord:**  
    als records een 'ID'-veld hebben en de kopieerbewerking wordt geprobeerd een record met dezelfde ID in te voegen, de copy-bewerking een fout genereert.  
 
3. **Vraag:** 
    Data Factory ondersteunt het [bereik of de hash - gegevens partitioneren](https://azure.microsoft.com/documentation/articles/documentdb-partition-data/)? 

    **Antwoord:** 
    nr. 
4. **Vraag:** 
    kan ik meer dan één DocumentDB collectie voor een tabel opgeven?
    
    **Antwoord:** 
    nr. Op dit moment kan slechts één verzameling worden opgegeven.
     
## <a name="performance-and-tuning"></a>Prestatie en afstemming  
Zie de [uitvoering van de activiteit & Tuning Guide](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties gevolgen van verplaatsing van gegevens (kopiëren activiteit) in Azure Data Factory en verschillende manieren optimaliseren.

## <a name="column-mapping-with-translator-rules"></a>Kolommen toewijzen met de voorschriften van de vertaler
Kolommen toewijzen kan worden gebruikt om aan te geven hoe de kolommen in de 'structuur' van bron tabel toewijzen aan kolommen opgegeven in 'structuur' van tabel sink opgegeven. De eigenschap **Kolomtoewijzing** is beschikbaar in de sectie **typeProperties** van de activiteit van de kopie.

Kolomtoewijzing ondersteunt de volgende scenario's:

- Alle kolommen in de brontabel 'structuur' zijn toegewezen aan alle kolommen in de tabel sink 'structuur'.
- Een subset van de kolommen in de brontabel 'structuur' zijn toegewezen aan alle kolommen in de tabel sink 'structuur'.

De volgende fouten zijn en leidt tot een uitzondering:

- Minder kolommen of kolommen in de "structuur" van tabel sink dan opgegeven in de toewijzing.
- Dubbele toewijzing.
- Resultaat van de query SQL heeft geen naam van een kolom die is opgegeven in de toewijzing.

## <a name="column-mapping-samples"></a>Kolom toewijzing monsters
> [AZURE.NOTE] De onderstaande voorbeelden zijn van Azure SQL en Azure Blob maar van toepassing zijn op een gegevensarchief waarin de rechthoekige datasets. Hebt u dataset en gekoppelde servicedefinities in de volgende voorbeelden verwijzen naar gegevens in de desbetreffende gegevens aanpassen. 

### <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Voorbeeld 1: kolom Azure SQL toewijzen aan Azure blob
In dit voorbeeld wordt de invoertabel heeft een structuur en verwijst naar een SQL-tabel in een database SQL Azure.

    {
        "name": "AzureSQLInput",
        "properties": {
            "structure": 
             [
               { "name": "userid"},
               { "name": "name"},
               { "name": "group"}
             ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
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

In dit voorbeeld wordt de uitvoertabel een structuur heeft en deze verwijst naar een blob in een Azure blobopslag.

    {
        "name": "AzureBlobOutput",
        "properties":
        {
             "structure": 
              [
                    { "name": "myuserid"},
                    { "name": "myname" },
                    { "name": "mygroup"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

Hieronder vindt u de JSON voor de activiteit. De kolommen uit de gegevensbron toegewezen aan kolommen in sink (**columnMappings**) met behulp van de eigenschap **vertaler** .

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "Copy",
        "inputs":  [ { "name": "AzureSQLInput"  } ],
        "outputs":  [ { "name": "AzureBlobOutput" } ],
        "typeProperties":    {
            "source":
            {
                "type": "SqlSource"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
            }
        },
       "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

**Kolom toewijzing stroom:**

![Kolom toewijzing stroom](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow.png)

### <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Voorbeeld 2: kolom toewijzing met SQL-query in SQL Azure naar Azure blob
In dit voorbeeld wordt een SQL-query gebruikt om gegevens te extraheren uit Azure SQL in plaats van gewoon de naam van de tabel en de namen van kolommen in de sectie 'structuur'. 

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "CopyActivity",
        "inputs":  [ { "name": " AzureSQLInput"  } ],
        "outputs":  [ { "name": " AzureBlobOutput" } ],
        "typeProperties":
        {
            "source":
            {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "Translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
            }
        },
        "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

In dit geval worden de resultaten van de query eerst toegewezen aan kolommen opgegeven in 'structuur' van de bron. De kolommen van 'structuur' bron worden vervolgens toegewezen aan kolommen in 'structuur' sink met regels die zijn opgegeven in de columnMappings.  Stel dat de query als resultaat 5 kolommen, twee extra kolommen en vervolgens de instellingen in "structuur" van de bron.

**Kolom toewijzing stroom**

![Kolom toewijzing stroom-2](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow-2.png)








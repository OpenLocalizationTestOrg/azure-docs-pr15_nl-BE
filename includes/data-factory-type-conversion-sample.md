### <a name="type-conversion-sample"></a>Type conversie monster
In het volgende voorbeeld wordt voor het kopiëren van gegevens uit een Blob Azure SQL met conversie.

Stel dat de Blob-gegevensset is in CSV-indeling en 3 kolommen bevat. Een van hen is een datum/tijd-kolom met een aangepaste datum/tijd-notatie met Franse afkortingen voor de dag van de week.

U definieert de dataset Blob bron als volgt samen met de definities voor de kolommen.

    {
        "name": "AzureBlobTypeSystemInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
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
            "external": true,
            "availability":
            {
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

Gezien de SQL zou type .NET toewijzingstabel boven u de Azure SQL-tabel met het volgende schema definiëren.

| Naam van de kolom | SQL-Type |
| ----------- | -------- |
| gebruikers-id | bigint |
| naam | tekst |
| lastlogindate | datum/tijd |

Vervolgens definieert u de dataset Azure SQL als volgt. Opmerking: U hoeft niet op te geven 'structuur' sectie met informatie over het type sinds de type-informatie al in de onderliggende gegevensopslag opgegeven is.

    {
        "name": "AzureSQLOutput",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

Data factory genoemd wordt in dit geval automatisch het type conversies, met inbegrip van de datum/tijd-veld met de aangepaste datum/tijd met de fr-fr cultuur opmaken bij het verplaatsen van gegevens uit Blob Azure SQL uitvoeren.



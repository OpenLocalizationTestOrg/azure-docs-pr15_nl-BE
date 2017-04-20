<properties
   pageTitle="Gegevens laden van Azure blob-opslag in Azure SQL Data Warehouse (Azure Data Factory genoemd) | Microsoft Azure"
   description="Meer informatie over laden van gegevens met Azure Data Factory"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>
<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="load-data-from-azure-blob-storage-into-azure-sql-data-warehouse-azure-data-factory"></a>Gegevens laden van Azure blob-opslag in Azure SQL Data Warehouse (Azure Data Factory genoemd)

> [AZURE.SELECTOR]
- [Data Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

 In deze zelfstudie wordt beschreven hoe u voor het maken van een pijplijn in Azure Data Factory gegevens verplaatsen van Azure opslag Blob naar SQL Data Warehouse. U wordt met de volgende stappen uit:

+ Voorbeeldgegevens installeren in een opslag Azure Blob.
+ Verbinding maken met bronnen Azure Data Factory.
+ Maak een pijplijn gegevens verplaatsen van opslag BLOB's naar SQL Data Warehouse.

>[AZURE.VIDEO loading-azure-sql-data-warehouse-with-azure-data-factory]


## <a name="before-you-begin"></a>Voordat u begint

Zie [Inleiding tot Azure Data Factory][]om vertrouwd te raken met Azure Data Factory.

### <a name="create-or-identify-resources"></a>Maak of Identificeer bronnen

Voordat u deze zelfstudie begint, moet u de volgende bronnen.

   + **Azure opslag Blob**: in deze zelfstudie Azure opslag Blob gebruikt als gegevensbron voor de pijplijn Azure Data Factory en moeten dus een beschikbaar voor het opslaan van de voorbeeldgegevens. Als u niet al een hebt, lees hoe u [een account opslag maken][].

   + **SQL Data Warehouse**: in deze zelfstudie de gegevens vanuit Azure opslag Blob verplaatst naar SQL Data Warehouse en in dat geval moeten online een datawarehouse die wordt geladen met de voorbeeldgegevens AdventureWorksDW hebben. Als u nog geen een datawarehouse, informatie over het inrichten van [een][Create a SQL Data Warehouse]. Als u een datawarehouse hebt, maar met de voorbeeldgegevens niet inrichten, kunt u [deze handmatig laden][Load sample data into SQL Data Warehouse].

   + **Azure Data Factory**: Azure Data Factory de werkelijke werklast wordt voltooid en dus moet u een die u gebruiken kunt om samen te stellen van de pijpleiding data verkeer. Als u niet al een hebt, leren maken in stap 1 van het [aan de slag met Azure Data Factory (Data Factory-Editor)][].

   + **AZCopy**: U moet AZCopy de voorbeeldgegevens kopiëren van de lokale client naar de opslag Azure Blob. Zie de [AZCopy documentatie][]voor installatie-instructies.

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Stap 1: Voorbeeldgegevens kopiëren naar Azure opslag Blob

Zodra u alle van de stukken klaar hebt, bent u voorbeeldgegevens kopiëren naar uw opslag Azure Blob.

1. [Downloaden van de voorbeeldgegevens][]. Deze gegevens zullen nog eens drie jaar van verkoop-gegevens toevoegen aan uw voorbeeldgegevens AdventureWorksDW.

2. Gebruik deze opdracht AZCopy om de drie jaar gegevens kopiëren naar uw opslag Azure Blob.

````
AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
````


## <a name="step-2-connect-resources-to-azure-data-factory"></a>Stap 2: Verbinding maken met bronnen Azure Data Factory

De gegevens in de plaats is maken we de pijpleiding Azure Data Factory om te verplaatsen van de gegevens van Azure blob-opslag in SQL Data Warehouse.

Open de [Azure portal][] en de fabriek gegevens selecteren in het menu links om te beginnen.

### <a name="step-21-create-linked-service"></a>Stap 2.1: Gekoppelde Service maken

Uw account Azure opslag en SQL Data Warehouse koppelen aan de fabriek van uw gegevens.  

1. Eerste registratie starten door te klikken op het gedeelte 'Gekoppelde Services' van de fabriek van uw gegevens en klik vervolgens op "nieuwe gegevens worden opgeslagen." Kies een naam voor uw opslag azure onder Selecteer Azure opslag registreren als uw type en voer vervolgens uw accountnaam en Key Account.

2. Als u wilt registreren Ga SQL Data Warehouse naar de ' auteur implementeren ' sectie en selecteer nieuw-gegevensarchief en Azure SQL datawarehouse. Kopiëren en plakken in deze sjabloon en vul vervolgens de specifieke informatie.

```JSON
{
    "name": "<Linked Service Name>",
    "properties": {
        "description": "",
        "type": "AzureSqlDW",
        "typeProperties": {
             "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
         }
    }
}
```

### <a name="step-22-define-the-dataset"></a>2.2 stap: De gegevensset definiëren

We hebben de gegevenssets definiëren na het maken van de gekoppelde services.  Dit betekent hier definiëren de structuur van de gegevens die uit de opslag wordt verplaatst naar het datawarehouse.  U kunt meer lezen over het maken van

1. Start dit proces door te gaan naar de sectie 'Auteur en distribueren' van uw data factory genoemd.

2. Klik op 'Nieuwe dataset' en 'Azure Blob-opslag' uw opslag koppelen aan uw data factory genoemd.  U kunt het onderstaande script kunt u uw gegevens in Azure Blob-opslag definiëren:

```JSON
{
    "name": "<Dataset Name>",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "<linked storage name>",
        "typeProperties": {
            "folderPath": "<containter name>",
            "fileName": "FactInternetSales.csv",
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
```


3. We zullen nu ook onze dataset definiëren voor SQL Data Warehouse.  We starten op dezelfde manier, door te klikken op 'Nieuwe dataset' en Azure SQL datawarehouse.

```JSON
{
    "name": "DWDataset",
    "properties": {
        "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "FactInternetSales"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

## <a name="step-3-create-and-run-your-pipeline"></a>Stap 3: Maken en uitvoeren van de pijpleiding

Ten slotte, we zullen instellen en uitvoeren van de pijpleiding in Azure Data Factory.  Dit is de bewerking die de verplaatsing van de werkelijke gegevens invullen.  U vindt een volledige weergave van de bewerkingen die u met SQL Data Warehouse en Azure Data Factory uitvoeren kunt [hier][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].

Nu klikt u op 'Meer opdrachten' en vervolgens 'Nieuwe pijpleiding' in de sectie 'Auteur en implementeren'.  Nadat u de pijpleiding maakt, kunt u de onderstaande code om de gegevens overbrengen naar uw gegevensmagazijn:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
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
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Starten als u meer informatie wilt weergeven:

- [Azure Data Factory leren pad][].
- [Azure SQL datawarehouse Connector][]. Dit is het onderwerp voor het gebruik van Azure Data Factory met Azure SQL Data Warehouse core.


Deze onderwerpen bevatten gedetailleerde informatie over Azure Data Factory. Ze bespreken Azure SQL-Database of HDinsight, maar de informatie is ook van toepassing op Azure SQL Data Warehouse.

- [Zelfstudie: aan de slag met Azure Data Factory][] Dit is de core-zelfstudie voor het verwerken van gegevens met Azure Data Factory. In deze zelfstudie bouwt u uw eerste pijpleiding die HDInsight gebruikt om te transformeren en analyseren van weblogs op maandelijkse basis. Opmerking Er wordt geen kopie activiteit in deze zelfstudie.
- [Zelfstudie: gegevens kopiëren vanuit Azure opslag Blob met Azure SQL-Database][]. In deze zelfstudie maakt u een pijpleiding in Azure Data Factory om gegevens te kopiëren van Azure opslag Blob met Azure SQL-Database.


<!--Image references-->

<!--Article references-->
[AZCopy documentatie]: ../storage/storage-use-azcopy.md
[Azure SQL Data magazijn Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Maak een account voor opslag]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Aan de slag met Azure Data Factory (Data Factory-Editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Inleiding tot Azure Data Factory]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Zelfstudie: Gegevens kopiëren vanuit Azure opslag Blob Azure SQL-Database]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Zelfstudie: Aan de slag met Azure Data Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory learning pad]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure portal]: https://portal.azure.com
[Voorbeeldgegevens downloaden]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv

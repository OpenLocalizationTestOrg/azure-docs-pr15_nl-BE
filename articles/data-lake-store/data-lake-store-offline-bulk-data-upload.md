<properties
   pageTitle="Grote hoeveelheden gegevens uploaden naar Lake gegevensarchief offline manieren | Microsoft Azure"
   description="AdlCopy gebruiken om gegevens te kopiëren van Azure Storage Blobs naar Lake gegevensopslag"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/07/2016"
   ms.author="nitinme"/>

# <a name="use-azure-import-export-service-for-offline-copy-of-data-to-data-lake-store"></a>Azure Import Export Service gebruiken voor off line kopie van gegevens naar Lake gegevensopslag

In dit artikel leert u over het kopiëren van grote datasets (> 200GB) in een Azure Lake archief van de gegevens met behulp van off line kopie methoden, zoals [Azure Import/Export Service](../storage/storage-import-export-service.md). Met name is het bestand dat wordt gebruikt als voorbeeld in dit artikel 339,420,860,416 bytes, dat wil zeggen ongeveer 319GB op een schijf. Laten we dit bestand 319GB.tsv aanroepen.

Azure Service voor importeren/exporteren kunt u grote hoeveelheden gegevens veilig overdragen naar Azure blobopslag door verzending van harde schijven tot een Azure Datacenter.

## <a name="prerequisites"></a>Vereisten

Voordat u dit artikel hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

- **Azure opslag account**.

- **Azure gegevens Lake Analytics account (optioneel)** - Zie [aan de slag met Azure gegevens Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) voor instructies voor het maken van een account Lake gegevensarchief.


## <a name="preparing-the-data"></a>Voorbereiden van de gegevens

Voordat u de Service voor importeren/exporteren, moeten we het gegevensbestand worden overgedragen **naar de kopieën die minder dan 200 GB zijn** in grootte verbreken. Dit komt omdat het hulpprogramma voor importeren niet met bestanden die groter zijn dan 200GB werkt. Te voldoen, in deze zelfstudie we het bestand gesplitst in gedeelten van 100GB bytes. U kunt dit doen met [Cygwin](https://cygwin.com/install.html). Cygwin ondersteunt Linux-opdracht waarmee de gebruikers kunnen dit gemakkelijk doen. Voor ons geval gebruiken we de volgende opdracht.

    split -b 100m 319GB.tsv

De werking van de gesplitste bestanden gemaakt met de onderstaande namen.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Bereid u voor schijven met gegevens

Volg de instructies op [Azure Import/Export Service gebruiken](../storage/storage-import-export-service.md) (onder de sectie **uw schijven voorbereiden** ) ter voorbereiding van de vaste schijven. Hier wordt de totale stroom voor het voorbereiden van het station.

1. Aanschaffen van een vaste schijf die voldoet aan de vereisten voor Auzre Import/Export-Service moet worden gebruikt.

2. Identificatie van een Azure opslag account waar de gegevens worden gekopieerd zodra het si verzonden naar het datacenter Azure.

3. Gebruik het [Hulpprogramma voor importeren/exporteren Azure](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), een opdrachtregelprogramma. Hier is een steekproef fragment over het gebruik van het hulpprogramma.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Zie [Service voor Azure importeren/exporteren gebruiken](../storage/storage-import-export-service.md) voor meer voorbeelden fragmenten over het gebruik van het **Hulpprogramma voor importeren/exporteren van Azure**.

4. De bovenstaande opdracht wordt een journal-bestand op de opgegeven locatie gemaakt. U kunt deze journal-bestand maken van een taak importeren vanuit de [Klassieke Azure-Portal](https://manage.windowsazure.com).

## <a name="create-an-import-job"></a>Maak een taak importeren

Nu kunt u een taak importeren is volgens de aanwijzingen op [Azure Import/Export Service gebruiken](../storage/storage-import-export-service.md) (onder de sectie **maken de taak importeren** ). Voor deze taak importeren met andere details, bieden ook de journal-bestand gemaakt tijdens het voorbereiden van de schijfstations.

## <a name="physically-ship-the-disks"></a>De schijven fysiek verzenden

U kunt de schijven op een Azure Datacenter waar de gegevens worden gekopieerd via naar de Azure Storage Blobs die u hebt opgegeven tijdens het maken van de taak importeren fysiek nu verzenden. Tijdens het maken van de taak, als u ervoor gekozen om later de bijgehouden gegevens u kunt nu terug gaan naar de taak importeren en bijgewerkt het aantal.

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a>Gegevens kopiëren van Azure Storage Blobs naar Azure Lake gegevensarchief

Zodra de status van de taak importeren voltooid wordt, kunt u controleren of de gegevens zijn beschikbaar in de Azure Storage Blobs die u heeft opgegeven. Vervolgens kunt u verschillende methoden die gegevens uit de opslag Azure Blobs naar Azure Lake gegevensarchief. Zie voor alle beschikbare opties voor het uploaden van gegevens, [gegevens in het gegevensarchief Lake Ingesting](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).

In deze sectie bieden wij u de JSON-definities die u gebruiken kunt voor het maken van een pijpleiding Azure Data Factory voor het kopiëren van gegevens. U kunt deze definities JSON uit de [Azure portal](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md)gebruiken.

### <a name="source-linked-service-azure-storage-blob"></a>Bron gekoppeld Service opslag Azure Blob)

````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-store"></a>Doel gekoppeld Service (Azure Data Lake Store)

````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Input Data Set
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>Uitvoer-gegevensset
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>Pijpleiding (kopie activiteit)
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
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
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
Zie [gegevens van Azure opslag Blob met Azure Lake gegevensopslag met Azure Data Factory verplaatsen](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)voor meer informatie over het Azure Data Factory gebruiken om gegevens te verplaatsen van Azure opslag Blob en Azure Lake gegevensarchief.

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a>De bestanden in de gegevensopslag Lake Azure reconstrueren

We begonnen met een bestand dat 319GB groot is en moest het in bestanden van kleinere omvang die kunnen worden overgebracht via de Service Azure importeren/exporteren. Nu dat de gegevens in de gegevensopslag Lake Azure, kunnen wij reconstrcut het bestand naar de oorspronkelijke grootte. U kunt de volgende Azure PowerShell cmldts doen.

````
# Login to our account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Volgende stappen

- [Beveiliging van gegevens in het gegevensarchief Lake](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensarchief](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensarchief](data-lake-store-hdinsight-hadoop-use-portal.md)

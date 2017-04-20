<properties
   pageTitle="Uit Azure opslag BLOB's kopiëren naar een gegevensarchief Lake | Microsoft Azure"
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
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a>Gegevens kopiëren van Azure Storage Blobs naar Lake gegevensarchief

> [AZURE.SELECTOR]
- [Met behulp van DistCp](data-lake-store-copy-data-wasb-distcp.md)
- [Met behulp van AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)

Azure Lake gegevensarchief bevat een opdrachtregelprogramma [AdlCopy](http://aka.ms/downloadadlcopy)om gegevens te kopiëren van de volgende bronnen:

* Van opslag Azure BLOB's in Lake gegevensarchief. U AdlCopy niet gebruiken om gegevens te kopiëren van Lake gegevensarchief aan opslag Azure BLOB's.

* Tussen twee rekeningen Azure Lake gegevensarchief. 

U kunt ook het hulpprogramma AdlCopy in twee verschillende modi:

* **Zelfstandige**, waarbij het hulpprogramma Lake gegevensarchief bronnen wordt de taak uit te voeren.

* **Met behulp van een account gegevens Lake Analytics**, waar de eenheden die zijn toegewezen aan uw account gegevens Lake Analytics worden gebruikt voor het uitvoeren van de bewerking voor het kopiëren. U kunt deze optie gebruiken als u op zoek bent naar de kopie uitvoeren op een voorspelbare wijze.

##<a name="prerequisites"></a>Vereisten

Voordat u dit artikel hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

- **Azure Storage Blobs** container met enkele gegevens.

- **Azure gegevens Lake Analytics account (optioneel)** - Zie [aan de slag met Azure gegevens Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) voor instructies voor het maken van een account Lake gegevensarchief.

- **AdlCopy gereedschap**. Installeer het hulpprogramma AdlCopy uit [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-the-adlcopy-tool"></a>Syntaxis van het hulpprogramma AdlCopy

Gebruik de volgende syntaxis om te werken met het hulpprogramma AdlCopy

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern 

De parameters in de syntaxis worden hieronder beschreven:

| Optie    | Beschrijving                                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------|------------|
| Bron    | Hiermee geeft u de locatie van de brongegevens in de blob storage Azure. De bron kan een blob-container, een blob of een ander gegevensarchief Lake-account zijn.                                                                                                                                                                                                                                                                                                 |
| Doel      | Hiermee geeft u het gegevensarchief Lake doel kopiëren naar.                                                                                                                                                                                                                                                                                                                                                                |
| SourceKey | Hiermee geeft u de toegangstoets opslag voor de opslag van Azure blob-bron. Dit is alleen vereist als de bron een blob-container of een blob is.                                                                                                                                                                                                                                                                                                                                                 |
| Account   | Dit is **optioneel**. Gebruik deze optie als u Azure gegevens Lake Analytics-account gebruiken wilt voor het uitvoeren van de taak kopiëren. Als u de optie /Account in de syntaxis gebruiken maar niet een account gegevens Lake Analytics, AdlCopy een standaardaccount gebruikt de taak uit te voeren. Ook als u deze optie gebruikt, moet u toevoegen (Blob Azure opslag) bron en doel (Azure Data Lake Store) als gegevensbron voor uw account gegevens Lake Analytics.  |
| Eenheden     |  Hiermee geeft u het aantal gegevens Lake Analytics eenheden die worden gebruikt voor het project kopiëren. Deze optie is verplicht als u de gegevens Lake Analytics account opgeven met de optie **/Account** .
| Patroon   | Hiermee geeft u een regex patroon waarmee wordt aangegeven welke BLOB's of bestanden die u wilt kopiëren. AdlCopy maakt gebruik van hoofdlettergevoelige overeenkomen. De standaardpatroon dat wordt gebruikt wanneer er geen patroon wordt opgegeven, is om alle items te kopiëren. Meerdere patronen van het bestand op te geven wordt niet ondersteund.                                                                                                                                                                                                                                                                                                                                               



## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>AdlCopy (als zelfstandige) gebruiken om gegevens te kopiëren van een blob Storage Azure

1. Open een opdrachtprompt en Ga naar de map waarin AdlCopy is geïnstalleerd, doorgaans `%HOMEPATH%\Documents\adlcopy`.

2. Voer de volgende opdracht om een specifiek blob uit de Broncontainer kopiëren naar een gegevensarchief Lake:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Bijvoorbeeld:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    
    >[AZURE.NOTE] De bovenstaande syntaxis geeft het bestand wilt kopiëren naar een map in het gegevensarchief Lake. AdlCopy gereedschap maakt u een map als de naam van de opgegeven map niet bestaat.

    U wordt gevraagd de referenties invoeren voor de Azure abonnement waarmee u uw gegevensarchief Lake-account hebt. Ziet u de volgende uitvoer:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. U kunt ook de BLOB's kopiëren uit een container naar de gegevensopslag Lake rekening met de volgende opdracht:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Bijvoorbeeld:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a>AdlCopy (als zelfstandige) gebruiken om gegevens te kopiëren uit een ander gegevensarchief Lake account

U kunt ook AdlCopy gegevens te kopiëren tussen twee Lake gegevensarchief accounts gebruiken.

1. Open een opdrachtprompt en Ga naar de map waarin AdlCopy is geïnstalleerd, doorgaans `%HOMEPATH%\Documents\adlcopy`.

2. Voer de volgende opdracht om een bepaald bestand van de ene Lake gegevensarchief account naar de andere kopiëren.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Bijvoorbeeld:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

    >[AZURE.NOTE] De bovenstaande syntaxis geeft het bestand wilt kopiëren naar een map in het gegevensarchief Lake account. AdlCopy gereedschap maakt u een map als de naam van de opgegeven map niet bestaat.

    U wordt gevraagd de referenties invoeren voor de Azure abonnement waarmee u uw gegevensarchief Lake-account hebt. Ziet u de volgende uitvoer:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. De volgende opdracht kopieert alle bestanden van een specifieke map in de bron Lake gegevensarchief account naar een map in het gegevensarchief Lake account.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>AdlCopy (met gegevens Lake Analytics account) gebruiken om gegevens te kopiëren

U kunt ook uw gegevens Lake Analytics-account voor het uitvoeren van de functie AdlCopy om gegevens te kopiëren van de opslag van Azure BLOB's aan Lake gegevensarchief. Meestal gebruikt u deze optie wanneer de gegevens worden verplaatst in het bereik van gigabyte en terabyte, en u wilt de prestaties beter en voorspelbare doorvoer.

Als u uw account gegevens Lake Analytics met AdlCopy wilt kopiëren uit een opslag Azure Blob, moet de bron (Blob Azure opslag) als gegevensbron voor uw account gegevens Lake Analytics worden toegevoegd. Zie voor meer informatie over aanvullende gegevensbronnen toe te voegen aan uw account gegevens Lake Analytics [beheren gegevens Lake Analytics account gegevensbronnen](..//data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

>[AZURE.NOTE] Als u uit een gegevensarchief voor Lake Azure-account als de bron een gegevens Lake Analytics-account kopiëren wilt, hoeft u niet naar het gegevensarchief Lake account koppelen aan de account gegevens Lake Analytics. De vereiste bronarchief koppelen aan de account gegevens Lake Analytics is alleen wanneer de bron een opslag Azure-account.

Voer de volgende opdracht uit een blob Storage Azure kopiëren naar een gegevensarchief Lake rekening met gegevens Lake Analytics account:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Bijvoorbeeld:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2


De volgende opdracht uit een blob Storage Azure kopiëren naar een gegevensarchief Lake account met account gegevens Lake Analytics op dezelfde manier uitgevoerd:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>Gebruik AdlCopy voor het kopiëren van gegevens met behulp van jokertekens

In dit gedeelte vindt u informatie over het gebruik van AdlCopy om gegevens te kopiëren van een bron (in ons voorbeeld gebruiken we Azure opslag Blob) naar een bestemming gegevensarchief Lake met behulp van jokertekens. Bijvoorbeeld, kunt u de volgende stappen uit om alle bestanden met de extensie van de blob bron kopiëren naar de bestemming.

1. Open een opdrachtprompt en Ga naar de map waarin AdlCopy is geïnstalleerd, doorgaans `%HOMEPATH%\Documents\adlcopy`.

2. Voer de volgende opdracht alle bestanden met de extensie *.csv kopiëren uit een specifieke blob uit de Broncontainer naar een gegevensarchief Lake:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Bijvoorbeeld:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

    
## <a name="billing"></a>Facturering

* Als u het hulpprogramma AdlCopy als zelfstandig gebruiken wordt u gefactureerd voor egress-kosten voor het verplaatsen van gegevens, als de bron Azure opslag account niet in hetzelfde gebied, als het gegevensarchief Lake.

* Als u het hulpprogramma AdlCopy met uw account gegevens Lake Analytics gebruikt, wordt standaard [factureringstarieven gegevens Lake Analytics](https://azure.microsoft.com/pricing/details/data-lake-analytics/) toegepast.

## <a name="considerations-for-using-adlcopy"></a>Overwegingen bij het gebruik van AdlCopy

* AdlCopy (voor versie 1.0.5) ondersteunt kopiëren van gegevens uit bronnen die samen meer dan duizenden bestanden en mappen hebben. Echter als u problemen hebt met het kopiëren van een grote dataset, kunt u de bestanden/mappen verdelen over verschillende submappen en het pad naar de submappen als bron gebruiken in plaats daarvan.

## <a name="next-steps"></a>Volgende stappen

- [Beveiliging van gegevens in het gegevensarchief Lake](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensarchief](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensarchief](data-lake-store-hdinsight-hadoop-use-portal.md)

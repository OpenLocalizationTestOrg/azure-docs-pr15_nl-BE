<properties
    pageTitle="Functies voor Azure blob-opslaggegevens met behulp van Panda maken | Microsoft Azure"
    description="Het maken van functies voor gegevens die zijn opgeslagen in de container met de Panda Python pakket Azure blob."
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;garye" />

#<a name="create-features-for-azure-blob-storage-data-using-panda"></a>Functies voor Azure blob-opslaggegevens met behulp van Panda maken

Dit document bevat functies voor gegevens die zijn opgeslagen in Azure blob-container met behulp van het pakket [Pandas](http://pandas.pydata.org/) Python maken. Na het laden van de gegevens in een kader van Panda gegevens overzicht, wordt het genereren van bestaan functies met behulp van Python scripts met de indicatorwaarden en functies binning.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Dit **menu** bevat koppelingen naar onderwerpen over het maken van functies voor gegevens in diverse omgevingen. Deze taak is een stap in het [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u een account Azure blob-opslag hebt gemaakt en er uw gegevens hebt opgeslagen. Als u instructies voor het instellen van een account, Zie [een opslag Azure-account maken](../storage/storage-create-storage-account.md#create-a-storage-account)


## <a name="load-the-data-into-a-pandas-data-frame"></a>De gegevens in een frame Pandas gegevens laden
Om te verkennen en een gegevensset bewerken, moet de update gedownload van de blob-bron naar een lokaal bestand dat vervolgens kan worden geladen in een frame Pandas gegevens. Hier vindt u de volgende stappen uitvoeren om deze procedure:

1. Met de volgende voorbeeldcode Python blob-service met de gegevens van Azure blob-download. De variabele in de volgende code vervangen door uw eigen specifieke waarden:

        from azure.storage.blob import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))


2. De gegevens in een Pandas gegevensframe uit het gedownloade bestand gelezen.

        #LOCALFILE is the file path
        dataframe_blobdata = pd.read_csv(LOCALFILE)

U bent nu klaar om te verkennen, de gegevens en functies op deze dataset te genereren.

##<a name="blob-featuregen"></a>Functie voor het genereren van

De volgende twee secties hoe bestaan met de indicatorwaarden en binning functies met behulp van Python scripts genereren.

###<a name="blob-countfeature"></a>Functie generatie op basis van de waarde

Functies bestaan, kunnen als volgt worden gemaakt:

1. De verdeling van de kolom bestaan controleren:

        dataframe_blobdata['<categorical_column>'].value_counts()

2. Indicatorwaarden genereren voor elk van de kolomwaarden

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Lid worden van de indicatorkolom met de oorspronkelijke gegevensframe

            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. De oorspronkelijke variabele zelf verwijderen:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

###<a name="blob-binningfeature"></a>Functie voor het genereren van binning

Voor het genereren van binned functies, gaan we als volgt:

1. Een reeks kolommen aan bak een numerieke kolom toevoegen

        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)

2. Omzetten in een reeks van Boole-variabelen binning

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')

3. Ten slotte, deelnemen aan de dummy variabelen terug naar de oorspronkelijke gegevensframe

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

##<a name="sql-featuregen"></a>Schrijven van gegevens terug naar Azure blob en verbruikt in Azure Machine Learning

Nadat u de gegevens hebt verkend, en de noodzakelijke onderdelen hebt gemaakt, kunt u de gegevens uploaden (bemonsterd of featurized) om een Azure blob en verbruiken Azure Machine Learning met behulp van de volgende stappen: aanvullende functies kunnen worden gemaakt in de Azure Machine Learning Studio ook.
1. Het gegevensframe schrijven naar lokaal bestand

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Uploaden van de gegevens naar Azure blob als volgt:

        from azure.storage.blob import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory

        try:

        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)

        except:         
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Nu kunnen de gegevens worden gelezen uit de blob met behulp van de module Azure Machine Learning [Gegevens importeren](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) zoals in het volgende scherm:

![lezer blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)

<properties 
    pageTitle="Gegevens in Azure blobopslag met Pandas verkennen | Microsoft Azure" 
    description="Het verkennen van gegevens die zijn opgeslagen in Azure blob-container met behulp van Pandas." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-azure-blob-storage-with-pandas"></a>Gegevens in Azure blobopslag met Pandas verkennen

Dit document wordt beschreven hoe u gegevens die zijn opgeslagen in Azure blob-container met behulp van [Pandas](http://pandas.pydata.org/) Python pakket verkennen.

De volgende **menu** koppelingen naar onderwerpen over het gebruik van extra gegevens uit diverse storage-omgevingen te verkennen. Deze taak is een stap in de [Procedure voor wetenschappelijke gegevens]().

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u beschikt over:

* Een account Azure opslag gemaakt. Als u instructies nodig hebt, ziet u [een opslag Azure-account maken](../storage/storage-create-storage-account.md#create-a-storage-account)
* Uw gegevens opgeslagen in een account Azure blob-opslag. Als u instructies, Zie [Moving gegevens van en naar Azure opslag](../storage/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>De gegevens worden geladen in een DataFrame Pandas
Om te verkennen en een gegevensset bewerken, moet deze eerst worden gedownload van de blob-bron naar een lokaal bestand, kan vervolgens worden geladen in een DataFrame Pandas. Hier vindt u de volgende stappen uitvoeren om deze procedure:

1. Download de gegevens van Azure blob met het volgende Python voorbeeld met behulp van blob-service. De variabele in de volgende code vervangen door uw eigen specifieke waarden: 

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

##<a name="blob-dataexploration"></a>Voorbeelden van verkennen met behulp van Pandas

Hier volgen enkele voorbeelden van manieren om gegevens met behulp van Pandas verkennen:

1. Controleer het **aantal rijen en kolommen** 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. **Controleren** van de eerste of laatste paar **rijen** in de volgende gegevensset:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Het **gegevenstype** dat elke kolom is geïmporteerd met behulp van de volgende code controleren
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. De **basis-statistieken** voor de kolommen in de gegevens als volgt controleren
 
        dataframe_blobdata.describe()
    
5. Het aantal items voor elke waarde in de kolom als volgt bekijken

        dataframe_blobdata['<column_name>'].value_counts()

6. **Ontbrekende waarden tellen** en het werkelijke aantal items in elke kolom met de volgende voorbeeldcode

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Als er voor een bepaalde kolom **waarden ontbreken** in de gegevens, kunt u ze als volgt neer:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Ontbrekende waarden vervangen door een andere manier is met de modusfunctie:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Een **histogram** grafiek uitzetten van de verdeling van een variabele met variabel aantal opslaglocaties maken 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Bekijk de **correlaties** tussen variabelen met behulp van een scatterplot of met behulp van de ingebouwde correlatiefunctie

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

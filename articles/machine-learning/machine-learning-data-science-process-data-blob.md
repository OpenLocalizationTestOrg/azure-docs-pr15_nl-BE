<properties 
    pageTitle="Azure blob-gegevens met geavanceerde analytics | Microsoft Azure" 
    description="De procesgegevens in Azure Blob-opslag." 
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
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Azure blob-gegevens met geavanceerde analytics verwerken

Dit document omvat verkennen gegevens en functies voor genereren van gegevens die zijn opgeslagen in Azure Blob-opslag. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>De gegevens in een frame Pandas gegevens laden
Verkennen en manipuleren van een dataset, moet de update gedownload van de blob-bron naar een lokaal bestand dat vervolgens kan worden geladen in een frame Pandas gegevens. Hier vindt u de volgende stappen uitvoeren om deze procedure:

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


##<a name="blob-dataexploration"></a>Verkennen

Hier volgen enkele voorbeelden van manieren om gegevens met behulp van Pandas verkennen:

1. Controleer het aantal rijen en kolommen 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. Inspecteer de eerste of laatste paar rijen in de gegevensset zoals hieronder:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Het gegevenstype dat elke kolom is geïmporteerd met behulp van de volgende code controleren
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. De elementaire statistieken voor de kolommen in de gegevens als volgt controleren
 
        dataframe_blobdata.describe()
    
5. Het aantal items voor elke waarde in de kolom als volgt bekijken

        dataframe_blobdata['<column_name>'].value_counts()

6. Ontbrekende waarden ten opzichte van het werkelijke aantal items in elke kolom met de volgende voorbeeldcode tellen

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Als u ontbrekende waarden voor een bepaalde kolom in de gegevens hebt, kunt u deze als volgt neerzetten:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Ontbrekende waarden vervangen door een andere manier is met de modusfunctie:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Een histogram grafiek uitzetten van de verdeling van een variabele met variabel aantal opslaglocaties maken 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Bekijk de correlaties tussen variabelen met behulp van een scatterplot of met behulp van de ingebouwde correlatiefunctie

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
    
    
##<a name="blob-featuregen"></a>Functie voor het genereren van
    
We kunnen genereren functies gebruikt Python als volgt:

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

3. Nu de gegevens kunnen worden gelezen uit de blob met de Azure Machine Learning [Gegevens importeren] [ import-data] module, zoals in het volgende scherm:
 
![lezer blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 

<properties 
    pageTitle="Voorbeeldgegevens in Azure blob-opslag | Microsoft Azure" 
    description="Voorbeeldgegevens in Azure Blob-opslag" 
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

#<a name="heading"></a>Voorbeeldgegevens in Azure blob-opslag


Dit document heeft betrekking op de bemonstering gegevens opgeslagen in Azure blob-opslag door bestand voorgeprogrammeerd te downloaden en vervolgens bemonstering met behulp van procedures die zijn geschreven in Python.

**Waarom uw gegevens controleren?**
Als de gegevensset die u wilt analyseren groot is, is het meestal verstandig down-steekproef van de gegevens om deze naar een kleiner, maar representatieve en meer overzichtelijke omvang. Dit vereenvoudigt het wat gegevens, de exploratie en de functie engineering. Haar rol in het proces van Cortana Analytics is het snel maken van prototypen van de functies voor het verwerken van gegevens en computer modellen leren inschakelen.

Het **menu** onder koppelingen naar onderwerpen waarin wordt beschreven hoe u de voorbeeldgegevens uit diverse storage-omgevingen. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Deze taak bemonstering is een stap in het [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="download-and-down-sample-data"></a>Download en down-voorbeeldgegevens
1. De gegevens uit de Azure blobopslag met behulp van de blob-service op de volgende voorbeeldcode Python downloaden: 

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

2. Gegevens in een Pandas gegevensframe uit het bestand gedownload hierboven gelezen.

        import pandas as pd

        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. Down-monster, de gegevens met behulp van de `numpy`van `random.choice` als volgt:

        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

Nu kunt u met de bovenstaande gegevensframe met het monster van 1 procent voor verdere exploratie en de functie voor het genereren van werken.

##<a name="heading"></a>Gegevens uploaden en in Azure Machine leren lezen

U kunt de volgende voorbeeldcode down-voorbeeld van de gegevens en daarmee rechtstreeks in Azure ML:

1. Het gegevensframe naar een lokaal bestand schrijven

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Het lokale bestand uploaden naar een Azure blob met de volgende code:

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
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. De gegevens lezen van de Azure blob Azure ML [Gegevens importeren](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) gebruiken zoals in de onderstaande afbeelding:
 
![lezer blob](./media/machine-learning-data-science-sample-data-blob/reader_blob.png)

 

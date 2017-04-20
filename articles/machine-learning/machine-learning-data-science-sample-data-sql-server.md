<properties 
    pageTitle="Voorbeeld van gegevens in SQL Server op Azure | Microsoft Azure" 
    description="Voorbeeldgegevens in SQL Server op Azure" 
    services="machine-learning" 
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

#<a name="heading"></a>Voorbeeldgegevens in SQL Server op Azure


Dit document wordt opgeslagen in SQL Server op Azure voorbeeldgegevens met SQL of de programmeertaal Python. Ook ziet u hoe bemonsterde om gegevens te verplaatsen naar Azure Machine leren door te slaan in een bestand en vervolgens lezen naar Azure Machine Learning Studio uploaden naar een Azure blob.

De bemonstering Python maakt de [pyodbc](https://code.google.com/p/pyodbc/) -bibliotheek voor ODBC-verbinding met SQL Server op Azure en de bibliotheek [Pandas](http://pandas.pydata.org/) voor de monsterneming.

>[AZURE.NOTE] De SQL-code van de voorbeelden in dit document wordt ervan uitgegaan dat de gegevens in een SQL-Server op Azure. Als dat niet het geval is, Zie [gegevens naar SQL Server op Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) onderwerp voor instructies over hoe u uw gegevens naar SQL Server op Azure.

**Waarom uw gegevens controleren?**
Als de gegevensset die u wilt analyseren groot is, is het meestal verstandig down-steekproef van de gegevens om deze naar een kleiner, maar representatieve en meer overzichtelijke omvang. Dit vereenvoudigt het wat gegevens, de exploratie en de functie engineering. Haar rol in het [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) is het snel maken van prototypen van de functies voor het verwerken van gegevens en computer modellen leren inschakelen.

Het **menu** onder koppelingen naar onderwerpen waarin wordt beschreven hoe u de voorbeeldgegevens uit diverse storage-omgevingen. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Deze taak bemonstering is een stap in het [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

##<a name="SQL"></a>Met behulp van SQL

Deze sectie beschrijft verschillende methoden voor het uitvoeren van eenvoudige steekproeven ten opzichte van de gegevens in de database met behulp van SQL. Kies een methode op basis van de gegevensgrootte van uw en de distributie.

De onderstaande items hoe newid in SQL Server gebruiken voor het uitvoeren van de bemonstering. Welke methode u kiest, hangt af van hoe willekeurig u wilt dat het monster (pk_id in de onderstaande voorbeeldcode wordt aangenomen dat een automatisch gegenereerde primaire sleutel).

1. Minder strikte aselecte steekproef

        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())

2. Meer aselecte steekproef 

        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

TABLESAMPLE kan worden gebruikt voor de bemonstering alsmede aangetoond hieronder. Dit kan een betere benadering zijn als uw gegevens groot is (ervan uitgaande dat de gegevens op verschillende pagina's niet is gecorreleerd) en voor de query uit te voeren in een redelijke termijn.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

>[AZURE.NOTE] U kunt verkennen en functies van deze steekproef gegevens opslaan in een nieuwe tabel genereren


###<a name="sql-aml"></a>Verbinding maken met Azure Machine Learning

U kunt de voorbeeldquery's hierboven rechtstreeks in de Azure Machine Learning [Gegevens importeren] [ import-data] module down steekproeven van de gegevens op elk gewenst moment en zet het in een experiment Azure Machine Learning. Hieronder vindt u een schermafdruk van het gebruik van de module reader voor het lezen van de bemonsterde gegevens:
   
![lezer sql][1]

##<a name="python"></a>Met behulp van de programmeertaal Python 

In deze sectie wordt beschreven met behulp van de [bibliotheek pyodbc](https://code.google.com/p/pyodbc/) vast te stellen een ODBC verbinding maken met een SQL server-database in Python. De verbindingsreeks is als volgt: (vervangen door servername, dbname, gebruikersnaam en wachtwoord voor uw configuratie):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

De bibliotheek [Pandas](http://pandas.pydata.org/) in Python biedt een uitgebreide reeks gegevensstructuren en hulpprogramma's voor gegevensanalyse voor gegevensmanipulatie Python programmeren. De onderstaande code wordt een voorbeeld van 0,1% van de gegevens uit een tabel in Azure SQL-database in een Pandas data:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Nu kunt u werken met gegevens in het gegevensframe Pandas bemonsterde. 

###<a name="python-aml"></a>Verbinding maken met Azure Machine Learning

U kunt de volgende voorbeeldcode gedownsampled gegevens opslaan in een bestand en uploadt naar een Azure blob. De gegevens in de blob kan worden rechtstreeks lezen in een Experiment Azure Machine Learning met behulp van de [Gegevens importeren] [ import-data] module. De stappen zijn als volgt: 

1. Het frame pandas gegevens schrijven naar een lokaal bestand

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Lokaal bestand uploaden naar Azure blob

        from azure.storage import BlobService
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

3. Gegevens lezen van Azure blob Azure Machine Learning [Gegevens importeren] met[ import-data] module, zoals wordt weergegeven in het scherm schermafbeelding hieronder:
 
![lezer blob][2]

## <a name="the-team-data-science-process-in-action-example"></a>Het Team gegevens wetenschap proces in actie-voorbeeld

Zie voor een overzicht van end-to-end voorbeeld van het Team gegevens wetenschap proces een met een openbare dataset [Team gegevens wetenschap proces in actie: met behulp van SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

 [import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

<properties 
    pageTitle="Verwerken van gegevens uit SQL Azure | Microsoft Azure" 
    description="Procesgegevens uit SQL Azure" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Verwerken van gegevens in SQL Server virtuele Machine op Azure

Dit document wordt beschreven hoe u de gegevens en functies voor gegevens die zijn opgeslagen in een SQL Server-VM in Azure genereren. U kunt hiervoor gegevens worsteling met SQL of met een programmeertaal als Python.


> [AZURE.NOTE] Het voorbeeld SQL-instructies in dit document wordt ervan uitgegaan dat er gegevens in SQL Server. Dit niet het geval is, raadpleegt u de wolk wetenschap proces gegevenstoewijzing voor meer informatie over hoe u uw gegevens naar SQL Server.

##<a name="SQL"></a>Met behulp van SQL

De volgende gegevens wrangling taken in deze sectie met SQL worden beschreven:

1. [Verkennen](#sql-dataexploration)
2. [Functie voor het genereren van](#sql-featuregen)

###<a name="sql-dataexploration"></a>Verkennen
Hier volgen een paar voorbeelden SQL-scripts kunnen worden gebruikt voor het verkennen van de opgeslagen gegevens in SQL Server.


> [AZURE.NOTE] U kunt bijvoorbeeld een praktische [RDAM Taxi dataset](http://www.andresmh.com/nyctaxitrips/) gebruiken en verwijzen naar de IPNB met de titel [RDAM gegevens worsteling met IPython laptop en SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) voor een overzicht van de end-to-end.

1. Het aantal waarnemingen per dag

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Ophalen van de niveaus in een kolom bestaan

    `select  distinct <column_name> from <databasename>`

3. Het aantal niveaus in een combinatie van twee bestaan kolommen ophalen 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. De verdeling voor numerieke kolommen ophalen

    `select <column_name>, count(*) from <tablename> group by <column_name>`


###<a name="sql-featuregen"></a>Functie voor het genereren van

In deze sectie worden beschreven manieren voor het genereren van SQL-functies:  

1. [Functie genereren op basis van aantal](#sql-countfeature)
2. [Functie voor het genereren van binning](#sql-binningfeature)
3. [De functies rolt van een enkele kolom](#sql-featurerollout)


> [AZURE.NOTE] Zodra u extra functies genereert, kunt u kolommen toevoegen aan de bestaande tabel of maak een nieuwe tabel met de primaire sleutel, die kan worden gekoppeld met de oorspronkelijke tabel en extra functies. 

###<a name="sql-countfeature"></a>Functie genereren op basis van aantal

In dit document wordt op twee manieren voor het genereren van het aantal functies. De eerste methode maakt gebruik van Voorwaardelijke som en de tweede methode maakt gebruik van de component where'. Deze kunnen vervolgens worden gekoppeld met de oorspronkelijke tabel (met behulp van primaire-sleutelkolommen) als u wilt dat het aantal functies naast de oorspronkelijke gegevens.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

###<a name="sql-binningfeature"></a>Functie voor het genereren van binning

In het volgende voorbeeld ziet u een numerieke kolom die kan worden gebruikt als een functie in plaats daarvan het genereren van binned functies door binning (met 5 bakken):

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


###<a name="sql-featurerollout"></a>De functies rolt van een enkele kolom

We voorbeelden van het uitrollen van een enkele kolom in een tabel voor het genereren van extra functies in deze sectie. In het voorbeeld wordt verondersteld dat er een kolom breedtegraad of lengtegraad in de tabel waaruit u wilt genereren van functies.

Hier is een korte primer op locatiegegevens latitude/lengtegraad (resources voorzien van stackoverflow [meten van de nauwkeurigheid van de breedtegraad en lengtegraad?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). Dit is handig om te begrijpen voordat u featurizing het locatieveld:

- Het teken vertelt ons of we Noord of Zuid, Oost of west op de hele wereld.
- Een andere waarde dan nul honderden cijfers vertellen we maken gebruik van de geografische lengte, geen latitude!
- De tientallen cijfer geeft een positie op ongeveer 1000 kilometer. Het biedt ons nuttige informatie over welk continent of Oceaan zijn we op.
- Het cijfer eenheden (één decimale graad) resulteert in een positie tot 111 kilometer (60 zeemijl, ongeveer 69 mijl). Het kan ons ongeveer wat grote rechtsgebied zijn we in.
- De eerste decimaal is de moeite waard op 11.1 km: dat de positie van één grote stad van een nabijgelegen grote stad kunt onderscheiden.
- De tweede decimaal is de moeite waard maximaal 1,1 km: het village op één van de volgende scheiden.
- De derde decimaal is waard tot 110 m: dat het grote agrarische veld of institutionele campus kunt identificeren.
- De vierde decimaal is waard maximaal 11 m: dat het een perceel grond kunt identificeren. Het is vergelijkbaar met de gemiddelde nauwkeurigheid van een niet-gecorrigeerde GPS eenheid zonder interferentie.
- De vijfde decimaal is ter waarde van maximaal 1,1 m: bomen van elkaar onderscheiden. Nauwkeurigheid tot dit niveau met commerciële GPS-eenheden kan alleen worden bereikt met differential correctie.
- De zesde decimaal is waard maximaal 0.11 m: dat kunt u dit voor het opmaken van de structuren in detail voor het ontwerpen van landschappen, wegen bouwen. Moet meer dan goed genoeg voor de bewegingen van glaciers en rivieren. Dit kan worden bereikt door middel van hele maatregelen met GPS, zoals differentially gecorrigeerde GPS.

Informatie over de locatie kan worden featurized als volgt uit de regio, de locatie en de plaats van elkaar te scheiden. Opmerking u kunt ook een eindpunt REST zoals beschikbare Bing Maps-API aanroepen bij het [Zoeken naar een locatie door](https://msdn.microsoft.com/library/ff701710.aspx) de regio/district informatie.

    select 
        <location_columnname>
        ,round(<location_columnname>,0) as l1       
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

De bovenstaande functies op basis van locatie kunnen verder worden gebruikt voor het genereren van het aantal extra functies zoals eerder beschreven. 


> [AZURE.TIP] Via programmacode kunt u de records met de taal van uw keuze. Wellicht moet u de gegevens invoegen in stukken schrijven efficiëntie (voor een voorbeeld van hoe dit doen met behulp van pyodbc, Zie [voorbeeld HelloWorld A toegang tot SQL Server met python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)) te verbeteren. Een ander alternatief is het invoegen van gegevens in de database met het [hulpprogramma BCP](https://msdn.microsoft.com/library/ms162802.aspx).

###<a name="sql-aml"></a>Verbinding maken met Azure Machine Learning

De nieuwe functie kan als een kolom toegevoegd aan een bestaande tabel of opgeslagen in een nieuwe tabel en samengevoegd met de oorspronkelijke tabel voor machine learning. Functies kunnen worden gegenereerd of als u al hebt gemaakt, geopend met de [Gegevens importeren] [ import-data] module in Azure Machine Learning, zoals hieronder wordt weergegeven:

![azureml lezers][1] 

##<a name="python"></a>Met behulp van een programmeertaal als Python

Python is gegevens verkennen en functies genereren wanneer de gegevens in SQL Server vergelijkbaar met verwerken van gegevens in Azure blob met Python zoals beschreven in het [proces Azure Blob-gegevens in u data science-omgeving](machine-learning-data-science-process-data-blob.md). De gegevens moeten in een frame pandas gegevens uit de database worden geladen en vervolgens verder kunnen worden verwerkt. We leggen het proces van het verbinden met de database en het laden van de gegevens in het kader van de gegevens in deze sectie.

Indeling van de volgende verbindingsreeks kan verbinding maken met een SQL Server-database vanuit Python via pyodbc (servernaam vervangen, dbname, gebruikersnaam en wachtwoord met uw specifieke waarden) worden gebruikt:

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

De [Pandas library](http://pandas.pydata.org/) in Python biedt een uitgebreide reeks gegevensstructuren en hulpprogramma's voor gegevensanalyse voor gegevensmanipulatie Python programmeren. De volgende code wordt de resultaten geretourneerd uit een SQL Server-database in een kader Pandas gegevens:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nu kunt u werken met het gegevensframe Pandas zoals bedoeld in artikel [proces Azure Blob-gegevens in u data science-omgeving](machine-learning-data-science-process-data-blob.md).

## <a name="azure-data-science-in-action-example"></a>Wetenschap in actie voorbeeld Azure gegevens

Zie voor een voorbeeld van de end-to-end scenario van het Azure gegevens wetenschap proces met behulp van een openbare dataset, [Azure gegevens wetenschap proces in actie](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 

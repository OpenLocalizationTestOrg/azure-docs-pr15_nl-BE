<properties 
    pageTitle="Gegevens in SQL Server virtuele Machine verkennen op Azure | Microsoft Azure" 
    description="Het verkennen van gegevens die zijn opgeslagen in een SQL Server-VM in Azure." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Gegevens in SQL Server virtuele Machine op Azure verkennen


Dit document wordt beschreven hoe u gegevens die zijn opgeslagen in een SQL Server-VM in Azure verkennen. U kunt hiervoor gegevens worsteling met SQL of met een programmeertaal als Python.

De volgende **menu** koppelingen naar onderwerpen over het gebruik van extra gegevens uit diverse storage-omgevingen te verkennen. Deze taak is een stap in de Cortana Analytics proces (CAP).

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


> [AZURE.NOTE] Het voorbeeld SQL-instructies in dit document wordt ervan uitgegaan dat er gegevens in SQL Server. Als het niet, Ga naar de cloud wetenschap proces gegevenstoewijzing voor meer informatie over hoe u uw gegevens naar SQL Server.



## <a name="sql-dataexploration"></a>SQL-gegevens met SQL-scripts

Hier volgen een paar voorbeelden SQL-scripts kunnen worden gebruikt voor het verkennen van de opgeslagen gegevens in SQL Server.

1. Het aantal waarnemingen per dag

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Ophalen van de niveaus in een kolom bestaan

    `select  distinct <column_name> from <databasename>`

3. Het aantal niveaus in een combinatie van twee bestaan kolommen ophalen 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. De verdeling voor numerieke kolommen ophalen

    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [AZURE.NOTE] U kunt bijvoorbeeld een praktische [RDAM Taxi dataset](http://www.andresmh.com/nyctaxitrips/) gebruiken en verwijzen naar de IPNB met de titel [RDAM gegevens worsteling met IPython laptop en SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) voor een overzicht van de end-to-end.

##<a name="python"></a>SQL-gegevens met Python verkennen

Python is gegevens verkennen en functies genereren wanneer de gegevens in SQL Server vergelijkbaar met verwerken van gegevens in Azure blob met Python, zoals beschreven in het [proces Azure Blob-gegevens in de omgeving van de wetenschappelijke gegevens](machine-learning-data-science-process-data-blob.md). De gegevens moeten worden geladen uit de database in een pandas DataFrame en vervolgens verder kunnen worden verwerkt. We leggen het proces van het verbinden met de database en het laden van de gegevens in de DataFrame in deze sectie.

Indeling van de volgende verbindingsreeks kan verbinding maken met een SQL Server-database vanuit Python via pyodbc (servernaam vervangen, dbname, gebruikersnaam en wachtwoord met uw specifieke waarden) worden gebruikt:

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

De [Pandas library](http://pandas.pydata.org/) in Python biedt een uitgebreide reeks gegevensstructuren en hulpprogramma's voor gegevensanalyse voor gegevensmanipulatie Python programmeren. De volgende code wordt de resultaten geretourneerd uit een SQL Server-database in een kader Pandas gegevens:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nu kunt u werken met de Pandas DataFrame zoals besproken in het onderwerp [proces Azure Blob-gegevens in de omgeving van de wetenschappelijke gegevens](machine-learning-data-science-process-data-blob.md).

## <a name="cortana-analytics-process-in-action-example"></a>Cortana Analytics proces in actie-voorbeeld

Zie voor een voorbeeld van de end-to-end scenario van het Cortana Analytics proces met behulp van een openbare dataset, [het Team gegevens wetenschap proces in actie: met behulp van SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

 

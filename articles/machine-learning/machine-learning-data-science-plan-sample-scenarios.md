<properties
    pageTitle="Scenario's voor de geavanceerde analytische processen en technologie in Azure Machine Learning | Microsoft Azure"
    description="Selecteer de juiste scenario's voor geavanceerde voorspellende analytics met het Team gegevens wetenschap proces."
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
    ms.author="bradsev" />


# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Scenario's voor geavanceerde analytics in Azure Machine Learning

In dit artikel worden de verschillende gegevensbronnen monster en doel-scenario's die kunnen worden verwerkt door het [Team gegevens wetenschap proces (TDSP)](data-science-process-overview.md). De TDSP biedt een systematische aanpak voor teams samen te werken aan het maken van intelligente toepassingen. De hier gepresenteerde scenario illustreren opties die beschikbaar zijn in de werkstroom gegevens verwerken die afhankelijk van de gegevenskenmerken, bronlocaties en opslagplaatsen in Azure doel zijn.

De **beslissingsstructuur** voor het voorbeeldscenario's te selecteren die geschikt is voor uw gegevens en doelstelling wordt weergegeven in de laatste sectie.

>[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Elk van de volgende secties vindt een voorbeeldscenario. Voor elk scenario, een mogelijke data science of geavanceerde analytics stroom en ondersteuning van Azure bronnen worden vermeld.

>[AZURE.NOTE]**Voor alle van de volgende scenario's moet u:**
><br/>
>* [Maak een account voor opslag](../storage/storage-create-storage-account.md)
><br/>
>* [Een werkruimte Azure Machine Learning maken](machine-learning-create-workspace.md)


## <a name="smalllocal"></a>Scenario \#1: kleine tot middelgrote in tabelvorm dataset in een lokale bestanden

![Kleine tot middelgrote lokale bestanden][1]

#### <a name="additional-azure-resources-none"></a>Aanvullende bronnen voor Azure: geen

1.  Aanmelden bij de [Azure Machine Learning Studio](https://studio.azureml.net/).

2.  Upload een dataset.

3.  Bouw een Azure Machine Learning experiment stroom beginnen met geüploade gegevensset (s).

## <a name="smalllocalprocess"></a>Scenario \#2: kleine tot middelgrote dataset van lokale bestanden die moeten worden verwerkt

![Kleine tot middelgrote lokale bestanden met verwerking][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Aanvullende bronnen voor Azure: Azure Virtual Machine (laptop IPython server)

1.  Maak een Azure Virtual Machine met IPython-laptop.

2.  Gegevens uploaden naar een container Azure opslag.

3.  Vooraf verwerken en gegevens in de IPython-laptop, toegang krijgen tot gegevens uit Azure opslag container schoon.

4.  Transformatie van gegevens gereinigd, in tabelvorm.

5.  Getransformeerde gegevens opslaan in Azure BLOB's.

6.  Aanmelden bij de [Azure Machine Learning Studio](https://studio.azureml.net/).

7.  De gegevens lezen van Azure BLOB's met behulp van de [Gegevens importeren] [ import-data] module.

8. Bouw een Azure Machine Learning experiment stroom vanaf geconsumeerde gegevensset (s).

## <a name="largelocal"></a>Scenario \#3: grote dataset van lokale bestanden, gericht op Azure BLOB's

![Grote lokale bestanden][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Aanvullende bronnen voor Azure: Azure Virtual Machine (laptop IPython server)

1.  Maak een Azure Virtual Machine met IPython-laptop.

2.  Gegevens uploaden naar een container Azure opslag.

3.  Vooraf verwerken en gegevens in de IPython-laptop, toegang tot gegevens van Azure BLOB's schoon.

4.  Transformatie van gegevens gereinigd, in tabelvorm, als dat nodig is.

5.  Gegevens en functies maken als nodig is.

6.  Een voorbeeld van een kleine tot middelgrote gegevens extraheren.

7.  De bemonsterde gegevens opslaan in Azure BLOB's.

8. Aanmelden bij de [Azure Machine Learning Studio](https://studio.azureml.net/).

9. De gegevens lezen van Azure BLOB's met behulp van de [Gegevens importeren] [ import-data] module.

10. Bouw Azure Machine Learning experiment stroom vanaf geconsumeerde gegevensset (s).


## <a name="smalllocaltodb"></a>Scenario \#4: kleine tot middelgrote dataset van lokale bestanden, gericht op SQL Server in een Machine Azure Virtal

![Kleine tot middelgrote lokale bestanden naar SQL DB in Azure][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Aanvullende bronnen voor Azure: Azure Virtual Machine (SQL Server / laptop IPython server)

1.  Maak een Azure VM met SQL Server + IPython-laptop.

2.  Gegevens uploaden naar een container Azure opslag.

3.  Vooraf verwerken en gegevens in Azure opslag container met IPython laptop opschonen.

4.  Transformatie van gegevens gereinigd, in tabelvorm, als dat nodig is.

5.  Gegevens opslaan in een VM-lokale bestanden (IPython laptop op VM wordt uitgevoerd, lokale stations verwijzen naar de VM-stations).

6.  Gegevens naar een SQL Server-database op een Azure VM laden.

    Optie \#1: met behulp van SQL Server Management Studio.

    - Aanmelding bij SQL Server VM
    - Start SQL Server Management Studio.
    - Database- en tabellen maken.
    - Gebruik een van de meeste methoden voor het laden van de gegevens uit de VM-lokale bestanden importeren.

    Optie \#2: met behulp van IPython-laptop – niet aan te raden voor middelgrote en grote gegevenssets<!-- -->    
    - ODBC-verbindingsreeks gebruiken voor toegang tot SQL Server op een VM.
    - Database- en tabellen maken.
    - Gebruik een van de meeste methoden voor het laden van de gegevens uit de VM-lokale bestanden importeren.

7.  Gegevens verkennen, maken van functies als nodig is. Houd er rekening mee dat de functies niet hoeven te worden gerealiseerd in de databasetabellen. Opmerking alleen de query nodig om ze te maken.

8. Een besluit over een steekproefgrootte gegevens indien nodig en/of gewenst.

9. Aanmelden bij de [Azure Machine Learning Studio](https://studio.azureml.net/).

10. Lees de gegevens rechtstreeks vanuit de SQL Server- [Gegevens importeren] met[ import-data] module. Plak de nodige query haalt velden en functies maakt en monsters indien nodig de [Gegevens importeren] in[ import-data] query.

11. Bouw Azure Machine Learning experiment stroom vanaf geconsumeerde gegevensset (s).

## <a name="largelocaltodb"></a>Scenario \#5: grote gegevensset in een lokale bestanden doel SQL Server in Azure VM

![Grote lokale bestanden naar SQL DB in Azure][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Aanvullende bronnen voor Azure: Azure Virtual Machine (SQL Server / laptop IPython server)

1.  Maak een Azure VM met SQL Server en de server IPython-laptop.

2.  Gegevens uploaden naar een container Azure opslag.

3.  (Optioneel) Vooraf verwerken en gegevens opschonen.

    een.  Vooraf verwerken en gegevens in de IPython-laptop, toegang tot gegevens van Azure BLOB's schoon.

    b.  Transformatie van gegevens gereinigd, in tabelvorm, als dat nodig is.

    c.  Gegevens opslaan in een VM-lokale bestanden (IPython laptop op VM wordt uitgevoerd, lokale stations verwijzen naar de VM-stations).

4.  Gegevens naar een SQL Server-database op een Azure VM laden.

    een.  Aanmelding bij SQL Server VM.

    b.  Als gegevens niet reeds opgeslagen, gegevensbestanden downloaden vanuit Azure opslag container naar de map local VM.

    c.  Start SQL Server Management Studio.

    d.  Database- en tabellen maken.

    e.  Gebruik een van de bulk importeren methoden om de gegevens te laden.

    f.  Als tabel-joins vereist zijn, worden de indexen sneller joins maken.

     > [AZURE.NOTE] Voor sneller laden van grote omvang is het aanbevolen dat u gepartitioneerde tabellen maken en de gegevens gelijktijdig bulkimport. Zie [Parallelle gegevens importeren naar gepartitioneerd SQL-tabellen](machine-learning-data-science-parallel-load-sql-partitioned-tables.md)voor meer informatie.

5.  Gegevens verkennen, maken van functies als nodig is. Houd er rekening mee dat de functies niet hoeven te worden gerealiseerd in de databasetabellen. Opmerking alleen de query nodig om ze te maken.

6.  Een besluit over een steekproefgrootte gegevens indien nodig en/of gewenst.

7.  Aanmelden bij de [Azure Machine Learning Studio](https://studio.azureml.net/).

8. Lees de gegevens rechtstreeks vanuit de SQL Server- [Gegevens importeren] met[ import-data] module. Plak de nodige query haalt velden en functies maakt en monsters indien nodig de [Gegevens importeren] in[ import-data] query.

9. Eenvoudige Azure Machine Learning experiment stroom beginnen met geüploade dataset

## <a name="largedbtodb"></a>Scenario \#6: grote gegevensset in een SQL Server-database op-prem, gericht op SQL Server in een Azure Virtual Machine

![Grote SQL DB op-prem naar SQL DB in Azure][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Aanvullende bronnen voor Azure: Azure Virtual Machine (SQL Server / laptop IPython server)

1.  Maak een Azure VM met SQL Server en de server IPython-laptop.

2.  Gebruik een van de gegevens methoden om de gegevens van SQL Server exporteren naar bestanden exporteren.

    > [AZURE.NOTE] Als u alle gegevens uit de database op prem, alternatieve methode (sneller) de volledige database te verplaatsen naar de SQL Server-exemplaar in Azure verplaatsen. Slaat u de stappen voor het exporteren van gegevens, database maken en laden/import gegevens naar de doeldatabase en volgt u de alternatieve methode.

3.  Bestanden uploaden naar Azure opslag container.

4.  Laden van de gegevens naar een SQL Server-database op een Azure Virtual Machine.

    een.  Aanmelding bij de SQL Server-VM.

    b.  Bestanden downloaden van een Azure opslag container naar de map local VM.

    c.  Start SQL Server Management Studio.

    d.  Database- en tabellen maken.

    e.  Gebruik een van de bulk importeren methoden om de gegevens te laden.

    f.  Als tabel-joins vereist zijn, worden de indexen sneller joins maken.

    > [AZURE.NOTE] Voor sneller laden van grote omvang, gepartitioneerde tabellen maken en grote hoeveelheden gegevens tegelijkertijd importeren. Zie [Parallelle gegevens importeren naar gepartitioneerd SQL-tabellen](machine-learning-data-science-parallel-load-sql-partitioned-tables.md)voor meer informatie.

5.  Gegevens verkennen, maken van functies als nodig is. Houd er rekening mee dat de functies niet hoeven te worden gerealiseerd in de databasetabellen. Opmerking alleen de query nodig om ze te maken.

6.  Een besluit over een steekproefgrootte gegevens indien nodig en/of gewenst.

7.  Aanmelden bij de [Azure Machine Learning Studio](https://studio.azureml.net/).

8. Lees de gegevens rechtstreeks vanuit de SQL Server- [Gegevens importeren] met[ import-data] module. Plak de nodige query haalt velden en functies maakt en monsters indien nodig de [Gegevens importeren] in[ import-data] query.

9. Eenvoudige Azure Machine Learning experimenteren met geüploade dataset stroom.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Alternatieve methode volledige database kopiëren van een SQL-Server op ruimten naar Azure SQL-Database

![Lokale DB loskoppelen en koppelen aan SQL DB in Azure][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Aanvullende bronnen voor Azure: Azure Virtual Machine (SQL Server / laptop IPython server)

Om te repliceren de volledige SQL Server-database in uw SQL Server-VM, u moet een database kopiëren van de ene locatie/server naar de andere, ervan uitgaande dat de database kan worden gehouden die tijdelijk off line. Dit doet u in de SQL Server Management Studio Object Explorer of met de equivalente Transact-SQL-opdrachten.

1. Ontkoppel de database op de bronlocatie. Zie [ontkoppelen van een database](https://technet.microsoft.com/library/ms191491(v=sql.110).aspx)voor meer informatie.
2. Kopieer in Windows Verkenner of de Windows-opdrachtprompt venster losgekoppelde database of bestanden en het logboekbestand of meer bestanden naar de doellocatie op de SQL Server-VM in Azure.
3. De gekopieerde bestanden toevoegen aan de doelinstantie van SQL Server. Zie [een Database koppelen](https://technet.microsoft.com/library/ms190209(v=sql.110).aspx)voor meer informatie.

[Een Database met loskoppelen en koppelen (Transact-SQL)](https://technet.microsoft.com/library/ms187858(v=sql.110).aspx)

## <a name="largedbtohive"></a>Scenario \#7: grote gegevens in lokale bestanden doel component database in Azure HDInsight Hadoop-clusters

![Grote gegevens in lokale doel component][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Aanvullende bronnen voor Azure: Azure HDInsight Hadoop Cluster en Azure Virtual Machine (laptop IPython server)

1.  Maak een Azure Virtual Machine IPython laptop server uitgevoerd.

2.  Maak een cluster Azure HDInsight Hadoop.

3.  (Optioneel) Vooraf verwerken en gegevens opschonen.

    een.  Vooraf verwerken en gegevens in de IPython-laptop, toegang tot gegevens van Azure BLOB's schoon.

    b.  Transformatie van gegevens gereinigd, in tabelvorm, als dat nodig is.

    c.  Gegevens opslaan in een VM-lokale bestanden (IPython laptop op VM wordt uitgevoerd, lokale stations verwijzen naar de VM-stations).

4.  Gegevens uploaden naar de standaardcontainer van het Hadoop-cluster hebt geselecteerd in stap 2.

5.  Laden van gegevens aan de database component in Azure HDInsight Hadoop cluster.

    een.  Log in op de hoofd-knooppunt van het cluster Hadoop

    b.  Open de opdrachtregel Hadoop.

    c.  De hoofdmap van de component opgeven met opdracht `cd %hive_home%\bin` in het Hadoop-opdrachtregel.

    d.  De component query's uitvoeren om database en tabellen maken en gegevens laden van blob-opslag onderdeel tabellen.

    > [AZURE.NOTE] Als de gegevens groot is, kunnen gebruikers de component-tabel met partities maken. Vervolgens kunnen gebruikers gebruiken een `for` lus in de Hadoop opdrachtregel op het hoofd knooppunt om gegevens te laden in de component-tabel door partitie partities.

6.  Gegevens en functies maken als nodig is in het Hadoop-opdrachtregel. Houd er rekening mee dat de functies niet hoeven te worden gerealiseerd in de databasetabellen. Opmerking alleen de query nodig om ze te maken.

    een.  Log in op de hoofd-knooppunt van het cluster Hadoop

    b.  Open de opdrachtregel Hadoop.

    c.  De hoofdmap van de component opgeven met opdracht `cd %hive_home%\bin` in het Hadoop-opdrachtregel.

    d.  Voeren component Hadoop Command Line op het hoofd knooppunt van het cluster Hadoop de gegevens verkennen en het maken van functies als nodig is.

7.  Indien nodig en/of gewenst, voorbeeldgegevens het past in Azure Machine Learning Studio.

8.  Aanmelden bij de [Azure Machine Learning Studio](https://studio.azureml.net/).

9. Lees de gegevens rechtstreeks vanuit de `Hive Queries` met de [Gegevens importeren] [ import-data] module. Plak de nodige query haalt velden en functies maakt en monsters indien nodig de [Gegevens importeren] in[ import-data] query.

10. Eenvoudige Azure Machine Learning experimenteren met geüploade dataset stroom.

## <a name="decisiontree"></a>Beslissingsstructuur voor scenario-selectie
------------------------

In het volgende diagram bevat een overzicht van de scenario's beschreven en de geavanceerde Analytics proces technologie keuzes gemaakt waarmee u naar de gespecificeerde scenario's. Gegevensverwerking, exploratie, functie-engineering en bemonstering kunnen nemen plaats in een of meer methode/milieubescherming--bij de bron, gevorderde, of doel omgevingen- en iteratief nodig kunnen gaan. Het diagram alleen dient als een illustratie van een aantal mogelijke stromen en voorziet niet in een uitputtende opsomming.

![Voorbeeldscenario's DS proces scenario][8]

### <a name="advanced-analytics-in-action-examples"></a>Geavanceerde Analytics in actie voorbeelden

Zie voor end-to-end Azure Machine Learning-scenario's die gebruikmaken van de openbare datasets-technologie en geavanceerde Analytics proces:


* [Team gegevens wetenschap proces in actie: met behulp van SQL Server](machine-learning-data-science-process-sql-walkthrough.md).
* [Team gegevens wetenschap proces in actie: HDInsight Hadoop clusters met](machine-learning-data-science-process-hive-walkthrough.md).


[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

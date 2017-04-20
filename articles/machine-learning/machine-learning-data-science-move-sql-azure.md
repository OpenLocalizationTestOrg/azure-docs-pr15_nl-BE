<properties 
    pageTitle="Gegevens verplaatsen naar een SQL-Database Azure voor Azure Machine Learning | Azure" 
    description="SQL-tabel en gegevens laden voor SQL-tabel maken" 
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
    ms.date="09/14/2016"
    ms.author="bradsev" /> 

# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Gegevens verplaatsen naar een SQL-Database Azure voor Azure Machine Learning

In dit onderwerp worden de opties voor het verplaatsen van gegevens van bestanden (CSV- of TSV-indeling) of van gegevens die zijn opgeslagen in een lokale SQL Server naar Azure SQL-databases. Deze taken voor het verplaatsen van gegevens naar de cloud maken deel uit van het Team gegevens wetenschap proces.

Zie voor een onderwerp waarin de opties voor het verplaatsen van gegevens naar een lokale SQL Server voor Machine Learning, [verplaatsen van gegevens naar SQL Server op een Azure virtuele machine](machine-learning-data-science-move-sql-server-virtual-machine.md).

De volgende **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u de gegevens in te nemen doel omgevingen waar de gegevens kunnen worden opgeslagen en verwerkt tijdens het Team gegevens wetenschap proces (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

De volgende tabel bevat een overzicht van de opties voor het verplaatsen van gegevens naar een SQL-Database Azure.

<b>BRON</b> |<b>BESTEMMING: Azure SQL-Database</b> |
-------------- |--------------------------------|
<b>Plat bestand (CSV- of TSV-indeling)</b> |<a href="#bulk-insert-sql-query">Bulk Insert SQL-Query |
<b>Lokale SQL Server</b> | 1. <a href="#export-flat-file">exporteren naar een plat bestand<br> 2. <a href="#insert-tables-bcp">Wizard SQL-Database migreren<br> 3. <a href="#db-migration">database back-en terugzetten<br> 4. <a href="#adf">azure Data Factory |


## <a name="prereqs"></a>Vereisten
De hier beschreven procedures vereisen dat u over:

* Een **abonnement op Azure**. Als u niet een abonnement hebt, kunt u zich aanmelden voor een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).
* Een **account Azure opslag**. U een Azure opslag gebruikt voor het opslaan van de gegevens in deze zelfstudie. Als u geen account Azure opslag, Zie het artikel [een opslag-account maken](storage-create-storage-account.md#create-a-storage-account) . Nadat u de opslag-account hebt gemaakt, moet u de sleutel van de account gebruikt voor toegang tot de opslag te verkrijgen. Zie [weergave, kopieer en toegangstoetsen opslag opnieuw genereren](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Toegang tot een **Azure SQL-Database**. Als u een Azure SQL-Database, moet bevat [Aan de slag met Microsoft Azure SQL-Database](../sql-database/sql-database-get-started.md) informatie over het inrichten van een nieuw exemplaar van een Azure SQL-Database.
* Geïnstalleerde en geconfigureerde **Azure PowerShell** lokaal. Zie voor instructies [hoe u kunt installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

**Gegevens**: de migratie processen worden aangetoond door middel van de [Taxi RDAM dataset](http://chriswhong.com/open-data/foil_nyc_taxi/). De dataset RDAM Taxi bevat informatie over de gegevens van de reis en beurzen en beschikbaar is, zoals opgemerkt dat bericht, op Azure blob-opslag: [RDAM Taxi gegevens](http://www.andresmh.com/nyctaxitrips/). Een voorbeeld en een beschrijving van deze bestanden zijn beschikbaar in [RDAM Taxi Trips Dataset beschrijving](machine-learning-data-science-process-sql-walkthrough.md#dataset).
 
De procedures die hier worden beschreven om uw eigen gegevens aan te passen of de stappen zoals beschreven met behulp van de dataset RDAM Taxi. De dataset RDAM Taxi uploaden naar uw lokale SQL Server-database, volgt u de procedure die wordt beschreven in [Bulk importeren van gegevens in SQL Server-Database](machine-learning-data-science-process-sql-walkthrough.md#dbload). Deze instructies zijn voor een SQL-Server op een Azure Virtual Machine, maar de procedure voor het uploaden naar de lokale SQL Server hetzelfde is.


## <a name="file-to-azure-sql-database"></a>Gegevens uit de bron van een plat bestand te verplaatsen naar een Azure SQL-database

Gegevens in bestanden (CSV- of TSV opgemaakt), kan worden verplaatst naar een Azure SQL-database met behulp van een Bulk invoegen SQL-Query.

### <a name="bulk-insert-sql-query"></a>Bulk Insert SQL-Query

De stappen voor de procedure met behulp van de Bulk invoegen SQL-Query zijn vergelijkbaar met die waarvoor in de secties voor het verplaatsen van gegevens vanuit een plat bestandsbron met SQL Server op een Azure VM. Zie [SQL-Query Bulk invoegen](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery)voor meer informatie.


##<a name="sql-on-prem-to-sazure-sql-database"></a>Gegevens van een lokale SQL Server verplaatsen naar een Azure SQL-database

Als de brongegevens is opgeslagen in een lokale SQL Server, zijn er verschillende mogelijkheden voor het verplaatsen van de gegevens naar een Azure SQL-database:

1. [Exporteren naar een plat bestand](#export-flat-file) 
2. [Wizard voor SQL-Database migreren](#insert-tables-bcp)
3. [Database back-en terugzetten](#db-migration)
4. [Azure Data Factory](#adf)

Voor de eerste drie stappen zijn vergelijkbaar met de secties in het [verplaatsen van gegevens naar SQL Server op een Azure virtuele machine](machine-learning-data-science-move-sql-server-virtual-machine.md) die betrekking hebben op dezelfde procedure. Koppelingen naar de betreffende secties in dit onderwerp vindt u in de volgende instructies.

###<a name="export-flat-file"></a>Exporteren naar een plat bestand

De stappen om deze te exporteren naar een plat bestand zijn vergelijkbaar met die waarvoor in een [plat bestand exporteren](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file).

###<a name="insert-tables-bcp"></a>Wizard voor SQL-Database migreren

De stappen voor het gebruik van de Wizard SQL-Database migreren zijn vergelijkbaar met die waarvoor in de [Wizard SQL-Database migreren](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration).

###<a name="db-migration"></a>Database back-en terugzetten

De stappen voor het gebruik van de database een back-up en terugzetten zijn vergelijkbaar met die waarvoor in de [Database een back-up en terugzetten](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup).

###<a name="adf"></a>Azure Data Factory

De procedure voor het verplaatsen van gegevens naar een database van SQL Azure met Azure Data Factory (ADF) is beschikbaar in het onderwerp [gegevens van een lokale SQL server SQL Azure met Azure Data Factory verplaatsen](machine-learning-data-science-move-sql-azure-adf.md). In dit onderwerp wordt aangegeven hoe gegevens van een lokale SQL Server-database verplaatsen naar Azure SQL-databases via Azure Blob-opslag met behulp van de automatische Documentinvoer. 

Overweeg het gebruik van automatische Documentinvoer wanneer gegevens moeten voortdurend worden gemigreerd in een scenario met hybride die toegang heeft tot bronnen van zowel lokale als cloud, waarbij de gegevens is verwerkt of moet worden gewijzigd of toegevoegd wanneer wordt gemigreerd bedrijfslogica. Automatische Documentinvoer kunt u de planning en de controle van de taken met behulp van eenvoudige JSON-scripts die het verkeer van gegevens op een periodieke basis beheren. ADF heeft ook andere functies zoals ondersteuning voor complexe bewerkingen.





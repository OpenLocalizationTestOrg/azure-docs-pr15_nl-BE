   <properties
   pageTitle="Gegevens laden in Azure SQL Data Warehouse | Microsoft Azure"
   description="Informatie over de algemene scenario's voor gegevens in SQL Data Warehouse worden geladen. Deze omvatten PolyBase, Azure blob-opslag, platte bestanden en verzending van de schijf. U kunt ook hulpprogramma's van derden."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/12/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="load-data-into-azure-sql-data-warehouse"></a>Gegevens laden in Azure SQL Data Warehouse

Een overzicht van de opties voor scenario en aanbevelingen voor het laden van gegevens in SQL Data Warehouse.

Het lastigste onderdeel van het laden van gegevens is meestal met het voorbereiden van de gegevens voor de belasting. Azure vereenvoudigt laden door Azure blob-opslag gebruikt als een algemene gegevensarchief voor veel van de diensten en Azure Data Factory goedkeuringen door verplaatsing van gegevens en communicatie tussen de Azure services. Deze processen zijn geïntegreerd met PolyBase-technologie die massively parallel processing (MPP) gebruikt om gegevens te laden parallel van Azure blob-opslag in SQL Data Warehouse. 

Zie voor zelfstudies die voorbeelddatabases laden, [Voorbeelddatabases laden][].

## <a name="load-from-azure-blob-storage"></a>Laden van Azure blob-opslag
De snelste manier om gegevens te importeren in SQL Data Warehouse is PolyBase gebruiken om gegevens te laden uit de Azure blobopslag. SQL Data Warehouse massively parallel processing (MPP) ontwerp PolyBase gebruikt om gegevens te laden parallel van Azure blob-opslag. U kunt voor het gebruik van PolyBase, T-SQL-opdrachten of een pijpleiding Azure Data Factory gebruiken.

### <a name="1-use-polybase-and-t-sql"></a>1. PolyBase en T-SQL gebruiken

Overzicht van het proces worden geladen:

2. Uw gegevens opmaken als UTF-8, aangezien PolyBase biedt momenteel geen ondersteuning voor UTF-16.
2. Verplaats uw gegevens naar Azure blob-opslag en opslaan in tekstbestanden.
3. Externe objecten configureren in SQL Data Warehouse voor het definiëren van de locatie en de indeling van de gegevens
4. Een T-SQL-opdracht de gegevens gelijktijdig laden in een nieuwe databasetabel uitvoeren.

<!-- 5. Schedule and run a loading job. --> 

Zie voor een zelfstudie, [gegevens laden van Azure blob-opslag met SQL Data Warehouse (PolyBase)][].

### <a name="2-use-azure-data-factory"></a>2. Gebruik Azure Data Factory

Voor een eenvoudiger manier om PolyBase te gebruiken, kunt u een pijpleiding Azure Data Factory die PolyBase gebruikt om gegevens te laden uit de Azure blobopslag in SQL Data Warehouse. Dit is snel te configureren omdat u niet hoeft te definiëren van de T-SQL-objecten. Als u de externe gegevens opvragen zonder dat u deze importeert, gebruikt u T-SQL. 

Overzicht van het proces worden geladen:

2. Uw gegevens opmaken als UTF-8, aangezien PolyBase biedt momenteel geen ondersteuning voor UTF-16.
2. Verplaats uw gegevens naar Azure blob-opslag en opslaan in tekstbestanden.
3. Een pijpleiding Azure Data Factory om aan te nemen van de gegevens maken. Gebruik de optie PolyBase.
4. Plannen en uitvoeren van de pijpleiding.

Zie voor een zelfstudie, [gegevens laden van Azure blob-opslag met SQL Data Warehouse (Azure Data Factory genoemd)][].


## <a name="load-from-sql-server"></a>Laden van SQL Server
Om gegevens te laden vanuit SQL Server naar SQL datawarehouse kunt u Integration Services (SSIS), platte bestanden overdragen of schijven naar Microsoft verzenden. Lees bij Zie een overzicht van de verschillende processen en koppelingen naar cursussen laden.

Als u van plan bent een migratie volledige gegevens van SQL Server naar SQL Data Warehouse, Zie [migreren-overzicht][]. 

### <a name="use-integration-services-ssis"></a>Gebruik integratieservices (SSIS)
Als u al laden in SQL Server Integration Services (SSIS)-pakketten, kunt u uw pakketten voor het gebruik van SQL Server als de bron- en SQL Data Warehouse als de bestemming kunt bijwerken. Dit is snel en eenvoudig te doen, en is een goede keuze als u niet wilt migreren van het laadproces als gegevens wilt gebruiken al in de cloud. De verhouding is dat de belasting zijn lager dan het gebruik van PolyBase, omdat deze SSIS de belasting geen parallel voert.

Overzicht van het proces worden geladen:

1. Wijzig uw pakket Integration Services om te verwijzen naar de SQL Server-exemplaar voor de bron en de SQL Data Warehouse-database voor de bestemming.
2. Migreren naar SQL Data Warehouse, uw schema als dit nog niet bevat.
3. De toewijzing in uw pakketten wijzigen alleen de gegevenstypen die worden ondersteund door SQL Data Warehouse gebruiken.
3. Plannen en uitvoeren van het pakket.

Zie voor een zelfstudie [laden gegevens uit SQL Server naar Azure SQL gegevens magazijn (SSIS)][].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Gebruiken (aanbevolen voor < 10 TB) AZCopy
Als de gegevensgrootte van uw < 10 TB is, kunt u de gegevens van SQL Server exporteren naar bestanden, de bestanden kopiëren voor Azure blob-opslag en vervolgens de gegevens te laden in SQL Data Warehouse gebruiken PolyBase

Overzicht van het proces worden geladen:

1. Gebruik het opdrachtregelprogramma bcp gegevens uit SQL Server exporteren naar bestanden.
2. Gebruik het opdrachtregelprogramma AZCopy om gegevens te kopiëren van bestanden naar Azure blobopslag.
3. Gebruik PolyBase in SQL Data Warehouse worden geladen.

Zie voor een zelfstudie, [gegevens laden van Azure blob-opslag met SQL Data Warehouse (PolyBase)][].

### <a name="use-bcp"></a>Bcp gebruiken
Als u een kleine hoeveelheid gegevens kunt u bcp rechtstreeks in Azure SQL Data Warehouse worden geladen.

Overzicht van het proces worden geladen:
1. Gebruik het opdrachtregelprogramma bcp gegevens uit SQL Server exporteren naar bestanden.
2. Bcp gebruiken om gegevens te laden uit platte bestanden rechtstreeks naar SQL Data Warehouse.

Zie voor een zelfstudie [laden gegevens uit SQL Server Azure SQL Data Warehouse (bcp)][].


### <a name="use-importexport-recommended-for--10-tb-data"></a>Import/Export (aanbevolen voor gegevens > 10 TB) gebruiken
Als de gegevensgrootte van uw > 10 TB is en u wilt verplaatsen naar Azure, wordt aangeraden dat u onze verzendservice [Importeren/exporteren][]schijf gebruiken. 

Samenvatting van het laadproces
2. Gebruik het opdrachtregelprogramma bcp gegevens uit SQL Server exporteren naar bestanden op overdraagbare schijven.
3. De schijven naar Microsoft te verzenden.
4. Microsoft laadt de gegevens in SQL Data Warehouse

## <a name="load-from-hdinsight"></a>Laden van HDInsight
SQL Data Warehouse ondersteunt gegevens laden van HDInsight via PolyBase. Het proces is hetzelfde als het laden van gegevens uit de Azure Blob-opslag - PolyBase via verbinding met HDInsight om gegevens te laden. 

### <a name="1-use-polybase-and-t-sql"></a>1. PolyBase en T-SQL gebruiken

Overzicht van het proces worden geladen:

2. Uw gegevens opmaken als UTF-8, aangezien PolyBase biedt momenteel geen ondersteuning voor UTF-16.
2. Verplaats uw gegevens naar HDInsight en sla deze op tekstbestanden, ORC of parketvloeren-indeling.
3. Externe objecten configureren in SQL Data Warehouse voor het definiëren van de locatie en de indeling van de gegevens.
4. Een T-SQL-opdracht de gegevens gelijktijdig laden in een nieuwe databasetabel uitvoeren.

Zie voor een zelfstudie, [gegevens laden van Azure blob-opslag met SQL Data Warehouse (PolyBase)][].

## <a name="recommendations"></a>Aanbevelingen

Veel van onze partners hebben oplossingen te laden. Als u wilt meer weten, een overzicht van onze [oplossingspartners][]. 

Als de gegevens afkomstig zijn uit een niet-relationele gegevensbron en u laden in SQL Data Warehouse wilt, u moet deze om te zetten in rijen en kolommen voordat u het opent. De getransformeerde gegevens niet hoeft te worden opgeslagen in een database, kan deze worden opgeslagen in tekstbestanden.

Statistieken op het nieuw geladen gegevens maken. Azure SQL Data Warehouse biedt nog geen ondersteuning voor automatische maken of automatische update statistieken.  Om de beste prestaties van uw query's is het belangrijk om statistieken te maken op alle kolommen van alle tabellen na de eerste keer worden geladen of belangrijke wijzigingen optreden in de gegevens.  Zie voor meer informatie, [Statistieken][].


## <a name="next-steps"></a>Volgende stappen
Voor meer tips voor ontwikkeling, Zie het [overzicht van de ontwikkeling][].

<!--Image references-->

<!--Article references-->
[Gegevens laden vanuit Azure blob-opslag naar SQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Gegevens laden vanuit Azure blob-opslag naar SQL Data Warehouse (Azure Data Factory genoemd)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Laden van gegevens uit SQL Server naar Azure SQL gegevens magazijn (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Laden van gegevens van SQL Server naar Azure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Voorbeelddatabases laden]: ./sql-data-warehouse-load-sample-databases.md
[Migreren-overzicht]: ./sql-data-warehouse-overview-migrate.md
[Solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[ontwikkelen-overzicht]: ./sql-data-warehouse-overview-develop.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Importeren/exporteren]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/

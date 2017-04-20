<properties
   pageTitle="Azure Data Factory gebruiken met SQL datawarehouse | Microsoft Azure"
   description="Tips voor het gebruik van Azure Factory (ADF) met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-azure-data-factory-with-sql-data-warehouse"></a>Azure Data Factory met SQL datawarehouse gebruiken

Azure Data Factory biedt een volledig beheerde methode bij de regie van de overdracht van gegevens en het uitvoeren van opgeslagen procedures in SQL Data Warehouse.  Hierdoor kunt u gemakkelijker instellen en planning complexe uitpakken transformeren en laden (ETL) procedures met SQL Data Warehouse. Zie de [documentatie van Azure Data Factory][]voor een vollediger overzicht van Azure Data Factory.

## <a name="data-movement"></a>Verplaatsing van gegevens

Azure Data Factory kunt gegevens verplaatsen tussen bronnen op gebouwen en de andere Azure diensten.  Algemene, huidige integratie met Azure Data Factory ondersteunt verplaatsing van gegevens naar en van de volgende locaties:

+ Azure blob-opslag
+ Azure SQL-Database
+ On-premises SQL Server
+ SQL Server op IaaS

Voor meer informatie over het instellen van een data Zie kopie activiteit [Azure Data Factory-gegevens kopiëren][]

## <a name="stored-procedures"></a>Opgeslagen Procedures
 Op dezelfde manier die kan worden gebruikt voor het overdragen van gegevens plannen, worden Azure Data Factory ook gebruikt voor het uitvoeren van opgeslagen procedures uitvoert.  Hierdoor kan meer complexe pijpleidingen worden gemaakt en breidt Azure Data Factory mogelijk om met de rekenkundige kracht van SQL Data Warehouse.

## <a name="next-steps"></a>Volgende stappen
Zie voor een overzicht van de integratie van [SQL Data Warehouse-integratie-overzicht][].
Zie [SQL Data Warehouse ontwikkelen-overzicht][]voor meer tips voor ontwikkeling.

<!--Image references-->

<!--Article references-->

[Gegevens kopiëren met Azure Data Factory]: ../data-factory/data-factory-data-movement-activities.md
[SQL Data Warehouse ontwikkelen-overzicht]: ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse-integratie-overzicht]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory-documentatie]:https://azure.microsoft.com/documentation/services/data-factory/


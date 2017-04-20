<properties
   pageTitle="Analytics Azure Stream gebruiken met SQL datawarehouse | Microsoft Azure"
   description="Tips voor het gebruik van Azure Stream Analytics met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Azure Stream Analytics met SQL datawarehouse gebruiken

Azure Stream Analytics is een volledig beheerde service met lage latentie, hoge beschikbaarheid, schaalbaar en complexe gebeurtenis verwerken via streaming gegevens in de cloud. U kunt de basisbeginselen leren door te lezen [Inleiding tot Azure Stream Analytics][]. Vervolgens leert u hoe een end-to-end oplossing maken met Analytics stroom door de zelfstudie [aan de slag met Azure Stream Analytics][] .

In dit artikel leert u hoe u uw Azure SQL Data Warehouse-database gebruiken als een sink uitvoer voor uw projecten stoom Analytics.

## <a name="prerequisites"></a>Vereisten

Voer eerst de volgende stappen in de zelfstudie [aan de slag met Azure Stream Analytics][] .  

1. Maak een gebeurtenis Hub-invoer
2. Configureren en starten van de gebeurtenis generator toepassing
3. Een taak Analytics stroom voorziening
4. Functie input en de query opgeven

Maak vervolgens een Azure SQL Data Warehouse-database

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Uitvoer van de taak opgeven: Azure SQL Data Warehouse-database

### <a name="step-1"></a>Stap 1

Klik op **uitvoer** vanaf de bovenkant van de pagina en klik op **Uitvoer toevoegen**in uw project Stream Analytics.

### <a name="step-2"></a>Stap 2

Selecteer SQL-Database en klik op volgende.

![][add-output]

### <a name="step-3"></a>Stap 3
Voer de volgende waarden op de volgende pagina:

- *Uitvoeralias*: Geef een beschrijvende naam voor de uitvoer van deze taak.
- *Abonnement*:
    - Als uw SQL Data Warehouse-database in het abonnement hetzelfde als de taak van de Stream Analytics, selecteer SQL-Database gebruiken uit huidige abonnement.
    - Als de database zich in een ander abonnement, selecteer SQL-Database gebruiken vanaf een ander abonnement.
- *Database*: Geef de naam van een doeldatabase.
- *Naam*: Geef de naam van de server voor de database die u zojuist hebt opgegeven. De klassieke Azure-Portal kunt u dit zoeken.

![][server-name]

- *Gebruikersnaam*: Geef de gebruikersnaam van een account met machtigingen voor de database schrijven.
- *Wachtwoord*: het wachtwoord voor de opgegeven gebruikersaccount opgeven.
- *Tabel*: Geef de naam van de doeltabel in de database.

![][add-database]

### <a name="step-4"></a>Stap 4

Klik op de knop controleren om toe te voegen van de uitvoer van deze taak en om te controleren dat stroom Analytics verbinding met de database maken kunnen.

![][test-connection]

Wanneer de verbinding met de database is voltooid, ziet u een melding op de onderkant van de portal. U kunt klikken op verbinding testen onder aan het testen van de verbinding met de database.

## <a name="next-steps"></a>Volgende stappen

Zie voor een overzicht van de integratie van [SQL Data Warehouse-integratie-overzicht][].

Zie [SQL Data Warehouse ontwikkelen-overzicht][]voor meer tips voor ontwikkeling.

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Inleiding tot Analytics Azure Stream]: ../stream-analytics/stream-analytics-introduction.md
[Aan de slag met Azure Stream Analytics]: ../stream-analytics/stream-analytics-get-started.md
[SQL Data Warehouse ontwikkelen-overzicht]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse-integratie-overzicht]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/

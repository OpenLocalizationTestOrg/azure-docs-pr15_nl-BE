<properties
   pageTitle="Azure Machine leren gebruiken met SQL datawarehouse | Microsoft Azure"
   description="Zelfstudie voor het gebruik van Azure Machine Learning met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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

# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a>Azure Machine Learning met SQL datawarehouse gebruiken

Azure Machine Learning is een volledig beheerde voorspellende analytics-service die voorspellende modellen ten opzichte van de gegevens in SQL Data Warehouse maken en vervolgens publiceren als webservices gereed te verbruiken. U kunt de basisbegrippen van voorspellende analyses en machine leren door te lezen, [Inleiding tot de computer leren op Azure][].  U kunt vervolgens informatie over het maken, trainen, score en testen van een machine learning-model met behulp van de [zelfstudie experimenteren maken][].

In dit artikel leert u hoe u het volgende doen met de [Azure Machine Learning Studio][]:

- Gegevens lezen uit de database te maken, trainen en scoren een voorspellende model
- Gegevens schrijven naar de database


## <a name="read-data-from-sql-data-warehouse"></a>Gegevens lezen van SQL Data Warehouse

We zullen gegevens lezen uit de tabel producten in de database AdventureWorksDW.

### <a name="step-1"></a>Stap 1

Een nieuw experiment starten door te klikken op + Nieuw onder aan het venster Machine Learning Studio EXPERIMENT selecteren en selecteer vervolgens lege experimenteren. Selecteer de naam van het experiment standaard boven aan het canvas en wijzig de herkenbare, bijvoorbeeld fiets prijs voorspelling.

### <a name="step-2"></a>Stap 2

Zoek de module Reader in het palet van datasets en modules aan de linkerkant van het canvas experiment. Sleep de module naar het canvas experiment.
![][drag_reader]

### <a name="step-3"></a>Stap 3

Selecteer de module lezer en invullen in het deelvenster Eigenschappen.

1. Azure SQL-Database als gegevensbron selecteren.
2. Naam van de databaseserver: Typ de naam van de server. De [Azure portal][] kunt u dit zoeken.

![][server_name]

3. Naam: Typ de naam van een database op de server die u zojuist hebt opgegeven.
4. Naam van gebruikersaccount server: Typ de gebruikersnaam van een account met machtigingen voor de database.
5. Serverwachtwoord: het wachtwoord voor de opgegeven gebruikersaccount opgeven.
6. Accepteer het certificaat van elke server: deze optie (minder veilig) gebruiken als u overslaan van het certificaat van de site controleren wilt voordat u de gegevens lezen.
7. Databasequery: Voer een SQL-instructie over de gegevens die u wilt lezen. In dit geval zullen we gegevens lezen uit de tabel producten met de volgende query.


```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a>Stap 4

1. Het experiment uitvoeren door te klikken op uitvoeren in het canvas experiment.
2. Als het experiment is voltooid, heeft de lezer module een groen vinkje om aan te geven met succes is voltooid. U ziet ook de status actief in de rechterbovenhoek gereedgemeld.

![][run]

3. De geïmporteerde gegevens worden weergegeven en klikt u op de uitvoerpoort op de onderkant van de auto dataset visualiseren selecteren.


## <a name="create-train-and-score-a-model"></a>Maken, trainen en scoren van een model

U kunt nu deze dataset te gebruiken:

- Een Model maken: verwerken van gegevens en functies definiëren
- Het model van de trein: kiezen en toepassen van een leeralgoritme
- Score en het model test: nieuwe fiets prijs voorspellen


![][model]

Voor meer informatie over het maken, trainen, score en testen van een machine model gebruik leren de [maken zelfstudie experimenteren][].

## <a name="write-data-to-azure-sql-data-warehouse"></a>Gegevens schrijven naar Azure SQL Data Warehouse

We worden de resultaatset naar de tabel ProductPriceForecast in de AdventureWorksDW-database geschreven.

### <a name="step-1"></a>Stap 1

Zoek naar de schrijver module in het palet van datasets en modules aan de linkerkant van het canvas experiment. Sleep de module naar het canvas experiment.

![][drag_writer]

### <a name="step-2"></a>Stap 2

Selecteer de module schrijver en invullen in het deelvenster Eigenschappen.

1. Selecteer Azure SQL-Database als de gegevensbestemming.
2. Naam van de databaseserver: Typ de naam van de server. De [Azure portal][] kunt u dit zoeken.
3. Naam: Typ de naam van een database op de server die u zojuist hebt opgegeven.
4. Naam van gebruikersaccount server: Typ de gebruikersnaam van een account met machtigingen voor de database schrijven.
5. Serverwachtwoord: het wachtwoord voor de opgegeven gebruikersaccount opgeven.
6. Accepteren van een servercertificaat (onbeveiligde): Selecteer deze optie als u niet wilt dat het certificaat bekijken.
7. Door komma's gescheiden lijst van de kolommen op te slaan: een lijst van de dataset of het resultaat van de kolommen die u wilt uitvoeren.
8. De naam van de tabel: de naam van de tabel opgeven.
9. Door komma's gescheiden lijst met kolommen datatable: Geef de namen van de te gebruiken in de nieuwe tabel. De namen kunnen afwijken van de instellingen in de dataset bron, maar u moet hetzelfde aantal kolommen hier die u voor de uitvoertabel definieert worden vermeld.
10. Aantal rijen per bewerking SQL Azure geschreven: kunt u het aantal rijen die zijn geschreven met een SQL-database in één bewerking.

![][writer_properties]

### <a name="step-3"></a>Stap 3

1. Het experiment uitvoeren door te klikken op uitvoeren in het canvas experiment.
2. Als het experiment is voltooid, wordt alle modules een groen vinkje om aan te geven dat zij voltooid hebben.

## <a name="next-steps"></a>Volgende stappen

Zie [SQL Data Warehouse ontwikkelen-overzicht][]voor meer tips voor ontwikkeling.

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[SQL Data Warehouse ontwikkelen-overzicht]: ./sql-data-warehouse-overview-develop.md
[Handleiding experiment maken]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Inleiding tot de computer leren op Azure]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure Machine Learning Studio]: https://studio.azureml.net/Home
[Azure portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/

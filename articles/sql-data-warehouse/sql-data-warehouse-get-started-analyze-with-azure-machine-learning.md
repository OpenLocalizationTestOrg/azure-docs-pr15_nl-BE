<properties
   pageTitle="Gegevens analyseren met Azure Machine Learning | Microsoft Azure"
   description="Gebruik de Azure Machine leren een voorspellende machine leren model op basis van gegevens die zijn opgeslagen in Azure SQL Data Warehouse bouwen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/14/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="analyze-data-with-azure-machine-learning"></a>Gegevens analyseren met Azure Machine Learning

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

In deze zelfstudie wordt Azure Machine leren voor het maken van een anticiperende machine leren model op basis van gegevens die zijn opgeslagen in Azure SQL Data Warehouse. Met name dit stelt samen een gerichte marketingcampagne voor Adventure Works, de fietsenhandelaar voorspellen als een klant is het waarschijnlijk een fiets kopen of niet.

> [AZURE.VIDEO integrating-azure-machine-learning-with-azure-sql-data-warehouse]


## <a name="prerequisites"></a>Vereisten
Stap in deze zelfstudie, wilt u het volgende nodig:

- Een datawarehouse SQL is vooraf geladen met voorbeeldgegevens AdventureWorksDW. Om deze bepaling Zie [maken een datawarehouse SQL][] en kiest u de voorbeeldgegevens worden geladen. Als u al een datawarehouse maar geen voorbeeldgegevens hebt, kunt u de [voorbeeldgegevens handmatig laden][].

## <a name="1-get-data"></a>1. de gegevens ophalen
De gegevens zijn in de weergave van dbo.vTargetMail in de AdventureWorksDW-database. Dit om gegevens te lezen:

1. Aanmelden bij [studio Azure Machine Learning][] en klik op mijn experimenten.
2. Klik op **+ Nieuw** en selecteer **Lege Experiment**.
3. Geef een naam op voor uw experiment: Marketing gericht.
4. Sleep de module **lezer** uit het deelvenster modules in het canvas.
5. De details van uw SQL Data Warehouse-database opgeven in het deelvenster Eigenschappen.
6. Geef de database **query** de gegevens lezen van belang.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Het experiment uitvoeren door te klikken op **uitvoeren** in het canvas experiment.
![Het experiment uitvoeren][1]


Nadat het experiment is uitgevoerd met succes wordt uitgevoerd, klikt u op de uitvoerpoort op de onderkant van de Reader-module en **visualiseren** om te zien van de geïmporteerde gegevens te selecteren.
![Geïmporteerde gegevens weergeven][3]


## <a name="2-clean-the-data"></a>2. de gegevens opschonen
Neerzetten om de gegevens opschonen, sommige kolommen die niet relevant voor het model zijn. Om dit te doen:

1. Sleep de module **Project kolommen** naar het canvas.
2. Klik op **de kolomkiezer starten** in het deelvenster Eigenschappen kunt u opgeven welke kolommen u wilt neerzetten.
![Kolommen in Project][4]

3. Twee kolommen uitsluiten: CustomerAlternateKey en GeographyKey.
![Verwijder overbodige kolommen][5]


## <a name="3-build-the-model"></a>3. het model bouwen
We splitst de gegevens 80-20: 80% tot een machine leren model en 20% voor het testen van het model van de trein. Maken we gebruik van de 'Klasse 2' algoritmen voor dit probleem binaire indeling.

1. Sleep de module **splitsen** in het canvas.
2. 0,8 voor een fractie van de rijen in de eerste uitvoer gegevensset in het deelvenster Eigenschappen invoeren.
![Gegevens in de opleidings- en set splitsen][6]
3. Sleep de module **Twee klasse versterkt de beslissingsstructuur** in het canvas.
4. Sleep de module die **Train Model** in het canvas en geef de ingangen. Klik vervolgens op **starten kolomkiezer klikken** in het deelvenster Eigenschappen.
      - Eerste invoer: ML algoritme.
      - Tweede input: gegevens naar het algoritme op te trainen.
![Sluit de module Model trein][7]
5. Selecteer de kolom **BikeBuyer** als de kolom te voorspellen.
![Selecteer de kolom om te voorspellen][8]


## <a name="4-score-the-model"></a>4. het model score
Nu toetsen we hoe het model van testgegevens uitvoert. We zullen het algoritme van onze keuze met een ander algoritme om te zien welke presteert beter vergelijken.

1. Module **Score-Model** naar het canvas slepen.
    Eerste invoer: opgeleid model tweede input: gegevens testen ![Score van het model][9]
2. De **Twee klasse Bayes punt Machine** in de experiment canvas sleept. We zullen vergelijken hoe dit algoritme presteert ten opzichte van de beslissingsstructuur van twee klasse versterkt.
3. Kopieer en plak de modules trein Model en het Model van de Score op het tekenpapier.
4. Sleep de module **Evaluatie van Model** naar het canvas te vergelijken van de twee algoritmen.
5. **Voer** het experiment.
![Het experiment uitvoeren][10]
6. De uitvoerpoort op de onderkant van de module evaluatie van Model en klik op visualiseren.
![Visualiseren van de evaluatieresultaten][11]

De verstrekte cijfers zijn de ROC-curve, precision intrekken diagram en lift curve. Deze gegevens worden weergegeven, kunnen we zien dat het eerste model beter dan in het tweede voorbeeld worden uitgevoerd. Om te kijken wat het eerste model voorspeld, op de uitvoerpoort van de Score-Model en klik op visualiseren.
![Visualiseren van de resultaten van de score][12]

Ziet u twee of meer kolommen toegevoegd aan de dataset test.

- Gescoord waarschijnlijkheid: de kans dat een klant een fiets koper is.
- Labels gescoord: de classificatie gebeurt door het model – fiets koper (1) of niet (0). Deze drempel kans voor labeling is ingesteld op 50% en kan worden aangepast.

De kolom BikeBuyer (werkelijk) met de etiketten gescoord (voorspelling) te vergelijken, kunt u zien hoe goed het model heeft uitgevoerd. Als de volgende stappen kunt u dit model voorspellingen voor nieuwe klanten en het publiceren van dit model als een webservice of resultaten terugschrijven naar SQL Data Warehouse.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het bouwen van voorspellende machine learning modellen, [Inleiding tot de computer leren op Azure][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure Machine Learning-studio]:https://studio.azureml.net/
[Inleiding tot de computer leren op Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[voorbeeldgegevens handmatig laden]: sql-data-warehouse-load-sample-databases.md
[Maken van een SQL-datawarehouse]: sql-data-warehouse-get-started-provision.md

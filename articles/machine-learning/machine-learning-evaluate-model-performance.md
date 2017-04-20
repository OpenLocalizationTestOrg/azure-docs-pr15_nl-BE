<properties 
    pageTitle="Evalueren van prestaties in de Machine Learning model | Microsoft Azure" 
    description="Wordt uitgelegd hoe u de prestaties in Azure Machine Learning model evalueren." 
    services="machine-learning"
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="bradsev;garye" />


# <a name="how-to-evaluate-model-performance-in-azure-machine-learning"></a>Het evalueren van prestaties in Azure Machine Learning model

In dit onderwerp wordt gedemonstreerd hoe u de evaluatie van de prestaties van een model in Azure Machine Learning Studio en bevat een korte uitleg van de statistieken beschikbaar voor deze taak. Drie veelvoorkomende scenario's met leren onder toezicht worden weergegeven: 

* regressie
* binaire indeling 
* multiclass classificatie

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Evaluatie van de prestaties van een model is een van de core fasen in het proces van de wetenschappelijke gegevens. Hiermee wordt aangegeven hoe succesvol de score (voorspellingen) van een dataset is door een opgeleide model. 

Model-evaluatie door middel van twee van de belangrijkste machine learning modules Azure Machine leren ondersteunt: [Evalueren van Model] [ evaluate-model] en [Valideren van Cross-Model][cross-validate-model]. Deze modules kunnen u zien hoe uw model wordt uitgevoerd in een aantal cijfers die vaak worden gebruikt in de machine learning en statistieken.

##<a name="evaluation-vs-cross-validation"></a>Evaluatie vs. Cross-validatie##
Evaluatie en validatie cross zijn standaard manieren voor het meten van de prestaties van uw model. Beide genereren evaluatie parameters die u kunt controleren of vergelijken met die van andere modellen.

[Evalueren van Model] [ evaluate-model] een dataset scored verwacht als invoer (of 2 in het geval dat u wilt vergelijken van de prestaties van 2 verschillende modellen). Dit betekent dat u moet trainen uw model met behulp van het [Model van de trein] [ train-model] module en om voorspellingen op sommige gegevensset met behulp van de [Score-Model] [ score-model] module, voordat u de resultaten kunt evalueren. De evaluatie van de op basis van de scored labels/kansen samen met de waarde true labels, die allemaal uitgevoerd door het [Score-Model worden] is[ score-model] module.

Ook kunt u meerdere validatie voor het uitvoeren van een aantal bewerkingen trein score evalueren (10 vouwen) automatisch op verschillende subsets van de ingevoerde gegevens. De ingevoerde gegevens is opgedeeld in 10 delen, waarbij een is gereserveerd voor het testen, en de andere 9 voor de opleiding. Dit proces is 10 maal herhaald en de gemiddelde cijfers over de evaluatie. Dit helpt om te bepalen hoe goed een model in de nieuwe datasets zouden generaliseren. Het [Valideren van Cross-Model] [ cross-validate-model] module in een ongetrainde model en sommige gelabelde dataset neemt en de evaluatieresultaten van elk van de 10 vouwen, en de gemiddelde resultaten oplevert.

In de volgende secties wordt eenvoudige regressie en classificatie-modellen maken en evalueren van hun prestaties, met behulp van zowel het [Evalueren van Model] [ evaluate-model] en het [Model kruislings gevalideerd] [ cross-validate-model] modules.

##<a name="evaluating-a-regression-model"></a>Evaluatie van een regressiemodel##
Stel dat we willen voorspellen met bepaalde functies, zoals afmetingen, paardenkracht en specificaties van de motor van een auto-prijs. Dit is een typische regressie-probleem, waarbij de variabele doel (*prijs*) is een continue numerieke waarde. We past een eenvoudige lineaire regressiemodel dat, gezien de waarden van de functie van een bepaalde auto de prijs van de auto die voorspellen kunt. Dit regressiemodel kan worden gebruikt om te scoren we getraind op dezelfde dataset. Als we de voorspelde prijzen voor alle vijf de auto hebt, kunnen we de prestaties van het model door te kijken naar de voorspellingen hoeveel van de werkelijke prijzen gemiddeld afwijken evalueren. Ter illustratie gebruiken we de *auto prijs gegevens (Raw)-dataset* beschikbaar in de sectie **Opgeslagen Datasets** in Azure Machine Learning Studio.
 
###<a name="creating-the-experiment"></a>Het Experiment maken###
De volgende modules toevoegen aan uw werkruimte in Azure Machine Learning Studio:

- Auto prijsgegevens (Raw)
- [Lineaire regressie][linear-regression]
- [Model trein][train-model]
- [Score-Model][score-model]
- [Model evalueren][evaluate-model]


De poorten verbinden, zoals hieronder wordt weergegeven in afbeelding 1 en de Label-kolom van de [Trein Model] [ train-model] module op *prijs*.
 
![Evaluatie van een regressiemodel](media/machine-learning-evaluate-model-performance/1.png)

Figuur 1. Evaluatie van een regressiemodel.

###<a name="inspecting-the-evaluation-results"></a>Controleren van de evaluatieresultaten###
Nadat het experiment is uitgevoerd, kunt u klikken op de uitvoerpoort van het [Evalueren van Model] [ evaluate-model] module en selecteer *visualiseren* om te zien van de resultaten van de evaluaties. De evaluatie voor regressie-modellen beschikbaar zijn: *Gemiddelde Absolute fout*, *Root gemiddelde Absolute fout* *Relatieve fout van Absolute*, *Relatieve fout kwadraat*en de *Coëfficiënt van de bepaling*.

De term 'fout' hier het verschil aangeeft tussen de voorspelde waarde en de werkelijke waarde. De absolute waarde of het kwadraat van het verschil wordt meestal berekend voor het vastleggen van de totale omvang van de fout in alle exemplaren, zoals het verschil tussen de waarde van de voorspelde en waar in sommige gevallen negatief zijn. Cijfers over de fout meten de voorspellende prestaties van een regressiemodel in termen van de gemiddelde afwijking ten opzichte van de voorspellingen van de true-waarden. Lagere waarden betekenen dat het model bij het maken van prognoses nauwkeuriger is. Een algemene fout metric-waarde 0 betekent dat het model de gegevens perfect past.

De determinatiecoëfficiënt is ook bekend als R squared, is ook een standaardmethode voor het meten van hoe goed het model past bij de gegevens. Dit kan worden geïnterpreteerd als het deel van de variatie verklaard door het model. Een hoger percentage is in dit geval beter waarbij 1 geeft past perfect.
 
![Lineaire regressie evaluatie Metrics](media/machine-learning-evaluate-model-performance/2.png)

Figuur 2. Lineaire regressie evaluatie statistieken.

###<a name="using-cross-validation"></a>Met behulp van Cross-validatie###
Zoals eerder vermeld, kunt u uitvoeren herhaalde opleiding, scoren en evaluaties die automatisch met het [Valideren van Cross-Model] [ cross-validate-model] module. U hoeft in dit geval is een dataset een ongetrainde model en een [Model kruislings gevalideerd] [ cross-validate-model] module (Zie onderstaande afbeelding). U moet de labelkolom ingesteld op *prijs* in het [Model kruislings gevalideerd] [ cross-validate-model] eigenschappen van de module.

![Een regressiemodel cross valideren](media/machine-learning-evaluate-model-performance/3.png)

In figuur 3. Een regressiemodel cross valideren.

Nadat het experiment is uitgevoerd, kunt u de evaluatieresultaten controleren door te klikken op de juiste uitvoerpoort van het [Model kruislings gevalideerd] [ cross-validate-model] module. Dit biedt een gedetailleerde weergave van de statistieken voor elke iteratie (vouwen) en de gemiddelde resultaten van elk van de parameters (figuur 4).
 
![Resultaten van een regressiemodel cross-validatie](media/machine-learning-evaluate-model-performance/4.png)

In figuur 4. Cross-validatie van de resultaten van een regressiemodel.

##<a name="evaluating-a-binary-classification-model"></a>Evaluatie van een Model van de binaire indeling##
In een binaire indeling, is de variabele doel slechts twee mogelijke uitkomsten, bijvoorbeeld: {0, 1} of {ONWAAR, waar}, {negatieve, positieve}. Stel krijgt u een dataset van volwassen werknemers met enkele demografische en werkgelegenheid variabelen, en u wordt gevraagd om te voorspellen het inkomensniveau, een binaire variabele met de waarden {"< = 50K", "> 50K"}. Met andere woorden, de negatieve klasse vertegenwoordigt de werknemers die kleiner zijn dan of gelijk is aan 50K per jaar en de positieve klasse vertegenwoordigt alle andere werknemers. Als in het scenario regressie zouden we trainen een model, score van bepaalde gegevens en evalueren van de resultaten. Het belangrijkste verschil hier is de keuze van de metrische gegevens Azure Machine Learning berekent en uitgangen. Ter illustratie van het inkomen niveau voorspelling scenario gebruiken we de [volwassen](http://archive.ics.uci.edu/ml/datasets/Adult) dataset maken een experiment Azure Machine Learning en evalueren van de prestaties van een twee-klasse logistische regressiemodel, een veelgebruikte binaire classificatie.

###<a name="creating-the-experiment"></a>Het Experiment maken###
De volgende modules toevoegen aan uw werkruimte in Azure Machine Learning Studio:

- Volwassen Census inkomsten binaire indeling dataset
- [Twee klasse logistische regressie][two-class-logistic-regression]
- [Model trein][train-model]
- [Score-Model][score-model]
- [Model evalueren][evaluate-model]

De poorten verbinden, zoals hieronder wordt weergegeven in figuur 5 en de Label-kolom van de [Trein Model] [ train-model] module *inkomen*.

![Evaluatie van een Model van de binaire indeling](media/machine-learning-evaluate-model-performance/5.png)

In figuur 5. Evaluatie van het Model van een binaire indeling.

###<a name="inspecting-the-evaluation-results"></a>Controleren van de evaluatieresultaten###
Nadat het experiment is uitgevoerd, kunt u klikken op de uitvoerpoort van het [Evalueren van Model] [ evaluate-model] module en selecteer *visualiseren* voor een overzicht van de resultaten van de evaluaties (figuur 7). De evaluatie voor binaire indeling modellen beschikbaar zijn: *nauwkeurigheid*, *precisie*, *intrekken*, *F1 Score*en *AUC*. Bovendien wordt de module een verwarring matrix het aantal true positieven, fout-negatieve resultaten, valse meldingen en true negatieven, alsmede *ROC* *Precisie/intrekken*en *til* curven.

Nauwkeurigheid is het aantal exemplaren correct ingedeeld. Het is meestal de eerste metric die u kijken bij het evalueren van een classificatie. Wanneer de testgegevens is echter niet in balans (waar de meeste van de exemplaren die behoren tot een van de klassen), of als u meer geïnteresseerd in de prestaties op een van de klassen, nauwkeurigheid niet echt vastleggen van de effectiviteit van een classificatie. In het scenario inkomen niveau-indeling, wordt ervan uitgegaan dat u test op bepaalde gegevens waar 99% van de gevallen vertegenwoordigen mensen die kleiner zijn dan of gelijk is aan 50K per jaar verdienen. Het is mogelijk een 0.99 nauwkeurigheid te bereiken door het voorspellen van de klasse "< = 50K ' voor alle exemplaren. De classificatie in dit geval lijkt algemeen goed werk doen, maar in werkelijkheid niet aan een van de personen in hoog inkomen (1%) correct classificeren.

Daarom is het handig voor het berekenen van extra statistieken die meer specifieke aspecten van de evaluatie. Voordat u gaat naar de details van deze statistieken, is het belangrijk dat u begrijpt de verwarring matrix van een evaluatie van de binaire indeling. De klasse labels in de trainingsset kunnen uitvoeren op slechts 2 mogelijke waarden, gewoonlijk naar verwezen als positief of negatief. De positieve en negatieve exemplaren die een classificatie correct voorspelt worden true positieven (TP) en de waarde true negatieven (TN), respectievelijk genoemd. Op dezelfde manier worden de exemplaren van de verkeerd ingedeelde vals-positief (FP) en fout-negatieve resultaten (FN) genoemd. De matrix verwarring is gewoon een tabel met het nummer van de instanties die onder elke categorie 4 vallen. Azure Machine Learning bepaalt automatisch welke van de twee klassen in de dataset is de positieve klasse. Als de klasse Boolean of gehele getallen, worden de positieve klasse toegewezen door de gelabelde exemplaren 'true' of '1'. De etiketten zijn tekenreeksen, zoals in het geval van de dataset inkomen, de etiketten alfabetisch worden gesorteerd als het eerste niveau zijn de negatieve klasse, terwijl het tweede niveau de positieve klasse is wordt gekozen.

![Binaire indeling verwarring Matrix](media/machine-learning-evaluate-model-performance/6a.png)

Figuur 6. Binaire indeling verwarring Matrix.

Gaat het probleem van de classificatie inkomsten, zou willen we vragen enkele evaluatievragen die ons helpen begrijpen de prestaties van de classificatie gebruikt. Een zeer natuurlijke vraag is: ' van de personen aan wie het model voorspeld te worden verdienen > 50 K (TP + FP) hoeveel waren ingedeeld correct (TP)?' Deze vraag kan worden beantwoord door te kijken naar de **precisie** van het model, dat is het deel van de meldingen die juist zijn ingedeeld: TP/(TP+FP). Een andere vraag is "verdienen werknemers met inkomsten uit alle hoge > 50 k (FN + TP), hoeveel de classificatie classificeren correct (TP)". Dit is eigenlijk de die **intrekken**of de true positieve koers: TP/(TP+FN) van de classificatie. U zult merken dat er sprake is van een voor de hand liggende compromis tussen precisie en intrekken. Bijvoorbeeld een dataset relatief evenwichtige gegeven, hebben een classificatie die grotendeels positieve gevallen voorspelt een hoge intrekken, maar een relatief laag precisie zoals veel van de negatieve exemplaren zou worden verkeerd geclassificeerd wat resulteert in een groot aantal onjuiste positieve detecties. Overzicht van de tekening van hoe deze twee parameters verschillen, kunt u op de curve precisie/INTREKKEN in de pagina evaluatie uitvoer (boven links deel van figuur 7).

![Binaire indeling evaluatieresultaten](media/machine-learning-evaluate-model-performance/7.png) in figuur 7. Binaire indeling evaluatieresultaten.

Een andere gerelateerde metric die vaak wordt gebruikt is de **Score F1**precision- en terughalen in aanmerking neemt. Het harmonische gemiddelde van deze maatstaven 2 is en als zodanig wordt berekend: F1 = 2 (precision x intrekken) / (precision + intrekken). De F1-score is een goede manier om de evaluatie in één getal samenvatten, maar het is altijd verstandig om zowel de precisie als intrekken samen om inzicht te krijgen in de werking van een classificatie te bekijken.

Bovendien controleert een de true positieve snelheid versus de waarde false positieve snelheid in de curve **Ontvanger operationele kenmerken (ROC)** en de waarde van het overeenkomstige **Gebied onder de Curve (AUC)** . Hoe dichter deze curve wordt in de linkerbovenhoek, des te beter de prestaties van de classificatie is (dat is het optimaliseren van de positieve waar snelheid tegelijkertijd de waarde false positieve snelheid). Curven die lijken op de diagonaal van het waarnemingspunt, het resultaat van de classificaties die doorgaans de voorspellingen die lijken op willekeurige raden.

###<a name="using-cross-validation"></a>Met behulp van Cross-validatie###
Zoals in het voorbeeld regressie kunnen we cross validatie herhaaldelijk trainen, score en evalueren van verschillende subsets van de gegevens automatisch uitvoeren. Op dezelfde manier gebruiken we het [Model kruislings gevalideerd] [ cross-validate-model] module, een ongetrainde logistische regressiemodel en een dataset. De labelkolom moet worden ingesteld op *inkomsten* in het [Model kruislings gevalideerd] [ cross-validate-model] eigenschappen van de module. Na het experiment wordt uitgevoerd en op de juiste uitvoerpoort van het [Model kruislings gevalideerd] [ cross-validate-model] module, ziet de binaire indeling metrische waarden voor elke vouwen, ook voor het gemiddelde en de standaarddeviatie van elke. 
 
![Een binaire indeling Model cross valideren](media/machine-learning-evaluate-model-performance/8.png)

Figuur 8. Cross-validatie van het Model van een binaire indeling.

![Resultaten van een binaire Classifier cross-validatie](media/machine-learning-evaluate-model-performance/9.png)

Figuur 9. Cross-validatie van de resultaten van een binaire Classifier.

##<a name="evaluating-a-multiclass-classification-model"></a>Evaluatie van een Model Multiclass-classificatie##
In dit experiment gebruiken we de populaire [Iris](http://archive.ics.uci.edu/ml/datasets/Iris "Iris") dataset die exemplaren van 3 verschillende typen (klassen) van de iris plant bevat. Er zijn 4 functie waarden (sepal lengte/breedte en lengte/breedte van Bloemblad) voor elk exemplaar. In de voorgaande proeven we getraind en de modellen met de dezelfde datasets getest. Hier gebruiken we de [Gesplitste gegevens] [ split] module 2 subsets van de gegevens maken, trainen op de eerste, en score en evalueren op de tweede. De dataset Iris openbaar beschikbaar is op de [UCI Machine Learning opslagplaats](http://archive.ics.uci.edu/ml/index.html)en kan worden gedownload met behulp van een [Gegevens importeren] [ import-data] module.

###<a name="creating-the-experiment"></a>Het Experiment maken###
De volgende modules toevoegen aan uw werkruimte in Azure Machine Learning Studio:

- [Gegevens importeren][import-data]
- [Multiclass besluit Forest][multiclass-decision-forest]
- [Gesplitste gegevens][split]
- [Model trein][train-model]
- [Score-Model][score-model]
- [Model evalueren][evaluate-model]

Verbinding maken met de poorten zoals hieronder wordt weergegeven in afbeelding 10.

Stel de kolomindex van de Label van het [Model van de trein] [ train-model] module tot en met 5. De dataset bevat geen veldnamenrij, maar we weten dat de klasse-labels in de vijfde kolom staan.

Klik op [Gegevens importeren] van de[ import-data] -module en de eigenschap *Data source* *Web via HTTP-URL*en de *URL* van http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data is ingesteld.

Instellen van de fractie van de exemplaren moeten worden gebruikt voor de opleiding in de [Gesplitste gegevens] [ split] module (0,7 bijvoorbeeld).
 
![Evaluatie van een Multiclass classificatie](media/machine-learning-evaluate-model-performance/10.png)

Figuur 10. Evaluatie van een Multiclass classificatie

###<a name="inspecting-the-evaluation-results"></a>Controleren van de evaluatieresultaten###
Uitvoeren van het experiment en klikt u op de uitvoerpoort van [Evalueren Model][evaluate-model]. Resultaten van de evaluaties worden gepresenteerd in de vorm van een matrix van verwarring, in dit geval. De matrix geeft de werkelijke waarden versus voorspelde exemplaren voor alle 3 klassen.
 
![Multiclass classificatie resultaten](media/machine-learning-evaluate-model-performance/11.png)

Figuur 11. Multiclass classificatie evaluatieresultaten.

###<a name="using-cross-validation"></a>Met behulp van Cross-validatie###
Zoals eerder vermeld, kunt u uitvoeren herhaalde opleiding, scoren en evaluaties die automatisch met het [Valideren van Cross-Model] [ cross-validate-model] module. U moet een dataset, een ongetrainde model en een [Model kruislings gevalideerd] [ cross-validate-model] module (Zie onderstaande afbeelding). Opnieuw moet u de labelkolom van het [Model kruislings gevalideerd] [ cross-validate-model] module (kolomindex 5 in dit geval). Nadat het experiment wordt uitgevoerd en het recht op uitvoer poort van het [Model kruislings gevalideerd][cross-validate-model], controleert u de metrische waarden voor elke vouwen, alsmede de gemiddelde en standaard afwijking. De hier weergegeven statistieken zijn de vergelijkbaar met die in het geval van de binaire indeling beschreven. Bedenk wel dat in multiclass-indeling, de true-positieve/negatieve en fout-positieve/negatieve resultaten gebeurt door telling op basis van per klasse als er geen klasse over het algemeen positief of negatief is. Bijvoorbeeld bij het berekenen van de precisie of intrekken van de klasse 'Iris setosa', wordt ervan uitgegaan dat dit de positieve klasse en alle andere negatief is.
 
![Een Model Multiclass classificatie cross valideren](media/machine-learning-evaluate-model-performance/12.png)

Figuur 12. Een Model Multiclass classificatie cross valideren.


![Cross-validatieresultaten van een Model Multiclass-classificatie](media/machine-learning-evaluate-model-performance/13.png)

Figuur 13. Cross-validatieresultaten van een Model Multiclass-classificatie.


<!-- Module References -->
[cross-validate-model]: https://msdn.microsoft.com/library/azure/75fb875d-6b86-4d46-8bcc-74261ade5826/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[multiclass-decision-forest]: https://msdn.microsoft.com/library/azure/5e70108d-2e44-45d9-86e8-94f37c68fe86/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-logistic-regression]: https://msdn.microsoft.com/library/azure/b0fd7660-eeed-43c5-9487-20d9cc79ed5d/
 

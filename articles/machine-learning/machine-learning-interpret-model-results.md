<properties
    pageTitle="Interpreteren van resultaten in de Machine Learning model | Microsoft Azure"
    description="Het kiezen van de optimale parameter is ingesteld voor het gebruik van een algoritme en score-model visualiseren-uitgang."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />


# <a name="interpret-model-results-in-azure-machine-learning"></a>Interpreteren van resultaten in Azure Machine Learning model

In dit onderwerp wordt uitgelegd hoe te visualiseren en te interpreteren resultaten voorspelling in Azure Machine Learning Studio. Nadat u hebt een model getraind en voorspellingen erop ("het model gescoord") wordt uitgevoerd, moet u begrijpen en interpreteren van het resultaat voorspelling.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Er zijn vier belangrijke soorten machine modellen in Azure Machine leren leren:

* Classificatie
* Clustering
* Regressie
* Recommender systemen

De modules gebruikt voor voorspelling op deze modellen zijn:

* [Score van Model] [ score-model] module voor de indeling en regressie
* [Toewijzen aan Clusters] [ assign-to-clusters] -module voor clustering
* [Score van Recommender Matchbox] [ score-matchbox-recommender] voor aanbeveling systemen

Dit document wordt uitgelegd hoe te voorspellen resultaten voor elk van deze modules worden geïnterpreteerd. Zie voor een overzicht van deze modules, [het kiezen van de parameters voor het optimaliseren van de algoritmen in Azure Machine Learning](machine-learning-algorithm-parameters-optimize.md).

In dit onderwerp komen voorspelling interpretatie, maar niet de model-evaluatie. Zie voor meer informatie over het evalueren van uw model, [het evalueren van model prestaties in Azure Machine Learning](machine-learning-evaluate-model-performance.md).

Als u bekend met Azure Machine Learning bent en hulp bij het maken van een eenvoudig experiment aan de slag, Zie [maken van een eenvoudig experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md) in Azure Machine Learning Studio.

## <a name="classification"></a>Classificatie ##
Er zijn twee subcategorieën van indeling problemen:

* Problemen met slechts twee klassen (twee-klasse of binaire indeling)
* Problemen met meer dan twee klassen (met meerdere klasse-indeling)

Azure Machine Learning heeft verschillende modules met elk van deze typen van de classificatie, maar de methoden voor het interpreteren van de resultaten van hun voorspelling zijn vergelijkbaar.

### <a name="two-class-classification"></a>Twee klasse-indeling###
**Voorbeeld van de proef**

Een voorbeeld van een probleem met twee klasse-indeling is de indeling van de iris bloemen. Het is de taak voor het classificeren van iris bloemen op basis van hun functies. De Iris gegevensset beschikbaar in Azure Machine Learning is een subset van de populaire [Iris gegevensset](http://en.wikipedia.org/wiki/Iris_flower_data_set) die de instanties van slechts twee soorten van bloem (klassen 0 en 1). Er zijn vier functies voor elke bloem (sepal lengte, breedte sepal Bloemblad lengte en breedte Bloemblad).

![Screenshot van iris experiment](./media/machine-learning-interpret-model-results/1.png)

Figuur 1. IRIS twee klasse-indeling probleem experiment

Een experiment is uitgevoerd om het oplossen van dit probleem, zoals in figuur 1. Een boomstructuur van twee klasse gestimuleerd besluit is opgeleid en gescoord. Nu kunt u de resultaten van de voorspelling van de [Score-Model] visualiseren[ score-model] module door te klikken op de uitvoerpoort van de [Score-Model] [ score-model] -module en klik vervolgens op **visualiseren**.

![Score model module](./media/machine-learning-interpret-model-results/1_1.png)

Hiermee de score resultaten zoals weergegeven in figuur 2.

![Resultaten van het experiment van iris twee klasse-indeling](./media/machine-learning-interpret-model-results/2.png)

Figuur 2. Een score-model resultaat in twee klasse-indeling visualiseren

**Interpretatie van de resultaten**

Er zijn zes kolommen in de tabel met resultaten. Vier kolommen aan de linkerkant zijn de vier functies. Het recht twee kolommen, Labels gescoord en kansen gescoord, zijn de resultaten voorspelling. De kolom gescoord kansen ziet u de kans dat een bloem behoort tot de positieve klasse (klasse 1). Zo is het eerste nummer in de kolom (0.028571) betekent dat er 0.028571 kans dat de eerste bloem deel uitmaakt van klasse 1. De kolom gescoord Labels geeft de voorspelde klasse voor elke bloem. Dit is gebaseerd op de kolom kansen gescoord. Als de kans op een bloem scored groter dan 0,5 is, is het voorspelde als klasse 1. Anders wordt het als klasse 0 voorspeld.

**Publicatie van Web Services**

Nadat de resultaten voorspelling zijn begrepen en goed beoordeeld, kan het experiment worden gepubliceerd als een webservice, zodat u kunt in diverse toepassingen implementeren en aanroepen voor de klasse voorspellingen op elke nieuwe iris bloem. Zie informatie over het wijzigen van een experiment training in een experiment score en publiceren als een webservice, [publiceren de Azure Machine Learning webservice](machine-learning-walkthrough-5-publish-web-service.md). Deze procedure biedt een score experiment zoals in figuur 3.

![Screenshot van het experiment te scoren](./media/machine-learning-interpret-model-results/3.png)

In figuur 3. Het experiment iris twee klasse-indeling probleem scoren

U moet nu ingesteld voor de invoer en uitvoer voor de webservice. De invoer is de juiste invoerpoort van [Score-Model][score-model], die de Iris bloem is uitgerust met input. De keuze van de uitvoer is afhankelijk van of u geïnteresseerd in de voorspelde klasse (gescoord label) of de kans op scored bent. In dit voorbeeld wordt ervan uitgegaan dat u geïnteresseerd in beide bent. De gewenste output als kolommen wilt selecteren, gebruikt u een [Kolommen selecteren in de gegevensset] [ select-columns] module. Klik op [Kolommen selecteren in de gegevensset][select-columns], klikt u op de **kolomkiezer starten**en selecteer **Etiketten gescoord** en **Kansen gescoord**. Na het instellen van de uitvoerpoort van [Kolommen in een gegevensset selecteren] [ select-columns] en opnieuw wordt uitgevoerd, moet u het scorepatroon experiment publiceren als een webservice door te klikken op **Publiceren WEB SERVICE**. Het laatste experiment eruitziet als figuur 4.

![Het experiment iris twee klasse-indeling](./media/machine-learning-interpret-model-results/4.png)

In figuur 4. Uiteindelijke score experiment van een probleem met de iris twee klasse-indeling

Nadat u de webservice wordt uitgevoerd en sommige waarden van de functie van een exemplaar van de test, resultaat het bestaat uit twee getallen. Het eerste getal is de scored label en de tweede is de kans op scored. Deze bloem wordt voorspeld als klasse 1 met kans op 0.9655.

![Test interpreteren score-model](./media/machine-learning-interpret-model-results/4_1.png)

![Resultaten scoren](./media/machine-learning-interpret-model-results/5.png)

In figuur 5. Web service resultaat van iris twee klasse-indeling

### <a name="multi-class-classification"></a>Meerdere klasse indeling
**Voorbeeld van de proef**

In dit experiment voert u een taak brief erkenning als een voorbeeld van een multiclass indeling. De classificatie wordt geprobeerd te voorspellen een bepaalde letter (klasse) op basis van enkele handgeschreven kenmerkwaarden geëxtraheerd uit de handgeschreven afbeeldingen.

![Voorbeeld van de brief erkenning](./media/machine-learning-interpret-model-results/5_1.png)

In de trainingsgegevens zijn er 16 functies geëxtraheerd uit handgeschreven letter afbeeldingen. De 26 letters vormen onze 26 klassen. Figuur 6 ziet u dezelfde functie instellen voor een data test set voor een proef die een model multiclass-classificatie voor de herkenning van de letter van de trein en voorspellen.

![Brief erkenning multiclass classificatie experiment](./media/machine-learning-interpret-model-results/6.png)

Figuur 6. Brief erkenning multiclass classificatie probleem experiment

Visualiseren van de resultaten van de [Score-Model] [ score-model] module door te klikken op de uitvoerpoort van de [Score-Model] [ score-model] -module en klik vervolgens op **visualiseren**, ziet u de inhoud zoals weergegeven in afbeelding 7.

![Score model resultaten](./media/machine-learning-interpret-model-results/7.png)

Figuur 7. Score model resultaten in een indeling met meerdere klasse visualiseren

**Interpretatie van de resultaten**

16 kolommen aan de linkerkant staan voor de waarden van de functie van de proef ingesteld. De kolommen met namen zoals gescoord kansen voor klasse 'XX' net worden zoals de kolom kansen gescoord in het geval van twee klasse. Zij geven de kans dat de overeenkomstige vermelding in een bepaalde categorie valt. Voor het eerste item is er bijvoorbeeld kans op 0.003571 is een 'A' 0.000451 kans is een "B", enzovoort. De laatste kolom (gescoord Labels) is hetzelfde als Labels gescoord in het geval van twee klasse. Deze selecteert de klasse met de grootste kans op scored als de voorspelde klasse van het corresponderende item. Voor het eerste item is het label scored bijvoorbeeld "F" omdat het heeft de grootste kans om een 'F' (0.916995).

**Publicatie van Web Services**

U kunt ook het label scored voor elke post en de waarschijnlijkheid van het label scored opvragen. De eenvoudige logica is de grootste kans tussen alle kansen scored vinden. Hiertoe moet u het [R-Script uitvoeren] [ execute-r-script] module. De R-code wordt weergegeven in afbeelding 8 en het resultaat van het experiment wordt weergegeven in afbeelding 9.

![Voorbeeld van de code R](./media/machine-learning-interpret-model-results/8.png)

Figuur 8. R-code voor het extraheren van Labels gescoord en de kansen van de labels

![Resultaat van de proef](./media/machine-learning-interpret-model-results/9.png)

Figuur 9. Uiteindelijke score experiment van het probleem van de classificatie van multiclass brief erkenning

Nadat u publiceren en uitvoeren van de webservice en voert u de waarden van sommige input functie, het geretourneerde resultaat ziet er net als in figuur 10. Deze handgeschreven brief met de uitgepakte 16 functies wordt voorspeld te zijn van een "T" met kans op 0.9715.

![Interpreteren score testmodule](./media/machine-learning-interpret-model-results/9_1.png)

![Testresultaat](./media/machine-learning-interpret-model-results/10.png)

Figuur 10. Web service-resultaat van de classificatie van de multiclass

## <a name="regression"></a>Regressie

Regressie-problemen worden verschillende problemen met de indeling. U wilt een probleem classificatie aparte klassen, zoals welke klasse een iris bloem te behoort voorspellen. Maar zoals u in het volgende voorbeeld van een regressie-probleem zien kunt, waarmee u wilt een continue variabele, bijvoorbeeld de prijs van een auto te voorspellen.

**Voorbeeld van de proef**

Auto prijs voorspelling gebruiken als uw voorbeeld voor regressie. U probeert te voorspellen van de prijs van een auto op basis van de functies, waaronder merk, brandstoftype, hoofdtekst en station wiel. Het experiment wordt weergegeven in figuur 11.

![Auto prijs regressie experiment](./media/machine-learning-interpret-model-results/11.png)

Figuur 11. Auto prijs regressie probleem experiment

Visualiseren van de [Score-Model] [ score-model] module, het resultaat lijkt op figuur 12.

![Resultaten scoren voor auto prijs voorspelling probleem](./media/machine-learning-interpret-model-results/12.png)

Figuur 12. Puntenwaardering voor het probleem auto prijs voorspelling

**Interpretatie van de resultaten**

Labels gescoord is de resultaatkolom in deze puntenwaardering. De nummers zijn de voorspelde prijs voor elke auto.

**Publicatie van Web Services**

U kunt het experiment regressie naar een webservice publiceert en voor auto prijs voorspelling aanroepen op dezelfde manier als in het geval van twee klasse-indeling gebruiken.

![Experiment scoren voor auto prijs regressie-probleem](./media/machine-learning-interpret-model-results/13.png)

Figuur 13. Scoren experiment van een auto prijs regressie-probleem

Met de webservice, het geretourneerde resultaat ziet eruit als in figuur 14. De voorspelde prijs voor deze auto is $15,085.52.

![Interpreteren score testmodule](./media/machine-learning-interpret-model-results/13_1.png)

![Module resultaten scoren](./media/machine-learning-interpret-model-results/14.png)

Figuur 14. Web-service-resultaat van een auto prijs regressie-probleem

## <a name="clustering"></a>Clustering

**Voorbeeld van de proef**

Laten we de gegevensset Iris opnieuw gebruiken om te bouwen van een experiment clustering. Hier kunt u filteren van de klasse etiketten in de gegevensset zodat deze alleen functies en voor clustering kan worden gebruikt. In deze iris use-case, geeft u het aantal clusters worden twee tijdens de training, wat betekent dat u zou de bloemen cluster in twee klassen. Het experiment wordt weergegeven in figuur 15.

![IRIS clustering probleem experimenteren](./media/machine-learning-interpret-model-results/15.png)

Figuur 15. IRIS clustering probleem experimenteren

Clustering verschilt van classificatie in de gegevensset training is geen grond waarheid etiketten zelf. De exemplaren van de gegevensset training groepen in verschillende clusters Clustering. Tijdens de training wordt labels het model de posten door te leren van de verschillen tussen de functies. Daarna wordt kan de opgeleide worden gebruikt voor het classificeren van toekomstige posten verder. Er zijn twee delen van het resultaat we geïnteresseerd in binnen een clustering probleem zijn. Het eerste deel is de gegevensset training labeling en de tweede classificeert een nieuwe gegevensgroep opgeleide model met.

Het eerste deel van het resultaat kan worden weergegeven door te klikken op de links-uitvoerpoort van de [Trein Clustering Model] [ train-clustering-model] en vervolgens te klikken op **visualiseren**. De visualisatie wordt weergegeven in afbeelding 16.

![Resultaat clustering](./media/machine-learning-interpret-model-results/16.png)

Figuur 16. Resultaat voor de gegevensset training clustering visualiseren

Het resultaat van het tweede deel, nieuwe posten opgeleide clustering model met clustering wordt weergegeven in afbeelding 17.

![Resultaat clustering visualiseren](./media/machine-learning-interpret-model-results/17.png)

Figuur 17. Clustering resultaat op een nieuwe set met gegevens visualiseren

**Interpretatie van de resultaten**

Hoewel de resultaten van de twee delen afkomstig zijn van verschillende experiment fasen, ze er hetzelfde uitzien en op dezelfde manier worden geïnterpreteerd. De eerste vier kolommen zijn functies. De laatste kolom, toewijzingen, is het resultaat voorspelling. Hetzelfde nummer toegewezen posten worden worden in hetzelfde cluster, dat wil zeggen, dat ze delen overeenkomsten op een bepaalde manier (dit experiment gebruikt de standaard afstand Euclidean metric) voorspeld. Omdat u het aantal clusters 2 hebt opgegeven, worden de posten in toewijzingen label 0 of 1.

**Publicatie van Web Services**

U kunt publiceren de clustering-experiment in een web-service en deze aanroepen voor clustering voorspellingen is hetzelfde als de twee klasse-indeling in use-case.

![Experiment scoren voor clustering probleem iris](./media/machine-learning-interpret-model-results/18.png)

Figuur 18. Scoren experiment van een iris clustering probleem

Na het uitvoeren van de webservice, lijkt het geretourneerde resultaat figuur 19. Deze bloem wordt in cluster 0 voorspeld.

![Test score module interpreteren](./media/machine-learning-interpret-model-results/18_1.png)

![Module puntenwaardering](./media/machine-learning-interpret-model-results/19.png)

Figuur 19. Web service resultaat van iris twee klasse-indeling

## <a name="recommender-system"></a>Recommender-systeem
**Voorbeeld van de proef**

Voor recommender systemen, kunt u het probleem van de aanbeveling restaurant als voorbeeld: u kunt restaurants aanbevelen voor klanten op basis van hun beoordeling geschiedenis. De ingevoerde gegevens bestaat uit drie delen:

* Restaurant beoordelingen van klanten
* Functie van klantgegevens
* Restaurant functiegegevens

Er zijn verschillende dingen we met de [Trein Matchbox Recommender doen] [ train-matchbox-recommender] in Azure Machine Learning module:

- Beoordelingen voor een bepaalde gebruiker en artikel voorspellen
- Aanbevolen items voor een bepaalde gebruiker
- Gebruikers die zijn gerelateerd aan een bepaalde gebruiker zoeken
- Zoeken naar items die betrekking hebben op een bepaald item.

U kunt kiezen wat u wilt doen door te selecteren uit de vier opties in het menu **type van Recommender voorspelling** . Hier kunt u alle vier scenario's doorlopen.

![Matchbox recommender](./media/machine-learning-interpret-model-results/19_1.png)

Een typisch experiment Azure Machine Learning voor een systeem van recommender eruitziet als figuur 20. Zie voor informatie over het gebruik van deze modules recommender-systeem [trein matchbox recommender] [ train-matchbox-recommender] en [Score matchbox recommender][score-matchbox-recommender].

![Experiment van recommender-systeem](./media/machine-learning-interpret-model-results/20.png)

Figuur 20. Experiment van recommender-systeem

**Interpretatie van de resultaten**

**Beoordelingen voor een bepaalde gebruiker en artikel voorspellen**

**Voorspelling van de score** onder **Recommender voorspelling type**selecteert, vraagt u de recommender-systeem te voorspellen de classificatie voor een bepaalde gebruiker en het artikel. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer eruitziet als figuur 21.

![Resultaat van het systeem van recommender--voorspelling score score](./media/machine-learning-interpret-model-results/21.png)

Figuur 21. Het resultaat van de score van de recommender-systeem--voorspelling score visualiseren

De eerste twee kolommen zijn de gebruiker item paren die door de ingevoerde gegevens. De derde kolom is de voorspelde classificatie van een gebruiker voor een bepaald artikel. In de eerste rij, de klant U1048 bijvoorbeeld naar snelheid restaurant 135026 voorspeld door 2.

**Aanbevolen items voor een bepaalde gebruiker**

**Artikel aanbeveling** onder **Recommender voorspelling type**selecteert, vraagt u de recommender-systeem aan te bevelen artikelen voor een bepaalde gebruiker. De laatste parameter te kiezen in dit scenario is *aanbevolen Artikelselectie*. De optie **Van het prioriteitsniveau van artikelen (evaluatie model)** is voornamelijk bedoeld voor model-evaluatie tijdens de training. We kiezen **Uit alle Items**voor deze fase voorspelling. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer eruitziet als figuur 22.

![Resultaat van de score van recommender-systeem--item aanbeveling](./media/machine-learning-interpret-model-results/22.png)

Figuur 22. Resultaat van het recommender-systeem item aanbeveling score visualiseren

De eerste zes kolommen vertegenwoordigt in de opgegeven gebruikers-id's aan te bevelen voor, zoals bepaald door de ingevoerde gegevens. De vijf kolommen vertegenwoordigen de items in aflopende volgorde van relevantie aan de gebruiker aanbevolen. In de eerste rij is het meest aanbevolen restaurant voor klant U1048 bijvoorbeeld 134986, gevolgd door 135018, 134975, 135021 en 132862.

**Gebruikers die zijn gerelateerd aan een bepaalde gebruiker zoeken**

**Verwante gebruikers** onder **Recommender voorspelling type**selecteert, vraagt u de recommender-systeem om verwante gebruikers aan een bepaalde gebruiker te zoeken. Verwante gebruikers zijn de gebruikers die dezelfde voorkeuren hebben. De laatste parameter te kiezen in dit scenario is *selectie van de gebruiker*. De optie **Van gebruikers dat nominale artikelen (voor model-evaluatie)** is voornamelijk bedoeld voor model-evaluatie tijdens de training. Kies uit alle **Gebruikers** voor deze fase voorspelling. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer ziet eruit als in figuur 23.

![Resultaat van recommender-systeem--gerelateerde gebruikers score](./media/machine-learning-interpret-model-results/23.png)

Figuur 23. Visualiseren van de resultaten van het recommender-systeem gerelateerde gebruikers score

De eerste zes kolommen bevat de opgegeven gebruiker id's die nodig zijn voor verwante gebruikers vinden zoals verstrekt door de ingevoerde gegevens. De vijf kolommen de voorspelde gerelateerde gebruikers van de gebruiker worden opgeslagen in aflopende volgorde van relevantie. In de eerste rij is de meest relevante klant voor klant U1048 bijvoorbeeld U1051, gevolgd door U1066, U1044, U1017 en U1072.

**Zoeken naar items die betrekking hebben op een bepaald item.**

Als u **Verwante Items** onder **Recommender voorspelling type**selecteert, vraagt u de recommender-systeem om te zoeken naar verwante items voor een bepaald item. Verwante items zijn de items die waarschijnlijk zullen worden door dezelfde gebruiker beviel. De laatste parameter te kiezen in dit scenario is *gerelateerd item selecteren*. De optie **Van het prioriteitsniveau van artikelen (evaluatie model)** is voornamelijk bedoeld voor model-evaluatie tijdens de training. We kiezen **Uit alle Items** voor deze fase voorspelling. De visualisatie van de [Score Matchbox Recommender] [ score-matchbox-recommender] uitvoer eruitziet als figuur 24.

![Resultaat van de score van recommender-systeem--gerelateerde artikelen](./media/machine-learning-interpret-model-results/24.png)

Figuur 24. Visualiseren van de resultaten van de score van het recommender-systeem gerelateerde artikelen

De eerste zes kolommen vertegenwoordigt voor het opgegeven item-id's die nodig zijn voor het zoeken van verwante items, zoals bepaald door de ingevoerde gegevens. De voorspelde gerelateerde items van het artikel opslaan de vijf kolommen in aflopende volgorde van relevantie. In de eerste rij is het meest relevant artikel voor artikel 135026, 135074, gevolgd door 135035, 132875, 135055 en 134992.

**Publicatie van Web Services**

Het proces voor het publiceren van deze experimenten als webservices om voorspellingen lijkt voor elk van de vier scenario's. Hier nemen we het tweede scenario (aanbevolen items voor een bepaalde gebruiker) als voorbeeld. U kunt met de andere drie dezelfde procedure volgen.

U kunt de getrainde recommender-systeem op te slaan als een getrainde model en de invoergegevens voor een enkele gebruiker-ID-kolom filteren, zoals gevraagd, het experiment als in figuur 25 aansluiten en publiceren als een webservice.

![Proef van het probleem van de aanbeveling restaurant scoren](./media/machine-learning-interpret-model-results/25.png)

Figuur 25. Proef van het probleem van de aanbeveling restaurant scoren

Met de webservice, het geretourneerde resultaat ziet eruit als in figuur 26. De vijf aanbevolen restaurants voor gebruiker U1048 zijn 134986, 135018, 134975, 135021 en 132862.

![Voorbeeld van recommender-systeemservice](./media/machine-learning-interpret-model-results/25_1.png)

![Voorbeeldresultaten van het experiment](./media/machine-learning-interpret-model-results/26.png)

Figuur 26. Web service resultaat van restaurant aanbeveling probleem


<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/

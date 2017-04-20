<properties
   pageTitle="Een antwoord met een eenvoudig model - regressiemodel voorspellen | Microsoft Azure"
   description="Het maken van een eenvoudige regressiemodel om te voorspellen een prijs in de wetenschap voor Beginners video 4 gegevens. Een lineaire regressie met doelgegevens bevat."                                  
   keywords="Maak een model, eenvoudig model, prijs voorspelling, eenvoudige regressiemodel"
   services="machine-learning"
   documentationCenter="na"
   authors="cjgronlund"
   manager="jhubbard"
   editor="cjgronlund"/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="cgronlun;garye"/>

# <a name="predict-an-answer-with-a-simple-model"></a>Een antwoord met een eenvoudige model te voorspellen

## <a name="video-4-data-science-for-beginners-series"></a>Video 4: Gegevens wetenschap voor Beginners reeks

Informatie over het maken van een eenvoudige regressiemodel te voorspellen wat de prijs van een ruit in de wetenschap voor Beginners video 4 gegevens. We zullen een regressiemodel met doelgegevens tekenen.

Als u optimaal gebruik van de reeks, kijken ze allemaal. [Ga naar de lijst van video 's](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-predict-an-answer-with-a-simple-model]

## <a name="other-videos-in-this-series"></a>Andere video's in deze serie

*Gegevens van wetenschap voor Beginners* is een korte inleiding data science in vijf korte video's.

  * Video 1: [5 vragen gegevens wetenschap antwoorden](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec.)*
  * Video 2: [uw gegevens gereed Is voor data science?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min. 56 sec.)*
  * Video 3: [Stel een vraag kunt u beantwoorden met gegevens](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec.)*
  * Video 4: Voorspellen een antwoord met een eenvoudig model
  * Video 5: [Werk van anderen om uit te voeren wetenschappelijke gegevens kopiëren](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>Transcriptie: Voorspellen een antwoord met een eenvoudig model

Welkom bij de vierde video in de 'gegevens wetenschap voor Beginners"serie. In dit voorbeeld, we een eenvoudige model te bouwen en een voorspelling te maken.

Een *model* is een vereenvoudigde verhalen over onze gegevens. Ik ziet u bedoel ik.

## <a name="collect-relevant-accurate-connected-enough-data"></a>Verzamelen relevante, nauwkeurige, verbonden, voldoende gegevens

Zeg dat ik wil om te winkelen voor een ruit. Ik heb een ring die deel uitmaakten van mijn oma met een instelling voor een 1.35 karaat diamant en ik wil een idee krijgen van hoeveel het kost. Ik neem een Kladblok en een pen in het archief van juwelen en ik schrijf de prijs van de ruiten in de behuizing en hoeveel ze wegen in carats. Beginnend met de eerste ruit - van 1.01 carats en $7,366.

Ik Ga nu door en doe dit voor alle andere ruiten in de winkel.

![Kolommen met gegevens van de ruit](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

U ziet dat onze lijst met twee kolommen. Elke kolom heeft een ander kenmerk - gewicht in carats en prijs- en elke rij is één gegevenspunt dat een enkel ruit staat.

Wij hebben een kleine daadwerkelijk gemaakt gegevens hier - een tabel instellen. U ziet dat het voldoet aan onze criteria voor kwaliteit:

* De gegevens zijn **relevante** - gewicht wel degelijk betrekking heeft op prijs
* **Dit klopt** - we goed gecontroleerd met de prijzen die we noteren
* Het is **verbonden** : Er zijn geen spaties in een van deze kolommen
* En zullen we zien, is het **genoeg** gegevens om onze vraag te beantwoorden

## <a name="ask-a-sharp-question"></a>Vraag het de scherpe

Nu we onze vraag op een scherpe manier inhouden zult: "hoeveel kost het om te kopen van een diamant karaat 1.35?"

Onze lijst geen heeft een 1.35 karaat diamant in, zodat we met de rest van onze gegevens opgehaald van een antwoord op de vraag hebt.

## <a name="plot-the-existing-data"></a>De bestaande gegevens uitzetten

Het eerste wat dat we doen is een horizontale lijn nummer genoemd as, om het gewicht van de grafiek tekenen. Het bereik van de gewichten is 0-2, dus we zullen een lijn tekenen die betrekking heeft op dat bereik en maten voor elke helft karaat plaatsen.

Vervolgens zullen we een verticale as wilt opnemen van de prijs en verbind deze met de as horizontaal gewicht tekenen. Dit is in eenheden van dollars. Nu hebben we een reeks coördinaten-assen.

![Gewicht en prijs assen](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

Gaan we nu deze gegevens halen en in een *spreidingsdiagram uitzetten*. Dit is een uitstekende manier om sets van numerieke gegevens visualiseren.

We krijgen van een verticale lijn op carats 1.01 voor het eerste gegevenspunt. Vervolgens krijgen we een horizontale lijn op $7,366. Indien zij voldoen aan, tekenen wij een punt. Dit is onze eerste ruit.

Nu we gaan via elke ruit op deze lijst en hetzelfde doen. Wanneer we dit weet, is dit krijgen we: een aantal punten, één voor elke ruit.

![Spreiding waarnemingspunt](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-the-model-through-the-data-points"></a>Het model door de gegevenspunten te tekenen

Nu als u naar de punten en squint kijkt, de collectie ziet eruit als een lijn fat, onduidelijk. We kunnen onze markering nemen en teken een rechte lijn doorheen.

Tekent een lijn, maken we een *model*. Beschouw dit als het nemen van de echte wereld en het maken van een eenvoudig cartoon versie van deze. Nu de cartoon is het probleem - de regel niet doorlopen alle gegevenspunten. Maar het is een nuttige vereenvoudiging.

![Lineaire regressielijn](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

Het feit dat alle punten gaan niet precies door de regel is OK. Gegevens wetenschappers verklaren dit door te zeggen dat het model - dat is de regel - en vervolgens elke stip heeft sommige *ruis* of *afwijking* is gekoppeld. Er is de onderliggende relatie perfect en is de ingaan, echte wereld die ruis en onzekerheid worden toegevoegd.

Omdat de vraag te beantwoorden hebben we geprobeerd *hoeveel?* dit is een *regressie*genoemd. En omdat we een rechte lijn, is een *lineaire regressie*.

## <a name="use-the-model-to-find-the-answer"></a>Het model gebruiken om het antwoord te vinden

Nu we een model hebben en we onze vraag vragen: hoeveel kost een 1.35 karaat diamant?

Antwoord op onze vraag, we krijgen van 1.35 carats en een verticale lijn tekenen. Waar deze kruist de lijn model krijgen we van een horizontale lijn op de as van de dollar. Het treft recht op 10.000. Boom! Dat is het antwoord: een 1.35 karaat diamant ongeveer 10.000 dollar kosten.

![Het antwoord vinden op het model](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Maak een betrouwbaarheidsinterval

Het is vanzelfsprekend vraagt zich af hoe nauwkeurig deze voorspelling. Het is handig om te weten of de ruit 1.35 karaat zeer dicht bij $10.000 wordt, of veel hoger of lager. Als u wilt dit uit figuur, we tekenen een omhulsel rond de regressielijn die bevat het merendeel van de punten. Deze envelop heet onze *betrouwbaarheidsinterval*: We zijn vrij zeker van dat de prijzen binnen deze envelop vallen, omdat in het verleden de meeste ze hebben. We tekenen twee meer horizontale lijnen van waar de regel 1.35 karaat de bovenkant en de onderkant van de envelop kruist.

![Betrouwbaarheidsinterval](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Nu we iets over onze betrouwbaarheidsinterval zeggen kunnen: We vol vertrouwen kunt zeggen dat de prijs van een 1.35 karaat diamant ongeveer $10.000 - maar is het misschien zo laag als $8.000 en mogelijk zo hoog als $12.000.

## <a name="were-done-with-no-math-or-computers"></a>We klaar bent, zonder wiskunde of computers

We hebben wat gegevens wetenschappers betaald krijgen om te doen en we hebben het gewoon door te tekenen:

* We een vraag heeft gesteld dat we met de gegevens kan beantwoorden
* We gebruikten een *model* met *lineaire regressie*
* Wij een *voorspelling*gemaakt, met een *betrouwbaarheidsinterval*

En we wiskunde of computers hebt gebruikt om het te doen.

Nu, als we meer informatie hadden gehad, zoals...

* het knippen van de ruit
* kleurvariaties (hoe dicht de ruit worden wordt wit)
* het aantal opnames in de ruit

.. .en wij zouden hebben meer kolommen. In dat geval wordt de wiskundige nuttig. Als u meer dan twee kolommen hebt, is het moeilijk punten op papier te tekenen. De wiskunde kunt u die regel of dit vlak in uw gegevens zeer netjes past.

Ook als in plaats van slechts een handvol ruiten, hadden we tweeduizend of twee miljoen en vervolgens kunt u veel sneller dat werk doen met een computer.

Vandaag, wij hebben gesproken over het lineaire regressie en we een voorspelling met gegevens aangebracht.

Zorg ervoor dat de andere video's in 'gegevens wetenschap voor Beginners"van Microsoft Azure Machine Learning uitchecken.



## <a name="next-steps"></a>Volgende stappen

  * [Probeer een eerste data science experiment met Machine Learning Studio](machine-learning-create-experiment.md)
  * [Kennismaking met de computer leren op Microsoft Azure](machine-learning-what-is-machine-learning.md)

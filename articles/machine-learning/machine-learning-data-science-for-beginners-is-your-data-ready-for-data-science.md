<properties
   pageTitle="Uw gegevens gereed is voor data science? Evaluatie van de gegevens | Microsoft Azure"
   description="Informatie over de 4 criteria voor gegevens klaar voor wetenschappelijke gegevens. Wetenschap voor Beginners video 2 gegevens heeft concrete voorbeelden om te helpen bij de evaluatie van de basisgegevens."
   keywords="relevante gegevens evalueren van gegevens, gegevens, gegevens criteria, gereed gegevens voorbereiden"
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


# <a name="is-your-data-ready-for-data-science"></a>Uw gegevens gereed is voor data science?

## <a name="video-2-data-science-for-beginners-series"></a>Video 2: Gegevens wetenschap voor Beginners reeks

Informatie over het bepalen van uw gegevens om te controleren of het voldoet aan fundamentele criteria klaar voor wetenschappelijke gegevens.

Als u optimaal gebruik van de reeks, kijken ze allemaal. [Ga naar de lijst van video 's](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-is-your-data-ready-for-data-science]

## <a name="other-videos-in-this-series"></a>Andere video's in deze serie

*Gegevens van wetenschap voor Beginners* is een korte inleiding data science in vijf korte video's.

  * Video 1: [5 vragen gegevens wetenschap antwoorden](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec.)*
  * Video 2: Uw gegevens gereed Is voor data science?
  * Video 3: [Stel een vraag kunt u beantwoorden met gegevens](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec.)*
  * Video 4: [Een antwoord met een eenvoudige model te voorspellen](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sec.)*
  * Video 5: [Werk van anderen om uit te voeren wetenschappelijke gegevens kopiëren](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## <a name="transcript-is-your-data-ready-for-data-science"></a>Transcriptie: Uw gegevens gereed Is voor data science?

Welkom bij "Uw gegevens gereed Is voor data science?" de tweede video in de reeks *Gegevens wetenschap voor Beginners*.  

Voordat gegevens wetenschap u de gewenste antwoorden geeft, moet u geven sommige hoogwaardige grondstoffen werken met. Net als het een pizza, hoe beter de ingrediënten die u, hoe beter het eindproduct begint.

## <a name="criteria-for-data"></a>Criteria voor de gegevens

Dus in het geval van wetenschappelijke gegevens zijn sommige ingrediënten die we nodig hebben om te verzamelen.

We moeten de gegevens die zijn:

  * Relevante
  * Verbonden
  * Nauwkeurige
  * Genoeg om te werken met

## <a name="is-your-data-relevant"></a>Zijn uw gegevens relevant?

Dus het eerste ingrediënt - moeten we gegevens die relevant zijn.

![Relevante gegevens versus niet-relevante gegevens - de gegevens evalueren](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-relevant-and-irrelevant-data.png)

Bekijk de tabel aan de linkerkant. Wij zeven personen buiten Boston balken voldaan, hun bloed alcohol niveau, het gemiddelde van de Red Sox batting in hun laatste spel en de prijs van melk in de dichtstbijzijnde winkel gemeten.

Dit zijn alle perfect betrouwbare gegevens. Het enige probleem is dat niet relevant. Er is geen duidelijke relatie tussen de getallen. Als ik u heeft gegeven de huidige prijs van melk en de gemiddelde rode Sox batting, is er geen manier u de inhoud van mijn bloed alcohol kon raden.

Bekijk de tabel aan de rechterkant. Nu we van elke persoon de gemeten massa body en het aantal dranken al geteld.  De nummers in elke rij zijn nu relevant voor elkaar. Als ik u Mijn lichaam gegeven massa en het nummer van mijn Margaritas, u kunt een schatting op mijn bloed alcohol inhoud.

## <a name="do-you-have-connected-data"></a>U hebt gegevens aangesloten?

De volgende ingrediënten zijn verbonden gegevens.

![Gekoppelde gegevens versus verbroken gegevens - criteria gegevens gereed](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-connected-vs-disconnected-data.png)

Hier ziet u enkele relevante gegevens over de kwaliteit van hamburgers: grill temperatuur, patty gewicht en classificatie in de lokale voedsel tijdschrift. Maar let op de gaten in de tabel aan de linkerkant.

Enkele waarden ontbreken in de meeste gegevenssets. Is het gebruikelijk om gaten als volgt en manieren om ze te omzeilen. Maar als er te veel ontbreekt, uw gegevens als Zwitserse kaas begint.

Als u naar de tabel aan de linkerkant kijkt, is het dus veel ontbrekende gegevens is het moeilijk om te komen met de aard van de relatie tussen temperatuur en patty gewicht van rooster. Dit is een voorbeeld van gegevens verbroken.

De tabel aan de rechterkant, echter is vol en is voltooid - een voorbeeld van de gekoppelde gegevens.

## <a name="is-your-data-accurate"></a>Uw gegevens correct is?

De volgende ingrediënten die we nodig hebben is de nauwkeurigheid. Hier vindt u vier doelen die wij graag bestookt met pijlen.

![Nauwkeurige gegevens vergeleken met onjuiste gegevens - gegevens criteria](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-inaccurate-vs-accurate-data.png)

Kijk naar het doel in de rechterbovenhoek. Wij hebben een hechte groep rechts rond de roos. Dat is natuurlijk juist. Oddly, in de taal van de wetenschap gegevens onze prestaties op rechts onder het doel wordt ook beschouwd als accuraat.

Als u het midden van deze pijlen worden toegewezen, ziet u dat het lijkt erg veel op de roos. De pijlen zijn verdeeld rondom het doel, zodat deze worden beschouwd als onnauwkeurige, maar ze zijn gecentreerd rond de roos, zodat deze worden beschouwd als accuraat.

Bekijk nu het doel van de linkerbovenhoek. Onze pijlen Druk hier heel dicht bij elkaar staan, een hechte groep. Ze nauwkeurig zijn, maar ze zijn niet juist omdat het midden manier uit de roos. En natuurlijk de pijlen in het doel van de links onder zijn onjuiste en onnauwkeurige. Deze archer moet oefenen.

## <a name="do-you-have-enough-data-to-work-with"></a>Hebt u voldoende gegevens om met te werken?

Ten slotte ingrediënt #4 - moeten we voldoende gegevens hebben.

![Hebt u voldoende gegevens voor analyse? Evaluatie van de gegevens](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-barely-enough-data.png)

Beschouw elk gegevenspunt in een tabel als een penseelstreek in een schilderij. Als er maar een paar van hen, het schilderij is tamelijk vaag - het is moeilijk te zien wat het is.

Als u sommige meer penseelstreken, vervolgens begint uw schilderij te halen een beetje scherper.

Wanneer er nauwelijks genoeg streken, ziet u net genoeg om sommige grote beslissingen te nemen. Is het ergens die mogelijk wil bezoeken? Ziet er helder, dat ziet er schoon water – Ja, dat is waar ik ga op vakantie.

Als u meer gegevens toevoegt, wordt de afbeelding duidelijker wordt en u meer gedetailleerde beslissingen kunt nemen. Nu kan ik de drie hotels op de bank links bekijken. U weet, bevalt me uitstekend de architecturale functies van het op de voorgrond. Ik blijven staan, op de derde verdieping.

Met relevante, verbonden, nauwkeurige gegevens en genoeg, we hebben alle ingrediënten die we moeten doen sommige wetenschappelijke gegevens van hoge kwaliteit.

Zorg ervoor dat de andere vier video's in de *Wetenschap voor Beginners gegevens* van Microsoft Azure Machine Learning uitchecken.




## <a name="next-steps"></a>Volgende stappen

  * [Probeer een eerste data science experiment met Machine Learning Studio](machine-learning-create-experiment.md)
  * [Kennismaking met de computer leren op Microsoft Azure](machine-learning-what-is-machine-learning.md)

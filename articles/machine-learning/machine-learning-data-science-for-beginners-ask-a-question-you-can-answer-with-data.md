<properties
   pageTitle="U beantwoordt een vraag formuleren met gegevens - vragen | Microsoft Azure"
   description="Informatie over het formuleren van een vraag in de wetenschap voor Beginners video 3 gegevens wetenschap. Bevat een vergelijking van de indeling en regressie vragen."
   keywords="Data science vragen, vragen, vragen regressie, classificatievragen formuleren, scherpe vraag"
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

# <a name="ask-a-question-you-can-answer-with-data"></a>Stel een vraag die u met gegevens beantwoorden kunt

## <a name="video-3-data-science-for-beginners-series"></a>Video 3: Gegevens wetenschap voor Beginners reeks

Informatie over het formuleren van een vraag in de wetenschap voor Beginners video 3 gegevens wetenschap. Deze video bevat een vergelijking van de vragen met betrekking tot de indeling en regressie algoritmen.

Als u optimaal gebruik van de reeks, kijken ze allemaal. [Ga naar de lijst van video 's](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-ask-a-question-you-can-answer-with-data]

## <a name="other-videos-in-this-series"></a>Andere video's in deze serie

*Gegevens van wetenschap voor Beginners* is een korte inleiding data science in vijf korte video's.

  * Video 1: [5 vragen gegevens wetenschap antwoorden](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 sec.)*
  * Video 2: [uw gegevens gereed Is voor data science?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min. 56 sec.)*
  * Video 3: Een vraag die u met gegevens beantwoorden kunt
  * Video 4: [Een antwoord met een eenvoudige model te voorspellen](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sec.)*
  * Video 5: [Werk van anderen om uit te voeren wetenschappelijke gegevens kopiëren](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Transcriptie: Een vraag die u met gegevens beantwoorden kunt

Welkom bij de derde video in de reeks "Wetenschap voor Beginners gegevens".  

In dit voorbeeld krijgt u enkele tips voor het formuleren van een vraag die u met gegevens beantwoorden kunt.

U kunt optimaal profiteren van deze video, als u eerst de twee eerdere video's in deze serie kijken: "data science 5 vragen kunt beantwoorden" en "Is uw gegevens gereed is voor data science?"

## <a name="ask-a-sharp-question"></a>Vraag het de scherpe

Wij hebben gesproken over hoe wetenschappelijke gegevens het proces is van het gebruik van namen (ook wel categorieën of labels genoemd) en getallen te voorspellen een antwoord op een vraag. Maar niet elke vraag; Er moet een *vraag scherpe.*

Een vaag vraag hoeft niet te worden beantwoord met een naam of een nummer. Er moet een sterke vraag.

Stel dat u een magische lamp met een genie die zal vragen die u vragen naar waarheid beantwoorden gevonden. Het is een genie mischievous maar hij proberen te brengen van zijn antwoord als vaag en verwarrend zijn als hij weg met krijgen kunt. U wilt vastmaken hem af met een vraag dus waterdichte hij kan niet helpen maar vertellen wat u wilt weten.

Als u een vraag vaag, zoals 'Wat gaat er gebeuren met mijn voorraad?', het genie kan beantwoorden, 'de prijs wordt wijzigen'. Dat is een ware antwoord, maar is niet erg handig.

Maar als u een scherpe vraag, zoals "Wat de verkoopprijs van mijn aandeel worden volgende week?", de genie kan niet helpen maar geeft u een specifieke vragen zijn beantwoord en voorspellen een verkoopprijs.

## <a name="examples-of-your-answer-target-data"></a>Voorbeelden van uw antwoord: doel van de gegevens

Nadat u uw vraag formuleren, controleert u of er voorbeelden van het antwoord in uw gegevens.

Als de vraag "Wat de verkoopprijs van mijn aandeel worden volgende week?" vervolgens hebben we om ervoor te zorgen dat onze gegevens vindt u een overzicht van de aandelenkoers.

Als de vraag "welke auto in mijn vloot zal eerst mislukt?" vervolgens hebben we om ervoor te zorgen dat onze gegevens bevatten informatie over eerdere fouten.

![Doelgegevens - voorbeelden van uw antwoord. Het formuleren van een vraag van de wetenschappelijke gegevens.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-target-data.png)

Voorbeelden van antwoorden, een doel worden genoemd. Een doel is wat we probeert te voorspellen over toekomstige gegevenspunten, ongeacht of het een categorie of een getal.

Als u geen doelgegevens, moet u het krijgt. Niet mogelijk om het antwoord op uw vraag zonder.

## <a name="reformulate-your-question"></a>Reformulate uw vraag

Soms kunt u uw vraag om een beter antwoord uitgelegd.

De vraag "Is deze gegevens punt A of B?" de categorie (of de naam of label) van iets voorspelt. Om te reageren, maar we gebruiken een *classificatie-algoritme*.

De vraag "Hoeveel?" of "Hoeveel?" voorspelt een bedrag. We gebruiken een *regressie-algoritme*om te reageren.

Als u wilt zien hoe we deze kunnen transformeren, we kijken naar de vraag 'welke nieuws verhaal is het meest interessante aan deze lezer?' Wordt de gebruiker gevraagd een voorspelling van een enkele keuze uit de vele mogelijkheden - met andere woorden "Is dit A of B of C of D?" - en een classificatie-algoritme zou gebruiken.

Maar deze vraag wellicht gemakkelijker te beantwoorden als u uitgelegd als "hoe interessant is elk artikel op deze lijst voor deze lezer?" Nu kunt u geven elk artikel een numerieke score en vervolgens gemakkelijk te identificeren van de hoogst scorende artikel is. Dit is een van de vraag van de classificatie in een regressie vraag formuleren of hoeveel?

![Reformulate uw vraag. Classificatie vraag versus vraag regressie.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-classification-question-vs-regression-question.png)

Hoe vraagt u dat een vraag is een aanwijzing aan welk algoritme krijgt u een antwoord.

Hebt u bepaalde families van algoritmen - zoals de knoppen in het nieuws verhaal voorbeeld - zijn nauw verwant. U kunt uw vraag voor het gebruik van het algoritme waarmee u het meest nuttig antwoord reformulate.

Maar meest belangrijke vraag die scherp - de vraag die u met gegevens beantwoorden kunt. En zorg dat de juiste gegevens antwoord.

Wij hebben gesproken over bepaalde basisprincipes voor u een vraag stelt dat kunt u met de gegevens beantwoorden.

Zorg ervoor dat de andere video's in 'gegevens wetenschap voor Beginners"van Microsoft Azure Machine Learning uitchecken.


## <a name="next-steps"></a>Volgende stappen

  * [Probeer een eerste data science experiment met Machine Learning Studio](machine-learning-create-experiment.md)
  * [Kennismaking met de computer leren op Microsoft Azure](machine-learning-what-is-machine-learning.md)

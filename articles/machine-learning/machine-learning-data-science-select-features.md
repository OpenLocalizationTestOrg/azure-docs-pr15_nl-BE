<properties
    pageTitle="De selectie in het Team gegevens wetenschap proces functie | Microsoft Azure" 
    description="Het doel van de functieselectie en voorbeelden van hun rol in het proces van de uitbreiding van gegevens van machine learning."
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
    ms.date="09/19/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>Functies selecteren in het Team gegevens wetenschap proces (TDSP)

In dit artikel wordt de toepassing van de Functieselectie uitgelegd en vindt u voorbeelden van haar rol in het proces van de uitbreiding van gegevens van machine learning. Deze voorbeelden zijn ontleend aan Azure Machine Learning Studio. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


In dit onderwerp wordt het doel van de Functieselectie uitgelegd en vindt u voorbeelden van haar rol in het proces van de uitbreiding van gegevens van machine learning. Deze voorbeelden zijn ontleend aan Azure Machine Learning Studio. 

Engineering en selectie van functies is een onderdeel van de TDSP die worden beschreven in de [Wat is het proces van Team Data Science?](data-science-process-overview.md). Functie engineering en selectie maken deel uit van de **functies ontwikkelen** stap van de TDSP.

* **functie engineering**: dit proces wordt geprobeerd extra relevante functies maken van de bestaande raw functies in de gegevens en om voorspellende kracht op het algoritme leren te vergroten.

* **functies selecteren**: dit proces de belangrijkste deel van de oorspronkelijke gegevensfuncties selecteert in een poging tot het verminderen van de dimensionaliteit van het probleem van de opleiding.

Normaal **engineering functie** eerst toegepast voor het genereren van extra functies en vervolgens de **Functieselectie** stap wordt uitgevoerd om de functies niet relevant, redundante of sterk gecorreleerde elimineren.


## <a name="filtering-features-from-your-data---feature-selection"></a>Filterfuncties van uw gegevens - functies selecteren 

Functieselectie is een proces dat gewoonlijk wordt toegepast voor de bouw van datasets training voor voorspellende modelleertaken als taken classificatie of regressie. Het doel is om een subset van de functies van de originele dataset waarmee de afmetingen worden verminderd met behulp van een minimale set van functies voor de maximale hoeveelheid variantie in de gegevens te selecteren. Dit deel van de functies zijn vervolgens de functies alleen te worden opgenomen in het model van de trein. Functieselectie dient twee hoofddoelen.

* Eerst Functieselectie verhoogt de nauwkeurigheid van de classificatie vaak doordat niet relevant, redundante of sterke correlatie bestaat tussen functies.
* Ten tweede neemt het aantal functies waardoor het trainingsproces model efficiënter af. Dit is vooral belangrijk voor cursisten die echter duur zijn om te trainen zoals support vector machines.

Hoewel de Functieselectie proberen te verminderen het aantal functies in de gegevensset die wordt gebruikt om het model van de trein, wordt niet meestal aangeduid met de term "dimensionaliteit vermindering". Functie selectiemethoden Pak een subset van de oorspronkelijke functies van de gegevens niet wijzigen.  Dimensionaliteit reductie methoden gebruiken engineering functies kunnen transformeren de oorspronkelijke functies en aldus aan te passen. Voorbeelden van dimensionaliteit reductie methoden zijn principaal onderdeel analyse en canonieke correlatieanalyse enkelvoud waarde ontleding.

Onder andere is een alom toegepaste categorie selectiemethoden functie in een gecontroleerde context "filteren op basis van functieselectie" genoemd. Deze methoden gelden door de correlatie tussen elke functie en het doelkenmerk te evalueren, een statistische eenheid een score om aan te wijzen elke functie. De voorzieningen zijn vervolgens gerangschikt op de score, die kunnen worden gebruikt om u te helpen bij het instellen van de drempel voor het bewaren of verwijderen van een specifieke functie. Voorbeelden van de statistische metingen die worden gebruikt in deze methoden zijn persoon correlatie, wederzijdse informatie en de test Chi-kwadraat.

In Azure Machine Learning Studio zijn er modules vervatte functies selecteren. In de volgende afbeelding ziet deze modules bevatten [functies selecteren op basis van een Filter] [ filter-based-feature-selection] en [Fisher lineaire Discriminant analyse][fisher-linear-discriminant-analysis].

![Voorbeeld van de functie selectie](./media/machine-learning-data-science-select-features/feature-Selection.png)


Bijvoorbeeld, kunt u het gebruik van de [functies selecteren op basis van een Filter] [ filter-based-feature-selection] module. Voor het gemak blijven we tekst mijnbouw voorbeeld hierboven beschreven. Stel dat we opbouwen van een regressiemodel willen nadat een set van 256 functies zijn gemaakt door de [Hash-functie] [ feature-hashing] module, en dat de variabele antwoord is de "Col1" vertegenwoordigt een boek lees beoordelingen variërend van 1 tot 5. "Functie score methode" door "Pearson correlatie", de kolom"doel" zijn "Col1" en het "aantal gewenste functies" 50. Vervolgens de module [functies selecteren op basis van een Filter] [ filter-based-feature-selection] produceert een dataset met 50 onderdelen van het kenmerk target "Col1". De volgende afbeelding ziet u de stroom van dit experiment en de invoerparameters die hierboven wordt beschreven.

![Voorbeeld van de functie selectie](./media/machine-learning-data-science-select-features/feature-Selection1.png)

De volgende afbeelding ziet u de resulterende datasets. Elke functie is op basis van de Pearson-correlatie tussen zichzelf en het kenmerk target "Col1" gescoord. De functies met de hoogste scores worden gehouden.

![Voorbeeld van de functie selectie](./media/machine-learning-data-science-select-features/feature-Selection2.png)

De bijbehorende scores van de geselecteerde onderdelen worden weergegeven in de volgende afbeelding.

![Voorbeeld van de functie selectie](./media/machine-learning-data-science-select-features/feature-Selection3.png)

Door deze [functies selecteren op basis van een Filter] toe te passen[ filter-based-feature-selection] 50 van 256 functies zijn geselecteerd, omdat ze hebben de meeste gecorreleerde functies met de variabele doel "Col1," module op basis van de score "Pearson correlatie"-methode.

## <a name="conclusion"></a>Conclusie
Functie engineering en functies selecteren zijn twee veelgebruikte engineering en geselecteerde onderdelen verhogen de efficiëntie van het trainingsproces die pogingen om uit te pakken van belangrijke informatie die is opgenomen in de gegevens. Ze verbeteren ook de kracht van deze modellen voor het classificeren van de ingevoerde gegevens nauwkeurig te voorspellen resultaten van rente meer krachtig. Functie engineering en selectie kunnen ook combineren om het leerproces meer rekenkundig tractable. Dit gebeurt door verbetering en vervolgens het aantal functies die nodig zijn voor het kalibreren of een model van de trein te verminderen. Wiskundig gezien zijn de onderdelen geselecteerd om te leiden van het model een minimaal aantal onafhankelijke variabelen die de patronen in de gegevens wordt uitgelegd en vervolgens resultaten is te voorspellen.

Houd er rekening mee dat het is niet altijd noodzakelijk voor het uitvoeren van functies engineering of functie selecteren. Of het nodig is of niet hangt af van de gegevens die we hebben of verzamelen, de algoritme die wij verzamelen en het doel van het experiment.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
 

<properties
    pageTitle="Een eenvoudig experiment in Machine Learning Studio | Microsoft Azure"
    description="Deze machine learning zelfstudie doorloopt u een eenvoudige data science experiment. We zullen de prijs van een auto met behulp van een algoritme regressie voorspellen."
    keywords="experimenteren, lineaire regressie, machine learning algoritmen, machine learning zelfstudie, voorspellende modellen technieken, data science experiment"
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
    ms.topic="hero-article"
    ms.date="07/14/2016"
    ms.author="garye"/>

# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Machine learning zelfstudie: de eerste data science experiment in Azure Machine Learning Studio maken

Deze machine learning zelfstudie doorloopt u een eenvoudige data science experiment. Maken we een lineaire regressiemodel voorspelt de prijs van een auto op basis van verschillende variabelen zoals maken en technische specificaties. Hiervoor gebruikt Azure Machine Learning Studio te ontwikkelen en te herhalen op een eenvoudige voorspellende analytics experimenteren.

*Voorspellende analytics* is een soort van wetenschappelijke gegevens die de huidige gegevens gebruikt om te voorspellen van toekomstige resultaten. Bekijk voor een heel eenvoudig voorbeeld van anticiperende analytische gegevens wetenschap voor Beginners video 4: [voorspellen een antwoord met een eenvoudig model](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) (runtime: 7:42).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>Hoe beschermt de computer leren Studio?

Machine Learning Studio kunt u gemakkelijk instellen van een experiment met behulp van slepen en neerzetten modules voorgeprogrammeerde met modelleren voorspellende technieken. Uw experiment uitvoeren en een antwoord te voorspellen, kunt u Machine Learning Studio *maken een model*, *het model trein*en *score en test het model*.

Geef de Machine Learning Studio: [https://studio.azureml.net](https://studio.azureml.net). Als u naar Machine Learning Studio voordat u zich heeft aangemeld, klikt u op **aanmelden hier**. Anders klikt u op **Aanmelden** en gratis en betaalde opties kiezen.

Zie voor meer informatie over de Machine Learning Studio [Wat is Machine Learning Studio?](machine-learning-what-is-ml-studio.md)

## <a name="five-steps-to-create-an-experiment"></a>Vijf stappen voor het maken van een experiment

In deze zelfstudie leren-computer voert u vijf eenvoudige stappen voor het opbouwen van een experiment in Machine Learning Studio maken, trainen en het model van uw score:

- Een model maken
    - [Stap 1: Get gegevens]
    - [Stap 2: Gegevens voorverwerken]
    - [Stap 3: Functies definiëren]
- Het model van de trein
    - [Stap 4: Kiezen en toepassen van een leeralgoritme]
- Score en het model te testen
    - [Stap 5: Prijzen voor nieuwe auto's voorspellen]

[Stap 1: Get gegevens]: #step-1-get-data
[Stap 2: Gegevens voorverwerken]: #step-2-preprocess-data
[Stap 3: Functies definiëren]: #step-3-define-features
[Stap 4: Kiezen en toepassen van een leeralgoritme]: #step-4-choose-and-apply-a-learning-algorithm
[Stap 5: Prijzen voor nieuwe auto's voorspellen]: #step-5-predict-new-automobile-prices


## <a name="step-1-get-data"></a>Stap 1: Get gegevens

Er zijn een aantal voorbeeld datasets met Machine Learning Studio kunt u kiezen uit opgenomen en u kunt gegevens importeren uit vele bronnen. In dit voorbeeld gebruiken we de meegeleverde voorbeeldpagina dataset, **auto prijsgegevens (Raw)**.
Deze dataset bevat vermeldingen voor een aantal afzonderlijke auto, inclusief gegevens zoals het merk, model, technische specificaties en prijs.

1. Een nieuw experiment starten door te klikken op **+ Nieuw** onder aan het venster Machine Learning Studio **EXPERIMENTEREN**, en selecteer vervolgens Selecteer **Lege experimenteren**. Selecteer de naam van het experiment standaard boven aan het canvas en wijzig de herkenbare, bijvoorbeeld **auto prijs voorspelling**.

2. Canvas is links van het experiment een palet van datasets en modules. Type **auto** in het zoekvak boven aan dit palet vinden de dataset label **auto prijsgegevens (Raw)**.

    ![Palet zoeken][screen1a]

3. Sleep de dataset naar het canvas experiment.

    ![DataSet][screen1]

Om te zien hoe deze gegevens eruit, de uitvoerpoort op de onderkant van de auto dataset op en selecteer vervolgens **visualiseren**.

![Module output-poort][screen1c]

De variabelen in de dataset worden weergegeven als kolommen en elk exemplaar van een auto als een rij wordt weergegeven. De kolom uiterst rechts (kolom 26 en genaamd "prijs") is de variabele doel is gaan we proberen te voorspellen.

![DataSet visualisatie][screen1b]

Sluit het visualisatievenster door te klikken op de '**x**' in de rechterbovenhoek.

## <a name="step-2-preprocess-data"></a>Stap 2: Gegevens voorverwerken

Een gegevensset is meestal nodig sommige voorverwerken voordat deze kan worden geanalyseerd. Hebt misschien u opgemerkt waarden ontbreken in de kolommen van verschillende rijen aanwezig. Deze ontbrekende waarden moeten worden gereinigd, zodat het model goed de gegevens te analyseren. In ons geval verwijdert alle rijen die ontbrekende waarden. De kolom **genormaliseerd verliezen** heeft ook een groot deel van de waarden ontbreken, dus we die kolom helemaal van het model uitsluiten zullen.

> [AZURE.TIP] Reinigen van de ontbrekende waarden van invoergegevens is een vereiste voor het gebruik van de meeste van de modules.

Intrekt we de kolom **genormaliseerd verliezen** en zullen we er ontbreken gegevens in een rij verwijdert.

1. Typen **kolommen selecteren** in het vak Zoeken boven aan het palet module vinden de [Kolommen selecteren in de Dataset] [ select-columns] module en sleep om het experiment canvas en verbind deze met de uitvoerpoort van de dataset **auto prijsgegevens (Raw)** . In deze module kunt u selecteren welke kolommen met gegevens u wilt opnemen of uitsluiten in het model.

2. Selecteer de [Kolommen selecteren in de Dataset] [ select-columns] module en klikt u op **starten kolomkiezer klikken** in het deelvenster **Eigenschappen** .

    - Klik op de links, **met regels**
    - **Beginnen met**, klik op **alle kolommen**. Dit zorgt ervoor dat [De kolommen selecteren in de Dataset] [ select-columns] door de kolommen (behalve we bijna uitsluiten).
    - Selecteer **uitsluiten** en **kolomnamen**uit de vervolgkeuzelijsten en klik vervolgens in het tekstvak. Er wordt een lijst met kolommen weergegeven. Selecteer **genormaliseerd verliezen**, en het wordt toegevoegd aan het tekstvak.
    - Klik op het vinkje (OK) om de kolomkiezer.

    ![Kolommen selecteren][screen3]

    Het deelvenster Eigenschappen voor **Kolommen selecteren in de Dataset** geeft aan dat deze wordt geaccepteerd door alle kolommen uit de dataset, met uitzondering van **genormaliseerde verliezen**.

    ![Kolommen selecteren in het dialoogvenster Eigenschappen van de Dataset][screen4]

    > [AZURE.TIP] U kunt een opmerking toevoegen aan een module door te dubbelklikken op de module en het invoeren van tekst. Zo kunt u in één oogopslag zien wat de module doet in het experiment. In dat geval dubbelklikt u op de [Kolommen selecteren in de Dataset] [ select-columns] module en typ de opmerking "uitsluiten genormaliseerd verliezen."

3. Sleep de [Schone ontbrekende gegevens] [ clean-missing-data] module voor het experiment canvas en verbind deze met de [Kolommen selecteren in de Dataset] [ select-columns] module. Selecteer de **gehele rij verwijderen** onder **Schoonmaak-modus** om de gegevens opschonen door het verwijderen van rijen met ontbrekende waarden in het deelvenster **Eigenschappen** . Dubbelklik op de module en typ de opmerking "Ontbrekende waarde rijen verwijderen".

    ![Eigenschappen van schone ontbrekende gegevens][screen4a]

4. Het experiment uitvoeren door te klikken op **uitvoeren** in het canvas experiment.

Als het experiment is voltooid, zijn alle modules een groen vinkje om aan te geven dat deze is voltooid. U ziet ook de status **Gereedgemeld uitgevoerd** in de rechterbovenhoek.

![Eerste experiment uitvoeren][screen5]

We hebben gedaan in het experiment op dit punt is de gegevens opschonen. Als u wilt dat de wachtrij is opgeschoond gegevensset weergeven, klikt u op de links-uitvoerpoort van de [Schone ontbrekende gegevens] [ clean-missing-data] module ('opgeschoonde dataset") en selecteer **visualiseren**. U ziet dat de kolom **genormaliseerd verliezen** niet meer opgenomen is en er geen ontbrekende waarden zijn.

De gegevens is schoon, dus we klaar om op te geven welke functies gaan we in de voorspellende model gebruiken.

## <a name="step-3-define-features"></a>Stap 3: Functies definiëren

*Functies* zijn in de machine learning, afzonderlijke meetbare eigenschappen van iets waarin je geïnteresseerd bent. Elke rij vertegenwoordigt één auto in onze dataset, en elke kolom is een functie van die auto.

Zoeken naar een goede vereist set van functies voor het maken van een anticiperende model experimenten en kennis over het probleem dat u wilt oplossen. Sommige functies zijn beter voor het voorspellen van het doel dan andere. Ook sommige functies hebben een sterke correlatie met andere functies (bijvoorbeeld stad-mpg versus weg mpg), zodat deze wordt niet echt nieuwe informatie toegevoegd aan het model en kunnen worden verwijderd.

We maken een model dat een subset van de functies in onze dataset wordt gebruikt. U kunt terugkomen en selecteert u verschillende functies, opnieuw uitvoeren van het experiment en Zie als u betere resultaten. Maar om te beginnen, maar eens de volgende functies:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Sleep een andere [Kolommen selecteren in de Dataset] [ select-columns] module voor het experiment canvas en verbind deze met de links-uitvoerpoort van de [Schone ontbrekende gegevens] [ clean-missing-data] module. Dubbelklik op de module en typ 'Select functies voor voorspelling'.

2. Klik in het deelvenster **Eigenschappen** op **kolomkiezer te starten** .

3. Klik op **regels**.

4. Onder **Beginnen met** **geen kolommen**op en selecteer vervolgens **opnemen** en **kolomnamen** in de filterrij. Onze lijst van kolomnamen invoeren. Dit zorgt ervoor dat de module alleen kolommen die we passeren.

    > [AZURE.TIP] Door het experiment, we de kolomdefinities voor onze gegevens van de dataset doorgeven via de [Schone ontbrekende gegevens] hebt gezorgd[ clean-missing-data] module. Dit betekent dat andere modules die u hebt ook gegevens uit de gegevensset.

5. Klik op de knop check mark (OK).

![Kolommen selecteren][screen6]

Dit resulteert in de gegevensset die wordt gebruikt in het algoritme leren in de volgende stappen. U kunt later terug en probeer het opnieuw met een andere selectie van functies.

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>Stap 4: Kiezen en toepassen van een leeralgoritme

Nu dat de gegevens klaar is, bestaat maken een voorspellende model uit training en testen. Volgens onze gegevens gebruiken we het model van de trein en test vervolgens het model om te zien hoe dicht het kunnen voorspellen van de prijzen is. Op dit moment niet bang waarom we moeten trainen en te testen of een model.

*Indeling* en *regressie* zijn twee soorten onder toezicht staande machine technieken te leren. Classificatie voorspelt een antwoord van een gedefinieerde set categorieën, zoals een kleur (rood, blauw of groen). Regressie wordt gebruikt voor het voorspellen van een getal.

Aangezien we het voorspellen van prijs, een getal is, gebruiken we een regressiemodel. In dit voorbeeld zullen we een eenvoudige *lineaire regressie* model trainen en in de volgende stap, we zullen het testen.

1. We gebruiken onze gegevens voor training en testen opsplitsen in afzonderlijke opleiding en sets te testen. Selecteer en sleep de [Gesplitste gegevens] [ split] module voor het experiment canvas en verbind deze met de uitvoer van de laatste [Kolommen selecteren in de Dataset] [ select-columns] module. **Breuk van de rijen in de eerste uitvoer dataset** ingesteld op 0,75. Op deze manier we gebruiken 75 procent van de gegevens naar het model van de trein en 25 procent voor het testen kunnen hinderen.

    > [AZURE.TIP] Als u de parameter **willekeurige seed** wijzigt, kunt u verschillende aselecte steekproeven voor training en testen van produceren. Deze parameter bepaalt de seeding van de pseudo-willekeurige getallen.

2. Het experiment wordt uitgevoerd. Hierdoor kan de [Kolommen selecteren in de Dataset] [ select-columns] en de [Gesplitste gegevens] [ split] modules kolomdefinities aan de modules worden er nog volgende doorgeven.  

3. U selecteert het algoritme leren, vouw de categorie **Computer leren** in het palet van de module aan de linkerkant van het papier en vouw **Model initialiseren**. Hier worden opgedeeld in verschillende categorieën van modules die kunnen worden gebruikt voor het initialiseren van machine learning algoritmen.

    Selecteer de [Lineaire regressie] voor dit experiment,[ linear-regression] module onder de categorie **regressie** (vindt u ook de module "lineaire regressie" typt in het zoekvak palet) en sleep deze naar het canvas experiment.

4. Zoeken en sleept u het [Model trein] [ train-model] module aan het canvas experiment. De invoer links poort verbinding met de uitvoer van de [Lineaire regressie] [ linear-regression] module. Verbinding maken met de juiste invoerpoort training data output (poort links) van de [Gesplitste gegevens] [ split] module.

5. Selecteer het [Model van de trein] [ train-model] module, klikt u op **starten kolomkiezer klikken** in het deelvenster **Eigenschappen** en selecteer vervolgens de kolom **prijs** . Dit is de waarde die u ons model wilt voorspellen.

    ![Selecteer de kolom "prijs"][screen7]

6. Het experiment wordt uitgevoerd.

Het resultaat is een getrainde regressiemodel die kan worden gebruikt voor het verkrijgen van nieuwe monsters worden voorspellingen.

![Het algoritme van machine learning toepassen][screen8]

## <a name="step-5-predict-new-automobile-prices"></a>Stap 5: Prijzen voor nieuwe auto's voorspellen

Nu dat we het model met 75 procent van onze gegevens hebt getraind, we Hiermee kunt scoren de overige 25 procent van de gegevens om te zien hoe goed onze functies model.

1. Zoek en sleep het [Score-Model] [ score-model] module voor het experiment canvas en de invoerpoort links verbinden met de uitvoer van de [Trein Model] [ train-model] module. Verbinding maken met de juiste poort voor invoer aan de test data uitvoer (rechts poort) van de [Gesplitste gegevens] [ split] module.  

    ![Score Model module][screen8a]

2. Voor het uitvoeren van het experiment en weergave van de uitvoer uit het [Score-Model] [ score-model] module, klikt u op de haven van uitvoer en selecteer vervolgens **visualiseren**. De uitvoer bevat de voorspelde waarden voor prijs en de bekende waarden van de experimentele gegevens.  

3. Ten slotte, om de kwaliteit van de resultaten testen, selecteert en sleept u het [Evalueren van Model] [ evaluate-model] module voor het experiment canvas en de invoerpoort links verbinden met de uitvoer van de [Score-Model] [ score-model] module. (Er zijn twee ingangspoorten omdat het [Evalueren van Model] [ evaluate-model] module kan worden gebruikt voor het vergelijken van twee modellen.)

4. Het experiment wordt uitgevoerd.

Om de weergave van de uitvoer uit het [Evalueren van Model] [ evaluate-model] module, klikt u op de haven van uitvoer en selecteer vervolgens **visualiseren**. De volgende statistieken weergegeven voor het model:

- **Gemiddelde Absolute fout** (MAE): het gemiddelde van de absolute fouten (een *fout* is het verschil tussen de voorspelde waarde en de werkelijke waarde).
- **Gemiddelde gekwadrateerde fout Root** (RMSE): de vierkantswortel van het gemiddelde van het kwadraat fouten van voorspellingen van de dataset test gedaan.
- **Relatieve, Absolute fout**: het gemiddelde van de absolute fouten ten opzichte van het absolute verschil tussen de werkelijke waarden en het gemiddelde van alle werkelijke waarden.
- **Relatieve gekwadrateerde fout**: het gemiddelde van het kwadraat fouten ten opzichte van het kwadraat van het verschil tussen de werkelijke waarden en het gemiddelde van alle werkelijke waarden.
- **Coëfficiënt van bepaling**: ook bekend als de **waarde voor R-kwadraat**, dit is een statistische waarde die aangeeft hoe goed een model past bij de gegevens.

Voor elk van de fout is het beter de statistieken kleiner. Een kleinere waarde geeft aan dat de voorspellingen meer overeenkomt met de werkelijke waarden. Voor de **Coëfficiënt van de bepaling**, hoe dichter de waarde ervan is een (1.0), des te beter de voorspellingen.

![Evaluatieresultaten][screen9]

Het laatste experiment ziet er zo uit:

![Machine learning zelfstudie: lineaire regressie experiment met modelleren voorspellende technieken te voltooien.][screen10]

## <a name="next-steps"></a>Volgende stappen

Nu dat u een eerste machine learning zelfstudie hebt voltooid en uw experiment instellen, kunt u proberen te verbeteren van het model sequentieel. Bijvoorbeeld, kunt u de functies die u in uw voorspelling gebruikt. Of u kunt de eigenschappen van een [Lineaire regressie] wijzigen[ linear-regression] algoritme of probeer een ander algoritme helemaal niet. Zelfs meerdere computer leren uw experiment algoritmen in één keer toevoegen en vergelijken van twee met behulp van het [Evalueren van Model] [ evaluate-model] module.

> [AZURE.TIP] Gebruik de knop **Opslaan als** onder het canvas experiment een herhaling van het experiment te kopiëren. Alle herhalingen van het experiment kunt u bekijken door te klikken op **Uitvoeren geschiedenis weergeven** onder het canvas. Zie [beheren experimenteren iteraties in Azure Machine Learning Studio] [ runhistory] voor meer informatie.

[runhistory]: machine-learning-manage-experiment-iterations.md

Als u tevreden met uw model bent, kunt u deze implementeren als een webservice worden gebruikt om te voorspellen auto prijzen op basis van nieuwe gegevens. [Implementeren van een webservice Azure Machine Learning] Zie[ publish] voor meer informatie.

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Zie [een voorspellende oplossing via Azure Machine Learning ontwikkelen]voor een meer uitgebreid en gedetailleerd overzicht van voorspellende modellering technieken voor het maken van opleiding, scoren en implementeren van een model,[walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]:./media/machine-learning-create-experiment/screen1.png
[screen1a]:./media/machine-learning-create-experiment/screen1a.png
[screen1b]:./media/machine-learning-create-experiment/screen1b.png
[screen1c]: ./media/machine-learning-create-experiment/screen1c.png
[screen2]:./media/machine-learning-create-experiment/screen2.png
[screen3]:./media/machine-learning-create-experiment/screen3.png
[screen4]:./media/machine-learning-create-experiment/screen4.png
[screen4a]:./media/machine-learning-create-experiment/screen4a.png
[screen5]:./media/machine-learning-create-experiment/screen5.png
[screen6]:./media/machine-learning-create-experiment/screen6.png
[screen7]:./media/machine-learning-create-experiment/screen7.png
[screen8]:./media/machine-learning-create-experiment/screen8.png
[screen8a]:./media/machine-learning-create-experiment/screen8a.png
[screen9]:./media/machine-learning-create-experiment/screen9.png
[screen10]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

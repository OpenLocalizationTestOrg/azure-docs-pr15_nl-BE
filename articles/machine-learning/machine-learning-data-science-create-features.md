<properties
    pageTitle="Engineering in het proces van Cortana Analytics functie | Microsoft Azure" 
    description="De toepassing van de technische functie worden uitgelegd en vindt u voorbeelden van haar rol in het proces van de uitbreiding van gegevens van machine learning."
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


# <a name="feature-engineering-in-the-cortana-analytics-process"></a>In het proces van Cortana Analytics-functie engineering 

In dit onderwerp wordt uitgelegd van de toepassing van de functie engineering en voorbeelden van haar rol in het proces van de uitbreiding van gegevens van machine learning. De voorbeelden ter illustratie van dit proces wordt gebruikt worden in Azure Machine Learning Studio getekend. 

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Dit **menu** bevat koppelingen naar onderwerpen over het maken van functies voor gegevens in diverse omgevingen. Deze taak is een stap in het [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Functie engineering pogingen te verhogen van de voorspellende kracht van algoritmen leren door het maken van functies van onbewerkte gegevens die helpen bij het leerproces te bevorderen. Engineering en selectie van functies is een onderdeel van de TDSP die worden beschreven in de [Wat is het proces van Team Data Science?](data-science-process-overview.md) Functie engineering en selectie maken deel uit van de **functies ontwikkelen** stap van de TDSP. 

* **functie engineering**: dit proces wordt geprobeerd extra relevante functies maken van de bestaande raw functies in de gegevens en de voorspellende kracht van het algoritme leren te vergroten.

* **functies selecteren**: dit proces de belangrijkste deel van de oorspronkelijke gegevensfuncties selecteert in een poging tot het verminderen van de dimensionaliteit van het probleem van de opleiding.

Normaal **engineering functie** eerst toegepast voor het genereren van extra functies en vervolgens de **Functieselectie** stap wordt uitgevoerd om de functies niet relevant, redundante of sterk gecorreleerde elimineren.

De trainingsgegevens in de machine learning worden vaak verbeterd extractie van functies uit de onbewerkte gegevens verzameld. Een voorbeeld van een functie-engineering is toegepast in het kader van leren voor het classificeren van de afbeeldingen van handgeschreven tekens is het maken van een beetje dichtheid kaart gemaakt op basis van het gegevenstype raw bit distributie. Dit overzicht kunt de randen van de tekens nog efficiënter te gebruiken om de distributie van onbewerkte direct zoeken.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="creating-features-from-your-data---feature-engineering"></a>Functies maken van uw gegevens - functie Engineering

De gegevens van de opleiding bestaat uit een matrix bestaat uit voorbeelden (records of waarnemingen die zijn opgeslagen in rijen) een aantal voorzieningen (variabelen of velden die zijn opgeslagen in de kolommen zijn). De functies die zijn opgegeven in de proefopzet worden verwacht om aan te geven van de patronen in de gegevens. Hoewel veel van de velden van onbewerkte gegevens kunnen rechtstreeks worden opgenomen in de geselecteerde functie gebruikt voor het trainen van een model, is het vaak het geval dat extra (Engineering) functies worden gemaakt op basis van de functies in de onbewerkte gegevens moeten voor het genereren van een uitgebreide training dataset.

Wat voor soort functies moet worden gemaakt voor het verbeteren van de dataset als een model training? Engineering functies die de opleiding verbeteren bevatten informatie waarmee de patronen in de gegevens beter te onderscheiden. We verwachten dat de nieuwe functies om aanvullende gegevens die niet duidelijk vastgelegde of gemakkelijk zichtbaar in de oorspronkelijke of een bestaande functie. Maar dit proces is iets van een illustratie. Gezonde en productieve beslissingen vereist vaak bepaalde expertise in verschillende domeinen.

Wanneer u start met Azure Machine Learning, het gemakkelijkst om te leren van dit proces concrete invulling te geven met behulp van voorbeelden in de Studio. Twee voorbeelden worden hier weergegeven:

* Een voorbeeld van regressie [voorspelling van het nummer van de huur van de fiets](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) in een gecontroleerde experiment waarbij de streefwaarden zijn bekend
* Een voorbeeld text mining classificatie met [Hash-functie](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/)

### <a name="example-1-adding-temporal-features-for-regression-model"></a>Voorbeeld 1: Tijdelijke functies toe te voegen voor het regressiemodel ###

We gebruiken het experiment "Vraag het voorspellen van fietsen" in Azure Machine Learning Studio-engineering-functies voor een taak regressie voorbeelden. Het doel van dit experiment is om te voorspellen van de vraag naar de fietsen, dat wil zeggen het aantal fiets huur in een specifieke maand/dag/uur. Dataset ' fiets verhuur UCI dataset ' wordt gebruikt als de onbewerkte gegevens zijn ingevoerd. Deze dataset is gebaseerd op werkelijke gegevens van het bedrijf Bikeshare kapitaal die een netwerk van fiets verhuur in Washington D.C. in de Verenigde Staten onderhoudt. De dataset vertegenwoordigt het nummer van de huur van de fiets binnen een bepaald uur van een dag in het jaar 2011 en het jaar 2012 en bevat 17379 rijen en kolommen op 17. Raw-functie bevat de weersomstandigheden (temperatuur/luchtvochtigheid/wind snelheid) en het type van de dag (vakantie/dag). Het veld te voorspellen is "cnt", een aantal dat de huur van de fiets binnen een specifiek uur vertegenwoordigt en die kan variëren van 1 tot en met 977 bereiken.

Met het doel van het maken van effectieve functies in de trainingsgegevens, vier regressie modellen zijn opgebouwd via dezelfde algoritme maar met vier verschillende training datasets. De vier gegevenssets dezelfde ruwe invoergegevens vertegenwoordigen, maar met een toenemend aantal functies instellen. Deze functies zijn ingedeeld in vier categorieën:

1. A = weer + feestdagen + weekdag + functies voor de voorspelde dag weekend
2. B = het aantal fietsen dat in elk van de vorige 12 uur zijn gehuurd
3. C = het aantal fietsen dat in elk van de afgelopen 12 dagen op hetzelfde uur zijn gehuurd
4. D = het aantal fietsen dat in elk van de voorbije 12 weken op hetzelfde uur en dezelfde dag zijn gehuurd

Naast de functie set A, in de originele, onbewerkte gegevens bestaat, worden de drie sets van functies via de functie voor engineering proces gemaakt. Functie set B opnamen zeer recente vraag naar de fietsen. Functie set C opnamen de vraag naar fietsen op een bepaald uur. D opnamen vraag naar fietsen functie instellen op een bepaald uur en een bepaalde dag van de week. De vier training datasets die elke bevat functie set A, A + B, A + B + C en A + B + C + D, respectievelijk.

Deze vier training datasets worden in het experiment Azure Machine Learning gevormd via vier vertakkingen uit het voorbehandelde invoer dataset. Behalve de linkerkant bevat de meeste filialen, elk van deze filialen van een module [Execute R Script](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) , waarin een reeks functies (functie ingesteld B, C en D) afgeleid respectievelijk gebouwd zijn en toegevoegd aan de geïmporteerde gegevensset. In de volgende afbeelding ziet u het R-script gebruikt voor het maken van de functieset B in de tweede tak van de linker.

![functies maken](./media/machine-learning-data-science-create-features/addFeature-Rscripts.png)

De vergelijking van de prestatieresultaten van de vier modellen zijn samengevat in de volgende tabel. De beste resultaten worden weergegeven door functies van A + B + C. Opmerking de fout snelheid afneemt wanneer extra functies zijn opgenomen in de trainingsgegevens. Controle van ons vermoeden dat de functieset B, C, aanvullende relevante informatie voor de taak regressie bieden. Maar niet de functie D toe te voegen lijkt te bieden een extra verlaging van het tarief van de fout.

![vergelijking van de resultaten](./media/machine-learning-data-science-create-features/result1.png)

### <a name="example2"></a>Voorbeeld 2: Functies maken in tekst mijnbouw  

Functie engineering wordt veel toegepast in taken met betrekking tot de mijnbouw-tekst, zoals de indeling en het sentiment documentenanalyse. Bijvoorbeeld wanneer we klasseren van documenten in verschillende categorieën willen, is een typische hypothese dat het woord/zinnen opgenomen in één document categorie minder zullen optreden in een andere categorie van doc. Met andere woorden: de frequentie van de verdeling van de woorden/zinnen kan verschillende documentcategorieën kenmerkt. In tekst-mining toepassingen omdat afzonderlijke stukjes tekst inhoud wordt gewoonlijk als de ingevoerde gegevens gebruikt is de functie engineering proces nodig voor het maken van de functies met betrekking tot de frequenties woord of woordgroep.

Voor deze taak, wordt een **hash-functie** genoemd techniek toegepast om efficiënt tekstfuncties willekeurige omzetten in indexen. In plaats van elke functie (woorden/zinnen) koppelen aan een bepaalde index, deze methode werkt door het toepassen van een hash-functie op de functies en de hash-waarden als indices rechtstreeks.

In Azure Machine Learning, wordt een [Hash-functie](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) module die wordt gemaakt van dit woord of woordgroep functies gemakkelijk. Onderstaande figuur toont een voorbeeld van het gebruik van deze module. De invoer dataset bestaat uit twee kolommen: het boek rating variërend van 1 tot 5, en de inhoud van de feitelijke controle. Het doel van deze module [Hash-functie](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) is voor het ophalen van een aantal nieuwe functies die tonen de frequentie van het exemplaar van de bijbehorende tekst / r-zin(nen) binnen een bepaald boek bekijken. Deze module gebruikt, moeten we de volgende stappen uit:

* Selecteer eerst de kolom met de ingevoerde tekst ("Kol2" in dit voorbeeld).
* Ten tweede instellen "Hashing bitsize" tot en met 8, wat betekent dat 2 ^ 8 = 256 functies wordt gemaakt. Het woord/fase in de tekst wordt naar 256 indices opdelen. De parameter "Hashing bitsize" varieert van 1 tot en met 31. De woorden / r-zin(nen) opdelen in dezelfde index als het een groter getal worden minder snel zijn.
* Ten derde Stel de parameter "N g" 2. Dit zijn de frequentie van voorkomen van unigrams (een functie voor elk één woord) en bigrams (een functie voor elk paar aangrenzende woorden) afkomstig uit de ingevoerde tekst. De parameter "N g" bereik van 0 tot 10, geeft het maximum aantal opeenvolgende woorden in een voorziening moet worden opgenomen.  

![Module "Hashing functie"](./media/machine-learning-data-science-create-features/feature-Hashing1.png)

In de volgende afbeelding ziet u hoe deze nieuw uiterlijk functie zoals.

![Voorbeeld van de 'Functie Hashing'](./media/machine-learning-data-science-create-features/feature-Hashing2.png)


## <a name="conclusion"></a>Conclusie

Ontworpen en geselecteerde functies verhogen de efficiëntie van het trainingsproces waarbij wordt geprobeerd de belangrijkste informatie in de gegevens uitpakken. Ze verbeteren ook de kracht van deze modellen voor het classificeren van de ingevoerde gegevens nauwkeurig te voorspellen resultaten van rente meer krachtig. Functie engineering en selectie kunnen ook combineren om het leerproces meer rekenkundig tractable. Dit gebeurt door verbetering en vervolgens het aantal functies die nodig zijn voor het kalibreren of een model van de trein te verminderen. Wiskundig gezien zijn de onderdelen geselecteerd om te leiden van het model een minimaal aantal onafhankelijke variabelen die de patronen in de gegevens wordt uitgelegd en vervolgens resultaten is te voorspellen.

Houd er rekening mee dat het is niet altijd noodzakelijk voor het uitvoeren van functies engineering of functie selecteren. Of het nodig is of niet hangt af van de gegevens die we hebben of verzamelen, de algoritme die wij verzamelen en het doel van het experiment.
 

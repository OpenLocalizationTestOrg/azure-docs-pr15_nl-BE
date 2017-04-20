<properties
    pageTitle="Engineering en selectie in Azure Machine Learning functie | Microsoft Azure"
    description="De toepassing van de functieselectie en de functie engineering uitgelegd en vindt u voorbeelden van hun rol in het proces van verbetering van de gegevens van machine learning."
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
    ms.author="zhangya;bradsev" />


# <a name="feature-engineering-and-selection-in-azure-machine-learning"></a>Functie engineering en selectie in Azure Machine Learning

Dit onderwerp wordt uitgelegd dat de toepassing van de functie engineering en functies selecteren in het proces van verbetering van de gegevens van machine learning. Dit illustreert deze processen waarbij met behulp van voorbeelden die Azure Machine Learning Studio.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

De trainingsgegevens in de machine learning worden vaak verbeterd de selectie of de winning van functies uit de onbewerkte gegevens verzameld. Een voorbeeld van een functie-engineering is toegepast in het kader van leren voor het classificeren van de afbeeldingen van handgeschreven tekens is een bits dichtheid kaart gemaakt op basis van het gegevenstype raw bit distributie. Dit overzicht kunt de randen van de tekens efficiënter dan de ruwe verdeling te vinden.

Ontworpen en geselecteerde functies verhogen de efficiëntie van het proces van opleiding, waarin wordt getracht de belangrijkste informatie in de gegevens uitpakken. Ze verbeteren ook de kracht van deze modellen voor het classificeren van de ingevoerde gegevens nauwkeurig te voorspellen resultaten van rente meer krachtig. Functie engineering en selectie kunnen ook combineren om het leerproces meer rekenkundig tractable. Dit gebeurt door verbetering en vervolgens het aantal functies die nodig zijn voor het kalibreren of een model van de trein te verminderen. Wiskundig gezien zijn de onderdelen geselecteerd om te leiden van het model een minimaal aantal onafhankelijke variabelen die de patronen in de gegevens wordt uitgelegd en vervolgens resultaten is te voorspellen.

Engineering en selectie van functies is een onderdeel van een groter proces, meestal uit vier stappen bestaat:

* Het verzamelen van gegevens
* Gegevensverbetering
* Model bouw
* Achteraf worden verwerkt

Engineering en selectie vormen de stap gegevens verbetering van machine learning. Drie aspecten van dit proces worden voor onze doeleinden onderscheiden:

* **Voorbehandeling van gegevens**: dit proces probeert om ervoor te zorgen dat de verzamelde gegevens helder en consistent is. Het bevat integreren van meerdere gegevenssets, ontbrekende gegevens verwerken, inconsistente gegevens verwerken en converteren van gegevenstypen.
* **Functie engineering**: dit proces wordt geprobeerd extra relevante functies maken van de bestaande raw functies in de gegevens en voorspellend vermogen op het algoritme leren te vergroten.
* **Functies selecteren**: dit proces wordt geselecteerd voor de belangrijkste deel van de oorspronkelijke gegevensfuncties verminderen de dimensionaliteit van het probleem van de opleiding.

In dit onderwerp worden alleen de technische functie en de functie selectie aspecten van de uitbreiding van de gegevens. Voor meer informatie over de data pre-processing stap, Zie [vooraf verwerken van gegevens in Azure Machine Learning Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).


## <a name="creating-features-from-your-data--feature-engineering"></a>Functies maken van uw gegevens--functie engineering

De gegevens van de opleiding bestaat uit een matrix bestaat uit voorbeelden (records of waarnemingen die zijn opgeslagen in rijen) een aantal voorzieningen (variabelen of velden die zijn opgeslagen in de kolommen zijn). De functies die zijn opgegeven in de proefopzet worden verwacht om aan te geven van de patronen in de gegevens. Hoewel veel van de velden van onbewerkte gegevens kunnen rechtstreeks worden opgenomen in de geselecteerde functie gebruikt voor het trainen van een model, moeten extra engineering functies vaak worden gemaakt op basis van de functies in de onbewerkte gegevens voor het genereren van een uitgebreide training gegevensset.

Wat voor soort functies moet worden gemaakt ter verbetering van de gegevensverzameling bij de opleiding van een model? Engineering functies die de opleiding verbeteren bevatten informatie waarmee de patronen in de gegevens beter te onderscheiden. U verwacht dat de nieuwe functies om aanvullende gegevens die niet duidelijk is vastgelegd of gemakkelijk zichtbaar in de oorspronkelijke of de bestaande functie ingesteld, maar dit proces is iets van een illustratie. Gezonde en productieve beslissingen vereist vaak bepaalde expertise in verschillende domeinen.

Wanneer u start met Azure Machine Learning, het gemakkelijkst om concrete invulling te geven dit proces te vatten met behulp van voorbeelden in de Studio van Machine Learning. Twee voorbeelden worden hier weergegeven:

* Een voorbeeld van de regressie ([voorspelling van het nummer van de huur fiets](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4)) in een gecontroleerde experiment waarbij bekend is dat de streefwaarden
* Een voorbeeld van de tekst mijnbouw classificatie met [Hash-functie][feature-hashing]

### <a name="example-1-adding-temporal-features-for-a-regression-model"></a>Voorbeeld 1: Tijdelijke functies voor een regressiemodel toevoegen ###

Laat zien hoe engineering-functies voor een taak regressie, we gebruiken het experiment "Vraag het voorspellen van fietsen" in Azure Machine Learning Studio. Het doel van dit experiment is om te voorspellen van de vraag naar de fietsen, dat wil zeggen het aantal fiets huur in een specifieke maand, dag of uur. De gegevensset **fiets verhuur UCI gegevensset** wordt gebruikt als de onbewerkte gegevens zijn ingevoerd.

Deze gegevensset is gebaseerd op werkelijke gegevens van het bedrijf Bikeshare kapitaal die een netwerk van fiets verhuur in Washington D.C. in de Verenigde Staten onderhoudt. De gegevensset vertegenwoordigt het aantal fiets huur binnen een bepaald uur van een dag van 2011 naar 2012 en 17379 rijen en kolommen 17 bevat. Raw-functie bevat de weersomstandigheden (temperatuur, vochtigheid, windsnelheid) en het type van de dag (vakantie of weekdag). Het veld te voorspellen is **cnt**, een aantal dat de huur van de fiets binnen een specifiek uur vertegenwoordigt en die varieert van 1 tot 977.

Vier regressie-modellen zijn om effectieve functies in de trainingsgegevens samenstellen, ingebouwd via dezelfde algoritme, maar met vier verschillende training gegevens. De vier gegevenssets dezelfde ruwe invoergegevens vertegenwoordigen, maar met een toenemend aantal functies instellen. Deze functies zijn ingedeeld in vier categorieën:

1. A = weer + feestdagen + weekdag + functies voor de voorspelde dag weekend
2. B = het aantal fietsen dat in elk van de vorige 12 uur zijn gehuurd
3. C = het aantal fietsen dat in elk van de afgelopen 12 dagen op hetzelfde uur zijn gehuurd
4. D = het aantal fietsen dat in elk van de voorbije 12 weken op hetzelfde uur en dezelfde dag zijn gehuurd

Naast de functie set A, al in de oorspronkelijke raw-gegevens bestaat, worden de drie sets van functies via de functie voor engineering proces gemaakt. Functie set B opnamen de recente vraag naar de fietsen. Functie set C opnamen de vraag naar fietsen op een bepaald uur. D opnamen vraag naar fietsen functie instellen op een bepaald uur en een bepaalde dag van de week. Elk van de vier training gegevensverzamelingen functiesets bevat A, A + B, A + B + C en A + B + C + D, respectievelijk.

Deze vier training data sets worden in het experiment Azure Machine Learning via vier vertakkingen van de invoer voorverwerkte gegevensset gevormd. Met uitzondering van de meest linkse tak bevat elk van deze filialen van een [R-Script uitvoeren] [ execute-r-script] module waarin een reeks functies (functie stelt B, C en D) afgeleid is respectievelijk gebouwd en toegevoegd aan de geïmporteerde gegevensset. In de volgende afbeelding ziet u het R-script gebruikt voor het maken van de functieset B in de tweede tak van de linker.

![Een functie maken](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

De volgende tabel bevat een overzicht van de vergelijking van de prestatieresultaten van de vier modellen. De beste resultaten worden weergegeven door functies van A + B + C. Houd er rekening mee dat de fout snelheid afneemt wanneer extra functiesets worden opgenomen in de trainingsgegevens. Hiermee wordt onze veronderstelling dat de functiesets B en C aanvullende relevante informatie voor de taak regressie bieden gecontroleerd. Toevoegen van de D-functieset lijkt niet te bieden een extra verlaging van het tarief van de fout.

![Prestaties vergelijken](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a>Voorbeeld 2: Functies maken in tekst mijnbouw  

Functie engineering wordt veel toegepast in taken met betrekking tot de mijnbouw-tekst, zoals de indeling en het sentiment documentenanalyse. Bijvoorbeeld als u wilt dat documenten indelen in verschillende categorieën, is een typische veronderstelling dat de woorden of woordgroepen die zijn opgenomen in één documentcategorie minder zullen optreden in een andere documentcategorie. Met andere woorden, kan de frequentie van de verdeling van het woord of de woordgroep verschillende documentcategorieën kenmerkt. In tekst-mining toepassingen is de functie engineering proces nodig voor het maken van de functies met betrekking tot woord of woordgroep frequenties omdat afzonderlijke stukjes tekst inhoud wordt gewoonlijk als de ingevoerde gegevens gebruikt.

Voor deze taak, wordt een *hash-functie* genoemd techniek toegepast om efficiënt tekstfuncties willekeurige omzetten in indexen. In plaats van elk onderdeel van de tekst (woorden of zinnen) koppelen aan een bepaalde index, deze methode werkt door het toepassen van een hash-functie op de functies en door de hash-waarden als indices rechtstreeks.

In Azure Machine Learning, er is een [Hash-functie] [ feature-hashing] module die wordt gemaakt van deze functies met het woord of de woordgroep. In de volgende afbeelding ziet u een voorbeeld van het gebruik van deze module. De ingevoerde gegevens bevat twee kolommen: het boek rating variërend van 1 op 5 en de werkelijke inhoud te bekijken. Het doel van deze [Hash-functie] [ feature-hashing] module is het ophalen van de nieuwe functies die de frequentie van het exemplaar van de overeenkomende woorden of zinnen binnen de bepaalde Boekbespreking weergeven. Deze module gebruikt, moet u de volgende stappen uit:

1. Selecteer de kolom met de ingevoerde tekst (in dit voorbeeld**Kol2** ).
2. *Hashing bitsize* ingesteld op 8, dat wil zeggen 2 ^ 8 = 256 functies worden gemaakt. Het woord of de woordgroep in de tekst wordt vervolgens opgedeeld naar 256 indices. De parameter *bitsize Hashing* bereik van 1 tot en met 31. Als de parameter is ingesteld op een groter aantal, worden de woorden of woordgroepen minder waarschijnlijk opdelen in dezelfde index.
3. De parameter *N g* ingesteld op 2. Dit wordt de frequentie van voorkomen van unigrams (een functie voor elk één woord) en bigrams (een functie voor elk paar aangrenzende woorden) opgehaald uit de ingevoerde tekst. De parameter *N g* varieert van 0 tot 10, geeft het maximum aantal opeenvolgende woorden moeten worden opgenomen in een functie.  

![Hash-functie-module](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

De volgende afbeelding ziet hoe deze nieuwe functies eruit.

![Voorbeeld van de hash-functie](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## <a name="filtering-features-from-your-data--feature-selection"></a>Filterfuncties van uw--functies selecteren  ##

*Selectie functie* is een proces dat veel wordt toegepast op de bouw van gegevenssets training voor voorspellende modelleertaken als taken classificatie of regressie. Het doel is om een subset van de functies van de oorspronkelijke gegevensset die de afmetingen vermindert met behulp van een minimale set van functies voor de maximale hoeveelheid variantie in de gegevens te selecteren. Dit deel van de functies bevat alleen functies moeten worden opgenomen om het model van de trein. Functieselectie heeft twee hoofddoelen:

* Functieselectie verhoogt de nauwkeurigheid van de classificatie vaak doordat niet relevant, redundante of sterke correlatie bestaat tussen functies.
* Functieselectie vermindert het aantal functies, waardoor het proces model training efficiënter. Dit is vooral belangrijk voor cursisten die echter duur zijn om te trainen zoals support vector machines.

Hoewel de Functieselectie tracht te verminderen van het aantal functies in de gegevensset die wordt gebruikt om het model van de trein, wordt niet meestal aangeduid met de term *vermindering van dimensionaliteit.* Functie selectiemethoden Pak een subset van de oorspronkelijke functies van de gegevens niet wijzigen.  Dimensionaliteit reductie methoden gebruiken engineering functies kunnen transformeren de oorspronkelijke functies en aldus aan te passen. Voorbeelden van dimensionaliteit reductie methoden zijn analyse van de belangrijkste component, canonieke correlatieanalyse en ontleding van de enkelvoudige waarde.

Een breed worden toegepast categorie van selectiemethoden functie in een gecontroleerde context is op basis van een filter functies selecteren. Deze methoden gelden door de correlatie tussen elke functie en het doelkenmerk te evalueren, een statistische eenheid een score om aan te wijzen elke functie. De voorzieningen zijn vervolgens gerangschikt op de score, kunt u de drempel voor het bewaren of verwijderen van een bepaalde functie ingesteld. Voorbeelden van de statistische metingen die worden gebruikt in deze methoden zijn Pearson correlatie, wederzijdse informatie en de test van de chi-kwadraatverdeling.

Azure Machine Learning Studio biedt modules voor functies selecteren. In de volgende afbeelding ziet deze modules bevatten [functies selecteren op basis van een Filter] [ filter-based-feature-selection] en [Fisher lineaire Discriminant analyse][fisher-linear-discriminant-analysis].

![Voorbeeld van de functie selectie](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)


Gebruik bijvoorbeeld de [Functieselectie op basis van een Filter] [ filter-based-feature-selection] -module met het tekstvoorbeeld mijnbouw eerder beschreven. Stel dat u maken van een regressiemodel wilt nadat een set van 256 functies is gemaakt door de [Hash-functie] [ feature-hashing] module, en dat de variabele antwoord **Kol1** en vertegenwoordigt een boek bekijken rating variërend van 1 tot 5. **Functie methode scoren** op **Pearson correlatie**, **doelkolom** **Kol1**en het **nummer van de gewenste functies** op **50**ingesteld. De module [functies selecteren op basis van een Filter] [ filter-based-feature-selection] wordt een gegevensverzameling met 50 onderdelen van het doelkenmerk **Kol1**. De volgende afbeelding ziet u de stroom van dit experiment en de invoerparameters.

![Voorbeeld van de functie selectie](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

De volgende afbeelding ziet u de resulterende gegevenssets. Elke functie is op basis van de Pearson-correlatie tussen zichzelf en het doelkenmerk **Kol1**gescoord. De functies met de hoogste scores worden gehouden.

![Op basis van een filter functie selectie gegevenssets](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

De volgende afbeelding ziet u de bijbehorende scores van de geselecteerde onderdelen.

![Scores van de geselecteerde functie](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Door deze [functies selecteren op basis van een Filter] toe te passen[ filter-based-feature-selection] module, 50 van 256 functies zijn geselecteerd, omdat ze de meeste functies gecorreleerd aan de variabele **Kol1** op basis van de score methode **Pearson correlatie**doel hebben.

## <a name="conclusion"></a>Conclusie
Zijn twee stappen worden uitgevoerd ter voorbereiding van de trainingsgegevens bij het bouwen van een model van de machine learning functie engineering en functies selecteren. Normaal gesproken eerst functie-engineering wordt toegepast voor het genereren van extra functies en vervolgens de functie selectie stap wordt uitgevoerd om de functies niet relevant, redundante of sterk gecorreleerde elimineren.

Het is niet altijd noodzakelijk voor het uitvoeren van functies engineering of functie selecteren. Of het nodig is, is afhankelijk van de gegevens die u hebt of verzamelen, het algoritme dat u kiest en het doel van het experiment.


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/

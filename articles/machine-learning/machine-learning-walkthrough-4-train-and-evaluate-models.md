<properties
    pageTitle="Stap 4: Trainen en evalueren van de analytische voorspellende modellen | Microsoft Azure"
    description="Stap 4 van het ontwikkelen van een oplossing voor anticiperende scenario: trein, score en meerdere modellen in Azure Machine Learning Studio te evalueren."
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
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a>Scenario-stap 4: Trainen en evalueren van de analytische voorspellende modellen

Dit onderwerp bevat de vierde stap van de procedure, het [ontwikkelen van een voorspellende analytics oplossing in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Een Machine Learning werkruimte maken](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Bestaande gegevens uploaden](machine-learning-walkthrough-2-upload-data.md)
3.  [Maak een nieuw experiment](machine-learning-walkthrough-3-create-new-experiment.md)
4.  **Trainen en het evalueren van de modellen**
5.  [De web-service implementeren](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Toegang tot de webservice.](machine-learning-walkthrough-6-access-web-service.md)

----------

Een van de voordelen van het gebruik van Azure Machine Learning Studio voor het maken van machine learning modellen is de mogelijkheid om meer dan één type model dat tegelijk in een experiment te vergelijken van de resultaten. Dit soort experimenten helpt u bij het vinden van de beste oplossing voor uw probleem.

In het experiment dat we in dit scenario ontwikkelt, we twee soorten modellen maken en vergelijk de resultaten score om te bepalen welk algoritme die we in onze laatste experiment wilt gebruiken.  

Er zijn verschillende modellen die kunnen we kiezen uit. Vouw het knooppunt **Machine Learning** in het palet van de module en vouw vervolgens **Initialiseren Model** en de knooppunten onder het overzicht van de beschikbare modellen. Voor de toepassing van dit experiment gaat we twee klasse versterkt beslissingsstructuren modules en ondersteuning Vector Machine (SVM) selecteren.    

> [AZURE.TIP] Zie voor hulp bij het bepalen van welk algoritme Machine Learning best past bij het specifieke probleem dat u probeert op te lossen, [het kiezen van algoritmen voor Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).

##<a name="train-the-models"></a>De modellen van de trein
Ten eerste stellen we de boomstructuur gestimuleerd besluit:  

1.  De [Beslissingsstructuur van twee klasse versterkt] vinden[ two-class-boosted-decision-tree] module in de module palet en sleep dit naar het canvas.
2.  Zoeken naar het [Model van de trein] [ train-model] module, sleept u het naar het canvas en sluit vervolgens de output van de module gestimuleerd besluit boom aan de linkerkant invoerpoort ("ongetrainde model") van het [Model van de trein] [ train-model] module.
    
    De [Beslissingsstructuur van twee klasse versterkt] [ two-class-boosted-decision-tree] module initialiseert de algemene model, en het [Model van de trein] [ train-model] trainingsgegevens worden gebruikt om het model van de trein. 
     
3.  Verbinding maken met de linker uitvoer ('resultaat Dataset") van het linker [R Script uitvoeren] [ execute-r-script] module rechts input poort ("Dataset") van het [Model trein] [ train-model] module.

    > [AZURE.TIP] We hebben twee van de ingangen en één van de resultaten van het [R-Script uitvoeren] niet nodig[ execute-r-script] -module voor dit experiment, zodat we ze niet-vastgemaakte laat. 

4.  Selecteer het [Model van de trein] [ train-model] module. In het deelvenster **Eigenschappen** , klikt u op de **kolomkiezer starten**, **Alle typen** in de vervolgkeuzelijst selecteert u onder **Beschikbare kolommen** en "Kredietrisico" in het tekstveld invoert. Selecteer **Alle typen** in de vervolgkeuzelijst onder **Kolommen geselecteerd**. Selecteer 'Kredietrisico' en klik op de pijlknop-gemarkeerde naar **Geselecteerde kolommen**te verplaatsen. 
5.  Klik op **Opslaan**.


Dit gedeelte van het experiment nu uitziet:  

![Training van een model][1]

Vervolgens stelt het SVM-model.  

Eerste, uitleg over SVM. Beslissingsstructuren gestimuleerd goed werken met functies van elk type. Aangezien de SVM module een lineaire classificatie genereert, heeft het model dat wordt gegenereerd echter de beste testfout wanneer alle numerieke functies dezelfde schaal hebben. Als u wilt converteren van alle numerieke functies op dezelfde schaal, gebruiken we een transformatie "Tanh" (met de [Gegevens normaliseren] [ normalize-data] module.) Dit transformeert ons getallen in het bereik [0,1]. Tekenreeks-functies worden omgezet door de module SVM functies bestaan en vervolgens naar binaire 0/1-functies, zodat we niet hoeven te transformeren handmatig string functies. Ook wij niet wilt transformeren, het kredietrisico kolom (kolom 21) - numeriek is, maar is de waarde die we bent het model om te voorspellen, training, dus moeten we het laten.  

Als u het model SVM instellen, het volgende doen:

1.  De [Twee klasse Support Vector Machine] vinden[ two-class-support-vector-machine] module in de module palet en sleep dit naar het canvas.
2.  Klik met de rechtermuisknop op het [Model van de trein] [ train-model] module, selecteert u **kopiëren**, klik met de rechtermuisknop op het canvas en selecteer **Plakken**. De kopie van het [Model trein] [ train-model] module heeft dezelfde kolomselectie als het origineel.
3.  Verbinding maken met de uitvoer van de module SVM de linker invoerpoort ("ongetrainde model") van het tweede [Model trein] [ train-model] module.
4.  Zoek de [Gegevens normaliseren] [ normalize-data] module en sleep dit naar het canvas.
5.  Verbinding maken met de invoer van deze module de uitvoer links van het linker [R Script uitvoeren] [ execute-r-script] module (Let erop dat de uitvoerpoort van een module kan worden verbonden met meer dan een andere module).
6.  Verbinding maken met de linker uitvoerpoort ('getransformeerd Dataset") van de [Gegevens normaliseren] [ normalize-data] module rechts input poort ("Dataset") van het tweede [Model trein] [ train-model] module.
7.  In het deelvenster **Eigenschappen** voor de [Gegevens normaliseren] [ normalize-data] module, selecteer **Tanh** voor de parameter van de **methode van de transformatie** .
8.  Klikt u op de **kolomkiezer te starten**, selecteert u 'Geen kolommen' voor het **Beginnen met** **opnemen** in de eerste vervolgkeuzelijst selecteert, **kolomtype** selecteert in de tweede vervolgkeuzelijst en **numerieke** selecteren in de derde vervolgkeuzelijst. Hiermee geeft u alle numerieke kolommen (en alleen numerieke) worden getransformeerd.
9.  Klik op het plusteken (+) aan de rechterkant van deze rij - Hiermee maakt u een rij met vervolgkeuzelijsten. Selecteer **uitsluiten** in de eerste vervolgkeuzelijst **kolomnamen** in de tweede vervolgkeuzelijst en klikt u op het veld en selecteer "kredietrisico' uit de lijst met kolommen selecteren. Hiermee geeft u op dat de kolom kredietrisico moet worden genegeerd (moeten we dit doen, omdat deze kolom numerieke en dus anders zou worden omgezet).
10. Klik op **OK**.  


De [Gegevens normaliseren] [ normalize-data] module is nu ingesteld voor het uitvoeren van een transformatie Tanh op alle numerieke kolommen, met uitzondering van de kolom van het kredietrisico.  

Dit gedeelte van ons experiment ziet er nu ongeveer zo uit:  

![Het tweede model training][2]  

##<a name="score-and-evaluate-the-models"></a>Score en evalueren van de modellen

We gebruiken de gegevens testen is gescheiden door de [Gesplitste gegevens] [ split] module voor het verkrijgen van onze ervaren modellen. Vervolgens kunnen we vergelijken de resultaten van de twee modellen te zien die betere resultaten gegenereerd.  

1.  De [Score-Model] vinden[ score-model] module en sleep dit naar het canvas.
2.  Verbinding maken met het [Model van de trein] [ train-model] module die is verbonden aan de [Beslissingsstructuur van twee klasse versterkt] [ two-class-boosted-decision-tree] module links ingang poort van de [Score-Model] [ score-model] module.
3.  Verbinding maken met de juiste invoerpoort van de [Score-Model] [ score-model] module aan de linker uitvoer van het juiste [R Script uitvoeren] [ execute-r-script] module.

    De [Score-Model] [ score-model] module kan nu de creditcard gegevens uit de gegevens testen uitvoeren via het objectmodel en vergelijken de voorspellingen het model met de werkelijke krediet risico kolom de gegevens testen genereert.

4.  Kopieer en plak het [Score-Model] [ score-model] module voor het maken van een tweede exemplaar of een nieuwe module naar het canvas te slepen.
5.  Verbinding maken met de linker invoerpoort van deze module de SVM-model (dat wil zeggen, verbinding maken met de uitvoerpoort van het [Model van de trein] [ train-model] module die aangesloten op de [Machine van twee klasse Support Vector] [ two-class-support-vector-machine] module).
6.  We hebben de transformatie dezelfde gegevens test doen als we naar de trainingsgegevens was voor het model SVM. Dus kopiëren en plakken de [Gegevens normaliseren] [ normalize-data] module, een tweede kopie maken en verbinding maken met de uitvoer links van het juiste [R Script uitvoeren] [ execute-r-script] module.
7.  Verbinding maken met de juiste invoerpoort van de [Score-Model] [ score-model] module aan de linker uitvoer van de [Gegevens normaliseren] [ normalize-data] module.  

Evaluatie van de resultaten van twee score, gebruiken we een [Evaluatie van Model] [ evaluate-model] module.  

1.  Zoek het [Evalueren van Model] [ evaluate-model] module en sleep dit naar het canvas.
2.  Verbinding maken met de invoerpoort links naar de uitvoerpoort van de [Score-Model] [ score-model] module die is gekoppeld aan de boomstructuur besluit gestimuleerd.
3.  Verbinding maken met de juiste poort voor invoer naar het andere [Model Score] [ score-model] module.  

Het experiment wordt uitgevoerd, klikt u op de knop **uitvoeren** onder het canvas. Het kan enkele minuten duren. Een indicator draaiende op elke module bevat die wordt uitgevoerd en een groen vinkje als de module is voltooid. Als alle modules ingeschakeld hebt, is het experiment uitgevoerd.

Het experiment moet nu als volgt uitzien:  

![Evaluatie van beide modellen][3]

U kunt de resultaten controleren, klikt u op de uitvoerpoort van het [Evalueren van Model] [ evaluate-model] module en selecteer **visualiseren**.  

Het [Evalueren van Model] [ evaluate-model] module produceert een paar curven en parameters waarmee u de resultaten van de twee scored modellen vergelijken. U kunt de resultaten als ontvanger exploitant kenmerk (ROC) curven, Precision/intrekken curven of Lift curven weergeven. Aanvullende gegevens die worden weergegeven bevat een matrix van verwarring, cumulatieve waarden van het gebied onder de curve (AUC) en andere statistieken. U kunt de drempelwaarde door de schuifregelaar naar links of rechts te wijzigen en zien hoe dit van invloed op de set parameters.  

Rechts van de grafiek, klikt u op **Scored dataset** of **dataset Scored vergelijken** de curve met bijbehorende markeren en de bijbehorende parameters hieronder weergeven. In de legenda voor de curves "Dataset gescoord" komt overeen met de linker invoerpoort van het [Evalueren van Model] [ evaluate-model] module - in ons geval, dit is de gestimuleerd besluit boom. 'Gescoord dataset vergelijken' komt overeen met de juiste invoerpoort - het model SVM in ons geval. Wanneer u op een van deze labels, wordt de curve voor dit model is gemarkeerd en de bijbehorende parameters weergegeven zoals in de volgende afbeelding.  

![ROC curven voor modellen][4]

Door het onderzoeken van deze waarden, kunt u beslissen welk model zich het dichtst bij zodat u de resultaten die u zoekt. U kunt teruggaan en op uw experiment herhaald door de waarden in de verschillende modellen te wijzigen. 

> [AZURE.TIP] Telkens wanneer u het experiment een record van deze iteratie uitvoert wordt bewaard in de geschiedenis uitvoeren. U kunt deze herhalingen weergeven en terugkeren naar elk van hen, door te klikken op **Uitvoeren geschiedenis weergeven** onder het canvas. U kunt ook klikken op **Voorafgaande uitvoeren** in het deelvenster **Eigenschappen** terug te keren naar de voorgaande iteratie de website die u hebt geopend.
> 
U kunt een kopie maken van een herhaling van het experiment door te klikken op **Opslaan als** onder het canvas. Met de eigenschappen **Overzicht** en **Beschrijving** van het experiment kunt bijhouden wat u hebt geprobeerd in de experiment iteraties.

>  Voor meer informatie, Zie [beheren iteraties in Azure Machine Learning Studio experimenteren](machine-learning-manage-experiment-iterations.md).  


----------

**Volgende: [de web-service implementeren](machine-learning-walkthrough-5-publish-web-service.md)**

[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train1.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train2.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train3.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train4.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

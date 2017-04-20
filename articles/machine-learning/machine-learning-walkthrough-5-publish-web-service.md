<properties
    pageTitle="Stap 5: Implementeer de Machine Learning Web service | Microsoft Azure"
    description="Stap 5 van het ontwikkelen van een oplossing voor anticiperende scenario: een voorspellende experiment in Machine Learning Studio als een webservice implementeren."
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
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-web-service"></a>Procedure stap 5: Implementeer de Azure Machine Learning Web service

Dit is de vijfde stap van de procedure, het [ontwikkelen van een voorspellende analytics oplossing in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Een Machine Learning werkruimte maken](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Bestaande gegevens uploaden](machine-learning-walkthrough-2-upload-data.md)
3.  [Maak een nieuw experiment](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Trainen en het evalueren van de modellen](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  **De Web-service implementeren**
6.  [Toegang tot de webservice.](machine-learning-walkthrough-6-access-web-service.md)

----------

Om een kans om de voorspellende model die we hebben ontwikkeld in dit scenario gebruiken anderen, implementeren wij als een webservice op Azure.

Tot dusver Experimenteer we met ons model voor opleiding. Maar de geïmplementeerde service niet meer gaat doen training - voorspellingen wordt gegenereerd door de invoer van de gebruiker op basis van ons model scoren. Dus gaan we dit experiment converteren van een experiment ***training*** voorbereiding een ***voorspellende*** experimenteren. 

Dit is een uit twee stappen:  

1. De *training experimenteren* die we hebt gemaakt converteren naar een *voorspellende experiment*
2. Het experiment voorspellende implementeren als een webservice

Maar eerst moeten we dit experiment een beetje bijsnijden naar beneden. We hebben momenteel twee verschillende modellen in het experiment, maar we willen slechts één model wanneer we dit als een webservice implementeren.  

Stel dat we hebben besloten dat het model gestimuleerd structuur het model beter was te gebruiken. Dus het eerste wat u moet doen is verwijderen van de [Twee klasse Support Vector Machine] [ two-class-support-vector-machine] -module en de modules die zijn gebruikt voor deze training. U kunt een kopie maken van het experiment eerst door te klikken op **OpslaanAls** op de onderkant van het papier van het experiment.

We moeten verwijderen van de volgende modules:  

- [Ondersteuning van twee klasse Vector Machine][two-class-support-vector-machine]
- [Model van de trein] [ train-model] en [Score-Model] [ score-model] modules die zijn verbonden met het
- [Gegevens normaliseren] [ normalize-data] (beide)
- [Model evalueren][evaluate-model]

De module te selecteren en druk op de toets Delete of klik met de rechtermuisknop op de module en selecteer **verwijderen**.

Nu dit model met de [Twee klasse versterkt beslissingsstructuur]gaan we[two-class-boosted-decision-tree].

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Het experiment training converteren naar een voorspellende experiment

Converteren naar een voorspellende experiment bestaat uit drie stappen:

1. Het model dat we hebben opgeleid en vervang onze opleidings-modules opslaan
2. Snijd het experiment om te verwijderen van modules die zijn alleen nodig voor de opleiding
3. Wanneer de webservice invoer en waar de uitvoer genereert definiëren

Gelukkig kunnen alle drie stappen worden uitgevoerd door te klikken op **service Web instellen** onder aan het canvas experiment (Selecteer de optie **voorspellende Web service** ).

Wanneer u op een **webpagina instellen service**, gebeuren er verschillende dingen:

- De opgeleide model wordt opgeslagen als een enkel **Model opgeleide** module in het palet van de module aan de linkerkant van het experiment canvas (u vindt deze onder **Opgeleide modellen**).
- Modules die zijn gebruikt voor de opleiding worden verwijderd. Met name:
  - [Twee klasse gestimuleerd beslissingsstructuur][two-class-boosted-decision-tree]
  - [Model trein][train-model]
  - [Gesplitste gegevens][split]
  - de tweede [R Script uitvoeren] [ execute-r-script] module die is gebruikt voor het testen
- De opgeslagen opgeleide model weer toegevoegd aan het experiment.
- **Web service** en **Web service uitvoer** modules worden toegevoegd.

> [AZURE.NOTE] Het experiment is opgeslagen in twee delen onder tabbladen die zijn toegevoegd aan de bovenkant van het canvas experiment: het oorspronkelijke opleiding experiment is onder het tabblad **opleiding experimenteren**en het zojuist gemaakte voorspellende experiment is onder **Predictive experimenteren**.

Moeten we een extra stap met dit bijzonder experiment uitvoeren.
We twee [R Script uitvoeren] toegevoegd[ execute-r-script] modules aan een weging functie om de gegevens te verstrekken voor training en testen. We hoeven niet te doen in het uiteindelijke model.

Machine Learning Studio verwijderd één [R Script uitvoeren] [ execute-r-script] module wanneer deze de [splitsing] verwijderen[ split] module. Nu we kunnen nu de andere verwijderen en [Metagegevens Editor] verbinding[ metadata-editor] rechtstreeks naar de [Score-Model][score-model].    

Ons experiment moet er nu als volgt uitzien:  

![Het opgeleide evaluatiemodel][4]  

> [AZURE.NOTE] U vraagt zich misschien af waarom we de dataset UCI Duits creditcardgegevens links in de voorspellende experiment. De service gaat worden de gegevens van de gebruiker, niet de oorspronkelijke gegevensset gebruiken, dus waarom laat de oorspronkelijke gegevensset in het model?
>
>Het klopt dat de service de oorspronkelijke creditcardgegevens niet hoeft. Maar u hoeft het schema voor die gegevens, waarop informatie zoals hoeveel kolommen er zijn en welke kolommen zijn numeriek. Dit schema-informatie is nodig om het interpreteren van de gegevens van de gebruiker. We laten deze onderdelen gekoppeld, zodat de module score het schema van de dataset is wanneer de service wordt uitgevoerd. De gegevens wordt niet gebruikt, alleen het schema.  

Het experiment één keer uitgevoerd (Klik op **uitvoeren**.) Als u controleren of het model nog steeds werkt wilt, klikt u op de uitvoer van de [Score-Model] [ score-model] module en selecteer **Resultaten weergeven**. Ziet u de oorspronkelijke gegevens wordt weergegeven, samen met de krediet-risico waarde ('gescoord Labels') en de score waarde voor de kans ("gescoord kansen'.) 

## <a name="deploy-the-web-service"></a>De Web-service implementeren

U kunt het experiment als een klassieke webservice of een nieuwe webservice op basis van Azure Resource Manager implementeren.

### <a name="deploy-as-a-classic-web-service"></a>Als een klassieke Internet implementeren ###

Voor de implementatie van een klassieke webservice die is afgeleid van onze experiment op **Webservice implementeren** onder het canvas en selecteer **Webservice implementeren [klassiek]**. Machine Learning Studio implementeert het experiment als webservice en keert u terug naar het dashboard voor die service. U kunt hier terug te keren naar het experiment (**momentopname bekijken** of **laatste weergeven**) en voer een eenvoudige test van de webservice (Zie **de webservice Test** hieronder). Er is ook hier informatie over het maken van toepassingen die toegang hebben tot de webservice (meer hierover in de volgende stap van deze procedure).

![Web servicedashboard][6]

U kunt de service configureren door te klikken op het tabblad **configuratie** . Hier kunt u de naam van de service (dit is de naam van het experiment krijgen standaard) wijzigen en een omschrijving geven. U kunt ook meer beschrijvende labels voor de kolommen invoer en uitvoer geven.  

![De webservice configureren][5]  

### <a name="deploy-as-a-new-web-service"></a>Als een nieuw Web service implementeren

Klik op **Web-Service implementeert** onder het canvas en **Implementeren van Web Service [Nieuw]**voor de implementatie van een nieuwe webservice afgeleid van ons experiment. Machine Learning Studio brengt u naar de pagina Azure Machine Learning Web-services implementeren Experiment.

Voer een naam voor de webservice en selecteer een plan prijsstelling. Als u een bestaande prijsstelling plan dat kunt u dit, moet anders u een nieuw plan van de prijs voor de service. 

1.  Selecteer een bestaand schema of selecteert u de optie **nieuwe plan selecteren** in de vervolgkeuzelijst **Prijs Plan** .
2.  Typ in het vak **Naam**een naam waarmee het plan op uw rekening.
3.  Selecteer een van de **Maandelijkse plannen lagen**. De standaard plan lagen aan de plannen voor uw regio standaard en de webservice wordt geïmplementeerd in dat gebied.

Klik op **Deploy** en de **Quickstart** -pagina voor uw Web service wordt geopend.

U kunt de service configureren door te klikken op de menu-optie **configureren** . Hier kunt u de naam van de service (dit is de naam van het experiment krijgen standaard) wijzigen en een omschrijving geven. 

Als u wilt testen de Web service selecteren, klikt u op de optie **testen** (Zie **de webservice testen** hieronder). Klik op de menuoptie **verbruiken** (meer hierover in de volgende stap van deze procedure) voor meer informatie over het maken van toepassingen die toegang hebben tot de webservice.

> [AZURE.TIP] Nadat u deze hebt geïmplementeerd, kunt u de webservice bijwerken. Bijvoorbeeld, als u wijzigen van het model wilt, het experiment training bewerken, aanpassen van de Modelparameters en op **Web Service implementeren**. Selecteer **Webservice implementeren [Classic]** of **Implementeren van Web Service [Nieuw]**. Wanneer u opnieuw het experiment implementeert, vervangt deze de webservice, nu met uw bijgewerkte model.  

## <a name="test-the-web-service"></a>De Web-service testen

### <a name="test-a-classic-web-service"></a>Een klassieke webservice testen

U kunt de service testen in Machine Learning Studio of in de portal Azure Machine Learning Web-Services. Testen in de portal Azure Machine Learning Web Services heeft het voordeel dat u wilt inschakelen 

**Testen in de Studio van Machine Learning**

Op de **DASHBOARD** -pagina, klikt u op de knop **testen** onder **Standaard eindpunt**. Een dialoogvenster wordt weergegeven en vraagt u om de gegevens die zijn ingevoerd voor de service. Dit zijn dezelfde kolommen die zijn weergegeven in de oorspronkelijke Duitse krediet risico dataset.  

Voer een set gegevens en klik vervolgens op **OK**. 

**Testen in de webservices van Azure Machine Learning portal**

Op de pagina **DASHBOARD** Klik **Test** voorbeeld onder **Standaard eindpunt**. De testpagina in Azure Machine Learning Web Services portal voor het Web-service-eindpunt wordt geopend en u wordt gevraagd de gegevens die zijn ingevoerd voor de service. Dit zijn dezelfde kolommen die zijn weergegeven in de oorspronkelijke Duitse krediet risico dataset.

Klik op **Test verzoek-antwoord**. 

In de webservice gegevens invoert via de module **Web service invoer** via de [Metagegevens Editor] [ metadata-editor] module, en het [Score-Model] [ score-model] module waarin deze wordt gescoord. De resultaten worden vervolgens uitgevoerd via de Web-service via de **Web-service-uitvoer**.

**Een nieuwe webservice testen**

Klik op **Test** aan de bovenkant van de pagina in de portal Azure Machine Learning Web-services. **De testpagina** wordt geopend en u kunt gegevens voor de service. De invoervelden weergegeven komen overeen met de kolommen die zijn weergegeven in de oorspronkelijke Duitse krediet risico dataset. 

Gegevens invoeren en klik vervolgens op **Verzoek-antwoord van de Test**.

De resultaten van de test wordt weergegeven aan de rechterkant van de pagina in de uitvoerkolom. 

Bij het testen in de portal Azure Machine Learning Web-Services, kunt u de voorbeeldgegevens die u gebruiken kunt voor het testen van de verzoek-antwoord-service inschakelen. Als u de webservice in Studio van Machine Learning gemaakt, is de voorbeeldgegevens genomen uit de gegevens die uw gebruikte trainen uw model.

> [AZURE.TIP] De manier waarop we de voorspellende experiment is geconfigureerd, hebben de volledige resultaten van de [Score-Model] [ score-model] module worden geretourneerd. Dit omvat alle ingevoerde gegevens plus de krediet-risico-waarde en de kans op score. Als u wilt een ander - retourneren bijvoorbeeld alleen het krediet risico-waarde - en vervolgens kunt u een [Project kolommen] invoegen[ project-columns] module tussen [Score-Model] [ score-model] en de **uitvoer van Web service** te verwijderen van kolommen die u niet wilt dat de webservice om terug te keren. 

## <a name="manage-the-web-service"></a>De Web-service beheren

**Een klassieke Internet beheren**

Zodra u uw klassieke webservice hebt geïmplementeerd, kunt u deze vanaf de [Azure klassieke portal](https://manage.windowsazure.com)beheren.

1. Log in om de [Azure klassieke portal](https://manage.windowsazure.com).
2. Klik in het deelvenster services Microsoft Azure **MACHINE LEARNING**.
3. Klik op uw werkruimte.
4. Klik op het tabblad **webservices** .
5. Klik op de Web-service die we hebben gemaakt.
6. Klik op het eindpunt van de 'standaard'.

Hier kunt u doen als controleren hoe de webservice doet en prestaties-trucs maken door het wijzigen van het aantal gelijktijdige roept de service kunnen verwerken.
U kunt zelfs uw webservice publiceren in de markt Azure.

Zie voor meer informatie:

- [Eindpunten maken](machine-learning-create-endpoint.md)
- [Webservice schalen](machine-learning-scaling-webservice.md)
- [Azure Machine Learning Web-service op de markt Azure publiceren](machine-learning-publish-web-service-to-azure-marketplace.md)

**Een webservice in de portal Azure Machine Learning Web-Services beheren**

Zodra u hebt geïmplementeerd uw webservice Classic of New, kunt u deze beheren vanuit de [portal voor Azure Machine Learning Web-services](https://servics.azureml.net).

Voor het controleren van de prestaties van uw Web-service:

1. Log in om de [Azure Machine Learning services webportaal](https://servics.azureml.net).
2. Klik op de **webservices**.
3. Klik op de webservice.
4. Klik op het **Dashboard**.

----------

**Volgende: [toegang tot de webservice](machine-learning-walkthrough-6-access-web-service.md)**

[1]: ./media/machine-learning-walkthrough-5-publish-web-service/publish1.png
[2]: ./media/machine-learning-walkthrough-5-publish-web-service/publish2.png
[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
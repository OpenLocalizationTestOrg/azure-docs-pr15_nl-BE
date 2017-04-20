<properties
    pageTitle="Een Machine Learning training experiment converteren naar een voorspellende experiment | Microsoft Azure"
    description="Het converteren van een Machine Learning training experiment, voor het trainen van uw model anticiperende analytische methoden om een voorspellende experiment die kan worden geïmplementeerd als een webservice gebruikt."
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
    ms.date="08/19/2016"
    ms.author="garye"/>

# <a name="convert-a-machine-learning-training-experiment-to-a-predictive-experiment"></a>Een Machine Learning training experiment converteren naar een voorspellende experiment

Azure Machine Learning kunt u bouwen, testen en implementeren van anticiperende analytische oplossingen.

Zodra u hebt gemaakt en herhaald op een *training experimenteren* trainen uw model anticiperende analytische en u klaar om te gebruiken bent voor het verkrijgen van nieuwe gegevens, moet u voor het voorbereiden en het stroomlijnen van uw experiment voor het scoren. Vervolgens kunt u dit *experiment voorspellende* implementeren als een webservice Azure zodat gebruikers kunnen uw model gegevens verzenden en ontvangen van voorspellingen van het model.

Door te converteren naar een voorspellende experiment, krijgt u uw opgeleide model klaar om te worden geïmplementeerd als een webservice. Gebruikers van de webservice stuurt invoer aan uw model en het model van uw stuurt weer de resultaten voorspelling. Dus als u naar een voorspellende experiment converteert zult u rekening moet houden hoe u verwacht dat het model moet worden gebruikt door anderen.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Het proces van het converteren van een experiment training naar een voorspellende experiment bestaat uit drie stappen:

1.  Sla de machine learning-model dat u hebt getraind en vervang de machine leren algoritme en [Trein Model] [ train-model] modules met uw opgeslagen opgeleide model.
2.  Snijd het experiment om alleen die modules die nodig zijn voor het scoren. Een experiment training bevat een aantal modules die nodig zijn voor de opleiding, maar zijn niet nodig als het model is opgeleid en klaar voor gebruik voor het scoren.
3.  Bepalen waar uw experiment accepteert u gegevens van de gebruiker van service en welke gegevens worden geretourneerd.

## <a name="set-up-web-service-button"></a>De knop Web-Service instellen

Nadat u uw experiment (knop**uitvoeren** onder in het canvas experiment) hebt uitgevoerd, wordt de knop **Web Service instellen** (Selecteer de optie **Voorspellende Web Service** ) de drie stappen van het experiment training converteren naar een voorspellende experiment voor u uitvoeren:

1.  Uw ervaren model wordt opgeslagen als een module in de sectie **Opgeleide modellen** van het palet module (aan de linkerkant van het canvas experiment) wordt vervangen door de computer leren algoritme en [Trein Model] [ train-model] modules met de opgeslagen opgeleide model.
2.  Modules die duidelijk niet nodig zijn worden verwijderd. In ons voorbeeld hierbij de [Gesplitste gegevens][split], 2e [Score-Model][score-model], en het [Evalueren van Model] [ evaluate-model] modules.
3.  Het maakt Web service input en output modules en in verschillende locaties in de experiment toegevoegd.

Bijvoorbeeld traint het volgende experiment twee klasse gestimuleerd besluit boom model census voorbeeldgegevens:

![Training-experiment][figure1]

De modules in dit experiment uitvoeren eigenlijk vier verschillende functies:

![Module-functies][figure2]

Wanneer u dit experiment training naar een voorspellende experiment converteert, sommige van deze modules niet langer nodig zijn of ze een ander doel hebben:

- **Gegevens** - de gegevens in deze dataset monster wordt niet gebruikt tijdens het scoren van-de gebruiker van de web-service levert de gegevens die moeten worden gescoord. De metagegevens van deze dataset, zoals gegevenstypen, wordt echter door het opgeleide model gebruikt. U moet dus de gegevensset in het experiment voorspellende houden zodat deze metagegevens kan bieden.

- **Prep** - afhankelijk van de gegevens die worden verzonden voor het scoren, deze modules kunnen al dan niet nodig voor het verwerken van de binnenkomende gegevens.

    Bijvoorbeeld, in dit voorbeeld de dataset monster ontbrekende waarden kan hebben en kolommen die niet nodig zijn voor het trainen van het model bevat. Dus een [Schone ontbrekende gegevens] [ clean-missing-data] module is opgenomen om de deal met ontbrekende waarden en een [Kolommen selecteren in de Dataset] [ select-columns] module is opgenomen die extra kolommen uitsluiten van de gegevensstroom. Als u weet dat de gegevens die worden verzonden voor het scoren van via de web-service geen waarden ontbreken, dan kunt u de [Schone ontbrekende gegevens] [ clean-missing-data] module. Echter, sinds de [Kolommen selecteren in de Dataset] [ select-columns] module helpt u bij het definiëren van de set van functies wordt gescoord, die module moet blijven.

- **Trein** - nadat het model is getraind, u deze opslaan als een model met één daartoe opgeleide module. Vervolgens vervangt u deze afzonderlijke modules met de opgeslagen opgeleide model.

- **Score** - wordt In dit voorbeeld wordt de module splitsen gebruikt voor het verdelen van de gegevensstroom in een set testgegevens en training. In het experiment voorspellende is niet nodig en kan worden verwijderd. Ook de 2e [Score-Model] [ score-model] module en het [Evalueren van Model] [ evaluate-model] module worden gebruikt voor het vergelijken van resultaten van de experimentele gegevens, zodat deze modules zijn ook niet nodig is in de voorspellende experiment. De resterende [Score-Model] [ score-model] module, is echter een score resultaat via de webservice vereist.

Dit is de weergave van ons voorbeeld klikt u op **Webservice instellen**:

![Geconverteerde voorspellende experiment][figure3]

Dit kan worden volstaan met het voorbereiden van het experiment te worden geïmplementeerd als een webservice. U wilt echter aanvullende werken voor uw experiment.

### <a name="adjust-input-and-output-modules"></a>Modules voor invoer en uitvoer aanpassen

In het experiment training training gegevens gebruikt en vervolgens enkele verwerking om de gegevens in een vorm die het algoritme leren machine nodig heeft. Als de gegevens die u verwacht te ontvangen via de web-service niet nodig deze verwerking, kunt u de **invoer module Web service** aan een ander knooppunt verplaatsen in uw experiment.

Standaard wordt **Web Service stelt u** bijvoorbeeld de module **Web service invoer** aan de bovenkant van de gegevensstroom, zoals in de bovenstaande afbeelding geplaatst. Als de ingevoerde gegevens niet nodig deze verwerking, kunt vervolgens u echter handmatig plaatsen **Web service input** voorbij de gegevensverwerking modules:

![De invoer van de service web verplaatsen][figure4]

De ingevoerde gegevens verstrekt via de webservice wordt nu rechtstreeks in de module Score-Model doorgeven zonder enige voorverwerking.

Standaard wordt **Webservice instellen** op dezelfde manier de module Web service uitvoer onderaan in de gegevensstroom. In dit voorbeeld wordt de webservice keert terug naar de gebruiker de uitvoer van de [Score-Model] [ score-model] module waarin de volledige invoergegevens vector plus de scores voor het resultaat.

Echter, als u liever een ander - retourneren bijvoorbeeld, alleen de resultaten van de score en niet de hele vector van ingevoerde gegevens- en u kunnen invoegen een [Kolommen selecteren in de Dataset] [ select-columns] module uit te sluiten van alle kolommen behalve de resultaten van de score. U de **Web service output** module vervolgens verplaatst naar de uitvoer van de [Kolommen selecteren in de Dataset] [ select-columns] module:

![De uitvoer van de service web verplaatsen][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Toevoegen of verwijderen van modules voor verdere verwerking van gegevens

Als er meer modules in uw experiment waarvan u weet dat tijdens de score niet meer nodig zijn, kunnen deze worden verwijderd. Omdat we de **Web service input** module naar een positie na de gegevensverwerking modules verplaatst hebben, kunnen we bijvoorbeeld de [Schone ontbrekende gegevens] verwijderen[ clean-missing-data] module uit de voorspellende experiment.

Ons experiment voorspellende ziet er nu als volgt uit:

![Extra module verwijderen][figure6]

### <a name="add-optional-web-service-parameters"></a>Optionele Parameters van de Web Service toevoegen

In sommige gevallen wilt u mogelijk kan de gebruiker het gedrag van modules wijzigen wanneer de service wordt geopend van de webservice. *Parameters van de Web Service* kunt u dit doen.

Een algemeen voorbeeld wordt ingesteld op de [Gegevens importeren] [ import-data] module zodat de gebruiker van de geïmplementeerde webservice een andere gegevensbron opgeven kunt wanneer de webservice wordt geopend. Of de configuratie van de [Gegevens exporteren] [ export-data] module zodat u een andere bestemming kan worden opgegeven.

U kunt Web Service Parameters definiëren koppelen aan een of meer moduleparameters en kunt u opgeven of deze verplicht of optioneel zijn. De gebruiker van de web-service kan vervolgens waarden opgeven voor deze parameters wanneer toegang verkregen de service tot wordt en de acties van de module wordt dienovereenkomstig gewijzigd.

Zie voor meer informatie over de Parameters van de Web Service [Met behulp van Azure Machine Learning Web Service Parameters ] [ webserviceparameters].

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>Het experiment voorspellende implementeren als een webservice

Nu dat het experiment voorspellende voldoende is voorbereid, kunt u deze implementeren als een webservice Azure. Met de webservice, kunnen gebruikers gegevens verzenden naar het model en het model retourneert de voorspellingen.

Zie voor meer informatie over het proces van volledige implementatie, [implementeren, een webservice Azure Machine Learning][deploy]

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

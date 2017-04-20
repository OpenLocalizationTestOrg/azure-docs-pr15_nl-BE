<properties
    pageTitle="Kies de parameters voor het optimaliseren van de algoritmen in Azure Machine Learning | Microsoft Azure"
    description="Hoe kiest u de optimale parameter is ingesteld voor een algoritme in Azure Machine Learning."
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
    ms.author="bradsev" />


# <a name="choose-parameters-to-optimize-your-algorithms-in-azure-machine-learning"></a>Optimaliseer uw algoritmen in Azure Machine Learning parameters kiezen

Dit onderwerp wordt beschreven hoe u de juiste hyperparameter instelt voor een algoritme in Azure Machine Learning. De meeste machine learning algoritmen hebben parameters in te stellen. Wanneer u een model training, moet u waarden opgeven voor deze parameters. De werkzaamheid van de opgeleide model is afhankelijk van de Modelparameters die u hebt gekozen. Het proces van het zoeken naar de optimale set parameters wordt de *modelselectie*genoemd.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Er zijn verschillende manieren om Modelleer selectie. In de machine learning, cross-validatie is een van de meest gebruikte methoden voor de selectie van model en is het standaard model selectie mechanisme in Azure Machine leren. Omdat Azure Machine Learning zowel R als Python ondersteunt, kunt u altijd hun eigen model selectie mechanismen implementeren met behulp van R of Python.

Er zijn vier stappen bij het vinden van de beste parameter ingesteld:

1.  **De parameter ruimte definiëren**: voor het algoritme, de precieze parameterwaarden u eerst beslissen.
2.  **Instellingen voor de cross-validatie definiëren**: bepalen hoe u wilt cross-validatie vouwen voor de gegevensset kiezen.
3.  **De metrische gegevens definiëren**: beslissen welke metric te gebruiken voor het bepalen van de beste set van parameters, zoals de nauwkeurigheid, fout, precisie, intrekken of f-score gemiddelde basis het kwadraat.
4.  **Trein, evalueren, en vergelijken**: voor elke unieke combinatie van de parameterwaarden cross-validatie is uitgevoerd door en op basis van de fout metric die u definieert. Na evaluatie en vergelijking kunt u het best presterende model.

In de volgende afbeelding ziet u hoe dit kan worden bereikt in Azure Machine Learning geeft.

![Zoek de beste parameter ingesteld](./media/machine-learning-algorithm-parameters-optimize/fig1.png)

## <a name="define-the-parameter-space"></a>Definieer de parameter ruimte
U kunt de parameter is ingesteld op de stap van de initialisatie model definiëren. Het deelvenster parameter van alle machine learning algoritmen heeft twee modi van de cursusleider: *Één Parameter* en de *Parameter bereik*. Kies de modus Parameter bereik. In de modus Parameter bereik, kunt u meerdere waarden voor elke parameter. Door lijstscheidingstekens gescheiden waarden kunt u invoeren in het tekstvak.

![Twee klasse gestimuleerd beslissingsstructuur één parameter](./media/machine-learning-algorithm-parameters-optimize/fig2.png)

 Ook kunt u de maximale en minimale punten van het raster en het totale aantal punten met **Gebruik bereik Builder**worden gegenereerd. Standaard worden de parameterwaarden van een lineaire schaal gegenereerd. Maar als een **Logaritmische schaal** is ingeschakeld, worden de waarden in de logaritmische schaal gegenereerd (de verhouding van de aangrenzende punten is constant in plaats van hun verschil). Voor integer parameters, kunt u een bereik kunt definiëren met een koppelteken. Bijvoorbeeld '1-10' houdt in dat alle gehele getallen tussen 1 en 10 (beide volledig) de parameter is ingesteld vormen. Een gemengde modus wordt ook ondersteund. Zo stelt u de parameter ' 1-10, 20, 50 ' zijn gehele getallen van 1-10, 20, en 50.

![Twee klasse gestimuleerd beslissingsstructuur liggen](./media/machine-learning-algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Vouwen cross-validatie definiëren
De [partitie en monster] [ partition-and-sample] module kan worden gebruikt om willekeurig vouwen aan de gegevens toewijzen. In het volgende voorbeeld configuratie voor de module we vijf vouwen definiëren en willekeurig nummeren Vouw de exemplaren van het monster.

![Partitie en monster](./media/machine-learning-algorithm-parameters-optimize/fig4.png)


## <a name="define-the-metric"></a>De metrische gegevens definiëren
Het [Afstemmen van Model Hyperparameters] [ tune-model-hyperparameters] -module biedt ondersteuning voor het kiezen van de beste set parameters voor een bepaald algoritme en de dataset empirisch. Naast andere informatie bevat met betrekking tot het model, het deelvenster **Eigenschappen** van deze module training de metric voor de bepaling van de beste parameter ingesteld. Twee andere vervolgkeuzelijst vakken voor indeling en regressie algoritmen, heeft respectievelijk. Als een classificatie-algoritme is het algoritme in aanmerking genomen, de metric regressie wordt genegeerd en vice versa. De metric is in dit specifieke voorbeeld **nauwkeurigheid**.   

![Sweep-parameters](./media/machine-learning-algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>Trainen, evalueren en vergelijken  
De dezelfde [Afstemmen Model Hyperparameters] [ tune-model-hyperparameters] module traint u de modellen die met de parameter is ingesteld overeenkomen, wordt geëvalueerd verschillende maatstaven en maakt u het best opgeleide model op basis van de metric die u kiest. Deze module bestaat uit twee verplichte invoeren:

* De ongetrainde cursist
* De dataset

De module heeft ook een optionele dataset invoer. De dataset verbinden met de fold-gegevens naar de invoer verplicht dataset. Als u de dataset niet alle informatie vouwen is toegewezen, vervolgens een 10-fold cross-validatie automatisch standaard uitgevoerd. Als de toewijzing vouwen niet wordt gedaan en dataset validatie wordt geleverd in de haven van optionele dataset, een trein test-modus wordt gekozen en de eerste dataset wordt gebruikt voor het trainen van het model voor elke combinatie van parameters.

![Gestimuleerd besluit boom classificatie](./media/machine-learning-algorithm-parameters-optimize/fig6a.png)

Vervolgens wordt het model van de dataset validatie geëvalueerd. De linker-uitvoerpoort van de module bevat verschillende maatstaven als functies van de parameterwaarden. De opgeleide model dat overeenkomt met geeft de juiste uitvoerpoort tot de best presterende model op basis van de gekozen metric (**nauwkeurigheid** in dit geval).  

![Validatie dataset](./media/machine-learning-algorithm-parameters-optimize/fig6b.png)

Hier ziet u de exacte parameters gekozen door de juiste uitvoerpoort visualiseren. Dit model kan worden gebruikt in een set test score of een geoperationaliseerd webservice na opslaan als een getrainde model.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/

<properties 
    pageTitle="Fouten opsporen in uw Model in Azure Machine Learning | Microsoft Azure" 
    description="Wordt uitgelegd hoe u foutopsporing uitvoeren van uw Model in Azure Machine Learning." 
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
    ms.date="09/09/2016" 
    ms.author="bradsev;garye" />

# <a name="debug-your-model-in-azure-machine-learning"></a>Fouten opsporen in uw Model in Azure Machine Learning

In dit artikel wordt uitgelegd hoe de modellen in Microsoft Azure Machine Learning debug. Dit omvat met name de mogelijke redenen waarom een van de volgende twee scenario's voor storingen optreden kunnen bij het uitvoeren van een model:

* het [Model van de trein] [ train-model] module genereert een fout 
* de [Score-Model] [ score-model] module onjuiste resultaten oplevert 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-throws-an-error"></a>Trein Model Module genereert een fout

![image1](./media/machine-learning-debug-models/train_model-1.png)

Het [Model van de trein] [ train-model] Module de volgende 2 ingangen verwacht:

1. Het type indeling/regressie Model uit de collectie van modellen met Azure Machine leren
2. De trainingsgegevens met een opgegeven labelkolom. De kolom Label geeft de variabele te voorspellen. De rest van de kolommen die zijn opgenomen, wordt aangenomen dat functies.

In deze module genereert een fout in de volgende gevallen:

1. De Label-kolom is onjuist opgegeven omdat meer dan één kolom is geselecteerd als het Label of een onjuiste column-index is geselecteerd. Het tweede geval zou bijvoorbeeld van toepassing als de index van een kolom van 30 is gebruikt met een invoer dataset die slechts 25 kolommen heeft.

2. De dataset bevat geen functie kolommen. Bijvoorbeeld, als de invoer dataset slechts 1 kolom is gemarkeerd als de kolom Label bevat, zou er geen functies die het model te bouwen. In dit geval het [Model trein] [ train-model] module genereert een fout.

3. Invoer dataset (functies of Label) oneindig als een waarde bevatten.


## <a name="score-model-module-does-not-produce-correct-results"></a>Score Model Module levert geen juiste resultaten

![image2](./media/machine-learning-debug-models/train_test-2.png)

In een gewone grafiek training/tests voor het toezicht leren, de [Gesplitste gegevens] [ split] module wordt de oorspronkelijke gegevensset verdeeld in twee delen: het deel dat wordt gebruikt voor het model van de trein en het deel dat wordt gereserveerd voor de score hoe goed de opgeleide model wordt uitgevoerd op gegevens deze heeft geen training op. De opgeleide model wordt vervolgens gebruikt voor het verkrijgen van de testgegevens waarna de resultaten worden geëvalueerd om na te gaan van de juistheid van het model.

De [Score-Model] [ score-model] module vereist twee ingangen:

1. Een ervaren model uitvoer uit [Model trein] [ train-model] module
2. Een score dataset niet dat het model is niet getraind op

Het kan voorkomen dat, hoewel het experiment slaagt, wordt de [Score-Model] [ score-model] module onjuiste resultaten oplevert. Verschillende scenario's mogelijk dat dit gebeurt:

1. Als de opgegeven Label bestaan en een regressiemodel is getraind op de gegevens, een onjuist uitgevoerd zou worden teweeggebracht door het [Score-Model] [ score-model] module. Dit komt doordat de regressie is een variabele voor doorlopende antwoord vereist. In dit geval moet meer geschikt is voor een classificatie model gebruiken. 
2. Op dezelfde manier als een model voor de classificatie wordt getraind voor een dataset die getallen met drijvende komma in de kolom Label, kan er ongewenste resultaten opleveren. Dit komt omdat de indeling moet een aparte reactie variabele die alleen dat bereik waarden via een eindige en meestal iets kleine set van klassen kunt.
3. Als de score dataset niet alle functies bevat voor het trainen van het model, de [Score-Model] [ score-model] geeft een fout.
4. De [Score-Model] [ score-model] zou niet overeenkomt met een rij in de score dataset met een ontbrekende waarde of een oneindige waarde voor een van de functies van uitvoer produceren.
5. De [Score-Model] [ score-model] kan leiden tot identieke resultaten voor alle rijen in de dataset score. Dit kan gebeuren, bijvoorbeeld in de wanneer probeert indeling in Forests besluit als het minimumaantal monsters per leaf-knooppunt wilt worden van meer dan het aantal beschikbare oefenvoorbeelden.


<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
 

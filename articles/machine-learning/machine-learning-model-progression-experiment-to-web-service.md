<properties
    pageTitle="Hoe een Machine Learning model beweegt uit een experiment met een webservice operationalized | Microsoft Azure"
    description="Een overzicht van het mechanisme van de manier waarop de voortgang van uw Azure Machine Learning model van een ontwikkeling naar een webservice operationalized experimenteren."
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


# <a name="how-a-machine-learning-model-progresses-from-an-experiment-to-an-operationalized-web-service"></a>Hoe een Machine Learning model beweegt uit een experiment met een webservice operationalized

Azure Machine Learning Studio biedt een interactieve canvas waarmee u kunt ontwikkelen, uitvoeren, testen en doorlopen een ***experimenteren*** die een voorspellende analyse model vertegenwoordigt. Er zijn tal van modules die u kunt:

* Gegevens die zijn ingevoerd in het experiment.
* De gegevens bewerken
* Een model met machine learning algoritmen trainen
* Score van het model
* Evalueren van de resultaten
* Definitieve uitvoerwaarden

Als u tevreden met uw experiment bent, kunt u het implementeren als een ***klassieke Azure Machine Learning webservice*** of een ***nieuwe Azure Machine Learning Web service*** , zodat gebruikers kunnen nieuwe gegevens verzenden en ontvangen van vorige resultaten.

In dit artikel geven we een overzicht van het mechanisme van de manier waarop de voortgang van uw Machine Learning model van een ontwikkeling naar een webservice operationalized experimenteren.

>[AZURE.NOTE] Er zijn andere manieren om te ontwikkelen en implementeren van machine learning modellen, maar dit artikel is gericht op het gebruik van Machine Learning Studio. Zie voor een bespreking van een klassieke voorspellende webservice maken met R de blog [bouwen en implementeren voorspellende Web Apps met behulp van RStudio en Azure ML](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx)boeken.

Azure Machine Learning Studio is ontworpen voor het ontwikkelen en distribueren van een *model van de voorspellende analyse*, is het Studio gebruiken voor het ontwikkelen van een experiment waarin een model voorspellende analyse niet. Een experiment kan bijvoorbeeld alleen gegevens invoeren, bewerken en de resultaten. Net als een experiment voorspellende analyse kunt u dit niet-anticiperende experiment als een webservice implementeren, maar is een eenvoudiger proces omdat het experiment niet training of een machine learning model scoren. Het is niet de typische Studio op deze manier gebruiken, zullen we het ook in de discussie zodat wij een volledige uitleg geven van de werking van Studio.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Ontwikkelen en implementeren van een anticiperende webservice

Hier zijn de fasen die een standaardoplossing volgt bij het ontwikkelen en implementeren met behulp van Machine Learning Studio:

![Distributie stroom](media\machine-learning-model-progression-experiment-to-web-service\model-stages-from-experiment-to-web-service.png)

*Figuur 1 - fasen van een typische voorspellende analyse model*

### <a name="the-training-experiment"></a>Het experiment training

De ***training experimenteren*** is de eerste fase van de ontwikkeling van de webservice in de Studio van Machine Learning. Het doel van het experiment training is om een plaats voor het ontwikkelen, testen, herhalen en uiteindelijk een machine model leren trainen. U kunt zelfs trainen meerdere modellen gelijktijdig als u naar de beste oplossing zoeken, maar als u klaar bent kunt u uitproberen één opgeleid kiest model en de rest van het experiment te elimineren. Een voorbeeld van het ontwikkelen van een voorspellende analyse experimenteren, kunt u [een anticiperende analytische oplossing voor de beoordeling van het kredietrisico in Azure Machine Learning ontwikkelen](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="the-predictive-experiment"></a>De voorspellende experiment

Zodra er een opgeleide model in de experiment training, klikt u op **Webservice instellen** en selecteer **Voorspellende webservice** in Machine Learning Studio aan het proces van het experiment training converteren naar een ***voorspellende experiment***begint. Het doel van het experiment voorspellende is uw ervaren model gebruiken voor het verkrijgen van nieuwe gegevens, met het doel uiteindelijk steeds geoperationaliseerd als een webservice Azure.

Deze conversie is gedaan om u via de volgende stappen uit:

-   De set van modules voor opleiding in een enkele module gebruikt converteren en opslaan als een getrainde model

-   Elimineer alle overbodige modules die niet gerelateerd is aan de score

-   Invoer en uitvoer-poorten waarmee de uiteindelijke webservice toevoegen

Mogelijk zijn er meer wijzigingen aanbrengen in uw experiment voorspellende bereid te implementeren als een webservice. Als u wilt dat de webservice om alleen een subset van de resultaten, kunt u bijvoorbeeld een filter module voordat de uitvoerpoort toevoegen.

Tijdens deze conversie wordt het experiment training niet verwijderd. Wanneer het proces voltooid is, hebt u twee tabbladen in Studio: één voor het experiment training en één voor de voorspellende experiment. Op deze manier die u wijzigingen aan het experiment training aanbrengen kunt voordat u uw Web-service implementeert en de voorspellende experiment opnieuw. Of u kunt een kopie opslaan van het experiment opleiding om een andere regel van experimenten te beginnen.

>[AZURE.NOTE] Wanneer u klikt op **Voorspellende webservice** u automatisch starten als u wilt uw training experiment omzetten in een experiment voorspellende en dit werkt goed in de meeste gevallen. Als uw experiment training complex is (bijvoorbeeld: u hebt meerdere paden voor training die u samenvoegen), misschien wilt u deze conversie handmatig doen. Zie [een Machine Learning training experiment een voorspellende experiment converteren](machine-learning-convert-training-experiment-to-scoring-experiment.md)voor meer informatie.

### <a name="the-web-service"></a>De webservice

Als u tevreden bent dat uw experiment voorspellende gereed is, u uw service als een klassieke webservice implementeren kunt of een nieuwe webservice op basis van Azure Resource Manager. Als u wilt mogelijk uw model maken door als een *klassieke Machine Learning Web-service*implementeert, **Web Service implementeren** op en selecteer **Webservice implementeren [Classic]**. Als u wilt implementeren als *nieuwe Machine Learning Web service*, klikt u op **Webservice implementeren** en selecteer **Implementeren Web Service [Nieuw]**. Gebruikers kunnen nu gegevens verzenden naar het model met de webservice REST API en ontvangen van de resultaten. Zie voor meer informatie [een Azure Machine Learning Web-service die is geïmplementeerd in een Machine Learning experiment in beslag nemen](machine-learning-consume-web-services.md).

## <a name="the-non-typical-case-creating-a-non-predictive-web-service"></a>Het geval van niet-typische: maken van een niet-anticiperende webservice

Als uw experiment heeft geen training een voorspellende analyse model en vervolgens een experiment training en een score experiment maken hoeft niet - er is slechts één experiment en kunt u deze implementeren als een webservice. Machine Learning Studio detecteert of uw experiment een voorspellende model bevat door een analyse van de modules die u hebt gebruikt.

Nadat u hebt op uw experiment herhaald en er tevreden mee:

1.  Klik op **Webservice instellen** en selecteer **Webservice bijscholing** - invoer en uitvoer knooppunten worden automatisch toegevoegd

2.  Klik op **uitvoeren**

3. Klik op **Web Service implementeren** en selecteer **Webservice implementeren [Classic]** of **Implementeren van Web Service [Nieuw]** afhankelijk van de omgeving die u wilt implementeren.

De webservice wordt nu geïnstalleerd en kunt u deze net als een anticiperende webservice beheren en toegang.

## <a name="updating-your-web-service"></a>Bijwerken van uw Web-service

Nu u uw experiment als webservice hebt geïmplementeerd, wat gebeurt er als u moet werken?

Dat hangt af van wat u nodig hebt om te werken:

**U wilt wijzigen van de invoer of uitvoer, of u wilt wijzigen hoe gegevens worden bewerkt door de webservice**

Als u het model niet is gewijzigd, maar alleen hoe gegevens worden verwerkt door de webservice wilt wijzigen, kunt u bewerken het experiment voorspellende en vervolgens klikt u op **Webservice implementeren** en **Web-Service implementeren [Classic]** of **Implementeren van Web Service [Nieuw]** opnieuw selecteren. De Web-service is gestopt, de bijgewerkte voorspellende experiment wordt geïmplementeerd en de webservice opnieuw wordt gestart.

Hier volgt een voorbeeld: Stel dat de voorspellende experiment geeft als resultaat de hele rij met gegevens die zijn ingevoerd met het verwachte resultaat. U kan besluiten dat de webservice NET resultaat. Zo kunt u een module **Project kolommen** toevoegen in het experiment voorspellende rechts voordat de uitvoerpoort uitsluiten van andere kolommen dan het resultaat. Wanneer u op **Implementeren webservice** en **Webservice implementeren [Classic]** of **Implementeren van Web Service [Nieuw]** opnieuw selecteert, wordt de Web-service bijgewerkt.

**U wilt het model met de nieuwe gegevens retrain**

Als u wilt uw machine leren model behouden, maar u wilt deze retrain met nieuwe gegevens, hebt u twee mogelijkheden:

1.  **Het model terwijl de webservice actief is retrain** - als u wilt uw model tijdens de voorspellende retrain Web-service wordt uitgevoerd, kunt u dit doen door een paar wijzigingen worden aangebracht aan het experiment opleiding om een ***experiment opnieuw opleiden***en vervolgens kunt u deze implementeren als een **webservice *opnieuw opleiden* **. Zie voor instructies over hoe u dit doet, [Machine Learning Retrain modellen via programmacode](machine-learning-retrain-models-programmatically.md).

2.  **Ga terug naar de originele experiment van opleiding en training voor andere gegevens gebruiken voor het ontwikkelen van uw model** - de voorspellende experiment is gekoppeld aan de webservice, maar het experiment training niet rechtstreeks is gekoppeld op deze manier. Als u de oorspronkelijke opleiding experiment wijzigen en klikt u op **Webservice instellen**, het maakt een *nieuwe* voorspellende experimenteren, die als geïmplementeerd, wordt er een *nieuwe* webservice. Het bijwerken niet alleen de oorspronkelijke webservice.

    Als u het experiment training te wijzigen, opent u het en klikt u op **OpslaanAls** om een kopie te maken. Dit wordt behouden de oorspronkelijke opleiding experiment voorspellende experiment, en Web-service. U kunt nu een nieuwe webservice maken met uw wijzigingen. Zodra u de nieuwe webservice, die kunt u vervolgens bepalen of de vorige Web-service stoppen of te laten samen met de nieuwe sjabloon hebt geïmplementeerd.

**U wilt een ander model van de trein**

Als u wijzigingen aanbrengen aan de oorspronkelijke voorspellende experiment wilt, zoals het selecteren van een andere machine learning-algoritme probeert een andere training methode, enz., moet u de tweede procedure hierboven voor uw model bijscholing volgen: het experiment training te openen, klikt u op **OpslaanAls** om te kopiëren en start u het nieuwe pad van de ontwikkeling van het model , de voorspellende experiment maken en implementeren van de webservice. Hiermee maakt u een nieuw Web service die geen verband houden met de oorspronkelijke - kunt u bepalen welke één of beide blijven uitvoeren.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het proces van ontwikkeling en experiment, de volgende artikelen:

-   converteren van het experiment - [een experiment Machine Learning training een voorspellende experiment converteren](machine-learning-convert-training-experiment-to-scoring-experiment.md)

-   de webservice - [een webservice Azure Machine Learning implementeren](machine-learning-publish-a-machine-learning-web-service.md) distribueren

-   het model - [Retrain Machine Learning modellen programmatisch](machine-learning-retrain-models-programmatically.md) bijscholing

Zie voor voorbeelden van het hele proces:

-   [Machine learning zelfstudie: Maak uw eerste experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md)

-   [Procedure: Een anticiperende analytische oplossing voor de beoordeling van het kredietrisico in Azure Machine Learning ontwikkelen](machine-learning-walkthrough-develop-predictive-solution.md)
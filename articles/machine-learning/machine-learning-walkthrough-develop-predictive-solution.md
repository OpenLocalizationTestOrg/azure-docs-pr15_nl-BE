<properties
    pageTitle="Een anticiperende oplossing voor kredietrisico met Machine Learning | Microsoft Azure"
    description="Een gedetailleerd overzicht met het maken van een anticiperende analytische oplossing voor de beoordeling van het kredietrisico in Azure Machine Learning Studio."
    keywords="kredietrisico, voorspellende analytics oplossing, risico-evaluatie"
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
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Procedure: Een anticiperende analytische oplossing voor de beoordeling van het kredietrisico in Azure Machine Learning ontwikkelen

Stel dat u nodig hebt om het kredietrisico van een persoon op basis van deze gegevens op een kredietaanvraag te voorspellen.  

Beoordeling van het kredietrisico is een complex probleem, natuurlijk, maar laten we een beetje vereenvoudigen de parameters van de vraag. We kunnen het vervolgens gebruiken als voorbeeld van hoe u Microsoft Azure Machine Learning met Machine Learning Studio en de Machine Learning webservice kunt voor het maken van een dergelijke voorspellende analytics oplossing.  

In dit gedetailleerde overzicht volgen we het proces van een anticiperende analytische model in de Studio van Machine Learning ontwikkelen en implementeren van deze vervolgens als een webservice Azure Machine Learning. We beginnen met de gegevens van openbaar beschikbare krediet-risico, ontwikkelen en trainen een voorspellende model op basis van die gegevens en vervolgens het model als een webservice die door anderen kan worden gebruikt voor de beoordeling van het kredietrisico te implementeren.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<!-- -->

>[AZURE.TIP] Als u wilt downloaden en afdrukken van een diagram een overzicht van de mogelijkheden van Machine Learning Studio geeft, Zie [diagram overzicht van de mogelijkheden van Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md).

Maken een oplossing krediet risico-evaluatie, zullen we als volgt te werk:  

1.  [Een Machine Learning werkruimte maken](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Bestaande gegevens uploaden](machine-learning-walkthrough-2-upload-data.md)
3.  [Maak een nieuw experiment](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Trainen en het evalueren van de modellen](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [De web-service implementeren](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Toegang tot de webservice.](machine-learning-walkthrough-6-access-web-service.md)

In dit scenario is gebaseerd op een vereenvoudigde versie van de [binaire Classfication: risico voorspelling Credit](http://go.microsoft.com/fwlink/?LinkID=525270) monster experiment in de [Galerie met Cortana Intelligence](http://gallery.cortanaintelligence.com/).

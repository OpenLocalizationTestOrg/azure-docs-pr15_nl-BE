<properties
    pageTitle="Stap 6: Toegang tot de webservice Machine Learning | Microsoft Azure"
    description="Stap 6 van het ontwikkelen van een oplossing voor anticiperende scenario: toegang tot een actieve Azure Machine Learning webservice."
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


# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>Scenario-stap 6: Toegang tot de webservice Azure Machine Learning

Dit is de laatste stap van de procedure, het [ontwikkelen van een voorspellende analytics oplossing in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Een Machine Learning werkruimte maken](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Bestaande gegevens uploaden](machine-learning-walkthrough-2-upload-data.md)
3.  [Maak een nieuw experiment](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Trainen en het evalueren van de modellen](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [De web-service implementeren](machine-learning-walkthrough-5-publish-web-service.md)
6.  **Toegang tot de webservice.**

----------

We hebben geïmplementeerd een webservice die gebruikmaakt van onze voorspellingsmodel krediet risico's in de vorige stap in deze procedure. Gebruikers moeten nu worden die gegevens verzenden en ontvangen van resultaten. 

De webservice is een Azure webservice kunt ontvangen en gegevens met REST API's op twee manieren:  

-   **Aanvraag/reactie** - de gebruiker een of meer rijen met gegevens van de creditnota naar de service verzendt via een HTTP-protocol en de service reageert met een of meer sets van resultaten.
-   **Batch Execution** - de gebruiker een of meer rijen met creditcard gegevens opgeslagen in een Azure blob en verzendt vervolgens de locatie van de blob met de service. De service scoort alle gegevensrijen in de blob invoer, worden de resultaten opgeslagen in een andere blob en retourneert de URL van die container.  

De snelste en eenvoudigste manier om toegang tot de webservice is via het Web App sjablonen die beschikbaar zijn in de [Azure Web App marktplaats](https://azure.microsoft.com/marketplace/web-applications/all/).
Deze sjablonen web app kunnen maken voor een aangepaste web app dat weet uw webservice invoergegevens en wat wordt geretourneerd. Hoeft u toegang tot uw web-service en gegevens is en doet de rest van de sjabloon.

Zie voor meer informatie over het gebruik van app websjablonen [verbruiken een Azure Machine Learning web-service met een websjabloon app](machine-learning-consume-web-service-with-web-app-template.md).

U kunt ook een aangepaste toepassing voor toegang tot de webservice met starter-code waarin u R, C# en programmeertalen Python ontwikkelen.
Volledige details vindt u in [hoe een Azure Machine Learning web-service die is gepubliceerd vanuit een Machine Learning experiment in beslag neemt](machine-learning-consume-web-services.md).

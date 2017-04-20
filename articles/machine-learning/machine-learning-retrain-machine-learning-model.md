<properties
    pageTitle="Een Machine leren Model Retrain | Microsoft Azure"
    description="Informatie over het model retrain en bijwerken van de webservice voor het gebruik van het model nieuw opgeleide in Azure Machine Learning."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-machine-learning-model"></a>Een Machine leren Model Retrain

Als onderdeel van het proces voor de uitoefening van machine learning modellen in Azure computer leren uw model getraind en opgeslagen. Klikt u voor het maken van een predicative op Internet. De webservice kan vervolgens worden verbruikt in websites, dashboards en mobiele toepassingen. 

Meestal zijn de modellen die u maakt met de computer leren niet statisch. Zodra er nieuwe gegevens beschikbaar zijn of wanneer de consument van de API hun eigen gegevens heeft moet het model worden retrained. 

Bijscholing kan regelmatig voorkomen. Met de functie voor programmatische bijscholing API kunt u programmatisch retrain het model met behulp van de API's voor herscholing en de webservice met het model nieuw opgeleide bijwerken. 

Dit document beschrijft het proces opnieuw opleiden en ziet u hoe u de API's voor herscholing.

## <a name="why-retrain-defining-the-problem"></a>Waarom retrain: definiëren van het probleem  

Een model is getraind met behulp van een set gegevens als onderdeel van de machine opleiding leerproces. Meestal zijn de modellen die u maakt met de computer leren niet statisch. Zodra er nieuwe gegevens beschikbaar zijn of wanneer de consument van de API hun eigen gegevens heeft moet het model worden retrained.

In deze situaties is een programmatische API een handige manier om u of de consument van de API's voor het maken van een client die retrain kan, op een eenmalige of regelmatige basis, het model met hun eigen gegevens. Vervolgens kunnen zij evalueren van de resultaten van de bijscholing en Web service-API voor het gebruik van het model nieuw opgeleide bijwerken.

>[AZURE.NOTE] Als u een bestaande Training Experiment en nieuwe webservice, kunt u uitchecken Retrain een bestaande voorspellende webservice in plaats van de procedure vermeld in de volgende sectie.

## <a name="end-to-end-workflow"></a>End-to-end-werkstroom 

Het proces omvat de volgende onderdelen: A Training Experiment en een voorspellende Experiment gepubliceerd als een webservice. Om te schakelen van een opgeleide model bijscholing, moet het Experiment Training worden gepubliceerd als een webservice met de uitvoer van een opgeleide model. Hierdoor kunnen de API-toegang tot het objectmodel voor bijscholing. 

De volgende stappen zijn van toepassing op zowel nieuwe en klassieke Web services:

De eerste anticiperende webservice maken:

* Maak een experiment training
* Een experiment voorspellende web maken
* Een anticiperende webservice implementeren

De webservice Retrain:

* Update training experiment voor bijscholing
* De webservice opnieuw opleiden implementeren
* Gebruik de batchverwerking tot uitvoering van code aan het model retrain

Zie voor een overzicht van de voorgaande stappen [Retrain Machine Learning modellen via programmacode](machine-learning-retrain-models-programmatically.md).

Als u een webservice klassieke geïmplementeerd:

* Maak een nieuw eindpunt op de voorspellende Web service
* De PATCH URL en een code
* De PATCH-URL gebruiken om het nieuwe Endpoint wijst op het retrained model 

Zie voor een overzicht van de voorgaande stappen [Retrain een klassieke webservice](machine-learning-retrain-a-classic-web-service.md).

Als u in een klassieke webservice bijscholing moeilijkheden, Zie [problemen met de bijscholing van een Azure Machine Learning klassieke webservice](machine-learning-troubleshooting-retraining-models.md).

Als u een nieuw Web service geïmplementeerd:

* Aanmelden bij uw account Azure Resource Manager
* Definitie van de webservice ophalen
* Definitie van de webservice als JSON exporteren
* Bijwerken van de verwijzing naar de `ilearner` blob in de JSON
* De JSON importeren in een Web Service Definition
* De webservice bijwerken met nieuwe Web Service Definition

Zie [een nieuwe webservice met de Machine Learning Management PowerShell-cmdlets Retrain](machine-learning-retrain-new-web-service-using-powershell.md)voor een overzicht van de voorgaande stappen.

Het proces voor het instellen van omscholing voor een klassieke Internet omvat de volgende stappen uit:

![Overzicht bijscholing][1]

Het proces voor het instellen van omscholing voor een nieuwe webservice omvat de volgende stappen uit:

![Overzicht bijscholing][7]

## <a name="other-resources"></a>Andere bronnen

- [Bijscholing en bijwerken van Azure Machine Learning modellen met Azure Data Factory](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
- [Veel modellen van Machine Learning en web service-endpoints maken van een experiment met PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md)
- De [AML bijscholing modellen met behulp van API's](https://www.youtube.com/watch?v=wwjglA8xllg) video laat zien hoe u retrain Machine Learning modellen in Azure Machine Learning met behulp van de API's van herscholing en PowerShell.

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png


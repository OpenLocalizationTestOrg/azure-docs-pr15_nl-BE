<properties 
    pageTitle="Wat is Azure Machine Learning Studio? | Microsoft Azure"
    description="Overzicht van Azure ML Studio, een hulpprogramma voor slepen en neerzetten voor het snel bouwen van modellen uit een bibliotheek met kant-en-klare van algoritmen en modules."
    keywords="Azure machine leren, azure ml, ml studio"
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
    ms.date="09/09/2016"
    ms.author="garye"/>

# <a name="what-is-azure-machine-learning-studio"></a>Wat is Azure Machine Learning Studio?

Microsoft Azure Machine Learning Studio is een hulpprogramma voor samenwerking, slepen en neerzetten die kunt u bouwen, testen en implementeren van anticiperende analytische oplossingen voor uw gegevens. Machine Learning Studio worden modellen gepubliceerd als webservices die gemakkelijk kunnen worden gebruikt door aangepaste toepassingen of hulpprogramma's zoals Excel BI.

Machine Learning Studio is wanneer wetenschappelijke gegevens, voorspellende analytics cloud resources en de gegevens voldoen aan.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>De interactieve Machine Learning Studio-werkruimte

Voor het ontwikkelen van een voorspellende analyse model u meestal gegevens uit een of meer bronnen gebruiken, transformeren en analyseren via verschillende gegevens manipuleren en statistische functies en een reeks resultaten genereren. Ontwikkelen van een model zoals dit is een iteratief proces. Als u de verschillende functies en parameters wijzigt, uw resultaten convergeren totdat u tevreden bent dat u beschikt over een opgeleide, effectief model.

**Azure Machine Learning Studio** biedt u een interactieve, visuele werkruimte eenvoudig bouwen, testen en herhalen op een voorspellende analyse model. U slepen en neerzetten ***datasets*** en -analyse ***modules*** op een interactieve tekenpapier, zodat ze samen een ***experimenteren***, die u in de Machine Learning Studio uitvoert formulier verbinden. Om te bladeren op het modelontwerp, het experiment te bewerken, een kopie desgewenst opslaan en opnieuw uitvoeren. Wanneer u klaar bent, kunt u omzetten in uw ***training experimenteren*** een ***voorspellende experiment***en vervolgens publiceren als een ***webservice*** zodat het model kan worden geopend door anderen.

>[AZURE.TIP] Als u wilt downloaden en afdrukken van een diagram een overzicht van de mogelijkheden van Machine Learning Studio geeft, Zie [diagram overzicht van de mogelijkheden van Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md).

Er is geen programmeerkennis vereist, alleen visueel datasets en modules om uw voorspellende analyse model samen te stellen verbinding te maken.

![Azure ML Studio diagram: experimenten maken, gegevens voor vele bronnen lezen, schrijven gegevens gescoord, modellen schrijven.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Aan de slag met de computer leren Studio

Wanneer u eerst [Machine Learning Studio](https://studio.azureml.net) ziet u **de introductiepagina** . U kunt hier bekijken documentatie, video's en Webinars promotiemateriaal en andere waardevolle bronnen vinden.

Er zijn drie tabbladen aan de bovenzijde: **Home** (waar u begint), **Studio**en **Galerij**.

### <a name="studio"></a>Studio

Klik op het tabblad **Studio** en wordt u gevraagd aan te melden met uw Microsoft-account, of uw werk of school. Eenmaal ingelogd, ziet u de volgende tabbladen aan de linkerkant:

- **Projecten** - collecties van experimenten, datasets, laptops en andere bronnen die een project
- **EXPERIMENTEN** - experimenten die zijn gemaakt, worden uitgevoerd en opgeslagen als concept
- **WEB SERVICES** - Web-services die u hebt geïmplementeerd in uw experimenten
- **Laptops** - laptops voor Jupyter die u hebt gemaakt
- **DATASETS** - Datasets die je hebt geüpload naar Studio
- **Ervaren modellen** - modellen die u hebt getraind in experimenten en opgeslagen in de Studio
- **Instellingen** - een verzameling instellingen die u gebruiken kunt voor het configureren van uw account en bronnen.

### <a name="gallery"></a>Galerie

Klik op het tabblad **Galerie** en u zult worden genomen aan de galerie met Cortana Intelligence. De galerie is een plaats waar een Gemeenschap van wetenschappers van de gegevens en ontwikkelaars van oplossingen die zijn gemaakt met behulp van onderdelen van het pakket Cortana Intelligence kunt delen.

Zie voor meer informatie over de galerie [delen en oplossingen in de galerie met Cortana Intelligence ontdekken](machine-learning-gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Onderdelen van een experiment

Er bestaat een experiment uit datasets die gegevens voor analytische modules, die u met elkaar verbinden om een voorspellende analyse model samen te stellen. Een geldige experiment heeft met name deze kenmerken:

- De proef heeft ten minste één gegevensset en één module
- DataSets kan worden verbonden met modules
- Modules kunnen worden aangesloten op datasets of andere modules
- Alle input poorten voor modules die moeten een verbinding met de gegevensstroom hebben
- Alle vereiste parameters voor elke module moeten worden ingesteld.

Kunt u een experiment maken of u kunt een bestaande monster experiment als een sjabloon. Voor meer informatie, Zie [Gebruik monster experimenten voor het maken van nieuwe experimenten](machine-learning-sample-experiments.md).

Zie voor een voorbeeld van het maken van een eenvoudig experiment, [maken een eenvoudige experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md).

Zie voor een vollediger overzicht van het maken van een anticiperende analytische oplossing [ontwikkelen een voorspellende oplossing met Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>DataSets

Een dataset bestaat uit gegevens die is geüpload naar de Machine Learning Studio zodat deze kan worden gebruikt tijdens het modelleren. Een aantal voorbeeld datasets zijn opgenomen in de Studio van Machine Learning voor u om te experimenteren met en u kunt meer gegevenssets hebt uploaden. Hier volgen enkele voorbeelden van datasets opgenomen:

- **MPG-gegevens voor verschillende auto** - mijl per gallon (MPG) waarden voor auto geïdentificeerd door het aantal cilinders, paardenkracht, enz.
- **Borst kanker gegevens** - borst kanker diagnose gegevens.
- **Brand gegevens van Forest** - Forest fire formaten in het noordoosten van Portugal.

Het opzetten van een experiment kunt u uit de lijst met beschikbare datasets aan de linkerkant van het papier.

Zie [de steekproeven in Azure Machine Learning Studio gebruiken](machine-learning-use-sample-datasets.md)voor een lijst van monster datasets opgenomen in de Studio van Machine Learning.

### <a name="modules"></a>Modules

Een module is een algoritme die u op uw gegevens uitvoeren kunt. Machine Learning Studio heeft een aantal modules, variërend van ingress gegevensfuncties training scoren en validatieprocessen. Hier volgen enkele voorbeelden van modules opgenomen:

- [Converteren naar ARFF] [ convert-to-arff] -een dataset geserialiseerd .NET converteert naar kenmerk relatie bestand indeling (ARFF).
- [Elementaire statistieken berekenen] [ elementary-statistics] -berekent elementaire statistieken, zoals gemiddelde, standaarddeviatie, enz.
- [Lineaire regressie] [ linear-regression] -maakt een online verloop op basis van afkomst lineaire regressiemodel.
- [Score van Model] [ score-model] -scoort een opgeleide classificatie of regressie model.

Het opzetten van een experiment die kunt u in de lijst met beschikbare modules aan de linkerkant van het papier.  

Een module kan een set parameters die u gebruiken kunt voor het configureren van de module interne algoritmen hebben. Wanneer u een module op het canvas selecteren, worden de parameters van de module weergegeven in het deelvenster **Eigenschappen** aan de rechterkant van het papier. U kunt de parameters in dat deelvenster afstemmen van uw model wijzigen.

Zie voor sommige navigeren door de uitgebreide bibliotheek van machine learning algoritmen beschikbaar help, [het kiezen van algoritmen voor Microsoft Azure Machine leren](machine-learning-algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Een web analytics voorspellende service implementeren

Zodra uw analytics voorspellende model klaar is, kunt u deze implementeren als een webservice vanaf Machine Learning Studio. Zie voor meer informatie over dit proces [implementeren een webservice Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/

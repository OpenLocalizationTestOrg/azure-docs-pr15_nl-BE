<properties
    pageTitle="PowerShell-module voor Machine Learning | Microsoft Azure"
    description="De PowerShell-module voor Azure Machine Learning is beschikbaar in de openbare preview-modus. Gebruik PowerShell maakt en beheert werkruimten, experimenten en webservices."
    keywords="experimenteren, lineaire regressie, machine learning algoritmen, machine learning zelfstudie, voorspellende modellen technieken, data science experiment"
    services="machine-learning"
    documentationCenter=""
    authors="hning86"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="garye;haining"/>

# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>PowerShell-module voor Microsoft Azure Machine Learning

De PowerShell-module voor Azure Machine Learning is een krachtig hulpprogramma waarmee u Windows PowerShell gebruiken voor het beheren van werkruimten, experimenten, datasets, web, serivces en meer.

U kunt de documentatie en downloaden van de module, samen met de volledige broncode op [https://aka.ms/amlps](https://aka.ms/amlps). 

## <a name="what-is-the-machine-learning-powershell-module"></a>Wat is de module Machine Learning PowerShell?

De module Machine Learning PowerShell is een. Op basis van NET DLL-module waarmee u volledig Azure Machine Learning werkruimten, experimenten, datasets, webservices en eindpunten van een webservice beheren vanaf Windows PowerShell. Met de module kunt u downloaden van de volledige broncode inclusief een duidelijke gescheiden [C# API-laag](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). Hierdoor kunt u verwijzen naar dit dll-bestand uit uw eigen project .NET en Azure Machine Learning beheren via .NET-code. Het DLL-bestand is ook afhankelijk van onderliggende REST API's die u kunt rechtstreeks vanuit uw favoriete client.

## <a name="what-can-i-do-with-the-powershell-module"></a>Wat kan ik doen met de PowerShell-module

Hier zijn enkele van de taken die u met deze module PowerShell kunt uitvoeren. Bekijk de [volledige documentatie](https://aka.ms/amlps) voor deze en veel meer functies.

- Inrichten van een nieuwe werkruimte met behulp van een certificaat ([Nieuw AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
- Exporteren en importeren van een JSON-bestand van een grafiek van het experiment ([Export AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) en [Import-AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
- Uitvoeren van een experiment ([Start AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
- Een webservice van een anticiperende experiment ([Nieuw AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice)) maken
- Maak een nieuw eindpunt op een gepubliceerde webservice ([Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
- Roep een Bronrecords en/of BES web service-eindpunt ([Roep AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) en [Invoke-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Hier is een snelle voorbeeld van PowerShell gebruiken voor het uitvoeren van een bestaande experiment:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Zie dit artikel op de PowerShell-module gebruiken voor het automatiseren van een taak erg aangevraagd voor een uitgebreidere gebruiksvoorbeeld: [maakt veel modellen van Machine Learning en web service-endpoints in een experiment met PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Hoe moet ik beginnen?

Om te beginnen met de computer leren PowerShell- [release pakket](https://github.com/hning86/azuremlps/releases) downloaden van GitHub en volg de [instructies voor de installatie](https://github.com/hning86/azuremlps/blob/master/README.md). U wilt deblokkeren dll-bestand gedownload/uitgepakt en vervolgens importeren in uw omgeving met PowerShell. De meeste van de cmdlets moet de ID van de werkruimte, de werkruimte Autorisatietoken en de Azure regio die de werkruimte in te geven. De eenvoudigste manier om deze is door middel van een standaardbestand van config.json die in de installatie-instructies in detail wordt behandeld. Natuurlijk kunt u ook de structuur van het git clone en lokaal met behulp van Visual Studio code wijzigen/compileren.

## <a name="next-steps"></a>Volgende stappen

De PowerShell-module, blijven worden verbeterd en uitgebreid tijdens deze proefperiode. In het oog houden op de [Cortana intelligentie en Machine Learning Blog](https://blogs.technet.microsoft.com/machinelearning/) voor meer informatie en nieuws.

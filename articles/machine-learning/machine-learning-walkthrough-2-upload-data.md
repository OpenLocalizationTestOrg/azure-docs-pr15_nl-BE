<properties
    pageTitle="Stap 2: Gegevens uploaden in een experiment Machine Learning | Microsoft Azure"
    description="Stap 2 van het ontwikkelen van een oplossing voor anticiperende scenario: Upload openbare gegevens opgeslagen in Azure Machine Learning Studio."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Scenario-stap 2: Bestaande gegevens uploaden naar een experiment Azure Machine Learning

Dit is de tweede stap van de procedure, het [ontwikkelen van een voorspellende analytics oplossing in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Een Machine Learning werkruimte maken](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  **Bestaande gegevens uploaden**
3.  [Maak een nieuw experiment](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Trainen en het evalueren van de modellen](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [De web-service implementeren](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Toegang tot de webservice.](machine-learning-walkthrough-6-access-web-service.md)

----------

Voor het ontwikkelen van een voorspellende model voor kredietrisico, moeten we de gegevens die u gebruiken kunt om te trainen en te testen of het model. Voor dit scenario gebruiken we de "ICB Statlog (Duitse Creditgegevens) gegevensset' uit de opslagplaats UCI Machine Learning. U kunt deze hier vinden:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://Archive.ICS.uci.edu/ml/DataSets/Statlog+(German+credit+Data)</a>

We gebruiken het bestand met de naam **german.data**. Download dit bestand op de lokale vaste schijf.  

Deze dataset bevat rijen 20 variabelen voor 1000 asielzoekers afgelopen krediet. Deze 20 variabelen vertegenwoordigen van de dataset functie vector die identificerende kenmerken voor iedere aanvrager krediet biedt. Een extra kolom in elke rij vertegenwoordigt van de sollicitant berekende kredietrisico, met 700 sollicitanten geïdentificeerd als een laag kredietrisico en 300 als een hoog risico.

De ICB-website vindt u een beschrijving van de kenmerken van de functie vector, waaronder financiële informatie, krediet geschiedenis, werkgelegenheid, status en persoonlijke gegevens. Voor elke sollicitant een binaire beoordeling is gegeven die aangeeft of ze een lage zijn of hoge kredietrisico.  

Deze gegevens gebruiken we een model anticiperende analytische trainen. Als we klaar moet ons model mogelijk zijn informatie voor nieuwe gebruikers accepteren en of ze een lage of hoge kredietrisico zijn te voorspellen.  

Hier is een interessante twist. De beschrijving van de dataset wordt uitgelegd dat een persoon onjuiste een kredietrisico wanneer ze echt hoog kredietrisico zijn 5 keer duurder aan de financiële instelling dan onjuiste een kredietrisico zo hoog is. Een eenvoudige manier rekening te houden deze bij ons experiment is door te dupliceren (5 keer) posten die iemand met een hoge kredietrisico vertegenwoordigen. Klik, als het model ten onrechte een hoge kredietrisico zo klein classificeert, het doet dat misclassification 5 keer eenmaal voor elke dubbele record. Hierdoor neemt de kosten van deze fout in de resultaten van de training.  

##<a name="convert-the-dataset-format"></a>De dataset-indeling converteren
De oorspronkelijke gegevensset wordt gebruikt een blanco gescheiden. Machine Learning Studio werkt beter met een bestand met door komma's gescheiden waarden (CSV), zodat we de dataset converteren moet door spaties te vervangen door komma's.  

Er zijn veel manieren om deze gegevens te converteren. Eén manier is met behulp van de volgende Windows PowerShell-opdracht:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Een andere manier is met behulp van de opdracht van de sed Unix:  

    sed 's/ /,/g' german.data > german.csv  

In beide gevallen hebben wij een door komma's gescheiden versie van de gegevens in een bestand met de naam **german.csv** die we in ons experiment gebruiken.

##<a name="upload-the-dataset-to-machine-learning-studio"></a>De dataset uploaden naar Machine Learning Studio

Zodra de gegevens zijn geconverteerd naar een CSV-indeling, moeten we deze uploaden naar Machine Learning Studio. Zie voor meer informatie over aan de slag met de computer leren Studio [Microsoft Azure Machine Learning Studio Home](https://studio.azureml.net/).

1.  Open computer leren Studio ([https://studio.azureml.net](https://studio.azureml.net)). Wanneer u wordt gevraagd aan te melden, gebruikt u de account die u hebt opgegeven als de eigenaar van de werkruimte.
1.  Klik op het tabblad **Studio** aan de bovenkant van het venster.
1.  Klik op **+ Nieuw** onder aan het venster.
1.  Selecteer de **GEGEVENSSET**.
1.  Selecteer **lokaal bestand FROM**.
1.  Klik op **Bladeren** en zoek het bestand **german.csv** dat u hebt gemaakt in het dialoogvenster **een nieuwe gegevensset te uploaden** .
1.  Voer een naam voor de gegevensset. Voor dit scenario bellen wij het "UCI Duits creditcardgegevens".
1.  Voor het gegevenstype, selecteert u **algemene CSV-bestand met geen koptekst (. nh.csv)**.
1.  Een beschrijving toevoegen als u wilt.
1.  Klik op **OK**.  

![De dataset laden][1]  


Hiermee uploadt u de gegevens in een dataset-module die u in een experiment gebruiken kunt.

> [AZURE.TIP] Klik op het tabblad **DATASETS** aan de linkerkant van het venster Studio voor het beheren van datasets die je hebt geüpload naar de Studio.

Zie [uw trainingsgegevens naar Azure Machine Learning Studio importeren](machine-learning-data-science-import-data.md)voor meer informatie over het importeren van verschillende typen gegevens in een experiment.

**Volgende: [een nieuw experiment maken](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: ./media/machine-learning-walkthrough-2-upload-data/upload1.png

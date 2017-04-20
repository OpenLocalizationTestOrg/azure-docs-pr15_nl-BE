<properties
    pageTitle="Gegevens importeren in een Machine Learning Studio van een lokaal bestand | Microsoft Azure"
    description="Het importeren van uw trainingsgegevens Azure Machine Learning Studio van een lokaal bestand."
    keywords="importeren van gegevens, gegevens opmaken, gegevenstypen, gegevensbronnen, trainingsgegevens"
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
    ms.date="09/16/2016"
    ms.author="garye;bradsev" />


# <a name="import-your-training-data-into-azure-machine-learning-studio-from-a-local-file"></a>De trainingsgegevens van uw in Azure Machine Learning Studio van een lokaal bestand importeren

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]


Als u uw eigen gegevens in de Machine Learning Studio, kunt u een gegevensbestand tevoren uploaden van uw lokale vaste schijf voor het maken van een dataset-module in uw werkruimte. 


## <a name="import-data-from-a-local-file"></a>Gegevens importeren uit een lokaal bestand

U kunt gegevens importeren vanaf een lokale vaste schijf als volgt:

1. Klik op **+ Nieuw** onder aan het venster Machine Learning Studio.
2. **DATASET** en **van lokaal bestand**selecteren.
3. Blader in het dialoogvenster **een nieuwe gegevensset uploaden** naar het bestand dat u wilt uploaden
4. Voer een naam, identificatie van het gegevenstype en eventueel een omschrijving invoeren. Een beschrijving is aanbevolen - u kunt alle kenmerken van de gegevens die u onthouden wilt wanneer u de gegevens in de toekomst vastleggen.
5. Het selectievakje **Dit is de nieuwe versie van een bestaande dataset** kunt u een bestaande gegevensset bijwerken met nieuwe gegevens. Klik op dit selectievakje in en voer vervolgens de naam van een bestaande dataset.

U ziet een bericht dat het bestand wordt geladen tijdens het uploaden. Uploaden tijd afhankelijk van de grootte van uw gegevens en de snelheid van uw verbinding met de service.
Als u dat het bestand duurt lang, kunt u andere dingen binnen Machine Learning Studio doen terwijl u wacht. Echter, de browser te sluiten, wordt het uploaden van gegevens niet.

Als uw gegevens is geüpload, wordt opgeslagen in een dataset-module en is beschikbaar voor elk experiment in uw werkruimte.
Wanneer u een experiment bewerkt, kunt u vinden op de gegevenssets die u hebt gemaakt in de lijst **Mijn Datasets** onder de lijst met **Opgeslagen Datasets** in het palet van de module. U kunt slepen en neerzetten op het tekenpapier experiment dataset als u wilt de gegevens voor de verdere analyses en machine leren gebruiken.




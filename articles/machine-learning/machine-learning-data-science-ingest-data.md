<properties 
    pageTitle="Gegevens laden in omgevingen met opslag voor analytics | Microsoft Azure" 
    description="Gegevens verplaatsen en naar Azure Blob-opslag" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="bradsev" />

# <a name="load-data-into-storage-environments-for-analytics"></a>Gegevens laden in omgevingen met opslag voor analytics

Het Team gegevens wetenschap proces vereist dat de gegevens worden ingenomen of geladen in diverse omgevingen met verschillende opslag worden verwerkt of in de meest geschikte manier geanalyseerd in elke fase van het proces. Meestal gebruikt voor de verwerking van gegevensbestemmingen zijn Azure blobopslag, Azure SQL databases, SQL Server op een VM in Azure, HDInsight (Hadoop) en Azure Machine Learning. 

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Dit **menu** bevat koppelingen naar onderwerpen waarin wordt beschreven hoe u de consumptie van gegevens in deze omgevingen doel waar de gegevens worden opgeslagen en verwerkt.

Technische en zakelijke behoeften, alsmede de locatie, opmaken en de grootte van uw gegevens bepaalt de doel-omgevingen waarin de gegevens worden ingenomen moeten om de verwezenlijking van de doelstellingen van de analyse. Het is niet ongebruikelijk dat een scenario om gegevens te verplaatsen tussen verschillende omgevingen te bereiken van de verschillende taken die nodig zijn om een voorspellende model samen te stellen. Deze reeks taken kan bijvoorbeeld zijn verkennen, voorbehandeling, reinigen, down-sampling en model training.

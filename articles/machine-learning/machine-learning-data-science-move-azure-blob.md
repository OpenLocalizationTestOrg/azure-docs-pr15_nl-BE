<properties
    pageTitle="Gegevens verplaatsen en naar Azure Blob-opslag | Microsoft Azure"
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
    ms.date="09/14/2016"
    ms.author="bradsev;sachouks" />

# <a name="move-data-to-and-from-azure-blob-storage"></a>Gegevens verplaatsen en naar Azure Blob-opslag

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Advies over technologieën die worden gebruikt om gegevens te verplaatsen naar of uit de Azure Blob-opslag hier zijn gekoppeld:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]
 
Welke methode u het beste is, hangt af van uw scenario. De [scenario's voor geavanceerde analytics in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md) artikel helpt u bij het bepalen van de bronnen die u nodig hebt om allerlei gegevens wetenschap werkstromen gebruikt tijdens de geavanceerde analytics.

> [AZURE.NOTE] Verwijzen naar [Azure Blob grondbeginselen](../storage/storage-dotnet-how-to-use-blobs.md) en [Azure Blob](https://msdn.microsoft.com/library/azure/dd179376.aspx)-service voor een volledige inleiding op Azure blob-opslag.

Als alternatief kunt u [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) te gebruiken: 

- maken en plannen van een pijpleiding die u gegevens via Azure blobopslag downloadt 
- doorgegeven aan de webservice van een gepubliceerde Azure Machine Learning 
- ontvangt de anticiperende analytische resultaten, en 
- uploaden van de resultaten naar de opslag. 

Zie voor meer informatie [de voorspellende pijpleidingen maken met Azure Data Factory en Azure Machine Learning](../data-factory/data-factory-azure-ml-batch-execution-activity.md).

## <a name="prerequisites"></a>Vereisten

Dit document wordt ervan uitgegaan dat er een Azure-abonnement, een opslag-account en de bijbehorende sleutel opslag voor die account. Voor het uploaden/downloaden van gegevens, moet u uw account Azure opslag de sleutel de naam en account weten.

- Zie instellen van een Azure-abonnement, [gratis proefperiode van een maand](https://azure.microsoft.com/pricing/free-trial/).
- Zie voor instructies over het maken van een account voor opslag en voor rekening en belangrijke informatie ophalen [over Azure opslag rekeningen](../storage/storage-create-storage-account.md).

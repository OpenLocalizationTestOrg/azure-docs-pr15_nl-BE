<properties
    pageTitle="Gegevens verplaatsen of naar Azure Blob-opslag met behulp van SSIS connectoren | Microsoft Azure"
    description="Gegevens verplaatsen of naar Azure Blob-opslag met behulp van SSIS connectoren."
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
    ms.author="bradsev" />

# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Gegevens verplaatsen of naar Azure Blob-opslag met behulp van SSIS connectoren

De [SQL Server Integration Services Feature Pack voor Azure](https://msdn.microsoft.com/library/mt146770.aspx) biedt onderdelen Azure, verbinding maken met de overdracht van gegevens tussen Azure en gegevensbronnen op gebouwen en gegevens verwerken in Azure.

Advies over technologieën die worden gebruikt om gegevens te verplaatsen naar of uit de Azure Blob-opslag hier zijn gekoppeld:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


Klanten hebben op ruimten gegevens verplaatst naar de cloud, kunnen ze openen vanuit elke Azure service om te profiteren van alle mogelijkheden van de suite van Azure technologieën. Het kan worden gebruikt, bijvoorbeeld in Azure Machine Learning of op een HDInsight-cluster.

Dit is meestal de eerste stap moeten zijn voor de scenario's voor [SQL](machine-learning-data-science-process-sql-walkthrough.md) - en [HDInsight](machine-learning-data-science-process-hive-walkthrough.md) .

Zie voor een bespreking van de canonieke scenario's met SSIS bedrijfsbehoeften gemeenschappelijk in scenario's voor integratie van hybride-gegevens uitvoeren, [meer doen tegen SQL Server Integration Services Feature Pack voor Azure](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) blog.

> [AZURE.NOTE] Verwijzen naar [Azure Blob grondbeginselen](../storage/storage-dotnet-how-to-use-blobs.md) en [Azure Blob](https://msdn.microsoft.com/library/azure/dd179376.aspx)-service voor een volledige inleiding op Azure blob-opslag.

## <a name="prerequisites"></a>Vereisten

Voor het uitvoeren van de taken die in dit artikel wordt beschreven, hebt u een abonnement op Azure en Azure opslag account instellen. U moet weten dat uw opslag Azure account naam en account sleutel uploaden of downloaden van gegevens.

- Zie instellen van een **abonnement op Azure**, [gratis proefperiode van een maand](https://azure.microsoft.com/pricing/free-trial/).

- Zie voor instructies over het maken van een **account voor opslag** en voor het verkrijgen van account- en belangrijke informatie [over Azure opslag rekeningen](../storage/storage-create-storage-account.md).


U moet voor het gebruik van de **verbindingslijnen SSIS**downloaden:

- **SQL Server 2014 of 2016-standaard (of hoger)**: installatie omvat SQL Server Integration Services.
- **Microsoft SQL Server 2014 of 2016 Integration Services Feature Pack voor Azure**: deze kunnen worden gedownload, respectievelijk van de [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) en [SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492) -pagina's.

> [AZURE.NOTE] SSI's met SQL Server is geïnstalleerd, maar is niet opgenomen in de Express-versie. Zie voor informatie over welke toepassingen zijn opgenomen in de verschillende edities van SQL Server, [SQL Server-edities](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)

Zie voor cursusmateriaal op SSIS [Handen op Training voor SSIS](http://www.microsoft.com/download/details.aspx?id=20766)

Voor meer informatie over hoe u omhoog uitvoeren met behulp van SISS bouwen eenvoudige extractie, transformatie en laden (ETL) pakketten, Zie [SSIS zelfstudie: een eenvoudige ETL pakket](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>Taxi RDAM dataset downloaden  
In het voorbeeld beschreven gebruik hier een openbaar beschikbare dataset--de dataset [RDAM Taxi Trips](http://www.andresmh.com/nyctaxitrips/) . De dataset bestaat uit ongeveer 173 miljoen taxi ritten in RDAM is in het jaar 2013. Er zijn twee soorten gegevens: reis ritbedrag-gegevens en details. Als er een bestand voor elke maand is, hebben we 24 bestanden in alle, die elk ongeveer 2GB niet-gecomprimeerd is.


## <a name="upload-data-to-azure-blob-storage"></a>Gegevens uploaden naar Azure blob-opslag
Als u wilt verplaatsen van gegevens met behulp van de SSIS functiepakket op ruimten naar Azure blobopslag, gebruikt u een exemplaar van de [**Azure Blob uploaden taak**](https://msdn.microsoft.com/library/mt146776.aspx), die hier worden weergegeven:

![configureren-gegevens-wetenschap-vm](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)


Hier worden de parameters die de taak wordt beschreven:


Veld|Beschrijving|
----------------------|----------------|
**AzureStorageConnection**|Hiermee geeft u een bestaande Azure opslag Connection Manager of maakt u een nieuwe naam die naar een account Azure opslag die verwijst verwijst naar waar de blob-bestanden worden gehost.|
**BlobContainer**|Hiermee geeft u de naam van de blob-container waarin de geüploade bestanden als BLOB's.|
**BlobDirectory**|Hiermee geeft u de blob-map waarin het geüploade bestand wordt opgeslagen als een blob blokkeren. De blob is een virtuele hiërarchische structuur. Als de blob al bestaat, it ia vervangen.|
**LocalDirectory**|Hiermee geeft u de lokale map waarin de bestanden te uploaden.|
**Bestandsnaam**|Geeft een filter naam als bestanden met de opgegeven naampatroon wilt selecteren. Bijvoorbeeld: MySheet\*.xls\* behoren onder andere MySheet001.xls en MySheetABC.xlsx|
**TimeRangeFrom/TimeRangeTo**|Hiermee geeft u een filter tijdsbereik. Bestanden gewijzigd na *TimeRangeFrom* en vóór *TimeRangeTo* zijn opgenomen.|


> [AZURE.NOTE] De referenties van de **AzureStorageConnection** moeten correct en de **BlobContainer** moet bestaan voordat de overdracht wordt uitgevoerd.

## <a name="download-data-from-azure-blob-storage"></a>Gegevens downloaden vanaf Azure blob-opslag

Om te downloaden gegevens van lokale opslag met SSIS Azure blobopslag, gebruikt u een exemplaar van de [Taak van Azure Blob-uploaden](https://msdn.microsoft.com/library/mt146779.aspx).

##<a name="more-advanced-ssis-azure-scenarios"></a>Meer geavanceerde SSIS Azure-scenario 's
Wij weten, dat het SSIS Featurepack voor meer complexe stromen kunnen worden verwerkt door verpakking taken samen kan. Bijvoorbeeld kan de blob-gegevens invoer rechtstreeks in een cluster van HDInsight, waarvan de output kan worden gedownload naar een blob en vervolgens naar de lokale opslag. SSIS component en varken taken kan worden uitgevoerd op een HDInsight-cluster met behulp van extra SSIS connectoren:

- U kunt een component script uitvoeren op een cluster Azure HDInsight met SSIS [Azure HDInsight component taak](https://msdn.microsoft.com/library/mt146771.aspx)te gebruiken.
- U kunt een varken script uitvoeren op een cluster Azure HDInsight met SSIS [Azure HDInsight varken taak](https://msdn.microsoft.com/library/mt146781.aspx)te gebruiken.

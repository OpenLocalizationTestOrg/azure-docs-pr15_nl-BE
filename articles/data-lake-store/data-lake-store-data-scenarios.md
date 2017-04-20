<properties 
   pageTitle="Scenario's voor gegevens met betrekking tot de gegevensopslag Lake | Microsoft Azure" 
   description="Inzicht in de verschillende scenario's en hulpprogramma's gebruikt die gegevens kunnen ingenomen, verwerkt, gedownload en weergegeven in een gegevensarchief Lake" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Met behulp van Azure Lake gegevensarchief voor grote gegevensvereisten

Er zijn vier belangrijke fasen in grote gegevensverwerking:

* Ingesting grote hoeveelheden gegevens in een gegevensarchief in real-time of in batches
* Verwerken van de gegevens
* Downloaden van de gegevens
* De gegevens visualiseren

In dit artikel hebben bekijken we deze fasen van Azure gegevensarchief Lake te begrijpen van de opties en hulpprogramma's die beschikbaar zijn op de behoeften van uw big data.

## <a name="ingest-data-into-data-lake-store"></a>Consumptie van gegevens in het gegevensarchief Lake

In deze sectie worden de verschillende bronnen van de gegevens en de verschillende manieren waarop die gegevens een gegevensarchief Lake rekening mag worden ingenomen.

![Gegevens in het gegevensarchief Lake Ingest] (./media/data-lake-store-data-scenarios/ingest-data.png "Gegevens in het gegevensarchief Lake Ingest")

### <a name="ad-hoc-data"></a>Ad hoc-gegevens

Dit staat voor kleinere gegevenssets die worden gebruikt voor het maken van prototypen een grote toepassing. Er zijn verschillende manieren van ingesting ad hoc-gegevens afhankelijk van de bron van de gegevens.

| Gegevensbron        | Met behulp van consumptie                                                                        |
|--------------------|----------------------------------------------------------------------------------------|
| Lokale computer     | <ul> <li>[Azure Portal](/data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure platforms CLI](data-lake-store-get-started-cli.md)</li> <li>[Met de gegevens Lake's voor Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md#upload-source-data-files) </li></ul> |
| Opslag van Azure Blob | <ul> <li>[Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)</li> <li>[AdlCopy gereedschap](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp op HDInsight-cluster](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

 
### <a name="streamed-data"></a>Gestroomde gegevens

Hiermee worden gegevens die kunnen worden gegenereerd door verschillende bronnen, zoals toepassingen, apparaten, sensoren, enz. Deze gegevens kan door verschillende hulpprogramma's in een gegevensarchief Lake worden ingenomen. Deze hulpprogramma's worden gewoonlijk vastleggen en verwerken van de gegevens op basis van door de gebeurtenis in real-time, en schrijf vervolgens de gebeurtenissen in batches in Lake gegevensarchief zodat ze verder kunnen worden verwerkt. 

Hieronder vindt u hulpprogramma's die u kunt gebruiken:
 
* [Azure Stream Analytics] (.. (/ stream-analytics-gegevens-lake-output) - gebeurtenissen ingenomen in gebeurtenis Hubs kunnen worden beschreven Azure gegevens Lake met een output Azure Lake gegevensarchief.
* [Azure HDInsight Storm](../hdinsight/hdinsight-storm-write-data-lake-store.md) - kunt u gegevens rechtstreeks naar Lake gegevensopslag van het cluster Storm.
* [EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost) – u kunt gebeurtenissen ontvangen van gebeurtenis Hubs en vervolgens schrijven naar Lake gegevensopslag met de [Data Lake winkel .NET SDK](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>Relationele gegevens

U kunt ook brongegevens uit relationele databases. Relationele databases verzamelen gedurende een periode van tijd, enorme hoeveelheden gegevens die belangrijke inzichten bieden kan als via een pijpleiding grote gegevens verwerkt. U kunt de volgende hulpprogramma's die gegevens naar een gegevensarchief Lake.

* [Apache-Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Web server log data (uploaden met behulp van aangepaste toepassingen)

Dit type dataset is specifiek genoemd omdat de analyse van de web server log-gegevens is een algemene use-case voor big data toepassingen en grote hoeveelheden bestanden te uploaden naar de gegevensopslag Lake vereist. Kunt u een van de volgende hulpprogramma's voor het schrijven van uw eigen scripts of toepassingen voor het uploaden van dergelijke gegevens.

* [Azure platforms CLI](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Lake gegevensarchief .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

Voor het uploaden van gegevens van web server log en ook voor het uploaden van andere soorten gegevens (bv. Sociaal teksten), is een goede aanpak voor het schrijven van uw eigen aangepaste scripts/toepassingen omdat u de flexibiliteit om uw gegevens onderdeel uploaden als onderdeel van uw toepassing met grotere grote gegevens bevat. In sommige gevallen deze code kan de vorm aannemen van een script of een eenvoudige opdrachtregelprogramma. In andere gevallen kan de code worden gebruikt om grote gegevensverwerking te integreren in een zakelijke toepassing of een oplossing.


### <a name="data-associated-with-azure-hdinsight-clusters"></a>Gegevens met betrekking tot clusters Azure HDInsight

De meeste ondersteund HDInsight cluster (Hadoop, HBase, Storm) Lake gegevensarchief als een opslagplaats van gegevens opslag. HDInsight clusters toegang tot gegevens Azure opslag BLOB's (WASB). Voor betere prestaties kunt u de gegevens van de WASB in een gegevensarchief Lake-account die is gekoppeld aan het cluster. U kunt de volgende hulpprogramma's om de gegevens te kopiëren.

* [Apache-DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy Service](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)

### <a name="data-stored-in-on-premise-or-iaas-hadoop-clusters"></a>Gegevens die zijn opgeslagen in lokale of IaaS Hadoop clusters

Grote hoeveelheden gegevens kunnen worden opgeslagen in een bestaande clusters Hadoop, lokaal op machines met HDFS. Hadoop clusters mogelijk in een lokale installatie of mogelijk binnen een cluster IaaS op Azure. Er zijn eisen deze gegevens kopiëren naar Azure Lake gegevensarchief voor een eenmalige of terugkerende gereageerd. Er zijn verschillende opties die u hiervoor kunt gebruiken. Hieronder vindt u een lijst met alternatieven en de bijbehorende e-mailverkeer.

| Benadering  | Details | Voordelen   | Overwegingen met betrekking tot  |
|-----------|---------|--------------|-----------------|
| Azure Factory (ADF) gebruikt om gegevens te kopiëren van Hadoop clusters rechtstreeks naar Azure Lake gegevensopslag | [ADF ondersteunt HDFS als gegevensbron](../data-factory/data-factory-hdfs-connector.md) | ADF biedt out-of-the-box ondersteuning voor HDFS eerste klas end-to-end beheer en controle | Data Management Gateway te implementeren op de lokale of het cluster IaaS vereist |
| Gegevens exporteren uit Hadoop als bestanden. Kopieer de bestanden vervolgens naar Azure Lake gegevensopslag met passende mechanisme.                                   | U kunt bestanden kopiëren naar Azure Lake gegevensopslag gebruikt: <ul><li>[Azure PowerShell voor Windows OS](data-lake-store-get-started-powershell.md)</li><li>[Azure platforms CLI voor Windows OS](data-lake-store-get-started-cli.md)</li><li>Aangepaste app met alle gegevens Lake winkel SDK</li></ul> | Snel aan de slag. Uploads van aangepaste kunt doen                                                   | Meerdere stappen proces dat betrekking heeft op meerdere technologieën. Beheer en bewaking zal toenemen tot een uitdaging zijn na verloop van tijd, gezien het aangepaste karakter van de hulpprogramma 's |
| Distcp gebruiken om gegevens te kopiëren van Hadoop naar Azure opslag. Gegevens kopiëren uit de opslag van Azure naar Lake gegevensopslag met passende mechanisme. | U kunt gegevens kopiëren uit de opslag van Azure Lake gegevensopslag gebruikt: <ul><li>[Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)</li><li>[AdlCopy gereedschap](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Apache-DistCp op HDInsight-clusters](data-lake-store-copy-data-wasb-distcp.md)</li></ul>| U kunt extra open source. | Meerdere stappen proces dat betrekking heeft op meerdere technologieën |

### <a name="really-large-datasets"></a>Grote datasets

Voor het uploaden van datasets die in verschillende terabytes variëren kan met behulp van de bovenstaande manieren soms zijn langzaam en kostbaar. In dergelijke gevallen kunt u de onderstaande opties.

* **Met behulp van Azure ExpressRoute**. Azure ExpressRoute kunt u het maken van persoonlijke verbindingen tussen datacenters Azure en infrastructuur op uw locatie. Dit biedt een betrouwbare optie voor de overdracht van grote hoeveelheden gegevens. Zie [ExpressRoute Azure-documentatie](../expressroute/expressroute-introduction.md)voor meer informatie.


* **'Offline' uploaden van gegevens**. Als met behulp van Azure ExpressRoute niet haalbaar om een bepaalde reden is, kunt u [Azure Import/Export service](../storage/storage-import-export-service.md) levert vaste-schijfstations met uw gegevens naar een datacenter Azure. De gegevens wordt eerst naar Azure Storage Blobs geüpload. Vervolgens kunt u [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store) of [AdlCopy gebruikt](data-lake-store-copy-data-azure-storage-blob.md) om gegevens te kopiëren van Azure Storage Blobs naar Lake gegevensopslag.

    >[AZURE.NOTE] Terwijl u met de Import/Export-service, de grootte van de schijven die u naar Azure Datacenter verzendt moet niet groter zijn dan 200 GB.


## <a name="process-data-stored-in-data-lake-store"></a>Verwerking van gegevens die zijn opgeslagen in het gegevensarchief Lake

Zodra de gegevens beschikbaar in het gegevensarchief Lake zijn kunt u analyse uitvoeren op gegevens met de ondersteunde big data-toepassingen. Momenteel kunt u HDInsight Azure en Azure gegevens Lake Analytics data analysis taken uitvoeren op de gegevens in het gegevensarchief Lake. 

![Analyseren van gegevens in het gegevensarchief Lake] (./media/data-lake-store-data-scenarios/analyze-data.png "Analyseren van gegevens in het gegevensarchief Lake")

U kunt de volgende voorbeelden bekijken.

* [Maak een cluster HDInsight met Lake gegevensarchief als opslag](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Azure gegevens Lake Analytics gebruiken met Lake gegevensarchief](../data-lake-analytics/data-lake-analytics-get-started-portal.md)



## <a name="download-data-from-data-lake-store"></a>Gegevens uit een gegevensarchief Lake downloaden

U kunt ook downloaden of verplaatsen van gegevens uit Azure Lake gegevensarchief voor scenario's, zoals:

* Gegevens verplaatsen naar andere opslagplaatsen aan interface met uw bestaande pijpleidingen voor gegevensverwerking. U wilt gegevens verplaatsen van Lake gegevensarchief naar Azure SQL-Database of SQL-Server op locatie.
* Gegevens downloaden naar uw lokale computer voor verwerking in omgevingen met IDE tijdens het maken van prototypen.

![Gegevens uit een gegevensarchief Lake egress] (./media/data-lake-store-data-scenarios/egress-data.png "Gegevens uit een gegevensarchief Lake egress")

In dergelijke gevallen kunt u een van de volgende opties:

* [Apache-Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)
* [Apache-DistCp](data-lake-store-copy-data-wasb-distcp.md)

U kunt ook de volgende methoden om te schrijven van uw eigen script/toepassing voor het downloaden van gegevens uit het gegevensarchief Lake.

* [Azure platforms CLI](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Lake gegevensarchief .NET SDK](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Gegevens in het gegevensarchief Lake

Een combinatie van services kunt u visuele weergaven van gegevens die zijn opgeslagen in Lake gegevensarchief maken.

![Gegevens visualiseren in Lake gegevensarchief] (./media/data-lake-store-data-scenarios/visualize-data.png "Gegevens visualiseren in Lake gegevensarchief")

* U kunt starten via [Azure Data Factory verplaatsen van gegevens uit Lake gegevensarchief naar Azure SQL Data Warehouse](../data-factory/data-factory-data-movement-activities.md#supported-data-stores)
* Daarna kunt u [integreren Power BI met Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) visuele weergave van de gegevens maken.

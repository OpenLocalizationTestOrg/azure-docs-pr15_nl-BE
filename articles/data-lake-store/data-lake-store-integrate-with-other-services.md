<properties
   pageTitle="Lake gegevensarchief integreren met andere Azure diensten | Microsoft Azure"
   description="Begrijpen hoe Lake gegevensarchief is geïntegreerd met andere Azure services"
   documentationCenter=""
   services="data-lake-store"
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="integrating-data-lake-store-with-other-azure-services"></a>Lake gegevensarchief integreren met andere Azure Services

Azure Lake gegevensopslag kan worden gebruikt in combinatie met andere Azure services om een breder scala van scenario's. Het volgende artikel geeft een overzicht van de services die Lake gegevensopslag kan worden geïntegreerd met.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Gebruik gegevensarchief Lake met Azure HDInsight

U kunt een cluster [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) Lake gegevensopslag als de HDFS-compatibele opslag gebruikt inrichten. Voor deze release, kunt Hadoop en Storm clusters voor Windows en Linux, u gegevensarchief Lake alleen als een extra opslagruimte. Dergelijke clusters worden Azure opslag (WASB) nog steeds gebruiken als de standaard opslag. Voor HBase serverclusters op Windows en Linux, kunt u echter Lake gegevensarchief gebruiken als de standaard-opslag of extra opslagruimte.

Zie voor instructies over het inrichten van een HDInsight-cluster met gegevensarchief Lake:

* [Een HDInsight cluster inrichten met Lake gegevensarchief met Azure Portal](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Een HDInsight cluster inrichten met Lake gegevensarchief met Azure PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)

**Voorkeur voor video's?** Volg de onderstaande koppelingen voor video's met instructies over het gebruik van gegevensopslag Lake HDInsight clusters.

* [Maak een cluster HDInsight met toegang tot de gegevensopslag Lake](https://mix.office.com/watch/l93xri2yhtp2)
* Zodra de cluster is ingesteld, [Access-gegevens in het gegevensarchief van meer component en varken scripts gebruiken](https://mix.office.com/watch/1n9g5w0fiqv1q)


## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Gebruik gegevensarchief Lake met Azure gegevens Lake Analytics

[Azure gegevens Lake Analytics](../data-lake-analytics/data-lake-analytics-overview.md) kunt u werken met grote gegevens op de schaal van de wolk. Dynamisch bronnen bepalingen en kunt u analytics op terabytes of zelfs exabytes van de gegevens die kunnen worden opgeslagen in een aantal ondersteunde gegevensbronnen een van hen Lake gegevensarchief doen. Gegevens Lake Analytics is speciaal geoptimaliseerd voor gebruik met Azure Lake gegevensarchief - biedt het hoogste niveau van prestaties, doorvoer- en parallelization voor u werkbelasting van big data.

Zie [Aan de slag met Data Lake Analytics met Lake gegevensarchief](../data-lake-analytics/data-lake-analytics-get-started-portal.md)voor instructies over het gebruik van gegevens Lake Analytics met Lake gegevensarchief.

**Voorkeur voor video's?** Volg de onderstaande koppelingen voor video's met instructies over het gebruik van gegevensopslag Lake HDInsight clusters.

* [Verbinding maken met gegevens van Azure Lake Analytics Azure Lake gegevensarchief](https://mix.office.com/watch/qwji0dc9rx9k)
* [Access Azure Lake gegevensarchief via Data Lake Analytics](https://mix.office.com/watch/1n0s45up381a8)


## <a name="use-data-lake-store-with-azure-data-factory"></a>Gebruik gegevensarchief Lake met Azure Data Factory

[Azure Data Factory](https://azure.microsoft.com/services/data-factory/) kunt u gegevens uit tabellen Azure, Azure SQL-Database, Azure SQL DataWarehouse, Azure opslag BLOB's en databases op ruimten te nemen. Wordt een eerste-klas burgers in het ecosysteem Azure, kan Azure Data Factory goedkeuringen door het innemen van gegevens uit deze bron naar Azure Lake gegevensopslag worden gebruikt.

Zie [verplaatsen van gegevens van en naar Lake gegevensarchief Data Factory gebruiken](../data-factory/data-factory-azure-datalake-connector.md)voor instructies over het gebruik van Azure Data Factory met Lake gegevensarchief.

**Video's opnieuw!** Zie [Gegevens Orchestration met Azure Data Factory voor Azure Lake gegevensarchief](https://mix.office.com/watch/1oa7le7t2u4ka). 

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Gegevens kopiëren van Azure Storage Blobs in Lake gegevensarchief

Azure Lake gegevensarchief bevat een opdrachtregelprogramma, AdlCopy, waarmee u gegevens kopiëren uit Azure Blob-opslag rekening Lake gegevensarchief. Zie [gegevens uit Azure Storage Blobs naar Lake gegevensopslag kopiëren](data-lake-store-copy-data-azure-storage-blob.md)voor meer informatie.

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Gegevens kopiëren tussen Azure SQL-Database en Lake gegevensarchief

Apache-Sqoop kunt u importeren en exporteren van gegevens tussen Azure SQL-Database en Lake gegevensarchief. Voor meer informatie Zie [kopiëren van gegevens tussen Lake gegevensopslag en Azure SQL-database met behulp van Sqoop](data-lake-store-data-transfer-sql-sqoop.md).

**Bekijk deze video** op [Apache Sqoop gebruiken om gegevens tussen relationele gegevensbronnen en Azure Lake gegevensopslag te verplaatsen](https://mix.office.com/watch/1butcdjxmu114).

## <a name="use-data-lake-store-with-stream-analytics"></a>Gebruik gegevensarchief Lake met Analytics Stream

U kunt Lake gegevensarchief als een van de uitgangen gestreamd met behulp van Azure Stream Analytics gegevens op te slaan. Zie voor meer informatie [stroomgegevens van Azure opslag Blob in Lake gegevensarchief Azure Stream Analytics](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-store-with-power-bi"></a>Gebruik gegevensarchief Lake met Power BI

Power BI kunt u gegevens importeren uit een gegevensarchief Lake-account om te analyseren en gegevens visualiseren. Zie [gegevens in het gegevensarchief Lake analyseren met behulp van Power BI](data-lake-store-power-bi.md)voor meer informatie.

## <a name="use-data-lake-store-with-data-catalog"></a>Gebruik gegevensarchief Lake met gegevenscatalogus

U kunt gegevens uit het gegevensarchief Lake registreren in de catalogus Azure gegevens zodat u de gegevens in de hele organisatie kan worden gevonden. Zie [gegevens uit het gegevensarchief van Lake in Azure catalogus met gegevens registreren](data-lake-store-with-data-catalog.md)voor meer informatie.


## <a name="see-also"></a>Zie ook

- [Overzicht van Azure Lake gegevensarchief](data-lake-store-overview.md)
- [Aan de slag met Lake gegevensarchief met Portal](data-lake-store-get-started-portal.md)
- [Aan de slag met Lake gegevensarchief met PowerShell](data-lake-store-get-started-powershell.md)  

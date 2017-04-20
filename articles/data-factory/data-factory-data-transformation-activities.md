<properties 
    pageTitle="Data Transformation: Transformatie & verwerken gegevens | Microsoft Azure" 
    description="Informatie over het omzetten van gegevens of procesgegevens in Azure Data Factory met Hadoop of Machine Learning Azure gegevens Lake Analytics." 
    keywords="gegevenstransformatie van, gegevens verwerken, gegevens, transformatie activiteit transformeren"
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/23/2016" 
    ms.author="shlo"/>

# <a name="transform-data-in-azure-data-factory"></a>Transformatie van gegevens in Azure Data Factory
> [AZURE.SELECTOR]
[Component](data-factory-hive-activity.md)  
[Varken](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Streaming Hadoop](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md)
[Gegevens Lake Analytics U SQL](data-factory-usql-activity.md)
[.NET aangepaste](data-factory-use-custom-activities.md)
   

## <a name="overview"></a>Overzicht 
In dit artikel wordt uitgelegd dat data transformation activiteiten in Azure Data Factory genoemd die u gebruiken kunt om te zetten en de onbewerkte gegevens in de voorspellingen en inzichten verwerkt. De activiteit van een transformatie wordt uitgevoerd in een computeromgeving zoals Azure HDInsight cluster of een Batch Azure. Deze koppelingen naar artikelen met gedetailleerde informatie over elke activiteit transformatie.
 
Data Factory ondersteunt de volgende data transformation activiteiten die kunnen worden toegevoegd aan [pijpleidingen](data-factory-create-pipelines.md) hetzij afzonderlijk of reeksen met een andere activiteit.

> [AZURE.NOTE] Voor een scenario met stapsgewijze instructies, Zie [maken van een pijplijn met component transformatie](data-factory-build-your-first-pipeline.md) artikel.  

## <a name="hdinsight-hive-activity"></a>Component HDInsight activiteit
De activiteit van de HDInsight-component in een pijpleiding Data Factory voert component query's op uw eigen of bellen op Windows, Linux-gebaseerde HDInsight cluster. Zie [Component activiteit](data-factory-hive-activity.md) -artikel voor meer informatie over deze activiteit. 

## <a name="hdinsight-pig-activity"></a>HDInsight varkens activiteit
De activiteit HDInsight varkens in een pijpleiding Data Factory voert varken query's op uw eigen of bellen op Windows, Linux-gebaseerde HDInsight cluster. Zie [Activiteit van varkens](data-factory-pig-activity.md) -artikel voor meer informatie over deze activiteit. 

## <a name="hdinsight-mapreduce-activity"></a>Activiteit HDInsight MapReduce
De activiteit HDInsight MapReduce in een pijpleiding Data Factory voert MapReduce programma's op uw eigen of bellen op Windows, Linux-gebaseerde HDInsight cluster. Zie [Activiteit MapReduce](data-factory-map-reduce.md) -artikel voor meer informatie over deze activiteit.

U kunt MapReduce activiteit Spark programma's uitvoert op uw cluster HDInsight Spark. Zie [programma's aanroepen Spark van Azure Data Factory](data-factory-spark.md) voor meer informatie.

## <a name="hdinsight-streaming-activity"></a>Activiteit HDInsight Streaming
De activiteit HDInsight Streaming in een pijplijn Data Factory voert Hadoop Streaming programma's op uw eigen of bellen op Windows, Linux-gebaseerde HDInsight cluster. Zie [activiteiten op Streaming HDInsight](data-factory-hadoop-streaming-activity.md) voor meer informatie over deze activiteit.

## <a name="machine-learning-activities"></a>Machine Learning-activiteiten
Azure Data Factory kunt u eenvoudig pijpleidingen die gebruikmaken van een gepubliceerde Azure Machine Learning webservice voor anticiperende analytics maken. Met de [Batchverwerking uitvoeren van activiteiten](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) in een pijpleiding Azure Data Factory, kunt u een webservice Machine leren om voorspellingen van de gegevens in de batch aanroepen.

Na verloop van tijd moeten de voorspellende modellen in de Machine Learning scoren experimenten met nieuwe invoer datasets worden retrained. Nadat u klaar bent met de bijscholing, wilt u de score webservice bijwerken met de retrained Machine Learning-model. De [Resource activiteit bijwerken](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) kunt u de webservice bijwerken met het nieuwe opgeleide model.  

[Gebruik Machine Learning activiteiten](data-factory-azure-ml-batch-execution-activity.md) Zie voor meer informatie over deze Machine Learning-activiteiten. 

## <a name="stored-procedure-activity"></a>Opgeslagen procedure activiteit
U kunt de opgeslagen Procedure van SQL Server-activiteit in een pijplijn Data Factory aanroepen van een opgeslagen procedure in een van de volgende opgeslagen gegevens: Azure SQL-Database, datawarehouse Azure SQL, SQL Server-Database in uw onderneming of een Azure VM. Zie [Activiteit van opgeslagen Procedure](data-factory-stored-proc-activity.md) -artikel voor meer informatie.  

## <a name="data-lake-analytics-u-sql-activity"></a>Gegevens Lake Analytics U SQL-activiteit
Gegevens Lake Analytics U SQL activiteit een U SQL-script op een cluster Azure gegevens Lake Analytics wordt uitgevoerd. Zie [Activiteit van Data Analytics U SQL](data-factory-usql-activity.md) -artikel voor meer informatie. 

## <a name="net-custom-activity"></a>Aangepaste activiteit .NET
Als u nodig hebt om gegevens op een manier die niet wordt ondersteund door Data Factory te transformeren, kunt u een aangepaste activiteit maken met uw eigen logica gegevensverwerking en gebruik van de activiteit in de pijplijn. U kunt de aangepaste .NET activiteit uit te voeren met behulp van een Batch Azure service of een Azure HDInsight cluster configureren. Zie [aangepaste activiteiten gebruik](data-factory-use-custom-activities.md) artikel voor meer informatie. 

U kunt een aangepaste activiteit R scripts uitvoeren op de cluster HDInsight met R geïnstalleerd. Zie [R Script uitvoeren met Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Omgevingen berekenen
U maakt een gekoppelde service voor het milieu berekenen en vervolgens de gekoppelde service gebruiken bij het definiëren van de activiteit van een transformatie. Er zijn twee soorten compute-omgevingen ondersteund door Data Factory. 

1. **Op verzoek**: In dit geval de computeromgeving volledig wordt beheerd door Data Factory. Deze wordt automatisch gemaakt door de service Data Factory voordat een taak is ingediend om gegevens te verwerken en verwijderd wanneer de taak is voltooid. U kunt configureren en beheren van gedetailleerde instellingen van de omgeving op verzoek compute voor taakuitvoering Clusterbeheer en acties uitvoeren van de bootstrap. 
2. **Breng uw eigen**: In dit geval kunt u uw eigen computeromgeving (bijvoorbeeld HDInsight, cluster) registreren als een gekoppelde service in Data Factory. De computeromgeving wordt beheerd door u en de Data Factory-service gebruikt voor het uitvoeren van de activiteiten. 

Zie [Gekoppelde Services Compute](data-factory-compute-linked-services.md) -artikel voor meer informatie over services die worden ondersteund door Data Factory berekenen. 


## <a name="summary"></a>Samenvatting
Azure Data Factory ondersteunt de volgende data transformation activiteiten en de compute-omgevingen voor de activiteiten. De activiteiten van de transformatie kunnen worden toegevoegd aan pijpleidingen hetzij afzonderlijk of geschakeld met een andere activiteit.

Data transformation activiteit |  Berekenen van de omgeving 
:----------------------- | :--------------------
[Component](data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Varken](data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop-Streaming](data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Machine Learning activiteiten: Batch worden uitgevoerd en een bron van Update](data-factory-azure-ml-batch-execution-activity.md) | Azure VM 
[Opgeslagen Procedure](data-factory-stored-proc-activity.md) | Azure SQL, SQL Azure datawarehouse of SQL Server |
[Data Analytics Lake U-SQL](data-factory-usql-activity.md) | Azure gegevens Lake Analytics 
[DotNet](data-factory-use-custom-activities.md) | HDInsight [Hadoop] of Azure Batch
   


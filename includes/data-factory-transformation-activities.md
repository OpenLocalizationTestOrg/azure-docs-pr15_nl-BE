Azure Data Factory ondersteunt de volgende transformatie activiteiten die kunnen worden toegevoegd aan pijpleidingen hetzij afzonderlijk of reeksen met een andere activiteit.

Data transformation activiteit |  Berekenen van de omgeving 
:----------------------- | :--------------------
[Component](../articles/data-factory/data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Varken](../articles/data-factory/data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](../articles/data-factory/data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop-Streaming](../articles/data-factory/data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Machine Learning activiteiten: Batch worden uitgevoerd en een bron van Update](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) | Azure VM 
[Opgeslagen Procedure](../articles/data-factory/data-factory-stored-proc-activity.md) | Azure SQL, SQL Azure datawarehouse of SQL Server |
[Data Analytics Lake U-SQL](../articles/data-factory/data-factory-usql-activity.md) | Azure gegevens Lake Analytics 
[DotNet](../articles/data-factory/data-factory-use-custom-activities.md) | HDInsight [Hadoop] of Azure Batch
   
> [AZURE.NOTE] 
> U kunt MapReduce activiteit Spark programma's uitvoert op uw cluster HDInsight Spark. Zie [programma's aanroepen Spark van Azure Data Factory](../articles/data-factory/data-factory-spark.md) voor meer informatie.
> U kunt een aangepaste activiteit R scripts uitvoeren op de cluster HDInsight met R geïnstalleerd. Zie [R Script uitvoeren met Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).
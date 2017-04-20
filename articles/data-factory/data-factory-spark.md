<properties 
    pageTitle="Starten programma's Spark van Azure Data Factory" 
    description="Informatie over het Spark-programma's in de fabriek van een Azure gegevens met behulp van de activiteit MapReduce aanroepen." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="spelluru"/>

# <a name="invoke-spark-programs-from-data-factory"></a>Starten programma's Spark van Data Factory
## <a name="introduction"></a>Inleiding
Kunt u de activiteit MapReduce in een pijplijn Data Factory Spark programma's uitvoert op uw cluster HDInsight Spark. Zie [MapReduce activiteit](data-factory-map-reduce.md) artikel voor gedetailleerde informatie over het gebruik van de activiteit voor het lezen van dit artikel. 

## <a name="spark-sample-on-github"></a>Monster motoren op GitHub
De [Spark - sample Data Factory op GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) laat zien hoe MapReduce activiteit aan te roepen van een vonk programma gebruiken. De spark NET gekopieerd gegevens uit één Azure Blob container naar de andere. 

## <a name="data-factory-entities"></a>Data Factory-entiteiten
De **Spark-ADF/src/ADFJsons** -map bevat bestanden voor Data Factory entiteiten (gekoppelde services, datasets, pijpleiding).  

In dit voorbeeld zijn twee **gekoppelde services** : Azure opslag en Azure HDInsight. Geef uw naam Azure opslag en waarden van de in **StorageLinkedService.json** en clusterUri, gebruikersnaam en wachtwoord in **HDInsightLinkedService.json**.

Er zijn twee **datasets** in dit voorbeeld: **input.json** en **output.json**. Deze bestanden bevinden zich in de map **Datasets** .  Deze bestanden staan datasets invoer en uitvoer voor de activiteit MapReduce

U vinden monster pijpleidingen in de map **ADFJsons/pijpleiding** . Bekijk een pijpleiding om te begrijpen hoe een vonk-programma met behulp van de activiteit MapReduce aanroepen. 

De activiteit MapReduce is geconfigureerd voor het aanroepen van **com.adf.sparklauncher.jar** in de container **adflibs** in Azure opslag (opgegeven in de StorageLinkedService.json). De broncode voor dit programma is in Spark-ADF/src/main/java/com/adf/map en roept spark indienen en Spark taken uitvoeren. 

> [AZURE.IMPORTANT] 
> [, Leesmij-bestand lezen. TXT](https://github.com/Azure/Azure-DataFactory/blob/master/Samples/Spark/README.txt) voor de meest recente en aanvullende informatie voordat u het voorbeeld. 
>  
> HDInsight Spark cluster gebruiken met deze aanpak Spark programma's met behulp van de activiteit MapReduce aanroepen. Met behulp van een HDInsight-cluster op verzoek wordt niet ondersteund.   


## <a name="see-also"></a>Zie ook
- [Activiteit van de component](data-factory-hive-activity.md)
- [Varken activiteit](data-factory-pig-activity.md)
- [MapReduce-activiteit](data-factory-map-reduce.md)
- [Hadoop Streaming activiteit](data-factory-hadoop-streaming-activity.md)
- [R-scripts starten](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

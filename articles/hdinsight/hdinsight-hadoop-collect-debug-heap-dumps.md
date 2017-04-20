<properties
    pageTitle="Fouten opsporen en analyseren van Hadoop services met heap dumps | Microsoft Azure"
    description="Automatisch heap dumps voor Hadoop services verzamelen en plaats binnen de Azure Blob storage account voor foutopsporing en -analyse."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>


# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a>Collect-heap wordt gedumpt in Blob-opslag fouten opsporen en analyseren van Hadoop-services

[AZURE.INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Heap-dumps bevatten een momentopname van de toepassing, geheugen, met inbegrip van de waarden van variabelen op het moment dat de dump is gemaakt. Ze zijn erg handig voor het oplossen van problemen die tijdens de uitvoering optreden. Heap-dumps kunnen automatisch worden verzameld voor Hadoop services en geplaatst in de Azure Blob storage-account van een gebruiker onder HDInsightHeapDumps /. 

De collectie van heap dumps voor verschillende services moet zijn ingeschakeld voor services op afzonderlijke clusters. De standaardinstelling voor deze functie wordt uitgeschakeld voor een cluster. Deze dumpbestanden heap zijn groot is, is het raadzaam de Blob storage-account controleren waar ze worden opgeslagen na inschakeling van de collectie.

> [AZURE.NOTE] De informatie in dit artikel is alleen van toepassing op Windows gebaseerde HDInsight. Zie voor informatie over Linux-gebaseerde HDInsight, [dumpen heap voor Hadoop services op Linux-gebaseerde HDInsight inschakelen](hdinsight-hadoop-collect-debug-heap-dump-linux.md)

## <a name="eligible-services-for-heap-dumps"></a>In aanmerking komende diensten voor heap dumpen

U kunt de heap dumps voor de volgende services inschakelen:

*  **hcatalog** - tempelton
*  **component** - hiveserver2, metastore, derbyserver
*  **mapreduce** - jobhistoryserver
*  **garens** - resourcemanager, nodemanager, timelineserver
*  **hdfs** - datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Configuratie-elementen waarmee de heap dumpen

Als u wilt dumpen heap voor een service, moet u de juiste configuratie-elementen instellen in de sectie voor die service die is opgegeven in **servicenaam**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

De waarde van de **servicenaam** kan een van de bovengenoemde services zijn: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, of namenode.

## <a name="enable-using-azure-powershell"></a>Met Azure PowerShell inschakelen

Bijvoorbeeld, als u heap dumpen via Azure PowerShell voor jobhistoryserver, doet u het volgende:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Inschakelen met behulp van .NET SDK

Bijvoorbeeld, als u heap dumpen door de Azure HDInsight .NET SDK voor jobhistoryserver, doet u het volgende:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));

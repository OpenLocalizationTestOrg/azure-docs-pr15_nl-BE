<properties
   pageTitle="Hadoop-component gebruiken met PowerShell in HDInsight | Microsoft Azure"
   description="Gebruik PowerShell Hadoop component query's uitgevoerd op HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-using-powershell"></a>Met PowerShell component-query's uitvoeren

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Dit document bevat een voorbeeld van het gebruik van Azure PowerShell in de resourcegroep Azure modus component query's worden uitgevoerd in een Hadoop op HDInsight cluster.

> [AZURE.NOTE] Dit document geeft een gedetailleerde beschrijving van het doen van de overzichten van de HiveQL die in de voorbeelden worden gebruikt. Zie voor informatie over de HiveQL die in dit voorbeeld wordt gebruikt, [Gebruik component met Hadoop op HDInsight](hdinsight-use-hive.md).


**Vereisten**

Als u de stappen in dit artikel hebt uitgevoerd, moet u het volgende.

- **Een cluster Azure HDInsight (Hadoop op HDInsight) (Windows- of Linux-gebaseerde)**
- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a name="run-hive-queries-using-azure-powershell"></a>Component-query's met Azure PowerShell uitvoeren

Azure PowerShell biedt *cmdlets* waarmee u op afstand uitvoeren van query's component op HDInsight. Dit gebeurt intern via de overige aanroepen van [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (voorheen genaamd Templeton) op de cluster HDInsight.

De volgende cmdlets worden gebruikt bij het uitvoeren van query's component in een cluster met externe HDInsight:

* **Toevoegen-AzureRmAccount**: Azure PowerShell verifieert uw abonnement Azure

* **Nieuw AzureRmHDInsightHiveJobDefinition**: een nieuwe *taakdefinitie* maakt met behulp van de opgegeven instructies voor HiveQL

* **Start AzureRmHDInsightJob**: Hiermee verzendt u de taakdefinitie naar HDInsight, wordt de taak gestart en geeft als resultaat een *Project* -object dat kan worden gebruikt voor het controleren van de status van de taak

* **Wachten op AzureRmHDInsightJob**: het taakobject gebruikt om te controleren van de status van de taak. Deze wacht totdat de taak is voltooid of de wachttijd wordt overschreden.

* **Get-AzureRmHDInsightJobOutput**: gebruikt voor het ophalen van de uitvoer van de taak

* **Invoke-AzureRmHDInsightHiveJob**: gebruikt voor het uitvoeren van HiveQL-instructies. Hiermee blokkeert u de query is voltooid en geeft vervolgens de resultaten

* **Gebruik AzureRmHDInsightCluster**: Hiermee stelt u het huidige cluster moet worden gebruikt voor de opdracht **Invoke-AzureRmHDInsightHiveJob**

De volgende stappen geven aan het gebruik van deze cmdlets voor het uitvoeren van een taak in het cluster HDInsight:

1. Met een editor, sla de volgende code op als **hivejob.ps1**. U moet de **CLUSTERNAAM** vervangen door de naam van het cluster HDInsight.

        #Specify the values
        $clusterName = "CLUSTERNAME"
        $creds=Get-Credential

        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureRmAccount
        }

        #HiveQL
        #Note: set hive.execution.engine=tez; is not required for
        #      Linux-based HDInsight
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"

        #Create an HDInsight Hive job definition
        $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString 

        #Submit the job to the cluster
        Write-Host "Start the Hive job..." -ForegroundColor Green

        $hiveJob = Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $creds

        #Wait for the Hive job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $creds

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Print the output
        Write-Host "Display the standard output..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $hiveJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Open een nieuwe **Azure PowerShell** -prompt. Mappen naar de locatie van het bestand **hivejob.ps1** wijzigen en vervolgens het script uitvoeren met de volgende opdracht:

        .\hivejob.ps1

    Wanneer het script wordt uitgevoerd, wordt u gevraagd referenties op te geven de HTTPS/Admin-account voor uw cluster. U kunt ook aanmelden bij uw abonnement Azure gevraagd.
    
7. Wanneer de taak is voltooid, moet deze informatie de volgende retourneren:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Zoals eerder vermeld, kan een query uitvoeren en wachten op het antwoord **Invoke-component** worden gebruikt. Gebruik de volgende opdrachten en **CLUSTERNAAM** vervangen door de naam van het cluster:

        Use-AzureRmHDInsightCluster -ClusterName $clusterName -HttpCredential $creds
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
        Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query $queryString

    De uitvoer ziet er als volgt uit:

        2012-02-03  18:35:34    SampleClass0    [ERROR] incorrect   id
        2012-02-03  18:55:54    SampleClass1    [ERROR] incorrect   id
        2012-02-03  19:25:27    SampleClass4    [ERROR] incorrect   id

    > [AZURE.NOTE] Voor meer HiveQL's, kunt u de cmdlet Azure PowerShell **Hier tekenreeksen** of scriptbestanden van HiveQL. In het volgende fragment laat zien hoe de cmdlet **Invoke-component** gebruiken voor het uitvoeren van een scriptbestand HiveQL. De HiveQL script moet worden geüpload naar wasbs: / /.
    >
    > `Invoke-AzureRmHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
    >
    > Zie voor meer informatie over **Hier tekenreeksen** <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Met behulp van Windows PowerShell hier-tekenreeksen</a>.

##<a name="troubleshooting"></a>Het oplossen van problemen

Als er geen informatie wordt geretourneerd wanneer de taak is voltooid, kan een fout opgetreden tijdens de verwerking. Om foutinformatie te bekijken voor deze taak, het volgende toevoegen aan het einde van het bestand **hivejob.ps1** en sla deze opnieuw uitvoeren.

    # Print the output of the Hive job.
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Hiermee wordt de informatie die wordt geschreven naar STDERR op de server wanneer u de taak hebt uitgevoerd en u kunt bepalen waarom de taak niet goed werkt.

##<a name="summary"></a>Samenvatting

U kunt zien, zorgt Azure PowerShell voor een eenvoudige manier om de component query's uitvoeren in een cluster van HDInsight, de taakstatus controleren en ophalen van de uitvoer.

##<a name="next-steps"></a>Volgende stappen

Voor algemene informatie over component in HDInsight:

* [Component gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Varken gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-pig.md)

* [MapReduce gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

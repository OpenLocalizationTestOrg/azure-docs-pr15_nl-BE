<properties
   pageTitle="MapReduce en PowerShell gebruiken met Hadoop | Microsoft Azure"
   description="Leren werken met PowerShell MapReduce taken op afstand uitvoeren met Hadoop op HDInsight."
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
   ms.date="08/29/2016"
   ms.author="larryfr"/>

# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>MapReduce-taken uitvoeren met Hadoop op HDInsight met PowerShell

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Dit document bevat een voorbeeld van het gebruik van Azure PowerShell een MapReduce-taak wordt uitgevoerd in een Hadoop cluster HDInsight.

##<a id="prereq"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, wilt nodig u het volgende:

- **Een cluster Azure HDInsight (Hadoop op HDInsight) (Windows- of Linux-gebaseerde)**

- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a id="powershell"></a>Voer een MapReduce taak met Azure PowerShell

Azure PowerShell biedt *cmdlets* waarmee u MapReduce taken op afstand uitvoeren op HDInsight. Dit gebeurt intern via de overige aanroepen van [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (voorheen genaamd Templeton) op de cluster HDInsight.

De volgende cmdlets worden gebruikt wanneer MapReduce taken in een extern HDInsight cluster uitgevoerd.

* **Aanmelding AzureRmAccount**: Azure PowerShell verifieert uw abonnement Azure

* **Nieuw AzureRmHDInsightMapReduceJobDefinition**: een nieuwe *taakdefinitie* maakt met behulp van de opgegeven MapReduce informatie

* **Start AzureRmHDInsightJob**: Hiermee verzendt u de taakdefinitie naar HDInsight, wordt de taak gestart en geeft als resultaat een *Project* -object dat kan worden gebruikt voor het controleren van de status van de taak

* **Wachten op AzureRmHDInsightJob**: het taakobject gebruikt om te controleren van de status van de taak. Wacht totdat de taak is voltooid of de wachttijd wordt overschreden.

* **Get-AzureRmHDInsightJobOutput**: gebruikt voor het ophalen van de uitvoer van de taak

De volgende stappen laten zien hoe het gebruik van deze cmdlets voor het uitvoeren van een taak in het cluster HDInsight.

1. Met een editor, sla de volgende code op als **mapreducejob.ps1**. U moet de **CLUSTERNAAM** vervangen door de naam van het cluster HDInsight.

        #Specify the values
        $clusterName = "CLUSTERNAME"
                
        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Login-AzureRmAccount
        }

        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        #Define the MapReduce job
        #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
        # -JarFile = the JAR containing the MapReduce application
        # -ClassName = the class of the application
        # -Arguments = The input file, and the output directory
        $wordCountJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
            -ClassName "wordcount" `
            -Arguments `
                "wasbs:///example/data/gutenberg/davinci.txt", `
                "wasbs:///example/data/WordCountOutput"

        #Submit the job to the cluster
        Write-Host "Start the MapReduce job..." -ForegroundColor Green
        $wordCountJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $wordCountJobDefinition `
            -HttpCredential $creds

        #Wait for the job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $wordCountJob.JobId `
            -HttpCredential $creds
        # Download the output
        Get-AzureStorageBlobContent `
            -Blob 'example/data/WordCountOutput/part-r-00000' `
            -Container $container `
            -Destination output.txt `
            -Context $context
        # Print the output
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Open een nieuwe **Azure PowerShell** -prompt. Mappen naar de locatie van het bestand **mapreducejob.ps1** wijzigen en vervolgens het script uitvoeren met de volgende opdracht:

        .\mapreducejob.ps1
    
    Wanneer u het script uitvoert, kan u gevraagd om uw abonnement op Azure te verifiëren. U wordt ook gevraagd om HTTPS/Admin accountnaam en wachtwoord voor de cluster HDInsight.

3. Wanneer de taak is voltooid, wordt uitvoer van de volgende strekking weergegeven:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Deze uitvoer geeft aan dat de taak is voltooid.

    > [AZURE.NOTE] Zie [Probleemoplossing](#troubleshooting)als de **afsluitcode** een andere waarde dan 0 wordt.

    In dit voorbeeld worden de gedownloade bestanden naar een bestand **uitvoer.txt** ook opslaan in de map waarin u het script uitvoert.

###<a name="view-output"></a>Uitvoer weergeven

Open het bestand **uitvoer.txt** in een teksteditor om te zien de woorden en de aantallen die worden geproduceerd door de taak.

> [AZURE.NOTE] De uitvoerbestanden van een taak MapReduce zijn onveranderbaar. Dus als u dit voorbeeld herhalen, moet u de naam van het uitvoerbestand te wijzigen.

##<a id="troubleshooting"></a>Het oplossen van problemen

Als er geen informatie wordt geretourneerd wanneer de taak is voltooid, kan een fout opgetreden tijdens de verwerking. Om foutinformatie te bekijken voor deze taak, met de volgende opdracht toevoegen aan het einde van het bestand **mapreducejob.ps1** en sla het opnieuw uitvoeren.

    # Print the output of the WordCount job.
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Hiermee wordt de informatie die is geschreven naar STDERR op de server wanneer u de taak hebt uitgevoerd en u kunt bepalen waarom de taak niet goed werkt.

##<a id="summary"></a>Samenvatting

Zoals u zien kunt, biedt Azure PowerShell gemakkelijk MapReduce taken uitvoeren op een cluster van HDInsight, de taakstatus controleren en ophalen van de uitvoer.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over MapReduce taken in HDInsight:

* [MapReduce op HDInsight Hadoop gebruiken](hdinsight-use-mapreduce.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-hive.md)

* [Varken gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-pig.md)

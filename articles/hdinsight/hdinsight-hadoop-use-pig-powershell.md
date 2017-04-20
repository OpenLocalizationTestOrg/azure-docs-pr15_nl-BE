<properties
   pageTitle="Gebruik van Hadoop varkens met PowerShell in HDInsight | Microsoft Azure"
   description="Informatie over het verzenden van taken naar een cluster van Hadoop op HDInsight met Azure PowerShell varken."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-powershell"></a>Met PowerShell varken-taken uitvoeren

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Dit document bevat een voorbeeld van het gebruik van Azure PowerShell taken kunnen verzenden varkens naar een Hadoop op HDInsight cluster. Varken kunt u taken MapReduce schrijven met behulp van een taal (varken Latijn), die gegevenstransformaties modellen, in plaats van toewijzen en functies te beperken.

> [AZURE.NOTE] Dit document geeft een gedetailleerde beschrijving van het doen van de varkens Latijns-instructies die in de voorbeelden worden gebruikt. Zie voor meer informatie over de varkens Latijnse gebruikt in dit voorbeeld [Gebruik varken met Hadoop op HDInsight](hdinsight-use-pig.md).

##<a id="prereq"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, moet u het volgende.

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a id="powershell"></a>Met PowerShell varken-taken uitvoeren

Azure PowerShell biedt *cmdlets* waarmee u varken taken op afstand uitvoeren op HDInsight. Dit gebeurt intern via de overige aanroepen van [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (voorheen genaamd Templeton) op de cluster HDInsight.

De volgende cmdlets worden gebruikt bij het uitvoeren van taken varkens op een cluster met externe HDInsight:

* **Aanmelding AzureRmAccount**: Azure PowerShell om uw abonnement op Azure worden geverifieerd

* **Nieuw AzureRmHDInsightPigJobDefinition**: een nieuwe *taakdefinitie* maakt met behulp van de opgegeven varken Latijns-instructies

* **Start AzureRmHDInsightJob**: Hiermee verzendt u de taakdefinitie naar HDInsight, wordt de taak gestart en geeft als resultaat een *Project* -object dat kan worden gebruikt voor het controleren van de status van de taak

* **Wachten op AzureRmHDInsightJob**: het taakobject gebruikt om te controleren van de status van de taak. Deze wacht totdat de afdruktaak is voltooid of de wachttijd is overschreden.

* **Get-AzureRmHDInsightJobOutput**: gebruikt voor het ophalen van de uitvoer van de taak

De volgende stappen laten zien hoe het gebruik van deze cmdlets voor het uitvoeren van een taak op het cluster HDInsight.

1. Met een editor, sla de volgende code op als **pigjob.ps1**. U moet de **CLUSTERNAAM** vervangen door de naam van het cluster HDInsight.

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get credentials for the admin/HTTPs account
        $creds = Get-Credential

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"
        
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
        $container = $clusterInfo.DefaultStorageContainer
        $storageAccountKey = (Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Store the Pig Latin into $QueryString
        $QueryString =  "LOGS = LOAD 'wasbs:///example/data/sample.log';" +
        "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
        "RESULT = order FREQUENCIES by COUNT desc;" +
        "DUMP RESULT;"


        #Create a new HDInsight Pig Job definition
        $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
            -Query $QueryString `
            -Arguments "-w"

        # Start the Pig job on the HDInsight cluster
        Write-Host "Start the Pig job ..." -ForegroundColor Green
        $pigJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $pigJobDefinition `
            -HttpCredential $creds

        # Wait for the Pig job to complete
        Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds

        # Display the output of the Pig job.
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds

2. Open een nieuwe Windows PowerShell-prompt. Mappen naar de locatie van het bestand **pigjob.ps1** wijzigen en vervolgens het script uitvoeren met de volgende opdracht:

        .\pigjob.ps1
        
    U wordt eerst gevraagd aanmelden bij uw abonnement Azure. U wordt vervolgens gevraagd voor de naam HTTPs/Admin en het wachtwoord voor de cluster HDInsight.

7. Wanneer de taak is voltooid, moet deze informatie de volgende retourneren:

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="troubleshooting"></a>Het oplossen van problemen

Als er geen informatie wordt geretourneerd wanneer de taak is voltooid, kan een fout opgetreden tijdens de verwerking. Om foutinformatie te bekijken voor deze taak, met de volgende opdracht toevoegen aan het einde van het bestand **pigjob.ps1** en sla het opnieuw uitvoeren.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Hiermee herstelt u de informatie die is geschreven naar STDERR op de server wanneer u de taak hebt uitgevoerd en u kunt bepalen waarom de taak niet goed werkt.

##<a id="summary"></a>Samenvatting

U kunt zien, zorgt Azure PowerShell voor een eenvoudige manier om varkens taken uitvoeren op een cluster van HDInsight, de taakstatus controleren en ophalen van de uitvoer.

##<a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over varkens in HDInsight:

* [Varken gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-pig.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight:

* [Component gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-hive.md)

* [MapReduce gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-mapreduce.md)

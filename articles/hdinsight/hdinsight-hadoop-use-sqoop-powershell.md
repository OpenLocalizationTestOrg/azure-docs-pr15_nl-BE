<properties
    pageTitle="Gebruik van Hadoop Sqoop in HDInsight | Microsoft Azure"
    description="Informatie over het Azure PowerShell gebruiken vanaf een werkstation uitvoeren Sqoop importeren en exporteren tussen een cluster Hadoop en Azure SQL-database."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="run-sqoop-jobs-using-azure-powershell-for-hadoop-in-hdinsight"></a>Azure PowerShell voor Hadoop in HDInsight met Sqoop-taken uitvoeren

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het Azure PowerShell gebruiken voor het uitvoeren van taken van Sqoop in HDInsight importeren en exporteren tussen de cluster HDInsight en Azure SQL-database of SQL Server-database.

> [AZURE.NOTE] De stappen in dit artikel kunnen worden gebruikt met of een Windows- of Linux HDInsight cluster. deze stappen werken echter alleen vanuit een Windows-client. Andere methoden taak indienen, klikt u op de knop voor tabselectie boven aan het artikel.


###<a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Cluster A Hadoop in HDInsight**. Zie [cluster maken en SQL-database](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

    
## <a name="run-sqoop-using-powershell"></a>Sqoop met PowerShell uitvoeren

De volgende PowerShell script vooraf verwerkt het bronbestand en exporteert u deze naar een Azure SQL-database:

    $resourceGroupName = "<AzureResourceGroupName>"
    $hdinsightClusterName = "<HDInsightClusterName>"

    $httpUserName = "admin"
    $httpPassword = "<Password>"

    $defaultStorageAccountName = $hdinsightClusterName + "store"
    $defaultBlobContainerName = $hdinsightClusterName


    $sqlDatabaseServerName = $hdinsightClusterName + "dbserver"
    $sqlDatabaseName = $hdinsightClusterName + "db"
    $sqlDatabaseLogin = "sqluser"
    $sqlDatabasePassword = "<Password>"

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion
        
    #region - pre-process the source file
        
    Write-Host "`nPreprocessing the source file ..." -ForegroundColor Green
        
    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"
        
    # Define the connection string
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
    # Create block blob objects referencing the source and destination blob.
    $storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $defaultStorageAccountName
    $storageContainer = ($storageAccount |Get-AzureStorageContainer -Name $defaultBlobContainerName).CloudBlobContainer
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        
    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)
        
    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
        
    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")
        
        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)
        
            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }
        
        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }
        
    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)
        
    #endregion
        
    #region - export the log file from the cluster to the SQL database
        
    Write-Host "Exporting the log file ..." -ForegroundColor Green

    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
        
    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"
        
    $tableName_log4j = "log4jlogs"
    $exportDir_log4j = "/tutorials/usesqoop/data"
    $sqljdbcdriver = "/user/oozie/share/lib/sqoop/sqljdbc41.jar"
        
    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1" `
        -Files $sqljdbcdriver

    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
        
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    #endregion

##<a name="limitations"></a>Beperkingen

* Bulk export - met Linux-gebaseerde HDInsight, de Sqoop-connector gebruikt om gegevens te exporteren naar Microsoft SQL Server of SQL-Database Azure biedt momenteel geen ondersteuning voor bulksgewijs invoegen.

* Batchen - met Linux-gebaseerde HDInsight bij het gebruik van de `-batch` Ga bij het uitvoeren van toevoegingen, Sqoop meerdere ingevoegd in plaats van de batchverwerking de insert-bewerkingen worden uitgevoerd.

##<a name="next-steps"></a>Volgende stappen

U hebt nu geleerd dat het gebruik van Sqoop. Voor meer informatie, Zie:

- [Gebruik Oozie met HDInsight](hdinsight-use-oozie.md): gebruik Sqoop actie in de workflow van een Oozie.
- [Analyseren vertraging vluchtgegevens met HDInsight](hdinsight-analyze-flight-delay-data.md): component gebruiken voor het analyseren van flight uitstellen van gegevens en gebruik vervolgens Sqoop om gegevens te exporteren naar een database SQL Azure.
- [Uploaden van gegevens naar HDInsight](hdinsight-upload-data.md): zoeken naar andere methoden voor het uploaden van gegevens naar HDInsight/Azure Blob-opslag.


[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

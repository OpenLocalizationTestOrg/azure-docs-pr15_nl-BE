<properties
    pageTitle="Gebruik van Hadoop Sqoop in HDInsight | Microsoft Azure"
    description="Informatie over het uitvoeren van Sqoop importeren en exporteren tussen een cluster Hadoop en Azure SQL-database met HDInsight .NET SDK."
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
   ms.date="09/14/2016"
    ms.author="jgao"/>

#<a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>Met .NET SDK voor Hadoop in HDInsight Sqoop-taken uitvoeren

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het HDInsight .NET SDK gebruiken voor het uitvoeren van taken van Sqoop in HDInsight importeren en exporteren tussen de cluster HDInsight en Azure SQL-database of SQL Server-database.

> [AZURE.NOTE] De stappen in dit artikel kunnen worden gebruikt met of een Windows- of Linux HDInsight cluster. deze stappen werken echter alleen vanuit een Windows-client. Gebruik de knop voor tabselectie boven aan dit artikel om andere methoden.

###<a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Cluster A Hadoop in HDInsight**. Zie [cluster maken en SQL-database](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="run-sqoop-using-net-sdk"></a>Sqoop met .NET SDK wordt uitgevoerd

De HDInsight .NET SDK biedt clientbibliotheken .NET, waardoor het makkelijker om te werken met clusters van .NET HDInsight. In dit gedeelte maakt u een consoletoepassing C# de hivesampletable exporteren naar de SQL-databasetabel die u eerder in deze zelfstudie hebt gemaakt.

**Indienen van een project Sqoop**

1. Maak een C#-consoletoepassing in Visual Studio.
2. De Visual Studio Package Manager-console de volgende Nuget-opdracht voor het importeren van het pakket.

        Install-Package Microsoft.Azure.Management.HDInsight.Job
        
3. Gebruik de volgende code in het bestand Program.cs:

        using System.Collections.Generic;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
        
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
        
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
        
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
        
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
        
                    SubmitSqoopJob();
        
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
        
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
        
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
        
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
        
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
        
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
        
4. Druk op **F5** om het programma te starten. 

##<a name="limitations"></a>Beperkingen

* Bulk export - met Linux-gebaseerde HDInsight, de Sqoop-connector gebruikt om gegevens te exporteren naar Microsoft SQL Server of SQL-Database Azure biedt momenteel geen ondersteuning voor bulksgewijs invoegen.

* Batchen - met Linux-gebaseerde HDInsight bij het gebruik van de `-batch` Ga bij het uitvoeren van toevoegingen, Sqoop meerdere ingevoegd in plaats van de batchverwerking de insert-bewerkingen worden uitgevoerd.

##<a name="next-steps"></a>Volgende stappen

U hebt nu geleerd dat het gebruik van Sqoop. Voor meer informatie, Zie:

- [Gebruik Oozie met HDInsight](hdinsight-use-oozie.md): gebruik Sqoop actie in de workflow van een Oozie.
- [Analyseren vertraging vluchtgegevens met HDInsight](hdinsight-analyze-flight-delay-data.md): component gebruiken voor het analyseren van flight uitstellen van gegevens en gebruik vervolgens Sqoop om gegevens te exporteren naar een database SQL Azure.
- [Uploaden van gegevens naar HDInsight](hdinsight-upload-data.md): zoeken naar andere methoden voor het uploaden van gegevens naar HDInsight/Azure Blob-opslag.



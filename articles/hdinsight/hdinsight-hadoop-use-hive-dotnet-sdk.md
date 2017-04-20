<properties
    pageTitle="Component uitvoeren met behulp van HDInsight .NET SDK | Microsoft Azure"
    description="Informatie over het indienen van Hadoop taken naar Azure HDInsight Hadoop met HDInsight .NET SDK."
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

# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>Component uitvoeren met behulp van HDInsight .NET SDK

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


Informatie over het onderdeel query's uitvoeren met behulp van HDInsight .NET SDK.

> [AZURE.NOTE] De stappen in dit artikel moeten worden uitgevoerd vanaf een Windows-client. Gebruik de knop voor tabselectie weergegeven boven aan het artikel voor meer informatie over het gebruik van een Linux, OS X- of Unix-client werken met component.

##<a name="prerequisites"></a>Vereisten

Voordat u dit artikel hebt u het volgende:

- **Cluster A Hadoop in HDInsight**. Zie [aan de slag met Linux-gebaseerde Hadoop in HDInsight](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Visual Studio 2012/2013/2015**.

##<a name="submit-hive-queries-using-hdinsight-net-sdk"></a>Component-query's uitvoeren met behulp van HDInsight .NET SDK

De HDInsight .NET SDK biedt clientbibliotheken .NET, waardoor het makkelijker om te werken met clusters van .NET HDInsight. 

**Taken**

1. Maak een C#-consoletoepassing in Visual Studio.
2. Voer de volgende opdracht vanaf de Console Nuget Package Manager.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

2. De volgende code gebruiken:

        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
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

                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                    SubmitHiveJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "ravi" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);

                    System.Console.WriteLine("Waiting for the job completion ...");

                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }

                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure

                    System.Console.WriteLine("Job output is: ");

                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }

5. Druk op **F5** om de toepassing te starten.


## <a name="next-steps"></a>Volgende stappen

In dit artikel, kunt u op verschillende manieren voor het maken van een HDInsight-cluster hebt geleerd. Voor meer informatie, Zie de volgende artikelen:

* [Aan de slag met Azure HDInsight][hdinsight-get-started]
* [Hadoop clusters maken in HDInsight][hdinsight-provision]
* [Hadoop clusters in het HDInsight beheren via de Portal Azure](hdinsight-administer-use-management-portal.md)
* [HDInsight .NET SDK verwijzing](https://msdn.microsoft.com/library/mt271028.aspx)
* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)
* [Gebruik Sqoop met HDInsight](hdinsight-use-sqoop-mac-linux.md)
* [Verificatie met niet-interactieve HDInsight .NET-toepassingen maken](hdinsight-create-non-interactive-authentication-dotnet-applications.md)


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md



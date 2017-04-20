<properties
   pageTitle="Gebruik van Hadoop varkens met .NET in HDInsight | Microsoft Azure"
   description="Informatie over het gebruik van de .NET SDK voor Hadoop taken kunnen verzenden varkens naar Hadoop op HDInsight."
   services="hdinsight"
   documentationCenter=".net"
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Met de .NET SDK voor Hadoop in HDInsight varkens-taken uitvoeren

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Dit document bevat een voorbeeld van het gebruik van de .NET SDK voor Hadoop taken kunnen verzenden varkens naar een Hadoop op HDInsight cluster.

De HDInsight .NET SDK biedt .NET client libraries die gemakkelijker werken met clusters van .NET HDInsight. Varken kunt u MapReduce bewerkingen door het modelleren van een reeks gegevenstransformaties maken. Hier leert u hoe een varken-taak aan een cluster van HDInsight met een eenvoudig C#-toepassing.

## <a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel hebt uitgevoerd, moet u het volgende.

* Een cluster Azure HDInsight (Hadoop op HDInsight) (Windows of Linux-gebaseerde).
* Visual Studio 2012 of 2013 of 2015.

## <a name="create-the-application"></a>De toepassing maken

De HDInsight .NET SDK biedt clientbibliotheken .NET, waardoor het makkelijker om te werken met clusters van .NET HDInsight. 


1. Open Visual Studio 2012 of 2013
2. Selecteer **Nieuw** in het menu **bestand** en selecteer **Project**.
3. Typ of Selecteer de volgende waarden voor het nieuwe project.

    <table>
    <tr>
    <th>Eigenschap</th>
    <th>Waarde</th>
    </tr>
    <tr>
    <th>Categorie</th>
    <th>Sjablonen/Visual C# / Windows</th>
    </tr>
    <tr>
    <th>Sjabloon</th>
    <th>Consoletoepassing</th>
    </tr>
    <tr>
    <th>Naam</th>
    <th>SubmitPigJob</th>
    </tr>
    </table>
4. Klik op **OK** om het project te maken.
5. Selecteer **Library Package Manager** of **Nuget Package Manager**in het menu **Extra** en selecteer **Package Manager-Console**.
6. Voer de volgende opdracht in de console te installeren van de SDK voor .NET-pakketten.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

7. Dubbelklik in de Solution Explorer op **Program.cs** om deze te openen. De bestaande code te vervangen door de volgende.

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

                    SubmitPigJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitPigJob()
                {
                    var parameters = new PigJobSubmissionParameters
                    {
                        Query = @"LOGS = LOAD 'wasbs:///example/data/sample.log';
                                    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                    RESULT = order FREQUENCIES by COUNT desc;
                                    DUMP RESULT;"
                    };

                    System.Console.WriteLine("Submitting the Pig job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }


7. Druk op **F5** om de toepassing te starten.
8. Druk op **ENTER** om de toepassing te sluiten.

## <a name="summary"></a>Samenvatting

Zoals u ziet, wordt de .NET SDK voor Hadoop kunt u .NET toepassingen maken die aan een cluster HDInsight varkens taken en de status van de monitor.

## <a name="next-steps"></a>Volgende stappen

Voor algemene informatie over varkens in HDInsight.

* [Varken gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-pig.md)

Voor informatie over andere manieren waarop kunt u werken met Hadoop op HDInsight.

* [Component gebruiken in combinatie met Hadoop op HDInsight](hdinsight-use-hive.md)

* [Gebruik MapReduce met Hadoop op HDInsight](hdinsight-use-mapreduce.md) [voorbeeld portal]: https://portal.azure.com/

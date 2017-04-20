<properties
    pageTitle="Aanbevelingen met Mahout en HDInsight op basis van WIndows genereren | Microsoft Azure"
    description="Informatie over het leren van de bibliotheek Apache Mahout computer gebruiken voor het genereren van film aanbevelingen op basis van Windows HDInsight (Hadoop)."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight"></a>Film aanbevelingen genereren met behulp van Apache Mahout met Hadoop in HDInsight

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Leren leren-bibliotheek met Azure HDInsight [Apache Mahout](http://mahout.apache.org) computer gebruiken voor het genereren van film aanbevelingen.

> [AZURE.NOTE] De stappen in dit document moet een Windows-client en een cluster van Windows-gebaseerde HDInsight. Zie voor meer informatie over het gebruik van Mahout van Linux, OS X en Unix-client met een Linux-gebaseerde HDInsight cluster [genereren film aanbevelingen met behulp van Apache Mahout met Linux-gebaseerde Hadoop in HDInsight](hdinsight-hadoop-mahout-linux-mac.md)


##<a name="learn"></a>U leert

Mahout is een [machine learning] [ ml] library voor Apache Hadoop. Mahout bevat algoritmen voor het verwerken van gegevens, zoals filteren, indelen, en clusters. In dit artikel gebruikt u een aanbeveling engine voor het genereren van film aanbevelingen op basis van films die je vrienden hebt gezien. Ook leert u classificaties met een beschikking forest uitvoeren. Dit leert u het volgende:

* Mahout-taken uitvoeren met Windows PowerShell

* Mahout taken uitvoeren vanaf de opdrachtregel Hadoop

* Het installeren van Mahout op clusters 3.0 HDInsight en HDInsight 2.0

    > [AZURE.NOTE] Mahout wordt geleverd met de versie 3.1 van de HDInsight van de clusters. Als u een eerdere versie van HDInsight, Zie [Mahout installeren](#install) voordat u doorgaat.

##<a name="prerequisites"></a>vereisten

- **Hadoop een Windows gebaseerde cluster in HDInsight**. Zie voor informatie over het maken van een [aan de slag met Hadoop in HDInsight][getstarted]
- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a name="recommendations"></a>Aanbevelingen te genereren met behulp van Windows PowerShell

> [AZURE.NOTE] Hoewel de taak gebruikt in deze sectie maakt gebruik van Windows PowerShell, veel van de klassen Mahout voorzien momenteel werken niet met Windows PowerShell en moeten worden uitgevoerd met behulp van de opdrachtregel Hadoop. Zie de sectie [Probleemoplossing](#troubleshooting) voor een lijst van categorieën die niet met Windows PowerShell werken.
>
> Zie voor een voorbeeld van het gebruik van de opdrachtregel Hadoop Mahout taken uitvoeren, [gegevens met behulp van de opdrachtregel Hadoop Classify](#classify).

Een van de functies die beschikbaar is in de Mahout is een aanbeveling engine. Deze engine accepteert de gegevens in de indeling van `userID`, `itemId`, en `prefValue` (gebruikers van de voorkeur voor het artikel). Mahout kunt collega exemplaar analyse bepaalt vervolgens uitvoeren: _gebruikers met een voorkeur voor een artikel ook een voorkeur voor deze andere artikelen hebben_. Mahout bepaalt vervolgens gebruikers met dergelijke artikel voorkeuren, die kunnen worden gebruikt om aanbevelingen te doen.

Het volgende is een zeer eenvoudig voorbeeld met films:

* __Collega exemplaar__: Jan, Alice en Bob beviel _Star Wars_ _Terug stakingen in het rijk_en _van de Jedi terug_. Mahout bepaalt dat gebruikers die een van deze films ook zoals de andere twee.

* __Collega exemplaar__: Bob en Els ook interessant vond _De Phantom Menace_en _een aanval van de klonen_ _Revenge van de Sith_. Mahout bepaalt dat gebruikers die de vorige drie films ook interessant vond zoals deze drie.

* __Aanbeveling van de overeenkomsten__: Joe omdat de eerste drie films beviel, Mahout kijkt films die door anderen met soortgelijke voorkeuren beviel, maar Joe niet heeft gevolgd (beviel/geclassificeerd). In dat geval raadt Mahout _The Phantom Menace_en _een aanval van de klonen_ _Revenge van de Sith_.

###<a name="understanding-the-data"></a>Wat zijn de gegevens?

Gemakkelijk [GroupLens Research] [ movielens] beoordeling van gegevens voor de films in een indeling die compatibel is met de Mahout bevat. Deze gegevens zijn beschikbaar op uw van standaard clusteropslag op `/HdiSamples/MahoutMovieData`.

Er zijn twee bestanden `moviedb.txt` (informatie over de films,) en `user-ratings.txt`. De gebruiker-ratings.txt-bestand wordt gebruikt tijdens de analyse, terwijl moviedb.txt wordt gebruikt om beschrijvende tekst gegevens bij het weergeven van de resultaten van de analyse.

De gegevens in de gebruiker ratings.txt heeft een structuur van `userID`, `movieID`, `userRating`, en `timestamp`, die vertellen hoe sterk een film het prioriteitsniveau van elke gebruiker. Hier volgt een voorbeeld van de gegevens:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

###<a name="run-the-job"></a>De taak uitvoeren

De volgende Windows PowerShell-script gebruiken voor het uitvoeren van een taak die gebruikmaakt van de Mahout aanbeveling engine met de filmgegevens:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
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
            
    # NOTE: The version number in the file path
    # may change in future versions of HDInsight.
    $jarFile =  "file:///C:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar"
    #
    # If you are using an earlier version of HDInsight,
    # set $jarFile to the jar file you
    # uploaded.
    # For example,
    # $jarFile = "wasbs:///example/jars/mahout-core-0.9-job.jar"

    # The arguments for this job
    # * input - the path to the data uploaded to HDInsight
    # * output - the path to store output data
    # * tempDir - the directory for temp files
    $jobArguments = "--similarityClassname", "recommenditembased", `
                    "-s", "SIMILARITY_COOCCURRENCE", `
                    "--input", "wasbs:///HdiSamples/MahoutMovieData/user-ratings.txt",
                    "--output", "wasbs:///example/out",
                    "--tempDir", "wasbs:///example/temp"

    # Create the job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
      -JarFile $jarFile `
      -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
      -Arguments $jobArguments

    # Start the job
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

    # Wait on the job to complete
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
            -Blob example/out/part-r-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
            
    # Write out any error information
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

> [AZURE.NOTE] Mahout taken verwijderd tijdelijk gegevens die zijn gemaakt tijdens het verwerken van de taak niet. De `--tempDir` parameter is opgegeven in het voorbeeld-project voor het isoleren van de tijdelijke bestanden in een bepaalde map.

De Mahout taak geeft geen resultaat van de uitvoer naar STDOUT. In plaats daarvan deze opgeslagen in de uitvoermap opgegeven als __deel-r-00000__. Het script wordt dit bestand gedownload naar __uitvoer.txt__ in de huidige map op uw werkstation.

Hier volgt een voorbeeld van de inhoud van dit bestand:

    1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

De eerste kolom is de `userID`. De waarden in ' [' en ']' worden `movieId`:`recommendationScore`.

###<a name="view-the-output"></a>Weergave van de uitvoer

Hoewel de gegenereerde output mogelijk OK voor gebruik in een toepassing, is het niet erg leesbaar. De `moviedb.txt` van de server kan worden gebruikt voor het oplossen van de `movieId` aan een film naam, maar u moet eerst downloaden en het bestand beoordelingen van de server met het volgende script:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
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
    #Download the files
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/moviedb.txt" `
    -Container $container `
    -Destination moviedb.txt `
    -Context $context
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/user-ratings.txt" `
    -Container $container `
    -Destination user-ratings.txt `
    -Context $context

Nadat u de bestanden hebt gedownload, gebruik de volgende PowerShell script om aanbevelingen met de namen van de film weer te geven:

    <#
    .SYNOPSIS
        Displays recommendations for movies.
    .DESCRIPTION
        Displays recommendations generated by Mahout
        with HDInsight example in a human readable format.
    .EXAMPLE
        .\Show-Recommendation -userId 4
            -userDataFile "user-ratings.txt"
            -movieFile "moviedb.txt"
            -recommendationFile "output.txt"
    #>

    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
        #The user ID
        [Parameter(Mandatory = $true)]
        [String]$userId,

        [Parameter(Mandatory = $true)]
        [String]$userDataFile,

        [Parameter(Mandatory = $true)]
        [String]$movieFile,

        [Parameter(Mandatory = $true)]
        [String]$recommendationFile
    )
    # Read movie ID & description into hash table
    Write-Host "Reading movies descriptions" -ForegroundColor Green
    $movieById = @{}
    foreach($line in Get-Content $movieFile)
    {
        $tokens = $line.Split("|")
        $movieById[$tokens[0]] = $tokens[1]
    }
    # Load movies user has already seen (rated)
    # into a hash table
    Write-Host "Reading rated movies" -ForegroundColor Green
    $ratedMovieIds = @{}
    foreach($line in Get-Content $userDataFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            # Resolve the ID to the movie name
            $ratedMovieIds[$movieById[$tokens[1]]] = $tokens[2]
        }
    }
    # Read recommendations generated by Mahout
    Write-Host "Reading recommendations" -ForegroundColor Green
    $recommendations = @{}
    foreach($line in get-content $recommendationFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            #Trim leading/treailing [] and split at ,
            $movieIdAndScores = $tokens[1].TrimStart("[").TrimEnd("]").Split(",")
            foreach($movieIdAndScore in $movieIdAndScores)
            {
                #Split at : and store title and score in a hash table
                $idAndScore = $movieIdAndScore.Split(":")
                $recommendations[$movieById[$idAndScore[0]]] = $idAndScore[1]
            }
            break
        }
    }

    Write-Host "Rated movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $ratedFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                   @{Expression={$_.Value};Label="Rating"}
    $ratedMovieIds | format-table $ratedFormat
    Write-Host "---------------------------" -ForegroundColor Green

    write-host "Recommended movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $recommendationFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                            @{Expression={$_.Value};Label="Score"}
    $recommendations | format-table $recommendationFormat

Hier volgt een voorbeeld van het uitvoeren van het script:

    PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt

De uitvoer ziet er ongeveer als volgt:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237  

##<a name="classify"></a>Gegevens met behulp van de opdrachtregel Hadoop classificeren

Een van de classificatiemethoden beschikbaar met Mahout is het bouwen van een [willekeurige forest][forest]. Dit is een proces van meerdere waarbij met behulp van trainingsgegevens beslissingsstructuren, die worden gebruikt voor het classificeren van gegevens te genereren. Dit wordt de __org.apache.mahout.classifier.df.tools.Describe__ class van Mahout gebruikt. Het moet op dat moment worden uitgevoerd met behulp van de opdrachtregel Hadoop.

###<a name="load-the-data"></a>De gegevens worden geladen

1. De volgende bestanden downloaden van [de NSL KDD gegevensset](http://nsl.cs.unb.ca/NSL-KDD/).

  * [KDDTrain +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTrain+.arff): het bestand training

  * [KDDTest +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTest+.arff): de testgegevens

2. Open elk bestand en verwijder de regels boven die beginnen met '@', en de bestanden vervolgens opslaan. Als deze niet worden verwijderd, ontvangt u foutberichten als u deze gegevens met Mahout.

2. De bestanden uploaden naar __voorbeeld/gegevens__. U kunt dit doen met behulp van het volgende script. __CLUSTERNAAM__ vervangen door de naam van het cluster HDInsight. BESTANDSNAAM vervangen door de naam voor het bestand te uploaden.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName="CLUSTERNAME"
        $fileToUpload="FILENAME"
        $blobPath="example/data/FILENAME"
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
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context

###<a name="run-the-job"></a>De taak uitvoeren

1. Deze taak moet de opdrachtregel Hadoop. Extern bureaublad inschakelen voor het cluster HDInsight en vervolgens verbinding maken met het door de instructies op [verbinding maken met clusters van HDInsight met RDP](hdinsight-administer-use-management-portal.md#rdp).

3. Nadat u hebt aangesloten, de opdrachtregel Hadoop openen via het pictogram __Hadoop vanaf de opdrachtregel__ :

    ![hadoop-cli][hadoopcli]

3. Gebruik de volgende opdracht genereert de bestandsbeschrijving (__KDDTrain + .info__), dat gebruikmaakt van Mahout.

        hadoop jar "c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar" org.apache.mahout.classifier.df.tools.Describe -p "wasbs:///example/data/KDDTrain+.arff" -f "wasbs:///example/data/KDDTrain+.info" -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L

    De `N 3 C 2 N C 4 N C 8 N 2 C 19 N L` vindt u de kenmerken van de gegevens in het bestand. L geeft bijvoorbeeld een label aan.

4. Een forest van beslissingsbomen bouwen met behulp van de volgende opdracht:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapred.max.split.size=1874231 -d wasbs:///example/data/KDDTrain+.arff -ds wasbs:///example/data/KDDTrain+.info -sl 5 -p -t 100 -o nsl-forest

    De uitvoer van deze bewerking is opgeslagen in de map __nsl forest__ , dat in de opslag voor het cluster HDInsight bij __ wasbs://user/ bevindt zich&lt;gebruikersnaam > / nsl forest/nsl forest.seq. De &lt;gebruikersnaam > is de gebruikersnaam die u voor de extern bureaublad-sessie gebruikt. Dit bestand kan niet worden gelezen door de mens.

5. Het forest testen door het classificeren van de dataset __KDDTest + .arff__ . Gebruik de volgende opdracht:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.TestForest -i wasbs:///example/data/KDDTest+.arff -ds wasbs:///example/data/KDDTrain+.info -m nsl-forest -a -mr -o wasbs:///example/data/predictions

    Met deze opdracht haalt samenvattingsgegevens over het classificatieproces met de volgende strekking:

        14/07/02 14:29:28 INFO mapreduce.TestForest:

        =======================================================
        Summary
        -------------------------------------------------------
        Correctly Classified Instances          :      17560       77.8921%
        Incorrectly Classified Instances        :       4984       22.1079%
        Total Classified Instances              :      22544

        =======================================================
        Confusion Matrix
        -------------------------------------------------------
        a       b       <--Classified as
        9437    274      |  9711        a     = normal
        4710    8123     |  12833       b     = anomaly

        =======================================================
        Statistics
        -------------------------------------------------------
        Kappa                                       0.5728
        Accuracy                                   77.8921%
        Reliability                                53.4921%
        Reliability (standard deviation)            0.4933

  Dit project maakt ook het bestand vinden op __wasbs:///example/data/predictions/KDDTest+.arff.out__. Dit bestand is echter niet kan worden gelezen door de mens.

> [AZURE.NOTE] Mahout taken overschrijven bestanden niet. Als u wilt dat deze taken opnieuw uitvoeren, moet u de bestanden die zijn gemaakt door vorige taken verwijderen.

##<a name="troubleshooting"></a>Het oplossen van problemen

###<a name="install"></a>Mahout installeren

Mahout is geïnstalleerd op de clusters HDInsight 3.1 en dit kan handmatig worden geïnstalleerd op HDInsight 3.0 of 2.1 HDInsight clusters met behulp van de volgende stappen uit:

1. De versie van Mahout te gebruiken, is afhankelijk van de versie HDInsight van het cluster. Door de eigenschappen van het cluster in de Azure portal vindt u de versie van het cluster.

  * __2.1 voor HDInsight__, kunt u een Java-archief (JAR) bestand met [0,9 Mahout](http://repo2.maven.org/maven2/org/apache/mahout/mahout-core/0.9/mahout-core-0.9-job.jar)downloaden.

  * __Voor HDInsight 3.0__, moet u [bouwen Mahout uit de bron] [ build] en geef de Hadoop versie geleverd door HDInsight. Vereisten zoals vermeld op de pagina bouwen installeren, downloadt u de bron en gebruikt u de volgende opdracht om de Mahout jar-bestanden te maken:

            mvn -Dhadoop2.version=2.2.0 -DskipTests clean package

        After the build completes, you can find the JAR file at __mahout\mrlegacy\target\mahout-mrlegacy-1.0-SNAPSHOT-job.jar__.

        > [AZURE.NOTE] Wanneer Mahout 1.0 is vrijgegeven, moet u mogelijk zijn de vooraf gedefinieerde pakketten gebruiken met HDInsight 3.0.

2. Het jar-bestand uploaden naar __voorbeeld/potten__ in de standaard opslaglocatie voor uw cluster. Vervangen door de CLUSTERNAAM in het volgende script de naam van het cluster HDInsight en vervangt u FILENAME door het pad naar het bestand __mahout coure-0,9 job.jar__ ...

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName = "CLUSTERNAME"
        $fileToUpload = "FILENAME"
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
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob "example/jars/mahout-core-0.9-job.jar" `
            -Container $container `
            -Context $context

###<a name="cannot-overwrite-files"></a>Kan bestanden niet overschrijven.

Mahout taken kan niet opschonen van tijdelijke bestanden die zijn gemaakt tijdens de verwerking. Bovendien wordt een bestaand uitvoerbestand niet overschreven door de taken.

Om fouten te voorkomen bij het uitvoeren van taken Mahout, tijdelijke en uitvoer van bestanden tussen twee uitvoeringen verwijderen of unieke tijdelijke en uitvoer mapnamen gebruiken.

###<a name="cannot-find-the-jar-file"></a>Het JAR-bestand vinden niet

3.1 HDInsight clusters zijn Mahout. Het pad en de naam het versienummer van de Mahout die is geïnstalleerd op het cluster bevatten. Het voorbeeldscript Windows PowerShell in deze zelfstudie wordt een pad dat geldig is vanaf November 2015, maar het versienummer wordt gewijzigd in toekomstige updates in HDInsight. Gebruik de volgende Windows PowerShell-opdracht om te bepalen van het huidige pad naar de Mahout JAR-bestand voor uw cluster, en wijzig het script om te verwijzen naar het pad dat wordt geretourneerd:

    Use-AzureRmHDInsightCluster -ClusterName $clusterName
    #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "wasbs:///example/statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target\*-job.jar'

###<a name="nopowershell"></a>Klassen die niet met Windows PowerShell werken

Mahout-functies die gebruikmaken van de volgende klassen retourneren verschillende foutberichten worden weergegeven als ze worden gebruikt in Windows PowerShell:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Functies die gebruikmaken van deze klassen wordt uitgevoerd, verbinding maken met het cluster HDInsight, en de taken worden uitgevoerd met behulp van de opdrachtregel Hadoop. Zie [Classify gegevens via de opdrachtregel Hadoop](#classify) voor een voorbeeld.

##<a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u Mahout, andere manieren van werken met gegevens op HDInsight ontdekken:

* [Component met HDInsight](hdinsight-use-hive.md)
* [Varken met HDInsight](hdinsight-use-pig.md)
* [MapReduce met HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: ../powershell-install-configure.md
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 

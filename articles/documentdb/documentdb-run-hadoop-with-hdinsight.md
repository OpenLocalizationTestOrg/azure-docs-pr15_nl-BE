<properties
    pageTitle="Een Hadoop-up met behulp van DocumentDB en HDInsight | Microsoft Azure"
    description="Informatie over het uitvoeren van een eenvoudige taak van component-, varkens- en MapReduce met DocumentDB en HDInsight van Azure."
    services="documentdb"
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"
    documentationCenter=""/>


<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="denlee"/>

#<a name="DocumentDB-HDInsight"></a>Een Hadoop-up met behulp van DocumentDB en HDInsight

In deze zelfstudie wordt beschreven hoe u [Apache component]uitvoeren u[apache-hive], [Apache varken][apache-pig], en [Apache Hadoop] [ apache-hadoop] MapReduce projecten in Azure HDInsight met DocumentDB van Hadoop-connector. DocumentDB van Hadoop connector staat DocumentDB om te fungeren als een bron- en sink voor component-, varkens- en MapReduce taken. In deze zelfstudie wordt DocumentDB gebruikt als de gegevensbron en de bestemming voor Hadoop taken.

Na het voltooien van deze zelfstudie, zult u de volgende vragen beantwoorden:

- Hoe ik gegevens laden van DocumentDB met behulp van een component, varken of MapReduce.?
- Hoe kan ik gegevens opslaan in DocumentDB met behulp van een component, varken of MapReduce.?

Het beste aan de slag in de volgende video, waar we uitvoeren via een component-project met behulp van DocumentDB en HDInsight volgen.

> [AZURE.VIDEO use-azure-documentdb-hadoop-connector-with-azure-hdinsight]

Vervolgens terug naar dit artikel, waar u ontvangt de volledige details over hoe u analytics taken op uw gegevens DocumentDB uitvoeren kunt.

> [AZURE.TIP] In deze zelfstudie wordt ervan uitgegaan dat u ervaring met Apache Hadoop, component en/of varkens. Als u Apache Hadoop, component en varkens, wordt aangeraden de [documentatie voor Apache Hadoop]bezoeken[apache-hadoop-doc]. In deze zelfstudie wordt ervan uitgegaan dat u ervaring met DocumentDB en een DocumentDB-account hebt. Als u bekend met DocumentDB bent of er niet een account DocumentDB, check onze [Aan de slag] [ getting-started] pagina.

Geen tijd om de zelfstudie te voltooien en wilt krijgen de volledige PowerShell voorbeeldscripts voor component-, varkens- en MapReduce? Geen probleem, ze [hier][documentdb-hdinsight-samples]. De download bevat ook de hql-, varkens- en java-bestanden voor deze monsters.

## <a name="NewestVersion"></a>Nieuwste versie

<table border='1'>
    <tr><th>Versie van Hadoop-Connector</th>
        <td>1.2.0</td></tr>
    <tr><th>Script-Uri</th>
        <td>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</td></tr>
    <tr><th>Datum gewijzigd</th>
        <td>26-04/2016</td></tr>
    <tr><th>Ondersteunde HDInsight versies</th>
        <td>3.1, 3.2</td></tr>
    <tr><th>Wijzigingslogbestand</th>
        <td>Bijgewerkte DocumentDB Java 1.6.0-SDK</br>
            Ondersteuning toegevoegd voor de gepartitioneerde collecties als een bron- en wastafel</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Vereisten
Voordat u de instructies in deze zelfstudie, zorg ervoor dat het volgende:

- Een DocumentDB, een database, en een collectie met behulp van documenten in. Zie voor meer informatie, [Aan de slag met DocumentDB][getting-started]. Voorbeeldgegevens importeren in uw account DocumentDB met de [DocumentDB importeren gereedschap][documentdb-import-data].
- De doorvoer. Leest en schrijft van HDInsight naar uw verzoek toegewezen eenheden voor de collecties worden geteld. Zie voor meer informatie [Provisioned doorvoer, aanvraag eenheden, database of][documentdb-manage-throughput].
- De uitvoer van capaciteit voor een aanvullende opgeslagen procedure in elke collectie. De opgeslagen procedures worden voor het overbrengen van de resulterende documenten gebruikt. Zie voor meer informatie, [collecties en doorvoer ingerichte][documentdb-manage-document-storage].
- Capaciteit voor de documenten van de component, varken of MapReduce taken. Zie voor meer informatie, [DocumentDB beheren, capaciteit en prestaties][documentdb-manage-collections].
- [*Optioneel*] Capaciteit voor een extra-collectie. Zie voor meer informatie, [Provisioned documentopslag en overhead index][documentdb-manage-document-storage].

> [AZURE.WARNING] Om te voorkomen dat het maken van een nieuwe collectie tijdens een van de taken, kunt u de resultaten naar stdout afdrukken, de uitvoer naar de container WASB opslaan of een al bestaande collectie opgeven. Nieuwe documenten worden gemaakt binnen de collectie en al bestaande documenten wordt alleen beïnvloed als *id's*een conflict bij het opgeven van een bestaande collectie. **Bestaande documenten met ID-conflicten worden automatisch overschreven door de connector**. U kunt deze functie uitschakelen door de optie upsert instellen op false. Als upsert ingesteld op false is en een conflict optreedt, wordt de taak Hadoop zal mislukken; een ID-conflict fout melden.


## <a name="ProvisionHDInsight"></a>Stap 1: Maak een nieuw cluster van HDInsight
In deze zelfstudie wordt de actie Script vanaf de Portal Azure cluster HDInsight aanpassen. In deze zelfstudie gebruiken we de Azure-Portal te maken van het cluster HDInsight. Uitchecken voor instructies over het gebruik van PowerShell-cmdlets of HDInsight .NET SDK [clusters van HDInsight aanpassen met behulp van de actie Script] [ hdinsight-custom-provision] artikel.

1. Aanmelden bij de [Azure Portal][azure-portal].

2. Klik op **+ Nieuw** boven op de navigatiebalk aan de linkerkant, zoeken naar **HDInsight** op de bovenste balk op het nieuwe blad.

3. **HDInsight** , gepubliceerd door **Microsoft** worden weergegeven aan de bovenkant van de resultaten. Klik erop en klik vervolgens op **maken**.

4. Op het nieuwe HDInsight-Cluster maken blade, voer de **Naam van het Cluster** en selecteer de gewenste gegevens leveren voor deze resource onder **abonnement** .

    <table border='1'>
        <tr><td>De naam van cluster</td><td>Naam van het cluster.<br/>
   DNS-naam moet beginnen en eindigen met een alfanumerieke tekens en streepjes kan bevatten.<br/>
   Het veld moet een tekenreeks tussen 3 en 63 tekens.</td></tr>
        <tr><td>De naam van abonnement</td>
            <td>Als u meer dan één Azure-abonnement hebt, selecteer het abonnement die als host voor uw cluster HDInsight fungeert. </td></tr>
    </table>

5. Klik op **Cluster-Type selecteren** en de volgende eigenschappen ingesteld op de opgegeven waarden.

    <table border='1'>
        <tr><td>Clustertype</td><td><strong>Hadoop</strong></td></tr>
        <tr><td>Cluster-laag</td><td><strong>Standaard</strong></td></tr>
        <tr><td>Besturingssysteem</td><td><strong>Windows</strong></td></tr>
        <tr><td>Versie</td><td>nieuwste versie</td></tr>
    </table>

    Nu, klik op **selecteren**.

    ![De bijzonderheden Hadoop HDInsight eerste cluster][image-customprovision-page1]

6. Klik op **referenties** instellen uw aanmeldings-id en de RAS-referenties. Kies je **Cluster Login gebruikersnaam** en **wachtwoord van de Cluster-aanmelding**.

    Als u wilt op afstand in het cluster, selecteert u *Ja* onder aan het blad en geef een gebruikersnaam en wachtwoord.

7. Klik op de **Gegevensbron** voor het instellen van de primaire locatie voor toegang tot gegevens. Kiest u de **Methode selecteren** en geef een al bestaande opslag account of maak een nieuwe.

8. Geef een **Standaard Container** en een **locatie**op de dezelfde blade. En klik op **selecteren**.

    > [AZURE.NOTE] Selecteer een locatie dicht bij uw DocumentDB account regio voor betere prestaties

8. Klik op **prijzen** te selecteren, het aantal en type van knooppunten. U kunt de standaardconfiguratie behouden en het aantal knooppunten werknemer hoger op schalen.

9. Klik op **optionele configuratie**vervolgens **scriptacties** in de optionele configuratie Blade.

    Voer de volgende gegevens voor het aanpassen van uw cluster HDInsight in Script-acties.

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Naam</td>
            <td>Geef een naam voor de scriptactie.</td></tr>
        <tr><td>URI-script</td>
            <td>Geef de URI voor het script dat wordt aangeroepen voor het aanpassen van het cluster.</br></br>
   Voer in: </br> <strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</td></tr>
        <tr><td>Hoofd</td>
            <td>Schakel het selectievakje in het PowerShell script naar de hoofd-knooppunt uitgevoerd.</br></br>
            <strong>Schakel dit selectievakje in</strong>.</td></tr>
        <tr><td>Werknemer</td>
            <td>Schakel het selectievakje in als u wilt uitvoeren op het knooppunt van de werknemer de PowerShell-script.</br></br>
            <strong>Schakel dit selectievakje in</strong>.</td></tr>
        <tr><td>Zookeeper</td>
            <td>Schakel het selectievakje in als u wilt uitvoeren de PowerShell-script naar de Zookeeper.</br></br>
            <strong>Niet nodig is</strong>.
            </td></tr>
        <tr><td>Parameters</td>
            <td>Geef de parameters, indien nodig door het script.</br></br>
            <strong>Geen Parameters nodig</strong>.</td></tr>
    </table>

10. Maak een nieuwe **Resourcegroep** of gebruik een bestaande resourcegroep onder uw abonnement Azure.

11. Controleer nu **vastmaken aan het dashboard** wilt bijhouden van de implementatie ervan en klik op **maken**!

## <a name="InstallCmdlets"></a>Stap 2: Installeer en configureer Azure PowerShell

1. Azure PowerShell te installeren. Instructies vindt u [hier][powershell-install-configure].

    > [AZURE.NOTE] Ook voor de component query's, kunt u Editor voor online component van de HDInsight. Hiervoor aanmelden bij de [Azure Portal][azure-portal], klikt u op **HDInsight** in het linkerdeelvenster om een lijst met clusters HDInsight weer te geven. Klik op het gewenste onderdeel query's uitvoeren op cluster en klik vervolgens op **Query-Console**.

2. Open de Azure PowerShell Integrated Scripting Environment:
    - Op een computer met Windows 8 of Windows Server 2012 of hoger, kunt u de ingebouwde zoeken. Het startscherm, typ **powershell ise** en klik op **Enter**.
    - Gebruik het menu Start op een computer met een versie ouder dan Windows 8 of Windows Server 2012. In het menu Start, typ **opdrachtprompt** in het zoekvak en klik vervolgens in de lijst met resultaten, klik op **opdrachtprompt**. Typ **powershell_ise** in het opdrachtpromptvenster en klikt u op **Enter**.

3. Uw Azure Account toevoegen.
    1. In het consolevenster **Toevoegen AzureAccount** typt en op **Enter**.
    2. Typ in het e-mailadres dat is gekoppeld aan uw abonnement Azure en klik op **Doorgaan**.
    3. Typ het wachtwoord voor uw abonnement op Azure.
    4. Klik op **aanmelden**.

4. Het volgende diagram worden de belangrijke onderdelen van uw omgeving Azure PowerShell scripts.

    ![Diagram voor Azure PowerShell][azure-powershell-diagram]

## <a name="RunHive"></a>Stap 3: Uitvoeren van de taak van een component met behulp van DocumentDB en HDInsight

> [AZURE.IMPORTANT] Alle variabelen die worden aangegeven door de combinatie moeten worden ingevuld met behulp van de configuratie-instellingen.

1. De volgende variabelen instellen in uw PowerShell Script-veld.

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"

2. <p>We beginnen met het samenstellen van de query-tekenreeks. We een query component waarmee alle documenten door systeem gegenereerd tijdstempels (_ts) gaat schrijven en unieke id's (_rid) uit een collectie DocumentDB telt alle documenten per minuut en winkels de resultaten weer in een nieuwe DocumentDB-collectie.</p>

    <p>Eerst maken we een tabel onderdeel van onze collectie DocumentDB. Het volgende codefragment toevoegen aan het PowerShell Script deelvenster <strong>na</strong> het codefragment van #1. Zorg ervoor dat u de knipgreep optioneel DocumentDB.query parameter t onze documenten alleen _ts en _rid.</p>

    > [AZURE.NOTE]**DocumentDB.inputCollections benoemen niet een fout was.** Ja, we kunnen meerdere collecties als invoer toe te voegen: </br>

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> The collection names are separated without spaces, using only a single comma.


        # Create a Hive table using data from DocumentDB. Pass DocumentDB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Vervolgens maken we een component-tabel voor de uitvoer-collectie. De eigenschappen van de uitvoer is de maand, dag, uur, minuut en het totale aantal exemplaren.

    > [AZURE.NOTE]**Nog opnieuw naamgeving van DocumentDB.outputCollections is niet een vergissing.** Ja, we kunnen meerdere collecties als uitvoer toe te voegen: </br>
"*DocumentDB.outputCollections*'='*\<DocumentDB uitvoer Collectienaam 1\>*,*\<DocumentDB uitvoer Collectienaam 2\>*" </br> De collectienamen zonder spaties, met alleen een komma gescheiden. </br></br>
Documenten worden in meerdere collecties gedistribueerde round robin. Een reeks documenten worden opgeslagen in één collectie en vervolgens een tweede reeks documenten worden opgeslagen in de volgende verzameling, enzovoort.

        # Create a Hive table for the output data to DocumentDB.
        $queryStringPart2 = "drop table DocumentDB_analytics; " +
                              "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                              "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                              "tblproperties ( " +
                                  "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                  "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                  "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                  "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "

4. Tot slot laten we tally de documenten per maand, dag, uur en minuut en plaats de resultaten weer in de uitvoer component-tabel.

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "

5. In het volgende fragment script wilt maken van de definitie van een component uit de vorige query toevoegen.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    U kunt ook gebruiken om op te geven van een scriptbestand HiveQL op HDFS-schakeloptie in het bestand.

6. In het volgende fragment als u de begintijd opslaan en verzenden van de taak van de component wilt toevoegen.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition

7. Voeg de volgende te wachten op het onderdeel taak is voltooid.

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600

8. Voeg de volgende de standaarduitvoer en de begin- en eindtijden wilt afdrukken.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Voer** het nieuwe script! **Klik op** de knop groen uitvoeren.

10. Controleer de resultaten. Aanmelden bij de [Azure Portal][azure-portal].
    1. Klik op <strong>Bladeren</strong> in het deelvenster links. </br>
    2. Klik op <strong>Alles</strong> rechts boven in het deelvenster bladeren. </br>
    3. Zoek en klik op <strong>Accounts met DocumentDB</strong>. </br>
    4. Zoek vervolgens de <strong>DocumentDB Account</strong>en vervolgens de <strong>DocumentDB Database</strong> en de <strong>DocumentDB-collectie</strong> die is gekoppeld aan de opgegeven in uw query component output-collectie.</br>
    5. Ten slotte klikt u op <strong>Document Explorer</strong> onder <strong>Developer Tools</strong>.</br></p>

    U ziet de resultaten van uw query component.

    ![Queryresultaten component][image-hive-query-results]

## <a name="RunPig"></a>Stap 4: Voer de taak van een varken met DocumentDB en HDInsight

> [AZURE.IMPORTANT] Alle variabelen die worden aangegeven door de combinatie moeten worden ingevuld met behulp van de configuratie-instellingen.

1. De volgende variabelen instellen in uw PowerShell Script-veld.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"

2. <p>We beginnen met het samenstellen van de query-tekenreeks. Zullen we een varken query waarmee alle documenten door systeem gegenereerd tijdstempels (_ts) schrijven en unieke id's (_rid) uit een collectie DocumentDB telt alle documenten per minuut en winkels de resultaten weer in een nieuwe DocumentDB-collectie.</p>
    <p>Documenten uit de DocumentDB eerst geladen in HDInsight. Het volgende codefragment toevoegen aan het PowerShell Script deelvenster <strong>na</strong> het codefragment van #1. Zorg ervoor dat u een query DocumentDB toevoegen aan de optionele parameter DocumentDB query voor het bijsnijden van onze documenten alleen _ts en _rid.</p>

    > [AZURE.NOTE]Ja, we kunnen meerdere collecties als invoer toe te voegen: </br>
"*\<DocumentDB collectie invoernaam 1\>*,*\<DocumentDB collectie invoernaam 2\>*"</br> De collectienamen zonder spaties, met alleen een komma gescheiden. </b>

    Documenten worden in meerdere collecties gedistribueerde round robin. Een reeks documenten worden opgeslagen in één collectie en vervolgens een tweede reeks documenten worden opgeslagen in de volgende verzameling, enzovoort.

        # Load data from DocumentDB. Pass DocumentDB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Vervolgens tally we de documenten van de maand, dag, uur, minuut en het totale aantal exemplaren.

        # GROUP BY minute and COUNT entries for each.
        $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                            "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                            "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "

4. Laten we ten slotte de resultaten opslaan in onze nieuwe collectie voor de uitvoer.

    > [AZURE.NOTE]Ja, we kunnen meerdere collecties als uitvoer toe te voegen: </br>
"\<DocumentDB uitvoer Collectienaam 1\>,\<DocumentDB uitvoer Collectienaam 2\>"</br> De collectienamen zonder spaties, met alleen een komma gescheiden.</br>
Gedistribueerde round robin-documenten worden in de verschillende collecties. Een reeks documenten worden opgeslagen in één collectie en vervolgens een tweede reeks documenten worden opgeslagen in de volgende verzameling, enzovoort.

        # Store output data to DocumentDB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "

5. In het volgende fragment script wilt maken van een taakdefinitie varken uit de vorige query toevoegen.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    U kunt ook gebruiken om op te geven van een scriptbestand varken op HDFS-schakeloptie in het bestand.

6. In het volgende fragment om te slaan de begintijd en de varkens-taak toevoegen.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition

7. Voeg de volgende te wachten op de varkens taak te voltooien.

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600

8. Voeg de volgende de standaarduitvoer en de begin- en eindtijden wilt afdrukken.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Voer** het nieuwe script! **Klik op** de knop groen uitvoeren.

10. Controleer de resultaten. Aanmelden bij de [Azure Portal][azure-portal].
    1. Klik op <strong>Bladeren</strong> in het deelvenster links. </br>
    2. Klik op <strong>Alles</strong> rechts boven in het deelvenster bladeren. </br>
    3. Zoek en klik op <strong>Accounts met DocumentDB</strong>. </br>
    4. Zoek vervolgens de <strong>DocumentDB Account</strong>en vervolgens de <strong>DocumentDB Database</strong> en de <strong>DocumentDB-collectie</strong> die is gekoppeld aan de uitvoer-collectie opgegeven in uw query varken.</br>
    5. Ten slotte klikt u op <strong>Document Explorer</strong> onder <strong>Developer Tools</strong>.</br></p>

    U ziet de resultaten van uw query varken.

    ![Queryresultaten varken][image-pig-query-results]

## <a name="RunMapReduce"></a>Stap 5: Uitvoeren van de taak van een MapReduce met DocumentDB en HDInsight

1. De volgende variabelen instellen in uw PowerShell Script-veld.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name

2. We zullen een taak MapReduce het aantal exemplaren voor elke eigenschap van het Document uit de collectie DocumentDB telt uitvoeren. Dit script fragment toevoegen **nadat** het bovenstaande fragment.

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

    > [AZURE.NOTE] TallyProperties-v01.jar wordt geleverd met de aangepaste installatie van de DocumentDB Hadoop-Connector.

3. Voeg de volgende opdracht om de taak MapReduce indienen.

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    Naast de taakdefinitie MapReduce bieden u ook de naam van het cluster HDInsight waar u de taak MapReduce en de referenties worden uitgevoerd. De Start-AzureHDInsightJob is een asynchrone uitvoering. Gebruik de cmdlet *Wachten AzureHDInsightJob* te controleren de voltooiing van de taak.

4. Voeg de volgende opdracht om te controleren, fouten met de batchverwerking MapReduce.

        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

5. **Voer** het nieuwe script! **Klik op** de knop groen uitvoeren.

6. Controleer de resultaten. Aanmelden bij de [Azure Portal][azure-portal].
    1. Klik op <strong>Bladeren</strong> in het deelvenster links.
    2. Klik op <strong>Alles</strong> rechts boven in het deelvenster bladeren.
    3. Zoek en klik op <strong>Accounts met DocumentDB</strong>.
    4. Zoek vervolgens uw <strong>DocumentDB Account</strong>en vervolgens de <strong>DocumentDB Database</strong> en de <strong>DocumentDB-collectie</strong> die is gekoppeld aan de uitvoer-collectie in de taak MapReduce opgegeven.
    5. Ten slotte klikt u op <strong>Document Explorer</strong> onder <strong>Developer Tools</strong>.

    U ziet de resultaten van uw werk MapReduce.

    ![Queryresultaten MapReduce][image-mapreduce-query-results]

## <a name="NextSteps"></a>Volgende stappen

Gefeliciteerd! U hebt zojuist uw eerste component, varkens en MapReduce taken met Azure, DocumentDB en HDInsight.

We hebben openen die afkomstig zijn onze Hadoop-Connector. Als u geïnteresseerd bent, kunt u contribute op [GitHub][documentdb-github].

Voor meer informatie, Zie de volgende artikelen:

- [Ontwikkelen van een Java-toepassing met Documentdb][documentdb-java-application]
- [MapReduce Java-programma's ontwikkelen voor Hadoop in HDInsight][hdinsight-develop-deploy-java-mapreduce]
- [Aan de slag met Hadoop component in HDInsight voor het analyseren van mobiele telefoon gebruiken][hdinsight-get-started]
- [MapReduce gebruiken met HDInsight][hdinsight-use-mapreduce]
- [Gebruik component met HDInsight][hdinsight-use-hive]
- [Varken met HDInsight gebruiken][hdinsight-use-pig]
- [HDInsight clusters met actie Script aanpassen][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/documentdb-run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[documentdb-hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[documentdb-github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[documentdb-manage-collections]: documentdb-manage.md#Collections
[documentdb-manage-document-storage]: documentdb-manage.md#IndexOverhead
[documentdb-manage-throughput]: documentdb-manage.md#ProvThroughput
[documentdb-import-data]: documentdb-import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md#powershell
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/documentdb-run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: ../powershell-install-configure.md

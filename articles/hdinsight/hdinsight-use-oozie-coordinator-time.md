<properties
    pageTitle="Gebruik van Hadoop Oozie coördinator op basis van tijd in HDInsight | Microsoft Azure"
    description="Hadoop Oozie coördinator op basis van tijd in HDInsight, een grote service gebruiken. Informatie over het definiëren van de Oozie workflows en coördinatoren en taken."
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
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>Oozie coördinator op basis van tijd met Hadoop in HDInsight gebruiken om workflows te definiëren en coördineren van taken

In dit artikel leert u het definiëren van werkstromen en coördinatoren en hoe de taken coördinator, op basis van tijd worden geactiveerd. Het is handig om via het [Gebruik van Oozie met HDInsight] [ hdinsight-use-oozie] voordat u dit artikel leest. Naast het Oozie, kunt u taken met behulp van Azure Data Factory plannen. Azure Data Factory, Zie [Gebruik varken en Data Factory-component](../data-factory/data-factory-data-transformation-activities.md).

> [AZURE.NOTE] Dit artikel is een cluster van Windows-gebaseerde HDInsight vereist. Zie voor meer informatie over het gebruik van Oozie, met inbegrip van taken op basis van tijd op een Linux-gebaseerde cluster [Gebruik Oozie met Hadoop definiëren en uitvoeren van een werkstroom op Linux-gebaseerde HDInsight](hdinsight-use-oozie-linux-mac.md)

##<a name="what-is-oozie"></a>Wat is Oozie

Oozie van Apache is een workflow en coördinatie van systeem die Hadoop taken worden beheerd. Het is geïntegreerd met de stack Hadoop en ondersteunt Hadoop taken voor MapReduce Apache, Apache varken component Apache en Apache Sqoop. Het kan ook worden gebruikt voor het plannen van taken die specifiek voor een systeem, zoals Java-programma's of shell-scripts zijn.

De volgende afbeelding toont de workflow die u wilt implementeren:

![Werkstroomdiagram][img-workflow-diagram]

De werkstroom bevat twee acties:

1. Een component-actie wordt uitgevoerd op een HiveQL-script voor het tellen van elk type niveau voor vastleggen in een logboekbestand log4j. Elk logboek log4j bestaat uit een reeks velden waarin een veld [LOGBOEKNIVEAU] om weer te geven van het type en de ernst, bijvoorbeeld:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    De uitvoer van het script component is vergelijkbaar met:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Zie voor meer informatie over component [Gebruik component met HDInsight][hdinsight-use-hive].

2.  De uitvoer van de actie HiveQL exporteert een Sqoop actie naar een tabel in een Azure SQL-database. Zie voor meer informatie over Sqoop [Met Sqoop met HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Zie voor ondersteunde versies van de Oozie in HDInsight, clusters, [Wat is er nieuw in de cluster-versies die door HDInsight?] [hdinsight-versions].


##<a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Een workstation met Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Een HDInsight-cluster**. Zie voor meer informatie over het maken van een cluster HDInsight [clusters maken HDInsight][hdinsight-provision], of [aan de slag met HDInsight][hdinsight-get-started]. U moet de volgende gegevens om de zelfstudie doorlopen:

    <table border = "1">
    <tr><th>Cluster, eigenschap</th><th>Naam van variabele Windows PowerShell</th><th>Waarde</th><th>Beschrijving</th></tr>
    <tr><td>HDInsight de naam van cluster</td><td>$clusterName</td><td></td><td>De HDInsight cluster waarop u deze zelfstudie wilt uitvoeren.</td></tr>
    <tr><td>HDInsight cluster gebruikersnaam</td><td>$clusterUsername</td><td></td><td>HDInsight gebruikersnaam van het cluster. </td></tr>
    <tr><td>HDInsight cluster gebruikerswachtwoord </td><td>$clusterPassword</td><td></td><td>HDInsight gebruikerswachtwoord van de cluster.</td></tr>
    <tr><td>De naam van Azure opslag</td><td>$storageAccountName</td><td></td><td>Een opslag Azure-account beschikbaar is voor het cluster HDInsight. Voor deze zelfstudie gebruiken de standaard opslag-account die u hebt opgegeven tijdens het verrichten van de cluster.</td></tr>
    <tr><td>Naam van de container Azure Blob</td><td>$containerName</td><td></td><td>In dit voorbeeld gebruikt de Azure Blob opslag container die wordt gebruikt voor het bestandssysteem standaard HDInsight cluster. Deze heeft standaard dezelfde naam als het cluster HDInsight.</td></tr>
    </table>

- **Een Azure SQL-database**. Moet u een firewallregel voor de SQL-databaseserver toegang toegestaan vanaf uw werkstation. Voor instructies over het maken van een Azure SQL database- en [aan de slag met Azure SQL-database]configureren van de firewall, Zie[sqldatabase-get-started]. Dit artikel bevat een Windows PowerShell-script voor het maken van de Azure SQL-databasetabel die u nodig hebt voor deze zelfstudie.

    <table border = "1">
    <tr><th>SQL-database, eigenschap</th><th>Naam van variabele Windows PowerShell</th><th>Waarde</th><th>Beschrijving</th></tr>
    <tr><td>SQL server-databasenaam</td><td>$sqlDatabaseServer</td><td></td><td>De SQL-databaseserver waarnaar Sqoop gegevens worden geëxporteerd. </td></tr>
    <tr><td>Aanmeldingsnaam voor SQL-database</td><td>$sqlDatabaseLogin</td><td></td><td>Aanmeldingsnaam voor SQL-Database.</td></tr>
    <tr><td>Wachtwoord voor SQL-database-aanmelding</td><td>$sqlDatabaseLoginPassword</td><td></td><td>Wachtwoord voor SQL-Database-aanmelding.</td></tr>
    <tr><td>De naam van de SQL-database</td><td>$sqlDatabaseName</td><td></td><td>De Azure SQL-database die Sqoop gegevens worden geëxporteerd. </td></tr>
    </table>

    > [AZURE.NOTE] Standaard staat een Azure SQL-database verbindingen van Azure Services, zoals Azure HDInsight. Als deze firewallinstelling is uitgeschakeld, moet u deze inschakelen via de Portal Azure. Zie voor instructies over het maken van een SQL-Database en configureert firewall-regels [maken en configureren van SQL-Database][sqldatabase-get-started].


> [AZURE.NOTE] Invullen de waarden in de tabellen. Dit is handig voor het doorlopen van deze zelfstudie.


##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Oozie-workflow en het bijbehorende HiveQL-script definiëren

Oozie workflows definities zijn geschreven in hPDL (een XML-proces definition language). De standaardbestandsnaam voor de werkstroom is *workflow.xml*.  U de werkstroom bestand lokaal opslaan en vervolgens op het cluster HDInsight toepassen via Azure PowerShell verderop in deze handleiding.

De actie van de component in de werkstroom wordt een scriptbestand HiveQL. Dit scriptbestand bevat drie HiveQL instructies:

1. **De DROP TABLE-instructie** verwijdert de component log4j tabel als deze bestaat.
2. **De CREATE TABLE-instructie** maakt u een log4j component externe tabel, die naar de locatie van het logboekbestand log4j wijst;
3.  **De locatie van het logboekbestand log4j**. Het veldscheidingsteken is ",". Het scheidingsteken voor de regel is "\n". Een tabel in een externe component wordt gebruikt om te voorkomen dat het bestand wordt verwijderd uit de oorspronkelijke locatie als u wilt dat de werkstroom Oozie meerdere keren uitvoeren.
3. **Instructie OVERSCHRIJVEN de plaats** telt exemplaren van elk type logboek niveau uit de tabel log4j component en de uitvoer wordt opgeslagen op een locatie Azure Blob-opslag.

**Opmerking**: Er is een bekend probleem met het pad component. U wilt uitvoeren in dit probleem bij het indienen van een project Oozie. De instructies voor het oplossen van het probleem kunt u vinden op de TechNet-Wiki: [component HDInsight fout: kan de naam van][technetwiki-hive-error].

**Het bestand HiveQL script worden aangeroepen door de werkstroom definiëren**

1. Maak een tekstbestand met de volgende inhoud:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Er zijn drie variabelen in het script worden gebruikt:

    - ${hiveTableName}
    - ${hiveDataFolder}
    - ${hiveOutputFolder}

    Het definitiebestand workflow (workflow.xml in deze zelfstudie) geven deze waarden met het script HiveQL tijdens runtime.

2. Sla het bestand op als **C:\Tutorials\UseOozie\useooziewf.hql** met behulp van ANSI (ASCII)-codering. (Kladblok gebruiken als uw teksteditor niet deze optie). Dit scriptbestand worden geïmplementeerd met de cluster HDInsight verderop in de handleiding.



**Een werkstroom definiëren**

1. Maak een tekstbestand met de volgende inhoud:

        <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
            <start to = "RunHiveScript"/>

            <action name="RunHiveScript">
                <hive xmlns="uri:oozie:hive-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.job.queue.name</name>
                            <value>${queueName}</value>
                        </property>
                    </configuration>
                    <script>${hiveScript}</script>
                    <param>hiveTableName=${hiveTableName}</param>
                    <param>hiveDataFolder=${hiveDataFolder}</param>
                    <param>hiveOutputFolder=${hiveOutputFolder}</param>
                </hive>
                <ok to="RunSqoopExport"/>
                <error to="fail"/>
            </action>

            <action name="RunSqoopExport">
                <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                    <job-tracker>${jobTracker}</job-tracker>
                    <name-node>${nameNode}</name-node>
                    <configuration>
                        <property>
                            <name>mapred.compress.map.output</name>
                            <value>true</value>
                        </property>
                    </configuration>
                <arg>export</arg>
                <arg>--connect</arg>
                <arg>${sqlDatabaseConnectionString}</arg>
                <arg>--table</arg>
                <arg>${sqlDatabaseTableName}</arg>
                <arg>--export-dir</arg>
                <arg>${hiveOutputFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\001"</arg>
                </sqoop>
                <ok to="end"/>
                <error to="fail"/>
            </action>

            <kill name="fail">
                <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>

           <end name="end"/>
        </workflow-app>

    Er zijn twee acties in de workflow wordt gedefinieerd. De actie start op is *RunHiveScript*. Als de actie wordt uitgevoerd *OK*, is de volgende actie *RunSqoopExport*.

    De RunHiveScript heeft verschillende variabelen. Wanneer u de taak Oozie vanaf uw werkstation met Azure PowerShell indient, wordt u de waarden doorgeven.

    <table border = "1">
    <tr><th>Werkstroomvariabelen</th><th>Beschrijving</th></tr>
    <tr><td>${jobTracker}</td><td>Geef de URL van de taak Hadoop vastleggen. <strong>Jobtrackerhost:9010</strong> op HDInsight cluster versie 3.0 en 2.0 gebruiken.</td></tr>
    <tr><td>${nameNode}</td><td>Geef de URL van het knooppunt van de naam Hadoop. Gebruik de standaard bestand systeem wasbs: / / adres, bijvoorbeeld <i>wasbs: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${Wachtrijnaam}</td><td>Hiermee geeft u de naam van de wachtrij die de taak om te worden verzonden. <strong>Standaard</strong>gebruiken.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Actie variabele component</th><th>Beschrijving</th></tr>
    <tr><td>${hiveDataFolder}</td><td>De bronmap voor de opdracht Create Table component.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>De uitvoermap voor de instructie invoegen OVERSCHRIJVEN.</td></tr>
    <tr><td>${hiveTableName}</td><td>De naam van de component-tabel die verwijst naar de log4j gegevensbestanden.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Actie-variabele Sqoop</th><th>Beschrijving</th></tr>
    <tr><td>${sqlDatabaseConnectionString}</td><td>Verbindingsreeks van SQL-Database.</td></tr>
    <tr><td>${sqlDatabaseTableName}</td><td>De Azure SQL databasetabel waarin de gegevens worden geëxporteerd.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>De uitvoermap voor het overzicht component invoegen OVERSCHRIJVEN. Dit is de map voor de export van Sqoop (export dir).</td></tr>
    </table>

    Zie [Apache Oozie 4.0-documentatie] voor meer informatie over de workflow van de Oozie en met de werkstroomacties,[ apache-oozie-400] (voor HDInsight cluster versie 3.0) of de [documentatie voor Apache Oozie 3.3.2] [ apache-oozie-332] (voor HDInsight cluster versie 2.1).

2. Sla het bestand op als **C:\Tutorials\UseOozie\workflow.xml** met behulp van ANSI (ASCII)-codering. (Kladblok gebruiken als uw teksteditor niet deze optie).

**Coördinator definiëren**

1. Maak een tekstbestand met de volgende inhoud:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
           <action>
              <workflow>
                 <app-path>${wfPath}</app-path>
              </workflow>
           </action>
        </coordinator-app>

    Er zijn vijf variabelen in het definitiebestand wordt gebruikt:

  	| Variabele          | Beschrijving |
  	| ------------------|------------ |
  	| ${coordFrequency} | Tijd voor taak onderbreken. Frequentie wordt altijd uitgedrukt in minuten. |
  	| ${coordStart}     | Begintijd taak. |
  	| ${coordEnd}       | De eindtijd van de taak. |
  	| ${coordTimezone}  | Taken van de coördinator in de tijdzone van een vaste verwerkt Oozie met geen zomertijd (meestal weergegeven met behulp van UTC). Deze tijdzone wordt verwezen als de "Oozie verwerking timezone." |
  	| ${wfPath}         | Het pad voor de workflow.xml.  Als de naam van de werkstroom niet de standaardbestandsnaam (workflow.xml), moet u deze opgeven. |

2. Sla het bestand op als **C:\Tutorials\UseOozie\coordinator.xml** met behulp van de codering van ANSI (ASCII). (Kladblok gebruiken als uw teksteditor niet deze optie).

##<a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>Het project Oozie implementeren en voorbereiden van de zelfstudie

U kunt een Azure PowerShell scripts uitvoeren voor de volgende wordt uitgevoerd:

- Kopieer het script HiveQL (useoozie.hql) naar Azure Blob-opslag, wasbs:///tutorials/useoozie/useoozie.hql.
- Kopieer workflow.xml naar wasbs:///tutorials/useoozie/workflow.xml.
- Kopieer coordinator.xml naar wasbs:///tutorials/useoozie/coordinator.xml.
- Kopieer het bestand (/ example/data/sample.log) op wasbs:///tutorials/useoozie/data/sample.log.
- Maak een tabel Azure SQL database voor het opslaan van gegevens voor Sqoop exporteren. De naam is *log4jLogCount*.

**Opslag HDInsight begrijpen**

Azure Blob-opslag HDInsight gebruikt voor gegevensopslag. wasbs: / / is de implementatie van Microsoft van Hadoop distributed file system (HDFS) in Azure Blob-opslag. Voor meer informatie, Zie [Gebruik Azure Blob-opslag met HDInsight][hdinsight-storage].

Wanneer u een cluster HDInsight inrichten, wordt een account Azure Blob-opslag en een specifieke container van de account aangewezen als het standaardbestandssysteem, zoals in HDFS. Naast deze account opslag kunt u extra opslagruimte accounts toevoegen uit de dezelfde Azure abonnement of andere Azure abonnementen tijdens het inrichten. Zie voor instructies over het toevoegen van extra opslagruimte accounts, [clusters bepaling HDInsight][hdinsight-provision]. Om te vereenvoudigen het Azure PowerShell script in deze zelfstudie wordt gebruikt, worden alle bestanden opgeslagen in de standaardcontainer voor systeem te vinden op */tutorials/useoozie*. Deze container heeft standaard dezelfde naam als de naam van het cluster HDInsight.
De syntaxis is:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [AZURE.NOTE] Alleen de *wasb: / /* -syntaxis wordt ondersteund in HDInsight cluster versie 3.0. De oudere *luchtverzadigingswaarde: / /* -syntaxis wordt ondersteund in HDInsight 2.1 en 1,6 clusters, maar wordt niet ondersteund in clusters HDInsight 3.0.

> [AZURE.NOTE] De wasb: / / pad is een virtueel pad. Zie voor meer informatie [Gebruik Azure Blob-opslag met HDInsight][hdinsight-storage].

Een bestand dat is opgeslagen in de systeemcontainer van standaard bestand toegankelijk is vanuit HDInsight met behulp van een van de volgende URLs (ik gebruik workflow.xml als voorbeeld):

    wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasbs:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Als u toegang tot het bestand rechtstreeks vanuit de opslag wilt, is de naam voor het bestand van de blob:

    tutorials/useoozie/workflow.xml

**De interne en externe tabellen component begrijpen**

Er zijn een paar dingen die u moet weten over de interne en externe tabellen component:

- De opdracht CREATE TABLE maakt een interne tabel, ook bekend als een beheerde. Het gegevensbestand moet zich bevinden in de standaardcontainer.
- De opdracht CREATE TABLE wordt het bestand verplaatst naar de /hive/magazijn/<TableName> map in de standaardcontainer.
- De opdracht externe tabel maken maakt u een externe tabel. Het bestand kan zich bevinden buiten de standaardcontainer.
- De opdracht externe tabel maken verplaatst het bestand niet.
- De opdracht externe tabel maken is niet mogelijk submappen onder de map die is opgegeven in de component voor de locatie. Dit is de reden waarom de zelfstudie een kopie van het bestand sample.log wordt.

Zie voor meer informatie [HDInsight: component interne en externe tabellen Intro][cindygross-hive-tables].

**Voorbereiden van de zelfstudie**

1. Open de Windows PowerShell ISE (in het Windows 8 startscherm, typ **PowerShell_ISE**en klik op **Windows PowerShell ISE**. Zie voor meer informatie, [Start Windows PowerShell in Windows 8 en Windows][powershell-start]).
2. In het onderste deelvenster de volgende opdracht uit te verbinden met uw Azure abonnement:

        Add-AzureAccount

    U wordt gevraagd uw accountreferenties Azure-in te voeren. Deze methode van het toevoegen van een abonnement verbinding time-out en na 12 uur hebt u de cmdlet opnieuw uitvoeren.

    > [AZURE.NOTE] Als er meerdere Azure abonnementen en het standaardabonnement is niet het bestand dat u wilt gebruiken, selecteer een abonnement met behulp van de cmdlet <strong>Selecteren AzureSubscription</strong> .

3. Kopieer het volgende script in het script-veld, en stel vervolgens de eerste zes variabelen:

        # WASB variables
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<BlobStorageContainerName>"

        # SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"  
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  
        $sqlDatabaseTableName = "log4jLogsCount"

        # Oozie files for the tutorial
        $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
        $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
        $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

        # WASB folder for storing the Oozie tutorial files.
        $destFolder = "tutorials/useoozie"  # Do NOT use the long path here


    Zie de sectie [vereisten](#prerequisites) voor meer beschrijvingen van de variabelen in deze zelfstudie.

3. Het volgende toevoegen aan het script in het script-veld:

        # Create a storage context object
        $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        function uploadOozieFiles()
        {
            Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
            Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
            Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
            Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
        }

        function prepareHiveDataFile()
        {
            Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
            Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
        }

        function prepareSQLDatabase()
        {
            # SQL query string for creating log4jLogsCount table
            $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [Level] [nvarchar](10) NOT NULL,
                    [Total] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                [Level] ASC
                )
                )"

            #Create the log4jLogsCount table
            Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
            $conn = New-Object System.Data.SqlClient.SqlConnection
            $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
            $conn.open()
            $cmd = New-Object System.Data.SqlClient.SqlCommand
            $cmd.connection = $conn
            $cmd.commandtext = $cmdCreateLog4jCountTable
            $cmd.executenonquery()

            $conn.close()
        }

        # upload workflow.xml, coordinator.xml, and ooziewf.hql
        uploadOozieFiles;

        # make a copy of example/data/sample.log to example/data/log4j/sample.log
        prepareHiveDataFile;

        # create log4jlogsCount table on SQL database
        prepareSQLDatabase;

4. Klik op **Script uitvoeren** of druk op **F5** om het script uitvoert. De uitvoer is vergelijkbaar met:

    ![Zelfstudie voorbereiding uitvoer][img-preparation-output]

##<a name="run-the-oozie-project"></a>Uitvoeren van het project Oozie

Azure PowerShell biedt momenteel niet alle cmdlets voor het definiëren van Oozie taken. U kunt de cmdlet **Invoke RestMethod** Oozie webservices aanroepen. De Oozie web services API is een HTTP-REST JSON-API. Zie voor meer informatie over de Oozie webservices API [documentatie voor Apache Oozie 4.0] [ apache-oozie-400] (voor HDInsight cluster versie 3.0) of de [documentatie voor Apache Oozie 3.3.2] [ apache-oozie-332] (voor HDInsight cluster versie 2.1).

**Een Oozie-taak verzenden**

1. Open de Windows PowerShell ISE (in Windows 8 startscherm, typ **PowerShell_ISE**en klik op **Windows PowerShell ISE**. Zie voor meer informatie, [Start Windows PowerShell in Windows 8 en Windows][powershell-start]).

3. Kopieer het volgende script in het script-veld, en stel vervolgens de eerste veertien variabelen (echter **$storageUri**overslaan).

        #HDInsight cluster variables
        $clusterName = "<HDInsightClusterName>"
        $clusterUsername = "<HDInsightClusterUsername>"
        $clusterPassword = "<HDInsightClusterUserPassword>"

        #Azure Blob storage (WASB) variables
        $storageAccountName = "<StorageAccountName>"
        $storageContainerName = "<BlobContainerName>"
        $storageUri="wasbs://$storageContainerName@$storageAccountName.blob.core.windows.net"

        #Azure SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  

        #Oozie WF/coordinator variables
        $coordStart = "2014-03-21T13:45Z"
        $coordEnd = "2014-03-21T13:45Z"
        $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
        $coordTimezone = "UTC"  #UTC/GMT

        $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
        $waitTimeBetweenOozieJobStatusCheck=10

        #Hive action variables
        $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
        $hiveTableName = "log4jlogs"
        $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
        $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

        #Sqoop action variables
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
        $sqlDatabaseTableName = "log4jLogsCount"

        $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
        $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    Zie de sectie [vereisten](#prerequisites) voor meer beschrijvingen van de variabelen in deze zelfstudie.

    $coordstart en $coordend zijn de workflow van begin- en eindtijd. Als u wilt weten wat de UTC/GMT-tijd, zoeken "utc-tijd" in bing.com. De $coordFrequency is hoe vaak in minuten moet worden uitgevoerd van de werkstroom.

3. Het volgende toevoegen aan het script. Dit deel wordt de nettolading Oozie gedefinieerd:

        #OoziePayload used for Oozie web service submission
        $OoziePayload =  @"
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

           <property>
               <name>nameNode</name>
               <value>$storageUrI</value>
           </property>

           <property>
               <name>jobTracker</name>
               <value>jobtrackerhost:9010</value>
           </property>

           <property>
               <name>queueName</name>
               <value>default</value>
           </property>

           <property>
               <name>oozie.use.system.libpath</name>
               <value>true</value>
           </property>

           <property>
               <name>oozie.coord.application.path</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>wfPath</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>coordStart</name>
               <value>$coordStart</value>
           </property>

           <property>
               <name>coordEnd</name>
               <value>$coordEnd</value>
           </property>

           <property>
               <name>coordFrequency</name>
               <value>$coordFrequency</value>
           </property>

           <property>
               <name>coordTimezone</name>
               <value>$coordTimezone</value>
           </property>

           <property>
               <name>hiveScript</name>
               <value>$hiveScript</value>
           </property>

           <property>
               <name>hiveTableName</name>
               <value>$hiveTableName</value>
           </property>

           <property>
               <name>hiveDataFolder</name>
               <value>$hiveDataFolder</value>
           </property>

           <property>
               <name>hiveOutputFolder</name>
               <value>$hiveOutputFolder</value>
           </property>

           <property>
               <name>sqlDatabaseConnectionString</name>
               <value>&quot;$sqlDatabaseConnectionString&quot;</value>
           </property>

           <property>
               <name>sqlDatabaseTableName</name>
               <value>$SQLDatabaseTableName</value>
           </property>

           <property>
               <name>user.name</name>
               <value>admin</value>
           </property>

        </configuration>
        "@

    >[AZURE.NOTE] Het belangrijkste verschil ten opzichte van de werkstroom indienen payload-bestand, wordt de variabele **oozie.coord.application.path**. Als u een werkstroomtaak hebt verzonden, gebruikt u **oozie.wf.application.path** in plaats daarvan.

4. Het volgende toevoegen aan het script. Dit deel controleert de status van de webpagina Oozie:

        function checkOozieServerStatus()
        {
            Write-Host "Checking Oozie server status..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieServerSatus = $jsonResponse[0].("systemMode")
            Write-Host "Oozie server status is $oozieServerSatus..."

            if($oozieServerSatus -notmatch "NORMAL")
            {
                Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
                exit 1
            }
        }

5. Het volgende toevoegen aan het script. Dit deel maakt u een taak van de Oozie:

        function createOozieJob()
        {
            # create Oozie job
            Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
            Write-Host "`n--------`n$OoziePayload`n--------"
            $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieJobId = $jsonResponse[0].("id")
            Write-Host "Oozie job id is $oozieJobId..."

            return $oozieJobId
        }

    > [AZURE.NOTE] Wanneer u een werkstroomtaak indient, moet u een andere webservice aanroepen om de taak te starten nadat de taak is gemaakt. In dit geval wordt de taak van de coördinator geactiveerd door de tijd. De taak wordt automatisch gestart.

6. Het volgende toevoegen aan het script. Dit deel wordt de status van de Oozie gecontroleerd:

        function checkOozieJobStatus($oozieJobId)
        {
            # get job status
            Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

            Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
            $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")

            while($JobStatus -notmatch "SUCCEEDED|KILLED")
            {
                Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
                Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
                $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
                $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
                $JobStatus = $jsonResponse[0].("status")
            }

            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
            if($JobStatus -notmatch "SUCCEEDED")
            {
                Write-Host "Check logs at http://headnode0:9014/cluster for detais."
                exit -1
            }
        }

7. (Optioneel) Het volgende toevoegen aan het script.

        function listOozieJobs()
        {
            Write-Host "Listing Oozie jobs..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

            write-host "Job ID                                   App Name        Status      Started                         Ended"
            write-host "----------------------------------------------------------------------------------------------------------------------------------"
            foreach($job in $response.workflows)
            {
                Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
            }
        }

        function ShowOozieJobLog($oozieJobId)
        {
            Write-Host "Showing Oozie job info..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
            write-host $response
        }

        function killOozieJob($oozieJobId)
        {
            Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
            $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
            $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
        }

7. Het volgende toevoegen aan het script:

        checkOozieServerStatus
        # listOozieJobs
        $oozieJobId = createOozieJob($oozieJobId)
        checkOozieJobStatus($oozieJobId)
        # ShowOozieJobLog($oozieJobId)
        # killOozieJob($oozieJobId)

    De # tekens verwijderen als u wilt dat de extra functies uitvoeren.

7. Als het cluster HDinsight versie 2.1, vervangen door "https://$clusterName.azurehdinsight.net:443/oozie/v2/" met "https://$clusterName.azurehdinsight.net:443/oozie/v1/". HDInsight cluster versie 2.1 biedt geen ondersteunt versie 2 van de webservices.

7. Klik op **Script uitvoeren** of druk op **F5** om het script uitvoert. De uitvoer is vergelijkbaar met:

    ![Zelfstudie uitvoeren workflow uitvoer][img-runworkflow-output]

8. Verbinding maken met de SQL-Database voor een overzicht van de geëxporteerde gegevens.

**Het foutenlogboek van de taak controleren**

Voor het oplossen van een werkstroom, het logboekbestand Oozie vindt u op C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log van de headnode van het cluster. Zie voor informatie over RDP, [HDInsight beheer van clusters met de Azure portal][hdinsight-admin-portal].

**De zelfstudie opnieuw uitvoeren**

Als de werkstroom opnieuw uit te voeren, moet u de volgende taken uitvoeren:

- Het onderdeel script uitvoerbestand verwijderen.
- Verwijder de gegevens in de tabel log4jLogsCount.

Hier volgt een voorbeeld Windows PowerShell-script dat u kunt gebruiken:

    $storageAccountName = "<AzureStorageAccountName>"
    $containerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()


##<a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd het definiëren van een workflow voor Oozie en de coördinator van een Oozie en het uitvoeren van een taak van de coördinator Oozie met Azure PowerShell. Voor meer informatie, Zie de volgende artikelen:

- [Aan de slag met HDInsight][hdinsight-get-started]
- [Azure Blob-opslag gebruiken met HDInsight][hdinsight-storage]
- [HDInsight met Azure PowerShell beheren][hdinsight-admin-powershell]
- [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
- [Gebruik Sqoop met HDInsight][hdinsight-use-sqoop]
- [Gebruik component met HDInsight][hdinsight-use-hive]
- [Varken met HDInsight gebruiken][hdinsight-use-pig]
- [MapReduce Java-programma's ontwikkelen voor HDInsight][hdinsight-develop-java-mapreduce]



[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563


[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png  

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

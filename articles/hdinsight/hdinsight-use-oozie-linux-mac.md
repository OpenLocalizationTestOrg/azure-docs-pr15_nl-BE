<properties
    pageTitle="Gebruik van Hadoop Oozie workflows in Linux-gebaseerde HDInsight | Microsoft Azure"
    description="Hadoop Oozie in Linux-gebaseerde HDInsight gebruiken. Informatie over het definiëren van een workflow voor Oozie en een Oozie-taak."
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


# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-on-linux-based-hdinsight"></a>Oozie met Hadoop gebruiken voor het definiëren en uitvoeren van een werkstroom op Linux-gebaseerde HDInsight

[AZURE.INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Leren Apache Oozie gebruiken voor het definiëren van een werkstroom die gebruikmaakt van component- en Sqoop en vervolgens de werkstroom alleen uitvoeren op een cluster van Linux-gebaseerde HDInsight.

Oozie van Apache is een workflow en coördinatie van systeem die Hadoop taken worden beheerd. Het is geïntegreerd met de stack Hadoop en ondersteunt Hadoop taken voor MapReduce Apache, Apache varken component Apache en Apache Sqoop. Ook kan worden gebruikt voor het plannen van taken die specifiek voor een systeem, zoals Java-programma's of shell-scripts zijn

> [AZURE.NOTE] Een andere optie voor het definiëren van workflows met HDInsight is Azure Data Factory. Zie voor meer informatie over Azure Data Factory, [Gebruik varken en Data Factory-component][azure-data-factory-pig-hive].

##<a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Abonnement op een Azure**: Zie [gratis proefversie ophalen Azure](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI**: Zie [installeren en configureren van de Azure CLI](../xplat-cli-install.md)
    
    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- **Een HDInsight cluster**: Zie [Aan de slag met HDInsight op Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Een Azure SQL-database**: dit wordt gemaakt met behulp van de stappen in dit document

##<a name="example-workflow"></a>Voorbeeld van de workflow

De werkstroom die u implementeren wilt door de instructies in dit document omvat twee acties. Acties zijn de definities voor taken zoals het uitvoeren van de component, Sqoop, MapReduce of andere processen:

![Werkstroomdiagram][img-workflow-diagram]

1. Een component-actie wordt uitgevoerd op een script HiveQL records ophalen uit de **hivesampletable** met HDInsight opgenomen. Beschrijving van elke rij met gegevens een bezoek van een bepaald mobiel apparaat. De recordindeling weergegeven met de volgende strekking:

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    Het onderdeel script gebruikt in dit document telt het totaal aantal bezoeken voor elk platform (zoals Android of iPhone) en de tellingen om een nieuwe component-tabel wordt opgeslagen.

    Zie voor meer informatie over component [Gebruik component met HDInsight][hdinsight-use-hive].

2.  Een Sqoop actie exporteert de inhoud van de nieuwe tabel in de component aan een tabel in een Azure SQL-database. Zie voor meer informatie over Sqoop, [Gebruik Hadoop-Sqoop met HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Zie voor ondersteunde versies van de Oozie in HDInsight, clusters, [Wat is er nieuw in de versies van Hadoop cluster geleverd door HDInsight?] [hdinsight-versions].

##<a name="create-the-working-directory"></a>De werkmap maken

Oozie verwacht dat bronnen die nodig zijn voor een project in dezelfde map worden opgeslagen. In dit voorbeeld wordt **wasbs: / / / zelfstudies/useoozie**. Gebruik de volgende opdracht voor het maken van deze map en de map data waarin u de nieuwe component-tabel die door deze werkstroom zijn gemaakt:

    hdfs dfs -mkdir -p /tutorials/useoozie/data

> [AZURE.NOTE] De `-p` parameter met betrekking tot alle mappen in het pad worden gemaakt als deze nog niet bestaan. De map **data** wordt gebruikt voor het opslaan van gegevens die worden gebruikt door het script **useooziewf.hql** .

Ook de volgende opdracht, die ervoor zorgt dat Oozie uw gebruikersaccount imiteren kan bij het uitvoeren van taken voor component- en Sqoop uitvoeren. Vervang **USERNAME** door uw aanmeldingsnaam:

    sudo adduser USERNAME users

Als er een foutbericht dat de gebruiker al lid van de gebruikers is, kunt u deze gewoon negeren.

##<a name="add-a-database-driver"></a>Een databasestuurprogramma toevoegen

Aangezien deze werkstroom wordt Sqoop gebruikt om gegevens te exporteren naar een SQL-Database, moet u een kopie van de JDBC-stuurprogramma gebruikt om te communiceren met een SQL-Database opgeven. Gebruik de volgende opdracht om het te kopiëren naar de werkmap:

    hdfs dfs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/

Als de werkstroom andere bronnen, zoals een Weckpot met een MapReduce-toepassing gebruikt, moet u deze ook toevoegen.

##<a name="define-the-hive-query"></a>De component query definiëren

Gebruik de volgende stappen voor het maken van een HiveQL script definieert u een query, in een workflow Oozie verderop in dit document wordt gebruikt.

1. SSH verbinding maken met het cluster Linux-gebaseerde HDInsight gebruiken:

    * **Linux, Unix of OS X-clients**: Zie [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Linux, OS X en Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-clients**: Zie [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Gebruik de volgende opdracht om een nieuw bestand maken:

        nano useooziewf.hql

1. Zodra de nano editor wordt geopend, gebruikt u het volgende als de inhoud van het bestand:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
        FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;

    Er zijn twee variabelen in het script worden gebruikt:

    - **${hiveTableName}**: bevat de naam van de tabel moet worden gemaakt
    - **${hiveDataFolder}**: bevat de opslaglocatie voor de gegevensbestanden voor de tabel

    Het definitiebestand workflow (workflow.xml in deze zelfstudie) geeft deze waarden met het script HiveQL tijdens runtime.

2. Druk op Ctrl-X om af te sluiten van de editor. Wanneer dat wordt gevraagd, selecteer **Y** het bestand wilt opslaan en klik **Enter** naar de naam van het **useooziewf.hql** gebruiken.

3. De volgende opdrachten gebruiken om te kopiëren van **useooziewf.hql** naar **wasbs:///tutorials/useoozie/useooziewf.hql**:

        hdfs dfs -copyFromLocal useooziewf.hql /tutorials/useoozie/useooziewf.hql

    Deze opdrachten opslaan van het bestand **useooziewf.hql** op de Azure opslag-account die is gekoppeld aan dit cluster waar het bestand behouden blijft, zelfs als het cluster is verwijderd. Hiermee kunt u geld besparen door clusters verwijderen wanneer deze niet in gebruik, terwijl uw taken en werkstromen.

##<a name="define-the-workflow"></a>De werkstroom definiëren

Oozie workflows definities zijn geschreven in hPDL (een XML-proces Definition Language). Gebruik de volgende stappen kunt u de werkstroom definiëren:

1. De volgende instructie gebruiken voor het maken en bewerken van een nieuw bestand:

        nano workflow.xml

1. Als eenmaal de nano editor geopend, typ het volgende als inhoud van het bestand:

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
                <arg>${hiveDataFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\t"</arg>
                <archive>sqljdbc41.jar</archive>
                </sqoop>
            <ok to="end"/>
            <error to="fail"/>
            </action>
            <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>
            <end name="end"/>
        </workflow-app>

    Er zijn twee acties in de workflow wordt gedefinieerd:

    - **RunHiveScript**: dit is de startactie en **useooziewf.hql** component script wordt uitgevoerd

    - **RunSqoopExport**: zo exporteert u de gegevens die zijn gemaakt op basis van het script component SQL-database met behulp van Sqoop. Dit kan alleen worden uitgevoerd als de actie **RunHiveScript** gelukt is.

        > [AZURE.NOTE] Zie [Apache Oozie 4.0-documentatie] voor meer informatie over de workflow van de Oozie en het gebruik van workflowacties,[ apache-oozie-400] (voor HDInsight versie 3.0) of de [documentatie voor Apache Oozie 3.3.2] [ apache-oozie-332] (voor HDInsight versie 2.1).

    De werkstroom heeft verschillende posten, zoals `${jobTracker}`, die zal worden vervangen door waarden die u in de taakdefinitie verderop in dit document.

    Ook de `<archive>sqljdbc4.jar</arcive>` vermelding in de sectie Sqoop. Dit geeft de opdracht Oozie dit archief beschikbaar maken voor Sqoop wanneer deze actie wordt uitgevoerd.

2. Gebruik Ctrl-X en **Y** en op **Enter** in het bestand wilt opslaan.

3. Gebruik de volgende opdracht in het bestand **workflow.xml** te kopiëren naar **wasbs:///tutorials/useoozie/workflow.xml**:

        hdfs dfs -copyFromLocal workflow.xml /tutorials/useoozie/workflow.xml

##<a name="create-the-database"></a>De database maken

Volg de stappen in het document [een SQL-Database maken](../sql-database/sql-database-get-started.md) in een nieuwe database te maken. Wanneer u de database maakt, gebruikt u __oozietest__ als naam van de database. Maak een notitie van de naam van de databaseserver, ook als dit nodig in de volgende sectie is.

###<a name="create-the-table"></a>De tabel maken

> [AZURE.NOTE] Er zijn tal van manieren verbinding maken met een SQL-Database een tabel te maken. De volgende stappen gebruikt [FreeTDS](http://www.freetds.org/) uit het cluster HDInsight.

3. Gebruik de volgende opdracht FreeTDS installeren op de cluster HDInsight:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Eenmaal FreeTDS is geïnstalleerd, gebruikt u de volgende opdracht verbinding maken met de SQL-databaseserver die u eerder hebt gemaakt:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest

    Uitvoer van de volgende strekking wordt weergegeven:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

5. Op de `1>` wordt gevraagd, voert u de volgende regels:

        CREATE TABLE [dbo].[mobiledata](
        [deviceplatform] [nvarchar](50),
        [count] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
        GO

    Wanneer de `GO` instructie is ingevoerd, wordt de vorige overzichten worden geëvalueerd. Hiermee maakt u een nieuwe tabel met de naam **mobiledata** wordt vastgelegd in Sqoop.

    Om te controleren of de tabel is gemaakt, gebruikt u het volgende:

        SELECT * FROM information_schema.tables
        GO

    Uitvoer van de volgende strekking worden weergegeven:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        oozietest       dbo     mobiledata      BASE TABLE

8. Voer `exit` op de `1>` vragen om het hulpprogramma tsql af te sluiten.

##<a name="create-the-job-definition"></a>De taakdefinitie maken

De taakdefinitie wordt beschreven waar vind ik de workflow.xml, alsmede van andere bestanden die worden gebruikt door de werkstroom (bijvoorbeeld useooziewf.hql). Ook Hiermee definieert u de waarden voor eigenschappen binnen de werkstroom gebruikt en de bijbehorende bestanden.

1. Gebruik de volgende opdracht om het volledige adres van de WASB naar de standaard opslag. Deze wordt gebruikt in het configuratiebestand een moment:

        sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml

    Dit moet geven als resultaat informatie met de volgende strekking:

        <name>fs.defaultFS</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>

    Sla de **wasbs://mycontainer@mystorageaccount.blob.core.windows.net** -waarde, zoals deze wordt gebruikt in de volgende stappen.

2. Gebruik de volgende opdracht om de FQDN-naam van het cluster headnode. Dit wordt gebruikt voor het JobTracker adres van het cluster. Deze wordt gebruikt in het configuratiebestand een moment:

        hostname -f

    Het resultaat is informatie met de volgende strekking:

        hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net

    De poort die wordt gebruikt voor de JobTracker is 8050, zodat het volledige adres moet worden gebruikt voor de JobTracker **hn0 CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050**.

1. Gebruik de volgende definitie van de configuratie van de Oozie te maken:

        nano job.xml

2. Zodra de nano editor wordt geopend, gebruikt u het volgende als de inhoud van het bestand:

        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

          <property>
            <name>nameNode</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>
          </property>

          <property>
            <name>jobTracker</name>
            <value>JOBTRACKERADDRESS</value>
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
            <name>hiveScript</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
          </property>

          <property>
            <name>hiveTableName</name>
            <value>mobilecount</value>
          </property>

          <property>
            <name>hiveDataFolder</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
          </property>

          <property>
            <name>sqlDatabaseConnectionString</name>
            <value>"jdbc:sqlserver://serverName.database.windows.net;user=adminLogin;password=adminPassword;database=oozietest"</value>
          </property>

          <property>
            <name>sqlDatabaseTableName</name>
            <value>mobiledata</value>
          </property>

          <property>
            <name>user.name</name>
            <value>YourName</value>
          </property>

          <property>
            <name>oozie.wf.application.path</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
          </property>
        </configuration>

    * Vervang alle exemplaren van **wasbs://mycontainer@mystorageaccount.blob.core.windows.net** met de waarde die u eerder hebt ontvangen.

    > [AZURE.WARNING] Met de account van de container- en als onderdeel van het pad, moet u het volledige pad van de WASB gebruiken. Met de korte notatie (wasbs: / / /), wordt de actie RunHiveScript is mislukt wanneer de taak wordt gestart.

    * Vervang **JOBTRACKERADDRESS** door de JobTracker/ResourceManager-adres dat u eerder hebt ontvangen.

    * **Uwnaam** vervangen door uw aanmeldingsnaam voor het cluster HDInsight.

    * **Servernaam**en **adminLogin** **adminPassword** vervangen door de gegevens voor de Azure SQL-Database.

    De meeste informatie in dit bestand wordt gebruikt voor het vullen van de waarden in de workflow.xml of ooziewf.hql-bestanden (zoals ${nameNode}.)

    > [AZURE.NOTE] De vermelding **oozie.wf.application.path** bepaalt waar u het bestand workflow.xml te vinden die de workflow bevat deze taak is uitgevoerd.

2. Gebruik Ctrl-X en **Y** en op **Enter** in het bestand wilt opslaan.

##<a name="submit-and-manage-the-job"></a>Indienen en beheren van de taak

De volgende stappen gebruikt de opdracht Oozie indienen en beheren van workflows voor Oozie op het cluster. De opdracht Oozie is een gebruiksvriendelijke interface via de [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

> [AZURE.IMPORTANT] Wanneer u de opdracht Oozie, moet u de FQDN-naam voor de headnode HDInsight. Deze FQDN is alleen toegankelijk vanuit het cluster, of als het cluster zich op een virtueel netwerk van Azure vanaf andere computers op hetzelfde netwerk.

1. Ophalen van de URL van de Oozie-service, gebruikt u het volgende:

        sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml

    Dit resulteert in een waarde met de volgende strekking:

        <name>oozie.base.url</name>
        <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>

    Het gedeelte van de **http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie** is de URL te gebruiken met de opdracht Oozie.

2. Een variabele maken voor de URL, zodat u niet hoeft te typen voor elke opdracht, gebruikt u het volgende:

        export OOZIE_URL=http://HOSTNAMEt:11000/oozie

    De URL vervangen door een die u eerder hebt ontvangen.

3. Gebruik het volgende in te dienen van het project:

        oozie job -config job.xml -submit

    Dit laadt de taakgegevens van **job.xml** en dient ze Oozie, maar wordt niet uitgevoerd.

    Nadat de opdracht is voltooid, moet deze uitvoer retourneren de ID van de taak. Bijvoorbeeld `0000005-150622124850154-oozie-oozi-W`. Deze wordt gebruikt voor het beheren van de taak.

4. Bekijk de status van de taak met de volgende opdracht. Voer de taak-ID die wordt geretourneerd door de vorige opdracht:

        oozie job -info <JOBID>

    Hiermee herstelt u informatie met de volgende strekking.

        Job ID : 0000005-150622124850154-oozie-oozi-W
        ------------------------------------------------------------------------------------------------------------------------------------
        Workflow Name : useooziewf
        App Path      : wasbs:///tutorials/useoozie
        Status        : PREP
        Run           : 0
        User          : USERNAME
        Group         : -
        Created       : 2015-06-22 15:06 GMT
        Started       : -
        Last Modified : 2015-06-22 15:06 GMT
        Ended         : -
        CoordAction ID: -
        ------------------------------------------------------------------------------------------------------------------------------------

    Deze taak heeft de status van `PREP`, wat aangeeft dat deze is ingediend, maar nog niet is gestart.

4. Om de taak te starten, gebruikt u het volgende:

        oozie job -start JOBID

    Als u de status nadat u deze opdracht hebt uitgevoerd controleren, worden uitgevoerd en wordt informatie geretourneerd voor de acties binnen de taak.

5. Zodra de taak is voltooid, kunt u controleren dat de gegevens is gegenereerd en naar de SQL-Database tabel geëxporteerd met behulp van de volgende opdrachten:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest

    Op de `1>` wordt gevraagd, typt u het volgende:

        SELECT * FROM mobiledata
        GO

    U ontvangt de volgende informatie:

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

Zie voor meer informatie over de opdracht Oozie [Opdrachtregelprogramma Oozie](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).

##<a name="oozie-rest-api"></a>Oozie REST API

De REST Oozie API kunt u bouw uw eigen hulpprogramma's die met Oozie werken. Specifieke informatie over het gebruik van de API Oozie overige HDInsight zijn:

* **URI**: de REST API toegankelijk van buiten het cluster op`https://CLUSTERNAME.azurehdinsight.net/oozie`

* **Verificatie**: de API de HTTP-clusteraccount (admin) en wachtwoord moet worden geverifieerd. Bijvoorbeeld:

        curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions

Zie voor meer informatie over het gebruik van de API Oozie REST [Oozie Web Services API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

##<a name="oozie-web-ui"></a>Oozie Web UI

De Oozie Web interface biedt een webweergave in de status van de taken van de Oozie op het cluster. U kunt de status, de taakdefinitie, configuratie, een grafiek van de acties in de taak en de logboeken van de taak weergeven. U kunt ook details voor acties in een taak weergeven.

Voor toegang tot de on line gebruikersinterface Oozie, gebruikt u de volgende stappen uit:

1. Maak een SSH-tunnel met het cluster HDInsight. Zie voor informatie over hoe u dit doet, [Gebruik SSH Tunneling Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, en andere web UI van toegang tot](hdinsight-linux-ambari-ssh-tunnel.md).

2. Nadat een tunnel is gemaakt, opent u het web Ambari UI in uw webbrowser. De URI voor de site Ambari is **https://CLUSTERNAME.azurehdinsight.net**. **CLUSTERNAAM** vervangen door de naam van het cluster Linux-gebaseerde HDInsight.

3. Selecteren vanaf de linkerkant van de pagina, **Oozie**, en vervolgens **Snelkoppelingen**en ten slotte **Oozie Web UI**.

    ![afbeelding van de menu 's](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. De Oozie Web interface standaard uitgevoerd werkstroomtaken weergeven. Selecteer **Alle taken**overzicht van alle projecten van de werkstroom.

    ![Alle taken die worden weergegeven](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. Selecteer een taak voor meer informatie over de taak weergeven.

    ![Job-info](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. Op het tabblad taak Info ziet u algemene functie-informatie, alsmede de individuele acties binnen het project. Met behulp van de tabbladen boven kunt u de taakdefinitie, configuratie, toegang tot het logboek van de taak weergeven of een gestuurde acyclische grafiek (DAG) van de taak weergeven.

    * **Job-logbestand**: klik op de knop **GetLogs** voor alle logboeken voor de taak of het veld **Invoeren zoekfilter** gebruiken om Logboeken filteren

        ![Job-logbestand](./media/hdinsight-use-oozie-linux-mac/joblog.png)

    * **JobDAG**: de DAG is een grafisch overzicht van de paden genomen via de workflow

        ![Taak DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. Selecteren van een van de acties van het tabblad **Taak Info** weergeven voor de actie. Selecteer bijvoorbeeld de actie **RunHiveScript** .

    ![Actie info](./media/hdinsight-use-oozie-linux-mac/action.png)

8. Hier ziet u de details voor de actie, inclusief een koppeling naar de **URL van de Console**, die kan worden gebruikt om JobTracker informatie voor de taak te bekijken.

##<a name="scheduling-jobs"></a>Taken plannen

De coördinator kunt u een begin, einde en frequentie van voorkomen van taken opgeven, zodat deze kunnen worden ingepland voor bepaalde tijden.

Gebruik een schema voor de werkstroom definiëren de volgende stappen uit:

1. Maak een nieuw bestand met de naam **coordinator.xml**gebruikt u het volgende:

        nano coordinator.xml

    Als de inhoud van het bestand, gebruikt u het volgende:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
          <action>
            <workflow>
              <app-path>${workflowPath}</app-path>
            </workflow>
          </action>
        </coordinator-app>

    Merk op dat deze maakt gebruik van `${...}` variabelen die worden vervangen door de waarden in de taakdefinitie. De variabelen zijn:

    * **${coordFrequency}**: de tijd tussen de exemplaren van de taak
    * **${coordStart}**: de begintijd van de taak
    * **${coordEnd}**: de eindtijd van taak
    * **${coordTimezone}**: coördinator taken worden in een vaste tijdzone geen zomertijd (meestal weergegeven met behulp van UTC). Deze tijdzone wordt verwezen als de "Oozie verwerking timezone'
    * **${wfPath}**: het pad naar de workflow.xml

2. Gebruik Ctrl-X en **Y** en op **Enter** in het bestand wilt opslaan.

3. Om deze te kopiëren naar de werkmap die u voor deze taak gebruikt u het volgende:

        hadoop fs -copyFromLocal coordinator.xml /tutorials/useoozie/coordinator.xml

4. Het bestand **job.xml** te wijzigen, gebruikt u het volgende:

        nano job.xml

    De volgende wijzigingen aanbrengen:

    * Wijziging `<name>oozie.wf.application.path</name>` naar `<name>oozie.coord.application.path</name>`. Dit geeft de opdracht Oozie het bestand coördinator uitvoeren in plaats van een bestand met de workflow

    * Een variabele die verwijst naar de locatie van de workflow.xml in de coordinator.xml gebruikt voor de volgende sets worden toevoegen:

            <property>
              <name>workflowPath</name>
              <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
            </property>

        De waarden voor **mycontainer** en **mystorageaccount** vervangen door de waarden in andere vermeldingen in het bestand job.xml.

    * Voeg de volgende die het begin, einde en frequentie te gebruiken voor het bestand coordinator.xml te definiëren:

            <property>
              <name>coordStart</name>
              <value>2015-06-25T12:00Z</value>
            </property>

            <property>
              <name>coordEnd</name>
              <value>2015-06-27T12:00Z</value>
            </property>

            <property>
              <name>coordFrequency</name>
              <value>1440</value>
            </property>

            <property>
              <name>coordTimezone</name>
              <value>UTC</value>
            </property>

        Deze stelt u de begintijd tot 12:00 uur op 25 juni 2015, de eindtijd voor het 27th juni 2015 en het interval voor het uitvoeren van deze taak op dagelijks (de frequentie is in minuten, dus 24 uur x 60 minuten = 1440 minuten.) Ten slotte, de tijdzone is ingesteld op UTC.

5. Gebruik Ctrl-X en **Y** en op **Enter** in het bestand wilt opslaan.

6. Als u de taak uitvoert, gebruikt de volgende opdracht:

        oozie job -config job.xml -run

    Dit zal indienen en start de taak.

7. Als u Ga naar de webpagina Oozie UI en selecteer het tabblad **Taken van de coördinator** , moet u de volgende informatie:

    ![coördinator projecten tab](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    Noteer de **Volgende intreding** vermelding; Dit is wanneer de taak wordt uitgevoerd als volgende.

8. Net als bij de eerdere werkstroomtaak, de projectpost selecteren in de on line gebruikersinterface alleen informatie weergegeven over de taak:

    ![Coördinator taak info](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    Opmerking waarin dit alleen succesvol uitgevoerd van de taak, niet op afzonderlijke acties binnen de geplande workflow. Om te zien dat een van de **actie** -items selecteren. Hierdoor wordt informatie die vergelijkbaar is met die wordt opgehaald bij de eerdere werkstroomtaak weergegeven.

    ![Actie info](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

##<a name="troubleshooting"></a>Het oplossen van problemen

Bij het oplossen van problemen met de Oozie, de Oozie UI is erg handig omdat u eenvoudig beide logboeken Oozie bekijken, evenals koppelingen naar Logboeken voor taken zoals component query's MapReduce JobTracker. In het algemeen moet het patroon voor het oplossen van problemen:

1. U kunt de taak weergeven in Oozie Web UI.

2. Als er een fout of storing van de specifieke actie, selecteert u de actie te zien als het veld **Foutbericht** vindt u meer informatie over de fout.

3. Indien beschikbaar, gebruikt u de URL van de actie meer details (bijvoorbeeld JobTracker-Logboeken) voor de actie.

De volgende zijn specifieke fouten die kunnen optreden en hoe u deze kunt oplossen.

###<a name="ja009-cannot-initialize-cluster"></a>JA009: Kan het cluster niet initialiseren

**Symptomen**: status van de taak wordt gewijzigd in **uitgesteld**. Details voor de taak, wordt de status van de RunHiveScript weergegeven als **START_MANUAL**. De actie te selecteren, zullen het volgende foutbericht weergegeven:

    JA009: Cannot initialize Cluster. Please check your configuration for map

**Oorzaak**: de WASB adressen in het bestand **job.xml** bevatten geen van de container voor opslag of opslag account. De adresnotatie WASB moet `wasbs://containername@storageaccountname.blob.core.windows.net`.

**Resolutie**: de WASB-adressen die worden gebruikt door de taak te wijzigen.

###<a name="ja002-oozie-is-not-allowed-to-impersonate-ltuser"></a>JA002: Oozie is niet toegestaan om te imiteren &lt;gebruiker >

**Symptomen**: status van de taak wordt gewijzigd in **uitgesteld**. Details voor de taak, wordt de status van de RunHiveScript weergegeven als **START_MANUAL**. De actie te selecteren, zullen het volgende foutbericht weergegeven:

    JA002: User: oozie is not allowed to impersonate <USER>

**Oorzaak**: huidige machtigingsinstellingen Oozie imiteren van de opgegeven gebruikersaccount niet toestaan.

**Resolutie**: Oozie mag uitgeven voor gebruikers in de groep **gebruikers** . Gebruik de `groups USERNAME` voor een overzicht van de groepen die de gebruikersaccount lid van is. Als de gebruiker geen lid van de groep **gebruikers is** , gebruikt u de gebruiker wilt toevoegen aan de groep met de volgende opdracht:

    sudo adduser USERNAME users

> [AZURE.NOTE] Het kan enkele minuten duren voordat HDInsight wordt herkend dat de gebruiker is toegevoegd aan de groep.

###<a name="launcher-error-sqoop"></a>Startpictogram voor fout (Sqoop)

**Symptomen**: status van de taak wordt gewijzigd in **KILLED**. Details voor de taak, wordt de status van RunSqoopExport als **fout**weergegeven. De actie te selecteren, zullen het volgende foutbericht weergegeven:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**Oorzaak**: Sqoop kan niet worden geladen, het databasestuurprogramma dat is vereist voor toegang tot de database is.

**Oplossing**: wanneer u de Sqoop van een project Oozie, moet u het stuurprogramma met de andere bronnen (zoals de workflow.xml) die wordt gebruikt door de taak opnemen.

U moet ook verwijzen naar het archief met het stuurprogramma van de `<sqoop>...</sqoop>` sectie van de workflow.xml.

Bijvoorbeeld voor de taak in dit document gebruikt u de volgende stappen uit:

1. Kopieer het bestand sqljdbc4.1.jar naar de map /tutorials/useoozie:

         hadoop fs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar

2. Wijzigen van de workflow.xml om toe te voegen het volgende op een nieuwe regel hierboven `</sqoop>`:

        <archive>sqljdbc41.jar</archive>

##<a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd het definiëren van een workflow voor Oozie en het uitvoeren van een taak Oozie. Zie voor meer informatie over het werken met HDInsight, de volgende artikelen:

- [Op basis van tijd Oozie coördinator gebruiken met HDInsight][hdinsight-oozie-coordinator-time]
- [Uploaden van gegevens voor Hadoop projecten in HDInsight][hdinsight-upload-data]
- [Sqoop gebruiken in combinatie met Hadoop in HDInsight][hdinsight-use-sqoop]
- [Component gebruiken in combinatie met Hadoop op HDInsight][hdinsight-use-hive]
- [Varken gebruiken in combinatie met Hadoop op HDInsight][hdinsight-use-pig]
- [MapReduce Java-programma's ontwikkelen voor HDInsight][hdinsight-develop-mapreduce]


[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started]: hdinsight-get-started.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started-emulator]: hdinsight-get-started-emulator.md

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: sql-database-create-configure.md
[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]: storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

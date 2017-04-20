<properties
   pageTitle="Sensorgegevens analyseren met Apache Storm en HBase | Microsoft Azure"
   description="Informatie over verbinding maken met Apache Storm met een virtueel netwerk. Storm met HBase sensor om gegevens te verwerken vanaf een hub-gebeurtenis en het visualiseren met D3.js."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/20/2016"
   ms.author="larryfr"/>

# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Sensorgegevens analyseren met Apache Storm gebeurtenis Hub en HBase in HDInsight (Hadoop) 

Leren gebruiken Apache Storm op HDInsight sensor om gegevens te verwerken van Azure gebeurtenis Hub, opslaan in Apache HBase op HDInsight en het visualiseren met behulp van D3.js die wordt uitgevoerd als een Azure Web App.

De bronnenbeheerder Azure-sjabloon die wordt gebruikt in dit document laat zien hoe meerdere Azure resources in een resourcegroep te maken. Met name maakt een virtueel netwerk Azure, twee HDInsight clusters (Storm en HBase) en een Azure Web App. Een node.js-implementatie van een real-time webdashboard is automatisch web App geïmplementeerd.

> [AZURE.NOTE] De informatie in dit document en het voorbeeld, is getest met behulp van Linux-gebaseerde HDInsight 3.3 en 3.4 cluster versies.

## <a name="prerequisites"></a>Vereisten

* Een abonnement op Azure. Zie [Azure krijg gratis proefperiode](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

    > [AZURE.IMPORTANT] U hoeft niet een bestaand cluster van HDInsight; de stappen in dit document maakt de volgende bronnen:
    >
    > * Een virtueel netwerk van Azure
    > * Een Storm op de cluster HDInsight (Linux-gebaseerde 2 werknemer knooppunten)
    > * Een HBase op HDInsight cluster (Linux-gebaseerde 2 werknemer knooppunten)
    > * Een Azure Web App die het webdashboard host

* [Node.js](http://nodejs.org/): dit wordt gebruikt om het dashboard van de webpagina lokaal op uw ontwikkelomgeving te bekijken.

* [Java en de JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): gebruikt voor het ontwikkelen van de Storm-topologie.

* [Maven](http://maven.apache.org/what-is-maven.html): te compileren van het project gebruikt.

* [GIT](http://git-scm.com/): gebruikt om het project downloaden van GitHub.

* Een __SSH__ -client: verbinding maken met de HDInsight op basis van Linux-clusters worden gebruikt. Zie de volgende documenten voor meer informatie over het gebruik van SSH in HDInsight.

    * [SSH met HDInsight van een Windows-client gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [SSH met HDInsight in een Unix-, Linux- of Mac-client gebruiken](hdinsight-hadoop-linux-use-ssh-unix.md)

    > [AZURE.NOTE] U moet ook toegang hebben tot de `scp` -opdracht, die wordt gebruikt voor het kopiëren van bestanden tussen uw lokale ontwikkelomgeving en de HDInsight-cluster met behulp van SSH.

## <a name="architecture"></a>Architectuur

![Architectuurdiagram](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

In dit voorbeeld bestaat uit de volgende onderdelen:

* **Azure gebeurtenis Hubs**: bevat gegevens die worden verzameld van sensoren. In dit voorbeeld wordt een toepassing verstrekt die de gegevens genereert.

* **Storm op HDInsight**: biedt real-time verwerking van gegevens van gebeurtenis-Hub.

* **HBase op HDInsight**: biedt een permanente NoSQL gegevensarchief voor gegevens nadat deze is verwerkt door de Storm.

* **Azure Virtual Network service**: kunt u beveiligde communicatie tussen de Storm op HDInsight en HBase op HDInsight-clusters.

    > [AZURE.NOTE] Een virtueel netwerk is vereist voor de Java HBase client-API gebruiken als deze niet beschikbaar is via de openbare gateway voor HBase clusters. HBase en Storm clusters installeren in hetzelfde virtuele netwerk, kunt het cluster Storm (of een ander in het virtuele netwerk) direct toegang tot HBase client-API gebruiken.

* **Website van dashboard**: een voorbeeld van die gegevens in real-time grafieken dashboard.

    * De website is geïmplementeerd in Node.js, zodat deze kan worden uitgevoerd op een besturingssysteem van de client voor het testen of Azure Websites kan worden geïnstalleerd.

    * [Socket.IO](http://socket.io/) wordt gebruikt voor realtime-communicatie tussen de Storm-topologie en de website.

        > [AZURE.NOTE] Dit is een implementatiedetail. U kunt een kader communicatie, zoals raw, WebSockets of SignalR.

    * [D3.js](http://d3js.org/) wordt gebruikt om de gegevens die worden verzonden naar de website van een grafiek.

> [AZURE.IMPORTANT] Twee clusters zijn vereist, als er geen ondersteunde methode is om een cluster van HDInsight voor zowel Storm en HBase.

De topologie leest gegevens van gebeurtenis Hub met behulp van de klasse [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) en schrijft gegevens naar HBase met behulp van de klasse [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/javadoc/apidocs/org/apache/storm/hbase/bolt/class-use/HBaseBolt.html) . Communicatie met de website wordt uitgevoerd door middel van [socket.io client.java](https://github.com/nkzawa/socket.io-client.java).

Hier volgt een overzicht van de topologie.

![topologiediagram](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [AZURE.NOTE] Dit is een erg vereenvoudigde weergave van de topologie. Tijdens de uitvoering een exemplaar van elk onderdeel gemaakt voor elke partitie voor de gebeurtenis Hub die gelezen wordt. Deze exemplaren worden verdeeld over de knooppunten in het cluster en gegevens wordt gerouteerd tussen deze als volgt:
>
> * Gegevens uit de spout in de parser is verdeeld.
> * Gegevens uit de parser in het Dashboard en HBase worden gegroepeerd per apparaat-ID, zodat berichten van hetzelfde apparaat altijd naar hetzelfde onderdeel vloeien.

### <a name="topology-components"></a>Topologie onderdelen

* **EventHub Spout**: de spout wordt gegeven van Apache Storm versie 0.10.0 of hoger.

    > [AZURE.NOTE] De gebeurtenis Hubs spout gebruikt in dit voorbeeld vereist een Storm op HDInsight cluster versie 3.3 of 3.4. Zie [gebeurtenissen laten verwerken Azure gebeurtenis Hubs met Storm op HDInsight](hdinsight-storm-develop-java-event-hub-topology.md)voor meer informatie over het gebruik van Hubs gebeurtenis met een oudere versie van Storm op HDInsight.

* **ParserBolt.java**: de gegevens die worden verstrekt door de spout is raw JSON en soms meer dan één gebeurtenis tegelijk wordt uitgestoten. Deze bout laat zien hoe de gegevens verstrekt door de spout lezen en verzenden naar een nieuwe stroom als een tupel met meerdere velden.

* **DashboardBolt.java**: dit geeft aan hoe de Socket.io client library for Java gebruiken voor het verzenden van gegevens real-time aan het webdashboard.

In dit voorbeeld wordt in het kader van de [lichtstroom](https://storm.apache.org/releases/0.10.0/flux.html) , zodat de definitie van de topologie is opgenomen in YAML bestanden. Er zijn twee:

* __geen hbase.yaml__ - dit bestand bij het testen van de topologie in uw ontwikkelomgeving gebruiken. HBase onderdelen, niet omdat u geen toegang de API HBase Java van buiten het netwerk van dat het cluster zich tot in virtuele wordt gebruikt.

* __met hbase.yaml__ - dit bestand bij het implementeren van de topologie tot het cluster Storm gebruiken. HBase onderdelen worden gebruikt omdat deze wordt uitgevoerd in hetzelfde virtuele netwerk als het cluster HBase.

## <a name="prepare-your-environment"></a>De omgeving voorbereiden

Voordat u dit voorbeeld wilt gebruiken, moet u een Azure gebeurtenis-Hub van de Storm-topologie wordt gelezen.

### <a name="configure-event-hub"></a>Hub gebeurtenis configureren

Gebeurtenis Hub is de gegevensbron voor dit voorbeeld. Gebruik de volgende stappen voor het maken van een nieuwe gebeurtenis Hub.

1. Selecteren vanaf de [portal Azure](https://portal.azure.com) **+ Nieuw** -> __Internet van dingen__ -> __Gebeurtenis Hubs__.

2. Op het blad __Namespace maken__ de volgende taken uitvoeren:

    1. Voer een __naam__ voor de naamruimte.
    2. Selecteer een laag prijzen. __Basic__ is voldoende voor dit voorbeeld.
    3. Selecteer de Azure- __abonnement__ te gebruiken.
    4. Selecteer een bestaande resourcegroep of een nieuwe maken.
    5. Selecteer de __locatie__ voor de gebeurtenis Hub.
    6. __Pin om dashboard te__selecteren en klik vervolgens op __maken__.

3. Wanneer het proces is voltooid, wordt de gebeurtenis Hubs blade voor de naamruimte wordt weergegeven. Hier selecteert u __+ toevoegen gebeurtenis Hub__. Voer een naam van __sensordata__ en selecteer __maken__op het blad __Gebeurtenis Hub maken__ . Laat de andere velden op de standaardwaarden.

4. Selecteer de gebeurtenis Hubs blade voor de naamruimte, __Gebeurtenis Hubs__. Selecteer de post __sensordata__ .

5. Selecteer in het blad voor de sensordata gebeurtenis Hub, __Shared-beleid__. Gebruik de koppeling __+ toevoegen__ aan de volgende beleidsregels toevoegen:


  	| Naam van beleid | Vorderingen |
  	| ----- | ----- |
  	| apparaten | Verzenden |
  	| storm | Luisteren |

5. Selecteer beide beleidsinstellingen en noteer de waarde van de __Primaire sleutel__ . U moet de waarde voor het beleid in het toekomstige werk.

## <a name="download-and-configure-the-project"></a>Downloaden en configureren van het project

Gebruik de volgende downloaden van het project van GitHub.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

Nadat de opdracht is voltooid, hebt u de volgende mappenstructuur:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal
                no-hbase.yaml - topology definition for local testing
                with-hbase.yaml - topology definition that uses HBase in a virutal network
            src/ - the Java bolts
            dev.properties - contains configuration values for your environment
        dashboard/nodejs/ - this is the node.js web dashboard
        SendEvents/ - utilities to send fake sensor data

> [AZURE.NOTE] Dit document gaat op alle details van de code die is opgenomen in deze steekproef; de code is echter volledig toegelicht.

Open het bestand **hdinsight-eventhub-example/TemperatureMonitor/dev.properties** en de gebeurtenis Hub informatie toevoegen aan de volgende regels:

    eventhub.read.policy.name: storm
    eventhub.read.policy.key: KeyForTheStormPolicy
    eventhub.namespace: YourNamespace
    eventhub.name: sensordata

> [AZURE.NOTE] In dit voorbeeld wordt ervan uitgegaan dat u __storm__ gebruikt als de naam van het beleid dat een claim __luisteren is__ en de Hub gebeurtenis heet __sensordata__.

 Sla het bestand op nadat u deze informatie toevoegen.

## <a name="compile-and-test-locally"></a>Compileren en lokaal te testen

Voor de proef moet u het dashboard om de uitvoer van de topologie weergeven en gegevens op te slaan in de gebeurtenis Hub genereren te starten.

> [AZURE.IMPORTANT] Het onderdeel HBase van deze topologie is niet actief als lokaal testen als de Java API voor het cluster HBase is niet toegankelijk van buiten het netwerk van Azure virtuele clusters met.

### <a name="start-the-web-application"></a>Start de webtoepassing

1. Open een nieuwe opdrachtprompt of terminal en verander de directories naar het **hdinsight-eventhub-voorbeeld/dashboard**gebruikt u de volgende opdracht installeren de afhankelijkheden die nodig zijn voor de webtoepassing:

        npm install

2. Gebruik de volgende opdracht start u de webtoepassing:

        node server.js

    Het volgende bericht weergegeven:

        Server listening at port 3000

2. Open een webbrowser en voer **http://localhost:3000 /** als het adres. Een pagina met de volgende strekking worden weergegeven:

    ![webdashboard](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)

    Sluit de opdrachtprompt of de terminal. Na het testen, Ctrl-C te gebruiken op de webserver stoppen.

### <a name="start-generating-data"></a>Beginnen met het genereren van gegevens

> [AZURE.NOTE] De stappen in deze sectie gebruikt Node.js zodat ze kunnen worden gebruikt op elk platform. Zie de map **SendEvents** voor voorbeelden van andere taal.

1. Open een nieuwe opdrachtprompt, shell of terminal, en **hdinsight-eventhub-voorbeeld/SendEvents/nodejs**mappen wijzigen en vervolgens gebruikt u de volgende opdracht installeren de afhankelijkheden die nodig zijn voor de toepassing:

        npm install

2. Open het bestand **app.js** in een teksteditor en voeg de gebeurtenis Hub informatie die u eerder hebt aangeschaft:

        // ServiceBus Namespace
        var namespace = 'YourNamespace';
        // Event Hub Name
        var hubname ='sensordata';
        // Shared access Policy name and key (from Event Hub configuration)
        var my_key_name = 'devices';
        var my_key = 'YourKey';
    
    > [AZURE.NOTE] In dit voorbeeld wordt ervan uitgegaan dat u __sensordata__ als de naam van de gebeurtenis Hub en __apparaten__ hebt gebruikt als de naam van het beleid dat een claim __verzenden is__ .

2. Gebruik de volgende opdracht voor het invoegen van nieuwe posten in gebeurtenis Hub:

        node app.js

    Worden er meerdere regels van de uitvoer met de gegevens verzonden naar de gebeurtenis Hub. Deze wordt met de volgende strekking weergegeven:

        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="start-the-topology"></a>Start de topologie

2. Open een nieuwe opdrachtprompt, shell of mappen voor terminal- en naar __Hdinsight-eventhub-voorbeeld/TemperatureMonitor__en gebruikt u de volgende opdracht de topologie te starten:

        mvn compile exec:java -Dexec.args="--local -R /no-hbase.yaml --filter dev.properties"
    
    Als u met PowerShell, gebruik in plaats daarvan het volgende:

        mvn compile exec:java "-Dexec.args=--local -R /no-hbase.yaml --filter dev.properties"

    > [AZURE.NOTE] Als u op een systeem van Unix/Linux/OS X, en [Storm in uw ontwikkelomgeving geïnstalleerd](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html)hebt, kunt u de volgende opdrachten gebruiken:
    >
    > `mvn compile package`
    > `storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /no-hbase.yaml`

    De topologie die is gedefinieerd in het bestand __geen hbase.yaml__ in de lokale modus wordt gestart. De waarden in het bestand __dev.properties__ , geef de verbindingsgegevens voor de gebeurtenis Hubs. Eenmaal gestart, de topologie posten van Hub gebeurtenis leest en verzendt deze aan het dashboard op uw lokale computer. Worden er regels in het dashboard webpagina met de volgende strekking weergegeven:

    ![dashboard met gegevens](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

3. Terwijl het dashboard wordt uitgevoerd, gebruikt u de `node app.js` van de vorige stappen om nieuwe gegevens verzenden naar de gebeurtenis Hubs. Omdat de temperatuur waarden willekeurig gegenereerd, moet de grafiek bijwerken zodat grote veranderingen in temperatuur.

    > [AZURE.NOTE] U moet in de directory __Hdinsight-eventhub-voorbeeld/SendEvents/Nodejs__ bij het gebruik van de `node app.js` opdracht.

3. Nadat u hebt gecontroleerd dat dit werkt, stop de topologie met behulp van Ctrl + C. Ctrl + C kunt u ook de lokale webserver stoppen.

## <a name="create-a-storm-and-hbase-cluster"></a>Maak een cluster Storm en HBase

Om de topologie op HDInsight worden uitgevoerd en de bout HBase inschakelen, moet u een nieuw cluster van Storm en HBase cluster maken. De stappen in dit gedeelte gebruiken een [bronnenbeheerder Azure sjabloon](../resource-group-template-deploy.md) een nieuw virtueel netwerk Azure en een Storm en HBase cluster maken in het virtuele netwerk. De sjabloon is ook een Azure Web App maakt en implementeert u een kopie van het dashboard in het.

> [AZURE.NOTE] Een virtueel netwerk wordt gebruikt, zodat de topologie die op het cluster Storm kan rechtstreeks communiceren met de HBase cluster HBase Java API gebruiken.

De bronnenbeheerder sjabloon gebruikt in dit document bevindt zich in een openbare blob-container op __https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet.json__.

1. Klik op de knop Volgende om te melden bij Azure en open de sjabloon Resource Manager in de portal Azure.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Voer de volgende gegevens van de bladeserver **Parameters** :

    ![HDInsight-parameters](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
    
    * **BASECLUSTERNAME**: deze waarde wordt gebruikt als de naam voor de Storm en HBase clusters. Bijvoorbeeld maakt __hdi__ invoeren een cluster met Storm __storm-hdi__ met de naam en een HBase cluster met de naam __hbase hdi__.
    * __CLUSTERLOGINUSERNAME__: de gebruikersnaam admin voor de Storm en HBase clusters.
    * __CLUSTERLOGINPASSWORD__: de gebruiker admin-wachtwoord voor de clusters Storm en HBase.
    * __SSHUSERNAME__: de SSH gebruiker voor de Storm en HBase clusters maken.
    * __SSHPASSWORD__: het wachtwoord voor de gebruiker SSH voor de Storm en HBase clusters.
    * __Locatie__: de regio die in de clusters worden gemaakt.
    
    Klik op __OK__ om op te slaan van de parameters.
    
3. De sectie __resourcegroep__ gebruiken om een nieuwe resourcegroep maken of een bestaande selecteren.

4. Selecteer in de vervolgkeuzelijst __locatie bron__ dezelfde locatie als u hebt geselecteerd voor de parameter __locatie__ .

5. Selecteer de __juridische voorwaarden__en selecteer __maken__.

6. Ten slotte __aan het dashboard vastmaken__ controleren en selecteer __maken__. Het duurt ongeveer 20 minuten voor het maken van clusters.

Zodra de bronnen zijn gemaakt, wordt u omgeleid naar een blade voor de resourcegroep met clusters en webdashboard.

![Groep-blade resource voor de vnet en clusters](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [AZURE.IMPORTANT] U ziet dat de namen van de clusters HDInsight __storm BASENAME__ en __hbase BASENAME__, BASENAME is de naam die u aan de sjabloon. Gebruikt u deze namen in de volgende stappen als u verbinding maakt met de clusters. Ook vermelden dat de naam van de dashboard-site __basename dashboard__. U gebruikt dit later bij het bekijken van het dashboard.

## <a name="configure-the-dashboard-bolt"></a>De bout Dashboard configureren

Om de gegevens op het dashboard is geïmplementeerd als een web app verzendt, moet u de volgende regel in het bestand __dev.properties__ wijzigen:

    dashboard.uri: http://localhost:3000

Wijziging `http://localhost:3000` naar `http://BASENAME-dashboard.azurewebsites.net` en sla het bestand. __BASENAME__ vervangen door de naam van de opgegeven in de vorige stap. U kunt ook de resourcegroep hebt gemaakt om het dashboard selecteren en de URL weer te gebruiken.

## <a name="create-the-hbase-table"></a>Maak de tabel HBase

Om de gegevens opslaan in HBase, maken we een tabel. In het algemeen kunt u vooraf bronnen die nodig zijn om te schrijven naar Storm, als bij het maken van bronnen uit binnen een topologie Storm kunnen leiden tot meerdere, verspreide kopieën van de code bij het maken van dezelfde bron. De bronnen buiten de topologie maken en Storm alleen gebruiken voor het lezen of schrijven en analytics.

1. Met SSH kunt u verbinding maken met de HBase cluster met behulp van de SSH gebruiker en het wachtwoord die u aan de sjabloon hebt opgegeven tijdens het maken van het cluster. Bijvoorbeeld, als u verbinding maakt met behulp van de `ssh` opdracht, gebruikt u de volgende syntaxis:

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net
    
    Vervang in deze opdracht __gebruikersnaam__ met de SSH-gebruikersnaam die u hebt opgegeven bij het maken van het cluster en __BASENAME__ met de opgegeven naam. Voer desgevraagd het wachtwoord voor de gebruiker SSH.

2. Start de shell HBase uit de SSH-sessie.

        hbase shell
    
    Nadat de shell is geladen, ziet u een `hbase(main):001:0>` vragen.

3. Voer de volgende opdracht voor het maken van een tabel voor het opslaan van de sensorgegevens vanuit de shell HBase:

        create 'SensorData', 'cf'

4. Controleer of de tabel is aangemaakt met de volgende opdracht:

        scan 'SensorData'
        
    Hiermee wordt informatie zoals in het volgende voorbeeld, wat betekent dat er 0 rijen in de tabel.
    
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Typ het volgende om de shell HBase af te sluiten:

        exit

## <a name="configure-the-hbase-bolt"></a>De bout HBase configureren

Om te schrijven naar HBase uit de Storm-cluster, moet u de configuratiegegevens van het cluster HBase de bout HBase bieden. De eenvoudigste manier om dit te doen is de __hbase site.xml__ uit het cluster te downloaden en opnemen in uw project. U moet ook opmerkingen bij meerdere afhankelijkheden die de vereiste afhankelijkheden en storm hbase component laden in het bestand __pom.xml__ .

> [AZURE.IMPORTANT] U moet ook de storm-hbase.jar-bestand op uw Storm op HDInsight cluster 3.3 of 3.4 cluster; downloaden Deze versie is gecompileerd voor HBase 1.1.x, die wordt gebruikt voor HBase op HDInsight 3.3 en 3.4 clusters. Als u een onderdeel storm hbase elders gebruikt, kan het worden gecompileerd tegen een oudere versie van HBase.

### <a name="download-the-hbase-sitexml"></a>Download de hbase-site.xml

Gebruik SCP de __hbase site.xml__ -bestand te downloaden van het cluster vanaf een opdrachtprompt. In het volgende voorbeeld wordt de SSH gebruiker die u hebt opgegeven bij het maken van het cluster en __BASENAME__ met de naam van de die eerder opgegeven vervangen __gebruikersnaam__ . Voer desgevraagd het wachtwoord voor de gebruiker SSH. Vervang de `/path/to/TemperatureMonitor/resources/hbase-site.xml` met het pad naar dit bestand in het TemperatureMonitor-project.

    scp USERNAME@hbase-BASENAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml /path/to/TemperatureMonitor/resources/hbase-site.xml

Dit wordt de __hbase site.xml__ gedownload naar het opgegeven pad.

### <a name="download-and-install-the-storm-hbase-component"></a>Download en installeer het onderdeel storm hbase

1. Vanaf een opdrachtprompt gebruiken SCP de __storm-hbase.jar__ -bestand te downloaden van het cluster Storm. In het volgende voorbeeld wordt de SSH gebruiker die u hebt opgegeven bij het maken van het cluster en __BASENAME__ met de naam van de die eerder opgegeven vervangen __gebruikersnaam__ . Voer desgevraagd het wachtwoord voor de gebruiker SSH.

        scp USERNAME@storm-BASENAME-ssh.azurehdinsight.net:/usr/hdp/current/storm-client/contrib/storm-hbase/storm-hbase*.jar .

    Dit wordt gedownload een bestand met de naam `storm-hbase-####.jar`, waarbij ### is het versienummer van de Storm voor dit cluster. Noteer dit nummer, zoals later wordt gebruikt.

2. Gebruik de volgende opdracht als u dit onderdeel installeren naar de lokale bibliotheek Maven op uw ontwikkelomgeving. Hierdoor Maven vinden het pakket bij het compileren van het project. Vervangen __####__ met het versienummer dat is opgenomen in de bestandsnaam.

        mvn install:install-file -Dfile=storm-hbase-####.jar -DgroupId=org.apache.storm -DartifactId=storm-hbase -Dversion=#### -Dpackaging=jar
    
    Als u met PowerShell, gebruikt u de volgende opdracht:

        mvn install:install-file "-Dfile=storm-hbase-####.jar" "-DgroupId=org.apache.storm" "-DartifactId=storm-hbase" "-Dversion=####" "-Dpackaging=jar"

### <a name="enable-the-storm-hbase-component-in-the-project"></a>De storm hbase onderdeel in het project inschakelen

1. Open het bestand __TemperatureMonitor/pom.xml__ en verwijder de volgende regels:

        <!-- uncomment this section to enable the hbase-bolt
        end comment for hbase-bolt section -->
    
    > [AZURE.IMPORTANT] Alleen verwijderen van deze twee regels; een van de lijnen tussen hen niet verwijderen.
    
    Hierdoor kunnen verschillende componenten die nodig zijn bij de communicatie met de bout hbase met HBase.

2. Zoek naar de volgende regels en vervang __####__ met het versienummer van de storm hbase-bestand dat u eerder hebt gedownload.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-hbase</artifactId>
            <version>####</version>
        </dependency>

    > [AZURE.IMPORTANT] Het versienummer moet overeenkomen met de versie die u gebruikt bij het installeren van het onderdeel in de opslagplaats lokale Maven, Maven gebruikt deze informatie om het onderdeel te laden wanneer het project wordt gebouwd.

2. Sla het bestand __pom.xml__ .

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>Bouwen, pakket- en HDInsight van de oplossing implementeren

Gebruik de volgende stappen de Storm-topologie naar de storm cluster implementeren in uw ontwikkelomgeving.

1. Gebruik de volgende opdracht uit te voeren een nieuwe build een JAR-pakket maken vanuit uw project uit de map __TemperatureMonitor__ :

        mvn clean compile package

    Hiermee maakt u een bestand met de naam **TemperatureMonitor-1.0-SNAPSHOT.jar** in **de doelmap van uw project** .

2. Scp gebruiken het bestand te uploaden __1.0-TemperatureMonitor-SNAPSHOT.jar__ in uw cluster Storm. In het volgende voorbeeld wordt de SSH gebruiker die u hebt opgegeven bij het maken van het cluster en __BASENAME__ met de naam van de die eerder opgegeven vervangen __gebruikersnaam__ . Voer desgevraagd het wachtwoord voor de gebruiker SSH.

        scp target\TemperatureMonitor-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:TemperatureMonitor-1.0-SNAPSHOT.jar
    
    > [AZURE.NOTE] Het duurt enkele minuten voor het uploaden van het bestand, zoals verschillende megabyte groot zal zijn.

    Scp gebruiken voor het uploaden van het bestand __dev.properties__ als dit document de informatie die wordt gebruikt bevat voor verbinding met de gebeurtenis Hubs en het dashboard.

        scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:dev.properties

3. Zodra de bestanden zijn geüpload, verbinding maken met het cluster via SSH.

        ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net

4. Gebruik de volgende opdracht start u de topologie van de SSH-sessie.

        storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /with-hbase.yaml --filter dev.properties
    
    Hiermee start u de topologie met behulp van de definitie in het bestand __met de hbase.yaml__ van de topologie en de configuratiewaarden in het bestand __dev.properties__ .

3. Nadat de topologie is gestart, opent u een browser naar de website die u hebt gepubliceerd op Azure, gebruikt u de `node app.js` de opdracht om gegevens te verzenden naar de gebeurtenis Hub. Hier ziet u het webdashboard bijwerken om de gegevens weer te geven.

    ![dashboard](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>HBase gegevens weergeven

Nadat u de gegevens voor het gebruik van de topologie hebt ingediend `node app.js`, gebruikt u de volgende stappen verbinding maken met HBase en controleer of de gegevens is geschreven aan de tabel die u eerder hebt gemaakt.

1. Met SSH verbinding maken met het cluster HBase.

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net

2. Start de shell HBase uit de SSH-sessie.

        hbase shell
    
    Nadat de shell is geladen, ziet u een `hbase(main):001:0>` vragen.

2. Weergave rijen uit de tabel:

        scan 'SensorData'
        
    Dit moet gegevens als resultaat als de volgende, wat betekent dat er 0 rijen in de tabel.
    
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds

    > [AZURE.NOTE] Deze scanbewerking retourneert alleen een maximum van 10 rijen uit de tabel.

## <a name="delete-your-clusters"></a>Verwijderen van clusters

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Als u wilt verwijderen van de clusters, opslag en web app in één keer de resourcegroep verwijderen waarin ze zich bevinden.

## <a name="next-steps"></a>Volgende stappen

U hebt nu geleerd hoe Storm met gegevens van de gebeurtenis Hubs lezen, opslaan in HBase en de gegevens op een externe dashboard weergeven met behulp van Socket.io en D3.js.

* Zie voor meer voorbeelden van topologieën Storm met HDInsight:

    * [Voorbeeld van de topologieën voor Storm op HDInsight](hdinsight-storm-example-topology.md)

* Zie voor meer informatie over Apache Storm, de [Storm Apache](https://storm.incubator.apache.org/) -site.

* Zie de [HBase met HDInsight-overzicht](hdinsight-hbase-overview.md)voor meer informatie over HBase op HDInsight.

* Zie de website [socket.io](http://socket.io/) voor meer informatie over Socket.io.

* Zie voor meer informatie over D3.js, [D3.js - Driven documenten](http://d3js.org/).

* Zie voor meer informatie over het maken van topologieën in Java, [ontwikkelen van Java topologieën voor Apache Storm op HDInsight](hdinsight-storm-develop-java-topology.md).

* Zie voor meer informatie over het maken van topologieën in .NET [ontwikkelen C# topologieën voor Apache Storm op HDInsight met behulp van Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com

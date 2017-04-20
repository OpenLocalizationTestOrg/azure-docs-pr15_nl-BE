<properties
   pageTitle="Implementeren en beheren van Apache Storm topologieën op Linux-gebaseerde HDInsight | Microsoft Azure"
   description="Informatie over het implementeren, bewaken en beheren van Apache Storm topologieën met het Dashboard Storm op Linux-gebaseerde HDInsight. Hadoop-hulpprogramma's gebruiken voor Visual Studio."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

# <a name="deploy-and-manage-apache-storm-topologies-on-linux-based-hdinsight"></a>Implementeren en beheren van Apache Storm topologieën op Linux-gebaseerde HDInsight

In dit document, informatie over de basisbeginselen van het beheren en controleren van Storm topologieën op Linux gebaseerde Storm op HDInsight-clusters.

> [AZURE.IMPORTANT] De stappen in dit artikel is een Linux-gebaseerde Storm op de cluster HDInsight vereist. Zie voor meer informatie over implementatie en de controle van topologieën op Windows gebaseerde HDInsight [implementeren en beheren van Apache Storm topologieën op Windows gebaseerde HDInsight](hdinsight-storm-deploy-monitor-topology.md)

## <a name="prerequisites"></a>Vereisten

* **Een Linux-gebaseerde Storm op de cluster HDInsight**: Zie [aan de slag met Apache Storm op HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) voor stapsgewijze instructies voor het maken van een cluster

* **Vertrouwdheid met SSH en SCP**: Zie voor meer informatie over het gebruik van SSH en SCP met HDInsight het volgende:

    * **Linux, Unix of OS X-clients**: Zie [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Linux, OS X en Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-clients**: Zie [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

* **Een SCP-client**: dit is voorzien van alle Linux, Unix en OS X-systemen. Voor Windows-clients wordt aangeraden PSCP, dat verkrijgbaar is bij de [stopverf pagina downloaden](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

## <a name="start-a-storm-topology"></a>Start een topologie Storm

### <a name="using-ssh-and-the-storm-command"></a>SSH en de opdracht Storm

1. Met SSH verbinding maken met het cluster HDInsight. **Gebruikersnaam** vervangt het de naam van uw SSH-aanmelding. **CLUSTERNAAM** vervangen door de naam van het cluster HDInsight:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Zie de volgende documenten voor meer informatie over het gebruik van SSH verbinding maken met uw cluster HDInsight:
    
    * **Linux, Unix of OS X-clients**: Zie [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Linux, OS X en Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * **Windows-clients**: Zie [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Gebruik de volgende opdracht start u een topologie:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.9.3.2.2.4.9-1.jar storm.starter.WordCountTopology WordCount

    Hiermee start u de topologie WordCount op het cluster. Willekeurig worden genereren zinnen en tellen van elk woord in de zinnen.

    > [AZURE.NOTE] Bij de indiening van de topologie van het cluster, moet u eerst kopiëren het jar-bestand met het cluster voordat u de `storm` opdracht. Dit kunt u doen met behulp van de `scp` van de client waar het bestand zich bevindt. Bijvoorbeeld:`scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > In het voorbeeld WordCount en andere voorbeelden van de starter storm zijn al opgenomen in het cluster op `/usr/hdp/current/storm-client/contrib/storm-starter/`.

### <a name="programmatically"></a>Via programmering

U kunt via programmering een topologie implementeren op Storm op HDInsight door te communiceren met de Nimbus-service host in het cluster. [https://github.com/Azure-Samples/hdinsight-Java-Deploy-storm-Topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) biedt een voorbeeld van de Java-toepassing die laat hoe zien te implementeren en een topologie via de Nimbus-service te starten.

## <a name="monitor-and-manage-using-the-storm-command"></a>Bewaken en beheren met de opdracht storm

De `storm` hulpprogramma kunt u werken met topologieën uitvoeren vanaf de opdrachtregel. Hier volgt een lijst met veelgebruikte opdrachten. Gebruik `storm -h` voor een volledige lijst met opdrachten.

### <a name="list-topologies"></a>Lijst topologieën

Gebruik de volgende opdracht om een lijst van alle topologieën uitgevoerd:

    storm list
    
Het resultaat is informatie met de volgende strekking:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Deactiveren en reactiveren

Het deactiveren van een topologie pauzeert het totdat het wordt gedood of opnieuw geactiveerd. Gebruik de volgende te deactiveren en opnieuw activeren:

    storm Deactivate TOPOLOGYNAME
    
    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Een topologie met kill

Storm topologieën, eenmaal gestart, blijft actief totdat deze wordt gestopt. Als u wilt stoppen met een topologie, gebruik de volgende opdracht:

    storm stop TOPOLOGYNAME

### <a name="rebalance"></a>Balans

Opnieuw een topologie kan het systeem te herzien, de parallellisme van de topologie. Bijvoorbeeld als het formaat van het cluster om toe te voegen meer notities opnieuw kunt een actieve topologie maken gebruik van de nieuwe knooppunten.

> [AZURE.WARNING] Een topologie eerst opnieuw deactiveert de topologie, opnieuw werknemers gelijkmatig verdeeld over de cluster vervolgens, tot slot wordt de topologie naar de staat voordat het opnieuw is opgetreden. Dus als de topologie actief is, wordt het weer actief. Als deze optie is uitgeschakeld, blijft deze gedeactiveerd.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-using-the-storm-ui"></a>Bewaken en beheren met behulp van de gebruikersinterface voor de Storm

De Storm-gebruikersinterface biedt een web-interface voor het werken met topologieën uitgevoerd en is opgenomen in het cluster HDInsight. Als u wilt weergeven in de gebruikersinterface van de Storm, een webbrowser gebruiken voor het openen van __https://CLUSTERNAME.azurehdinsight.net/stormui__, waarbij de __CLUSTERNAAM__ de naam van het cluster is.

> [AZURE.NOTE] Als een gebruikersnaam en wachtwoord wordt gevraagd, voert u Clusterbeheer (admin) en wachtwoord dat u wanneer gebruikt u het cluster maakt.


### <a name="main-page"></a>Hoofdpagina

De hoofdpagina van de Storm-gebruikersinterface bevat de volgende informatie:

* **Cluster samenvatting**: informatie over het cluster Storm.

* **Topologie samenvatting**: een lijst met actieve topologieën. Gebruik de koppelingen in deze sectie voor meer informatie over bepaalde topologieën.

* **Supervisor samenvatting**: informatie over de Storm supervisor.

* **Nimbus configuratie**: Nimbus-configuratie voor het cluster.

### <a name="topology-summary"></a>Topologie-overzicht

Een koppeling in de sectie **Samenvatting topologie** te selecteren, worden de volgende gegevens over de topologie:

* **Topologie overzicht**: basisinformatie over de topologie.

* **Topologie acties**: acties die u voor de topologie uitvoeren kunt.

  * **Activeren**: verwerking van een gedeactiveerde topologie wordt hervat.

  * **Uitschakelen**: een actieve topologie wordt onderbroken.

  * **Vastleggen**: Hiermee past u de evenwijdigheid van de topologie. Nadat u het aantal knooppunten in het cluster hebt gewijzigd, moet u lopende topologieën opnieuw. Hierdoor wordt de topologie aanpassen parallellisme ter compensatie van de verhoogde of verlaagde aantal knooppunten in het cluster.

    Zie <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Wat is de evenwijdigheid van een topologie Storm</a>voor meer informatie.

  * **Kill**: een topologie Storm na de opgegeven time-out beëindigd.

* **Topologie statistieken**: statistieken over de topologie. Gebruik de koppelingen in het **venster** kolom het tijdsbestek voor de overige items op de pagina instellen.

* **Spouts**: de spouts die wordt gebruikt door de topologie. Gebruik de koppelingen in deze sectie voor meer informatie over specifieke spouts.

* **Bolts**: de bouten die wordt gebruikt door de topologie. Gebruik de koppelingen in deze sectie voor meer informatie over specifieke bouten.

* **Configuratie van de topologie**: de configuratie van de geselecteerde topologie.

### <a name="spout-and-bolt-summary"></a>Spout en bout samenvatting

Een spout selecteren in de secties **Spouts** of **Bolts** bevat de volgende informatie over het geselecteerde item:

* **Onderdeel overzicht**: basisinformatie over de spout of bout.

* **Spout/bout statistieken**: statistieken over de spout of bout. Gebruik de koppelingen in het **venster** kolom het tijdsbestek voor de overige items op de pagina instellen.

* **Input-statistieken** (alleen bout): informatie over de invoer streams die worden gebruikt door de bout.

* **Uitvoer stats**: informatie over de streams uitgestoten door deze spout of bout.

* **Executors**: informatie over de exemplaren van de spout of bout. Selecteer de post **poort** voor een specifieke executor een logboek van diagnostische gegevens voor dit exemplaar geproduceerd te bekijken.

* **Fouten**: eventuele fouten voor deze spout of bout.

## <a name="rest-api"></a>REST-API

De gebruikersinterface voor de Storm is gebouwd op de REST API, zodat u kunt soortgelijke beheer en bewaking van functionaliteit met behulp van de REST API. De REST-API kunt u aangepaste hulpprogramma's voor het beheren en controleren van Storm topologieën maken.

Voor meer informatie Zie [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html). De volgende informatie is specifiek voor de REST API gebruiken met Apache Storm op HDInsight.

> [AZURE.IMPORTANT] De Storm REST API niet algemeen beschikbaar is via het internet en zijn toegankelijk via een SSH-tunnel met het hoofd clusterknooppunt van HDInsight. Zie [Gebruik SSH Tunneling voor toegang tot Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, en andere web UI van](hdinsight-linux-ambari-ssh-tunnel.md)voor meer informatie over het maken en met behulp van een SSH-tunnel.

### <a name="base-uri"></a>Base URI

De basis-URI voor de REST API op Linux-gebaseerde HDInsight clusters beschikbaar is op het knooppunt head op **api-https://HEADNODEFQDN:8744/v1/**; echter de naam van het domein van het hoofd knooppunt tijdens het maken van het cluster wordt gegenereerd en is niet statisch.

U vindt de FQDN-naam (Fully Qualified Domain Name) voor het clusterknooppunt hoofd op verschillende manieren:

* __Van een SSH-sessie__: de opdracht `headnode -f` van een SSH-sessie aan het cluster.

* __Website van Ambari__: Selecteer __Services__ vanaf de bovenkant van de pagina en selecteer vervolgens __Storm__. Selecteer op het tabblad __Samenvatting__ __Storm UI-Server__. De FQDN-naam van het knooppunt waarop de Storm-gebruikersinterface en de REST-API wordt worden aan de bovenkant van de pagina.

* __Uit Ambari REST API__: Gebruik de opdracht `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` voor het ophalen van informatie over het knooppunt met de Storm-gebruikersinterface en de REST API. __Wachtwoord__ vervangen door de admin-wachtwoord voor de cluster. __CLUSTERNAAM__ vervangen door de naam van het cluster. In het antwoord bevat de vermelding "hostnaam" de FQDN-naam van het knooppunt.

### <a name="authentication"></a>Verificatie

Aanvragen voor de REST API moeten **Basisverificatie**gebruiken zodat u de naam van HDInsight cluster administrator en het wachtwoord gebruiken.

> [AZURE.NOTE] Omdat basisverificatie via leesbare tekst worden verzonden, moet u **altijd** gebruik HTTPS voor het beveiligen van communicatie met het cluster.

### <a name="return-values"></a>Retourwaarden

Informatie die wordt geretourneerd door de REST API mogelijk bruikbaar uit binnen het cluster of virtuele machines in dezelfde Azure virtuele netwerk als het cluster. Bijvoorbeeld de FQDN-naam (Fully Qualified Domain Name) geretourneerd voor Zookeeper-servers niet toegankelijk zijn via Internet.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe te implementeren en topologieën controleren met behulp van het Dashboard Storm, informatie over het [ontwikkelen van Java gebaseerde topologieën met Maven](hdinsight-storm-develop-java-topology.md).

Zie voor een lijst van meer voorbeeld topologieën [voorbeeld topologieën voor Storm op HDInsight](hdinsight-storm-example-topology.md).

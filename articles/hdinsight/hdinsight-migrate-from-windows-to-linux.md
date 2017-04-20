<properties
pageTitle="Migreren van HDInsight op basis van Windows naar Linux-gebaseerde HDInsight | Azure"
description="Informatie over het migreren vanaf een cluster dat Windows-gebaseerde HDInsight naar een Linux-gebaseerde HDInsight cluster."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/28/2016"
ms.author="larryfr"/>

#<a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Migreren vanaf een cluster dat Windows-gebaseerde HDInsight naar een Linux-gebaseerde cluster

Terwijl Windows gebaseerde HDInsight een eenvoudige manier biedt gebruik van Hadoop in de cloud, ontdekt u dat u een Linux-gebaseerde cluster gebruik te maken van hulpprogramma's en technologieën die nodig voor uw oplossing zijn nodig. Veel dingen in het Hadoop-ecosysteem op Linux-systemen zijn ontwikkeld en sommige mogelijk niet beschikbaar voor gebruik met Windows-gebaseerde HDInsight. Ook veel boeken, video's en ander trainingsmateriaal wordt ervan uitgegaan dat u een Linux-systeem gebruiken bij het werken met Hadoop.

Dit document bevat details over de verschillen tussen de richtlijnen voor het migreren van bestaande werkbelasting aan een Linux-gebaseerde cluster en HDInsight voor Windows en Linux.

> [AZURE.NOTE] HDInsight clusters gebruiken Ubuntu lange termijn ondersteuning (TNS) als het besturingssysteem voor de knooppunten in het cluster. Zie voor meer informatie over de versie van Ubuntu die beschikbaar zijn met HDInsight, samen met andere informatie onderdeel versioning, [HDInsight onderdeel versies](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Migratietaken

De algemene workflow voor migratie is als volgt.

![Migratie van werkstroomdiagram](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1.  Lees elke sectie van dit document te begrijpen van de wijzigingen die nodig zijn bij het migreren van de bestaande workflow, taken, enz. naar een Linux-gebaseerde cluster.

2.  Maak een cluster op basis van Linux als een omgeving test/quality assurance. Zie voor meer informatie over het maken van een Linux-gebaseerde cluster [maken Linux gebaseerde clusters in het HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

3.  Bestaande projecten, gegevensbronnen en put kopiëren naar de nieuwe omgeving. Zie de gegevens kopiëren naar de sectie test omgeving voor meer informatie.

4.  Gegevensvalidatie testen om ervoor te zorgen dat uw projecten werken zoals verwacht op het nieuwe cluster uit te voeren.

Nadat u hebt gecontroleerd of alles werkt zoals verwacht, plant u uitvaltijd voor de migratie. Tijdens deze downtime, kunt u de volgende acties uitvoeren.

1.  Back-up van alle tijdelijke gegevens lokaal opgeslagen op de clusterknooppunten. Bijvoorbeeld als u gegevens rechtstreeks op een knooppunt head.

2.  De op Windows gebaseerde cluster verwijderen.

3.  Maak een Linux-gebaseerde cluster met behulp van de dezelfde winkel standaard die de Windows-gebaseerde clusters gebruikt. Hierdoor wordt het nieuwe cluster om verder te werken ten opzichte van de bestaande productiegegevens.

4.  Tijdelijke u back-ups van gegevens importeren.

5.  Start taken/blijven verwerken met behulp van het nieuwe cluster.

### <a name="copy-data-to-the-test-environment"></a>Gegevens kopiëren naar de testomgeving

Er zijn veel methoden voor het kopiëren van de gegevens en taken, de twee besproken in deze sectie zijn echter de eenvoudigste methoden om rechtstreeks bestanden verplaatsen naar een cluster test.

#### <a name="hdfs-dfs-copy"></a>DFS HDFS kopiëren

U kunt de opdracht Hadoop HDFS rechtstreeks gegevens kopiëren uit de opslag voor uw bestaande cluster van de productie naar de opslag voor een nieuwe test-cluster met behulp van de volgende stappen uit.

1. De opslag en de standaard container informatie vinden voor uw bestaande cluster. U kunt dit doen met behulp van de volgende Azure PowerShell script.

        $clusterName="Your existing HDInsight cluster name"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
        write-host "Default container: $clusterInfo.DefaultStorageContainer"

2. Volg de stappen in het maken van Linux-gebaseerde clusters in het document voor het maken van een testomgeving voor nieuwe HDInsight. Stop voordat u het cluster maakt, en in plaats daarvan selecteert u **Optionele configuratie**.

3. Selecteer **Gekoppelde Accounts voor opslag**in de blade optionele configuratie.

4. Selecteer **een sleutel opslag toevoegen**en wanneer dat wordt gevraagd, selecteert u de account van opslag die is geretourneerd door het PowerShell script in stap 1. Klik op **selecteren** op elke bladeserver om ze te sluiten. Tot slot maakt u het cluster.

5. Wanneer u het cluster hebt gemaakt, een verbinding maken met behulp van **SSH.** Als u niet bekend bent met via SSH met HDInsight, Zie de volgende artikelen.

    * [SSH gebruiken met Linux-gebaseerde HDInsight van Windows-clients](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [SSH gebruiken met Linux-gebaseerde HDInsight van clients voor Linux, Unix en Mac](hdinsight-hadoop-linux-use-ssh-unix.md)

6. Gebruik de volgende opdracht om bestanden te kopiëren van de rekening gekoppelde opslag naar de nieuwe standaard opslag-account van de SSH-sessie. CONTAINER- en vervangen door de container en de accountgegevens die wordt geretourneerd door de PowerShell-script in stap 1. Het pad naar gegevens vervangen door het pad naar een bestand.

        hdfs dfs -cp wasbs://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location

    [AZURE.NOTE] Als de directory-structuur die de gegevens bevat die niet in de testomgeving bestaat, kunt u met de volgende opdracht.

        hdfs dfs -mkdir -p /new/path/to/create

    De `-p` schakelt het maken van alle mappen in het pad.

#### <a name="direct-copy-between-azure-storage-blobs"></a>Rechtstreekse kopie tussen opslag Azure BLOB 's

U kunt ook gebruik van de `Start-AzureStorageBlobCopy` Azure PowerShell-cmdlet BLOB's tussen rekeningen buiten HDInsight opslag te kopiëren. Zie voor meer informatie de sectie Azure BLOB's van Azure PowerShell gebruiken met Azure opslag beheren.

##<a name="client-side-technologies"></a>Client-side technologieën

In het algemeen blijft client-side technologieën zoals [Azure PowerShell-cmdlets](../powershell-install-configure.md), [Azure CLI](../xplat-cli-install.md) of de [.NET SDK voor Hadoop](https://hadoopsdk.codeplex.com/) werken hetzelfde met Linux gebaseerde clusters, omdat ze afhankelijk zijn van de REST API's die hetzelfde in beide clustertypen OS zijn.

##<a name="server-side-technologies"></a>Server-side technologieën

De volgende tabel verschaft leidraden voor migratie serveronderdelen die specifieke Windows zijn.

| Als u van deze technologie gebruikmaakt... | Deze actie ondernemen... |
| ----- | ----- |
| **PowerShell** (serverscripts, met inbegrip van acties Script gebruikt tijdens het maken van het cluster) | Als Bash scripts schrijven. Zie voor scriptacties, [HDInsight op basis van Linux aanpassen met scriptacties](hdinsight-hadoop-customize-cluster-linux.md) en [actie scripts te ontwikkelen voor Linux-gebaseerde HDInsight](hdinsight-hadoop-script-actions-linux.md). |
| **Azure CLI** (server-side scripts) | Terwijl de CLI Azure op Linux beschikbaar is, is niet afkomstig vooraf geïnstalleerde HDInsight hoofd knooppunten. Als u nodig hebt voor serverscripts, Zie [de Azure CLI installeren](../xplat-cli-install.md) voor informatie over het installeren op Linux-gebaseerde platforms. |
| **Onderdelen van .NET** | .NET wordt niet volledig ondersteund in HDInsight op basis van Linux-clusters. Storm op HDInsight op basis van Linux-clusters gemaakt na 28-10/2017 ondersteuning C# Storm topologieën met behulp van het kader van de SCP.NET. Aanvullende ondersteuning voor .NET worden toegevoegd in toekomstige updates. |
| **Win32-onderdelen of een andere technologie alleen-Windows** | Richtlijnen is afhankelijk van het onderdeel of de technologie; u mogelijk een versie die compatibel is met Linux of moet u een alternatieve oplossing vinden of dit onderdeel te herschrijven. |

##<a name="cluster-creation"></a>Maken van het cluster

Deze sectie bevat informatie over de verschillen in het maken van het cluster.

### <a name="ssh-user"></a>SSH gebruiker

**Secure Shell (SSH)** protocol Linux-gebaseerde HDInsight clusters gebruiken voor externe toegang tot de clusterknooppunten. In tegenstelling tot extern bureaublad voor Windows-gebaseerde clusters meeste SSH clients bieden een grafische gebruikersinterface, maar in plaats daarvan biedt een opdrachtregelprogramma waarmee u opdrachten kunt uitvoeren op het cluster. Sommige clients (zoals [MobaXterm](http://mobaxterm.mobatek.net/),) bieden een grafische bestand systeem browser naast een afstandsbediening vanaf de opdrachtregel.

Tijdens het maken van het cluster, moet u een SSH-gebruiker en een **wachtwoord** of **certificaat met openbare sleutel** opgeven voor de verificatie.

Wij raden aan met behulp van openbare-sleutelcertificaat is veiliger dan het gebruik van een wachtwoord. Certificaatverificatie werkt door een ondertekende openbare/persoonlijke sleutelpaar te genereren en vervolgens de openbare sleutel die bij het maken van het cluster. Wanneer u verbinding maakt met de server via SSH, bevat de persoonlijke sleutel op de client verificatie voor de verbinding.

Zie voor meer informatie over het gebruik van SSH in HDInsight:

- [SSH gebruiken met HDInsight van Windows-clients](hdinsight-hadoop-linux-use-ssh-windows.md)

- [SSH met HDInsight van Linux, Unix en OS X-clients gebruiken](hdinsight-hadoop-linux-use-ssh-unix.md)

### <a name="cluster-customization"></a>Cluster aanpassen

**Acties script** gebruikt met Linux gebaseerde clusters moet worden geschreven in een Bash-script. Terwijl het scriptacties kunnen worden gebruikt tijdens het maken van het cluster, voor Linux gebaseerde clusters kunnen ze ook worden gebruikt om de aanpassingen uitvoeren nadat een cluster is en wordt uitgevoerd. Voor meer informatie, Zie [HDInsight op basis van Linux aanpassen met scriptacties](hdinsight-hadoop-customize-cluster-linux.md) en [actie scripts te ontwikkelen voor Linux-gebaseerde HDInsight](hdinsight-hadoop-script-actions-linux.md).

Een andere functie voor het aanpassen is **bootstrap**. Voor Windows-clusters kunt zo u de locatie opgeven van extra bibliotheken voor gebruik met de component. Na het maken van het cluster, deze bibliotheken zijn automatisch beschikbaar voor gebruik met de query hoeft te gebruiken component `ADD JAR`.

De bootstrap voor Linux gebaseerde clusters beschikt niet over deze functionaliteit. Gebruik in plaats daarvan scriptactie beschreven in [bibliotheken component toevoegen tijdens het maken van het cluster](hdinsight-hadoop-add-hive-libraries.md).

### <a name="virtual-networks"></a>Virtuele netwerken

HDInsight Windows-gebaseerde clusters alleen werken met klassieke virtuele netwerken, terwijl HDInsight Linux gebaseerde clusters Resource Manager virtuele netwerken vereisen. Als u in een klassieke virtueel netwerk die het Linux HDInsight cluster verbinding met maken moet bronnen, Zie [verbinding maken met een klassieke virtueel netwerk met een bronnenbeheerder virtueel netwerk](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Zie voor meer informatie over de configuratievereisten voor het gebruik van virtuele netwerken Azure met HDInsight, [mogelijkheden voor HDInsight uitbreiden met behulp van een virtueel netwerk](hdinsight-extend-hadoop-virtual-network.md).

##<a name="management-and-monitoring"></a>Beheer en bewaking

Veel van de web UI's die u in combinatie met Windows-HDInsight, zoals geschiedenis of garens UI gebruikt, zijn beschikbaar via Ambari. Bovendien kunt de Ambari component weergeven u onderdeel uitvoeren via uw webbrowser. De Ambari Web UI is beschikbaar op Linux gebaseerde clusters op https://CLUSTERNAME.azurehdinsight.net.

Zie de volgende documenten voor meer informatie over het werken met Ambari:

- [Web Ambari](hdinsight-hadoop-manage-ambari.md)

- [Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari waarschuwingen

Ambari heeft een waarschuwingssysteem dat u mogelijke problemen met het cluster kunt zien. Er worden waarschuwingen weergegeven als rode of gele items in de gebruikersinterface van het Web Ambari maar u ze ook via de REST API ophalen kunt.

> [AZURE.IMPORTANT] Ambari waarschuwingen geven aan dat er *mogelijk* een probleem, niet dat er *is* een probleem. Bijvoorbeeld, verschijnt een waarschuwing dat HiveServer2 kan niet worden geopend, zelfs als u toegang hebt tot het normaal.
>
> Veel waarschuwingen worden geïmplementeerd als query's op basis van het interval voor een service en verwacht een antwoord binnen een bepaalde tijd. Zodat de waarschuwing betekent niet dat de service niet actief is, resultaat alleen dat het niet binnen het verwachte tijdsbestek.

In het algemeen moet u beoordelen of een waarschuwing heeft is gepland voor een langere periode, of komt overeen met problemen van gebruikers die eerder zijn gemeld voor het cluster alvorens actie te ondernemen op het.

##<a name="file-system-locations"></a>Locaties van systeem

Het bestandssysteem van de Linux-cluster is de lay-out anders dan HDInsight op basis van Windows-clusters. Gebruik de volgende tabel om te zoeken naar veelgebruikte bestanden.

| Ik wil... | Deze bevindt zich... |
| ----- | ----- |
| Configuratie | `/etc`. Bijvoorbeeld:`/etc/hadoop/conf/core-site.xml` |
| Logboekbestanden | `/var/logs` |
| Hortonworks Data Platform (HDP) | `/usr/hdp`. Er zijn twee mappen zich bevinden, dat is de huidige HDP-versie (bijvoorbeeld, `2.2.9.1-1`,) en `current`. De `current` directory symbolische koppelingen bevat naar bestanden en mappen in de versie nummer directory en wordt geleverd als een handige manier toegang krijgen tot bestanden HDP sinds de versie nummer verandert als de HDP-versie wordt bijgewerkt. |
| hadoop-streaming.jar | `/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

In het algemeen als u de naam van het bestand kent, kunt u de volgende opdracht vanaf een SSH-sessie het pad naar het bestand te zoeken:

    find / -name FILENAME 2>/dev/null

U kunt ook jokertekens gebruiken met de naam van het bestand. Bijvoorbeeld, `find / -name *streaming*.jar 2>/dev/null` het pad gaat terug naar jar-bestanden die het woord streaming als onderdeel van de bestandsnaam bevatten.

##<a name="hive-pig-and-mapreduce"></a>Component-, varkens- en MapReduce

Varkens en MapReduce werkbelastingen lijken veel op Linux gebaseerde clusters - het voornaamste verschil is als u via Extern bureaublad verbinding maken met een Windows-gebaseerde clusters en taken uitvoeren, kunt u SSH met Linux gebaseerde clusters gebruiken wilt.

- [Varken met SSH gebruiken](hdinsight-hadoop-use-pig-ssh.md)

- [MapReduce met SSH gebruiken](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Component

De volgende grafiek biedt richtlijnen over het migreren van uw werkbelasting component.

| Op Windows gebaseerde ik gebruik... | Op Linux gebaseerde... |
| ----- | ----- |
| **Component-Editor** | [Weergave van de component in Ambari](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`Tez inschakelen | Tez is de standaard uitvoering-engine voor Linux gebaseerde clusters, zodat de instructie set niet langer nodig is. |
| CMD-bestanden of scripts op de server als onderdeel van een taak onderdeel aangeroepen | Bash scripts gebruiken |
| `hive`opdracht van extern bureaublad | [Beeline](hdinsight-hadoop-use-hive-beeline.md) of [component van een SSH-sessie](hdinsight-hadoop-use-hive-ssh.md) |

##<a name="storm"></a>Storm

| Op Windows gebaseerde ik gebruik... | Op Linux gebaseerde... |
| ----- | ----- |
| Storm-Dashboard | Het Dashboard Storm is niet beschikbaar. Zie [implementeren en beheren van Storm topologieën op Linux-gebaseerde HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md) voor informatie over het indienen van topologieën |
| Storm UI | De Storm UI is beschikbaar op https://CLUSTERNAME.azurehdinsight.net/stormui |
| Visual Studio voor het maken, distribueren en beheren van C# of hybride topologieën | Visual Studio kan worden gebruikt voor het maken, implementeren en beheren van C# (SCP.NET) of hybride topologieën op Linux gebaseerde Storm op HDInsight clusters gemaakt na 28-10/2017. |

##<a name="hbase"></a>HBase

Op Linux gebaseerde clusters, de bovenliggende znode voor HBase is `/hbase-unsecure`. U moet instellen dit in de configuratie voor een willekeurige Java-client toepassingen die native HBase Java API gebruiken.

Zie [een HBase Java gebaseerde toepassing maken](hdinsight-hbase-build-java-maven.md) voor een voorbeeld van de client die deze waarde worden ingesteld.

##<a name="spark"></a>Spark

Spark clusters zijn beschikbaar op Windows-clusters bij voorbeeld; voor release is Spark echter alleen beschikbaar met Linux gebaseerde clusters. Er is geen migratiepad vanaf een cluster dat Windows gebaseerde Spark voorbeeld naar een Linux-gebaseerde Spark release cluster.

##<a name="known-issues"></a>Bekende problemen

### <a name="azure-data-factory-custom-net-activities"></a>Azure Data Factory aangepaste .NET activiteiten

Azure Data Factory aangepaste .NET activiteiten worden momenteel niet ondersteund op HDInsight op basis van Linux-clusters. In plaats daarvan moet u een van de volgende methoden voor het implementeren van aangepaste activiteiten als onderdeel van de ADF pipeline.

-   .NET-activiteiten op Azure Batch van toepassingen worden uitgevoerd. Zie de sectie gebruik Azure Batch gekoppeld service van [aangepaste activiteiten gebruik van een pijpleiding Azure Data Factory](../data-factory/data-factory-use-custom-activities.md#AzureBatch)

-   De activiteit als activiteit MapReduce implementeren. [MapReduce programma's aanroepen van Data Factory](../data-factory/data-factory-map-reduce.md) Zie voor meer informatie.

### <a name="line-endings"></a>Regeleinden

Regeleinden op Windows-systemen in het algemeen gebruikt CRLF, LF terwijl Linux-systemen. Als u produceren of verwacht gegevens met CRLF regeleinden dat, moet u mogelijk wijzigen om te werken met het einde van de regel LF producenten of consumenten.

Bijvoorbeeld, retourneert met behulp van Azure PowerShell aan query HDInsight op een op Windows gebaseerde cluster gegevens met CRLF. Met een Linux-gebaseerde cluster dezelfde query retourneert LF. In veel gevallen dit niet van belang voor de consument gegevens maar het moet worden onderzocht voordat u migreert naar een Linux-gebaseerde cluster.

Als u scripts die worden uitgevoerd op de Linux-clusterknooppunten (zoals een Python-script gebruikt met een component of een taak MapReduce), gebruik altijd LF als het einde van de regel. Als u CRLF gebruikt, ziet u mogelijk fouten bij het uitvoeren van de scripts op een Linux-gebaseerde cluster.

Als u weet dat de scripts geen tekenreeksen met ingesloten CR-tekens bevatten, kunt u wijzigen bulksgewijs de regeleinden met behulp van een van de volgende methoden:

-   **Als u scripts hebt die u van plan bent het uploaden van het cluster**, de volgende PowerShell-instructies gebruiken voor het wijzigen van de regeleinden van CRLF in LF voordat het script uploaden naar het cluster.

        $original_file ='c:\path\to\script.py'
        $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
        [IO.File]::WriteAllText($original_file, $text)

-   **Als u de scripts die al in de opslag die wordt gebruikt door het cluster zijn hebt**, kunt u de volgende opdracht vanaf een SSH-sessie met de Linux-gebaseerde cluster het script wijzigen.

        hdfs dfs -get wasbs:///path/to/script.py oldscript.py
        tr -d '\r' < oldscript.py > script.py
        hdfs dfs -put -f script.py wasbs:///path/to/script.py

##<a name="next-steps"></a>Volgende stappen

-   [Informatie over het maken van HDInsight op basis van Linux-clusters](hdinsight-hadoop-provision-linux-clusters.md)

-   [Verbinding maken met een Linux-gebaseerde cluster met behulp van SSH in een Windows-client](hdinsight-hadoop-linux-use-ssh-windows.md)

-   [Verbinding maken met een Linux-gebaseerde cluster met behulp van SSH in een Unix-, Linux- of Mac-client](hdinsight-hadoop-linux-use-ssh-unix.md)

-   [Beheren van een Linux-gebaseerde cluster met behulp van Ambari](hdinsight-hadoop-manage-ambari.md)

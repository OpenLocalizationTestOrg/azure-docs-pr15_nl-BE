<properties
    pageTitle="Ontwikkeling van de actie met de Linux-gebaseerde HDInsight script | Microsoft Azure"
    description="Het HDInsight op basis van Linux-clusters met actie Script aanpassen. Scriptacties zijn een manier om clusters Azure HDInsight aanpassen door het cluster configuratie-instellingen op te geven of extra services, hulpprogramma's of andere software te installeren op het cluster. "
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="larryfr"/>

# <a name="script-action-development-with-hdinsight"></a>Ontwikkeling van de actie script met HDInsight

Scriptacties zijn een manier om clusters Azure HDInsight aanpassen door het cluster configuratie-instellingen op te geven of extra services, hulpprogramma's of andere software te installeren op het cluster. U kunt scriptacties tijdens het maken van het cluster of op een cluster uitgevoerd.

> [AZURE.NOTE] De informatie in dit document is specifiek voor de HDInsight op basis van Linux-clusters. Zie voor meer informatie over het gebruik van scriptacties met Windows-gebaseerde clusters [actie scripts te ontwikkelen met HDInsight (Windows)](hdinsight-hadoop-script-actions.md).

## <a name="what-are-script-actions"></a>Wat zijn scriptacties?

Scriptacties Bash scripts zijn die Azure configuratiewijzigingen aanbrengen of installeren van software op de knooppunten van het cluster wordt uitgevoerd. De scriptactie van een wordt uitgevoerd als root en biedt volledige toegangsrechten voor de clusterknooppunten.

Scriptacties kunnen worden toegepast met de volgende methoden:

| Gebruik deze optie om een script te passen... | Tijdens het maken van het cluster... | Op een cluster uitgevoerd... |
| ----- |:-----:|:-----:|
| Azure Portal | ✓ | ✓ |
| Azure PowerShell | ✓ | ✓ |
| Azure CLI | &nbsp; | ✓ |
| HDInsight .NET SDK | ✓ | ✓ |
| Sjabloon voor Azure Resource Manager | ✓ | &nbsp; |

Zie voor meer informatie over het gebruik van deze methoden toe te passen scriptacties [clusters van HDInsight aanpassen met behulp van scriptacties](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Aanbevolen procedures voor het ontwikkelen van scripts

Wanneer u een aangepast script voor een cluster HDInsight ontwikkelt, zijn verschillende aanbevelingen rekening mee moet houden:

- [Doel de Hadoop-versie](#bPS1)
- [Doel van de versie van het besturingssysteem](#bps10)
- [Stabiele koppelingen naar scriptbronnen](#bPS2)
- [Vooraf gecompileerde resources gebruiken](#bPS4)
- [Zorg ervoor dat het cluster aanpassing script idempotency is ingeschakeld](#bPS3)
- [Hoge beschikbaarheid van de cluster-architectuur](#bPS5)
- [De aangepaste onderdelen voor het gebruik van Azure Blob-opslag configureren](#bPS6)
- [Gegevens schrijven naar STDOUT en STDERR](#bPS7)
- [ASCII-bestanden opslaan met LF regeleinden](#bps8)
- [Pogingslogica gebruiken om te herstellen van fouten van voorbijgaande aard](#bps9)

> [AZURE.IMPORTANT] Scriptacties binnen 60 minuten moeten uitvoeren of ze een time-out wordt. Tijdens het knooppunt inrichten, wordt het script uitgevoerd als andere processen van de installatie en configuratie. Concurrentie voor bronnen, zoals CPU-tijd of netwerk bandbreedte mogelijk het script duurt langer dan in uw ontwikkelomgeving te voltooien.

### <a name="bPS1"></a>Doel de Hadoop-versie

Verschillende versies van de HDInsight hebben verschillende versies van Hadoop services en onderdelen die worden geïnstalleerd. Als uw script een bepaalde versie van een onderdeel of een service verwacht, moet u het script alleen gebruiken met de versie van HDInsight met de vereiste onderdelen. Vindt u informatie over de versies van onderdeel uit het HDInsight met behulp van het document [HDInsight onderdeel versiebeheer](hdinsight-component-versioning.md) .

###<a name="bps10"></a>Doel van de versie van het besturingssysteem

Linux-gebaseerde HDInsight is gebaseerd op de Ubuntu Linux-distributie. Verschillende versies van de HDInsight is afhankelijk van verschillende versies van Ubuntu die kan invloed hebben op de werking van uw script. Bijvoorbeeld HDInsight 3,4 en eerdere versies zijn gebaseerd op Ubuntu versies die Upstart gebruiken. Versie 3.5 is gebaseerd op Ubuntu 16.04 met Systemd. Systemd en Upstart zijn afhankelijk van verschillende opdrachten, zodat uw script moet worden geschreven om te werken met beide.

Een ander belangrijk verschil tussen HDInsight 3.4 en 3.5 is dat `JAVA_HOME` nu verwijst naar Java 8.

U kunt de versie van het besturingssysteem controleren met behulp van `lsb_release`. De volgende codefragmenten uit de kleurtoon installatiescript laat zien hoe om te bepalen als het script wordt uitgevoerd op Ubuntu 14 of 16:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi
    ...
    if [[ $OS_VERSION == 16* ]]; then
        echo "Using systemd configuration"
        systemctl daemon-reload
        systemctl stop webwasb.service    
        systemctl start webwasb.service
    else
        echo "Using upstart configuration"
        initctl reload-configuration
        stop webwasb
        start webwasb
    fi
    ...
    if [[ $OS_VERSION == 14* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
    elif [[ $OS_VERSION == 16* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
    fi

U vindt het volledige script waarin deze fragmenten op https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.

Zie het document [HDInsight onderdeelversie](hdinsight-component-versioning.md) voor de versie van Ubuntu die door HDInsight wordt gebruikt.

Zie [Systemd voor Upstart gebruikers](https://wiki.ubuntu.com/SystemdForUpstartUsers)inzicht in de verschillen tussen Systemd en Upstart.

### <a name="bPS2"></a>Stabiele koppelingen naar scriptbronnen

U moet ervoor zorgen dat scripts en bronnen die door het script worden gebruikt gedurende de levensduur van het cluster beschikbaar blijft en dat de versies van deze bestanden niet wijzigen voor de duur. Deze bronnen zijn vereist als nieuwe knooppunten aan het cluster worden toegevoegd tijdens het schalen van bewerkingen.

De beste manier is om te downloaden en archiveren van alles in een opslag Azure-account op uw abonnement.

> [AZURE.IMPORTANT] De account van de opslag die wordt gebruikt moet de opslag standaardaccount voor het cluster of een container openbaar, alleen-lezen op een andere account voor opslag.

De door Microsoft geleverde monsters worden bijvoorbeeld opgeslagen in de account [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) opslag, dit een openbare, alleen-lezen-container beheerd door het team van HDInsight is.

### <a name="bPS4"></a>Vooraf gecompileerde resources gebruiken

Om de tijd te beperken nodig die is voor het script wordt uitgevoerd, voorkomen van bewerkingen die resources uit de broncode compileren. In plaats daarvan vooraf de bronnen te compileren en de binaire versie opslaan in Azure Blob-opslag zodat deze kan snel worden gedownload naar het cluster uit het script.

### <a name="bPS3"></a>Zorg ervoor dat het cluster aanpassing script idempotency is ingeschakeld

Scripts moeten zodanig zijn ontworpen dat in die zin dat als het script wordt uitgevoerd meerdere keren idempotency is ingeschakeld, moeten ervoor zorgen dat het cluster wordt geretourneerd naar een zelfde staat elke keer dat deze wordt uitgevoerd.

Als u een aangepast script installeert een toepassing op usr op haar eerste uitvoeren en op elke latere uitvoering het script moet controleren of de toepassing al op de locatie usr bestaat voordat u verdergaat met de andere in het script stappen.

### <a name="bPS5"></a>Hoge beschikbaarheid van de cluster-architectuur

Linux-gebaseerde HDInsight clusters bevat twee hoofd knooppunten die actief zijn binnen het cluster en script acties worden uitgevoerd voor beide knooppunten. Als slechts één hoofd knooppunt verwacht dat u de onderdelen die u installeert, moet u een script dat de component alleen op een van de twee hoofd knooppunten in het cluster installeren wordt te ontwerpen.

> [AZURE.IMPORTANT] Services die standaard geïnstalleerd als onderdeel van de HDInsight zijn ontworpen om failover tussen de twee knooppunten head, indien nodig, maar deze functionaliteit niet wordt uitgebreid tot de aangepaste onderdelen die zijn geïnstalleerd via het script ctions. Als u de onderdelen die worden geïnstalleerd via een scriptactie uiterst beschikbaar zijn, moet u uw eigen failover-mechanisme dat gebruikmaakt van de twee beschikbare knooppunten head implementeren.

### <a name="bPS6"></a>De aangepaste onderdelen voor het gebruik van Azure Blob-opslag configureren

Onderdelen die u in het cluster installeert hebt een standaardconfiguratie met Hadoop Distributed bestand System (HDFS) opslag. Azure Blob-opslag (WASB) HDInsight gebruikt als de standaard opslag. Dit biedt een HDFS compatibel bestand waarin gegevens aanhoudt, zelfs als het cluster is verwijderd. Onderdelen u WASB gebruikt in plaats van HDFS installeert, moet u configureren.

Het volgende wordt bijvoorbeeld het bestand giraph-examples.jar uit het lokale bestandssysteem op WASB:

    hadoop fs -copyFromLocal /usr/hdp/current/giraph/giraph-examples.jar /example/jars/

### <a name="bPS7"></a>Gegevens schrijven naar STDOUT en STDERR

Informatie geschreven naar STDOUT en STDERR tijdens de uitvoering van het script wordt geregistreerd en kan worden bekeken via het web Ambari UI.

> [AZURE.NOTE] Ambari is alleen beschikbaar als het cluster is gemaakt. Als u een scriptactie tijdens het maken van het cluster gebruiken, niet worden gemaakt, Zie de sectie probleemoplossing [dat hdinsight aanpassen met behulp van de scriptactie-clusters](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) voor andere manieren van toegang tot geregistreerde gegevens.

De meeste hulpprogramma's en -installatiepakketten wordt al gegevens schrijven naar STDOUT en STDERR, maar u kunt aanvullende logboekregistratie toevoegen. Tekst verzenden naar STDOUT gebruik `echo`. Bijvoorbeeld:

        echo "Getting ready to install Foo"

Standaard `echo` stuurt de tekenreeks naar STDOUT. Het rechtstreeks naar STDERR, toevoegen `>&2` voor `echo`. Bijvoorbeeld:

        >&2 echo "An error occurred installing Foo"

Hiermee wordt informatie verzonden naar STDOUT (1, namelijk standaard dus hier niet wordt vermeld) omgeleid naar STDERR (2). Zie [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html)voor meer informatie over i/o-omleiding.

Zie voor meer informatie over het weergeven van informatie die door het scriptacties vastgelegd [clusters van HDInsight aanpassen met behulp van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

###<a name="bps8"></a>ASCII-bestanden opslaan met LF regeleinden

Bash scripts moeten worden opgeslagen als ASCII-indeling die met lijnen door LF beëindigd. Als bestanden worden opgeslagen als UTF-8, waaronder een Byte Order Mark aan het begin van het bestand of met regeleinden van CRLF die gemeenschappelijk voor Windows-editors is, mislukt het script met de volgende fouten:

    $'\r': command not found
    line 1: #!/usr/bin/env: No such file or directory

###<a name="bps9"></a>Pogingslogica gebruiken om te herstellen van fouten van voorbijgaande aard

Bij het downloaden van bestanden, het installeren van pakketten met apt-get of andere acties die gegevens via het internet verzenden, wordt de actie mislukken door fouten van voorbijgaande aard netwerken. Bijvoorbeeld de externe bron die u wilt communiceren is mogelijk bij het niet over een back-knooppunt.

Als u wilt uw script veerkrachtig fouten van voorbijgaande aard, kunt u de logica opnieuw implementeren. Hier volgt een voorbeeld van een functie die wordt elke opdracht doorgegeven aan het uitvoeren en (indien de opdracht niet werkt,) maximaal drie keer opnieuw. Zij Wacht twee seconden tussen elke poging.

    #retry
    MAXATTEMPTS=3

    retry() {
        local -r CMD="$@"
        local -i ATTMEPTNUM=1
        local -i RETRYINTERVAL=2

        until $CMD
        do
            if (( ATTMEPTNUM == MAXATTEMPTS ))
            then
                    echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                    return 1
            else
                    echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                    sleep $(( RETRYINTERVAL ))
                    ATTMEPTNUM=$ATTMEPTNUM+1
            fi
        done
    }

Hier volgen enkele voorbeelden van het gebruik van deze functie.

    retry ls -ltr foo

    retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh

## <a name="helpermethods"></a>Methoden voor aangepaste scripts

actie script methoden zijn hulpprogramma's die u gebruiken kunt bij het schrijven van aangepaste scripts. Deze worden gedefinieerd in [https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh)en kunnen worden opgenomen in uw scripts met behulp van de volgende opties:

    # Import the helper method module.
    wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh

Hiermee maakt u de volgende hulpprogramma's beschikbaar voor gebruik in uw script:

| Gebruik van de helper | Beschrijving |
| ------------ | ----------- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` | Een bestand downloadt van de bron-URL naar het opgegeven pad. Standaard wordt er een bestaand bestand niet overschreven. |
| `untar_file TARFILE DESTDIR` | Wordt een tar-bestand (met `-xf`,) naar de doelmap. |
| `test_is_headnode` | Als uitgevoerd op een hoofd clusterknooppunt, geeft als resultaat 1; anders 0. |
| `test_is_datanode` | Als het huidige knooppunt is een gegevensknooppunt (werknemer), geeft een 1; anders 0. |
| `test_is_first_datanode` | Als het huidige knooppunt het eerste (werknemer)-gegevensknooppunt (met de naam workernode0), geeft een 1; anders 0. |
| `get_headnodes` | Deze eigenschap retourneert de FQDN-naam van de headnodes in het cluster. Namen worden gescheiden door komma's. Een lege tekenreeks wordt van de fout geretourneerd. |
| `get_primary_headnode` | Deze eigenschap haalt de FQDN-naam van de primaire headnode. Een lege tekenreeks wordt van de fout geretourneerd. |
| `get_secondary_headnode` | Deze eigenschap haalt de FQDN-naam van de secundaire headnode. Een lege tekenreeks wordt van de fout geretourneerd. |
| `get_primary_headnode_number` | Deze eigenschap haalt het numerieke achtervoegsel van de primaire headnode. Een lege tekenreeks wordt van de fout geretourneerd. |
| `get_secondary_headnode_number` | Deze eigenschap haalt het numerieke achtervoegsel van de secundaire headnode. Een lege tekenreeks wordt van de fout geretourneerd. |

## <a name="commonusage"></a>Algemene gebruikspatronen

Deze sectie bevat instructies over het implementeren van enkele van de algemene gebruikspatronen die u tegenkomen kunt tijdens het schrijven van een eigen script.

### <a name="passing-parameters-to-a-script"></a>Parameters doorgeven aan een script

In sommige gevallen kan het script parameters vereist. Bijvoorbeeld, wellicht het admin-wachtwoord voor de cluster om informatie ophalen uit de Ambari REST API.

Parameters die worden doorgegeven aan het script _positionele parameters_worden genoemd en worden toegewezen aan `$1` voor de eerste parameter, `$2` voor het tweede, en dus op. `$0`bevat de naam van het script zelf.

Waarden worden doorgegeven aan het script als parameters moeten worden omsloten door enkele aanhalingstekens (') zodat de doorgegeven waarde wordt beschouwd als een letterlijke waarde en speciale behandeling niet wordt gegeven aan opgenomen tekens bevat, zoals '!'.

### <a name="setting-environment-variables"></a>Omgevingsvariabelen instellen

Als u een omgevingsvariabele wordt uitgevoerd door het volgende:

    VARIABLENAME=value

VARIABELENAAM is waar de naam van de variabele. Gebruiken voor toegang tot de variabele na dit, `$VARIABLENAME`. Bijvoorbeeld om te wijzen op een waarde die een omgevingsvariabele met de naam wachtwoord door een positionele parameter is opgegeven, gebruikt u het volgende:

    PASSWORD=$1

Latere toegang tot de informatie vervolgens kan gebruiken `$PASSWORD`.

Omgevingsvariabelen instellen in het script bestaat alleen binnen het toepassingsgebied van het script. In sommige gevallen moet u toevoegen breed systeemomgevingsvariabelen dat blijft bestaan nadat het script is voltooid. Meestal is dit zodat gebruikers die verbinding maken met het cluster via SSH de onderdelen die worden geïnstalleerd door het script kunnen gebruiken. U kunt dit doen door het toevoegen van de omgevingsvariabele `/etc/environment`. Bijvoorbeeld het volgende voorbeeld wordt __HADOOP\_Ov\_DIR__:

    echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Toegang tot de locaties waar de aangepaste scripts zijn opgeslagen

Scripts die worden gebruikt voor het aanpassen van een cluster moeten aan worden de standaard opslag-account voor de cluster of, indien het op een andere account voor opslag, in een openbare container voor alleen-lezen. Als uw script toegang krijgt bronnen die zich elders bevindt tot deze ook moeten worden in een openbaar toegankelijke (ten minste openbare alleen-lezen). U wilt bijvoorbeeld een bestand downloaden aan het cluster met behulp van `download_file`.

Slaat het bestand in een toegankelijke Azure opslag-account door het cluster (bijvoorbeeld de standaard opslag-account), bieden snelle toegang, aangezien deze opslag binnen het netwerk Azure.

### <a name="checking-the-operating-system-version"></a>Het controleren van de versie van het besturingssysteem

Verschillende versies van HDInsight zijn gebaseerd op specifieke versies van Ubuntu. Er zijn verschillen tussen OS versies die u in uw script moet controleren. U wilt bijvoorbeeld een binair bestand dat is gekoppeld aan de versie van Ubuntu installeren.

U controleert de versie van het besturingssysteem te gebruiken `lsb_release`. Bijvoorbeeld het volgende voorbeeld toont hoe u kunt verwijzen naar een andere tar-bestand, afhankelijk van de versie van het besturingssysteem:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi

## <a name="deployScript"></a>Controlelijst voor het implementeren van een scriptactie

Hier zijn de stappen die we hebben bij de opstelling van deze scripts implementeren:

- Plaats de bestanden met de aangepaste scripts op een locatie die toegankelijk is voor de clusterknooppunten tijdens de implementatie. Dit is een van de standaard of extra opslag rekeningen die zijn opgegeven op het moment van het cluster distributie of opslag van openbaar toegankelijke containers.

- Controles in scripts om ervoor te zorgen dat ze impotently, uitvoeren, zodat het script kan meerdere keren worden uitgevoerd op hetzelfde knooppunt toevoegen.

- Een tijdelijk bestand directory /tmp gebruiken om te houden van de gedownloade bestanden die worden gebruikt door de scripts en vervolgens opschonen van nadat de scripts zijn uitgevoerd.

- In het geval dat OS-niveau-instellingen of Hadoop service configuratiebestanden zijn gewijzigd, kunt u HDInsight-services opnieuw starten zodat ze een OS-instellingen op objectniveau, zoals de omgevingsvariabelen instellen in de scripts kunnen afhalen.

## <a name="runScriptAction"></a>Het uitvoeren van een scriptactie

U kunt scriptacties HDInsight clusters aanpassen met behulp van de portal voor Azure Azure PowerShell, sjablonen Azure Resource Manager (ARM) of de HDInsight .NET SDK. Zie voor instructies [hoe u met de scriptactie](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Voorbeelden van aangepast script

Microsoft biedt voorbeeldscripts om onderdelen te installeren op een cluster van HDInsight. Voorbeeldscripts en instructies over hoe u ze gebruikt vindt u op de onderstaande koppelingen:

- [Installeren en gebruiken van tint op clusters HDInsight](hdinsight-hadoop-hue-linux.md)
- [Installeren en gebruiken van R op HDInsight Hadoop-clusters](hdinsight-hadoop-r-scripts-linux.md)
- [Installeren en gebruiken van Solr op HDInsight clusters](hdinsight-hadoop-solr-install-linux.md)
- [Installeer en gebruik de Giraph op HDInsight-clusters](hdinsight-hadoop-giraph-install-linux.md)  

> [AZURE.NOTE] De documenten gekoppeld hierboven zijn specifiek voor de HDInsight op basis van Linux-clusters. Voor scripts die werken met Windows-gebaseerde HDInsight Zie [actie scripts te ontwikkelen met HDInsight (Windows)](hdinsight-hadoop-script-actions.md) of gebruik van de beschikbare koppelingen boven aan elk artikel.

##<a name="troubleshooting"></a>Het oplossen van problemen

Er zijn fouten die optreden bij het gebruik van scripts die u hebt ontwikkeld:

__Error__: `$'\r': command not found`. Soms gevolgd door `syntax error: unexpected end of file`.

_Oorzaak_: deze fout wordt veroorzaakt wanneer de regels in een script met CRLF eindigt. UNIX-systemen wordt alleen LF verwacht als de laatste regel.

Dit probleem treedt meestal wanneer het script is geschreven in een Windows-omgeving, zoals CRLF is een algemene regel die eindigt voor veel teksteditors op Windows.

_Oplossing_: als het is een optie in een teksteditor, selecteert u Unix-indeling of LF voor het einde van de regel. U kunt ook de volgende opdrachten op een Unix-systeem een LF de CRLF wijzigen:

> [AZURE.NOTE] De volgende opdrachten zijn ongeveer gelijk in die zin dat ze de CRLF regeleinden LF moeten wijzigen. Selecteer een op basis van de hulpprogramma's die beschikbaar zijn op uw systeem.

| Opdracht | Notities |
| ------- | ----- |
| `unix2dos -b INFILE` | Het oorspronkelijke bestand worden gestaafd met een. De extensie BAK |
| `tr -d '\r' < INFILE > OUTFILE` | UITVOERBESTAND bevat een versie met alleen LF-uitgangen |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | Dit zal het bestand rechtstreeks zonder dat er een nieuw bestand wijzigen |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` | UITVOERBESTAND bevat een versie met alleen LF-uitgangen.

__Error__: `line 1: #!/usr/bin/env: No such file or directory`.

_Oorzaak_: deze fout treedt op wanneer het script is opgeslagen als UTF-8 met een Byte Order Mark (BOM).

_Resolutie_: het bestand opslaan als een ASCII- of als UTF-8 zonder een stuklijst. U kunt ook de volgende opdracht op een Linux- of Unix-systeem voor het maken van een nieuw bestand zonder de stuklijst:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Het bestand met de stuklijst voor de bovenstaande opdracht vervangen __INVOERBESTAND__ . __UITVOERBESTAND__ moet een nieuwe bestandsnaam waarin het script zonder de stuklijst zijn.

## <a name="seeAlso"></a>Volgende stappen

* Meer informatie over het [aanpassen van HDInsight clusters met scriptactie](hdinsight-hadoop-customize-cluster-linux.md)

* Gebruik de [verwijzing HDInsight .NET SDK](https://msdn.microsoft.com/library/mt271028.aspx) voor meer informatie over het maken van .NET-toepassingen die HDInsight beheren

* De [HDInsight REST-API](https://msdn.microsoft.com/library/azure/mt622197.aspx) gebruiken voor informatie over het gebruik van andere acties uitvoeren op clusters van HDInsight.

<properties
    pageTitle="Ontwikkeling van de actie met HDInsight script | Microsoft Azure"
    description="Informatie over het aanpassen van Hadoop-clusters met actie Script. De scriptactie kan worden gebruikt op een cluster Hadoop-extra software te installeren of wijzigen van de configuratie van toepassingen die zijn geïnstalleerd op een cluster."
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
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>De actie Script scripts voor HDInsight Windows-gebaseerde clusters ontwikkelt

Informatie over de actie Script scripts schrijven voor HDInsight. Zie voor meer informatie over het gebruik van de actie Script scripts [aanpassen HDInsight clusters met actie Script](hdinsight-hadoop-customize-cluster.md). Zie voor hetzelfde artikel geschreven voor HDInsight Linux gebaseerde clusters, [scripts ontwikkelen scriptactie voor HDInsight](hdinsight-hadoop-script-actions-linux.md).

> [AZURE.NOTE] De informatie in dit document is specifiek voor de HDInsight op basis van Windows-clusters. Zie voor meer informatie over het gebruik van scriptacties met Windows-gebaseerde clusters [actie scripts te ontwikkelen met HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).


De scriptactie kan worden gebruikt op een cluster Hadoop-extra software te installeren of wijzigen van de configuratie van toepassingen die zijn geïnstalleerd op een cluster. Scriptacties zijn scripts die worden uitgevoerd op de clusterknooppunten wanneer HDInsight clusters zijn geïmplementeerd en worden uitgevoerd zodra de knooppunten in het cluster HDInsight configuratie te voltooien. De scriptactie van een wordt uitgevoerd onder de account admin bevoegdheden en biedt volledige toegangsrechten voor de clusterknooppunten. Elk cluster kan worden geleverd met een lijst met scriptacties moet worden uitgevoerd in de volgorde waarin deze zijn opgegeven.

> [AZURE.NOTE] Als u het volgende foutbericht weergegeven:
>
>     System.Management.Automation.CommandNotFoundException; ExceptionMessage : The term 'Save-HDIFile' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
> Het is omdat u de methoden hebt opgenomen.  Zie [methoden voor aangepaste scripts](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).

## <a name="sample-scripts"></a>Voorbeeldscripts

Voor het maken van clusters van HDInsight op het Windows-besturingssysteem, wordt de actie Script Azure PowerShell-script. Het volgende is een voorbeeld van een script voor de configuratiebestanden configureren:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

Het script heeft vier parameters, de naam van het configuratie bestand, de eigenschap die u wijzigen wilt, wordt de waarde die u instellen wilt, en een beschrijving. Bijvoorbeeld:

    hive-site.xml hive.metastore.client.socket.timeout 90

Deze parameters wordt de waarde hive.metastore.client.socket.timeout ingesteld op 90 in de component site.xml-bestand.  De standaardwaarde is 60 seconden.

Dit voorbeeldscript kan ook worden gevonden op [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).

HDInsight biedt verschillende scripts voor de extra onderdelen installeren op HDInsight clusters:

Naam | Script
----- | -----
**Installatie van motoren** | https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. Zie [installeren en gebruiken Wek op clusters HDInsight][hdinsight-install-spark].
**R installeren** | https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Zie [installeren en gebruiken R op clusters HDInsight][hdinsight-r-scripts].
**Solr installeren** | https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Zie [installeren en gebruiken Solr op HDInsight](hdinsight-hadoop-solr-install.md).
- **Giraph installeren** | https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Zie [installeren en gebruiken Giraph op HDInsight](hdinsight-hadoop-giraph-install.md).

De scriptactie kan worden ingezet vanuit de portal Azure Azure PowerShell of met behulp van de HDInsight .NET SDK.  Zie voor meer informatie, [clusters van HDInsight aanpassen met behulp van de actie Script][hdinsight-cluster-customize].

> [AZURE.NOTE] De voorbeeldscripts werkt alleen met HDInsight cluster versie 3.1 of hoger. Zie voor meer informatie over de versies van HDInsight cluster, [cluster versies van HDInsight](hdinsight-component-versioning.md).





## <a name="helper-methods-for-custom-scripts"></a>Methoden voor aangepaste scripts

Actie script methoden zijn hulpprogramma's die u gebruiken kunt bij het schrijven van aangepaste scripts. Deze worden gedefinieerd in [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1)en kunnen worden opgenomen in uw scripts met behulp van de volgende opties:

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

Hier zijn de methoden die worden geleverd door dit script:

, Helpmethode | Beschrijving
-------------- | -----------
**Opslaan HDIFile** | Download een bestand van de opgegeven URI Uniform Resource Identifier () naar een locatie op de lokale vaste schijf die is gekoppeld aan het knooppunt Azure VM is toegewezen aan het cluster.
**Expand HDIZippedFile** | Een ZIP-bestand uitpakken.
**Aanroepen van HDICmdScript** | Een script uitvoeren vanaf de cmd.exe.
**Write-HDILog** | Uitvoer van het aangepaste script gebruikt voor de scriptactie van een schrijven.
**Get-Services** | Krijg een lijst met services op de computer waarop het script wordt uitgevoerd.
**Get-Service** | Met de specifieke naam als invoer, krijgt u gedetailleerde informatie voor een bepaalde service (servicenaam, proces-ID, status, enzovoort) op de computer waarop het script wordt uitgevoerd.
**Get-HDIServices** | Krijg een lijst met HDInsight services op de computer waarop het script wordt uitgevoerd.
**Get-HDIService** | Met de specifieke HDInsight servicenaam als invoer krijgt gedetailleerde informatie voor een bepaalde service (servicenaam, proces-ID, status, enzovoort) op de computer waarop het script wordt uitgevoerd.
**Get-ServicesRunning** | Krijg een lijst met services die worden uitgevoerd op de computer waarop het script wordt uitgevoerd.
**Get-ServiceRunning** | Controleer als een bepaalde service (op naam) wordt uitgevoerd op de computer waarop het script wordt uitgevoerd.
**Get-HDIServicesRunning** | Krijg een lijst met HDInsight services op de computer waarop het script wordt uitgevoerd.
**Get-HDIServiceRunning** | Controleer als een bepaalde service HDInsight (op naam) wordt uitgevoerd op de computer waarop het script wordt uitgevoerd.
**Get-HDIHadoopVersion** | De versie van Hadoop geïnstalleerd op de computer waarop het script wordt uitgevoerd te krijgen.
**Test IsHDIHeadNode** | Controleer of de computer waarop het script wordt uitgevoerd een hoofd knooppunt is.
**Test IsActiveHDIHeadNode** | Controleer of de computer waarop het script wordt uitgevoerd een actief knooppunt head is.
**Test IsHDIDataNode** | Controleer of de computer waarop het script wordt uitgevoerd een gegevensknooppunt is.
**Bewerken HDIConfigFile** | De config bestanden component-site.xml, core-site.xml, hdfs-site.xml, mapred site.xml of garens site.xml bewerken.


## <a name="best-practices-for-script-development"></a>Aanbevolen procedures voor het ontwikkelen van scripts

Wanneer u een aangepast script voor een cluster HDInsight ontwikkelt, zijn verschillende aanbevelingen rekening mee moet houden:

- Controleer welke versie van Hadoop

    Alleen HDInsight versie 3.1 (Hadoop 2.4) en boven de ondersteuning van aangepaste onderdelen installeren op een cluster met behulp van de actie Script. In een script, moet u de methode **Get HDIHadoopVersion** helper controleren de Hadoop versie voordat u doorgaat met het uitvoeren van andere taken in het script.


- Stabiele koppelingen naar scriptbronnen

    Gebruikers moeten ervoor zorgen dat alle scripts en andere onderdelen in het aanpassen van een cluster wordt gebruikt gedurende de levensduur van het cluster beschikbaar blijven en dat de versies van deze bestanden niet voor de duur wijzigen. Deze bronnen zijn vereist als u opnieuw imaging van knooppunten in het cluster is vereist. De beste manier is om te downloaden en alles in een opslag-account waarmee de gebruiker archiveren. Dit is de standaardaccount voor opslag of een van de extra opslag rekeningen die zijn opgegeven op het moment van de implementatie van een aangepaste cluster.
    In Spark en R aangepast cluster monsters, mits in de documentatie, bijvoorbeeld, we hebben een lokale kopie van de bronnen in deze rekening opslag: https://hdiconfigactions.blob.core.windows.net/.


- Zorg ervoor dat het cluster aanpassing script idempotency is ingeschakeld

    U moet verwachten dat de knooppunten van een cluster van HDInsight tijdens de levensduur van het cluster opnieuw afbeelding zal zijn. Het cluster aanpassing-script wordt uitgevoerd wanneer u een cluster opnieuw afbeelding is. Dit script moet zodanig zijn ontworpen dat idempotency is ingeschakeld in de zin dat bij het opnieuw imaging, het script ervoor zorgen moet dat het cluster wordt geretourneerd naar dezelfde aangepaste staat waarin deze zich bevond vlak nadat het script is uitgevoerd voor de eerste keer wanneer het cluster werd gemaakt. Als u een aangepast script een toepassing op D:\AppLocation geïnstalleerd op de eerste uitvoeren en op elke latere uitvoering, na opnieuw imaging, het script moet controleren of de toepassing op de locatie D:\AppLocation bestaat voordat u verdergaat met de andere in het script stappen.


- Aangepaste onderdelen installeren op de optimale locatie

    Als knooppunten van het cluster opnieuw afbeelding, de C:\ resource station en station D:\ kunnen worden opnieuw opgemaakt, wat resulteert in het verlies van gegevens en toepassingen die op deze stations zijn geïnstalleerd. Dit kan ook gebeuren als een knooppunt voor Azure VM (virtual machine) die deel uitmaakt van het cluster uitvalt en wordt vervangen door een nieuw knooppunt. U kunt onderdelen installeren op de D:\ station of op de locatie C:\apps op het cluster. Alle locaties op het station C:\ zijn gereserveerd. Geef de locatie waar de toepassingen of bibliotheken in het cluster aanpassing script moet worden geïnstalleerd.


- Hoge beschikbaarheid van de cluster-architectuur

    HDInsight heeft een actieve-passieve architectuur voor hoge beschikbaarheid, waarin een hoofd knooppunt in de actieve modus wordt (waarbij de HDInsight-services worden uitgevoerd) en het hoofd knooppunt in de modus Stand-by is (in welke HDInsight services niet worden uitgevoerd). De knooppunten schakelen actieve en passieve modus als HDInsight services worden onderbroken. Als een scriptactie wordt gebruikt om services te installeren op beide knooppunten head voor hoge beschikbaarheid, houd er rekening mee dat het mechanisme van de failover-HDInsight niet mogelijk deze gebruiker geïnstalleerde services automatisch worden overgeplaatst. Dus door de gebruiker geïnstalleerde services op HDInsight hoofd knooppunten waarvan wordt verwacht dat maximaal beschikbaar moeten hebben hun eigen mechanisme voor failover-als in de actieve-passieve modus of in de actieve modus.

    Een opdracht HDInsight scriptactie wordt uitgevoerd op beide knooppunten hoofd wanneer de rol hoofd-knooppunt wordt opgegeven als een waarde in de parameter *ClusterRoleCollection* . Dus wanneer u een aangepast script ontwerpen, zorg dat uw script zich bewust is van deze instellingen. Moet niet worden uitgevoerd op problemen waar dezelfde services zijn geïnstalleerd en gestart op beide knooppunten head en ze met elkaar concurreerden. Let op dat gegevens niet verloren tijdens opnieuw imaging, zodat software geïnstalleerd via een scriptactie te worden tegen dergelijke gebeurtenissen. Toepassingen moeten worden ontworpen voor gebruik met hoge beschikbaarheid van gegevens die verdeeld is over een groot aantal knooppunten. Houd er rekening mee dat maar liefst 1/5 van de knooppunten in een cluster opnieuw beeldbewerking op hetzelfde moment.


- De aangepaste onderdelen voor het gebruik van Azure Blob-opslag configureren

    De aangepaste onderdelen die u op de clusterknooppunten installeert hebt een standaardconfiguratie gebruiken Hadoop Distributed bestand System (HDFS) opslag. Wijzig de configuratie in plaats daarvan gebruik van Azure Blob-opslag. In een cluster opnieuw image, het HDFS bestandssysteem wordt geformatteerd en verliest u gegevens die er zijn opgeslagen. Azure Blob-opslag in plaats daarvan zorgt u ervoor dat uw gegevens blijven bewaard.

## <a name="common-usage-patterns"></a>Algemene gebruikspatronen

Deze sectie bevat instructies over het implementeren van enkele van de algemene gebruikspatronen die u tegenkomen kunt tijdens het schrijven van een eigen script.

### <a name="configure-environment-variables"></a>Omgevingsvariabelen configureren

Ontwikkeling van de actie script, zal u vaak nodig voor het instellen van omgevingsvariabelen voelen. Bijvoorbeeld is een waarschijnlijk scenario wanneer een binair bestand vanaf een externe site downloaden, op het cluster installeren en voeg de locatie van de aan de omgevingsvariabele 'Pad' is geïnstalleerd. In het volgende fragment laat zien hoe u omgevingsvariabelen worden ingesteld in het aangepaste script.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

Deze instructie wordt de variabele **MDS_RUNNER_CUSTOM_CLUSTER** ingesteld op de waarde 'true' en wordt het toepassingsgebied van deze variabele op alle computers worden ingesteld. Soms is het belangrijk dat omgevingsvariabelen worden ingesteld op het juiste bereik – computer of gebruiker. Zie [hier] [ 1] voor meer informatie over het instellen van omgevingsvariabelen.

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Toegang tot de locaties waar de aangepaste scripts zijn opgeslagen

Scripts die worden gebruikt voor het aanpassen van een cluster moeten ofwel worden in de standaardaccount voor de opslag voor het cluster of in een openbare container voor alleen-lezen op een andere account voor opslag. Als uw script toegang krijgt bronnen die zich elders bevindt tot deze moeten worden in een openbaar toegankelijke (ten minste openbare alleen-lezen). U wilt bijvoorbeeld een bestand openen en opslaan met de opdracht SaveFile HDI.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

In dit voorbeeld moet u ervoor zorgen dat de container 'somecontainer' in opslag account 'somestorageaccount' openbaar toegankelijk is. Anders wordt het script genereert een uitzondering 'Niet gevonden' en mislukken.

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a>Geef parameters door aan de cmdlet Add-AzureRmHDInsightScriptAction

Meerdere parameters doorgeven aan de cmdlet Add-AzureRmHDInsightScriptAction, moet u voor het opmaken van de tekenreekswaarde bevat alle parameters voor het script. Bijvoorbeeld:

    "-CertifcateUri wasbs:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

of

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Uitzondering voor mislukte cluster implementeren

Als u nauwkeurig de hoogte worden gebracht wilt van het feit dat de aanpassing van het cluster is niet gelukt zoals verwacht, is het belangrijk dat een uitzondering en niet het maken van het cluster. U wilt bijvoorbeeld een bestand verwerkt als deze bestaat en verwerken fout waar het bestand niet bestaat. Dit zou ervoor zorgen dat het script wordt netjes afgesloten en de status van het cluster goed bekend is. Het volgende fragment wordt een voorbeeld van hoe dit:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

In dit fragment, als het bestand niet bestaat, zou dit leiden tot een staat waarin het script daadwerkelijk wordt afgesloten zonder problemen na het afdrukken het foutbericht en het cluster actief, ervan uitgaande dat het "" voltooid cluster aanpassingsproces bereikt. Als u wilt nauwkeurig de hoogte gesteld van het feit dat de aanpassing in feite cluster naar behoren vanwege een ontbrekend bestand is mislukt, het beter is een uitzondering en mislukt de aanpassing cluster stap. Hiervoor moet u het volgende codefragment voor het voorbeeld gebruiken.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Controlelijst voor het implementeren van een scriptactie
Hier zijn de stappen die we hebben bij de opstelling van deze scripts implementeren:

1. Plaats de bestanden met de aangepaste scripts op een locatie die toegankelijk is voor de clusterknooppunten tijdens de implementatie. Dit is een van de standaard of extra opslag rekeningen die zijn opgegeven op het moment van het cluster distributie of opslag van openbaar toegankelijke containers.
2. Controles in scripts om ervoor te zorgen dat zij idempotently uitvoeren, zodat het script kan meerdere keren worden uitgevoerd op hetzelfde knooppunt toevoegen.
3. Met de **Uitvoer van schrijven** Azure PowerShell-cmdlet kunt afdrukken op STDOUT en STDERR. Gebruik geen **Write-Host**.
4. Gebruik een tijdelijke map, zoals $env: TEMP te houden van het gedownloade bestand door de scripts gebruikt en vervolgens opschonen van nadat de scripts zijn uitgevoerd.
5. Aangepaste software alleen op de D:\ of C:\apps installeren. Andere locaties op het station C: worden niet gebruikt zoals ze gereserveerd zijn. Houd er rekening mee dat bestanden op station C: buiten de map C:\apps geïnstalleerd leiden setup-fouten tijdens het opnieuw beelden van het knooppunt tot kan.
6. In het geval dat OS-niveau-instellingen of Hadoop service configuratiebestanden zijn gewijzigd, kunt u HDInsight-services opnieuw starten zodat ze een OS-instellingen op objectniveau, zoals de omgevingsvariabelen instellen in de scripts kunnen afhalen.

## <a name="debug-custom-scripts"></a>Foutopsporing van aangepaste scripts

De foutenlogboeken script worden opgeslagen en de andere output in de standaard opslag-account die u hebt opgegeven voor het cluster op het werd gemaakt. De logboekbestanden worden opgeslagen in een tabel met de naam *u < \cluster-name-fragment >< \time-stamp > Setuplog.txt*. Dit zijn de geaggregeerde logboeken die records uit alle knooppunten (hoofd-knooppunt en knooppunten van de werknemer) waarop het script wordt uitgevoerd in het cluster hebben.
HDInsight-hulpprogramma's gebruiken voor Visual Studio is een eenvoudige manier om de logboeken te controleren. Voor het installeren van de hulpprogramma's Zie [aan de slag met Hadoop met Visual Studio tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-hdinsight-tools-for-visual-studio)

**Controleer het logboek met behulp van Visual Studio**

1. Open de Visual Studio.
2. Klik op **weergave**en klik vervolgens op **Server Explorer**.
3. Met de rechtermuisknop op 'Azure', klikt u op verbinding maken met **Microsoft Azure abonnementen**en geef uw referenties op.
4. Vouw van **opslag**, de opslag Azure-account gebruikt als het standaardbestandssysteem uit, vouw **tabellen**en dubbelklik vervolgens op de naam van de tabel.


U kunt ook externe in om te zien dat de beide knooppunten STDOUT en STDERR voor aangepaste scripts. De logboeken op elk knooppunt gelden alleen voor dat knooppunt en worden vastgelegd in de **C:\HDInsightLogs\DeploymentAgent.log**. Deze logboekbestanden registreren alle uitvoer van een aangepast script. Een voorbeeld logboek fragment voor een actie Spark script ziet er zo uit:

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


In dit logboek is het duidelijk dat de vonk scriptactie is uitgevoerd op de VM met de naam HEADNODE0 en dat er geen uitzonderingen zijn gegenereerd tijdens de uitvoering.

In het geval dat een uitvoering van een fout optreedt, wordt de uitvoer met een beschrijving van het ook in dit bestand worden opgenomen. De informatie in deze logboeken moeten nuttig zijn bij het opsporen van fouten in script-problemen die zich kunnen voordoen.


## <a name="see-also"></a>Zie ook

- [HDInsight clusters met actie Script aanpassen][hdinsight-cluster-customize]
- [Installeren en gebruiken van motoren op HDInsight clusters][hdinsight-install-spark]
- [Installeren en gebruiken van R op clusters HDInsight][hdinsight-r-scripts]
- [Installeren en gebruiken Solr op HDInsight-clusters](hdinsight-hadoop-solr-install.md).
- [Installatie en gebruik Giraph op HDInsight-clusters](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx

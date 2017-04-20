<properties
    pageTitle="Python gebruiken met de component en varken in HDInsight | Microsoft Azure"
    description="Leren werken met Python gebruiker gedefinieerde functies (UDF) van de component en varkens in de HDInsight, het Hadoop-technologiestack op Azure."
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
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/07/2016" 
    ms.author="larryfr"/>

#<a name="use-python-with-hive-and-pig-in-hdinsight"></a>Python gebruiken met de component en varken in HDInsight

Component en varkens zijn ideaal voor het werken met gegevens in de HDInsight, maar soms moet u een meer algemeen doel van de taal. Zowel de component en varkens kunt u maakt de gebruiker gedefinieerde functies (UDF) met behulp van een aantal verschillende programmeertalen. In dit artikel leert u hoe u met een UDF Python van component en varken.

##<a name="requirements"></a>Vereisten

* Een cluster van HDInsight (Windows of Linux-gebaseerde)

* Een teksteditor

    > [AZURE.IMPORTANT] Als u een server Linux-gebaseerde HDInsight, maar de Python bestanden maken op een Windows-client, moet u een editor die gebruikmaakt van LF als een lijn-uitgang. Als u niet zeker weet of uw editor LF of CRLF gebruikt, Zie de sectie [problemen oplossen](#troubleshooting) voor instructies over het gebruik van hulpprogramma's op het cluster HDInsight CR-teken wordt verwijderd.
    
##<a name="python"></a>Python op HDInsight

Python2.7 is standaard geïnstalleerd op HDInsight 3.0 en hoger clusters. Component kan worden gebruikt met deze versie van Python voor verwerking van stroom (de gegevens worden doorgegeven tussen component en Python met STDOUT/STDIN).

HDInsight tevens Jython een Python implementatie in Java is geschreven. Varken begrijpt hoe om te spreken Jython zonder gebruik te maken van streaming, dus het verdient de voorkeur bij het gebruik van varkens. U kunt normale Python (C-Python), echter ook gebruiken met varken ook.

##<a name="hivepython"></a>Component en Python

Python kan worden gebruikt als een UDF uit component via de **TRANSFORMATIE** HiveQL instructie. De volgende HiveQL wordt bijvoorbeeld een Python-script dat is opgeslagen in het bestand **streaming.py** .

**Linux-gebaseerde HDInsight**

    add file wasbs:///streaming.py;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
      USING 'python streaming.py' AS
      (clientid string, phoneLable string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;

**Windows-gebaseerde HDInsight**

    add file wasbs:///streaming.py;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
      USING 'D:\Python27\python.exe streaming.py' AS
      (clientid string, phoneLable string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;

> [AZURE.NOTE] Op Windows gebaseerde HDInsight clusters, moet de component **met** het volledige pad naar de python.exe opgeven. Dit is altijd `D:\Python27\python.exe`.

Dit is wat in het volgende voorbeeld wordt:

1. De instructie **bestand toevoegen** aan het begin van het bestand wordt het bestand **streaming.py** toegevoegd aan de gedistribueerde cache, zodat deze toegankelijk voor alle knooppunten in het cluster is.

2. De TRANSFORMATIE **selecteren... Met behulp van** instructie om de gegevens te selecteren uit de **hivesampletable**en clientid, devicemake en devicemodel doorgegeven aan het script **streaming.py** .

3. Beschrijving van de component **AS** de velden geretourneerd door **streaming.py**

<a name="streamingpy"></a>Hier is het **streaming.py** dat wordt gebruikt door het voorbeeld HiveQL.

    #!/usr/bin/env python

    import sys
    import string
    import hashlib

    while True:
      line = sys.stdin.readline()
      if not line:
        break

      line = string.strip(line, "\n ")
      clientid, devicemake, devicemodel = string.split(line, "\t")
      phone_label = devicemake + ' ' + devicemodel
      print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])

Aangezien we streaming gebruikt, heeft dit script het volgende doen:

1. Gegevens lezen van STDIN. Dit wordt gedaan met behulp van `sys.stdin.readline()` in dit voorbeeld.

2. Aan het einde-regelteken wordt verwijderd met behulp van `string.strip(line, "\n ")`, omdat we alleen de tekst en niet het einde van Regelindicator.

2. Bij het uitvoeren van stream processing bevat één regel de waarden met een tabteken tussen elke waarde. Dus `string.split(line, "\t")` kan worden gebruikt om de invoer op elk tabblad retourneert alleen de velden te splitsen.

3. Wanneer de verwerking is voltooid, moet de uitvoer naar STDOUT worden geschreven als een enkele lijn, met een tab tussen de velden. Dit wordt gedaan met behulp van `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.

4. Dit alles vindt plaats in een `while` lus dat wordt herhaald totdat geen `line` wordt gelezen, waarna `break` sluit de lus en het script wordt beëindigd.

Verder worden het script alleen de invoerwaarden voor `devicemake` en `devicemodel`, en wordt een hash van de samengevoegde waarde berekend. Mooie eenvoudige, maar beschrijft de basisbeginselen van hoe Python script aangeroepen vanuit component zou moeten functioneren: herhalen, input lezen totdat er niet meer splitsen elke regel van de invoer op de tabbladen proces, het schrijven van één regel met uitvoer met tabs gescheiden.

Zie [de voorbeelden wordt uitgevoerd](#running) voor het uitvoeren van dit voorbeeld op de cluster HDInsight.

##<a name="pigpython"></a>Varkens en Python

Een Python script kan worden gebruikt als een UDF van varken via de instructie **genereren** . Er is op twee manieren doen; met behulp van Jython (Python geïmplementeerd op de Java Virtual Machine) en C-Python (gewone Python). 

Het belangrijkste verschil tussen deze zijn dat Jython wordt uitgevoerd op de JVM en zichzelf kunnen worden aangeroepen vanuit varken (ook waarop de JVM.) C Python is een extern proces (geschreven in c) Zodat de gegevens van het varken op de JVM is verzonden naar het script dat wordt uitgevoerd in een proces van Python en de uitvoer van die wordt verzonden naar varkens.

Om te bepalen of varken Jython of Python, C gebruikt voor het uitvoeren van het script, gebruik __registreren__ wanneer u verwijst naar de Python-script uit Latijnse varken. Dit vertelt varken welke-interpreter te gebruiken en welke alias maken voor het script. De volgende voorbeelden voor het registreren van scripts met varken als __myfuncs__:

* __Jython gebruiken__:`register '/path/to/pig_python.py' using jython as myfuncs;`
* __C Python gebruiken__:`register '/path/to/pig_python.py' using streaming_python as myfuncs;`

> [AZURE.IMPORTANT] Wanneer u Jython, het pad naar het bestand pig_jython is een lokaal pad of een WASB: / / pad. Echter, wanneer u C, Python, moet u verwijzen naar een bestand op het lokale bestandssysteem van het knooppunt dat u gebruikt voor het indienen van de taak van het varken.

Eenmaal voorbij de registratie, de Latijnse varken voor dit voorbeeld is hetzelfde voor zowel:

    LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
    LOG = FILTER LOGS by LINE is not null;
    DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
    DUMP DETAILS;

Dit is wat in het volgende voorbeeld wordt:

1. De eerste regel laadt het voorbeeldgegevensbestand **sample.log** in **LOGBOEKEN**. Aangezien dit bestand geen schema consistent blijft, worden ook elke record (**regel** in dit geval) als een **chararray**definieert. Chararray is in wezen een tekenreeks.

2. De volgende regel gefilterd op een null-waarden en het resultaat van de bewerking in het **logboek**op te slaan.

3. Vervolgens een iteratie over de records in het **logboek** en **genereren** gebruikt voor het aanroepen van de methode **create_structure** is opgenomen in de Python/Jython script als **myfuncs**geladen.  **Regel** wordt gebruikt voor de huidige record doorgeven aan de functie.

4. Ten slotte worden de uitgangen in met de opdracht **DUMP** STDOUT gedumpt. Dit is om direct de resultaten weergeven nadat de bewerking is voltooid; in een echte script normaal u **WINKEL** de gegevens in een nieuw bestand.

Het Python scriptbestand lijkt ook tussen C Python en Jython, het enige verschil dat u moet importeren uit __varkens\_util__ bij het gebruik van Python C. Hier wordt de __varkens\_python.py__ script:

<a name="streamingpy"></a>

    # Uncomment the following if using C Python
    #from pig_util import outputSchema

    @outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
    def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail

> [AZURE.NOTE] 'pig_util' is niet iets die hoeft te installeren, moet u het is automatisch beschikbaar voor het script.

Houd er rekening mee dat we eerder de zojuist **regel** ingevoerd als een chararray gedefinieerde, omdat er geen consistente schema voor de invoer is? Wat de Python script doet is de gegevens transformeren naar een consistente schema voor uitvoer. Het werkt als volgt:

1. De **@outputSchema** instructie definieert de indeling van de gegevens die worden teruggestuurd naar de varkens. In dit geval is het een **zak met gegevens**die het gegevenstype van een varken. De zak bevat de volgende velden, die allemaal chararray (een reeks zijn):

    * datum: de datum waarop die de logboekvermelding is gemaakt.
    * tijd - de tijd waarop die de logboekvermelding is gemaakt.
    * klassenaam - naam van de klasse de post is gemaakt voor
    * niveau - het niveau voor vastleggen
    * detail - uitgebreide details voor de vermelding

2. Vervolgens wordt definieert het **def create_structure(input)** de functie die varkens regelartikelen wordt doorgegeven.

3. Van de voorbeeldgegevens, **sample.log**, voldoet grotendeels aan de datum, tijd, classname, niveau en gedetailleerd schema die we als resultaat wilt geven. Maar bevat ook enkele regels die beginnen met de tekenreeks '*java.lang.Exception*' die moeten worden aangepast aan het schema. De **if** -instructie die worden gecontroleerd en vervolgens de ingevoerde gegevens te verplaatsen van de tekenreeks '*java.lang.Exception*' aan het einde, waardoor de gegevens in-lijn met onze verwachte output schema massages.

4. Vervolgens wordt wordt de opdracht **splitsen** gebruikt voor het opsplitsen van de gegevens op de eerste vier tekens. Dit resulteert in vijf waarden die zijn toegewezen aan de **datum**, **tijd**, **classname**, **niveau**en **Details**.

5. Ten slotte worden de waarden geretourneerd naar de varkens.

Wanneer de gegevens weer varkens, is er een consistente schema zoals omschreven in de **@outputSchema** instructie.

##<a name="running"></a>De voorbeelden uitvoeren

Als u een HDInsight op basis van Linux-cluster gebruik **SSH** volgt. Als u een cluster met Windows-gebaseerde HDInsight en een Windows-client gebruikt, moet u de stappen **PowerShell** gebruiken.

###<a name="ssh"></a>SSH

Zie voor meer informatie over het gebruik van SSH <a href="../hdinsight-hadoop-linux-use-ssh-unix/" target="_blank">Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Linux, Unix, of OS X</a> of <a href="../hdinsight-hadoop-linux-use-ssh-windows/" target="_blank">Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Windows</a>.

1. Met de Python voorbeelden [streaming.py](#streamingpy) en [pig_python.py](#jythonpy), maakt u lokale kopieën van de bestanden op de ontwikkelcomputer.

2. Gebruik `scp` de bestanden kopiëren naar uw cluster HDInsight. Bijvoorbeeld, de volgende zou de bestanden kopiëren naar een cluster met de naam **mijncluster**.

        scp streaming.py pig_python.py myuser@mycluster-ssh.azurehdinsight.net:

3. Met SSH kunt u verbinding maken met het cluster. Bijvoorbeeld de volgende verbinding te maken met een cluster met de naam **mijncluster** als gebruiker **myuser**.

        ssh myuser@mycluster-ssh.azurehdinsight.net

4. Toevoegen van de SSH-sessie de python bestanden eerder geüpload naar de WASB opslag voor het cluster.

        hdfs dfs -put streaming.py /streaming.py
        hdfs dfs -put pig_python.py /pig_python.py

Gebruik de volgende stappen uit te voeren taken van de component en varken na het uploaden van bestanden.

####<a name="hive"></a>Component

1. Gebruik de `hive` opdracht de component shell te starten. U ziet nu een `hive>` wordt gevraagd nadat de shell is geladen.

2. Typ het volgende achter de `hive>` vragen.

        add file wasbs:///streaming.py;
        SELECT TRANSFORM (clientid, devicemake, devicemodel)
          USING 'python streaming.py' AS
          (clientid string, phoneLabel string, phoneHash string)
        FROM hivesampletable
        ORDER BY clientid LIMIT 50;

3. Na het invoeren van de laatste regel, moet de taak beginnen. Uiteindelijk wordt geretourneerd uitvoer de volgende strekking.

        100041  RIM 9650    d476f3687700442549a83fac4560c51c
        100041  RIM 9650    d476f3687700442549a83fac4560c51c
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9

####<a name="pig"></a>Varken

1. Gebruik de `pig` opdracht de shell te starten. U ziet nu een `grunt>` wordt gevraagd nadat de shell is geladen.

2. Voer de volgende instructies uit op de `grunt>` vragen de Python script met behulp van de interpreter Jython uit te voeren.

        Register wasbs:///pig_python.py using jython as myfuncs;
        LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
        DUMP DETAILS;

3. Na het invoeren van de volgende regel, moet de taak beginnen. Uiteindelijk wordt geretourneerd uitvoer de volgende strekking.

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. Gebruik `quit` de Grunt-shell afsluiten en gebruikt u de volgende voor het bewerken van het bestand pig_python.py op het lokale bestandssysteem:

    nano-pig_python.py

5. Eenmaal in de editor, verwijder de opmerkingen van de volgende regel door het verwijderen van de `#` teken vanaf het begin van de regel:

        #from pig_util import outputSchema

    Nadat de wijziging is aangebracht, Ctrl + X om af te sluiten van de editor gebruiken. Selecteer Y en voer vervolgens de wijzigingen op te slaan.

6. Gebruik de `pig` de opdracht om de shell opnieuw te starten. Zodra u op de `grunt>` wordt gevraagd, gebruikt u de volgende de Python script met behulp van de C-Python interpreter uit te voeren.

        Register 'pig_python.py' using streaming_python as myfuncs;
        LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
        DUMP DETAILS;

    Zodra deze taak is voltooid, ziet u hetzelfde resultaat als wanneer u het script met behulp van Jython eerder is uitgevoerd.

###<a name="powershell"></a>PowerShell

Deze stappen Azure PowerShell gebruiken. Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) als dit nog niet is geïnstalleerd en geconfigureerd op de ontwikkelcomputer, voordat u de volgende stappen uit.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. Met de Python voorbeelden [streaming.py](#streamingpy) en [pig_python.py](#jythonpy), maakt u lokale kopieën van de bestanden op de ontwikkelcomputer.

2. De volgende PowerShell script gebruiken voor het uploaden van de **streaming.py** en **varkens\_python.py** bestanden naar de server. Vervang de naam van het cluster Azure HDInsight en het pad naar de **streaming.py** en **varkens\_python.py** bestanden op de eerste drie regels van het script.

        $clusterName = YourHDIClusterName
        $pathToStreamingFile = "C:\path\to\streaming.py"
        $pathToJythonFile = "C:\path\to\pig_python.py"

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
            -File $pathToStreamingFile `
            -Blob "streaming.py" `
            -Container $container `
            -Context $context
        
        Set-AzureStorageBlobContent `
            -File $pathToJythonFile `
            -Blob "pig_python.py" `
            -Container $container `
            -Context $context

    Dit script haalt de gegevens voor uw cluster HDInsight en vervolgens haalt de account en sleutel voor de standaardaccount voor de opslag en de bestanden kunnen uploaden naar de hoofdmap van de container.

    > [AZURE.NOTE] Andere methoden voor het uploaden van de scripts vindt u in het document [uploaden van gegevens voor Hadoop projecten in HDInsight](hdinsight-upload-data.md) .

Na het uploaden van de bestanden met de volgende PowerShell scripts kunt starten de taken. Wanneer de taak is voltooid, moet de uitvoer naar de PowerShell-console worden geschreven.

####<a name="hive"></a>Component

Het volgende script wordt het script __streaming.py__ uitvoeren. Voordat u, wordt u gevraagd voor de gegevens van de HTTPs/Admin voor uw cluster HDInsight.

    # Replace 'YourHDIClusterName' with the name of your cluster
    $clusterName = YourHDIClusterName
    $creds=Get-Credential
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
    
    # If using a Windows-based HDInsight cluster, change the USING statement to:
    # "USING 'D:\Python27\python.exe streaming.py' AS " +
    $HiveQuery = "add file wasbs:///streaming.py;" +
                 "SELECT TRANSFORM (clientid, devicemake, devicemodel) " +
                   "USING 'python streaming.py' AS " +
                   "(clientid string, phoneLabel string, phoneHash string) " +
                 "FROM hivesampletable " +
                 "ORDER BY clientid LIMIT 50;"

    $jobDefinition = New-AzureRmHDInsightHiveJobDefinition `
        -Query $HiveQuery

    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for the Hive job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -JobId $job.JobId `
        -ClusterName $clusterName `
        -HttpCredential $creds
    # Uncomment the following to see stderr output
    # Get-AzureRmHDInsightJobOutput `
    #   -Clustername $clusterName `
    #   -JobId $job.JobId `
    #   -DefaultContainer $container `
    #   -DefaultStorageAccountName $storageAccountName `
    #   -DefaultStorageAccountKey $storageAccountKey `
    #   -HttpCredential $creds `
    #   -DisplayOutputType StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds

De uitvoer voor het project **component** moet worden met de volgende strekking weergegeven:

    100041  RIM 9650    d476f3687700442549a83fac4560c51c
    100041  RIM 9650    d476f3687700442549a83fac4560c51c
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9

####<a name="pig-jython"></a>Varken (Jython)

De volgende gebruikt het script __pig_python.py__ , met behulp van de Jython-interpreter. Voordat u, wordt u gevraagd de informatie HTTPs/Admin voor de cluster HDInsight.

> [AZURE.NOTE] Wanneer een taak met PowerShell op afstand in te dienen, is het niet mogelijk te C Python gebruiken als de interpreter.

    # Replace 'YourHDIClusterName' with the name of your cluster
    $clusterName = YourHDIClusterName

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
            
    $PigQuery = "Register wasbs:///jython.py using jython as myfuncs;" +
                "LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);" +
                "LOG = FILTER LOGS by LINE is not null;" +
                "DETAILS = foreach LOG generate myfuncs.create_structure(LINE);" +
                "DUMP DETAILS;"

    $jobDefinition = New-AzureRmHDInsightPigJobDefinition -Query $PigQuery

    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
        
    Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -Job $job.JobId `
        -ClusterName $clusterName `
        -HttpCredential $creds
    # Uncomment the following to see stderr output
    # Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds

De uitvoer voor de taak van de **varkens** moet worden met de volgende strekking weergegeven:

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

##<a name="troubleshooting"></a>Het oplossen van problemen

###<a name="errors-when-running-jobs"></a>Fout bij het uitvoeren van taken

Wanneer u de component uitvoert, kan een foutbericht zoals het volgende optreden:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.
    
Dit probleem kan worden veroorzaakt door de regeleinden in het bestand streaming.py. Veel redacteuren standaard Windows CRLF meestal als de laatste regel, maar de Linux-toepassingen gebruiken LF verwacht.

Als u een editor die LF regeleinden niet maken of niet zeker weet welke regeleinden worden gebruikt, gebruikt u de volgende PowerShell instructies de CR om tekens te verwijderen voordat u het bestand uploadt naar HDInsight:

    $original_file ='c:\path\to\streaming.py'
    $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
    [IO.File]::WriteAllText($original_file, $text)

###<a name="powershell-scripts"></a>PowerShell-scripts

Beide voorbeeld PowerShell scripts die worden gebruikt voor het uitvoeren van de voorbeelden bevatten een commentaarregel die uitvoer van foutmeldingen voor de taak wordt weergegeven. Als u de verwachte output voor de taak niet ziet, wordt de volgende regel en zien als de gegevens van de duidt op een probleem.

    # Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

De gegevens van de (STDERR) en het resultaat van het project (STDOUT), worden ook geregistreerd in de standaardcontainer blob voor clusters op de volgende locaties.

Voor deze taak...|Bekijk deze bestanden in de blob-container
---|---
Component|/ HivePython/stderr<p>/ HivePython/stdout
Varken|/ PigPython/stderr<p>/ PigPython/stdout

##<a name="next"></a>Volgende stappen

Als u Python modules die standaard niet worden geladen, Zie [het implementeren van een module op Azure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx) voor een voorbeeld van hoe u dit doet.

Voor andere manieren om varkens, component, en Zie het volgende voor meer informatie over het gebruik van MapReduce.

* [Gebruik component met HDInsight](hdinsight-use-hive.md)

* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)

* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)

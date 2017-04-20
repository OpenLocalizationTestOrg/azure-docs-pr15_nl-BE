<properties
pageTitle="HDInsight toegang tot gegevens met behulp van handtekeningen voor gedeelde toegang beperken"
description="Leren werken met gedeelde toegang handtekeningen HDInsight toegang tot gegevens die zijn opgeslagen in de opslag van Azure BLOB's te beperken."
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
ms.date="10/11/2016"
ms.author="larryfr"/>

#<a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-with-hdinsight"></a>Azure opslag gedeelde toegang handtekeningen gebruiken om toegang te beperken tot gegevens met HDInsight

Opslag van Azure BLOB's HDInsight gebruikt voor gegevensopslag. HDInsight moet beschikken over volledige toegang tot de blob gebruikt als standaard opslag voor het cluster, kunt u machtigingen voor gegevens die zijn opgeslagen in andere blobs die door het cluster worden gebruikt te beperken. U kunt bijvoorbeeld bepaalde gegevens alleen-lezen maken. U kunt dit doen met gedeelde toegang handtekeningen.

Gedeelde toegang handtekeningen (SAS) zijn een functie van Azure opslag rekeningen waarmee u toegang tot gegevens te beperken. Bijvoorbeeld, alleen-lezen toegang verlenen tot gegevens. In dit document leert u hoe SAS om te lezen en lijst alleen schakelen naar een blob-container van HDInsight.

##<a name="requirements"></a>Vereisten

* Een abonnement op Azure

* C# of Python. C#-voorbeeldcode wordt geleverd als een Visual Studio-oplossing.

    * Visual Studio moet versie 2013 of 2015
    
    * Python moet versie 2.7 of hoger zijn

* Een HDInsight op basis van Linux cluster of [Azure PowerShell] [ powershell] -als u een bestaand cluster op basis van Linux hebt, kunt u Ambari naar een gedeelde Access-handtekening toevoegen aan het cluster. Als dat niet het geval is, kunt u Azure PowerShell een nieuw cluster maken en een gedeelde Access-handtekening toevoegen tijdens het maken van het cluster.

* Van de voorbeeldbestanden uit de [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Deze opslagplaats is het volgende:

    * Een Visual Studio-project dat een container voor opslag, SAS en opgeslagen beleid voor gebruik met HDInsight maken kunt
    
    * Een Python-script dat een container voor opslag, SAS en opgeslagen beleid voor gebruik met HDInsight maken kunt
    
    * Een PowerShell script dat kan een nieuw HDInsight-cluster maken en configureren voor het gebruik van de SA's.

##<a name="shared-access-signatures"></a>Gedeelde toegang handtekeningen

Er zijn twee vormen van gedeelde Access-handtekeningen:

* Ad hoc: de begintijd, verlooptijd en machtigingen voor de SA's worden alle opgegeven naar de URI SAS (of impliciet, in het geval waarin de begintijd is weggelaten).

* -Beleid opgeslagen: een opgeslagen-beleid is gedefinieerd in een container voor resource - een blob-container, tabel, wachtrij of bestandsshare - en beheren voor een of meer gedeelde toegang handtekeningen kunnen worden gebruikt. Wanneer u een SAS aan een opgeslagen-beleid koppelt, neemt de SA's beperkingen - de begintijd, verlooptijd en machtigingen - definitie voor het opgeslagen-beleid.

Het verschil tussen de twee formulieren is belangrijk voor een belangrijke scenario: intrekking. Een SAS is een URL, zodat iedereen die de SA's ontvangt kan worden gebruikt, ongeacht wie eerst aangevraagd. Als een SAS openbaar wordt gepubliceerd, kan het worden gebruikt door iedereen in de wereld. Een SA's dat wordt verdeeld is geldig tot vier dingen gebeurt:

1. De opgegeven op de SAS verlooptijd is bereikt.

2. De verlooptijd die is opgegeven in de opgeslagen toegangsbeleid waarnaar wordt verwezen door de SA's is bereikt (als een opgeslagen-beleid wordt verwezen en als het een verlooptijd aangeeft). Dit kan ofwel gebeuren omdat het interval is verstreken, of omdat u het beleid opgeslagen access als u wilt dat een verlooptijd in het verleden, één manier is om het intrekken van de SA's hebt gewijzigd.

3. De opgeslagen toegangsbeleid waarnaar wordt verwezen door de SA's worden verwijderd, is een andere manier in te trekken van de SA's. Zorg ervoor dat als u het beleid opgeslagen access met dezelfde naam opnieuw te maken, alle bestaande SAS-tokens wordt opnieuw geldig is volgens de machtigingen die zijn gekoppeld aan dat access opgeslagen beleid (ervan uitgaande dat de verlooptijd van de SAS niet is verstreken). Als u met het intrekken van de SA's zijn voornemen, moet een andere naam te gebruiken als u het toegangsbeleid met een verlooptijd in de toekomst opnieuw kunt maken.

4. De account-sleutel die is gebruikt voor het maken van de SA's opnieuw gegenereerd. Houd er rekening mee dat hierdoor alle componenten met die sleutel rekening worden te verifiëren totdat deze zijn bijgewerkt voor het gebruik van de andere geldige sleutel of de sleutel nieuwe geregenereerde account is mislukt.

> [AZURE.IMPORTANT] Een handtekening voor gedeelde toegang URI is gekoppeld aan de account-sleutel gebruikt om de handtekening te maken en de bijbehorende opgeslagen-beleid (indien aanwezig). Als er geen opgeslagen-beleid is opgegeven, is de enige manier om een handtekening gedeelde toegang intrekken om de sleutel van de account te wijzigen. 

Het verdient aanbeveling-opgeslagen beleid altijd te gebruiken zodat u kunt handtekeningen intrekken of uitbreiden van de uiterste gebruiksdatum indien nodig. De stappen in dit document opgeslagen-beleid voor het genereren van SAS.

Voor meer informatie over handtekeningen voor gedeelde toegang Zie [informatie over het model SAS](../storage/storage-dotnet-shared-access-signature-part-1.md).

##<a name="create-a-stored-policy-and-generate-a-sas"></a>Een opgeslagen beleid maken en genereren een SAS

Op dit moment moet u een opgeslagen beleid via programmacode. U vindt de C#- en Python voorbeeld van het maken van een opgeslagen beleid en SA's op [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).

###<a name="create-a-stored-policy-and-sas-using-c"></a>Een opgeslagen beleid en SA's met behulp van C maken\#

1. Open de oplossing in Visual Studio.

2. In de Solution Explorer met de rechtermuisknop op het __SASToken__ -project en selecteer __Eigenschappen__.

3. Selecteer __Instellingen__ en waarden toevoegen voor de volgende vermeldingen:

    * StorageConnectionString: De verbindingsreeks voor de opslag-account die u wilt maken van een opgeslagen beleid en Beveiligingskoppelingen. Moet worden `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` waarbij `myaccount` is de naam van uw account voor opslag en `mykey` is de sleutel voor de opslag.
    
    * ContainerName: De container in de opslag die u toegang wilt beperken tot.
    
    * SASPolicyName: De naam van het opgeslagen beleid dat wordt gemaakt.
    
    * FileToUpload: Het pad naar een bestand dat wordt geüpload naar de container.
    
4. Het project wordt uitgevoerd. Een consolevenster weergegeven en informatie van de volgende strekking wordt weergegeven wanneer de SA's is gegenereerd:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
        
    Sla het token SAS beleid, moet u dit bij de opslag-account koppelen aan uw cluster HDInsight. Ook moet u de naam van de opslag en de containernaam van de.
    
###<a name="create-a-stored-policy-and-sas-using-python"></a>Een opgeslagen beleid en SA's met Python maken

1. Open het bestand SASToken.py en wijzigt u de volgende waarden:

    * beleid voor\_naam: de naam voor het opgeslagen beleid dat wordt gemaakt.
    
    * opslag\_account\_naam: de naam van de account van uw opslag.
    
    * opslag\_account\_sleutel: de sleutel voor de opslag.
    
    * opslag\_container\_naam: de container in de opslag-account die u toegang wilt beperken tot.
    
    * Voorbeeld van de\_bestand\_pad: het pad naar een bestand dat wordt geüpload naar de container

2. Het script uitvoeren. De SAS-token met de volgende strekking wordt weergegeven wanneer het script is voltooid:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
    
    Sla het token SAS beleid, moet u dit bij de opslag-account koppelen aan uw cluster HDInsight. Ook moet u de naam van de opslag en de containernaam van de.
    
##<a name="use-the-sas-with-hdinsight"></a>De SA's gebruiken met HDInsight

Bij het maken van een HDInsight-cluster, moet u een primaire opslag-account en kunt u desgewenst extra opslagruimte rekeningen opgeven. Beide methoden voor het toevoegen van opslag is vereist voor volledige toegang tot de opslag en containers die worden gebruikt.

Een gedeelde Access-handtekening gebruiken toegang tot een container wilt beperken, moet u een aangepaste vermelding toevoegen aan de __core-site__ configureren voor het cluster.

* Voor clusters met __Windows-__ of __Linux-gebaseerde__ HDInsight, kunt u dit doen tijdens het maken van het cluster met PowerShell.

* Voor __Linux-gebaseerde__ HDInsight clusters, kunt u de configuratie na het maken van het cluster met behulp van de Ambari wijzigen.

###<a name="create-a-new-cluster-that-uses-the-sas"></a>Maak een nieuw cluster met de SAS

Een voorbeeld van het maken van een cluster van HDInsight die gebruikmaakt van de SA's is opgenomen in de `CreateCluster` map van de bibliotheek. Om dit te gebruiken, gebruikt u de volgende stappen uit:

1. Open de `CreateCluster\HDInsightSAS.ps1` -bestand in een teksteditor en wijzig de volgende waarden aan het begin van het document.

        # Replace 'mycluster' with the name of the cluster to be created
        $clusterName = 'mycluster'
        # Valid values are 'Linux' and 'Windows'
        $osType = 'Linux'
        # Replace 'myresourcegroup' with the name of the group to be created
        $resourceGroupName = 'myresourcegroup'
        # Replace with the Azure data center you want to the cluster to live in
        $location = 'North Europe'
        # Replace with the name of the default storage account to be created
        $defaultStorageAccountName = 'mystorageaccount'
        # Replace with the name of the SAS container created earlier
        $SASContainerName = 'sascontainer'
        # Replace with the name of the SAS storage account created earlier
        $SASStorageAccountName = 'sasaccount'
        # Replace with the SAS token generated earlier
        $SASToken = 'sastoken'
        # Set the number of worker nodes in the cluster
        $clusterSizeInNodes = 2
        
    Wijzig bijvoorbeeld `'mycluster'` op de naam van het cluster dat u wilt maken. De SAS-waarden moeten overeenkomen met de waarden uit de vorige stap biedt bij het maken van een opslag- en SAS-token.
    
    Als u de waarden hebt gewijzigd, moet u het bestand opslaan.

1. Open een nieuwe Azure PowerShell-prompt. Als u niet bekend met Azure PowerShell bent of niet hebt geïnstalleerd, raadpleegt u [installeren en configureren van Azure PowerShell][powershell].

2. Vanaf de prompt, gebruikt u de volgende om uw abonnement op Azure te verifiëren:

        Login-AzureRmAccount
    
    Wanneer dat wordt gevraagd, meld u aan met de account voor uw abonnement op Azure.
    
    Als uw gebruikersnaam gekoppeld aan meerdere abonnementen voor Azure is, moet u mogelijk gebruik van `Select-AzureRmSubscription` te selecteren van het abonnement dat u wilt gebruiken.

2. Wijzigen vanaf de opdrachtprompt, mappen naar de `CreateCluster` de map met het bestand HDInsightSAS.ps1. Gebruik de volgende vervolgens het script uitvoeren
        
        .\HDInsightSAS.ps1
    
    Als het script wordt uitgevoerd, wordt deze uitvoer Meld u bij de PowerShell-prompt als de bron en de opslag accounts gemaakt. Er wordt u gevraagd de HTTP-gebruiker invoeren voor het cluster HDInsight. Dit is de gebruikersaccount die wordt gebruikt voor het beveiligen van toegang tot het cluster HTTP/s.
    
    Als u een Linux-gebaseerde cluster maakt, wordt u ook een SSH gebruikersnaam en wachtwoord gevraagd. Dit wordt gebruikt om extern aanmelden bij het cluster.
    
    > [AZURE.IMPORTANT] Wanneer het HTTP/s of SSH-gebruikersnaam en wachtwoord wordt gevraagd, dient u een wachtwoord dat voldoet aan de volgende criteria:
    >
    > - Ten minste 10 tekens lang moet zijn
    > - Moet ten minste één cijfer bevatten
    > - Moet ten minste één niet-alfanumerieke tekens bevatten
    > - Moet ten minste één hoofdletters of kleine letter bevatten


Het duurt een tijdje om dit script te voltooien, meestal ongeveer 15 minuten. Wanneer het script is voltooid zonder fouten, is het cluster gemaakt.

###<a name="update-an-existing-cluster-to-use-the-sas"></a>Een bestaand cluster voor het gebruik van de SA's bijwerken

Als u een bestaand cluster op basis van Linux, kunt u de SA's in de configuratie van de __core-site__ toevoegen met behulp van de volgende stappen uit:

1. Open het Ambari web UI voor uw cluster. Het adres voor deze pagina is https://YOURCLUSTERNAME.azurehdinsight.net. Wanneer dat wordt gevraagd, worden geverifieerd bij het cluster met de naam van de beheerder (admin) en wachtwoord die u gebruikt bij het maken van het cluster.

2. Vanaf de linkerkant van het Ambari web UI __HDFS__ selecteren en selecteer vervolgens het tabblad __configuraties__ in het midden van de pagina.

3. Selecteer het tabblad __Geavanceerd__ en schuif vervolgens totdat u de sectie __aangepaste core-site__ .

4. Vouw de sectie __aangepaste core-site__ en Ga naar het einde en selecteer de koppeling __... eigenschap toevoegen__ . Gebruik de volgende waarden voor de __ __sleutel__ - en__ velden:

    * __Sleutel__: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
    
    * __Waarde__: de SA's die wordt geretourneerd door de C# of Python toepassing die u eerder hebt uitgevoerd
    
    Vervang __CONTAINERNAME__ met de containernaam die u met de C#- of SAS-toepassing wordt gebruikt. __STORAGEACCOUNTNAME__ vervangen door de naam van de opslag die u gebruikt.

5. Klik op de knop __toevoegen__ om op te slaan voor deze sleutel en de waarde en klik vervolgens op de knop __Opslaan__ om de wijzigingen in de configuratie opslaan. Wanneer dat wordt gevraagd, een beschrijving van de verandering ('toevoegen SAS-opslagtoegang' bijvoorbeeld) toevoegen en klik vervolgens op __Opslaan__.

    Klik op __OK__ wanneer de wijzigingen zijn voltooid.

    > [AZURE.IMPORTANT] Hiermee slaat u wijzigingen in de configuratie, maar u moet verschillende services opnieuw starten voordat de wijziging wordt van kracht.

6. In het Ambari web UI __HDFS__ selecteren in de lijst aan de linkerkant en selecteer __Alles opnieuw starten__ van de __Serviceacties__ vervolgkeuzelijst aan de rechterkant. Wanneer dat wordt gevraagd, selecteert u __Onderhoudsmodus inschakelen__ en vervolgens selecteert u opnieuw alle __Conform".

    Herhaal dit proces voor de posten MapReduce2 en garens die uit de lijst aan de linkerkant van de pagina.

7. Zodra u deze opnieuw hebt opgestart, elk item afzonderlijk selecteren en uitschakelen in de onderhoudsmodus uit de __Serviceacties__ uitgevouwen.

##<a name="test-restricted-access"></a>Testen met beperkte toegang

Om te controleren of u toegang hebt beperkt, moet u de volgende methoden gebruiken:

* Voor __Windows-gebaseerde__ HDInsight clusters via Extern bureaublad verbinding maken met het cluster. Zie [verbinding met het gebruik van RDP HDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp) voor meer informatie.

    Zodra verbonden, open een opdrachtprompt via het pictogram voor de __opdrachtregel Hadoop__ op het bureaublad.

* Voor __Linux-gebaseerde__ HDInsight clusters met SSH verbinding maken met het cluster. Zie een van de volgende onderwerpen voor informatie over het gebruik van SSH met Linux gebaseerde clusters:

    * [SSH gebruiken in combinatie met Hadoop op HDInsight van Linux, OS X en Unix, Linux gebaseerde](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [SSH met Linux-gebaseerde Hadoop op HDInsight van Windows gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md)
    
Wanneer een verbinding met het cluster, gebruik de volgende stappen uit om te verifiëren dat u alleen lees- en items op de rekening SAS-opslag kunt:

1. Typ de volgende opdracht vanaf de opdrachtprompt. __SASCONTAINER__ vervangen door de naam van de container voor de account SA's opslag gemaakt. Vervang __SASACCOUNTNAME__ door de naam van de account van de opslag die wordt gebruikt voor de SA's:

        hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    
    Hiermee wordt een lijst met de inhoud van de container die het bestand dat is geüpload moet bevatten wanneer de container en SAS is gemaakt.
    
2. Gebruik de volgende om te controleren dat u de inhoud van het bestand kan lezen. Vervang de __SASCONTAINER__ en __SASACCOUNTNAME__ als in de vorige stap. __Bestandsnaam__ vervangen door de naam van het bestand weergegeven in de vorige opdracht:

        hdfs dfs -text wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
        
    Hiermee wordt een lijst met de inhoud van het bestand.
    
3. Download het bestand naar het lokale bestandssysteem gebruikt u het volgende:

        hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    
    Dit wordt het bestand gedownload naar een lokaal bestand met de naam __testbestand.txt__.

4. Het lokale bestand uploaden naar een nieuw bestand met de naam __testupload.txt__ op de SAS-opslag gebruikt u het volgende:

        hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    
    U ontvangt een bericht met de volgende strekking:
    
        put: java.io.IOException
        
    Deze fout treedt op omdat de opslaglocatie lezen + lijst alleen. Gebruik het volgende om de gegevens op de standaard opslag voor het cluster, geschreven worden:
    
        hdfs dfs -put testfile.txt wasbs:///testupload.txt
        
    Deze periode wordt voltooid de bewerking.
    
##<a name="troubleshooting"></a>Het oplossen van problemen

###<a name="a-task-was-canceled"></a>Een taak is geannuleerd.

__Symptomen__: bij het maken van een cluster met de PowerShell-script, wordt het volgende foutbericht weergegeven:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

__Oorzaak__: deze fout kan optreden als u een wachtwoord voor de gebruiker admin/HTTP voor het cluster of (voor Linux gebaseerde clusters) gebruikt de gebruiker SSH.

__Oplossing__: gebruik een wachtwoord dat voldoet aan de volgende criteria:

- Ten minste 10 tekens lang moet zijn
- Moet ten minste één cijfer bevatten
- Moet ten minste één niet-alfanumerieke tekens bevatten
- Moet ten minste één hoofdletters of kleine letter bevatten

##<a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe opslag met beperkte toegang toevoegen aan het cluster HDInsight, informatie over andere manieren om te werken met gegevens op het cluster:

* [Gebruik component met HDInsight](hdinsight-use-hive.md)

* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)

* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)

[powershell]: ../powershell-install-configure.md

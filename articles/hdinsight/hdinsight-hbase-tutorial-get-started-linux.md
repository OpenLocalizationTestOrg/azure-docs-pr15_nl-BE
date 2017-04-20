<properties
    pageTitle="HBase zelfstudie: aan de slag met HBase Linux gebaseerde clusters in het Hadoop | Microsoft Azure"
    description="Volg deze HBase zelfstudie aan de slag met Apache HBase met Hadoop in HDInsight. Tabellen maken vanuit de shell HBase en deze query met behulp van de component."
    keywords="Apache hbase, hbase, hbase shell, hbase zelfstudie"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-linux-based-hadoop-in-hdinsight"></a>HBase zelfstudie: aan de slag met Apache HBase met Linux-gebaseerde Hadoop in HDInsight 

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Informatie over het maken van een cluster HBase in HDInsight, HBase tabellen maken en tabellen opvragen met behulp van de component. Zie [HDInsight HBase-overzicht]voor algemene informatie HBase,[hdinsight-hbase-overview].

De informatie in dit document is specifiek voor de HDInsight op basis van Linux-clusters. Voor meer informatie over Windows-gebaseerde clusters, gebruik de knop voor tabselectie boven aan de pagina om over te schakelen.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie HBase, hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- [Shell(SSH) beveiligen](hdinsight-hadoop-linux-use-ssh-unix.md). 
- [curl](http://curl.haxx.se/download.html).

### <a name="access-control-requirements"></a>Access controle-eisen

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>HBase cluster maken

De volgende procedure wordt een sjabloon Azure Resource Manager voor het maken van een Linux-gebaseerde HBase versie 3.4 cluster en de afhankelijke standaard Azure opslag account. Zie [Hadoop maken Linux gebaseerde clusters in het HDInsight](hdinsight-hadoop-provision-linux-clusters.md)de parameters die worden gebruikt in de procedure en andere methoden voor het cluster maken.

1. Klik op de onderstaande afbeelding om de sjabloon te openen in de portal voor Azure. De sjabloon bevindt zich in een openbare blob-container. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Voer de volgende gegevens van de bladeserver **aangepaste implementatie** :

    - **Abonnement**: Selecteer uw Azure abonnement die wordt gebruikt voor het maken van het cluster.
    - **Resourcegroep**: een nieuwe Azure Resource Management groep maken of een bestaande gebruiken.
    - **Locatie**: Geef de locatie van de resourcegroep. 
    - **Clusternaam**: Voer een naam voor het cluster HBase die u wilt maken.
    - **Cluster-aanmeldingsnaam en wachtwoord**: de standaard login-naam **admin**is.
    - **SSH-gebruikersnaam en wachtwoord**: de standaardgebruikersnaam is **sshuser**.  U kunt deze wijzigen.
     
    Andere parameters zijn optioneel.  
    
    Elk cluster heeft een afhankelijkheid van Azure Blob storage-account. Nadat u een cluster verwijdert, behoudt de gegevens in de opslag. Naam van de cluster standaard opslag is de naam van het cluster met 'store' toegevoegd. Is het vastgelegde variabelen in de sjabloon.
        
3. Selecteer **ik ga akkoord met de bovenstaande voorwaarden**en klik vervolgens op **Inkoop**. Het duurt ongeveer 20 minuten voor het maken van een cluster.


>[AZURE.NOTE] Als een cluster HBase is verwijderd, kunt u een ander HBase cluster maken met behulp van dezelfde standaard blob container. Het nieuwe cluster komt de HBase tabellen die u hebt gemaakt in het oorspronkelijke cluster ophalen. Als u wilt voorkomen dat de inconsistente, is het raadzaam de tabellen HBase uit te schakelen voordat u het cluster verwijderen.

## <a name="create-tables-and-insert-data"></a>Tabellen maken en invoegen van gegevens

SSH kunt u verbinding maken met clusters HBase en vervolgens met HBase Shell HBase tabellen maken, invoegen van gegevens en querygegevens. Zie voor meer informatie over het gebruik van SSH [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Linux, Unix, of OS X](hdinsight-hadoop-linux-use-ssh-unix.md) en [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
 

Voor de meeste mensen, gegevens in de indeling in tabelvorm worden weergegeven:

![HDInsight HBase tabelgegevens][img-hbase-sample-data-tabular]

In HBase is een implementatie van BigTable, dezelfde gegevens ziet:

![HDInsight HBase bigtable gegevens][img-hbase-sample-data-bigtable]

Het zal ook beter als u klaar bent met de volgende procedure.  


**Gebruik de shell HBase**

1. Van SSH, voer de volgende opdracht:

        hbase shell

4. Maak een HBase met twee kolommen families:

        create 'Contacts', 'Personal', 'Office'
        list
5. Sommige gegevens invoegen:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![hdinsight hadoop hbase shell][img-hbase-shell]

6. Een enkele rij ophalen

        get 'Contacts', '1000'

    Ziet u hetzelfde resultaat als wanneer u de opdracht scannen omdat er slechts één rij.

    Zie [Inleiding tot HBase Schema ontwerp]voor meer informatie over het schema van de tabel HBase[hbase-schema]. Zie voor meer HBase opdrachten [Apache HBase referentie handleiding][hbase-quick-start].

6. De shell afsluiten

        exit



**Bulksgewijs laden gegevens in de tabel Contactpersonen HBase**

HBase bevat verschillende methoden voor het laden van gegevens in tabellen.  Voor meer informatie Zie [bulksgewijs laden](http://hbase.apache.org/book.html#arch.bulk.load).


Voorbeeld van een gegevensbestand is geüpload naar een openbare blob-container, *wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  De inhoud van het bestand is:

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

U kunt een tekstbestand maken en het bestand op uw eigen account opslag uploaden als u wilt. Zie voor meer informatie het [uploaden van gegevens voor Hadoop projecten in HDInsight][hdinsight-upload-data].

> [AZURE.NOTE] Deze procedure wordt de contactpersonen HBase tabel die u hebt gemaakt in de laatste procedure gebruikt.

1. Van SSH, voer de volgende opdracht om te zetten van het gegevensbestand op StoreFiles en winkel op een relatief pad is opgegeven in Dimporttsv.bulk.output:.  Als u in de HBase Shell, gebruik de opdracht exit om af te sluiten.

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Voer de volgende opdracht om de gegevens van de /example/data/storeDataFileOutput naar de tabel HBase uploaden:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. U kunt de shell HBase openen en de opdracht scannen gebruiken om de tabelinhoud.



## <a name="use-hive-to-query-hbase"></a>Gebruik component aan query HBase

U kunt gegevens in tabellen HBase zoeken met behulp van de component. In deze sectie wordt een component-tabel die wordt toegewezen aan de tabel HBase en gebruikt voor het opvragen van de gegevens in de tabel HBase.

1. **Stopverf**openen en verbinding maken met het cluster.  Zie de instructies in de vorige procedure.
2. Open de shell component.

       hive
3. Voer het volgende script HiveQL om een component-tabel die is toegewezen aan de tabel HBase te maken. Zorg ervoor dat u hebt gemaakt aan de tabel waarnaar wordt verwezen in deze zelfstudie eerder met behulp van de shell HBase voordat u deze instructie uitvoert.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

2. Voer het volgende script HiveQL om de gegevens in de tabel HBase opvragen:

        SELECT count(*) FROM hbasecontacts;

## <a name="use-hbase-rest-apis-using-curl"></a>Gebruik HBase REST API's met krul

> [AZURE.NOTE] Als krul of overige communicatie met WebHCat, moet u de aanvragen worden geverifieerd door middel van de gebruikersnaam en het wachtwoord voor de beheerder van de cluster HDInsight. U moet de naam van het cluster ook gebruiken als onderdeel van de URI Uniform Resource Identifier () gebruikt voor het verzenden van de aanvragen naar de server.
>
> Voor de opdrachten in deze sectie **gebruikersnaam** vervangen door de gebruiker te verifiëren met het cluster en vervang **wachtwoord** door het wachtwoord voor de gebruikersaccount. **CLUSTERNAAM** vervangen door de naam van het cluster.
>
> De REST API is via [Basisverificatie](http://en.wikipedia.org/wiki/Basic_access_authentication)beveiligd. Zorg altijd aanvragen via de HTTP-Secure (HTTPS) om ervoor te zorgen dat uw gegevens veilig worden verzonden naar de server.

1. Gebruik de volgende opdracht om te controleren of u verbinding kunt maken met uw cluster HDInsight vanaf een opdrachtregel:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status

    U ontvangt een reactie van de volgende strekking:

        {"status":"ok","version":"v1"}

    Dit zijn de parameters die worden gebruikt in deze opdracht:

    * **-u** - de gebruikersnaam en het wachtwoord dat wordt gebruikt voor de verificatie van de aanvraag.
    * **-G** - geeft aan dat dit een GET-aanvraag.

2. Gebruik de volgende opdracht om de bestaande HBase tabellen:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/hbaserest/

3. Gebruik de volgende opdracht als u een nieuwe tabel wilt maken HBase met twee kolom families:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
        -v

    Het schema is beschikbaar in de JSon-indeling.

4. Gebruik de volgende opdracht als u bepaalde gegevens in te voegen:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"Row\":{\"key\":\"MTAwMA==\",\"Cell\":{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}}}" \
        -v

    U moet base64 coderen van de waarden die zijn opgegeven in de schakeloptie -d.  In het voorbeeld:

    - MTAwMA ==: 1000
    - UGVyc29uYWw6TmFtZQ ==: persoonlijke naam:
    - Sm9obiBEb2xl: John Dole

    [Onwaar-rij-toets](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) kunt u meerdere (batch) waarde invoegen.

5. Gebruik de volgende opdracht om een rij:

        curl -u <UserName>:<Password> \
        -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
        -H "Accept: application/json" \
        -v

Zie voor meer informatie over de HBase Rest [Apache HBase Reference Guide](https://hbase.apache.org/book.html#_rest).

## <a name="check-cluster-status"></a>De status van het cluster controleren

HBase in HDInsight wordt geleverd met een gebruikersinterface Web voor het controleren van clusters. Met behulp van de gebruikersinterface voor het Web, kunt u statistische gegevens of informatie over de regio's aanvragen.

SSH kan ook worden gebruikt voor lokale aanvragen, zoals webverzoeken aan het cluster HDInsight tunnel. De aanvraag wordt daarna naar de aangevraagde bron als het op het hoofd clusterknooppunt van HDInsight afkomstig zijn. Voor meer informatie, Zie [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Windows](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel).

**Om een SSH tunneling-sessie tot stand brengen**

1. Open **PuTTY**.  
2. Als u een SSH-sleutel opgegeven wanneer u tijdens het maken van uw gebruikersaccount hebt gemaakt, moet u de volgende stap schakelt u de persoonlijke sleutel moet worden gebruikt bij het verifiëren van het cluster uitvoeren:

    In de **categorie**, vouw van **verbinding**, **SSH**uit en selecteer **verificatie**. Ten slotte klikt u op **Bladeren** en selecteer het bestand .ppk met uw persoonlijke sleutel.

3. In de **categorie**, klik op de **sessie**.
4. Voer de volgende waarden van de basisopties voor uw scherm stopverf sessie:

    - **Hostnaam**: het SSH-adres van uw server in de Host-naam (of het IP-adres) HDInsight veld. De SSH-adres is de naam van het cluster, **-ssh.azurehdinsight.net**. Bijvoorbeeld *mijncluster ssh.azurehdinsight.net*.
    - **Poort**: 22. De ssh-poort op de primaire headnode is 22.  
5. In de sectie **categorie** aan de linkerkant van het dialoogvenster **verbinding**uitvouwen, **SSH**uitbreiden en klik op **Tunnels**.
6. Geef de volgende informatie over de opties beheren SSH port forwarding formulier:

    - **Bronpoort** - de poort op de client die u wilt doorsturen. Bijvoorbeeld 9876.
    - **Dynamische** - schakelt dynamische SOCKS-proxy-routering.
7. Klik op **toevoegen** om de instellingen toevoegen.
8. Klik op **openen** onder in het dialoogvenster openen een SSH-verbinding.
9. Wanneer dat wordt gevraagd, aanmelden bij de server met behulp van een SSH-account. Dit zal een SSH-sessie tot stand brengen en inschakelen van de tunnel.

**De FQDN-naam van de Ambari met zoohouder zoeken**

1. Ga naar https://<ClusterName>.azurehdinsight.net/.
2. De cluster-gebruikersaccountreferenties tweemaal op ENTER.
3. Klik in het linkermenu op **zookeeper**.
4. Klik op een van de drie koppelingen **ZooKeeper Server** uit het overzicht.
5. Kopieer de **hostnaam**. Zk0-CLUSTERNAME.xxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net.

**Een clientprogramma gebruikt (Firefox) configureren en controleren van de status van het cluster**

1. Open Firefox.
2. Klik op de knop **Menu openen** .
3. Klik op **Opties**.
4. Klik op **Geavanceerd**, klik op **netwerk**en klik vervolgens op **Instellingen**.
5. Selecteer **Handmatige proxyconfiguratie**.
6. Voer de volgende waarden:

    - **Socks-Host**: localhost
    - **Poort**: u hebt geconfigureerd in de stopverf SSH tunneling dezelfde poort gebruiken.  Bijvoorbeeld 9876.
    - **SOCKS v5**: (geselecteerd)
    - **Externe DNS**: (geselecteerd)
7. Klik op **OK** om de wijzigingen opslaan.
8. Ga naar http://&lt;de FQDN-naam van een ZooKeeper >: master-60010-status.

In een cluster met hoge beschikbaarheid vindt u een koppeling naar de huidige actieve HBase master node die als host voor de gebruikersinterface voor het Web fungeert.

##<a name="delete-the-cluster"></a>Het cluster te verwijderen

Als u wilt voorkomen dat de inconsistente, is het raadzaam de tabellen HBase uit te schakelen voordat u het cluster verwijderen.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie HBase voor HDInsight, hebt u geleerd hoe een cluster HBase maken en hoe tabellen maken en weergeven van de gegevens in die tabellen uit vanuit de shell HBase. U hebt ook geleerd hoe u met een component-query van de gegevens in de tabellen HBase en het gebruik van de HBase C# REST API's voor het maken van een tabel HBase en ophalen van gegevens uit de tabel.

Voor meer informatie, Zie:

- [Overzicht HDInsight HBase][hdinsight-hbase-overview]: HBase is een Apache open source NoSQL database gebouwd op Hadoop met willekeurige toegang en sterke samenhang voor grote hoeveelheden ongestructureerde en semistructured-gegevens.


[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png

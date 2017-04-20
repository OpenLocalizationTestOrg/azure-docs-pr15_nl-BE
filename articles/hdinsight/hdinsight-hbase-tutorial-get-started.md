<properties
    pageTitle="HBase zelfstudie: aan de slag met HBase in Hadoop | Microsoft Azure"
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
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-windows-based-hadoop-in-hdinsight"></a>HBase zelfstudie: aan de slag met Apache HBase met op Windows gebaseerde Hadoop in HDInsight

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Informatie over het HBase clusters maken in HDInsight en de tabellen opvragen met behulp van Apache component HBase tabellen maken. Zie [HDInsight HBase-overzicht]voor algemene informatie HBase,[hdinsight-hbase-overview].

De informatie in dit document is specifiek voor de HDInsight op basis van Windows-clusters. Voor meer informatie over Windows-gebaseerde clusters, gebruik de knop voor tabselectie boven aan de pagina om over te schakelen.

> [AZURE.NOTE] HBase (versie 0.98.0) op Windows gebaseerde HDInsight is alleen beschikbaar voor gebruik met 3.1 HDInsight clusters (gebaseerd op Apache Hadoop en garens 2.4.0). Zie voor informatie over de versie, [Wat is er nieuw in de versies van Hadoop cluster geleverd door HDInsight?][hdinsight-versions]

## <a name="before-you-begin"></a>Voordat u begint

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Voordat u deze zelfstudie HBase, hebt u het volgende:

- **Abonnement A Microsoft Azure**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Een werkstation** met Visual Studio 2013 of hoger: Zie [Visual Studio installeren](http://msdn.microsoft.com/library/e2h7fzkw.aspx)voor instructies.

### <a name="access-control-requirements"></a>Access controle-eisen

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>HBase cluster maken

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

**Voor het maken van een cluster HBase via de portal voor Azure**

1. Aanmelden bij de [Azure portal][azure-management-portal].
2. Klik op **Nieuw** of **+** in de linksboven hoek en klik vervolgens op **gegevens + Analytics**, **HDInsight**.
3. Voer de volgende waarden:

    - **De naam van cluster** - Voer een naam voor dit cluster.
    - **Clustertype** - Selecteer **HBase**.
    - **Cluster besturingssysteem** - Select **Windows**.  Voor het maken van Linux-gebaseerde HBase cluster, Zie [HBase zelfstudie: aan de slag met Apache HBase met Hadoop in HDInsight (Linux)](hdinsight-hbase-tutorial-get-started-linux.md).
    - **Versie** - Selecteer een HBase versie.
    - **Abonnement** - Selecteer uw Azure abonnement gebruikt voor het maken van dit cluster.
    - **Resourcegroep** - een nieuwe Azure resourcegroep maken of Selecteer een bestaande. Zie [Overzicht van Azure Resource Manager](azure-resource-manager/resource-group-overview.md) voor meer informatie
    - **Referenties** - voor Windows gebaseerde cluster, kunt u een gebruiker van het cluster (a.k.a HTTP-gebruiker, gebruiker-service HTTP) en een gebruiker extern bureaublad. Klik op **Extern bureaublad inschakelen** als u wilt toevoegen van de referenties van de gebruiker voor extern bureaublad. De volgende sectie vereist RDP.
    - **Gegevensbron** - Maak een nieuwe account met Azure opslag of Selecteer een bestaande account Azure opslag moet worden gebruikt als het standaardbestandssysteem voor het cluster. De standaardlocatie voor opslag account bepaalt de locatie van de locatie van het cluster. De standaardaccount voor de opslag en het cluster moeten samen kunnen vinden in het midden met dezelfde gegevens.
    - **Knooppunt prijzen lagen** - het aantal servers voor het cluster HBase regio selecteren

        > [AZURE.WARNING] Voor maximale beschikbaarheid van HBase services, moet u een cluster met ten minste **drie** knooppunten. Dit zorgt ervoor dat, als een knooppunt uitvalt, de gebieden HBase gegevens beschikbaar op andere knooppunten zijn.

        > Als u HBase leren, altijd 1 voor het formaat van een cluster kiezen en het cluster verwijderen na elk gebruik om de kosten te beperken.

    - **Optionele configuratie** - virtueel netwerk configureren Azure, scriptacties configureren en extra opslagruimte accounts toevoegen.

4. Klik op **maken**.

>[AZURE.NOTE] Als een cluster HBase is verwijderd, kunt u een ander HBase cluster maken met behulp van dezelfde standaard opslag account en de standaardcontainer blob. Het nieuwe cluster komt de HBase tabellen die u hebt gemaakt in het oorspronkelijke cluster ophalen. Als u wilt voorkomen dat de inconsistente, is het raadzaam de tabellen HBase uit te schakelen voordat u het cluster verwijderen.

## <a name="create-tables-and-insert-data"></a>Tabellen maken en invoegen van gegevens

Er zijn twee manier toegang te krijgen tot HBase. Deze sectie bevat met de shell HBase. De volgende sectie beschreven hoe u met de SDK voor .NET.

Voor de meeste mensen, gegevens in de indeling in tabelvorm worden weergegeven:

![hdinsight hbase tabelgegevens][img-hbase-sample-data-tabular]

In HBase is een implementatie van BigTable, dezelfde gegevens ziet:

![hdinsight hbase bigtable gegevens][img-hbase-sample-data-bigtable]

Het gaat ook beter als u klaar bent met de volgende procedure.  

**Gebruik de shell HBase**

1. Met RDP verbinding maken met het cluster HBase in HDInsight. Zie voor de instructies RDP [clusters beheren Hadoop in HDInsight met behulp van de Portal Azure][hdinsight-manage-portal].
2. Klik op het bureaublad een snelkoppeling naar de **opdrachtregel Hadoop** in RDP-sessie.
3. Open de shell HBase:

        cd %HBASE_HOME%\bin
        hbase shell

4. Maak een HBase met twee kolom families:

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

    Zie [Inleiding tot HBase Schema ontwerp]voor meer informatie over het schema van de tabel Hbase[hbase-schema]. Zie voor meer HBase opdrachten [Apache HBase referentie handleiding][hbase-quick-start].


6. De shell afsluiten

        exit

**Bulksgewijs laden gegevens in de tabel Contactpersonen HBase**

HBase bevat verschillende methoden voor het laden van gegevens in tabellen. Voor meer informatie Zie [bulksgewijs laden](http://hbase.apache.org/book.html#arch.bulk.load).


Voorbeeld van een gegevensbestand is geüpload naar een openbare blob-container, wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt. De inhoud van het bestand is:

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

1. Klik op het bureaublad een snelkoppeling naar de **opdrachtregel Hadoop** in RDP-sessie.
2. Klik op map wijzigen:

        cd %HBASE_HOME%\bin

3. Voer de volgende opdracht om te zetten van het bestand StoreFiles en een relatief pad is opgegeven bij Dimporttsv.bulk.output-archief:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Voer de volgende opdracht om de gegevens van de /example/data/storeDataFileOutput naar de tabel HBase uploaden:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. U kunt de shell HBase openen en de opdracht scannen gebruiken om de tabelinhoud.



## <a name="use-hive-to-query-hbase-tables"></a>Component gebruiken om tabellen van de query HBase

U kunt de gegevens die zijn opgeslagen in HBase met behulp van de component zoeken. In deze sectie wordt een component-tabel die wordt toegewezen aan de tabel HBase en gebruikt voor het opvragen van de gegevens in de tabel HBase.

**Het dashboard met cluster openen**

1. Ga naar **https://<HDInsight Cluster Name>.azurehdinsight.net/**.
5. Het Hadoop-account gebruikersnaam en wachtwoord invoeren. De standaardgebruikersnaam is **admin** en het wachtwoord dat is opgegeven tijdens het maken. Hiermee opent u een nieuwe browser tab.
6. Klik op **Component-Editor** aan de bovenkant van de pagina. De component-Editor ziet er zo uit:

    ![HDInsight cluster dashboard.][img-hdinsight-hbase-hive-editor]

**Component-query's uitvoeren**

1. Het volgende script voor HiveQL in Editor component en klik op **verzenden** om een component-tabel die is toegewezen aan de tabel HBase te maken. Zorg ervoor dat u hebt gemaakt de tabel waarnaar wordt verwezen in deze zelfstudie eerder met behulp van de shell HBase voordat u deze instructie uitvoert.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

    Wacht totdat updates van de **Status** **voltooid**.

2. Voer het volgende script voor HiveQL in Editor component en klik op **verzenden**. De component query query's de gegevens in de tabel HBase:

        SELECT count(*) FROM hbasecontacts;

4. Voor het ophalen van de resultaten van de query component, klikt u op de koppeling **Details bekijken** in het venster **Taak sessie** wanneer de taak is voltooid. Zal er slechts één taak uitvoerbestand omdat u één record in de tabel HBase plaatsen.




**Het uitvoerbestand bladeren**

1. Klik in de Console Query **File Browser**.
2. Klik op de account Azure opslag die wordt gebruikt als het standaardbestandssysteem voor het cluster HBase.
3. Klik op de naam van het cluster HBase. De standaardcontainer Azure opslag-account maakt gebruik van de clusternaam.
4. Klik op **gebruiker**en klik vervolgens op **beheer**. (Dit is de naam Hadoop.)
6. Klik op de naam van de taak met de laatste tijd van de **Wijziging** die overeenkomt met de tijd waarop de query selecteren component is uitgevoerd.
4. Klik op **stdout**. Sla het bestand op en open het bestand met Kladblok. Er is een uitvoerbestand.

    ![HDInsight HBase component Editor bestandsbrowser][img-hdinsight-hbase-file-browser]

## <a name="use-the-net-hbase-rest-api-client-library"></a>De bibliotheek .NET HBase REST API-client gebruiken

Als u de bibliotheek HBase REST API-client downloaden voor .NET van GitHub en bouwt het project zodat u de HBase .NET SDK kunt. De volgende procedure bevat de instructies voor deze taak.

1. Maak een nieuwe C# Visual Studio Desktop Console Windows-toepassing.
2. Open de Console NuGet Package Manager door te klikken op **Extra** > **NuGet Package Manager** > **Package Manager-Console**.
3. Voer de volgende NuGet-opdracht in de console:

        Install-Package Microsoft.HBase.Client

5. De volgende instructies voor het **gebruik van** toevoegen boven aan het bestand:

        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;

6. De functie **Main** vervangen door het volgende:

        static void Main(string[] args)
        {
            string clusterURL = "https://<yourHBaseClusterName>.azurehdinsight.net";
            string hadoopUsername= "<yourHadoopUsername>";
            string hadoopUserPassword = "<yourHadoopUserPassword>";

            string hbaseTableName = "sampleHbaseTable";

            // Create a new instance of an HBase client.
            ClusterCredentials creds = new ClusterCredentials(new Uri(clusterURL), hadoopUsername, hadoopUserPassword);
            HBaseClient hbaseClient = new HBaseClient(creds);

            // Retrieve the cluster version.
            var version = hbaseClient.GetVersion();
            Console.WriteLine("The HBase cluster version is " + version);

            // Create a new HBase table.
            TableSchema testTableSchema = new TableSchema();
            testTableSchema.name = hbaseTableName;
            testTableSchema.columns.Add(new ColumnSchema() { name = "d" });
            testTableSchema.columns.Add(new ColumnSchema() { name = "f" });
            hbaseClient.CreateTable(testTableSchema);

            // Insert data into the HBase table.
            string testKey = "content";
            string testValue = "the force is strong in this column";
            CellSet cellSet = new CellSet();
            CellSet.Row cellSetRow = new CellSet.Row { key = Encoding.UTF8.GetBytes(testKey) };
            cellSet.rows.Add(cellSetRow);

            Cell value = new Cell { column = Encoding.UTF8.GetBytes("d:starwars"), data = Encoding.UTF8.GetBytes(testValue) };
            cellSetRow.values.Add(value);
            hbaseClient.StoreCells(hbaseTableName, cellSet);

            // Retrieve a cell by its key.
            cellSet = hbaseClient.GetCells(hbaseTableName, testKey);
            Console.WriteLine("The data with the key '" + testKey + "' is: " + Encoding.UTF8.GetString(cellSet.rows[0].values[0].data));
            // with the previous insert, it should yield: "the force is strong in this column"

            //Scan over rows in a table. Assume the table has integer keys and you want data between keys 25 and 35.
            Scanner scanSettings = new Scanner()
            {
                batch = 10,
                startRow = BitConverter.GetBytes(25),
                endRow = BitConverter.GetBytes(35)
            };

            ScannerInformation scannerInfo = hbaseClient.CreateScanner(hbaseTableName, scanSettings);
            CellSet next = null;
            Console.WriteLine("Scan results");

            while ((next = hbaseClient.ScannerGetNext(scannerInfo)) != null)
            {
                foreach (CellSet.Row row in next.rows)
                {
                    Console.WriteLine(row.key + " : " + Encoding.UTF8.GetString(row.values[0].data));
                }
            }

            Console.WriteLine("Press ENTER to continue ...");
            Console.ReadLine();
        }

7. De eerste drie variabelen instellen in de **Main** functie.
8. Druk op **F5** om de toepassing te starten.

## <a name="check-cluster-status"></a>De status van het cluster controleren

HBase in HDInsight wordt geleverd met een gebruikersinterface Web voor het controleren van clusters. Met behulp van de gebruikersinterface voor het Web, kunt u statistische gegevens of informatie over de regio's aanvragen.

Om de gebruikersinterface voor het Web opent, u RDP moet in het cluster, en klikt u op de HMaster Info Web UI-snelkoppeling op het bureaublad of gebruik de volgende URL in een webbrowser:

    http://zookeeper[0-2]:60010/master-status

In een cluster met hoge beschikbaarheid vindt u een koppeling naar de huidige actieve HBase master node die als host voor de gebruikersinterface voor het Web fungeert.

##<a name="delete-the-cluster"></a>Het cluster te verwijderen
Als u wilt voorkomen dat de inconsistente, is het raadzaam de tabellen HBase uit te schakelen voordat u het cluster verwijderen.
[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="whats-next"></a>Hoe nu verder?
In deze zelfstudie HBase voor HDInsight, hebt u geleerd hoe een cluster HBase maken en hoe tabellen maken en weergeven van de gegevens in die tabellen uit vanuit de shell HBase. U leert ook hoe een query component gebruiken van gegevens in tabellen HBase en het gebruik van de HBase C# REST API's voor het maken van een tabel HBase en ophalen van gegevens uit de tabel.

Zie voor meer informatie:

- [Overzicht HDInsight HBase][hdinsight-hbase-overview].
HBase is een Apache open source NoSQL database gebouwd op Hadoop met willekeurige toegang en sterke samenhang voor grote hoeveelheden ongestructureerde en semistructured-gegevens.
- [HBase clusters maken op Azure Virtual Network][hdinsight-hbase-provision-vnet].
Dankzij virtuele kunnen clusters HBase worden toegepast op hetzelfde virtuele netwerk als uw toepassingen zodat toepassingen rechtstreeks met HBase communiceren kunnen.
- [Replicatie configureren HBase in HDInsight](hdinsight-hbase-geo-replication.md). Informatie over het configureren HBase replicatie tussen twee Azure datacenters.
- [Analyseren van Twitter sentiment met HBase in HDInsight][hbase-twitter-sentiment].
Informatie over hoe u real-time [sentiment analyse](http://en.wikipedia.org/wiki/Sentiment_analysis) van grote gegevens met behulp van HBase in een cluster Hadoop in HDInsight.

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
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-bigtable.png

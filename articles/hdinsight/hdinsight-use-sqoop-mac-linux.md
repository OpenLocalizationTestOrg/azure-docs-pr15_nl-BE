<properties
    pageTitle="Gebruik van Hadoop Sqoop in Linux-gebaseerde HDInsight | Microsoft Azure"
    description="Informatie over het uitvoeren van Sqoop importeren en exporteren tussen een Linux-gebaseerde Hadoop op HDInsight cluster en Azure SQL-database."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight-ssh"></a>Sqoop gebruiken in combinatie met Hadoop in HDInsight (SSH)

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het gebruik van Sqoop om te importeren en exporteren tussen een Linux-gebaseerde HDInsight cluster en Azure SQL-Database of SQL Server-database.

> [AZURE.NOTE] De stappen in dit artikel gebruiken SSH verbinding maken met een cluster van Linux-gebaseerde HDInsight. Windows-clients kunnen ook Azure PowerShell en HDInsight .NET SDK gebruiken om te werken met Sqoop op Linux gebaseerde clusters. Gebruik de knop voor tabselectie te openen die artikelen.

##<a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Cluster A Hadoop in HDInsight** en een __Azure SQL-Database__: de stappen in dit document zijn gebaseerd op het cluster en de database die is gemaakt met behulp van het document [cluster maken en SQL-database](hdinsight-use-sqoop.md#create-cluster-and-sql-database) . Als u al een cluster van HDInsight en een SQL-Database, kunt u vervangen door die voor de waarden die in dit document worden gebruikt.
- **Werkstation**: een computer met een SSH-client.

##<a name="install-freetds"></a>FreeTDS installeren

1. Met SSH verbinding maken met het cluster Linux-gebaseerde HDInsight. Het adres dat wordt gebruikt wanneer een verbinding is `CLUSTERNAME-ssh.azurehdinsight.net` en de poort is `22`.

    Zie voor meer informatie over het gebruik van SSH verbinding maken met de HDInsight de volgende documenten:

    * **Linux, Unix of OS X-clients**: Zie [verbinding maken met een cluster Linux-gebaseerde HDInsight van Linux, OS X en Unix](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)

    * **Windows-clients**: Zie [verbinding maken met een HDInsight op basis van Linux cluster van Windows](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)

3. Gebruik de volgende opdracht FreeTDS installeren:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

    FreeTDS wordt gebruikt in verschillende stappen verbinding maken met een SQL-Database.

##<a name="create-the-table-in-sql-database"></a>De tabel maken in SQL-Database

> [AZURE.IMPORTANT] Als u een cluster van HDInsight en een SQL-Database gemaakt met de stappen in een [cluster maken en SQL-database](hdinsight-use-sqoop.md)gebruikt, negeert de stappen in deze sectie als de database en tabel zijn gemaakt als onderdeel van de stappen in dat document.

1. Gebruik de volgende opdracht verbinding maken met de SQL-databaseserver en Kreta in de tabel die wordt gebruikt in de overige stappen van de SSH-verbinding met HDInsight:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Uitvoer van de volgende strekking wordt weergegeven:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Op de `1>` wordt gevraagd, voert u de volgende regels:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
        GO

    Wanneer de `GO` instructie is ingevoerd, wordt de vorige overzichten worden geëvalueerd. Eerst de **mobiledata** -tabel wordt gemaakt en vervolgens een gegroepeerde index is toegevoegd (vereist voor SQL-Database).

    Om te controleren of de tabel is gemaakt, gebruikt u het volgende:

        SELECT * FROM information_schema.tables
        GO

    Uitvoer van de volgende strekking worden weergegeven:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

8. Voer `exit` op de `1>` vragen om het hulpprogramma tsql af te sluiten.

##<a name="sqoop-export"></a>Sqoop exporteren

3. Van de SSH-verbinding met HDInsight, se de volgende opdracht om te controleren of de Sqoop ziet de SQL-Database:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    Dit moet als resultaat een lijst van databases, met inbegrip van de **sqooptest** database die u eerder hebt gemaakt.

4. De volgende opdracht gebruiken om gegevens te exporteren uit **hivesampletable** aan de **mobiledata** -tabel:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasbs:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1

    Dit geeft de opdracht Sqoop verbinding maken met SQL-Database op de database **sqooptest** en exporteren van gegevens uit de **wasbs: / / / onderdeel/magazijn/hivesampletable** (fysieke bestanden voor de *hivesampletable*) aan de tabel **mobiledata** .

5. Nadat de opdracht is voltooid, gebruikt u de volgende verbinding maken met de database via TSQL:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Zodra verbonden, gebruikt u de volgende instructies om te controleren dat de gegevens zijn geëxporteerd naar de tabel **mobiledata** :

        SELECT * FROM mobiledata
        GO

    Hier ziet u een overzicht van de gegevens in de tabel. Type `exit` om af te sluiten van het hulpprogramma tsql.

##<a name="sqoop-import"></a>Sqoop importeren

1. Gebruik de volgende gegevens te importeren uit de tabel **mobiledata** in de SQL-Database op de **wasbs: / / / zelfstudies/usesqoop/importeddata** op HDInsight:

        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

    De geïmporteerde gegevens hebben velden die worden gescheiden door een tab en de regels wordt beëindigd door een nieuwe-regelteken.

2. Nadat het importeren is voltooid, gebruikt u de volgende opdracht om een lijst met de gegevens in de nieuwe map:

        hadoop fs -text wasbs:///tutorials/usesqoop/importeddata/part-m-00000

##<a name="using-sql-server"></a>Met behulp van SQL Server

U kunt ook Sqoop gebruiken om te importeren en exporteren van gegevens van SQL Server in uw datacenter of op een virtuele Machine die wordt gehost in Azure. De verschillen tussen het gebruik van SQL-Database en SQL Server zijn:

* Zowel HDInsight als de SQL-Server moet zich op hetzelfde virtuele netwerk Azure

    > [AZURE.NOTE] HDInsight ondersteunt virtuele netwerken alleen op basis van locatie en deze werkt momenteel niet met virtuele netwerken op basis van affiniteit groep.

    Wanneer u SQL Server in uw datacenter gebruikt, moet u het virtuele netwerk configureren als *website* of *punt-naar-site*.

    > [AZURE.NOTE] Voor **punt-naar-site** virtuele netwerken, moet SQL Server worden uitgevoerd de VPN-client toepassing voor databaseconfiguratie, die beschikbaar via het **Dashboard** van uw Azure virtual netwerkconfiguratie is.

    Zie voor meer informatie Azure Virtual Network, [Virtual Network overzicht](../virtual-network/virtual-networks-overview.md).

* SQL Server moet worden geconfigureerd dat SQL-verificatie. Zie voor meer informatie, [Kies een andere modus](https://msdn.microsoft.com/ms144284.aspx)

* U moet SQL Server voor externe verbindingen configureren. Zie [problemen met verbinding maken met de database-engine van SQL Server](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) voor meer informatie

* U moet de **sqooptest** database maken in SQL Server met behulp van een hulpprogramma zoals **SQL Server Management Studio** of **tsql** - werken de stappen voor het gebruik van de CLI Azure alleen voor Azure SQL-Database

    Lijken de TSQL-instructies voor het maken van de tabel **mobiledata** die worden gebruikt voor SQL-Database, met uitzondering van het maken van een clusterd index - dit is niet vereist voor SQL Server:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])

* Wanneer u verbinding maakt met de SQL-Server van HDInsight, moet u wellicht het IP-adres van de SQL Server te gebruiken, tenzij u een systeem DNS (Domain Name) voor het herleiden van namen op het virtuele netwerk Azure hebt geconfigureerd. Bijvoorbeeld:

        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

##<a name="limitations"></a>Beperkingen

* Bulk export - met Linux-gebaseerde HDInsight, de Sqoop-connector gebruikt om gegevens te exporteren naar Microsoft SQL Server of SQL-Database Azure biedt momenteel geen ondersteuning voor bulksgewijs invoegen.

* Batchen - met Linux-gebaseerde HDInsight bij het gebruik van de `-batch` Ga bij het uitvoeren van toevoegingen, Sqoop meerdere ingevoegd in plaats van de batchverwerking de insert-bewerkingen worden uitgevoerd.

##<a name="next-steps"></a>Volgende stappen

U hebt nu geleerd dat het gebruik van Sqoop. Voor meer informatie, Zie:

- [Gebruik van Oozie met HDInsight][hdinsight-use-oozie]: gebruik Sqoop actie in de workflow van een Oozie.
- [Gegevens te analyseren vlucht vertraging met HDInsight][hdinsight-analyze-flight-data]: component gebruiken voor het analyseren van flight uitstellen van gegevens en gebruik vervolgens Sqoop om gegevens te exporteren naar een database SQL Azure.
- [Gegevens uploaden naar HDInsight][hdinsight-upload-data]: zoeken naar andere methoden voor het uploaden van gegevens naar HDInsight/Azure Blob-opslag.



[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

<properties 
   pageTitle="Kopiëren van gegevens tussen Lake gegevensopslag en Azure SQL-database met behulp van Sqoop | Microsoft Azure"
   description="Gebruik Sqoop gegevens te kopiëren tussen Azure SQL-Database en Lake gegevensarchief" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Kopiëren van gegevens tussen Lake gegevensopslag en Azure SQL-database met behulp van Sqoop

Leren werken met Apache Sqoop importeren en exporteren van gegevens tussen Azure SQL-Database en Lake gegevensarchief.
 

## <a name="what-is-sqoop"></a>Wat is Sqoop?

BIG data-toepassingen zijn een natuurlijke keuze voor de verwerking van niet-gestructureerde en semi-gestructureerde gegevens, zoals bestanden en Logboeken. Er kunnen ook wel nodig voor het verwerken van gestructureerde gegevens die zijn opgeslagen in relationele databases.

[Apache-Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) is een hulpprogramma dat is ontworpen voor het overbrengen van gegevens tussen relationele databases en een opslagplaats voor grote gegevens, zoals een gegevensarchief Lake. U kunt deze gegevens uit een relationele database management systeem (RDBMS) zoals Azure SQL-Database importeren in Lake gegevensarchief. U kunt vervolgens transformeren en analyseren van de gegevens met behulp van big data werkbelasting en vervolgens de gegevens exporteren naar een RDBMS. In deze zelfstudie kunt u een Azure SQL-Database als de relationele database importeren/exporteren uit.
 

## <a name="prerequisites"></a>Vereisten

Voordat u dit artikel hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).
- **Uw abonnement op Azure inschakelen** voor gegevensopslag Lake openbare preview. Zie de [instructies](data-lake-store-get-started-portal.md#signup). 
- **Azure HDInsight cluster** met toegang tot een gegevensarchief Lake-account. Zie [een cluster van HDInsight met Lake gegevensarchief maken](data-lake-store-hdinsight-hadoop-use-portal.md). In dit artikel wordt ervan uitgegaan dat u hebt een HDInsight Linux cluster toegang Lake gegevensarchief.
- **Azure SQL-Database**. Zie voor instructies voor het maken van een [Azure SQL-database maken](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Informatie snel met video's?

[Bekijk deze video](https://mix.office.com/watch/1butcdjxmu114) over het kopiëren van gegevens tussen Azure opslag BLOB's en het gegevensarchief Lake met DistCp.

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Tabellen maken in de Azure SQL-Database

1. Te beginnen met twee tabellen in de Azure SQL-Database maken. Gebruik [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) of Visual Studio verbinding maken met de Azure SQL-Database en voert u de volgende query's.

    **Tabel1 maken**

        CREATE TABLE [dbo].[Table1]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

    **Tabel2 maken**

        CREATE TABLE [dbo].[Table2]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

2. Voeg toe voorbeeldgegevens in **tabel1**. **Tabel2** leeg laten. We zullen gegevens importeren uit **tabel1** in Lake gegevensarchief. Vervolgens zullen we gegevens exporteren uit Lake gegevensarchief naar **tabel2**. In het volgende fragment wordt uitgevoerd.

         
        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson'); 
  

## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a>Sqoop gebruik van een cluster HDInsight met toegang naar Lake gegevensopslag

Een cluster HDInsight al het Sqoop pakketten beschikbaar. Als u het cluster HDInsight Lake gegevensopslag gebruikt als een extra opslagruimte hebt geconfigureerd, kunt u Sqoop (zonder eventuele wijzigingen in de configuratie) te importeren/exporteren van gegevens tussen een relationele database (in dit voorbeeld wordt een SQL-Database Azure) en een gegevensarchief Lake-account. 

1. Voor deze zelfstudie, nemen we aan dat u een Linux cluster gemaakt zodat u SSH gebruiken moet om verbinding te maken met het cluster. Zie [verbinding maken met een cluster van Linux-gebaseerde HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).

2. Controleer of u toegang heeft tot de account Lake gegevensarchief van het cluster. Voer de volgende opdracht vanaf de prompt SSH:

        
        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    Dit dient een lijst van bestanden/mappen in het gegevensarchief Lake.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Gegevens van Azure SQL-Database importeren in het gegevensarchief Lake

3. Ga naar de map waar Sqoop pakketten beschikbaar zijn. Meestal zijn dit op `/usr/hdp/<version>/sqoop/bin`. 

4. Importeer de gegevens uit **tabel1** Lake gegevensarchief rekening te houden. Gebruik de volgende syntaxis:

        
        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Houd er rekening mee dat aanduiding van de **sql-database-servernaam** de naam van de server waarop de Azure SQL-database wordt uitgevoerd. **SQL-database-naam** tijdelijke aanduiding vertegenwoordigt de naam van de feitelijke database.

    Bijvoorbeeld:

        
        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

5. Controleer of de gegevens is overgeboekt naar de rekening van het gegevensarchief Lake. Voer de volgende opdracht:

        
        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Hier ziet u de volgende uitvoer.

        
        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Elke **deel-m -** * bestand komt overeen met een rij in de tabel, * *tabel1**. U kunt de inhoud van het onderdeel bekijken-m -* bestanden te controleren.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Gegevens exporteren uit het gegevensarchief Lake naar Azure SQL-Database

6. De gegevens exporteren uit het gegevensarchief Lake account naar de lege tabel, **tabel2**in de Azure SQL-Database. Gebruik de volgende syntaxis.

        
        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Bijvoorbeeld:

        
        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

6. Controleer of de gegevens naar de SQL-databasetabel is geüpload. Gebruik [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) of Visual Studio verbinding maken met de Azure SQL-Database en voert u de volgende query.

        
        SELECT * FROM TABLE2

    Dit heeft de volgende uitvoer.

        ID  FName   LName
        ------------------
        1   Neal    Kell
        2   Lila    Fulton
        3   Erna    Myers
        4   Annette Simpson

## <a name="see-also"></a>Zie ook

- [Gegevens kopiëren van Azure Storage Blobs naar Lake gegevensarchief](data-lake-store-copy-data-azure-storage-blob.md)
- [Beveiliging van gegevens in het gegevensarchief Lake](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensarchief](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensarchief](data-lake-store-hdinsight-hadoop-use-portal.md)

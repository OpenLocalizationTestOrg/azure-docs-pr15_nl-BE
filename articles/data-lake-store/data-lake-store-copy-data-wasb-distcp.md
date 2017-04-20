<properties
   pageTitle="Gegevens kopiëren naar en van WASB naar Lake gegevensarchief met Distcp | Microsoft Azure"
   description="Distcp gebruiken om gegevens te kopiëren en naar Azure Storage Blobs naar Lake gegevensopslag"
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

# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a>Gebruik Distcp voor het kopiëren van gegevens tussen Azure opslag BLOB's en het gegevensarchief Lake

> [AZURE.SELECTOR]
- [Met behulp van DistCp](data-lake-store-copy-data-wasb-distcp.md)
- [Met behulp van AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)


Als u een cluster HDInsight die toegang tot een gegevensarchief Lake-account heeft hebt gemaakt, kunt u Hadoop-ecosysteem hulpmiddelen zoals Distcp gegevens te kopiëren **naar en van** de clusteropslag voor een HDInsight (WASB) rekening Lake gegevensarchief. Dit artikel bevat instructies over hoe dit te bereiken.

##<a name="prerequisites"></a>Vereisten

Voordat u dit artikel hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).
- **Uw abonnement op Azure inschakelen** voor gegevensopslag Lake openbare preview. Zie de [instructies](data-lake-store-get-started-portal.md#signup).
- **Azure HDInsight cluster** met toegang tot een gegevensarchief Lake-account. Zie [een cluster van HDInsight met Lake gegevensarchief maken](data-lake-store-hdinsight-hadoop-use-portal.md). Zorg ervoor dat u extern bureaublad inschakelen voor het cluster.

## <a name="do-you-learn-fast-with-videos"></a>Informatie snel met video's?

[Bekijk deze video](https://mix.office.com/watch/1liuojvdx6sie) over het kopiëren van gegevens tussen Azure opslag BLOB's en het gegevensarchief Lake met DistCp.

## <a name="use-distcp-from-remote-desktop-windows-cluster-or-ssh-linux-cluster"></a>Gebruik de Distcp van extern bureaublad (Windows, cluster) of SSH (Linux cluster)

Een cluster van HDInsight wordt geleverd met het hulpprogramma Distcp, die kan worden gebruikt om gegevens uit verschillende bronnen kopiëren naar een cluster HDInsight. Als u het cluster HDInsight Lake gegevensopslag gebruikt als een extra opslagruimte hebt geconfigureerd, kan het hulpprogramma Distcp gebruikte out-of-the-box om gegevens te kopiëren en naar een gegevensarchief Lake-account zijn. In deze sectie bekijken we hoe u het hulpprogramma Distcp gebruiken.

1. Als u een Windows-cluster, RAS in een cluster HDInsight die toegang heeft tot een gegevensarchief Lake-account. Zie [verbinding maken met clusters via RDP](../hdinsight/hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)voor instructies. Open het Hadoop-opdrachtregel uit het cluster bureaublad.

    Als u een Linux-cluster hebt, gebruik SSH verbinding maken met het cluster. Zie [verbinding maken met een cluster van Linux-gebaseerde HDInsight](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster). De opdrachten worden uitgevoerd vanaf de prompt SSH.

3. Controleer of u toegang hebt tot de Azure opslag BLOB's (WASB). Voer de volgende opdracht:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    Dit dient een lijst met de inhoud van de blob storage.

4. Controleer ook of u toegang heeft tot de account Lake gegevensarchief van het cluster. Voer de volgende opdracht:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    Dit dient een lijst van bestanden/mappen in het gegevensarchief Lake.

5. Distcp gebruiken om gegevens te kopiëren van de WASB naar een gegevensarchief Lake.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    Hiermee wordt de inhoud van de map **** voorbeeld/gegevens/gutenberg/in WASB kopiëren naar **/myfolder** in het gegevensarchief Lake.

6. Op deze manier Distcp gebruiken om gegevens uit een gegevensarchief Lake account kopiëren naar WASB.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Kopieer de inhoud van de **/myfolder** in het gegevensarchief Lake zal dit **** voorbeeld/gegevens/gutenberg/map in de WASB.

## <a name="see-also"></a>Zie ook

- [Gegevens kopiëren van Azure Storage Blobs naar Lake gegevensarchief](data-lake-store-copy-data-azure-storage-blob.md)
- [Beveiliging van gegevens in het gegevensarchief Lake](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensarchief](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensarchief](data-lake-store-hdinsight-hadoop-use-portal.md)

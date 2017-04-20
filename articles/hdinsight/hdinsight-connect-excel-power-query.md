<properties
    pageTitle="Excel verbinden met Hadoop met Power Query | Microsoft Azure"
    description="Informatie over het gebruik maken van business intelligence-onderdelen en Power Query gebruiken voor Excel toegang heeft tot gegevens die zijn opgeslagen in Hadoop op HDInsight."
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


#<a name="connect-excel-to-hadoop-by-using-power-query"></a>Hadoop Excel verbinding via Power Query

Een belangrijke functie van de Microsoft-oplossing voor grote data is de integratie van Microsoft business intelligence (BI)-onderdelen met clusters in Azure HDInsight Hadoop. Een primaire voorbeeld van deze integratie is de mogelijkheid om de opslag van Azure rekening met de gegevens die zijn gekoppeld aan het Hadoop-cluster via de Power Microsoft Query voor Excel-invoegtoepassing Excel verbinding. Dit artikel helpt u bij het instellen en gebruiken van Power Query query uitvoeren op gegevens die zijn gekoppeld aan een Hadoop cluster beheerd met HDInsight.

> [AZURE.NOTE] Terwijl u de stappen in dit artikel kunnen worden gebruikt met een Linux- of HDInsight op basis van het Windows-cluster, is Windows vereist voor het clientwerkstation.

### <a name="prerequisites"></a>Vereisten

Voordat u dit artikel hebt u het volgende:

- **Een HDInsight-cluster**. Zie [aan de slag met Azure HDInsight]configureren van een[hdinsight-get-started].
- **Een werkstation** waarop Windows 7, Windows Server 2008 R2 of een later besturingssysteem wordt uitgevoerd.
- **Office 2013 Professional Plus, Office 365 ProPlus, Excel 2013 zelfstandig of Office 2010 Professional Plus**.


## <a name="install-power-query"></a>Power Query installeren

Power Query kan worden gebruikt om gegevens te importeren uit een groot aantal bronnen in Microsoft Excel, waarbij het BI-hulpmiddelen zoals PowerPivot en Power View zet. In het bijzonder kunt Power Query importeren die heeft zijn uitgevoerd of die is gegenereerd door een Hadoop taak uitvoeren op een cluster HDInsight.

Download Microsoft Power Query voor Excel van het [Microsoft Download Center] [ powerquery-download] en te installeren.

## <a name="import-hdinsight-data-into-excel"></a>HDInsight-gegevens importeren in Excel

De Power Query-invoegtoepassing voor Excel kunt u gemakkelijk gegevens importeren uit het cluster HDInsight in Excel, waar BI-hulpprogramma's, zoals PowerPivot en toewijzing van kracht kunnen worden gebruikt om te controleren, analyseren en presenteren van de gegevens.

**Gegevens importeren uit een cluster van HDInsight**

1. Open Excel.

2. Een nieuwe, lege werkmap maken.

3. Klik in het menu **Power Query** en **Van Azure**op, **HDInsight van Microsoft Azure**.

    ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]

    **Opmerking:** Als u het menu **Power Query** niet ziet, gaat u naar het **bestand** > **Opties** > **Add-Ins**en selecteer **COM-invoegtoepassingen** uit het vak vervolgkeuzelijst **beheren** onder aan de pagina. Selecteer de knop **Ga naar...** en controleer of het vak voor de Power Query voor Excel-invoegtoepassing is ingeschakeld.

    **Opmerking:** Power Query kunt u gegevens importeren uit HDFS door te klikken op **Vanuit andere bronnen**.

3. Bij **Accountnaam**voert u de naam van de rekening van Azure Blob opslag die is gekoppeld aan het cluster en klik op **OK**. Dit is de [standaard opslag-account](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) of een account gekoppelde opslag.  De indeling is *https://<StorageAccountName>.blob.core.windows.net/*.

4. Voer de code voor de rekening Blob-opslag voor **Key Account**en klik op **Opslaan**. (U moet hiervoor alleen de eerste keer dat u toegang dit archief tot.)

5. Dubbelklik op de naam van de Blob storage container in het deelvenster **Navigator** aan de linkerkant van de Query Editor. De containernaam van de is standaard dezelfde naam als de naam van het cluster.

6. Ga naar **HiveSampleData.txt** in de kolom **naam** (pad naar de map is **... / component- / magazijn/hivesampletable/**), en klik vervolgens op **binaire** aan de linkerkant van het HiveSampleData.txt. HiveSampleData.txt wordt geleverd met alle cluster. Desgewenst kunt u uw eigen bestand.

    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]

7. Als u wilt, kunt u de kolomnamen wijzigen. Wanneer u klaar bent, klikt u op **sluiten en op te laden**.  De gegevens is in de werkmap geladen:

    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe Power Query gegevens ophaalt uit de HDInsight in Excel gebruiken. Op dezelfde manier kunt u gegevens ophalen uit HDInsight in Azure SQL-Database. Het is ook mogelijk om gegevens te uploaden naar HDInsight. Voor meer informatie, Zie de volgende artikelen:

* [Excel verbinden met HDInsight met de component Microsoft ODBC-stuurprogramma][hdinsight-ODBC]
* [Gegevens uploaden naar HDInsight][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.SelectHdiSource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportData.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportedTable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689

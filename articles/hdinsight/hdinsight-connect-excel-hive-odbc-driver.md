<properties
   pageTitle="Hadoop met ODBC-stuurprogramma voor het onderdeel Excel verbinding | Microsoft Azure"
   description="Informatie over het instellen en gebruiken van de Microsoft Component ODBC-stuurprogramma voor Excel query uitvoeren op gegevens in een cluster HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   tags="azure-portal"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

#<a name="connect-excel-to-hadoop-with-the-microsoft-hive-odbc-driver"></a>Hadoop Excel verbinding met de component ODBC-stuurprogramma

[AZURE.INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Onderdelen van Microsoft Business Intelligence (BI) integreert Big Data-oplossing van Microsoft met Apache Hadoop-clusters die zijn geïmplementeerd door de Azure-HDInsight. Een voorbeeld van deze integratie is de mogelijkheid voor Excel verbinding met het datawarehouse component van een cluster Hadoop in HDInsight met het stuurprogramma Microsoft Component Open Database Connectivity (ODBC).

Het kan ook verbinding maken met de gegevens die zijn gekoppeld aan een cluster van HDInsight en andere gegevensbronnen, waaronder andere Hadoop clusters (niet HDInsight), vanuit Excel met de Microsoft Power Query-invoegtoepassing voor Excel. Zie voor informatie over het installeren en gebruiken van Power Query en [Excel verbinding met HDInsight met Power Query][hdinsight-power-query].

> [AZURE.NOTE] Terwijl u de stappen in dit artikel kunnen worden gebruikt met een Linux- of HDInsight op basis van het Windows-cluster, is Windows vereist voor het clientwerkstation.

**Vereisten**:

Voordat u dit artikel hebt u het volgende:

- **Een HDInsight-cluster**. Zie [aan de slag met Azure HDInsight]om een[hdinsight-get-started].
- **Een workstation** met Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 zelfstandig of Office 2010 Professional Plus.


##<a name="install-microsoft-hive-odbc-driver"></a>Microsoft Component ODBC-stuurprogramma installeren

Download en installeer de ODBC-stuurprogramma voor Microsoft component van het [Download Center][hive-odbc-driver-download].

Dit stuurprogramma kan worden geïnstalleerd op 32-bits of 64-bits versies van Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 en Windows Server 2012 en de mogelijkheid verbinding Azure HDInsight (versie 1.6 en hoger) en Azure HDInsight Emulator (v.1.0.0.0 en hoger). U kunt de versie die overeenkomt met de versie van de toepassing waarin u van het ODBC-stuurprogramma gebruikmaakt moet installeren. Voor deze zelfstudie wordt het stuurprogramma gebruikt van Office Excel.

##<a name="create-hive-odbc-data-source"></a>Component ODBC-gegevensbron maken

De volgende stappen uit hoe u een component ODBC-gegevensbron maken.

1. Druk op de Windows-toets voor het openen van het startscherm van Windows 8 of Windows 10, en typt u **gegevensbronnen**.
2. Klik op **instellen van gegevens van ODBC-gegevensbronnen (32-bits)** of **ODBC-gegevensbronnen (64-bits) ingesteld** afhankelijk van uw versie van Office. Als u Windows 7 gebruikt, kiest u **ODBC-gegevensbronnen (32-bits)** of **ODBC-gegevensbronnen (64-bits)** vanuit **Systeembeheer**. Hierdoor wordt het dialoogvenster **ODBC-gegevensbronbeheer** gestart.

    ![OBDC-gegevensbronbeheer][img-hdi-simbahiveodbc-datasource-admin]

3. Van gebruiker DNS klikt u op **toevoegen** om de wizard **Nieuwe gegevensbron maken** .
4. Selecteer **Microsoft Component ODBC-stuurprogramma**en klik vervolgens op **Voltooien**. Hierdoor wordt het dialoogvenster **Microsoft Component ODBC-stuurprogramma voor DNS-installatie** gestart.

5. Typ of Selecteer de volgende waarden:

    Eigenschap|Beschrijving
    ---|---
    De naam van gegevensbron|Geef een naam voor de gegevensbron
    Host|Voer &lt;HDInsightClusterName >. azurehdinsight.net. Bijvoorbeeld: myHDICluster.azurehdinsight.net
    Poort|<strong>443</strong>gebruiken. (Deze poort is gewijzigd van 563 op 443.)
    Database|Gebruik <strong>standaard</strong>.
    Component servertype|Selecteer de <strong>Component Server 2</strong>
    Mechanisme|Selecteer <strong>Azure HDInsight Service</strong>
    HTTP-pad|Laat dit vak leeg.
    Gebruikersnaam|HDInsight cluster gebruikersnaam invoeren. Dit is de gebruikersnaam die is gemaakt tijdens de bepaling van de cluster. Als u de optie snel maken gebruikt, is de standaardgebruikersnaam is <strong>admin</strong>.
    Wachtwoord|HDInsight cluster gebruikerswachtwoord invoeren.
    </table>

    Er zijn enkele belangrijke parameters te houden wanneer u klikt op **Geavanceerde opties**:

    Parameter|Beschrijving
    ---|---
    Oorspronkelijke Query gebruiken|Als deze optie is ingeschakeld, probeert het ODBC-stuurprogramma niet TSQL converteren naar HiveQL. U dient alleen te gebruiken als u bent 100% zeker dat u puur HiveQL-instructies wilt versturen. Wanneer u verbinding maakt met SQL Server of Azure SQL-Database, laat u deze uitgeschakeld.
    Rijen opgehaald per blok|Wanneer u een grote hoeveelheid records ophalen, deze parameter tuning mogelijk moet garanderen optimale prestaties.
    Lengte van de kolom standaard string, binaire kolomlengte, schalen van decimale kolom|De lengte van het gegevenstype en kwaliteitscontroleprogramma kunnen invloed hebben op hoe gegevens worden geretourneerd. Zij wordt onjuiste informatie moet worden geretourneerd als gevolg van verlies van precisie en/of afkappen.


    ![Geavanceerde opties][img-HiveOdbc-DataSource-AdvancedOptions]

6. Klik op **testen** om te testen van de gegevensbron. Wanneer de gegevensbron juist is geconfigureerd, worden *TESTS voltooid!*.
7. Klik op **OK** om het dialoogvenster Test te sluiten. De nieuwe gegevensbron moet nu worden weergegeven op de **ODBC-gegevensbronbeheer**.
8. Klik op **OK** om de wizard te sluiten.

##<a name="import-data-into-excel-from-hdinsight"></a>Gegevens importeren in Excel van HDInsight

De volgende stappen beschrijven de manier om gegevens te importeren uit een component-tabel in een Excel-werkmap met behulp van de ODBC-gegevensbron die u in de bovenstaande stappen hebt gemaakt.

1. Een nieuwe of bestaande werkmap openen in Excel.
2. Klik op **Uit andere gegevensbronnen**vanuit het tabblad **gegevens** en klik vervolgens op **Vanuit Wizard Gegevensverbinding** om de **Wizard Gegevensverbinding**te starten.

    ![Wizard Gegevensverbinding openen][img-hdi-simbahiveodbc.excel.dataconnection]

3. **ODBC DSN** als gegevensbron selecteren en klik vervolgens op **volgende**.
4. Van ODBC-gegevensbronnen, selecteert u de naam van de gegevensbron die u in de vorige stap hebt gemaakt en klik op **volgende**.
5. Voer het wachtwoord voor de cluster in de wizard en klik op **testen** om te controleren van de configuratie opnieuw indien nodig.
6. Klik op **OK** om het dialoogvenster test te sluiten.
7. Klik op **OK**. Wacht tot het dialoogvenster **Database en tabel selecteren** te openen. Dit kan enkele seconden duren.
8. Selecteer de tabel die u wilt importeren en klik op **volgende**. De *hivesampletable* is een voorbeeldtabel component die wordt geleverd met clusters van HDInsight.  Als u nog niet hebt gemaakt, kunt u deze kiezen. Voor meer informatie over het uitvoeren component van query's en tabellen component maken, Zie [Gebruik component met HDInsight][hdinsight-use-hive].
8. Klik op **Voltooien**.
9. U kunt wijzigen of de query opgeven in het dialoogvenster **Gegevens importeren** . Hiertoe klikt u op **Eigenschappen**. Dit kan enkele seconden duren.
10. Klik op het tabblad **definitie** en vervolgens **LIMIET 200** toevoegen aan de component select-instructie in het tekstvak **de tekst van opdracht** . De wijziging beperkt de geretourneerde record is ingesteld op 200.

    ![Eigenschappen van de verbinding][img-hdi-simbahiveodbc-excel-connectionproperties]

11. Klik op **OK** om het dialoogvenster Eigenschappen te sluiten.
12. Klik op **OK** om het dialoogvenster **Gegevens importeren** te sluiten.  
13. Voer het wachtwoord opnieuw en klik op **OK**. Het duurt enkele seconden voordat gegevens naar Excel wordt geïmporteerd.

##<a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u met de component ODBC-stuurprogramma ophalen van gegevens uit de HDInsight Service in Excel. Op dezelfde manier kunt u gegevens ophalen uit de HDInsight-Service in een SQL-Database. Het is ook mogelijk om gegevens te uploaden naar een HDInsight Service. Voor meer informatie, Zie:

- [Gegevens te analyseren vlucht vertraging met HDInsight][hdinsight-analyze-flight-data]
- [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
- [Gebruik Sqoop met HDInsight] [hdinsight-use-sqoop]


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698

[img-hdi-simbahiveodbc-datasource-admin]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png
[img-HiveOdbc-DataSource-AdvancedOptions]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png
[img-hdi-simbahiveodbc-excel-connectionproperties]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveODBC.Excel.ConnectionProperties1.png
[img-hdi-simbahiveodbc.excel.dataconnection]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png

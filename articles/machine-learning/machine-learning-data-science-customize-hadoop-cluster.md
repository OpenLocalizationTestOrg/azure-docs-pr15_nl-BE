<properties 
    pageTitle="Hadoop clusters voor het Team gegevens wetenschap proces aanpassen | Microsoft Azure" 
    description="Populaire Python modules beschikbaar in aangepaste Azure HDInsight Hadoop-clusters."
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="hangzh;bradsev" />

# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a>Azure HDInsight Hadoop clusters voor het Team gegevens wetenschap proces aanpassen 

Dit artikel beschreven hoe een cluster HDInsight Hadoop aanpassen door 64-bits Anaconda (Python 2.7) installeren op elk knooppunt als het cluster is ingericht als een HDInsight-service. Ook ziet u hoe u de headnode in te dienen, aangepaste taken aan het cluster. Deze aanpassing wordt veel populaire Python modules die zijn opgenomen in de Anaconda gemakkelijk beschikbaar zijn voor gebruik in de gebruiker gedefinieerde functies (UDF's) die zijn ontworpen voor het verwerken van records van de component in het cluster. Zie voor instructies over de procedures die in dit scenario, [het indienen van query's component](machine-learning-data-science-move-hive-tables.md#submit).

Het menu onder koppelingen naar onderwerpen over het instellen van de verschillende data science omgevingen die worden gebruikt door het [Team gegevens wetenschap proces (TDSP)](data-science-process-overview.md).

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]


## <a name="customize"></a>Azure HDInsight Hadoop Cluster aanpassen

Een aangepaste HDInsight Hadoop als cluster wilt maken, moeten gebruikers op [**Klassieke Azure Portal**](https://manage.windowsazure.com/)aanmelden, klikt u op **Nieuw** op de hoek links onder, en vervolgens selecteren van gegevens -> HDINSIGHT -> **Aangepaste maken** om het venster **Details van de Cluster** . 

![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

De naam van het cluster wordt gemaakt op de configuratiepagina van 1 invoer, en accepteer de standaardwaarden voor de andere velden. Klik op de pijl om naar de volgende configuratiepagina. 

![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Het aantal **Knooppunten dat gegevens**worden ingevoerd op de configuratiepagina van 2 de **Regio/VIRTUEEL netwerk**selecteren en selecteert u de grootte van het **Knooppunt HEAD** en het **Knooppunt van de gegevens**. Klik op de pijl om naar de volgende configuratiepagina.

>[AZURE.NOTE] De **Regio/VIRTUEEL netwerk** moet hetzelfde zijn als de regio van de opslag-account die wordt gebruikt voor het cluster HDInsight Hadoop. Anders in de vierde pagina van de configuratie, de opslag-account waarmee de gebruikers niet op de keuzelijst met **De naam**.

![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

Op de configuratiepagina van 3 bieden u een gebruikersnaam en wachtwoord voor de cluster HDInsight Hadoop. _De component Oozie/Metastore Enter_selecteert u **niet** . Klik vervolgens op de pijl om naar de volgende configuratiepagina. 

![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

Geef op de configuratiepagina van 4 naam van de opslag, de standaardcontainer van het cluster HDInsight Hadoop. Als gebruikers _standaardcontainer maken_ in de vervolgkeuzelijst **Standaard-CONTAINER** , wordt een container met de naam van het cluster gemaakt. Klik op de pijl om door te gaan naar de laatste configuratiepagina.

![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

Op de laatste pagina in de configuratie **Scriptacties** , klikt u op **scriptactie toevoegen** en vult u de velden voor tekst met de volgende waarden.
 
* **Naam** - een willekeurige tekenreeks als de naam van deze scriptactie. 
* Het **TYPE knooppunt** - selecteert **alle knooppunten**. 
* **URI SCRIPT** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
    * *publicscripts* is een openbare container in opslag account 
    * *getgoing* we PowerShell scriptbestanden gebruiken te vergemakkelijken werk in Azure gebruikers delen. 
* **PARAMETERS** - (leeg laten)

Ten slotte klikt u op het vinkje voor het maken van de aangepaste HDInsight Hadoop cluster start. 

![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Toegang tot de hoofd-knooppunt van het Cluster Hadoop

Gebruikers moeten externe toegang tot het cluster Hadoop in Azure inschakelen voordat deze toegang heeft tot het head-knooppunt van het cluster Hadoop via RDP. 

1. Log in op de [**Klassieke Portal Azure**](https://manage.windowsazure.com/), **HDInsight** aan de linkerkant selecteren Hadoop cluster selecteren uit de lijst met clusters, klikt u op het tabblad **configuratie** en klik vervolgens op het pictogram **Externe inschakelen** onder aan de pagina.
    
    ![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)

2. De velden gebruikersnaam en wachtwoord invoeren in het venster met **Extern bureaublad configureren** en selecteer de vervaldatum voor externe toegang. Klik vervolgens op het selectievakje voor de externe toegang tot de hoofd-knooppunt van het cluster Hadoop.

    ![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)
    
>[AZURE.NOTE] De gebruikersnaam en het wachtwoord voor externe toegang zijn niet de gebruikersnaam en het wachtwoord die u gebruikt bij het maken van het cluster Hadoop. Dit zijn een aparte set met referenties. Ook is de vervaldatum van het RAS binnen 7 dagen vanaf de huidige datum.

Nadat RAS is ingeschakeld, klikt u op **VERBINDEN** onderaan de pagina op afstand in het head-knooppunt. U aanmelden bij het hoofd knooppunt van het cluster Hadoop door het invoeren van de referenties voor de RAS-gebruiker die u eerder hebt opgegeven.

![Werkruimte maken](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

De volgende stappen in het proces van geavanceerde analytics zijn toegewezen in het [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) en kunnen bevatten stappen waarmee u gegevens naar een HDInsight, verwerken en het monster in de voorbereiding op het leren van de gegevens met de computer leren werken met Azure.

Zie [component query's indienen](machine-learning-data-science-move-hive-tables.md#submit) voor instructies over hoe u de Python modules die zijn opgenomen in de Anaconda van de hoofd-knooppunt van het cluster in de gebruiker gedefinieerde functies (UDF's) die worden gebruikt voor het verwerken van component records opgeslagen in het cluster.

 

<properties
    pageTitle="Wat is R op HDInsight? Inleiding tot de Server op HDInsight (voorbeeld) R | Microsoft Azure"
    description="Wat is R Server voor HDInsight (voorbeeld) en het R-Server gebruiken voor het maken van toepassingen voor grote gegevensanalyse."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/17/2016"
   ms.author="jeffstok"/>


# <a name="overview-of-r-server-on-hdinsight-preview"></a>Overzicht van R-Server op HDInsight \(voorbeeld\)

Met Microsoft Azure HDInsight Premium Microsoft R Server is nu beschikbaar als optie bij het maken van HDInsight clusters in Azure. Deze nieuwe mogelijkheid biedt gegevens wetenschappers, statistici en programmeurs R op verzoek toegang tot schaalbare, gedistribueerde methoden van analytics op HDInsight.

Clusters kunnen worden aangepast aan de projecten en taken bij de hand en verwijderd wanneer ze niet langer nodig zijn. Aangezien die deel uitmaken van Azure HDInsight, komen deze clusters op ondernemingsniveau 24 x 7 support een SLA van 99,9% uptime en de flexibiliteit te integreren met andere onderdelen in het ecosysteem van Azure.

>[AZURE.NOTE] R-Server is alleen beschikbaar voor HDInsight Premium. HDInsight Premium is momenteel alleen beschikbaar voor clusters van Hadoop en Spark. Dus kunt op dit moment u R Server alleen met clusters van Hadoop en motoren op HDInsight. Zie voor meer informatie [Wat zijn de verschillende serviceniveaus en Hadoop onderdelen beschikbaar bij HDInsight?](hdinsight-component-versioning.md).

R-Server op HDInsight biedt de nieuwste mogelijkheden voor analytics R gebaseerd op grote gegevenssets die worden geladen op Azure Blob-opslag. Omdat R-Server is gebaseerd op open source R, kunnen de R-gebaseerde toepassingen die u bouwt gebruikmaken van een van de 8000 + R voor open source pakketten, evenals de routines in ScaleR, van big data analytics pakket van Microsoft die is opgenomen met R-Server.

Het randknooppunt van Premium clusters is een handige locatie verbinding maken met het cluster en de R-scripts worden uitgevoerd. Met een randknooppunt hebt u de optie van de ScaleR parallelized gedistribueerde functies uitvoert via de kernen van de rand knooppunt server. U hebt ook de mogelijkheid om ze uit te voeren op de knooppunten van het cluster met behulp van ScaleR Hadoop kaart verminderen of Spark contexten berekenen.

Modellen of voorspellingen die resulteren uit de analyses kan worden gedownload voor gebruik op locatie. Ze kunnen ook worden geoperationaliseerd elders in Azure, bijvoorbeeld via een [Azure Machine Learning Studio](http://studio.azureml.net) [webservice](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-on-hdinsight"></a>Aan de slag met R op HDInsight

Wilt opnemen R Server in een cluster van HDInsight, moet u een Hadoop of een vonk cluster met de optie premie bij het maken van een cluster met behulp van de portal Azure. Beide clustertypen gebruikt dezelfde configuratie, inclusief R-Server op de gegevensknooppunten van een cluster en een randknooppunt als een zone aanvoer voor R servergebaseerde analytics. Zie [Aan de slag met R-Server op HDInsight](hdinsight-hadoop-r-server-get-started.md) voor een uitgebreid overzicht over het maken van een cluster.

## <a name="learn-about-data-storage-options"></a>Meer informatie over opties voor opslag van gegevens

Standaard opslag voor clusters HDInsight is met het bestandssysteem HDFS is toegewezen aan een container blob Blob-opslag. Dit zorgt ervoor dat welke gegevens is geüpload naar de clusteropslag of clusteropslag in de loop van de analyse geschreven, persistent wordt gemaakt. U kunt het hulpprogramma [AzCopy](../storage/storage-use-azcopy.md) om gegevens te kopiëren naar en van de blob.

Blob-opslag gebruiken, hebt u de mogelijkheid van het gebruik van [Azure Lake gegevensopslag](https://azure.microsoft.com/services/data-lake-store/) met het cluster. Als u meer gegevens, kunt vervolgens u zowel Blob-opslag en meer gegevens voor HDFS opslag.

Kun je [Bestanden Azure](../storage/storage-how-to-use-files-linux.md) als opslagoptie voor gebruik op het randknooppunt. Azure-bestanden kunt u wilt koppelen op een bestandsshare die is gemaakt in Azure opslag naar het bestandssysteem voor Linux. Zie voor meer informatie over opties voor het opslaan van gegevens voor R Server op HDInsight cluster [opslagopties voor R Server op HDInsight-clusters](hdinsight-hadoop-r-server-storage.md).

## <a name="access-r-server-on-the-cluster"></a>-R-Server op het cluster

Nadat u een cluster met R-Server hebt gemaakt, kunt u verbinding maken met de R-console op het randknooppunt van het cluster via SSH/stopverf. U kunt ook verbinding maken via een browser als u de optionele RStudio Server IDE te installeren op het randknooppunt. Zie voor meer informatie over het installeren van de RStudio Server [RStudio Server installeren op HDInsight-clusters](hdinsight-hadoop-r-server-install-r-studio.md).   

## <a name="develop-and-run-r-scripts"></a>Ontwikkelen en uitvoeren van scripts R

De R-scripts maken en uitvoeren kunnen van 8000 + R voor open source pakketten naast de parallelized en gedistribueerde routines in de bibliotheek ScaleR gebruiken. In het script dat wordt uitgevoerd in de R-Server op het randknooppunt uitgevoerd binnen de R-interpreter op dat knooppunt. De uitzondering is de stappen die een ScaleR aanroepen, werken met een compute-context die wordt ingesteld Hadoop kaart verminderen (RxHadoopMR) of vonk (RxSpark).

In deze gevallen de functie wordt uitgevoerd in een gedistribueerde wijze over deze gegevens (taak)-knooppunten van het cluster die gekoppeld aan de gegevens waarnaar wordt verwezen zijn. Voor meer informatie over de verschillende compute context opties Zie [Opties voor R Server op HDInsight Premium context te berekenen](hdinsight-hadoop-r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Mogelijk maken een model

Wanneer de gegevens modelleren voltooid is, kunt u het model om voorspellingen voor nieuwe gegevens zowel in Azure en op locatie mogelijk te maken. Dit proces staat bekend als het scoren. Hier volgen een paar voorbeelden.

### <a name="score-in-hdinsight"></a>Score in HDInsight

Om de score in de HDInsight, een R functie schrijven die roept uw model wilt toelichten voor een nieuw bestand dat u hebt geladen op uw account voor opslag. Sla de voorspellingen op de rekening van de opslag. De routine op verzoek kunt u op het randknooppunt van het cluster of met behulp van een geplande taak uitvoeren.  

### <a name="score-in-azure-machine-learning"></a>Score in Azure Machine Learning

Gebruik om te scoren met behulp van een webservice Azure Machine Learning, het open-source Azure Machine Learning R pakket bekend als [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) voor het publiceren van het model als een webservice Azure. Dit pakket is vooraf geïnstalleerd op het randknooppunt voor het gemak. Vervolgens de faciliteiten van Machine Learning gebruiken voor het maken van een gebruikersinterface voor de webservice en roept zo nodig voor het scoren van de webservice.

Als u deze optie kiest, moet u objecten model ScaleR omzetten in equivalente open source modelobjecten voor gebruik met de webservice. U kunt dit doen met behulp van ScaleR dwang functies, zoals `as.randomForest()` voor modellen op basis van het ensemble.


### <a name="score-on-premises"></a>Score van gebouwen

Om te scoren op gebouwen na het maken van uw model, kunt u serialiseren van het model in R, downloaden, interpreteren deze en vervolgens gebruiken voor het scoren van nieuwe gegevens. U kunt nieuwe gegevens scoort met behulp van de benadering die eerder is beschreven in [de score in de HDInsight](#scoring-in-hdinsight) of via [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-the-cluster"></a>Het cluster te beheren

### <a name="install-and-maintain-r-packages"></a>Installeren en onderhouden van de R-pakketten

De R-pakketten die u met de meeste moet op het randknooppunt omdat de meeste van uw scripts R er wordt uitgevoerd. Extra R pakketten installeren op het randknooppunt, kunt u de gebruikelijke `install.packages()` methode in R.

In de meeste gevallen hoeft niet extra R pakketten installeren op de gegevensknooppunten als u alleen routines uit de bibliotheek ScaleR in het cluster. U moet echter extra pakketten met ondersteuning voor het gebruik van **rxExec** of **RxDataStep** kan worden uitgevoerd op de gegevensknooppunten.

In deze gevallen moeten de extra pakketten door gebruik te maken van de scriptactie van een worden opgegeven nadat u het cluster hebt gemaakt. Zie het [maken van een cluster HDInsight met R-Server](hdinsight-hadoop-r-server-get-started.md)voor meer informatie.   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Hadoop kaart minder geheugen wijzigen

Een cluster kan worden gewijzigd om de hoeveelheid geheugen die beschikbaar is voor R-Server tijdens het uitvoeren van een taak verminderen kaart wijzigen. Als u wilt wijzigen in een cluster, gebruik de Apache Ambari gebruikersinterface die beschikbaar is via de Azure portal blade voor uw cluster. Zie voor meer informatie over toegang tot de Ambari UI voor uw cluster, [clusters van HDInsight beheren met behulp van de gebruikersinterface van de website Ambari](hdinsight-hadoop-manage-ambari.md).

Het kan ook de hoeveelheid geheugen die beschikbaar is voor R-Server met behulp van Hadoop-switches in de aanroep van **RxHadoopMR** als volgt wijzigen:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Schaal van het cluster

Een bestaand cluster kan worden geschaald omhoog of omlaag via de portal. Door te schalen, kunt u de extra capaciteit die u mogelijk nodig voor grotere verwerkingstaken toegang of kunt u de schaal terug een cluster wanneer het niet actief is. Zie voor instructies over het schalen van een cluster, [clusters beheren HDInsight](hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Het systeem onderhouden

Onderhoud wordt uitgevoerd op de onderliggende Linux VMs in een cluster HDInsight buiten kantooruren OS patches en andere updates toe te passen. Onderhoud wordt gewoonlijk gedaan om 3:30 AM (gebaseerd op de lokale tijd voor VM) elke maandag en donderdag. Updates worden op zodanige wijze dat zij niet van invloed op meer dan een kwart van het cluster tegelijkertijd worden uitgevoerd.  

Aangezien de hoofd-knooppunten overbodig zijn en niet alle gegevensknooppunten die erdoor worden beïnvloed, kunnen alle taken die worden uitgevoerd tijdens deze periode vertragen. Ze moeten nog steeds volledig uitgevoerd, echter. Aangepaste software of lokale gegevens die u hebt bewaard over deze gebeurtenissen onderhoud tenzij er een onherstelbare fout optreedt waarvoor een cluster opnieuw maken.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>Meer informatie over IDE-opties voor R-Server in een cluster van HDInsight

Het Linux randknooppunt van een cluster HDInsight Premium is de aanvoer zone voor analyse op basis van R. Na het maken van een verbinding met het cluster, kunt u interface van de console met R-Server starten door te typen **R** bij de opdrachtprompt voor Linux. Gebruik van de console-interface is verbeterd als u een teksteditor R ontwikkelen van scripts uitgevoerd in een ander venster en knippen en plakken van secties van het script in de R console wanneer dat nodig is.

Een meer geavanceerde tool voor de ontwikkeling van uw script R is de R-gebaseerde IDE voor gebruik op het bureaublad, zoals onlangs aangekondigd [R Tools voor Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS) van Microsoft. Dit is een familie van desktop- en hulpprogramma's van [RStudio](https://www.rstudio.com/products/rstudio-server/). U kunt ook de Walware op basis van een Eclips [StatET](http://www.walware.de/goto/statet).

Een andere mogelijkheid is een IDE op de Linux randknooppunt zelf installeren.  Een populaire keuze is [RStudio Server](https://www.rstudio.com/products/rstudio-server/)een browser gebaseerde IDE voor externe clients biedt. RStudio Server installeren op de randknooppunt van een cluster HDInsight Premium, biedt een volledige IDE-ervaring voor de ontwikkeling en uitvoering van scripts R met R-Server in het cluster. Kan het zijn aanzienlijk efficiënter dan de R-console.  Als u wilt RStudio Server gebruiken, raadpleegt u [RStudio-Server installeren op HDInsight-clusters](hdinsight-hadoop-r-server-install-r-studio.md).

## <a name="learn-about-pricing"></a>Klik hier voor informatie over prijzen

De kosten die gekoppeld aan een cluster HDInsight Premium met R-Server zijn op dezelfde manier gestructureerd aan de kosten voor de standaard HDInsight clusters. Ze zijn gebaseerd op de grootte van de onderliggende VMs in de naam, de gegevens en de knooppunten van de rand, met de toevoeging van een opwaartse core uur voor de premie. Zie voor meer informatie over HDInsight Premium prijzen, met inbegrip van de prijzen tijdens de Public Preview en de beschikbaarheid van een gratis proefperiode van 30 dagen [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Volgende stappen

Volg de onderstaande koppelingen voor meer informatie over het gebruik van R Server clusters van HDInsight.

- [Aan de slag met R-Server op HDInsight](hdinsight-hadoop-r-server-get-started.md)

- [RStudio-Server toevoegen aan de HDInsight premie](hdinsight-hadoop-r-server-install-r-studio.md)

- [Berekenen context opties voor R Server op HDInsight (voorbeeld)](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure opslagopties R Server op HDInsight Premium](hdinsight-hadoop-r-server-storage.md)

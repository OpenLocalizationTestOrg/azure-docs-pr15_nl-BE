<properties
    pageTitle="Release-opmerkingen voor onderdelen van Hadoop op Azure HDInsight | Microsoft Azure"
    description="Meest recente releaseopmerkingen en versies van onderdelen voor Azure HDInsight Hadoop. Ontwikkeling, tips en informatie opvragen voor Hadoop, Apache Storm en HBase."
    services="hdinsight"
    documentationCenter=""
    editor="cgronlun"
    manager="jhubbard"
    authors="nitinme"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="nitinme"/>


# <a name="release-notes-for-hadoop-components-on-azure-hdinsight"></a>Release-opmerkingen voor onderdelen van Hadoop op Azure HDInsight

## <a name="notes-for-10262016-release-of-r-server-on-hdinsight"></a>Opmerkingen bij de release van R-Server op HDInsight 26-10/2016

- R-Server in het cluster wordt ingericht HDInsight is gestroomlijnd.
- R-Server op HDInsight is nu beschikbaar als gewone HDInsight "R-Server cluster type en niet meer als een afzonderlijke toepassing in de HDInsight geïnstalleerd. Het randknooppunt en de R Server binaire bestanden zijn nu ingericht als onderdeel van de implementatie van R Server cluster. Dit verbetert de snelheid en betrouwbaarheid van de inrichting. Prijsmodel voor R-Server wordt dan overeenkomstig bijgewerkt.
- R Server cluster type prijs is nu gebaseerd op de standaard laag prijs plus R Server toeslag prijs. Premium laag zal nu worden gereserveerd voor Premium-functies die beschikbaar zijn in verschillende clustertypen voor verschillende en wordt niet gebruikt voor R Server cluster. Deze wijziging heeft geen invloed op de effectieve prijzen van R-Server, alleen verandert hoe de kosten worden weergegeven in de wissel. Alle bestaande R Server clusters blijven werken en ARM sjablonen blijven tot de aankondiging van de afschrijving. **Het is raadzaam al voor het bijwerken van uw script implementaties als nieuwe ARM sjabloon wilt gebruiken.**


## <a name="notes-for-08302016-release-of-r-server-on-hdinsight"></a>Notities voor 30/08/2016 release van R-Server op HDInsight

De volledige versienummers voor HDInsight op basis van Linux-clusters in deze release zijn geïmplementeerd:

|HDI |HDI cluster versie   |HDP |HDP-Build   |Ambari bouwen |
|----|----------------------|----|------------|-------------|
|3.2 |3.2.1000.0.8268980    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8268980    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8269383    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

De volledige versienummers voor HDInsight Windows-gebaseerde clusters met deze release zijn geïmplementeerd:

|HDI |HDI cluster versie   |HDP |HDP-Build     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1033.2559206   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1033.2559206    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1033.2559206    |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.1033.2559206    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1033.2559206    |2.3 |2.3.3.1-25    |

## <a name="notes-for-08172016-release-of-r-server-on-hdinsight"></a>Notities voor 17-08/2016 release van R-Server op HDInsight

- R Server 8.0.5 – hoofdzakelijk een bug fix release. Zie de [Releaseopmerkingen R-Server](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) voor meer informatie. 
- AzureML-pakket op het randknooppunt – [Dit pakket R](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) kunt R modellen moeten worden gepubliceerd en verbruikt als een webservice Azure ML.  Zie de sectie [' Model mogelijk maken '](hdinsight-hadoop-r-server-overview.md#operationalize-a-model) van onze ["overzicht van R-Server op HDInsight"](hdinsight-hadoop-r-server-overview.md) -artikel voor meer informatie.
- Linux afhankelijkheden van de [top 100 meest populaire R pakketten](https://github.com/metacran/cranlogs) – deze Linux pakketafhankelijkheden worden nu geïnstalleerd.  
- De optie voor het gebruik van de repo CRAN wanneer R toe te voegen aan de gegevensknooppunten pakketten. Zie de sectie ['Pakketten installeren R'](hdinsight-hadoop-r-server-get-started.md#install-r-packages) onze ["Aan de slag met R-Server op HDInsight"](hdinsight-hadoop-r-server-get-started.md) -artikel voor meer informatie.
- De betrouwbaarheid van R inrichting van servers bij het maken van clusters zijn verbeterd.


## <a name="notes-for-08012016-release-of-hdinsight"></a>Notities voor de release 08/01/2016 van HDInsight

De volledige versienummers voor HDInsight op basis van Linux-clusters in deze release zijn geïmplementeerd:

|HDI |HDI cluster versie   |HDP |HDP-Build   |Ambari bouwen |
|----|----------------------|----|------------|-------------|
|3.2 |3.2.1000.0.8028416    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8028416    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8053402    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

De volledige versienummers voor HDInsight Windows-gebaseerde clusters met deze release zijn geïmplementeerd:

|HDI |HDI cluster versie   |HDP |HDP-Build     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1005.2488842   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1005.2488842    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1005.2488842    |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.1005.2488842    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1005.2488842    |2.3 |2.3.3.1-25    |

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Clustertype (bijvoorbeeld Spark, Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Wijzigingen in clusters 3.4 HDInsight | De standaardwaarde voor de volgende component configuraties zijn gewijzigd voor betere prestaties <ul><li>`hive.vectorized.execution.reduce.enabled=true`</li><li>`hive.tez.min.partition.factor=1f`</li><li>`hive.tez.max.partition.factor=3f`</li><li>`tez.shuffle-vertex-manager.min-src-fraction=0.9`</li><li>`tez.shuffle-vertex-manager.max-src-fraction=0.95`</li><li>`tez.runtime.shuffle.connect.timeout= 30000`</li></ul>| Service    | Alle| N.V.T.|
| Volgende correcties zijn opgenomen in deze release | COMPONENT 13632, HIVE-12897,HIVE-12907,HIVE-12908,HIVE-12988,HIVE-13510,HIVE-13572,HIVE-13716,HIVE-13726,HIVE-12505,HIVE-13632,HIVE-13661,HIVE-13705,HIVE-13743,HIVE-13810,HIVE-13857,HIVE-13902,HIVE-13911,HIVE-13933| Service    | Alle| N.V.T.

## <a name="notes-for-07142016-release-of-hdinsight"></a>Notities voor 14/07/2016 release van HDInsight

De volledige versienummers voor HDInsight op basis van Linux-clusters in deze release zijn geïmplementeerd:

|HDI |HDI cluster versie   |HDP |HDP-Build   |Ambari bouwen |
|----|----------------------|----|------------|-------------|
|3.2 |3.2.1000.0.7932505    |2.2 |2.2.9.1-11  |2.2.1.12-2   |
|3.3 |3.3.1000.0.7932505    |2.3 |2.3.3.1-18  |2.2.1.12-2   |
|3.4 |3.4.1000.0.7933003    |2.4 |2.4.2.0     |2.2.1.12-2   |

De volledige versienummers voor HDInsight Windows-gebaseerde clusters met deze release zijn geïmplementeerd:

|HDI |HDI cluster versie   |HDP |HDP-Build     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.989.2441725    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.989.2441725     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.989.2441725     |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.989.2441725     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.989.2441725     |2.3 |2.3.3.1-21    |

## <a name="notes-for-07072016-release-of-hdinsight"></a>Notities voor de release van HDInsight 07-07/2016

De volledige versienummers voor HDInsight op basis van Linux-clusters in deze release zijn geïmplementeerd:

|HDI |HDI cluster versie   |HDP |HDP-Build   |
|----|----------------------|----|------------|
|3.2 |3.2.1000.0.7864996    |2.2 |2.2.9.1-11  |
|3.3 |3.3.1000.0.7864996    |2.3 |2.3.3.1-18  |
|3.4 |3.4.1000.0.7861906    |2.4 |2.4.2.0     |

De volledige versienummers voor HDInsight Windows-gebaseerde clusters met deze release zijn geïmplementeerd:

|HDI |HDI cluster versie   |HDP |HDP-Build     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.977.2413853    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.977.2413853     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.977.2413853     |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.977.2413853     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.977.2413853     |2.3 |2.3.3.1-21    |

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Clustertype (bijvoorbeeld Spark, Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| [Extra HDInsight voor IntelliJ](hdinsight-apache-spark-intellij-tool-plugin.md) | Invoegtoepassing voor clusters HDInsight Spark IntelliJ IDEE is nu geïntegreerd met Azure Toolkit voor IntelliJ. Het ondersteunt v2.9.1 Azure SDK, nieuwste Java-SDK's en bevat alle functies van de zelfstandige HDInsight Plugin voor IntelliJ.| Hulpprogramma 's    | Spark| N.V.T.|
| [Extra HDInsight voor Eclips](hdinsight-apache-spark-eclipse-tool-plugin.md) | Azure Toolkit voor Eclips biedt nu ondersteuning voor clusters HDInsight Spark. U kunt de volgende functies. <ul><li>Maken en de toepassing van een vonk gemakkelijk in Scala en Java schrijven met eerste klasse schrijven ondersteuning voor IntelliSense automatisch opmaken, foutcontrole, enz.</li><li>De toepassing van de vonk lokaal testen.</li><li>Taken naar een cluster HDInsight Spark indienen en ophalen van de resultaten.</li><li>Log in Azure en toegang tot alle Spark-clusters die zijn gekoppeld aan uw Azure abonnementen.</li><li>De bijbehorende opslagbronnen van het cluster HDInsight Spark navigeren.</li></ul>| Hulpprogramma 's    | Spark| N.V.T.

Wij zijn de Gast OS patch beleid voor HDInsight op basis van Linux-clusters vanaf deze versie is gewijzigd. Het doel van het nieuwe beleid is aanzienlijk minder herstarts vanwege het patchen. Het nieuwe beleid blijft patch virtuele machines (VMs) op Linux-clusters elke maandag of donderdag vanaf 12 uur UTC op een wijze gespreid over de knooppunten in een cluster gegeven. Een bepaalde VM wordt echter alleen opnieuw opgestart hooguit eenmaal per 30 dagen als gevolg van Gast OS patches. Ook de eerste keer opnieuw opstarten van een nieuw cluster gebeurt niet eerder dan 30 dagen na de datum waarop het cluster is gemaakt.

>[AZURE.NOTE] Deze wijzigingen gelden alleen voor nieuwe clusters gelijk of groter is dan deze versie.

## <a name="notes-for-06062016-release-of-hdinsight"></a>Notities voor de release 06/06/2016 van HDInsight

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

|HDP    |HDI versie    |Spark-versie  |Ambari-Build-nummer    |HDP Build-nummer|
|-------|---------------|---------------|-----------------------|----------------|
|2.3    |3.3.1000.0.7702215|    1.5.2|  2.2.1.8-2|  2.3.3.1-18|
|2.4    |3.4.1000.0.7702224|    1.6.1|  2.2.1.8-2|  2.4.2.0|


Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Clustertype (bijvoorbeeld Spark, Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Motoren op HDInsight wordt over het algemeen beschikbaar | Deze versie brengt verbeteringen in de beschikbaarheid, schaalbaarheid en de productiviteit van open source Apache Spark op HDInsight. <ul><li>Toonaangevende beschikbaarheid SLA van 99,9%, waardoor het geschikt is voor veeleisende werklast van de onderneming.</li><li>Schaalbare opslaglaag met Azure Lake gegevensarchief.</li><li>Hulpprogramma's voor productiviteit voor elke fase van de gegevens exploratie en ontwikkeling. Jupyter-laptops met aangepaste Spark kernel inschakelen interactieve gegevens exploratie, integratie met BI-dashboards zoals Power BI, Tableau en Qlik is geschikt voor het snel gegevens delen en continue melden, IntelliJ plugin betrouwbare optie is voor lange termijn code artefact ontwikkeling en foutopsporing.</li></ul>| Service    | Spark| N.V.T.|
| Extra HDInsight voor IntelliJ | Dit is een invoegtoepassing IntelliJ IDEE voor HDInsight Spark clusters. U kunt de volgende functies.<ul><li>Maken en de toepassing van een vonk gemakkelijk in Scala en Java schrijven met eerste klasse schrijven ondersteuning voor IntelliSense automatisch opmaken, foutcontrole, enz.</li><li>De toepassing van de vonk lokaal testen.</li><li>Taken naar een cluster HDInsight Spark indienen en ophalen van de resultaten.</li><li>Log in Azure en toegang tot alle Spark-clusters die zijn gekoppeld aan uw Azure abonnementen.</li><li>De bijbehorende opslagbronnen van het cluster HDInsight Spark navigeren.</li><li>De geschiedenis van taken en taakgegevens voor uw cluster HDInsight Spark navigeren.</li><li>Spark taken op afstand vanaf uw computer opsporen.</li></ul>| Hulpprogramma 's    | Spark| N.V.T.

## <a name="notes-for-05132016-release-of-hdinsight"></a>Notities voor de release van HDInsight 13-05/2016

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight (Windows) 2.1.10.875.2159884 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.875.2159884 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.922.2266903 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.922.2266903 (HDP 2.2.9.1-11)
* HDInsight (Windows) 3.3.0.922.2266903 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.2.1000.0.7565644 (HDP 2.2.9.1-11)
* HDInsight (Linux) 3.3.1000.0.7565644 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.4.1000.0.7548380 (HDP 2.4.2.0)

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Clustertype (bijvoorbeeld Spark, Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Versie-update voor motoren en andere correcties | Deze release updates van de cluster HDInsight Spark-versie 1.6.1 en andere fouten corrigeert| Service    | Spark| N.V.T.

## <a name="notes-for-04112016-release-of-hdinsight"></a>Opmerkingen bij 11-04/2016-versie van HDInsight

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight (Windows) 2.1.10.889.2191206 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.889.2191206 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.889.2191206 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.889.2191206 (HDP 2.2.9.1-10)
* HDInsight (Windows) 3.3.0.889.2191206 (2.3.3.1-16 HDP-ongewijzigd)
* HDInsight (Linux) 3.2.1000.0.7339916 (HDP 2.2.9.1-10)
* HDInsight (Linux) 3.3.1000.0.7339916 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.4.1000.0.7338911 (HDP 2.4.1.1-3)
* 1.5.8 SDK

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Aangepaste metastore upgrade problemen voor HDI 3.4 | Maken van het cluster zou mislukken als u een aangepaste metastore die eerder werd gebruikt in een lagere versie van een ander HDInsight cluster gebruikt. Het gevolg was van een upgrade fout in het script dat is nu opgelost| Maken van het cluster    | Alle | N.V.T.
| Livius Crash Recovery | Project status tolerantie biedt voor elke taak ingediend door Livius | Betrouwbaarheid | Spark op Linux| N.V.T.
| Inhoud Jupyter HA | Hiermee kunt u opslaan en laden van Jupyter laptop inhoud naar en van de opslag-account die is gekoppeld aan het cluster. Voor meer informatie Zie [Kernels die beschikbaar zijn voor Jupyter-laptops](hdinsight-apache-spark-jupyter-notebook-kernels.md).| Laptops | Spark op Linux| N.V.T.
| Verwijdering van hiveContext in Jupter-laptops | Gebruik `%%sql` in plaats van magische `%%hive` magic. SqlContext is gelijk aan hiveContext. Zie voor meer informatie, [Kernels die beschikbaar zijn voor Jupyter-laptops](hdinsight-apache-spark-jupyter-notebook-kernels.md)| Laptops    | Spark clusters op Linux| N.V.T.
| Afschrijving van oudere versies van de vonk | Oudere versie 1.3.1 vonk wordt verwijderd uit de service op 31-5 | Service | Spark-clusters in Windows | N.V.T.

## <a name="notes-for-03292016-release-of-hdinsight"></a>Notities voor 29/03/2016 release van HDInsight

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight (Windows) 2.1.10.875.2159884 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.875.2159884 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.875.2159884 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.875.2159884 (2.2.9.1-7 HDP - ongewijzigd)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (2.2.9.1-8 HDP - ongewijzigd)
* HDInsight (Linux) 3.3.1000.0.7193255 (2.3.3.1-7 HDP - ongewijzigd)
* HDInsight (Linux) 3.4.1000.0.7195842 (HDP 2.4.1.0-327)
* 1.5.8 SDK

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Toegevoegde HDInsight 3.4 versie en de bijgewerkte versies van de HDP voor alle clusters van HDInsight | Deze release, we HDInsight v3.4 (op basis van 2,4 HDP) hebt toegevoegd en ook andere versies HDP hebt bijgewerkt. 2.4 HDP release-opmerkingen zijn beschikbaar [hier](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html) en meer informatie over de HDInsight versies kunnen worden gevonden [hier](hdinsight-component-versioning.md).| Service    | Alle Linux-clusters| N.V.T.
| HDInsight Premium | HDInsight is nu beschikbaar in twee categorieën - Standard en Premium. HDInsight Premium is momenteel in Preview en alleen beschikbaar voor Hadoop en Spark clusters op Linux. Zie voor meer informatie [hier](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium).| Service    | Hadoop en Spark op Linux| N.V.T.
| Microsoft R-Server | HDInsight Premium biedt Microsoft R Server die toegevoegd aan Hadoop en Spark clusters op Linux worden kan. Zie [Overzicht van R-Server op HDInsight](hdinsight-hadoop-r-server-overview.md)voor meer informatie.| Service    | Hadoop en Spark op Linux| N.V.T.
| Spark 1.6.0 | 3.4 HDInsight clusters opnemen Spark 1.6.0 nu| Service    | Spark clusters op Linux| N.V.T.
| Jupyter-laptop-verbeteringen | Jupyter-laptops beschikbaar met vonkontsteking clusters bieden nu extra Spark kernels. Ze bevatten ook verbeteringen, zoals het gebruik van %% magic, auto-visualisatie en integratie met Python visualisatie bibliotheken (zoals matplotlib). Voor meer informatie Zie [Kernels die beschikbaar zijn voor Jupyter-laptops](hdinsight-apache-spark-jupyter-notebook-kernels.md). | Service | Spark clusters op Linux | N.V.T.

## <a name="notes-for-03222016-release-of-hdinsight"></a>Notities voor de release van HDInsight 22-03/2016

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight (Windows) 2.1.10.875.2159884 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.875.2159884 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.875.2159884 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.875.2159884 (2.2.9.1-7 HDP - ongewijzigd)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (2.2.9.1-8 HDP - ongewijzigd)
* HDInsight (Linux) 3.3.1000.0.7193255 (2.3.3.1-7 HDP - ongewijzigd)
* 1.5.8 SDK

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte versies van de HDInsight voor alle clusters van HDInsight | Met deze release versies van de HDInsight voor alle HDInsight clusters is bijgewerkt| Service    | Alle| N.V.T.


## <a name="notes-for-03102016-release-of-hdinsight"></a>Notities voor de release van HDInsight 03/10/2016

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight (Windows) 2.1.10.859.2123216 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.859.2123216 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.859.2123216 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.859.2123216 (HDP 2.2.9.1-7)
* HDInsight (Windows) 3.3.0.859.2123216 (2.3.3.1-5 HDP - ongewijzigd)
* HDInsight (Linux) 3.2.1000.7076817 (HDP 2.2.9.1-8)
* HDInsight (Linux) 3.3.1000.7076817 (HDP 2.3.3.1-7)
* 1.5.8 SDK

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte versies van de HDInsight voor alle clusters van HDInsight | Met deze release versies van de HDInsight voor alle HDInsight clusters is bijgewerkt| Service    | Alle| N.V.T.

## <a name="notes-for-01272016-release-of-hdinsight"></a>Notities voor de release van HDInsight 27-01/2016

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight (Windows) 2.1.10.817.2028315 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.817.2028315 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.817.2028315 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.817.2028315 (HDP 2.2.9.1-1)
* HDInsight (Windows) 3.3.0.817.2028315 (2.3.3.1-5 HDP - ongewijzigd)
* HDInsight (Linux) 3.2.1000.4072335 (HDP 2.2.9.1-1)
* HDInsight (Linux) 3.3.1000.4072335 (HDP 2.3.3.1-1)
* 1.5.8 SDK

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte versies van de HDInsight voor alle clusters van HDInsight | Met deze release versies van de HDInsight voor alle HDInsight clusters is bijgewerkt| Service    | Alle| N.V.T.

## <a name="notes-for-12022015-release-of-hdinsight"></a>Notities voor de release van HDInsight 12/02/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight (Windows) 2.1.10.763.1931434 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.763.1931434 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.763.1931434 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.763.1931434 (2.2.7.1-34 HDP - ongewijzigd)
* HDInsight (Windows) 3.3.1000.0 (HDP 2.3.3.1-5)
* HDInsight (Linux) 3.2.1000.0.6392801 (2.2.7.1-34 HDP - ongewijzigd)
* HDInsight (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* 1.5.8 SDK

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Toegevoegde HDInsight 3.3 versie en de bijgewerkte versies van de HDP voor alle clusters van HDInsight | Deze release, we HDInsight v3.3 (op basis van 2.3 HDP) hebt toegevoegd en ook andere versies HDP hebt bijgewerkt. HDP 2.3 release-opmerkingen zijn beschikbaar [hier](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) en meer informatie over de HDInsight versies kunnen worden gevonden [hier](hdinsight-component-versioning.md).| Service    | Alle| N.V.T.

## <a name="notes-for-11302015-release-of-hdinsight"></a>Notities voor de release van HDInsight 30-11/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight (Windows) 2.1.10.757.1923908 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.757.1923908 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.757.1923908 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.757.1923908 (HDP 2.2.7.1-34)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* 1.5.8 SDK

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte versies van de HDInsight voor alle HDInsight, clusters en HDP-versies voor clusters HDInsight 3.2 (Windows en Linux) | Met deze release, zijn HDInsight en HDP-versies bijgewerkt | Service    | Alle| N.V.T.


## <a name="notes-for-10272015-release-of-hdinsight"></a>Notities voor de release van HDInsight 27-10/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight (Windows) 2.1.10.726.1866228 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight (Windows) 3.0.6.726.1866228 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight (Windows) 3.1.4.726.1866228 (2.1.15.0-2374 HDP - ongewijzigd)
* HDInsight (Windows) 3.2.7.726.1866228 (HDP 2.2.7.1-33)
* HDInsight (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* 1.5.8 SDK

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte versies van de HDInsight voor alle clusters HDInsight (Windows en Linux) | Met deze release, zijn HDInsight en HDP-versies bijgewerkt | Service    | Alle| N.V.T.
| Vaste Jupyter voor Windows Spark clusters met hoofdletters clusters | Clusters met DNS-namen in hoofdletters is opgegeven had problemen met Jupyter-laptops als gevolg van een controle van de aanvraag oorsprong. De oplossing is de DNS-naam voor de configuratie van de Jupyter wijzigen in kleine letters. | Service    | HDInsight vonk (Windows)| N.V.T.


## <a name="notes-for-10202015-release-of-hdinsight"></a>Notities voor de release van HDInsight 20-10/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.716.1846990 (Windows) (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.716.1846990 (Windows) (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.4.716.1846990 (Windows) (HDP 2.1.16.0-2374)
* HDInsight 3.2.7.716.1846990 (Windows) (HDP 2.2.7.1-0004)
* HDInsight 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* 1.5.8 SDK

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Standaard HDP versie 2.2 HDP gewijzigd | De standaardversie voor HDInsight Windows clusters is HDP 2.2 gewijzigd. HDInsight versie 3.2 (HDP 2.2) is sinds het algemeen beschikbaar zijn in februari 2015. Deze wijziging wordt alleen de standaardversie cluster draait wanneer een expliciete keuze niet is gemaakt tijdens het inrichten van het cluster via de portal Azure, PowerShell-cmdlets of de SDK. | Service    | Alle| N.V.T.                  |
|Wijzigingen in de indeling van de VM voor de implementatie van meerdere HDInsight op Linux-clusters in één virtueel netwerk | In deze release wordt ondersteuning voor de implementatie van meerdere HDInsight Linux clusters in één virtuele netwerk toegevoegd. Als onderdeel van dit, de notatie van de namen van de virtuele machine in het cluster is gewijzigd van headnode\*, workernode\* en zookeepernode\* voor hn\*, wn\*, en zk\* respectievelijk. Het is niet aanbevolen een directe afhankelijkheid nemen op de notatie van de namen van de virtuele machine, omdat dit kan worden gewijzigd. Gebruik 'hostname -f' op de lokale computer of Ambari APIs aan de lijst met hosts en de toewijzing van onderdelen om hosts te bepalen. U vindt meer info op [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md) en [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md). | Service | HDInsight clusters op Linux | N.V.T. |
| Wijzigingen in de configuratie | De volgende configuraties zijn nu beschikbaar voor HDInsight 3.1-clusters: <ul><li>tez.yarn.ATS.Enabled en yarn.log.server.url. Hierdoor wordt de tijdlijn Application Server en de server logboek te kunnen rijden op Logboeken.</li></ul>3.2 HDInsight clusters, zijn de volgende configuraties gewijzigd: <ul><li>mapreduce.fileoutputcommitter.Algorithm.Version is ingesteld op 2. Dit maakt gebruik van de V2 versie van de FileOutputCommitter.</li></ul> | Service | Alle | N.V.T. |


## <a name="notes-for-09092015-release-of-hdinsight"></a>Notities voor de release van HDInsight 09/09/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.675.1768697 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.675.1768697 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.4.675.1768697 (2.1.15.0-2334 HDP - ongewijzigd)
* HDInsight 3.2.6.675.1768697 (2.2.6.1-0012 HDP - ongewijzigd)
* 1.5.8 SDK

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte versies van de HDInsight voor alle clusters van HDInsight | Met deze release zijn versies van HDInsight bijgewerkt | Service    | Alle| N.V.T.                  |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Notities voor de release van HDInsight 31-07/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.640.1695824 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.640.1695824 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.4.640.1695824 (2.1.15.0-2334 HDP - ongewijzigd)
* HDInsight 3.2.6.640.1695824 (2.2.6.1-0012 HDP - ongewijzigd)
* 1.5.8 SDK

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Spark cluster knooppunt opnieuw imaging workflow oplossen | Een fout die was het gevolg van Spark knooppunten van het cluster niet herstellen nadat het image opnieuw vast | Service    | Spark| N.V.T.                  |


## <a name="notes-for-07312015-release-of-hdinsight"></a>Notities voor de release van HDInsight 31-07/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.635.1684502 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.635.1684502 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.4.635.1684502 (2.1.15.0-2334 HDP - ongewijzigd)
* HDInsight 3.2.6.635.1684502 (2.2.6.1-0012 HDP - ongewijzigd)
* 1.5.8 SDK

Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte versies van de HDInsight voor alle clusters van HDInsight | Met deze release zijn versies van HDInsight bijgewerkt | Service    | Alle| N.V.T.                  |


## <a name="notes-for-07072015-release-of-hdinsight"></a>Notities voor de release van HDInsight 07-07/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.610.1630216 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.610.1630216 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.4.610.1630216 (2.1.15.0-2334 HDP - ongewijzigd)
* HDInsight 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* 1.5.8 SDK


Deze versie bevat de volgende updates.

| Titel                                           | Beschrijving                                          | Risico gebied (bijvoorbeeld Service, onderdeel of SDK) | Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm) | JIRA (indien van toepassing) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Bijgewerkte versies van de HDP HDInsight 3.2 clusters | Deze versie implementeert 3.2 HDInsight HDP 2.2.6.1-0012 | Service    | Alle                                                 | N.V.T.                  |


## <a name="notes-for-06262015-release-of-hdinsight"></a>Notities voor de release van HDInsight 26-06/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.601.1610731 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.601.1610731 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.4.601.1610731 (2.1.15.0-2334 HDP - ongewijzigd)
* HDInsight 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* 1.5.8 SDK


Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm)</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Bijgewerkte versies van de HDP HDInsight 3.2 clusters</td>
<td>Deze versie implementeert 3.2 HDInsight HDP 2.2.6.1</td>
<td>Service</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

</table>

## <a name="notes-for-06182015-release-of-hdinsight"></a>Notities voor de release van HDInsight 18-06/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.596.1601657 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.596.1601657 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* HDInsight 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* 1.5.8 SDK


Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm)</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Extra HTTPS-poorten geopend</td>
<td>De service cloud nu 5 poorten 8001 aan 8005 blijven ingedeeld op het cluster bijv geopend op https://<clustername>.azurehdinsight.net:8001/. Aanvragen naar deze URL's worden geverifieerd met de methode Basisverificatie wachtwoord als poort 443. Deze poorten worden gebonden op dezelfde poort op de actieve headnode. Scriptacties kunnen worden gebruikt om klantenservice luisteren op deze poorten op de headnode en de route naar buiten het cluster.</td>
<td>Cloud-Service</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Periodieke MapReduce willekeurige volgorde probleem voor HDInsight 3.2</td>
<td>Voor een zeldzaam, met tussenpozen conflictsituatie in willekeurige volgorde Map beperken op grote clusters, waardoor er fouten oplossen. Zie <a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a> voor meer informatie.</td>
<td>Hadoop-Core</td>
<td>Alle</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE-6361</a></td>
</tr>

<tr>
<td>Verplaatsen naar laatste Azure Java SDK 2.2 voor HDInsight 3.2</td>
<td>Verplaatst naar de meest recente versie van de SDK Azure voor Java door het stuurprogramma WASB gebruikt. De nieuwste SDK heeft enkele correcties en de release-opmerkingen voor dezelfde zijn beschikbaar op https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt.</td>
<td>Hadoop-Core</td>
<td>Alle</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOP 11959</a></td>
</tr>

<tr>
<td>Hiermee gaat u naar de HDP 2.1.15 HDInsight 3.1 clusters</td>
<td>Release-opmerkingen voor de Hortonworks zijn beschikbaar <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">hier</a>.</td>
<td>HDP</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

</table>

## <a name="notes-for-06042015-release-of-hdinsight"></a>Notities voor de release van HDInsight 06/04/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.583.1575584 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.583.1575584 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.3.583.1575584 (2.1.12.1-0003 HDP - ongewijzigd)
* HDInsight 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* 1.5.8 SDK


Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm)</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>502 Ongeldige gateway fout voor Storm clusters oplossen</td>
<td>Deze versie herstelt een bug die gevolgen heeft voor de taak indiening API waardoor de website moet u na het opnieuw opstarten.</td>
<td>Service</td>
<td>Storm</td>
<td>N.V.T.</td>
</tr>

</table>

## <a name="notes-for-06012015-release-of-hdinsight"></a>Notities voor de release van HDInsight 01-06/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.577.1563827 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.577.1563827 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.3.577.1563827 (2.1.12.1-0003 HDP - ongewijzigd))
* HDInsight 3.2.4.577.1563827 (2.2.6.0-2800 HDP - ongewijzigd)
* 1.5.8 SDK


Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm)</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Verschillende correcties</td>
<td>Deze release corrigeert fouten die betrekking hebben op het cluster wordt ingericht.</td>
<td>Service</td>
<td>Alle soorten van cluster</td>
<td>N.V.T.</td>
</tr>

</table>

## <a name="notes-for-05272015-release-of-hdinsight"></a>Notities voor de release van HDInsight 05/27/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* Andere versies van de cluster en de SDK zijn niet als onderdeel van deze release geïmplementeerd.


Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm)</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>2.2 HDP-update</td>
<td>Deze versie van HDInsight 3.2 bevat HDP 2.2.6 en brengt enkele belangrijke bugfixes tot HDInsight. De volledige release-informatie is beschikbaar op <a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 Release-informatie voor</a>.</td>
<td>HDP</td>
<td>Alle soorten van cluster</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Standaardconfiguratie garen Container geheugen wijzigen</td>
<td>In deze update wordt wordt de standaard geheugen beschikbaar om containers garens (yarn.nodemanager.resource.memory- en yarn.scheduler.maximum-toewijzing-mb), gestart door Node Manager verhoogd tot 5632MB. Eerder dit is teruggebracht tot 4608MB, maar op basis van verschillende taak wordt uitgevoerd, de nieuwe waarde moet bieden betere betrouwbaarheid en prestaties voor de meeste taken, en is daarom een betere standaard. Als de gebruikelijke, als u een kritische afhankelijkheid van deze geheugenconfiguratie hebben, moet expliciet instellen tijdens het maken van het cluster.</td>
<td>HDP</td>
<td>Alle soorten van cluster</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Standaard Config pariteit voor clusters HBase en Storm</td>
<td>Deze update herstelt Hbase en Storm clusters voor het gebruik van GAREN configuraties dezelfde waarden als Hadoop-clusters. Dit wordt gedaan voor pariteit bij alle clustertypen.</td>
<td>HDP</td>
<td>HBase, Storm</td>
<td>N.V.T.</td>
</tr>

</table>

## <a name="notes-for-05202015-release-of-hdinsight"></a>Notities voor de release van HDInsight 20/05/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.564.1542093 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.564.1542093 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* HDInsight 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* 1.5.8 SDK

Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm)</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Ondersteuning voor SCP.NET-EventHub</td>
<td>De pakketten met bijgewerkte cluster voor HDInsight Storm weer SCP.NET nieuwe functies. U hebt nu toegang tot de nieuwe API's in de topologie builder die via EventHubSpout of Spouts van Java te gebruiken. Uw SCP.NET client SDK werken met nieuwe clusters, zoals de contracten zijn bijgewerkt, moet u bijwerken. Raadpleeg het Leesmij-bestand opgenomen in het pakket SCP.NET nuget voor meer informatie over de nieuwe API's, het gebruik en release notes (inclusief correcties).</td>
<td>VS, gereedschap</td>
<td>Storm-3.2 HDInsight clusters</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>JDBC-stuurprogramma bijwerken</td>
<td>Het stuurprogramma is bijgewerkt naar de versie van SQL Server wordt ondersteund in sqljdbc_4.1.5605.100.</td>
<td>Metastore</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>
</table>

## <a name="notes-for-04272015-release-of-hdinsight"></a>Notities voor de release van HDInsight 27/04/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.537.1486660 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.537.1486660 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.3.537.1486660 (2.1.12.0-2329 HDP - ongewijzigd)
* HDInsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* 1.5.8 SDK

Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm)</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>FIX DLL-afhankelijkheid</td>
<td>Hiermee verwijdert u een afhankelijkheid op Unit Test Framework HDInsight.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Bug fix voor conflictsituatie</td>
<td>Een cluster maken verzoek nu wacht op verzoek moeten worden geaccepteerd voordat polling in de status plaatsen</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N.V.T.</td>
</tr>
</table>

## <a name="notes-for-04142015-release-of-hdinsight"></a>Notities voor de release van HDInsight 14-04/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.521.1453250 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.521.1453250 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.3.521.1453250 (2.1.12.0-2329 HDP - ongewijzigd)
* HDInsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* SDK 1.5.6 WORDEN UITGEZET

Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm)</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Tez bugfixes</td>
<td>Correcties voor Apache TEZ. 2214 en TEZ 1923 zijn opgenomen in deze versie van HDI 3.2. Deze zijn specifiek voor een bepaalde component query's op Tez waarvoor een aanzienlijke hoeveelheid gegevens in willekeurige volgorde nodig.
</td>
<td>HDP</td>
<td>Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ. 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ, 1923</a></td>
</tr>
</table>

## <a name="notes-for-04062015-release-of-hdinsight"></a>Notities voor de release van HDInsight 04/06/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.521.1453250 (1.3.12.0-01795 HDP - ongewijzigd)
* HDInsight 3.0.6.521.1453250 (2.0.13.0-2117 HDP - ongewijzigd)
* HDInsight 3.1.3.521.1453250 (2.1.12.0-2329 HDP - ongewijzigd)
* HDInsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* SDK 1.5.6 WORDEN UITGEZET

Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm)</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>HDInsight .NET SDK 1.5.6 worden uitgezet</td>
<td>Updates voor het verwijderen van enkele interne klassen voor HDInsight op Linux.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Avro-bibliotheek 1.5.6 worden uitgezet</td>
<td>Toegevoegde <b>KnownTypeAttribute</b> voor de methode <b>GetAllKnownTypes</b>. Vaste NullReferenceException wanneer een type leeg voor de methode GetAllKnownTypes is.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Programmafouten</td>
<td>Verschillende correcties voor de service</td>
<td>Service</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

</table>
<br>

## <a name="notes-for-04012015-release-of-hdinsight"></a>Notities voor de release van HDInsight 01-04/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* HDInsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* HDInsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* HDInsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* 1.5.5 SDK

Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm)</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Mogelijkheid om extern bureaublad referenties op Windows-clusters via .NET SDK in-of uitschakelen</td>
<td>Softwareondersteuning voor in- of uitschakelen van RDP referenties op Windows-clusters.</td>
<td>SDK</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Om ervoor te zorgen extern bureaublad referenties op clusters terwijl deze wordt ingericht</td>
<td>Softwareondersteuning voor referenties voor extern bureaublad inschakelen als het cluster wordt gemaakt. Hiermee verwijdert u het proces voor het eerst het cluster wordt ingericht en vervolgens extern bureaublad inschakelen.</td>
<td>SDK</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Bijgewerkte Python aan punt 2.7.8</td>
<td>Bijgewerkte Python in punt 2.7.8, Python HDInsight Clusters bevat enkele belangrijke security oplossingen voor HDInsight versie 2.1, 3.0, 3.1 en 3.2</td>
<td>Service</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Wijziging in de configuratie garens</td>
<td>Gewijzigde GAREN configuratie yarn.resourcemanager.max voltooid-toepassingen op 1000 voor alle clustertypen voor HDInsight versie 3.1 en 3.2. Deze waarde bepaalt alleen de lijst van voltooide toepassingen in de gebruikersinterface van het GAREN. Als u informatie over toepassingen die zijn ingediend voor de lijst met toepassingen die worden weergegeven in de gebruikersinterface, kunt u rechtstreeks naar de Server geschiedenis gaan.</td>
<td>GARENS</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Grootte van de knooppunten in een cluster HBase</td>
<td>HBase clusters nu mag formaat van knooppunten (omhoog en omlaag) voor HDInsight versie 3.1 en 3.2</td>
<td>Service</td>
<td>HBase</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>JDBC-upgrade</td>
<td>SQL JDBC-stuurprogramma is bijgewerkt naar versie sqljdbc_4.0.2206.100 voor HDInsight versie 3.2. Deze versie bevat belangrijke beveiligingsverbeteringen.</td>
<td>HDP</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Configuratie-update van JVM</td>
<td>Bijgewerkte JVM configuratie networkaddress.cache.ttl tot 300 seconden van de standaardwaarde-1 voor HDInsight versie 3.1 en 3.2. Deze configuratiewaarde bepaalt het cachebeleid voor geslaagde zoekacties uit de van naamservice. Dit lost een bug die betrekking hebben op het vergroten en verkleinen HBase clusters.</td>
<td>Service</td>
<td>HBase</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Een upgrade uitvoeren naar Azure opslag SDK for Java 2.0</td>
<td>HDInsight versie 3.2 is bijgewerkt om de meest recente versie van de opslag Azure SDK voor Java gebruiken. Dit document bevat verschillende belangrijke bugfixes via de huidige 0.6.0 versie.</td>
<td>HDP</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Upgrade naar de broncode van Apache WASB</td>
<td>HDInsight versie 3.2 nu gebruikt de laatste code voor het stuurprogramma WASB filesystem van Apache Hadoop. Met deze wijziging wordt nu het stuurprogramma WASB geleverd als een afzonderlijke jar. Dit is uitsluitend een wijziging van de verpakking en bevat niet alle wijzigingen in het gedrag van het stuurprogramma WASB. De naam van dit bestand JAR is hadoop-azure-2.6.0.jar.</td>
<td>HDP</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>JAR-bestand bijgewerkt in punt 3.2 HDInsight</td>
<td>Deze update aan HDInsight versie 3.2 bevat verschillende correcties en een paar potten van interne geleverd als onderdeel van de HDP zijn bijgewerkt. Let op: deze JAR-bestanden zijn intern aan het pakket HDP en niet voor rechtstreeks gebruik door toepassingen van de klant. Toepassingen moeten hun eigen versie van de potten pakket, zodat een upgrade naar de interne potten HDP beschadigt toepassingen van de klant.</td>
<td>HDP</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

</table>
<br>

## <a name="notes-for-03032015-release-of-hdinsight"></a>Notities voor de release van HDInsight 03/03/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.488.1375841 (1.3.9.0-01351 HDP - ongewijzigd)
* HDInsight 3.0.6.488.1375841 (2.0.9.0-2097 HDP - ongewijzigd)
* HDInsight 3.1.3.488.1375841 (2.1.10.0-2290 HDP - ongewijzigd)
* HDInsight 3.2.3.488.1375841 (HDP-2.2.10.0-. 2340 - ongewijzigd)
* SDK 1.5.0 (ongewijzigd)

Deze versie bevat de volgende update.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld service, onderdeel of SDK)</p></th>
<th>Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm)</th>
<th>JIRA</th>
</tr>


<tr>
<td>Verbeteringen van de betrouwbaarheid</td>
<td>We oplossingen die de service beter geschaald met de toegenomen belasting met betrekking tot het maken van het cluster hebt gemaakt.</td>
<td>Service</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>



</table>
<br>

## <a name="notes-for-02182015-release-of-hdinsight"></a>Notities voor de release van HDInsight 18-02/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.471.1342507 (1.3.9.0-01351 HDP - ongewijzigd)
* HDInsight 3.0.6.471.1342507 (2.0.9.0-2097 HDP - ongewijzigd)
* HDInsight 3.1.3.471.1342507 (2.1.10.0-2290 HDP - ongewijzigd)
* HDInsight 3.2.3.471.1342507 (HDP-2.2.10.0-. 2340)
* SDK 1.5.0

Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm)</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>3.2 HDInsight clusters</td>
<td>Hadoop 2.6/HDP2.2 3.2 HDInsight clusters beschikbaar is. Het bevat belangrijke updates voor alle open source componenten. Zie Wat is nieuw in HDInsight en <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">HDP 2.2.0.0 Release-opmerkingen</a>voor meer informatie.</td>
<td>Open source software</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>HDinsight op Linux (voorbeeld)</td>
<td>Clusters kunnen worden ingezet op Ubuntu Linux. Voor meer informatie, Zie aan de slag met HDInsight op Linux.</td>
<td>Service</td>
<td>Hadoop</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Storm algemene beschikbaarheid</td>
<td>Apache Storm clusters zijn in het algemeen beschikbaar. Voor meer informatie Zie beginnen met Storm in HDInsight.</td>
<td>Service</td>
<td>Storm</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Grootte van de virtuele machine</td>
<td>Azure HDInsight is beschikbaar op meer virtuele machine soorten en maten. HDInsight kunnen gebruikmaken van A2 naar A7 formaten gebouwd voor algemene doeleinden; D-reeks knooppunten die functie solid-state schijven (SSD) en 60 procent snellere processor; en A8 en A9-formaat met InfiniBand ondersteuning voor snelle toegang.</td>
<td>Service</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Cluster schalen</td>
<td>U kunt het aantal gegevensknooppunten voor een actief HDInsight cluster wijzigen zonder te verwijderen of opnieuw te maken. Op dit moment hebben alleen Hadoop Query Apache Storm clustertypen en deze mogelijkheid, maar de ondersteuning voor Apache HBase cluster binnenkort is te volgen. Zie HDInsight beheren van clusters voor meer informatie.</td>
<td>Service</td>
<td>Hadoop, Storm</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Visual Studio tooling</td>
<td>Naast de volledige tooling voor Apache Storm is de tooling voor Apache component in Visual Studio bijgewerkt met voltooiing van instructies, lokale validatie en verbeterde ondersteuning voor foutopsporing. Zie aan de slag met HDInsight Hadoop-hulpprogramma's voor Visual Studio voor meer informatie.</td>
<td>Tooling</td>
<td>Hadoop</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Hadoop-Connector voor DocumentDB</td>
<td>Met Hadoop-Connector voor DocumentDB, kunt u complexe aggregaties, analyse en manipulaties uitvoeren via uw schema minder JSON-documenten via DocumentDB collecties of via de rekeningen van de database opgeslagen. Zie voor meer informatie en een zelfstudie Hadoop uitvoeren van taken met behulp van DocumentDB en HDInsight.</td>
<td>Service</td>
<td>Hadoop</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Programmafouten</td>
<td>We hebben verschillende kleine bugfixes voor services HDInsight aangebracht. Geen gedragswijzigingen klantgerichte worden verwacht.</td>
<td>Service</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

</table>
<br>

## <a name="notes-for-02062015-release-of-hdinsight"></a>Notities voor de release van HDInsight 06-02/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.463.1325367 (1.3.9.0-01351 HDP - ongewijzigd)
* HDInsight 3.0.6.463.1325367 (2.0.9.0-2097 HDP - ongewijzigd)
* HDInsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* N.V.T. SDK

Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm)</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Programmafouten</td>
<td>We hebben verschillende kleine bugfixes voor services HDInsight aangebracht. Geen gedragswijzigingen klantgerichte worden verwacht.</td>
<td>Service</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Update voor 2.1 HDP-onderhoud</td>
<td>3.1 HDInsight is HDP 2.1.10.0 implementeren bijgewerkt. Zie voor meer informatie, <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">Release Notes HDP-2.1.10</a>. </td>
<td>Open source software</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>HDP binaire updates</td>
<td>Er zijn een paar JAR-bestanden in HBase voor welk bestand namen zijn bijgewerkt. Deze JAR-bestanden worden intern gebruikt door HBase, dus is het niet waarschijnlijk dat klanten een afhankelijkheid van de namen van deze JAR-bestanden hebben. Deze omvatten:
<ul>
<li>./lib/jetty-6.1.26.hwx.jar</li>
<li>./lib/jetty-sslengine-6.1.26.hwx.jar</li>
<li>./lib/jetty-Util-6.1.26.hwx.jar</li>
</ul>
</td>
<td>Open source software</td>
<td>HBase</td>
<td>N.V.T.</td>
</tr>

</table>
<br>

## <a name="notes-for-1292015-release-of-hdinsight"></a>Notities voor de release van HDInsight 29-1/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.455.1309616 (1.3.9.0-01351 HDP - ongewijzigd)
* HDInsight 3.0.6.455.1309616 (2.0.9.0-2097 HDP - ongewijzigd)
* HDInsight 3.1.2.455.1309616 (2.1.9.0-2196 HDP - ongewijzigd)
* N.V.T. SDK

Deze versie bevat de volgende update.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Risico gebied (bijvoorbeeld Service, onderdeel of SDK)</p></th>
<th>Cluster-Type (bijvoorbeeld Hadoop, HBase of Storm)</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>

<td>Programmafouten</td>
<td>We hebben een paar belangrijke bugfixes ter verbetering van de betrouwbaarheid van de Clusters HDInsight tijdens upgrades Azure aangebracht.</td>
<td>Service</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>



</table>
<br>

## <a name="notes-for-152015-release-of-hdinsight"></a>Notities voor de release van HDInsight 1/5/2015

De volledige versienummers voor HDInsight clusters in deze release zijn geïmplementeerd:

* HDInsight 2.1.10.420.1246118 (1.3.9.0-01351 HDP - ongewijzigd)
* HDInsight 3.0.6.420.1246118 (2.0.9.0-2097 HDP - ongewijzigd)
* HDInsight 3.1.2.420.1246118 (2.1.9.0-2196 HDP - ongewijzigd)


Deze versie bevat de volgende updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Onderdeel</th>
<th>Clustertype</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Twitter trend analysemateriaal en aanbevelingen op basis van Mahout film</td>
<td><p>In deze versie is de HDInsight Query console twee extra monsters:</p>

<p><b>Trendanalyse Twitter</b><br>
Openbare API's die door websites zoals Twitter zijn een nuttige bron van gegevens voor het analyseren en begrijpen van de populaire trends. In deze zelfstudie leren component gebruiken om een lijst van Twitter gebruikers die de meeste tweets die een bepaald woord bevatten verzonden. </p>

<p><b>Aanbeveling mahout film</b><br>
Apache Mahout is een Apache Hadoop machine learning library. Mahout bevat algoritmen voor het verwerken van gegevens (zoals filteren, classificatie en clustering). In deze zelfstudie gebruiken een aanbeveling engine voor het genereren van film aanbevelingen op basis van films die je vrienden hebben gezien.</p></td>
<td>Query-console</td>
<td>Hadoop</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Wijzigen in een standaardwaarde voor de configuratie van component: hive.auto.convert.join.noconditionaltask.size</td>
<td><p>Deze configuratie grootte is van toepassing op de kaart automatisch geconverteerde joins. De waarde geeft de som van de grootte van tabellen die kunnen worden geconverteerd naar een hash-kaarten die in het geheugen passen. In een eerdere versie, deze waarde verhoogd tot 128 MB van de standaardwaarde van 10 MB. De nieuwe waarde van 128 MB is echter taken mislukken vanwege onvoldoende geheugen veroorzaken. Deze versie wordt de standaardwaarde terug naar 10 MB. Klanten kunnen nog steeds kiezen voor deze waarde overschrijven tijdens het maken van het cluster, gezien hun query's en de grootte van de tabel. Zie voor meer informatie over deze instelling en het negeren van het <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">Automatisch optimaliseren Join conversie</a> in Hortonworks documentatie. </p></td>
<td>Component</td>
<td>Hadoop, Hbase</td>
<td>N.V.T.</td>
</tr>

</table>
<br>

## <a name="notes-for-12232014-release-of-hdinsight"></a>Opmerkingen bij de release van HDInsight 23-12/2014

De volledige versie nummers voor HDInsight-serverclusters die in deze versie zijn:

* HDInsight 2.1.10.420.1246783 (HDP versie ongewijzigd)
* HDInsight 3.0.6.420.1246783 (HDP versie ongewijzigd)
* HDInsight 3.1.1.420.1246783 (HDP versie ongewijzigd)

Deze versie bevat de volgende update.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Onderdeel</th>
<th>Clustertype</th>
<th>JIRA (indien van toepassing)</th>
</tr>


<tr>
<td>Fouten bij het maken van periodieke cluster als gevolg van overmatige belasting</td>
<td><p>Verbeterd algoritme voor het downloaden van pakketten HDP tijdens het maken van het cluster kan meer robuuste behandeling van fouten als gevolg van overmatige belasting.</p></td>
<td>Service</td>
<td>Hadoop, Hbase, Storm</td>
<td>N.V.T.</td>
</tr>



</table>
<br>

## <a name="notes-for-12182014-release-of-hdinsight"></a>Opmerkingen bij de release van HDInsight 18-12/2014

Deze versie bevat de volgende update voor het onderdeel.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Onderdeel</th>
<th>Clustertype</th>
<th>JIRA (indien van toepassing)</th>
</tr>

<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">Cluster aanpassing algemene Avalability</a></td>
<td><p>Aanpassing kunt u clusters met projecten die beschikbaar via de Apache Hadoop-ecosysteem zijn Azure HDInsight aanpassen. Met deze nieuwe functie kunt u experimenteren en implementeren van Hadoop projecten naar Azure HDInsight. Dit is ingeschakeld via de **Actie Script** functie Hadoop clusters in willekeurige manieren aanpassen kunt met behulp van aangepaste scripts. Deze aanpassing is beschikbaar op alle soorten HDInsight clusters zoals Hadoop, HBase en Storm. Voorbeelden van de kracht van deze mogelijkheid hebben we het proces om de populaire <a href = "hdinsight-hadoop-spark-install.md" target="_blank">Spark</a>, <a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>, <a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solr</a>en <a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a> modules te installeren beschreven. Deze release voegt ook de mogelijkheid voor klanten om aan te geven hun actie aangepast script via de portal voor Azure bevat richtlijnen en aanbevolen procedures over het bouwen van aangepaste scriptacties met behulp van methoden en richtlijnen over het testen van de scriptactie. </p></td>
<td>Functie algemene beschikbaarheid</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>


</table>
<br>

## <a name="notes-for-12052014-release-of-hdinsight"></a>Notities voor 05-12-2014-versie van HDInsight

De volledige versie nummers voor HDInsight-serverclusters die in deze versie zijn:

* HDInsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* HDInsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* HDInsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* HDInsight SDK N.V.T.

Deze versie bevat de volgende updates van onderdelen.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Onderdeel</th>
<th>Clustertype</th>
<th>JIRA (indien van toepassing)</th>
</tr>

<tr>
<td>Bug fix: periodieke fout tijdens het toevoegen van een groot aantal partities op een tabel in een component-DDL </td>
<td><p>Als er een fout met de verbinding met de component metastore database wanneer u een groot aantal partities toevoegt aan een component-tabel, kan de component DDL mislukken. De volgende instructie is zichtbaar in het foutenlogboek component als deze fout zich voordoet: </p><p>"Fout [main]: ql. Stuurprogramma (SessionState.java:printError(547)) - is mislukt: fout bij uitvoering, de retourcode 1 van org.apache.hadoop.hive.ql.exec.DDLTask. MetaException (message:java.lang.RuntimeException: commitTransaction is aangeroepen, maar openTransactionCalls = 0. Dit waarschijnlijk geeft aan dat er onbalans aanroepen van openTransaction/commitTransaction)"</p></td>
<td>Component</td>
<td>Hadoop, Hbase</td>
<td>COMPONENT-482 (dit is een interne JIRA kan niet extern worden aangeboden. Vermeld hier voor verwijzing.)</td>
</tr>

<tr>
<td>Bug fix: af en toe vastlopen in de Console van de Query HDInsight</td>
<td>Als dit gebeurt, met de volgende instructie ziet u in het logboek voor WebHCat voor de taak WebHCat starten: <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper | org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): geschiedenisbestand {wasb url naar het geschiedenisbestand} kan niet worden geladen '</p></td>
<td>WebHCat</td>
<td>Hadoop</td>
<td>COMPONENT-482 (dit is een interne JIRA kan niet extern worden aangeboden. Vermeld hier voor verwijzing.)</td>
</tr>

<tr>
<td>Bug fix: incidentele Prikker in de latentie van query's Hbase</td>
<td>Als dit gebeurt, zullen gebruikers merken dat een incidentele Prikker van 3 seconden in de latentie van query's Hbase. </td>
<td>HDInsight Cluster Gateway</td>
<td>HBase</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>HDP JAR bestand naamswijzigingen</td>
<td>Cluster voor HDI versie 3.0, er zijn een aantal wijzigingen in de interne JAR-bestanden geïnstalleerd door HDP. jetty 6.1.26.jar is vervangen door jetty 6.1.26.hwx.jar. jetty-util-6.1.26.jar is vervangen door jetty-util-6.1.26.hwx.jar. Deze wijzigingen zijn van toepassing op projecten van Hadoop, Mahout, WebHCat en Oozie.</td>
<td>Hadoop, Mahout, WebHCat, Oozie</td>
<td>Hadoop, HBase</td>
<td>N.V.T.</td>
</tr>

</table>
<br>


## <a name="notes-for-11212014-release-of-hdinsight"></a>Notities voor de release van HDInsight 21-11/2014

De volledige versie nummers voor HDInsight-serverclusters die in deze versie zijn:

* HDInsight 2.1.9.382.1169709 (geen wijziging van 14-11/2014)
* HDInsight 3.0.5.382.1169709 (geen wijziging van 14-11/2014)
* HDInsight 3.1.1.382.1169709 (geen wijziging van 14-11/2014)
* HDINsight SDK 1.4.0

Deze versie bevat de volgende updates van onderdelen.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Onderdeel</th>
<th>Clustertype</th>
<th>JIRA (indien van toepassing)</th>
</tr>

<tr>
<td>Toegang tot toepassingslogboeken</td>
<td>Mogelijkheid voor het programmatisch opsommen en toepassingen die op clusters worden uitgevoerd en relevante toepassingsspecifieke of container-specifieke Logboeken om foutopsporing problematische toepassingen te downloaden.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Mogelijkheid om de regionaam opgeven in de IHdInsightClient.DeleteCluster </td>
<td>De Azure HDInsight SDK biedt de mogelijkheid om een regio opgeven wanneer u met behulp van **DeleteCluster**. Dit helpt klanten die had twee resources met dezelfde naam in verschillende regio's en kan niet worden verwijderd op een van hen was de blokkering opheffen.</td>
<td>SDK</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>ClusterDetails.DeploymentId</td>
<td>Het **ClusterDetails** -object geeft als resultaat een **DeploymentID** veld dat een unieke id voor het cluster vertegenwoordigt. Het is gegarandeerd uniek binnen een cluster maken pogingen met dezelfde naam.</td>
<td>SDK</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>
</table>
<br>

## <a name="notes-for-11142014-release-of-hdinsight"></a>Opmerkingen bij de release van HDInsight 14-11/2014

De volledige versie nummers voor HDInsight-serverclusters die in deze versie zijn:

* HDInsight 2.1.9.382.1169709
* HDInsight 3.0.5.382.1169709
* HDInsight 3.1.1.382.1169709

Deze versie bevat de volgende nieuwe voorzieningen, updates van onderdelen en bugfixes.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Onderdeel</th>
<th>Clustertype</th>
<th>JIRA (indien van toepassing)</th>
</tr>

<tr>
<td>Scriptactie (voorbeeld)</td>
<td>Voorbeeld van de functie voor het cluster aanpassen waarmee een wijziging van Hadoop-clusters op willekeurige manieren met behulp van aangepaste scripts. Met deze functie kunnen gebruikers experimenteren met en implementeren van projecten die uit de Apache Hadoop-ecosysteem voor Azure HDInsight clusters beschikbaar zijn. Deze aanpassing functie is beschikbaar op alle soorten HDInsight, clusters, met inbegrip van Hadoop, HBase en Storm.</td>
<td>Nieuwe functie</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Vooraf gedefinieerde taken voor logboekanalyse Azure websites en opslag</td>
<td>De Query HDInsight Console heeft een galerie aan de slag, die ondersteuning voor oplossingen die werken op uw gegevens of voorbeeldgegevens.
<p>**Oplossingen die op uw gegevens werken**:<br>
Wij hebben de taken voor een aantal van de meest voorkomende gegevens analyse scenario's bieden een uitgangspunt voor het maken van uw eigen oplossingen gemaakt. U kunt uw gegevens met de taak om te zien hoe het werkt. Wanneer u klaar bent, gebruikt u geleerd om een oplossing die gebaseerd is op de vooraf gedefinieerde taak te maken.</p>
<p>**Oplossingen die op voorbeeldgegevens werken**:<br>
Informatie over het werken met HDInsight door stap voor stap enkele basisscenario's (zoals weblogs en sensorgegevens te analyseren). U leert hoe u HDInsight gebruikt om dergelijke gegevens te analyseren en hoe kunt u andere toepassingen en services deze gegevens. Gegevens visualiseren door verbinding te maken met Microsoft Excel, wordt een voorbeeld van de kracht van deze aanpak.</p></td>
<td>Query-console</td>
<td>Hadoop</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Geheugenlek fix in Templeton</td>
<td>Een geheugenlek in Templeton die klanten die langdurige clusters had of indienen van 100s van taak zijn beïnvloed vraagt om dat een tweede is behandeld. Het probleem manifested als Templeton 5xx fouten en de oplossing is de service opnieuw starten. De oplossing is niet meer nodig.</td>
<td>Templeton</td>
<td>Alle</td>
<td>https://Issues.apache.org/jira/Browse/HADOOP-11248</td>
</tr>
</table>
<br>


**Opmerking**: om aan te tonen van de nieuwe mogelijkheden door aanpassing van het cluster beschikbaar gesteld, de actie Script vonk en R-modules te installeren op een cluster met procedures heeft onderbouwd. Zie voor meer informatie:

* [Installeren en gebruiken van Spark 1.0 op clusters HDInsight](hdinsight-hadoop-spark-install.md)
* [Installeren en gebruiken van R op HDInsight Hadoop-clusters](hdinsight-hadoop-r-scripts.md)




## <a name="notes-for-11072014-release-of-hdinsight"></a>Opmerkingen bij de release van HDInsight 11/07/2014

De volledige versie nummers voor HDInsight clusters die in deze release zijn geïmplementeerd, zijn:

* 2.1 HDInsight 2.1.9.374.1153876
* HDInsight 3.0 3.0.5.374.1153876
* 3.1 HDInsight 3.1.1.374.1153876

Deze versie bevat de volgende updates van onderdelen.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschrijving</th>
<th>Onderdeel</th>
<th>Clustertype</th>
<th>JIRA (indien van toepassing)</th>
</tr>

<tr>
<td>2.1.7 HDP</td>
<td>Deze versie is gebaseerd op Hortonworks Data Platform (HDP) 2.1.7. Zie de <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">Release-opmerkingen voor HDP 2.1.7</a>voor meer informatie.</td>
<td>HDP</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>GARENS tijdlijn Server</td>
<td>De garens tijdlijn Server (ook wel bekend als de algemene geschiedenis toepassingsserver) is standaard ingeschakeld. De tijdlijn Server biedt algemene informatie over voltooide toepassingen (zoals toepassings-ID, naam van de toepassing Sollicitatiestatus, toepassing benodigde tijd en voltooiingstijd van toepassing).

Deze toepassingsinformatie uit het hoofd knooppunt kan worden opgehaald via de URI http://headnodehost:8188 of door de opdracht GAREN: garen toepassing – appStates – lijst alle.

Deze informatie kan ook worden opgehaald op afstand al een REST API op https://{ClusterDnsName}. azurehdinsight.NET/ws/V1/applicationhistory/.

Zie voor meer gedetailleerde informatie <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">GAREN tijdlijn Server</a>.</td>
<td>Service, GAREN</td>
<td>Hadoop, HBase</td>
<td>N.V.T.</td>
</tr>

<tr>
<td>Cluster distributie-ID</td>
<td>Vanaf versie 1.3.3.1.5426.29232 SDK kunnen gebruikers toegang krijgen tot een unieke ID voor elk cluster dat is afgegeven HDInsight. Hierdoor kan klanten unieke exemplaren van clusters bepalen wanneer een DNS-naam is opnieuw wordt gebruikt meerdere maken of verbreken van scenario's.</td>
<td>SDK</td>
<td>Alle</td>
<td>N.V.T.</td>
</tr>
</table>
<br>

**Opmerking**: de bug waardoor het volledige versienummer weergegeven in de portal of wordt geretourneerd met de SDK of met Windows PowerShell is opgelost in deze release.

## <a name="notes-for-10152014-release"></a>Notities voor vrijgave 15-10/2014

Deze hotfix release opgelost een geheugenlek in Templeton die de zware gebruikers van Templeton beïnvloed. In sommige gevallen ziet de gebruikers die sterk Templeton uitgeoefend fouten als 500 foutcodes manifested omdat de aanvragen zou niet genoeg geheugen om te worden uitgevoerd. De oplossing voor dit probleem is de Templeton-service opnieuw starten. Dit probleem is opgelost.


## <a name="notes-for-1072014-release"></a>Opmerkingen bij versie 7-10/2014

* Bij het gebruik van eindpunt Ambari, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" retourneert het veld *hostnaam* de FQDN-naam (Fully Qualified Domain Name) van het knooppunt in plaats van de naam van de host. Bijvoorbeeld, in plaats van "**headnode0**", dat u de FQDN-naam '**headnode0. { ClusterDNS} .azurehdinsight .net**". Deze wijziging is vereist voor scenario's waarbij meerdere clustertypen (zoals HBase en Hadoop) kunnen worden geïmplementeerd in een virtueel netwerk te vergemakkelijken. Dit gebeurt, bijvoorbeeld wanneer u HBase als een back-end-platform voor Hadoop.

* We hebben nieuwe instellingen voor het geheugen voor de standaard implementatie van het cluster HDInsight verstrekt. Vorige standaardinstellingen geheugen geen afdoende rekening gehouden met de richtsnoeren voor het aantal processor-cores wordt ingezet. Deze nieuwe instellingen voor het geheugen moeten bieden betere standaardinstellingen (volgens de aanbevelingen van de Hortonworks). Raadpleeg de SDK-documentatie over het wijzigen van de configuratie van het cluster als u wilt deze wijzigen. De nieuwe instellingen van het geheugen die worden gebruikt door het standaardcluster 4 CPU core (8 container) HDInsight zijn gespecificeerd in de volgende tabel. (De waarden die worden gebruikt voor deze release worden ook geleverd getallen.)

<table border="1">
<tr><th>Onderdeel</th><th>De toewijzing van geheugen</th></tr>
<tr><td> yarn.scheduler.minimum toewijzing</td><td>768 MB (eerder 512 MB)</td></tr>
<tr><td> yarn.scheduler.maximum toewijzing</td><td>6144 MB (ongewijzigd)</td></tr>
<tr><td>yarn.nodemanager.resource.Memory</td><td>6144 MB (ongewijzigd)</td></tr>
<tr><td>mapreduce.map.Memory</td><td>768 MB (eerder 512 MB)</td></tr>
<tr><td>mapreduce.map.Java.opts</td><td>kiest u = Xmx512m (voorheen - Xmx410m)</td></tr>
<tr><td>mapreduce.reduce.Memory</td><td>1536 MB (eerder 1024 MB)</td></tr>
<tr><td>mapreduce.reduce.Java.opts</td><td>kiest u = Xmx1024m (voorheen - Xmx819m)</td></tr>
<tr><td>yarn.app.mapreduce.AM.resource</td><td>768 MB (eerder 1024 MB)</td></tr>
<tr><td>yarn.app.mapreduce.AM.Command</td><td>kiest u = Xmx512m (voorheen - Xmx819m)</td></tr>
<tr><td>mapreduce.Task.IO.Sort</td><td>256 MB (eerder 200 MB)</td></tr>
<tr><td>tez.AM.resource.Memory</td><td>1536 MB (ongewijzigd)</td></tr>

</table><br>

Zie voor meer informatie over de configuratie-instellingen van het geheugen wordt gebruikt door garens en MapReduce op het Platform voor Hortonworks die wordt gebruikt door HDInsight [HDP geheugen configuratie-instellingen bepalen](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html). Hortonworks ook beschikbaar een hulpmiddel voor het berekenen van de instellingen voor het juiste geheugen.

Met betrekking tot de Azure PowerShell en de HDInsight SDK-foutbericht: ' het*Cluster niet is geconfigureerd voor HTTP-services toegang*':

* Deze fout is een bekend [probleem](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) dat als gevolg van een verschil in de versie van de HDInsight SDK of Azure PowerShell en de versie van het cluster optreden kan. Clusters gemaakt op 15-8 of hoger hebben ondersteuning voor de nieuwe functie voor de inrichting in virtuele netwerken. Maar deze mogelijkheid wordt niet juist geïnterpreteerd door oudere versies van de HDInsight SDK of Azure PowerShell. Het resultaat is een storing in sommige bewerkingen project indienen. Als u HDInsight SDK API's of Azure PowerShell cmdlets (**Gebruik AzureRmHDInsightCluster** of **Invoke AzureRmHDInsightHiveJob**) taken, bewerkingen mislukken met het foutbericht '*Cluster <clustername> is niet geconfigureerd voor HTTP-services toegang*. " Of (afhankelijk van de bewerking), krijgt u mogelijk andere foutberichten worden weergegeven, zoals '*kan geen verbinding met het cluster*'.

* Deze compatibiliteitsproblemen worden opgelost in de nieuwste versies van de HDInsight SDK en Azure PowerShell. Het is raadzaam de HDInsight SDK update naar versie 1.3.1.6 of hoger en Azure PowerShell extra naar versie 0.8.8 of hoger. U kunt toegang kan krijgen tot de nieuwste SDK HDInsight van [](http://nuget.codeplex.com/wikipage?title=Getting%20Started) en de Azure PowerShell hulpmiddelen bij [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).



## <a name="notes-for-9122014-release-of-hdinsight-31"></a>Notities voor 9/12/2014 versie 3.1 HDInsight

* Deze versie is gebaseerd op Hortonworks Data Platform (HDP) 2.1.5. Zie de pagina [vast in deze versie](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) op de site Hortonworks voor een overzicht van de fouten die verholpen zijn in deze release.
* In de map bibliotheken van varkens, is het bestand 'avro mapred 1.7.4.jar' gewijzigd in "avro-mapred-1.7.4-hadoop2.jar." De inhoud van dit bestand bevat een kleine correctie bug is niet verbreken. Het is raadzaam dat klanten niet direct afhankelijk van de naam van het bestand JAR maken-einden voorkomen wanneer bestanden worden gewijzigd.


## <a name="notes-for-8212014-release"></a>Notities voor de release van 8/21/2014

* Wij zijn de volgende WebHCat-configuratie (Component-7155), waarin de standaard geheugenlimiet voor een project Templeton-controller met 1 GB toevoegen. (De vorige standaardwaarde is 512 MB).

     templeton.Mapper.Memory.MB (= 1024)

    * Deze wijziging wordt de volgende fout die bepaalde component query was uitgevoerd door lagere geheugenlimieten: 'De Container wordt uitgevoerd buiten de grenzen van de fysieke geheugen'.
    * Als u wilt terugkeren naar de standaardinstellingen van de oude, kunt u deze configuratiewaarde instelt op 512 via Azure PowerShell bij het cluster maken met behulp van de volgende opdracht:

        Toevoegen-AzureRmHDInsightConfigValues-Core@{"templeton.mapper.memory.mb"="512";}


* De hostnaam van de rol van zookeeper is *zookeeper*gewijzigd. Dit heeft gevolgen voor naamomzetting binnen het cluster, maar dit heeft geen invloed op externe REST API's. Als u onderdelen die gebruikmaken van de host-naam *zookeepernode* hebt, moet u bijwerken om de nieuwe naam gebruikt. De nieuwe namen voor de drie zookeeper knooppunten zijn:
    * zookeeper0
    * zookeeper1
    * zookeeper2
* HBase versie ondersteuningsmatrix wordt bijgewerkt. Alleen HDInsight versie 3.1 (0,98 versie HBase) wordt voor de productie van HBase werkbelasting ondersteund. Versie 3.0 (die was beschikbaar voor preview) wordt niet ondersteund verplaatsen naar voren.

## <a name="notes-about-clusters-created-prior-to-8152014"></a>Opmerkingen over clusters gemaakt voor 8/15/2014

Een foutbericht Azure PowerShell of HDInsight SDK ' Cluster <clustername> is niet geconfigureerd voor HTTP-services toegang "(of afhankelijk van de bewerking, andere foutberichten, zoals: 'Kan geen verbinding met cluster') kan optreden als gevolg van een versie verschil tussen Azure PowerShell of de HDInsight SDK en een cluster. Clusters gemaakt op 15-8 of hoger hebben ondersteuning voor de nieuwe functie voor de inrichting in virtuele netwerken. Deze mogelijkheid is niet goed geïnterpreteerd door oudere versies van de Azure PowerShell of de HDInsight SDK, wat in mislukte taak indiening bewerkingen resulteert. Als u taken HDInsight SDK API's of Azure PowerShell cmdlets (zoals gebruik AzureRmHDInsightCluster of Invoke-AzureRmHDInsightHiveJob), worden deze bewerkingen kunnen mislukken met een van de foutberichten die worden beschreven.

Deze compatibiliteitsproblemen worden opgelost in de nieuwste versies van de HDInsight SDK en Azure PowerShell. Het is raadzaam de HDInsight SDK update naar versie 1.3.1.6 of hoger en Azure PowerShell extra naar versie 0.8.8 of hoger. U krijgt toegang tot de meest recente HDInsight SDK van [NuGet][nuget-link]. U kunt de Azure PowerShell's openen met [Microsoft Web Platform Installer][webpi-link].


## <a name="notes-for-7282014-release"></a>Opmerkingen bij versie 7/28/2014

* **HDInsight beschikbaar in nieuwe regio's**: We HDInsight geografische aanwezigheid uitgebreid naar drie regio's. HDInsight klanten kunnen clusters maken in deze regio's:
    * Oost-Azië
    * Centrale Noord-Amerikaanse
    * Zuid-centraal-Verenigde Staten
* Versie 1.6 (HDP 1.1 en Hadoop 1.0.3) HDInsight en HDInsight versie 2.1 (HDP1.3 en Hadoop 1.2) worden verwijderd uit de Azure portal. U kunt blijven Hadoop clusters voor deze versies maken door met de Azure PowerShell-cmdlet, [Nieuw-AzureRmHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx) of via de [HDInsight SDK](http://msdn.microsoft.com/library/azure/dn469975.aspx). Raadpleeg de pagina [HDInsight component versiebeheer](hdinsight-component-versioning.md) voor meer informatie.
* Hortonworks Data Platform (HDP) wijzigingen in deze release:

<table border="1">
<tr><th>HDP</th><th>Wijzigingen</th></tr>
<tr><td>1.3 HDP / 2.1 HDI</td><td>Geen wijzigingen</td></tr>
<tr><td>HDP 2.0 / 3.0 HDI</td><td>Geen wijzigingen</td></tr>
<tr><td>2.1 HDP / 3.1 HDI</td><td>zookeeper: [3.4.5.2.1.3.0-1948] [3.4.5.2.1.3.2-0002] -></td></tr>


</table><br>

## <a name="notes-for-6242014-release"></a>Notities voor 6/24/2014

Deze versie bevat verbeteringen in de HDInsight-service:

* **2.1 HDP beschikbaarheid**: HDInsight 3.1 (met daarin de HDP 2.1) is in het algemeen beschikbaar en is de standaardversie voor nieuwe clusters.
* **HBase – Azure portal verbeteringen**: maken We HBase clusters beschikbaar in Preview. U kunt HBase clusters maken vanaf de portal met slechts enkele muisklikken. 

Met HBase, kunt u tal van real-time werkbelasting op HDInsight, bouwen van interactieve websites die werken met grote datasets Services opslaan sensor en telemetrie van miljoenen eindpunten. De volgende stap zou zijn voor het analyseren van de gegevens in deze werkbelasting met Hadoop projecten, en dit is mogelijk in HDInsight via Azure PowerShell en het onderdeel cluster-dashboard.

### <a name="apache-mahout-preinstalled-on-hdinsight-31"></a>Apache Mahout vooraf geïnstalleerd op 3.1 HDInsight

 [Mahout](http://hortonworks.com/hadoop/mahout/) is vooraf geïnstalleerd op de HDInsight 3.1 Hadoop clusters, zodat u taken zonder dat extra clusterconfiguratie Mahout kunt uitvoeren. Bijvoorbeeld, kunt u extern een Hadoop-cluster met behulp van Remote Desktop Protocol (RDP) en zonder aanvullende stappen kunt u de volgende Hello World Mahout opdracht uitvoeren:

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L  

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

Zie de documentatie bij het [Voorbeeld van de Breiman](https://mahout.apache.org/users/classification/breiman-example.html) op de website van Apache Mahout voor een uitgebreidere uitleg van deze procedure.


### <a name="hive-queries-can-use-tez-in-hdinsight-31"></a>Query's component kunnen Tez gebruiken in 3.1 HDInsight

Component 0,13 is beschikbaar in HDInsight 3.1 en is het werken met query's met behulp van Tez, die kan worden gebruikt voor aanzienlijke prestatieverbeteringen.
Tez is niet inschakelen voor component-query's. Als u wilt gebruiken, moet u opt-in. U kunt Tez inschakelen door het volgende stukje code uit te voeren:

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

Hortonworks is een gedetailleerde uitsplitsing van de component query prestatieverbeteringen met Tez gepubliceerd die worden geleverd in standaard-benchmarks. Zie voor meer informatie, [Apache Benchmarking component 13 voor Enterprise Hadoop](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/).

Zie voor meer informatie over het gebruik van de component met Tez de [component op Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez).

###<a name="global-availability"></a>Globale beschikbaarheid
Met de release van HDInsight op Hadoop 2.2, heeft Microsoft HDInsight beschikbaar gesteld in alle belangrijke regio's waar de Azure beschikbaar is. Met name hebben de west-Europa en Zuidoost-Azië datacenters on line is gebracht. Hierdoor kan klanten in een datacenter dat dicht en mogelijk in een zone van soortgelijke conformiteitsvereisten clusters gevonden.


###<a name="dos--donts-between-cluster-versions"></a>Taken & van tussen de versies van de cluster

**Oozie metastores gebruikt in combinatie met een cluster HDInsight 3.1 zijn niet achterwaarts compatibel met clusters HDInsight 2.1, en ze kunnen niet worden gebruikt met deze vorige versie**.

Een aangepaste Oozie metastore database geïmplementeerd met een cluster HDInsight 3.1 worden niet met een HDInsight-2.1-cluster gebruikt. Dit is het geval, zelfs als de metastore afkomstig met een cluster HDInsight 2.1 is. In dit scenario wordt niet ondersteund omdat het metastore-schema wordt bijgewerkt zodat het niet langer compatibel met de voorgeschreven HDInsight 2.1 clusters metastore is in combinatie met een cluster 3.1 HDInsight. Elke poging om een metastore Oozie die is gebruikt met een HDInsight 3.1 cluster opnieuw te gebruiken krijgt u het cluster HDInsight 2.1 onbruikbaar.

**Oozie metastores kan niet worden gedeeld met clusters.**

Oozie metastores zijn gekoppeld aan specifieke clusters en ze kunnen niet worden gedeeld met clusters.

###<a name="breaking-changes"></a>Overtredingen van de wijzigingen

**Voorvoegsel syntaxis**: alleen de "wasbs: / / ' syntaxis wordt ondersteund in HDInsight 3.1 en 3.0 clusters. De oudere "luchtverzadigingswaarde: / / ' syntaxis wordt ondersteund in HDInsight 2.1 en 1,6 clusters, maar wordt niet ondersteund in HDInsight 3.1 of 3.0 clusters. Daarbij wordt ingediend bij een HDInsight 3.1 of 3.0 cluster taken die expliciet de "luchtverzadigingswaarde: / / ' syntaxis niet worden uitgevoerd. De ' wasbs: / / ' syntaxis in plaats daarvan moet worden gebruikt. Ook projecten ingediend bij een HDInsight 3.1 of 3.0 clusters die zijn gemaakt met een bestaande metastore met expliciete verwijzingen naar bronnen met behulp van de "luchtverzadigingswaarde: / / ' syntaxis niet worden uitgevoerd. Deze metastores moeten opnieuw worden gemaakt met behulp van de ' wasbs: / / "syntaxis voor het adres.


**Poorten**: de poorten die worden gebruikt door de service HDInsight hebt gewijzigd. De poortnummers die werden gebruikt zijn in het tijdelijke poortbereik van het Windows-besturingssysteem. Poorten worden automatisch toegewezen uit een bereik van vooraf gedefinieerde tijdelijke voor tijdelijk Internet protocol-communicatie. De nieuwe reeks toegestane Hortonworks Data Platform (HDP) service-poortnummers vallen buiten dit bereik om te voorkomen dat er conflicten die kunnen optreden met de poorten die worden gebruikt door services die worden uitgevoerd op het knooppunt head. De nieuwe poortnummers zou geen recente wijzigingen veroorzaken. De nummers zijn als volgt:

 **HDInsight 1.6 (HDP 1.1)**
<table border="1">
<tr><th>Naam</th><th>Waarde</th></tr>
<tr><td>DFS.http.Address</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.Secondary.http.Address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.job.Tracker.http.Address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.Task.Tracker.http.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>mapreduce.history.server.http.Address</td><td>0.0.0.0:31111</td></tr>
<tr><td>templeton.Port</td><td>30111</td></tr>
</table><br>

 **HDInsight 3.1 en 3.0 (HDP 2.1 en 2.0)**
<table border="1">
<tr><th>Naam</th><th>Waarde</th></tr>
<tr><td>DFS.namenode.HTTP adres</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.namenode.https adres</td><td>headnodehost:30470</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.namenode.Secondary.HTTP adres</td><td>0.0.0.0:30090</td></tr>
<tr><td>yarn.nodemanager.WebApp.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>templeton.Port</td><td>30111</td></tr>
</table><br>

###<a name="dependencies"></a>Afhankelijkheden

De volgende afhankelijkheden zijn toegevoegd in HDInsight 3.x (HDP2.x):

* guice servlet
* optiq-core
* javax.inject
* activering
* jsr305
* geronimo-jaspic_1.0_spec
* jul-slf4j-
* Java-xmlbuilder
* ant
* Commons-compiler
* jdo-api
* Commons math3
* paranamer
* jaxb-impl
* stringtemplate
* eigenbase-xom
* Jersey servlet
* Commons-exec
* jaxb-api
* jetty-all-server
* janino
* xercesImpl
* optiq avatica
* jta
* eigenbase-eigenschappen
* groovy-alle
* hamcrest-core
* e-mail
* linq4j
* jpam
* Jersey-client
* aopalliance
* geronimo-annotation_1.0_spec
* ant starten
* Jersey guice
* XML-API 's
* stax-api
* Asm-commons
* Asm-structuur
* wadl
* geronimo-jta_1.1_spec
* guice
* leveldbjni-alle
* Velocity
* jettison
* treffende java
* jetty-alle
* Commons-dbcp

De volgende afhankelijkheden bestaat niet meer in HDInsight 3.x (HDP2.x):

* jdeb
* kfs
* sqlline
* klimop
* aspectjrt
* JSON
* Core
* jdo2-api
* avro-mapred
* datanucleus-enhancer
* JSP
* Commons-registratie-api
* Commons-math
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* hbase
* treffende

###<a name="version-changes"></a>Wijzigingen in versie

De volgende versiewijzigingen zijn aangebracht tussen HDInsight 2.x (HDP1.x) en HDInsight 3.x (HDP2.x):

* metrics core: [2.1.2]-[3.0.0] >
* derbynet: [10.4.2.0] [10.10.1.1] ->
* datanucleus: ['rdbms-3.0.8'] -> ['rdbms-3.2.9']
* Jasper-compiler: [5.5.12] [5.5.23] ->
* log4j: ['en met 1.2.15', '1.2.16'] -> ['1.2.16', '1.2.17']
* derbyclient: [10.4.2.0] [10.10.1.1] ->
* httpcore: [4.2.4]-[4.2.5] >
* hsqldb: [1.8.0.10]-[2.0.0] >
* jets3t: [0.6.1]-[0.9.0] >
* java-protobuf: [2.4.1]-[2.5.0] >
* derby: [10.4.2.0] [10.10.1.1] ->
* Jasper: ['runtime-5.5.12'] -> ['runtime-5.5.23']
* Commons-daemon: [1.0.1]-[1.0.13] >
* datanucleus-core: [3.0.9]-[3.2.10] >
* datanucleus-api-jdo: [3.0.7]-[3.2.6] >
* zookeeper: [3.4.5.1.3.9.0-01320] [3.4.5.2.1.3.0-1948] ->
* bonecp: [0.7.1.RELEASE] -> ['
* 0.8.0.RELEASE "]


### <a name="drivers"></a>Stuurprogramma 's
De Java Database Connnectivity (JDBC)-stuurprogramma voor SQL Server wordt intern gebruikt door de HDInsight en wordt niet gebruikt voor externe bewerkingen. Als u wilt verbinding maken met de HDInsight via Open Database Connectivity (ODBC), gebruikt u de component ODBC-stuurprogramma. Zie voor meer informatie [HDInsight met component ODBC-stuurprogramma voor de Microsoft Excel verbinding maakt](hdinsight-connect-excel-hive-odbc-driver.md).


### <a name="bug-fixes"></a>Programmafouten

Met deze release hebben we de volgende versies van HDInsight met verschillende bugfixes vernieuwd:

* 2.1 (HDP 1.3) HDInsight
* HDInsight 3.0 (HDP 2.0)
* HDInsight 3.1 (HDP-2.1)


## <a name="hortonworks-release-notes"></a>Hortonworks release-opmerkingen

Release-opmerkingen voor de Hortonworks gegevens Platforms (HDPs) die worden gebruikt door de HDInsight versie clusters zijn beschikbaar op de volgende locaties:

* HDInsight versie 3.1 maakt gebruik van een Hadoop verdeling op basis van het [Platform voor Hortonworks 2.1.7][hdp-2-1-7]. Dit is het standaard Hadoop cluster gemaakt met de Azure portal na 7-11/2014. 3.1 HDInsight clusters gemaakt voordat de 7-11/2014 waren gebaseerd op het [Platform voor Hortonworks 2.1.1][hdp-2-1-1]

* HDInsight versie 3.0 gebruikt een Hadoop verdeling op basis van de [Hortonworks Data Platform 2.0][hdp-2-0-8].

* HDInsight versie 2.1 gebruikt een Hadoop verdeling op basis van de [Hortonworks Data Platform 1.3][hdp-1-3-0].

* HDInsight versie 1.6 gebruikt een Hadoop verdeling op basis van de [Hortonworks Data Platform 1.1][hdp-1-1-0].

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
 

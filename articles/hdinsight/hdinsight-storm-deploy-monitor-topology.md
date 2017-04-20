<properties
   pageTitle="Implementeren en beheren van Apache Storm topologieën op HDInsight | Microsoft Azure"
   description="Informatie over het implementeren, bewaken en beheren van Apache Storm topologieën met het Dashboard Storm op HDInsight. Hadoop-hulpprogramma's gebruiken voor Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a>Implementeren en beheren van Apache Storm topologieën op Windows gebaseerde HDInsight

De Storm Dashboard kunt u eenvoudig implementeren en uitvoeren van Apache Storm topologieën aan uw cluster HDInsight via uw webbrowser. U kunt ook het dashboard te bewaken en beheren van topologieën uitgevoerd. Als u Visual Studio, bieden de HDInsight's voor Visual Studio vergelijkbare functies in Visual Studio.

Het Dashboard Storm en de Storm-functies van de HDInsight's, is afhankelijk van de Storm REST API die kan worden gebruikt voor het maken van uw eigen controle- en beheeroplossingen.

> [AZURE.IMPORTANT] De stappen in dit document moet een op Windows gebaseerde Storm op HDInsight cluster. Zie voor meer informatie over het gebruik van een Linux-gebaseerde cluster [implementeren en beheren van Apache Storm topologieën op Linux-gebaseerde HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

##<a name="prerequisites"></a>Vereisten

* **Storm op HDInsight Apache** - Zie <a href="../hdinsight-storm-getting-started/" target="_blank">aan de slag met Apache Storm op HDInsight</a> voor stapsgewijze instructies voor het maken van een cluster

* Voor de **Storm-Dashboard**: een moderne webbrowser die HTML5 ondersteunt

* Voor **Visual Studio** - Azure SDK 2.5.1 of nieuwer en de HDInsight's voor Visual Studio. Zie <a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">aan de slag met de HDInsight's voor Visual Studio</a> te installeren en configureren van de HDInsight's voor Visual Studio.

    Een van de volgende versies van Visual Studio:

    * Visual Studio 2012 met <a href="http://www.microsoft.com/download/details.aspx?id=39305" target="_blank">Update 4</a>

    * Visual Studio 2013 met <a href="http://www.microsoft.com/download/details.aspx?id=44921" target="_blank">Update 4</a> of <a href="http://go.microsoft.com/fwlink/?LinkId=517284" target="_blank">Visual Studio 2013 Gemeenschap</a>

    * <a href="http://visualstudio.com/downloads/visual-studio-2015-ctp-vs" target="_blank">Visual Studio 2015 CTP6</a>

    > [AZURE.NOTE] Op dit moment ondersteuning de HDInsight's voor Visual Studio alleen voor Storm op HDInsight cluster versie 3.2.

##<a name="storm-dashboard"></a>Storm-Dashboard

Het Dashboard Storm is een webpagina beschikbaar voor de Storm-cluster. De URL is **https://&lt;Clusternaam >.azurehdinsight.net/**, waarbij **clusternaam** de naam van de Storm op de cluster HDInsight.

Selecteren vanaf de bovenzijde van het Dashboard Storm **Topologie indienen**. Volg de instructies op de pagina voor het uitvoeren van een topologie van het monster of voor het uploaden en uitvoeren van een topologie die u hebt gemaakt.

![de topologie submit pagina][storm-dashboard-submit]

###<a name="storm-ui"></a>Storm UI

Selecteer de koppeling **Storm UI** uit het Dashboard Storm. Dit geeft informatie over het cluster, naast alle topologieën uitgevoerd.

![de gebruikersinterface voor de storm][storm-dashboard-ui]

> [AZURE.NOTE] In sommige versies van Internet Explorer mogelijk de Storm-gebruikersinterface wordt niet vernieuwd nadat u het eerst hebt bezocht. Bijvoorbeeld, kan niet worden weergegeven de nieuwe topologieën die u hebt ingediend of een topologie als actief kan worden weergegeven als eerder gedeactiveerd. Microsoft is zich bewust van dit probleem en werkt aan een oplossing.

####<a name="main-page"></a>Hoofdpagina

De hoofdpagina van de Storm-gebruikersinterface bevat de volgende informatie:

* **Cluster samenvatting**: informatie over het cluster Storm.

* **Topologie samenvatting**: een lijst met actieve topologieën. Gebruik de koppelingen in deze sectie voor meer informatie over bepaalde topologieën.

* **Supervisor samenvatting**: informatie over de Storm supervisor.

* **Nimbus configuratie**: Nimbus-configuratie voor het cluster.

####<a name="topology-summary"></a>Topologie-overzicht

Een koppeling in de sectie **Samenvatting topologie** te selecteren, worden de volgende gegevens over de topologie:

* **Topologie overzicht**: basisinformatie over de topologie.

* **Topologie acties**: acties die u voor de topologie uitvoeren kunt.

    * **Activeren**: verwerking van een gedeactiveerde topologie wordt hervat.

    * **Uitschakelen**: een actieve topologie wordt onderbroken.

    * **Vastleggen**: Hiermee past u de evenwijdigheid van de topologie. Nadat u het aantal knooppunten in het cluster hebt gewijzigd, moet u lopende topologieën opnieuw. Hierdoor wordt de topologie aanpassen parallellisme ter compensatie van de verhoogde of verlaagde aantal knooppunten in het cluster.

        Zie <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Wat is de evenwijdigheid van een topologie Storm</a>voor meer informatie.

    * **Kill**: een topologie Storm na de opgegeven time-out beëindigd.

* **Topologie statistieken**: statistieken over de topologie. Gebruik de koppelingen in het **venster** kolom het tijdsbestek voor de overige items op de pagina instellen.

* **Spouts**: de spouts die wordt gebruikt door de topologie. Gebruik de koppelingen in deze sectie voor meer informatie over specifieke spouts.

* **Bolts**: de bouten die wordt gebruikt door de topologie. Gebruik de koppelingen in deze sectie voor meer informatie over specifieke bouten.

* **Configuratie van de topologie**: de configuratie van de geselecteerde topologie.

####<a name="spout-and-bolt-summary"></a>Spout en bout samenvatting

Een spout selecteren in de secties **Spouts** of **Bolts** bevat de volgende informatie over het geselecteerde item:

* **Onderdeel overzicht**: basisinformatie over de spout of bout.

* **Spout/bout statistieken**: statistieken over de spout of bout. Gebruik de koppelingen in het **venster** kolom het tijdsbestek voor de overige items op de pagina instellen.

* **Input-statistieken** (alleen bout): informatie over de invoer streams die worden gebruikt door de bout.

* **Uitvoer stats**: informatie over de streams uitgestoten door deze spout of bout.

* **Executors**: informatie over de exemplaren van de spout of bout. Selecteer de post **poort** voor een specifieke executor een logboek van diagnostische gegevens voor dit exemplaar geproduceerd te bekijken.

* **Fouten**: eventuele fouten voor deze spout of bout.

##<a name="hdinsight-tools-for-visual-studio"></a>Extra HDInsight voor Visual Studio

De HDInsight's kan indienen van C# of hybride topologieën aan de Storm-cluster worden gebruikt. De volgende stappen gebruikt een voorbeeldtoepassing. Zie voor meer informatie over het maken van uw eigen topologieën met behulp van de HDInsight's [ontwikkelen C# topologieën met behulp van de HDInsight's voor Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Gebruik de volgende stappen uit om een monster voor de Storm-cluster HDInsight implementeren en vervolgens weergeven en beheren van de topologie.

1. Als u de meest recente versie van de HDInsight's zijn niet voor Visual Studio hebt geïnstalleerd, ziet u <a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">aan de slag met de HDInsight's voor Visual Studio</a>.

2. Visual Studio openen, selecteert u **bestand** > **Nieuw** > **Project**.

3. Vouw in het dialoogvenster **Nieuw Project** **Installed** > **sjablonen**en selecteer vervolgens **HDInsight**. Selecteer in de lijst met sjablonen **Storm monster**. Onderaan in het dialoogvenster, typ een naam voor de toepassing.

    ![afbeelding](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

1. In de **Solution Explorer**met de rechtermuisknop op het project en selecteer **indienen bij Storm op HDInsight**.

    > [AZURE.NOTE] Voer desgevraagd de inloggegevens voor uw abonnement op Azure. Als er meer dan één abonnement, aanmelden bij de database met de Storm op de cluster HDInsight.

2. Uw Storm op HDInsight cluster selecteren in de vervolgkeuzelijst **Storm Cluster** en selecteer **verzenden**. U kunt controleren of het indienen gelukt is met behulp van het venster **uitvoer** .

3. Wanneer de topologie is ingediend, weergegeven de **Storm topologieën** voor het cluster. De topologie selecteren uit de lijst voor informatie over de lopende topologie.

    ![Visual studio-monitor](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

    > [AZURE.NOTE] U kunt ook **Storm topologieën** van **Server Explorer** weergeven door uit te breiden **Azure** > **HDInsight**, klik met de rechtermuisknop op een Storm op de cluster HDInsight en **Storm topologieën voor weergave**selecteren.

    Selecteer de shape voor de spouts of bouten voor informatie over deze onderdelen. Voor elk geselecteerd item een nieuw venster geopend.
    
    > [AZURE.NOTE] De naam van de topologie is de naam van de klasse van de topologie (in dit geval `HelloWord`,) met een tijdstempel toegevoegd.

4. De **Topologie** overzichtsweergave Selecteer **Kill** te stoppen, de topologie.

    > [AZURE.NOTE] Storm topologieën blijven actief totdat ze worden gestopt of het cluster wordt verwijderd.

##<a name="rest-api"></a>REST-API

De gebruikersinterface voor de Storm is gebouwd op de REST API, zodat u kunt soortgelijke beheer en bewaking van functionaliteit met behulp van de REST API. De REST-API kunt u aangepaste hulpprogramma's voor het beheren en controleren van Storm topologieën maken.

Voor meer informatie Zie [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md). De volgende informatie is specifiek voor de REST API gebruiken met Apache Storm op HDInsight.

###<a name="base-uri"></a>Base URI

Is de basis-URI voor de REST API op clusters HDInsight **https://&lt;Clusternaam >.azurehdinsight.net/stormui/api/v1/**, waarbij **clusternaam** de naam van de Storm op de cluster HDInsight.

###<a name="authentication"></a>Verificatie

Aanvragen voor de REST API moeten **Basisverificatie**gebruiken zodat u de naam van HDInsight cluster administrator en het wachtwoord gebruiken.

> [AZURE.NOTE] Omdat basisverificatie via leesbare tekst worden verzonden, moet u **altijd** gebruik HTTPS voor het beveiligen van communicatie met het cluster.

###<a name="return-values"></a>Retourwaarden

Informatie die wordt geretourneerd door de REST API mogelijk bruikbaar uit binnen het cluster of virtuele machines in dezelfde Azure virtuele netwerk als het cluster. Bijvoorbeeld de FQDN-naam (Fully Qualified Domain Name) geretourneerd voor Zookeeper-servers niet toegankelijk zijn via Internet.

##<a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe te implementeren en topologieën controleren met behulp van het Dashboard Storm, informatie over hoe u kunt:

* [Ontwikkelen van C# topologieën met behulp van de HDInsight's voor Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [Ontwikkelen van Java gebaseerde topologieën met Maven](hdinsight-storm-develop-java-topology.md)

Zie voor een lijst van meer voorbeeld topologieën [voorbeeld topologieën voor Storm op HDInsight](hdinsight-storm-example-topology.md).

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png

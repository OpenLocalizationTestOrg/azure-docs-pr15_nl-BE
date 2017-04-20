<properties
   pageTitle="Context opties voor R Server op HDInsight (voorbeeld) berekenen | Microsoft Azure"
   description="Meer informatie over de verschillende compute context opties beschikbaar voor gebruikers met R-Server op HDInsight (voorbeeld)"
   services="HDInsight"
   documentationCenter=""
   authors="jeffstokes72"
   manager="jhubbard"
   editor="cgronlun"
/>

<tags
   ms.service="HDInsight"
   ms.devlang="R"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-services"
   ms.date="10/18/2016"
   ms.author="jeffstok"
/>

# <a name="compute-context-options-for-r-server-on-hdinsight-preview"></a>Berekenen context opties voor R Server op HDInsight (voorbeeld)

Microsoft R Server op Azure HDInsight (voorbeeld) biedt de nieuwste mogelijkheden voor analytics R is gebaseerd. Gegevens die zijn opgeslagen in HDFS in een container in uw account [Azure Blob](../storage/storage-introduction.md "Azure Blob-opslag") opslag of het lokale bestandssysteem voor Linux wordt gebruikt. Omdat R-Server is gebaseerd op open source R, kunnen de R-gebaseerde toepassingen die u bouwt gebruikmaken van een van de 8000 + R voor open source pakketten. Ze kunnen ook gebruikmaken van de routines in [ScaleR](http://www.revolutionanalytics.com/revolution-r-enterprise-scaler "Revolutie Analytics ScaleR"), van big data analytics pakket van Microsoft die is opgenomen met R-Server.  

De randknooppunt van een cluster Premium biedt een handige locatie om verbinding te maken met het cluster en de R-scripts uitvoeren. Met een randknooppunt hebt u de optie van de ScaleR parallelized gedistribueerde functies uitvoert via de kernen van de rand knooppunt server. U hebt ook de mogelijkheid om ze uit te voeren op de knooppunten van het cluster met behulp van ScaleR Hadoop kaart verminderen of Spark contexten berekenen.

## <a name="compute-contexts-for-an-edge-node"></a>Contexten voor een randknooppunt berekenen

Een R-script dat wordt uitgevoerd in de R-Server op het randknooppunt wordt in het algemeen binnen de R-interpreter uitgevoerd op dat knooppunt. De uitzondering is de stappen die een ScaleR-functie aanroept. De ScaleR-aanroepen uitvoert in een compute-omgeving die wordt bepaald door de instelling van de compute ScaleR context.  Wanneer u uw script R vanaf een randknooppunt uitvoert, de mogelijke waarden van de context compute lokale sequentiële ('local'), zijn lokale parallelle (localpar), tekens beperken en Spark.

De 'local' en 'localpar'-opties verschillen alleen met hoe rxExec-oproepen worden uitgevoerd. Beide uitvoeren andere rx-functieaanroepen parallelle wijze over alle beschikbare cores, tenzij anders opgegeven door middel van de ScaleR numCoresToUse optie, bijvoorbeeld rxOptions(numCoresToUse=6). Het volgende geeft een overzicht van de verschillende opties van de compute-context

| Context berekenen  | Het instellen van                      | Uitvoeringscontext                                                                     |
|------------------|---------------------------------|---------------------------------------------------------------------------------------|
| Lokale sequentiële | rxSetComputeContext('local')    | Parallelized worden uitgevoerd tussen de kernen van de server van het knooppunt rand, met uitzondering van rxExec wordt aangeroepen die serie worden uitgevoerd |
| Lokale parallelle   | rxSetComputeContext('localpar') | Parallelized worden uitgevoerd tussen de kernen van de rand knooppunt server                                 |
| Spark            | RxSpark()                       | Gedistribueerde worden uitgevoerd via een op de knooppunten van het cluster HDI parallelized      |
| Kaart verkleinen       | RxHadoopMR()                    | Gedistribueerde worden uitgevoerd via het verminderen van de kaart op de knooppunten van het cluster HDI parallelized |


Ervan uitgaande dat u parallelized worden uitgevoerd ten behoeve van prestaties wilt, vervolgens zijn er drie opties. Welke optie u kiest, hangt af van de aard van uw werk analytics en de grootte en de locatie van uw gegevens.

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Richtlijnen voor het kiezen van een compute-context

Er is momenteel geen formule waarin wordt aangegeven welke context gebruik berekenen. Er zijn echter enkele leidende beginselen die u de juiste keuze te maken helpen kunnen, of ten minste helpen u uw keuzes beperken voordat u een benchmark. Deze richtsnoeren omvatten:

1.  Het lokale bestandssysteem van Linux is sneller dan HDFS.
2.  Herhaalde analyses zijn sneller als de gegevens lokaal en als het XDF.
3.  Het verdient de voorkeur voor het stroomsgewijs verzenden van kleine hoeveelheden gegevens uit een tekstgegevensbron; Als de hoeveelheid gegevens groter is, converteren naar XDF voordat de analyse.
4.  De overhead van het kopiëren of de gegevens naar het randknooppunt voor analyse streaming wordt voor zeer grote hoeveelheden gegevens.
5.  Spark is sneller dan de toewijzing voor analyse in Hadoop verminderen.

Gezien deze beginselen, zijn enkele algemene vuistregels voor het selecteren van een compute-context:

### <a name="local"></a>Lokale

- Als het bedrag van de te analyseren gegevens klein is en niet voor herhaalde analyse vereist is, stream-rechtstreeks in de analyse van routine en 'local' of 'localpar' gebruiken.
- Als het bedrag van de te analyseren gegevens kleine of middelgrote ondernemingen en herhaalde analyse vereist, vervolgens kopiëren naar het lokale bestandssysteem op XDF importeren en analyseren via 'local' of 'localpar'.

### <a name="hadoop-spark"></a>Hadoop Spark

- Als het bedrag van de te analyseren gegevens groot is, vervolgens importeren in XDF in HDFS (tenzij de opslag is een probleem), en deze analyseren via 'Spark'.

### <a name="hadoop-map-reduce"></a>Hadoop Map beperken

- Alleen gebruiken als u een onoverkomelijke probleem met het gebruik van de context van de compute Spark optreden omdat in het algemeen langzamer worden.  

## <a name="inline-help-on-rxsetcomputecontext"></a>In line Help-informatie over rxSetComputeContext

Zie voor meer informatie en voorbeelden van ScaleR compute contexten de inline in R help op de rxSetComputeContext-methode, bijvoorbeeld:

    > ?rxSetComputeContext

U kunt ook verwijzen naar de 'ScaleR Distributed Computing Guide' die is beschikbaar op de(https://msdn.microsoft.com/library/mt674634.aspx "Server op MSDN R") [R Server MSDN]library.


## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe een nieuwe HDInsight cluster met R-Server maken. U hebt ook geleerd over de basisbeginselen van het gebruik van de console R met een SSH-sessie. Nu kunt u lezen in de volgende artikelen om te ontdekken van andere manieren van het werken met R-Server op HDInsight:

- [Overzicht van Server voor Hadoop R](hdinsight-hadoop-r-server-overview.md)
- [Aan de slag met R-server voor Hadoop](hdinsight-hadoop-r-server-get-started.md)
- [RStudio-Server toevoegen aan de HDInsight premie](hdinsight-hadoop-r-server-install-r-studio.md)
- [Azure opslagopties R Server op HDInsight Premium](hdinsight-hadoop-r-server-storage.md)

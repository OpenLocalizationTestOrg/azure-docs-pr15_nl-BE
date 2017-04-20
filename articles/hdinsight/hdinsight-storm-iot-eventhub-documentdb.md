<properties
 pageTitle="Verwerken van gegevens over het voertuig sensor met Apache Storm op HDInsight | Microsoft Azure"
 description="Informatie over het verwerken van gegevens over het voertuig sensor van gebeurtenis Hubs met Apache Storm op HDInsight. Modelgegevens uit DocumentDB toevoegen en opslaan van de uitvoer naar de opslag."
 services="hdinsight,documentdb,notification-hubs"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Verwerken van gegevens over het voertuig sensor van Azure gebeurtenis Hubs met Apache Storm op HDInsight

Informatie over het verwerken van gegevens over het voertuig sensor van Azure gebeurtenis Hubs met Apache Storm op HDInsight. In dit voorbeeld leest sensorgegevens van Azure gebeurtenis Hubs, de gegevens door te verwijzen naar gegevens die zijn opgeslagen in Azure DocumentDB verrijkt en ten slotte de gegevens opslaan in Azure opslag met behulp van het Hadoop bestand System (HDFS).

![HDInsight en het diagram Internet dingen (IoT) architectuur](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

##<a name="overview"></a>Overzicht

Sensoren aan voertuigen toe te voegen, kunt u problemen met apparatuur op basis van historische gegevenstrends voorspellen, evenals verbeteringen voor toekomstige versies op basis van een patroon van gebruiksanalyse. Terwijl de traditionele MapReduce batch-verwerking kan worden gebruikt voor deze analyse, moet u kunnen snel en efficiënt laden van de gegevens van alle voertuigen in Hadoop MapReduce verwerking kan worden uitgevoerd. U kunt bovendien analyse van kritieke fout paden (temperatuur motor, remmen, enz.) in real time.

Azure gebeurtenis Hubs zijn gemaakt voor het verwerken van de enorme omvang van de gegevens die zijn gegenereerd door sensoren en Apache Storm op HDInsight om te laden en het verwerken van de gegevens voordat u ze opslaat in HDFS (ondersteund door opslag Azure) kan worden gebruikt voor verdere verwerking van MapReduce.

##<a name="solution"></a>Oplossing

Telemetriegegevens voor motor temperatuur, temperatuur en snelheid van het voertuig wordt vastgelegd door sensoren, die vervolgens wordt verzonden naar de gebeurtenis Hubs en van de auto voertuig identificatie nummer (VIN) en een tijdstempel. Daar een Storm-topologie op een Apache Storm op de cluster HDInsight leest de gegevens verwerkt en opgeslagen in HDFS.

Tijdens de verwerking wordt de VIN modelgegevens ophalen uit Azure DocumentDB gebruikt. Dit wordt toegevoegd aan de gegevensstroom voordat deze wordt opgeslagen.

De onderdelen die worden gebruikt in de topologie Storm zijn:

* **EventHubSpout** - leest gegevens van Azure gebeurtenis Hubs

* **TypeConversionBolt** - converteert de JSON-string van gebeurtenis Hubs naar een tupel met afzonderlijke waarden van de motor temperatuur, temperatuur, snelheid, VIN en tijdstempel

* **DataReferencBolt** - het model van het voertuig in opgezocht met behulp van het VIN-nummer DocumentDB

* **WasbStoreBolt** - slaat de gegevens op HDFS (Azure opslag)

Hieronder volgt een overzicht van deze oplossing:

![storm-topologie](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

> [AZURE.NOTE] Dit is een vereenvoudigde diagram en elk onderdeel in de oplossing kan meerdere exemplaren hebben. Bijvoorbeeld, zijn meerdere exemplaren van elk onderdeel in de topologie verdeeld over de knooppunten in de Storm op de cluster HDInsight.

##<a name="implementation"></a>Implementatie

Een volledig geautomatiseerde oplossing voor dit scenario is beschikbaar als onderdeel van de bibliotheek [HDInsight-Storm-voorbeelden](https://github.com/hdinsight/hdinsight-storm-examples) op GitHub. Dit voorbeeld kunt gebruiken, volg de stappen in de [IoTExample README. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Volgende stappen

Zie [voorbeeld topologieën voor Storm op HDInsight](hdinsight-storm-example-topology.md)voor meer voorbeeld Storm-topologieën.

<properties
    pageTitle="Aan de slag met de gebeurtenis Hubs met C en Apache Storm | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure gebeurtenis Hubs; verzenden van gebeurtenissen in de C- en ze worden ontvangen in een cluster Apache Storm."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="java"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Aan de slag met gebeurtenis-Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Inleiding

Gebeurtenis Hubs is een zeer schaalbare opname-systeem dat miljoenen inname kan gebeurtenissen per seconde, waardoor een aanvraag te verwerken en analyseren van de enorme hoeveelheden gegevens geproduceerd door de aangesloten apparaten en toepassingen. U kunt eenmaal verzameld in de gebeurtenis Hubs, transformeren en gegevens met behulp van een real time analytics provider of opslag cluster op te slaan.

Zie [gebeurtenis Hubs-overzicht]voor meer informatie.

In deze zelfstudie leert u hoe consumptie van berichten op de Hub van een gebeurtenis met een consoletoepassing in C en halen ze parallel met Apache Storm.

Als u deze zelfstudie hebt voltooid, moet u het volgende:

+ Een C-ontwikkelomgeving. Voor deze zelfstudie, nemen we de stack gcc op een [Azure Linux VM](../virtual-machines/virtual-machines-linux-quick-create-cli.md) met Ubuntu 14.04 aan. In de externe koppelingen krijgt instructies voor andere omgevingen.

+ Een Java-ontwikkelomgeving is geconfigureerd voor het uitvoeren van [Maven](http://maven.apache.org/). Voor deze zelfstudie, nemen we [Eclips](https://www.eclipse.org/)aan.

+ Een actieve account Azure. Als u geen account hebt, kunt u een gratis account maken in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>De toepassingen worden uitgevoerd.

U bent nu gereed voor de toepassingen worden uitgevoerd.

1.  De klasse **LogTopology** uit Eclips uitvoeren en vervolgens wachten voor het starten van de ontvangers voor alle partities.

2.  Voer het programma van de **afzender** en de gebeurtenissen weergegeven in het venster van de ontvanger zien.

    ![][23]

> [AZURE.NOTE] In deze zelfstudie alleen Storm in de lokale modus te gebruiken voor ontwikkelingsdoeleinden. Ga naar de [HDInsight Storm-overzicht] en de officiële [Storm Apache] -documentatie voor meer informatie van de Storm-implementaties en patronen.

## <a name="next-steps"></a>Volgende stappen

De volgende bronnen zijn beschikbaar voor het ontwikkelen van toepassingen, integratie van gebeurtenis Hubs en Storm.

- [Analyseren van sensorgegevens met Storm en HDInsight][] is een volledige scenario zelfstudie gebeurtenis Hubs, Storm en HBase met sensorgegevens in een cluster Hadoop nemen.
- [Ontwikkel streaming toepassingen gegevens verwerken met SCP.NET en C# op Storm- en HDInsight][] is een zelfstudie over het schrijven van Storm pijpleidingen met C#.

<!-- Images. -->
[23]: ./media/event-hubs-c-storm-getstarted/receive-storm3.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Overzicht van de gebeurtenissen Hubs]: event-hubs-overview.md

[Apache Storm]: https://storm.incubator.apache.org
[HDInsight-Storm-overzicht]: ../hdinsight/hdinsight-storm-overview.md/
[Analyseren van sensorgegevens met Storm en HDInsight]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[Streaming toepassingen gegevens verwerken met SCP.NET en C# op Storm- en HDInsight ontwikkelen]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md

<properties
    pageTitle="Aan de slag met Hubs in C# met Apache Storm gebeurtenis | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure gebeurtenis Hubs; verzenden van gebeurtenissen in C# en ze worden ontvangen in een cluster Apache Storm."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/06/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Aan de slag met gebeurtenis-Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Inleiding

Gebeurtenis Hubs is een zeer schaalbare opname-systeem dat miljoenen inname kan gebeurtenissen per seconde, waardoor een aanvraag te verwerken en analyseren van de enorme hoeveelheden gegevens geproduceerd door de aangesloten apparaten en toepassingen. U kunt eenmaal verzameld in de gebeurtenis Hubs, transformeren en gegevens met behulp van een real time analytics provider of opslag cluster op te slaan.

Zie de [gebeurtenis Hubs-overzicht]voor meer informatie.

In deze zelfstudie leert u hoe consumptie van berichten op de Hub van een gebeurtenis met een consoletoepassing in C# en halen ze parallel met Apache Storm.

Deze zelfstudie te voltooien moet u het volgende:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Een Java-ontwikkelomgeving is geconfigureerd voor het uitvoeren van [Maven](http://maven.apache.org/). Voor deze zelfstudie wordt ervan uitgegaan [Eclips](https://www.eclipse.org/)

+ Een actieve account Azure. <br/>Als u geen account hebt, kunt u een gratis account maken in een paar minuten. Zie voor meer informatie, <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Gratis proefperiode van Azure</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>De toepassingen worden uitgevoerd.

U bent nu gereed voor de toepassingen worden uitgevoerd.

1.  De klasse **LogTopology** uit Eclips uitvoeren en vervolgens wachten voor het starten van de ontvangers voor alle partities.

2.  Zie de gebeurtenissen weergegeven in het venster van de ontvanger de **afzender** project uitvoeren en druk op **Enter** in het consolevenster.

    ![][22]

## <a name="next-steps"></a>Volgende stappen

Nu dat u een werkende toepassing die een gebeurtenis Hub maakt en verzendt en ontvangt gegevens hebt gemaakt, kunt u op verplaatsen naar de volgende scenario's:

- Een volledig [voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs][].
- Het monster van de [schaal van het verwerken van de gebeurtenis met gebeurtenis-Hubs][] .

<!-- Images. -->
[22]: ./media/event-hubs-csharp-storm-getstarted/receive-storm1.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Overzicht van de gebeurtenissen Hubs]: event-hubs-overview.md
[de voorbeeldtoepassing met gebeurtenis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Schaal van het verwerken van de gebeurtenis met gebeurtenis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 
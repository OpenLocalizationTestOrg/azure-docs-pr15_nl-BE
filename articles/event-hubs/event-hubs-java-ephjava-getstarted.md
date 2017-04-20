<properties
    pageTitle="Aan de slag met Hubs in Java gebeurtenis | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure gebeurtenis Hubs; gebeurtenissen gebruiken met Java verzenden en ontvangen van deze in met behulp van de EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="core"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Aan de slag met gebeurtenis-Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Inleiding

Gebeurtenis Hubs is een zeer schaalbare opname-systeem dat miljoenen inname kan gebeurtenissen per seconde, waardoor een aanvraag te verwerken en analyseren van de enorme hoeveelheden gegevens geproduceerd door de aangesloten apparaten en toepassingen. U kunt eenmaal verzameld in de gebeurtenis Hubs, transformeren en gegevens met behulp van een real time analytics provider of opslag cluster op te slaan.

Zie de [gebeurtenis Hubs-overzicht][]voor meer informatie.

Deze zelfstudie laat zien hoe consumptie van berichten op de Hub van een gebeurtenis met een consoletoepassing in Java en ze in parallel met de bibliotheek Java gebeurtenis Processor Host ophalen.

Wilt u deze zelfstudie hebt voltooid, moet u het volgende:

+ Een Java-ontwikkelomgeving. Voor deze zelfstudie, nemen we [Eclips](https://www.eclipse.org/)aan.

+ Een actieve account Azure. <br/>Als u geen account hebt, kunt u een gratis account maken in een paar minuten. Zie voor meer informatie, <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Gratis proefperiode van Azure</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>De toepassingen worden uitgevoerd.

U bent nu gereed voor de toepassingen worden uitgevoerd.

1.  De **ontvanger** project uitvoeren.

    ![][21]

2.  Het project van de **afzender** worden uitgevoerd.

    ![][22]

## <a name="next-steps"></a>Volgende stappen

Nu dat u een werkende toepassing die een gebeurtenis Hub maakt en verzendt en ontvangt gegevens hebt gemaakt, kunt u op verplaatsen naar de volgende scenario's:

- Een volledig [voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs][].
- Het monster van de [schaal van het verwerken van de gebeurtenis met gebeurtenis-Hubs][] .

Zie [Java Developer Center](/develop/java/)voor meer informatie.

<!-- Images. -->
[21]: ./media/event-hubs-java-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-java-ephjava-getstarted/java-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Overzicht van de gebeurtenissen Hubs]: event-hubs-overview.md
[de voorbeeldtoepassing met gebeurtenis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Schaal van het verwerken van de gebeurtenis met gebeurtenis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 
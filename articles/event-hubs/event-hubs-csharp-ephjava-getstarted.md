<properties
    pageTitle="Aan de slag met Hubs in C# gebeurtenis | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure gebeurtenis Hubs; verzenden van gebeurtenissen in C# en ontvangen ze in Java met behulp van de EventProcessorHost."
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
    ms.topic="hero-article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Aan de slag met gebeurtenis-Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Inleiding

Gebeurtenis Hubs is een service waarmee grote hoeveelheden gegevens (telemetrie) gebeurtenissen van aangesloten apparaten en toepassingen worden verwerkt. Na het verzamelen van gegevens in gebeurtenis Hubs kunnen de gegevens met behulp van een cluster met opslag op te slaan of transformeren met behulp van een real time analytics provider. Deze mogelijkheid van verzameling en verwerking van grootschalige gebeurtenis is een belangrijk onderdeel van de toepassing van moderne architectuur, met inbegrip van het Internet van dingen (IoT).

Deze zelfstudie laat zien hoe de Azure klassieke portal gebruiken voor het maken van een Hub gebeurtenis. Dit ziet u ook hoe u berichten in een gebeurtenis-Hub met een consoletoepassing die is geschreven in C# verzamelen en hoe u ze ophaalt in parallel met de bibliotheek Java gebeurtenis Processor Host.

Deze zelfstudie hebt u het volgende nodig:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Een actieve account Azure. <br/>Als u niet hebt, kunt u een gratis account maken in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

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
- [Overzicht van de gebeurtenissen Hubs][]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Overzicht van de gebeurtenissen Hubs]: event-hubs-overview.md
[de voorbeeldtoepassing met gebeurtenis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Schaal van het verwerken van de gebeurtenis met gebeurtenis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3

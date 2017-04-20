<properties
    pageTitle="Aan de slag met Hubs in C# gebeurtenis | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag Azure gebeurtenis Hubs met C# gebruiken en de EventProcessorHost."
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
    ms.date="09/02/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Aan de slag met gebeurtenis-Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Inleiding

Gebeurtenis Hubs is een service waarmee grote hoeveelheden gegevens (telemetrie) gebeurtenissen van aangesloten apparaten en toepassingen worden verwerkt. Na het verzamelen van gegevens in gebeurtenis Hubs kunnen de gegevens met behulp van een cluster met opslag op te slaan of transformeren met behulp van een real time analytics provider. Deze mogelijkheid van verzameling en verwerking van grootschalige gebeurtenis is een belangrijk onderdeel van de toepassing van moderne architectuur, met inbegrip van het Internet van dingen (IoT).

Deze zelfstudie laat zien hoe de Azure klassieke portal gebruiken voor het maken van een Hub gebeurtenis. Dit ziet u ook hoe u berichten in een gebeurtenis-Hub met een consoletoepassing die is geschreven in C# verzamelen en hoe u ze ophaalt in parallel met de C#- [Gebeurtenis Processor Host][] -bibliotheek.

Deze zelfstudie hebt u het volgende nodig:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Een actieve account Azure. Als u niet hebt, kunt u een gratis account maken in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/free/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>De toepassingen worden uitgevoerd.

U bent nu gereed voor de toepassingen worden uitgevoerd.

1. Open de **ontvanger** -project dat u eerder hebt gemaakt in Visual Studio.

2. Klik met de rechtermuisknop op de oplossing van de **ontvanger** , en vervolgens klikt u op **toevoegen**en klik vervolgens op **Bestaand Project**.
 
3. Zoek het bestand Sender.csproj en dubbelklik vervolgens op toe te voegen aan de oplossing.
 
4. Nogmaals, de oplossing in het **opvangvat** met de rechtermuisknop en klik vervolgens op **Eigenschappen**. De eigenschappenpagina van de **ontvanger** wordt weergegeven.

5. **Project opstarten**, klik op de knop **meerdere projecten voor opstarten** . Stel het **actie** voor projecten van de **ontvanger** en de **afzender** te **starten**.

    ![][19]

6. Klik op **Projectafhankelijkheden**. Klik in het vak **projecten** op **afzender**. In het vak **Depends.exe op** Controleer of **dat ontvanger** is ingeschakeld.

    ![][20]

7. Klik op **OK** om het dialoogvenster **Eigenschappen** te sluiten.

1.  Druk op F5 de **ontvanger** -project van Visual Studio worden uitgevoerd en vervolgens wachten op het begin van de ontvangers voor alle partities.

    ![][21]

2.  Het project van de **afzender** wordt automatisch uitgevoerd. Druk op **Enter** in het consolevenster, en de gebeurtenissen weergegeven in het venster van de ontvanger zien.

    ![][22]

Druk op **Ctrl + C** in het venster van de **afzender** naar de afzender toepassing te beëindigen, druk op **Enter** in het venster van de ontvanger die toepassing afsluiten.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een werkende toepassing die een gebeurtenis Hub maakt en verzendt en ontvangt gegevens hebt gemaakt, kunt u op verplaatsen naar de volgende scenario's:

- Een volledig [voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs][].
- Het monster van de [schaal van het verwerken van de gebeurtenis met gebeurtenis-Hubs][] .
- [Overzicht van de gebeurtenissen Hubs][]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Gebeurtenis Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Overzicht van de gebeurtenissen Hubs]: event-hubs-overview.md
[de voorbeeldtoepassing met gebeurtenis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Schaal van het verwerken van de gebeurtenis met gebeurtenis-Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 

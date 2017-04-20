<properties
    pageTitle="Routering en Tag-expressies"
    description="In dit onderwerp vindt routering en tag expressies voor Azure melding hubs."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="routing-and-tag-expressions"></a>Routering en tag expressies

##<a name="overview"></a>Overzicht

Tag-expressies kunnen u target bepaalde groepen apparaten of meer in het bijzonder registraties bij het verzenden van een push-bericht via Hubs melding.


## <a name="targeting-specific-registrations"></a>Gericht op specifieke registraties

De enige manier om het doel specifieke mededeling registraties is het koppelen van labels, vervolgens richten op deze codes. Zoals besproken in het [Beheer van de registratie](notification-hubs-push-notification-registration-management.md), push-meldingen ontvangen heeft een app een ingang op een hub-meldingen registreren. Zodra een inschrijving op een hub-melding is gemaakt, kunt de backend toepassing push-meldingen te verzenden.
De backend toepassing kunt de registraties tot doel een specifieke melding op de volgende manieren:

1. **Uitzending**: alle registraties in de hub kennisgeving ontvangen het bericht.
2. **Tag**: alle registraties die het opgegeven label bevat de melding ontvangt.
3. **Labelexpressie**: alle registraties set codes die overeenkomen met de opgegeven expressie melding.

## <a name="tags"></a>Tags

Een tag kan bestaan uit een tekenreeks, maximaal 120 met alfanumerieke tekens en de volgende niet-alfanumerieke tekens: '_', ‘@’, '#', '. ',':', '-'. In het volgende voorbeeld wordt een toepassing die u van meldingen over bepaalde muziekgroepen toast ontvangen. In dit scenario is een eenvoudige manier om meldingen route label registraties met codes die staan voor de verschillende banden, zoals in de volgende afbeelding.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

In deze afbeelding de gelabelde **Beatles** bereikt de Tablet PC die met het label **Beatles**geregistreerd.

Zie voor meer informatie over het maken van registraties voor tags [Registratie Management](notification-hubs-push-notification-registration-management.md).

U kunt meldingen verzenden aan labels met de methoden van de meldingen verzenden van de `Microsoft.Azure.NotificationHubs.NotificationHubClient` klasse in de [Microsoft Azure melding Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. U kunt ook Node.js of de Push-meldingen REST-API's.  Hier volgt een voorbeeld met behulp van de SDK.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Tags hoeft niet te worden vooraf ingerichte en kunnen verwijzen naar meerdere app-specifieke concepten. Bijvoorbeeld, gebruikers van de toepassing van dit voorbeeld commentaar op banden en wilt toasts, niet alleen voor het commentaar op hun favoriete banden, maar ook voor alle opmerkingen ontvangen van hun vrienden, ongeacht de band waarop ze zijn opmerkingen. In de volgende afbeelding toont een voorbeeld van dit scenario:



![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

Alice is geïnteresseerd in updates voor de Beatles en Bob is geïnteresseerd in updates voor de Wailers in deze afbeelding. Bob is ook geïnteresseerd in de opmerkingen van Charlie en Charlie is geïnteresseerd in de Wailers. Wanneer een melding wordt verzonden voor de opmerking van Charlie op de Beatles, ontvangen zowel Alice als Bob.

U kunt meerdere problemen in codes (bijvoorbeeld "band_Beatles" of "follows_Charlie") codeert, zijn tags eenvoudige tekenreeksen en niet naar eigenschappen met waarden. Een registratie is afgestemd op de aanwezigheid of afwezigheid van een specifieke tag.

Zie voor een volledige stapsgewijze zelfstudie over het gebruik van codes voor het verzenden van belangengroepen [Nieuws te analyseren](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).


## <a name="using-tags-to-target-users"></a>Met behulp van tags aan gebruikers van target

Een andere manier gebruik van labels is voor het identificeren van alle apparaten van een bepaalde gebruiker. Registraties kunnen worden gelabeld met een label met een gebruikers-id, zoals in de volgende afbeelding:


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

In deze afbeelding bereikt de uid:Alice gecodeerd bericht alle registraties gelabelde uid:Alice; dus, alle apparaten van Alice.


##<a name="tag-expressions"></a>Label expressies

Er zijn gevallen waarin een kennisgeving heeft tot doel een aantal registraties die niet door een afzonderlijke code, maar door een Boole-expressie op tags.

U kunt een sport-toepassing die een herinnering naar iedereen in Boston over een spel tussen de Red Sox en Cardinals stuurt. Als de clienttoepassing codes over rente in teams en de locatie wordt geregistreerd, moet de kennisgeving worden gericht naar iedereen in Boston die geïnteresseerd zijn in de Red Sox of de Cardinals is. Deze voorwaarde kan worden uitgedrukt met de volgende Boole-expressie:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Label expressies kunnen bevatten alle Boole-operators, zoals AND (& &), of (|), en geen (!). Ze kunnen ook haakjes bevatten. Label expressies zijn beperkt tot 20 tags bevatten alleen ORs; anders zijn ze beperkt tot 6 tags.

Hier is een voorbeeld voor het verzenden van berichten met label expressies met behulp van de SDK.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)"; 

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

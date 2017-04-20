<properties
    pageTitle="Push-meldingen toevoegen aan iOS-App met Azure mobiele Apps"
    description="Leren werken met Azure Mobile Apps push-meldingen verzenden naar uw iOS-app."
    services="app-service\mobile"
    documentationCenter="ios"
    manager="yochayk"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="yuaxu"/>


# <a name="add-push-notifications-to-your-ios-app"></a>Push-meldingen toevoegen aan uw iOS-App

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie voegt u push-meldingen aan het project [iOS snel starten] zodat een push-bericht wordt verzonden naar het apparaat elke keer dat een record wordt ingevoegd.

Als u niet het project van de server gedownload snel starten, moet u het pakket push notification-extensie. Zie [werken met de .NET back-endserver SDK voor Azure mobiele toepassingen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor meer informatie.

[IOS simulator biedt geen ondersteuning voor push-meldingen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). U moet een fysieke iOS-apparaat en een [Apple-ontwikkelaar programma lidmaatschap](https://developer.apple.com/programs/ios/).

##<a name="configure-hub"></a>Melding Hub configureren

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>App voor push-meldingen registreren

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Azure voor het verzenden van push-meldingen configureren

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a id="update-server"></a>Update back-end om push-meldingen te verzenden

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Push-meldingen aan app toevoegen

[AZURE.INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Test push-meldingen

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

##<a id="more"></a>Meer

* Sjablonen geven u de flexibiliteit voor het verzenden van dit gereedschap duwt platforms en gelokaliseerde gereedschap duwt. [Hoe gebruik iOS Client Library voor Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) laat zien hoe u sjablonen registreren.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS snel starten]: app-service-mobile-ios-get-started.md

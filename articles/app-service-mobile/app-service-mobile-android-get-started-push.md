<properties
    pageTitle="Push-meldingen toevoegen aan Android-App met Azure mobiele Apps"
    description="Leren werken met Azure Mobile Apps push-meldingen verzenden naar uw Android app."
    services="app-service\mobile"
    documentationCenter="android"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-android-app"></a>Push-meldingen toevoegen aan uw Android App

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie voegt u push-meldingen aan het project [Android slag] zodat een push-bericht wordt verzonden naar het apparaat elke keer dat een record wordt ingevoegd.

Als u niet het project van de server gedownload snel starten, moet u het pakket push notification-extensie. Zie [werken met de .NET back-endserver SDK voor Azure mobiele toepassingen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)voor meer informatie.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig:

* Een IDE, afhankelijk van de backend van uw project:

    * [Android Studio](https://developer.android.com/sdk/index.html) als dit app een Node.js backend heeft.

    * [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) of later als dit app heeft een .net back-end.

* Android 2.3 of hoger, Google opslagplaats herziening 27 of hoger en Google Play Services 9.0.2 of hoger voor Firebase Cloud Messaging.

* Voltooi het [Android snel starten].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Een project maken dat Firebase Cloud Messaging ondersteunt

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Een Hub meldingen configureren

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Azure voor het verzenden van push-meldingen configureren

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Push-meldingen voor de serverproject inschakelen

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Push-meldingen toevoegen aan uw app

In deze sectie kunt bijwerken u uw client Android app voor het afhandelen van push-meldingen.

### <a name="verify-android-sdk-version"></a>Controleer of de versie van de Android SDK

[AZURE.INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

De volgende stap is om Google Play services te installeren. Google Cloud Messaging heeft sommige API niveau minimumeisen voor ontwikkelen en testen die de eigenschap **minSdkVersion** in het Manifest aan moet voldoen.

Als u met een ouder apparaat testen wilt, raadpleegt u het [Instellen van Google Play Services SDK] om te bepalen hoe laag u kunt deze waarde instellen en stel deze in op de juiste wijze.

### <a name="add-google-play-services-to-the-project"></a>Google Play Services toevoegen aan het project

[AZURE.INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Voeg code toe:

[AZURE.INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Test de app tegen de gepubliceerde mobiele service

U kunt de app testen door het koppelen van een Android telefoon met een USB-kabel rechtstreeks of via een virtueel apparaat in de emulator.

## <a name="more"></a>Meer

<!-- URLs -->
[Android snel starten]: app-service-mobile-android-get-started.md

[Google Play Services SDK instellen]:https://developers.google.com/android/guides/setup

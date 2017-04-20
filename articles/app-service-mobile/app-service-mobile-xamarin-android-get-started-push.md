<properties
    pageTitle="Push-meldingen toevoegen aan uw app Xamarin.Android | Azure App-Service"
    description="Informatie over het gebruik van App-Service Azure en Azure melding Hubs push-meldingen verzenden naar uw app Xamarin.Android"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Push-meldingen toevoegen aan uw app Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Overzicht


In deze zelfstudie voegt u push-meldingen aan het project [Xamarin.Android snelle start](app-service-mobile-windows-store-dotnet-get-started.md) zodat een push-bericht wordt verzonden naar het apparaat elke keer dat een record wordt ingevoegd.

Als u niet het project van de server gedownload snel starten, moet u het pakket push notification-extensie. Zie [werken met de .NET back-endserver SDK voor Azure mobiele toepassingen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor meer informatie.


##<a name="prerequisites"></a>Vereisten

Deze zelfstudie is het volgende vereist:

+ Een actieve Google-account. U kunt zich aanmelden voor een Google-account op [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).
+ [Google Cloud Messaging clientcomponent](http://components.xamarin.com/view/GCMClient/).

##<a name="configure-hub"></a>Een Hub meldingen configureren

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a id="register"></a>Firebase inschakelen Cloud Messaging

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

##<a name="configure-azure-to-send-push-requests"></a>Azure voor het verzenden van aanvragen push configureren

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

##<a id="update-server"></a>Update van het serverproject om push-meldingen te verzenden

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a id="configure-app"></a>Configureer de client-project voor push-meldingen

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>Push notifications code toevoegen aan uw app

[AZURE.INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Push-meldingen in de app testen

U kunt de app testen met behulp van een virtueel apparaat in de emulator. Er zijn extra configuratiestappen vereist wanneer een emulator wordt uitgevoerd.

1. Zorg ervoor dat u wilt implementeren of foutopsporing op een virtueel apparaat met Google APIs ingesteld als het doel, zoals hieronder wordt weergegeven in beheer van Android virtueel apparaat (AVD).

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. Een Google-account toevoegen aan het Android apparaat door te klikken op **Apps** > **Instellingen** > **account toevoegen**en volg de aanwijzingen op het scherm.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. De takenlijst van toepassing als voor uitvoeren en een nieuwe todo item invoegen. Deze periode wordt wordt een pictogram in het systeemvak weergegeven in het systeemvak. U kunt de melding lade om de volledige tekst van het bericht weer te openen.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)


<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/

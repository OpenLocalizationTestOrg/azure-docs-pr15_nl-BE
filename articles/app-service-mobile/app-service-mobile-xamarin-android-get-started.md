<properties
    pageTitle="Aan de slag met Azure mobiele Apps voor apps Xamarin.Android"
    description="Volg deze zelfstudie aan de slag met Azure Mobile Apps voor Xamarin Android ontwikkeling"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha" />

#<a name="create-a-xamarinandroid-app"></a>Maak een Xamarin.Android-App

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Overzicht

In deze zelfstudie wordt beschreven hoe u een cloud-gebaseerde back-end-service toevoegen aan een Xamarin.Android-app. Zie [Wat zijn mobiele Apps](app-service-mobile-value-prop.md)voor meer informatie.

Een schermafbeelding van de voltooide app lager is dan:

![][0]

Voltooien van deze zelfstudie is een vereiste voor alle andere mobiele Apps zelfstudies voor Xamarin.Android toepassingen.

##<a name="prerequisites"></a>Vereisten

Als u deze zelfstudie hebt voltooid, moet u de volgende vereisten:

* Een actieve account Azure. Als u geen account hebt, meld u aan voor een proefversie Azure en krijgt 10 gratis mobiele Apps. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio met Xamarin. Zie [Setup en installeren voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) voor instructies.

>[AZURE.NOTE]Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](https://tryappservice.azure.com/?appServiceName=mobile).  U kunt direct een tijdelijk starter Mobile App in App-Service maken. Geen creditcard vereist; geen verplichtingen.

## <a name="create-an-azure-mobile-app-backend"></a>Maak een backend Azure mobiele App

Volg deze stappen uit om een mobiele App backend.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

U hebt nu een backend Azure Mobile App die kan worden gebruikt door uw mobiele-clienttoepassingen ingericht. Download vervolgens een serverproject voor een eenvoudige "todo lijst" back-end en publiceren naar Azure.

## <a name="configure-the-server-project"></a>De serverproject configureren

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>Download en installeer de app Xamarin.Android

1. **Downloaden en uitvoeren van het project Xamarin.Android**, klik op de knop **downloaden** .

    Sla het gecomprimeerde bestand naar de lokale computer en maak een notitie van de plaats waar u het opslaat.

2. Druk op **F5 om te bouwen van het project en de app starten** .

3. In de app, typ zinvolle tekst, zoals _de zelfstudie voltooien_ en klik vervolgens op de knop **toevoegen** .

    ![][10]

    Gegevens uit de aanvraag wordt ingevoegd in de tabel TodoItem. Artikelen die zijn opgeslagen in de tabel worden geretourneerd door de mobiele app-end en de gegevens worden weergegeven in de lijst.

    > [AZURE.NOTE] U kunt de code voor toegang uw backend mobiele app tot voor het opvragen en het invoegen van gegevens uit het bestand ToDoActivity.cs in C# bekijken.

##<a name="next-steps"></a>Volgende stappen

* [Off line synchronisatie toevoegen aan uw app](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Verificatie toevoegen aan uw app](app-service-mobile-xamarin-android-get-started-users.md)
* [Push-meldingen toevoegen aan uw app Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md)
* [Het gebruik van beheerde client voor Azure mobiele Apps](app-service-mobile-dotnet-how-to-use-client-library.md)


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203

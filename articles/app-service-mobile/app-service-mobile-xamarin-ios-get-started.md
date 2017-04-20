<properties
    pageTitle="Aan de slag met Azure App Service Mobile Apps voor apps Xamarin.iOS | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met het gebruik van mobiele Apps voor de ontwikkeling van Xamarin.iOS."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>


#<a name="create-a-xamarinios-app"></a>Maak een Xamarin.iOS-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Overzicht

In deze zelfstudie wordt beschreven hoe u een cloud-gebaseerde back-end-service toevoegen aan een Xamarin.iOS mobiele app via een backend Azure mobiele app.  U maakt een eenvoudige _lijst van Todo_ Xamarin.iOS app die app gegevens worden opgeslagen in Azure en een nieuwe mobiele app backend.

Voltooien van deze zelfstudie is een vereiste voor alle andere Xamarin.iOS zelfstudies over het gebruik van de functie van mobiele Apps in Azure App-Service.

##<a name="prerequisites"></a>Vereisten

Als u deze zelfstudie hebt voltooid, moet u de volgende vereisten:

* Een actieve account Azure. Als u geen account hebt, meld u aan voor een evaluatie van Azure en krijgt 10 gratis mobiele apps die u ook na het einde van de proefperiode kunt blijven gebruiken. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio met Xamarin. Zie [Setup en installeren voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) voor instructies.

* Een Mac met Xcode v7.0 of hoger en de Gemeenschap van Xamarin Studio geïnstalleerd. Zie [Setup en installeren voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) en [instellingen, installatie en keuringen voor Mac-gebruikers](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

>[AZURE.NOTE]Als u aan de slag met Azure App-Service wilt voordat u zich voor een account met Azure aanmeldt, gaat u naar de [App-Service probeert](https://tryappservice.azure.com/?appServiceName=mobile). Onmiddellijk kunt u een tijdelijk starter mobiele app in de App-Service maken, geen creditcard vereist en geen verplichtingen.

## <a name="create-an-azure-mobile-app-backend"></a>Maak een backend Azure mobiele App

Volg deze stappen uit om een mobiele App backend.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>De serverproject configureren

U hebt nu een backend Azure Mobile App die kan worden gebruikt door uw mobiele-clienttoepassingen ingericht. Download vervolgens een serverproject voor een eenvoudige "todo lijst" back-end en publiceren naar Azure.

Volg de volgende stappen voor het configureren van de serverproject voor het gebruik van de Node.js of .NET back-end.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>Download en installeer de app Xamarin.iOS

1. De [Azure portal] in een browservenster geopend.

2. Klik op het blad van de instellingen voor uw mobiele App, **Aan de slag** > **Xamarin.iOS**. Stap 3, klik op **een nieuwe toepassing maken** als deze nog niet is geselecteerd.  Klik vervolgens op de knop **downloaden** .

    Een clienttoepassing die verbinding met uw mobiele backend maakt wordt gedownload. Sla het gecomprimeerde bestand naar de lokale computer en maak een notitie van de plaats waar u het opslaat.

3. Pak het project dat u hebt gedownload en vervolgens openen in Xamarin Studio (of Visual Studio).

    ![][9]

    ![][8]

4. Druk op F5 om het project bouwen en start de app in de iPhone emulator.

5. Typ in de app, zinvolle tekst, zoals _Informatie over Xamarin_, en klik vervolgens op de **+** knop.

    ![][10]

    Gegevens uit de aanvraag wordt ingevoegd in de tabel TodoItem. Artikelen die zijn opgeslagen in de tabel worden geretourneerd door de mobiele app-end en de gegevens worden weergegeven in de lijst.

>[AZURE.NOTE]U kunt de code voor toegang uw backend mobiele app tot voor het opvragen en het invoegen van gegevens in het bestand QSTodoService.cs in C# bekijken.

##<a name="next-steps"></a>Volgende stappen

* [Off line synchronisatie toevoegen aan uw app](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Verificatie toevoegen aan uw app](app-service-mobile-xamarin-ios-get-started-users.md)
* [Push-meldingen toevoegen aan uw app Xamarin.Android](app-service-mobile-xamarin-ios-get-started-push.md)
* [Het gebruik van beheerde client voor Azure mobiele Apps](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure portal]: https://portal.azure.com/

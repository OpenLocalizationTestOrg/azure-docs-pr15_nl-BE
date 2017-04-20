<properties
    pageTitle="Aan de slag met mobiele Apps met behulp van Xamarin.Forms"
    description="Volg deze zelfstudie aan de slag met Azure mobiele Apps voor de ontwikkeling van Xamarin.Forms"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-xamarinforms-app"></a>Maak een Xamarin.Forms-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Overzicht

In deze zelfstudie wordt beschreven hoe u een cloud-gebaseerde back-end-service toevoegen aan een Xamarin.Forms mobiele app met behulp van een backend Azure Mobile App. U maakt een eenvoudige _lijst van Todo_ Xamarin.Forms app die app gegevens worden opgeslagen in Azure en een nieuwe Mobile App backend.

Voltooien van deze zelfstudie is een vereiste voor alle andere mobiele Apps zelfstudies voor Xamarin.Forms.

##<a name="prerequisites"></a>Vereisten

Deze zelfstudie hebt u het volgende nodig:

* Een actieve account Azure. Als u geen account, kunt u zich aanmelden voor een proefperiode Azure en ontvang maximaal vrij 10 Mobile Apps die u ook na het einde van de proefperiode kunt blijven gebruiken. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio met Xamarin. Zie [Setup en installeren voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) voor instructies. 

* Een Mac met Xcode v7.0 of hoger en de Gemeenschap van Xamarin Studio geïnstalleerd. Zie [Setup en installeren voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) en [instellingen, installatie en keuringen voor Mac-gebruikers](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).
 
>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](https://tryappservice.azure.com/?appServiceName=mobile), waar u direct een tijdelijk starter mobiele App in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

## <a name="create-a-new-azure-mobile-app-backend"></a>Maak een nieuwe Azure Mobile App backend.

Volg deze stappen uit om een nieuwe Mobile App backend.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]


U hebt nu een backend Azure Mobile App die kan worden gebruikt door uw mobiele-clienttoepassingen ingericht. Vervolgens wordt u een serverproject voor een eenvoudige "todo lijst" downloaden backend en publiceren naar Azure.

## <a name="configure-the-server-project"></a>De serverproject configureren

De volgende stappen voor het configureren van de serverproject voor het gebruik van de Node.js of .NET back-end.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-xamarinforms-solution"></a>Download en installeer de oplossing Xamarin.Forms

U hebt hier een paar andere opties. U kunt de oplossing met een Mac downloaden en openen in Xamarin Studio, of u kunt downloaden van de oplossing op een Windows-computer en deze openen in Visual Studio voor het bouwen van de iOS-app met behulp van een Mac via het netwerk. Zie [Setup en installeren voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) als u meer gedetailleerde instructies over de Xamarin setup scenario's.

We gaan verder:

 1. Op uw Mac of op uw Windows-computer, opent u de [Azure Portal] in een browservenster.
 2. Klik op het blad van de instellingen voor uw mobiele App, **Aan de slag** (onder mobiel) > **Xamarin.Forms**. Stap 3, klik op **een nieuwe toepassing maken** als deze nog niet is geselecteerd.  Klik vervolgens op de knop **downloaden** .

    Hiermee downloadt u een project met een clienttoepassing die is verbonden met uw mobiele app. Sla het gecomprimeerde bestand naar de lokale computer en maak een notitie van de plaats waar u het opslaat.

 3. Pak het project dat u hebt gedownload en vervolgens openen in Xamarin Studio of Visual Studio.

    ![][9]

    ![][8]


##<a name="optional-run-the-ios-project"></a>(Optioneel) Voer de iOS-project

Deze sectie is voor het uitvoeren van het project Xamarin iOS voor iOS-apparaten. Als u niet met iOS apparaten werkt, kunt u deze sectie overslaan.

####<a name="in-xamarin-studio"></a>In de Studio van Xamarin

1. Klik met de rechtermuisknop op het iOS-project en klik vervolgens op **Instellen als Project opstarten**.
2. Klik op **uitvoeren** in het menu **Foutopsporing starten** om het project bouwen en start de app in de iPhone emulator.

####<a name="in-visual-studio"></a>In Visual Studio
1. Klik met de rechtermuisknop op het iOS-project en klik vervolgens op **instellen als Project opstarten**.
2. Klik op het menu **Build** **Configuration Manager**.
3. Schakel de selectievakjes **bouwen** en **implementeren** van het i/o-project in het dialoogvenster **Configuration Manager** .
4. Druk op **F5 om het project bouwen en start de app in de iPhone emulator** .

    >[AZURE.NOTE] Als u problemen ondervindt bij het bouwen, ondersteuning voeren NuGet package manager en de update naar de nieuwste versie van de Xamarin voor pakketten. Soms kunnen de Quickstart projecten achterblijven bij in worden bijgewerkt naar de nieuwste.    

Typ in de app, zinvolle tekst, zoals _Informatie over Xamarin_ en klik vervolgens op de **+** knop.

![][10]

Dit verzendt een POST-aanvraag naar de nieuwe mobiele app backend gehost in Azure. Gegevens uit de aanvraag wordt ingevoegd in de tabel TodoItem. Artikelen die zijn opgeslagen in de tabel worden geretourneerd door de mobiele app-end en de gegevens worden weergegeven in de lijst.

>[AZURE.NOTE]
> Hier vindt u de code die toegang heeft tot uw backend mobiele app in het bestand TodoItemManager.cs in C# van de draagbare class library-project van uw oplossing.

##<a name="optional-run-the-android-project"></a>(Optioneel) De Android project uitvoeren

Deze sectie is voor het uitvoeren van het project Xamarin droid voor Android. Als u niet met Android apparaten werkt, kunt u deze sectie overslaan.

####<a name="in-xamarin-studio"></a>In de Studio van Xamarin

1. Klik met de rechtermuisknop op het Android project en klik vervolgens op **Instellen als Project opstarten**.
2. Klik op **uitvoeren** in het menu **Foutopsporing starten** om te bouwen van het project en de app starten in een Android emulator.

####<a name="in-visual-studio"></a>In Visual Studio
1. Klik met de rechtermuisknop op het project Android (Droid) en klik vervolgens op **instellen als Project opstarten**.
4. Klik op het menu **Build** **Configuration Manager**.
5. Selecteer in het dialoogvenster **Configuration Manager** de selectievakjes **bouwen** en **implementeren** van het Android-project.
6. Druk op **F5 om te bouwen van het project en de app starten in een Android emulator** .

    >[AZURE.NOTE] Als u problemen ondervindt bij het bouwen, ondersteuning voeren NuGet package manager en de update naar de nieuwste versie van de Xamarin voor pakketten. Soms kunnen de Quickstart projecten achterblijven bij in worden bijgewerkt naar de nieuwste.    


Typ in de app, zinvolle tekst, zoals _Informatie over Xamarin_ en klik vervolgens op de **+** knop.

![][11]

Dit verzendt een POST-aanvraag naar de nieuwe mobiele app backend gehost in Azure. Gegevens uit de aanvraag wordt ingevoegd in de tabel TodoItem. Artikelen die zijn opgeslagen in de tabel worden geretourneerd door de mobiele app-end en de gegevens worden weergegeven in de lijst.

> [AZURE.NOTE]
> Hier vindt u de code die toegang heeft tot uw backend mobiele app in het bestand TodoItemManager.cs in C# van de draagbare class library-project van uw oplossing.


##<a name="optional-run-the-windows-project"></a>(Optioneel) Het project van Windows uitvoeren


Deze sectie is voor het uitvoeren van het project Xamarin WinApp voor Windows-apparaten. Als u niet met Windows-apparaten werkt kunt u deze sectie overslaan.


####<a name="in-visual-studio"></a>In Visual Studio
1. Met de rechtermuisknop op een van de projecten van Windows en klik vervolgens op **instellen als Project opstarten**.
4. Klik op het menu **Build** **Configuration Manager**.
5. Schakel de selectievakjes **bouwen** en **implementeren** van de Windows-project dat u hebt gekozen in het dialoogvenster **Configuration Manager** .
6. Druk op **F5 om het project bouwen en start de app in een Windows-emulator** .

    >[AZURE.NOTE] Als u problemen ondervindt bij het bouwen, ondersteuning voeren NuGet package manager en de update naar de nieuwste versie van de Xamarin voor pakketten. Soms kunnen de Quickstart projecten achterblijven bij in worden bijgewerkt naar de nieuwste.    


Typ in de app, zinvolle tekst, zoals _Informatie over Xamarin_ en klik vervolgens op de **+** knop.

Dit verzendt een POST-aanvraag naar de nieuwe mobiele app backend gehost in Azure. Gegevens uit de aanvraag wordt ingevoegd in de tabel TodoItem. Artikelen die zijn opgeslagen in de tabel worden geretourneerd door de mobiele app-end en de gegevens worden weergegeven in de lijst.

![][12]

> [AZURE.NOTE]
> Hier vindt u de code die toegang heeft tot uw backend mobiele app in het bestand TodoItemManager.cs in C# van de draagbare class library-project van uw oplossing.

##<a name="next-steps"></a>Volgende stappen

* [Verificatie toevoegen aan uw app](app-service-mobile-xamarin-forms-get-started-users.md)  
Informatie over het verifiëren van gebruikers van uw toepassing met een id-provider.

* [Push-meldingen toevoegen aan uw app](app-service-mobile-xamarin-forms-get-started-push.md)  
Informatie over het toevoegen van push-meldingen en support voor uw app en configureer uw mobiele App backend Azure melding Hubs gebruikt om push-meldingen te verzenden.

* [Off line synchronisatie van uw app inschakelen](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Informatie over het off line ondersteuning van uw app in de backend voor een mobiele App toevoegen. Off line synchronisatie kan eindgebruikers communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er geen netwerkverbinding is.

* [Het gebruik van beheerde client voor Azure mobiele Apps](app-service-mobile-dotnet-how-to-use-client-library.md)  
Informatie over het werken met de beheerde client SDK in uw app Xamarin. 


<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure Portal]: https://portal.azure.com/


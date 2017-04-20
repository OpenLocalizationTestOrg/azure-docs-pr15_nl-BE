<properties
    pageTitle="Aan de slag met verificatie voor Mobile Apps in iOS Xamarin"
    description="Informatie over het verifiëren van gebruikers van uw Xamarin iOS-app via een groot aantal identiteitsaanbieders van, met inbegrip van AAD, Google, Facebook, Twitter en Microsoft met mobiele Apps."
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
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinios-app"></a>Verificatie toevoegen aan uw app Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In dit onderwerp wordt beschreven hoe u verificatie van gebruikers van een App Service Mobile App van uw clienttoepassing. In deze zelfstudie kunt u verificatie toevoegen aan het Xamarin.iOS quickstart-project met een id-provider die wordt ondersteund door de App Service. Nadat het wordt met succes geverifieerd en gemachtigd door uw mobiele-App, de waarde van de gebruiker-ID wordt weergegeven en kunt u toegang kunnen krijgen tot gegevens in een tabel met beperkte toegang.

U moet eerst de zelfstudie voor [maken van een app Xamarin.iOS]voltooien. Als u niet het project van de server gedownload snel starten, moet u het pakket voor verificatie-extensie toevoegen aan uw project. Zie [werken met de .NET back-endserver SDK voor Azure mobiele toepassingen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)voor meer informatie over server-extensie.

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Registreer uw app voor verificatie en App-Services configureren

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Machtigingen voor geverifieerde gebruikers beperken

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. in Visual Studio of Xamarin Studio het client-project worden uitgevoerd op een apparaat of een emulator. Controleer of dat een niet-verwerkte uitzondering met de statuscode van 401 (Unauthorized) treedt op nadat de toepassing wordt gestart. De storing is aangemeld bij de console van de debugger. Dus in Visual Studio ziet u de fout in het uitvoervenster.

&nbsp;&nbsp;Deze ongeoorloofde fout treedt op omdat de app probeert te krijgen tot uw backend Mobile App als een niet-geverifieerde gebruiker. Verificatie vereist is door de tabel *TodoItem* .

Vervolgens wordt bijgewerkt de app client aanvraag bronnen van de mobiele App-end met een geverifieerde gebruiker.

##<a name="add-authentication-to-the-app"></a>Verificatie toevoegen aan de app.

In dit gedeelte wijzigt u de app om een loginscherm voor het weergeven van gegevens weer te geven. Wanneer de toepassing wordt gestart, wordt geen geen verbinding maken met uw App Service en gegevens kunnen niet worden weergegeven. Na de eerste keer dat de gebruiker uitvoert de penbeweging vernieuwen het aanmeldingsscherm wordt weergegeven; na een geslaagde aanmelding wordt de lijst met items die moeten worden weergegeven.

1. Open het bestand **QSTodoService.cs** in de client-project en voeg de volgende instructie en `MobileServiceUser` met de accessor aan de klasse QSTodoService:

    ```
        using UIKit;
    ```

        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. Voeg nieuwe methode met de naam **verifiëren** om te **QSTodoService** met de volgende definitie:


        public async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Als u een identiteit een Facebook-voorziening gebruikt, wijzigt de waarde doorgegeven aan de **LoginAsync** op een van de volgende: _MicrosoftAccount_, _Twitter_, _Google_of _WindowsAzureActiveDirectory_.

3. Open **QSTodoListViewController.cs**. Wijzigt de methodedefinitie van de van **ViewDidLoad** voor het verwijderen van het aanroepen van **RefreshAsync()** in de buurt van het einde:

        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            todoService = QSTodoService.DefaultService;
           await todoService.InitializeStoreAsync ();

           RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync ();
           }

            // Comment out the call to RefreshAsync
            // await RefreshAsync ();
        }


4. De methode **RefreshAsync** om te verifiëren als **de eigenschap** null-waarde wijzigen. Voeg de volgende code boven aan de methodedefinitie van de:

        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method

5. In Visual Studio of Xamarin Studio verbonden met de Host Xamarin bouwen op uw Macintosh-computer, start het project client is gericht op een apparaat of een emulator. Controleer of de app geen gegevens bevat.

    De penbeweging vernieuwen uitvoeren omdat op de lijst van artikelen, waardoor het aanmeldingsscherm wordt weergegeven. De app wordt de lijst van todo-items weergegeven nadat u geldige referenties hebt ingevoerd, en kunt u updates in de gegevens.


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Maak een Xamarin.iOS-app]: app-service-mobile-xamarin-ios-get-started.md

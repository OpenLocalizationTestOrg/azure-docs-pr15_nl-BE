<properties
    pageTitle="Aan de slag met verificatie voor Mobile Apps in app Xamarin.Forms | Microsoft Azure"
    description="Informatie over het verifiëren van gebruikers van uw app Xamarin formulieren via een groot aantal identiteitsaanbieders van, met inbegrip van AAD, Google, Facebook, Twitter en Microsoft met mobiele Apps."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinforms-app"></a>Verificatie toevoegen aan uw app Xamarin.Forms

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

##<a name="overview"></a>Overzicht

In dit onderwerp wordt beschreven hoe u verificatie van gebruikers van een App Service Mobile App van uw clienttoepassing. In deze zelfstudie kunt u verificatie toevoegen aan het Xamarin.Forms quickstart-project met een id-provider die wordt ondersteund door de App Service. Nadat het wordt met succes geverifieerd en gemachtigd door uw mobiele-App, de waarde van de gebruiker-ID wordt weergegeven en u toegang kunnen krijgen tot gegevens in een tabel met beperkte toegang.

##<a name="prerequisites"></a>Vereisten

Voor het beste resultaat in deze zelfstudie wordt aangeraden eerst de zelfstudie voor het [maken van een app Xamarin.Forms](app-service-mobile-xamarin-forms-get-started.md) te voltooien. Nadat u deze zelfstudie hebt voltooid, hebt u een Xamarin.Forms-project dat is een app van de takenlijst van meerdere platforms.

Als u niet het project van de server gedownload snel starten, moet u het pakket voor verificatie-extensie toevoegen aan uw project. Zie [werken met de .NET back-endserver SDK voor Azure mobiele toepassingen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)voor meer informatie over server-extensie.

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Registreer uw app voor verificatie en App-Services configureren

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Machtigingen voor geverifieerde gebruikers beperken

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]


##<a name="add-authentication-to-the-portable-class-library"></a>Verificatie op de draagbare class-bibliotheek toevoegen

Mobiele Apps gebruikt de uitbreidingsmethode [LoginAsync] op de [MobileServiceClient] om aan te melden met App Service verificatie van een gebruiker. In dit voorbeeld wordt een stroom authentication server beheerd die van de provider-in interface in de app wordt weergegeven. Voor meer informatie Zie [verificatie Server beheerd](app-service-mobile-dotnet-how-to-use-client-library.md#serverflow). Als u een betere ervaring in uw productie-app, kunt u overwegen in plaats daarvan met [beheerde Client verificatie](app-service-mobile-dotnet-how-to-use-client-library.md#clientflow). 

Als u wilt verifiëren met een project Xamarin.Forms, definieert u een **IAuthenticate** -interface in de draagbare Class Library voor de app. U kunt ook de gebruikersinterface die is gedefinieerd in de draagbare klassebibliotheek een **aanmeld -** knop, waarmee de gebruiker verificatie starten toevoegen bijwerken. Na een succesvolle verificatie worden gegevens geladen vanuit de back-end mobiele app.

U moet voor elk platform ondersteund door uw app de **IAuthenticate** -interface implementeren.


1. In Visual Studio of Xamarin Studio open App.cs van het project met **draagbare** in de naam, draagbare Class Library-project is, voegt u de volgende `using` instructie:

        using System.Threading.Tasks;

2. In App.cs, voeg de volgende `IAuthenticate` interface definition direct voor de `App` klasse definitie.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }

3. De volgende statische leden toevoegen aan de klasse **App** initialiseren van de interface met de specifieke implementatie van een platform.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }

4. TodoList.xaml van de draagbare Class Library-project openen, de volgende **Button** -element in het element van de lay-out *buttonsPanel* na de bestaande knop toevoegen: 

        <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30" 
            Clicked="loginButton_Clicked"/>

    Deze knop activeert de verificatie met de mobiele app-end-server wordt beheerd.

5. TodoList.xaml.cs van de draagbare Class Library-project openen en vervolgens het volgende veld toevoegen aan de `TodoList` klasse:

        // Track whether the user has authenticated. 
        bool authenticated = false;


6. De methode **OnAppearing** vervangen door de volgende code:

        protected override async void OnAppearing()
        {
            base.OnAppearing();
    
            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data 
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    Dit zorgt ervoor dat de gegevens alleen vernieuwd van de service nadat de gebruiker is geverifieerd.

7. De volgende-handler voor de gebeurtenis **Clicked** toevoegen aan de **takenlijst van** klasse:

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }

8. Uw wijzigingen opslaan en opnieuw opbouwen van de draagbare Class Library-project zonder fouten controleren.


##<a name="add-authentication-to-the-android-app"></a>Verificatie op de Android app toevoegen

In dit gedeelte ziet u hoe de **IAuthenticate** -interface implementeren in de Android-app-project. Als u Android-apparaten niet ondersteunt, kunt u deze sectie overslaan.

1. Klik met de rechtermuisknop op het project **droid** , vervolgens **ingesteld als Project opstarten**in Visual Studio of Xamarin Studio.

2. Druk op F5 om het project in de debugger te starten en vervolgens verifiëren dat een niet-verwerkte uitzondering met de statuscode van 401 (Unauthorized) treedt op nadat de toepassing wordt gestart. Dit gebeurt omdat de toegang op de backend is beperkt tot alleen geautoriseerde gebruikers.

3. Open MainActivity.cs in de Android-project en voeg de volgende `using` instructies:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. De klasse **MainActivity** implementatie van de **IAuthenticate** -interface als volgt bijwerken:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate


5. De klasse **MainActivity** door toevoeging van een **MobileServiceUser** veld en een **Authenticate** -methode, die wordt vereist door de **IAuthenticate** -interface als volgt bijwerken:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this,
                    MobileServiceAuthenticationProvider.Facebook);
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }


    Als u een identiteit Facebook-voorziening gebruikt, moet u een andere waarde kiezen voor [MobileServiceAuthenticationProvider].

6. Voeg de volgende code de methode **OnCreate** van de klasse **MainActivity** voor de aanroep `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    Dit zorgt ervoor dat de verificator is geïnitialiseerd voordat de toepassing wordt geladen.

7. De app bouwen, uit te voeren en vervolgens aanmelden met de verificatieprovider u hebt gekozen en controleer of u toegang tot de gegevens als een geverifieerde gebruiker.

##<a name="add-authentication-to-the-ios-app"></a>Verificatie toevoegen aan de iOS-app

In dit gedeelte ziet u hoe de **IAuthenticate** -interface implementeren in de iOS-app-project. Deze sectie overslaan als iOS-apparaten worden niet ondersteund.

1. Klik met de rechtermuisknop op de **iOS** -project **als Project opstarten**in Visual Studio of Xamarin Studio.

2. Druk op F5 om het project in de debugger te starten en vervolgens verifiëren dat een niet-verwerkte uitzondering met de statuscode van 401 (Unauthorized) treedt op nadat de toepassing wordt gestart. Dit gebeurt omdat de toegang op de backend is beperkt tot alleen geautoriseerde gebruikers.

4. Open AppDelegate.cs in het i/o-project en voeg de volgende `using` instructies:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. De klasse **AppDelegate** implementatie van de **IAuthenticate** -interface als volgt bijwerken:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate

5. De klasse **AppDelegate** door toevoeging van een **MobileServiceUser** veld en een **Authenticate** -methode, die wordt vereist door de **IAuthenticate** -interface als volgt bijwerken:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;                        
                    }
                }        
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();         

            return success;
        }

    Als u een identiteit Facebook-voorziening gebruikt, moet u een andere waarde kiezen voor [MobileServiceAuthenticationProvider].

6. Voeg de volgende regel code aan de methode **FinishedLaunching** voor de aanroep naar `LoadApplication()`: 

        App.Init(this);

    Dit zorgt ervoor dat de verificator is geïnitialiseerd voordat de toepassing wordt geladen.

7. De app bouwen, uit te voeren en vervolgens aanmelden met de verificatieprovider u hebt gekozen en controleer of u toegang tot de gegevens als een geverifieerde gebruiker.


##<a name="add-authentication-to-windows-app-projects"></a>Verificatie op Windows app projecten toevoegen

In dit gedeelte ziet u hoe de **IAuthenticate** -interface implementeren in de Windows 8.1 en Windows Phone 8.1 app projecten. Dezelfde stappen gelden voor projecten van Universal Windows Platform (UWP). Als u Windows-apparaten niet ondersteunt, kunt u deze sectie overslaan.

1. Met de rechtermuisknop op de **WinApp** of het **WinPhone81** -project, vervolgens **instellen als StartUp Project**in Visual Studio.

2. Druk op F5 om het project in de debugger te starten en vervolgens verifiëren dat een niet-verwerkte uitzondering met de statuscode van 401 (Unauthorized) treedt op nadat de toepassing wordt gestart. Dit gebeurt omdat de toegang op de backend is beperkt tot alleen geautoriseerde gebruikers.

3. MainPage.xaml.cs openen voor het Windows-app-project en voeg de volgende `using` instructies:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Vervangen `<your_Portable_Class_Library_namespace>` met de naamruimte voor uw draagbare class-bibliotheek.

4. De klasse **MainPage** implementatie van de **IAuthenticate** -interface als volgt bijwerken:

        public sealed partial class MainPage : IAuthenticate


5. De klasse **MainPage** door toevoeging van een **MobileServiceUser** veld en een **Authenticate** -methode, die wordt vereist door de **IAuthenticate** -interface als volgt bijwerken:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }
                
            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }


    Als u een identiteit Facebook-voorziening gebruikt, moet u een andere waarde kiezen voor [MobileServiceAuthenticationProvider].

6. Voeg de volgende regel code in de constructor van de klasse **MainPage** voor de aanroep naar `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);
 
    Vervangen `<your_Portable_Class_Library_namespace>` met de naamruimte voor uw draagbare class-bibliotheek.  
    Als dit het WinApp-project, kunt u doorgaan naar stap 8. De volgende stap geldt alleen voor het WinPhone81-project, waarin u moet voltooien de callback login.

7. (Optioneel) Open App.xaml.cs in de **WinPhone81** -app-project, en voeg de volgende `using` instructies:

        using Microsoft.WindowsAzure.MobileServices;
        using <your_Portable_Class_Library_namespace>;

    Vervangen `<your_Portable_Class_Library_namespace>` met de naamruimte voor uw draagbare class-bibliotheek.

8.  Het overschrijven van de volgende **OnActivated** methode toevoegen aan de klasse **App** :

        protected override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            // We just need to handle activation that occurs after web authentication. 
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Get the client and call the LoginComplete method to complete authentication.
                var client = TodoItemManager.DefaultManager.CurrentClient as MobileServiceClient;
                client.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
        }

    Wanneer de methode overschrijven al bestaat, wordt alleen de voorwaardelijke code toe te voegen uit het bovenstaande fragment.

7. De app bouwen, uit te voeren en vervolgens aanmelden met de verificatieprovider u hebt gekozen en controleer of u toegang tot de gegevens als een geverifieerde gebruiker.

##<a name="next-steps"></a>Volgende stappen

Nu dat u deze zelfstudie basisverificatie hebt voltooid, kunt u door te gaan op een van de volgende cursussen:

+ [Push-meldingen toevoegen aan uw app](app-service-mobile-xamarin-forms-get-started-push.md)  
  Informatie over het toevoegen van push-meldingen en support voor uw app en configureer uw mobiele App backend Azure melding Hubs gebruikt om push-meldingen te verzenden.

+ [Off line synchronisatie van uw app inschakelen](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Informatie over het off line ondersteuning van uw app in de backend voor een mobiele App toevoegen. Off line synchronisatie kan eindgebruikers communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er geen netwerkverbinding is.

<!-- Images. -->

<!-- URLs. -->
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
[LoginAsync]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[MobileServiceClient]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx


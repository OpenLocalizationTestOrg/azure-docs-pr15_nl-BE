<properties
    pageTitle="Aan de slag met melding Hubs voor apps Xamarin.Android | Microsoft Azure"
    description="In deze zelfstudie leert u hoe Azure melding Hubs met push-meldingen te verzenden naar een toepassing Xamarin Android."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter="xamarin"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Aan de slag met melding Hubs met Xamarin voor Android

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Overzicht

In deze zelfstudie wordt beschreven hoe u met Azure melding Hubs push-meldingen te verzenden naar een toepassing Xamarin.Android.
U maakt een lege Xamarin.Android app die push-meldingen ontvangen via Google Cloud Messaging (GCM). Wanneer u klaar bent, zult u uw melding hub gebruiken voor het uitzenden van push-meldingen voor alle apparaten die met uw app. De voltooide code is beschikbaar in de [app NotificationHubs] [ GitHub] monster.

Deze zelfstudie toont de eenvoudige broadcast scenario bij het gebruik van Hubs melding.


## <a name="before-you-begin"></a>Voordat u begint

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

De voltooide code voor deze zelfstudie kunt u vinden op GitHub [hier](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).



##<a name="prerequisites"></a>Vereisten

Deze zelfstudie is het volgende vereist:

+ Visual Studio met Xamarin in Windows of Xamarin Studio op Mac OS X. volledige installatie-instructies zijn voor het [instellen en installeren voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ Actieve Google-account
+ [Azure onderdeel Messaging]
+ [Onderdeel van Google Cloud Messaging-Client]

Voltooien van deze zelfstudie is een vereiste voor een andere melding Hubs zelfstudies voor Xamarin.Android toepassingen.

> [AZURE.IMPORTANT] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).

##<a name="enable-google-cloud-messaging"></a>Google Cloud Messaging inschakelen

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a name="configure-your-notification-hub"></a>Uw melding hub configureren

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li><p>Klik op het tabblad <b>configureren</b> aan de bovenkant, voert u de <b>API-sleutel</b> waarde die u hebt verkregen in de vorige sectie en klik op <b>Opslaan</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)


De hub van uw melding is nu geconfigureerd voor het werken met GCM, en u hebt verbindingsreeksen zowel uw app om meldingen te ontvangen en verzenden push-meldingen registreren.

##<a name="connect-your-app-to-the-notification-hub"></a>Uw app verbinding met de melding hub

###<a name="create-a-new-project"></a>Een nieuw project maken

1. Klik op **Nieuwe oplossing**Xamarin Studio naar **Android App**en klik op **volgende**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Voer uw **Naam** en **id**. Klik op **Doel Plaforms** u wilt ondersteunen en klik op **volgende** en **maken**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)


    Hiermee maakt u een nieuw project voor Android.

2. De Projecteigenschappen openen door met de rechtermuisknop op het nieuwe project in de oplossing weergeven en **Opties**te kiezen. Selecteer het item **Android toepassing** in de sectie **maken** .

    Zorg ervoor dat de eerste letter van de **naam** in kleine letters.

    > [AZURE.IMPORTANT] De eerste letter van de pakketnaam moet worden in kleine letters. Anders ontvangt u kennelijke fouten bij het registreren van uw **BroadcastReceiver** en **IntentFilter** van onderstaande push-meldingen.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)


3. Eventueel de **Minimum Android versie** instelt op een andere API-niveau.

4. Eventueel de **doel Android versie** instelt op een andere API-versie die u wilt richten (moet API niveau 8 of hoger).

Klik op **OK** en sluit het dialoogvenster Opties voor Project.


###<a name="add-the-required-components-to-your-project"></a>De vereiste onderdelen toevoegen aan het project

De Google Cloud Messaging-Client beschikbaar op het onderdeelarchief Xamarin vereenvoudigt het push-meldingen in de Xamarin.Android ondersteunen.

1. Klik met de rechtermuisknop op de map Components in app Xamarin.Android en **Meer onderdelen u**kiest.

2. Het onderdeel **Messaging van Azure** zoeken en toe te voegen aan het project.

3. De component **Google Cloud Messaging Client** zoeken en toe te voegen aan het project.


###<a name="set-up-notification-hubs-in-your-project"></a>Melding hubs in het project instellen

1. Verzamel de volgende informatie voor uw Android hub-app en de melding:

    - **GoogleProjectNumber**: Haal dit projectnummer uit het overzicht van uw app in de Google Developer-Portal. U hebt eerder deze waarde gemaakt wanneer u de toepassing op de portal gemaakt.
    - **Verbindingsreeks luisteren**: klik op het dashboard in de [Klassieke Portal Azure] **verbindingsreeksen weergeven**. Kopieer de *DefaultListenSharedAccessSignature* -verbindingsreeks voor deze waarde.
    - **Hubnaam**: dit is de naam van de hub van de [Klassieke Azure-Portal]. Bijvoorbeeld *mynotificationhub2*.

    Een **Constants.cs** maken voor uw project Xamarin en de volgende constante waarden in de klasse te definiëren. De tijdelijke aanduidingen vervangen door de waarden.

        public static class Constants
        {
            public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
            public const string ListenConnectionString = "<Listen connection string>";
            public const string NotificationHubName = "<hub name>";
        }

2. Voeg de volgende instructies voor **MainActivity.cs**:

        using Android.Util;
        using Gcm.Client;

3. Toevoegen van een instantievariabele met de `MainActivity` -klasse die wordt gebruikt voor het weergeven van een dialoogvenster met een waarschuwing wanneer de toepassing wordt uitgevoerd:

        public static MainActivity instance;


3. De volgende methode in de klasse **MainActivity** te maken:

        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }

4. In de `OnCreate` initialiseren methode van **MainActivity.cs**, de `instance` variabele en voeg een aanroep van `RegisterWithGCM`:

        protected override void OnCreate (Bundle bundle)
        {
            instance = this;

            base.OnCreate (bundle);

            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);

            RegisterWithGCM();
        }


4. Maak een nieuwe klasse **MyBroadcastReceiver**.

    > [AZURE.NOTE] We doorlopen hieronder helemaal zelf een **BroadcastReceiver** klasse maken. Een snel alternatief voor het handmatig maken van **MyBroadcastReceiver.cs** is echter om te verwijzen naar het bestand **GcmService.cs** in het voorbeeldproject Xamarin.Android opgenomen met de [NotificationHubs monsters][GitHub]. **GcmService.cs** kopiëren en wijzigen van namen van klassen is een prima plek om te beginnen ook.

5. Voeg de volgende instructies om de **MyBroadcastReceiver.cs** (die verwijzen naar het onderdeel en de assembly die u eerder hebt toegevoegd):

        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;

5. **MyBroadcastReceiver.cs**, voeg de volgende machtigingen aanvragen tussen de instructies voor het **Gebruik** en de declaratie van de **naamruimte** :

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]

        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]

6. Wijzigen in **MyBroadcastReceiver.cs**, de klasse **MyBroadcastReceiver** overeenkomt met het volgende:

        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };

            public const string TAG = "MyBroadcastReceiver-GCM";
        }

7. Een andere klasse toevoegen aan **MyBroadcastReceiver.cs** met de naam **PushHandlerService**, die is afgeleid van **GcmServiceBase**. Zorg ervoor dat **het kenmerk** van toepassing op de klasse:

        [Service] // Must use the service tag
        public class PushHandlerService : GcmServiceBase
        {
            public static string RegistrationID { get; private set; }
            private NotificationHub Hub { get; set; }

            public PushHandlerService() : base(Constants.SenderID)
            {
                Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
            }
        }


8. **GcmServiceBase** implementeert methoden, **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()**en **OnError()**. Onze **PushHandlerService** implementatieklasse moet deze methoden overschrijven en deze methoden wordt geactiveerd in reactie op de interactie met de melding hub.


9. Overschrijf de methode **OnRegistered()** in **PushHandlerService** met de volgende code:

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            createNotification("PushHandlerService-GCM Registered...",
                                "The device has been Registered!");

            Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                        context);
            try
            {
                Hub.UnregisterAll(registrationId);
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }

            //var tags = new List<string>() { "falcons" }; // create tags if you want
            var tags = new List<string>() {};

            try
            {
                var hubRegistration = Hub.Register(registrationId, tags.ToArray());
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }
        }

    > [AZURE.NOTE] In de bovenstaande code **OnRegistered()** Let u codes registreren voor messaging kanalen instellen.

10. Overschrijf de methode **OnMessage** in **PushHandlerService** met de volgende code:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }

11. De volgende methoden voor **createNotification** en **dialogNotify** aan **PushHandlerService** toevoegen voor de gebruikers te waarschuwen wanneer een bericht wordt ontvangen.

    >[AZURE.NOTE] Kennisgeving ontwerp in Android versie 5.0 en hoger vertegenwoordigt een belangrijke afwijking van die van eerdere versies. Als u deze op Android 5.0 of hoger test, moet de toepassing worden uitgevoerd als de melding wilt ontvangen. Zie [Android-meldingen](http://go.microsoft.com/fwlink/?LinkId=615880)voor meer informatie.

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);

            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;

            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));

            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }

        protected void dialogNotify(String title, String message)
        {

            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }


12. Abstracte leden, **OnUnRegistered()**, **OnRecoverableError()**en **OnError()** negeren zodat de code wordt gecompileerd:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);

            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }

        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);

            return base.OnRecoverableError (context, errorId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }


##<a name="run-your-app-in-the-emulator"></a>Het uitvoeren van uw app in de emulator

Als u dit app in de emulator wordt uitgevoerd, moet u ook een Android virtueel apparaat (AVD) dat Google APIs ondersteunt.

> [AZURE.IMPORTANT] Push-meldingen ontvangen, moet u een Google-account instellen op uw virtuele Android apparaat. (In de emulator, Ga naar **Instellingen** en klik op **Account toevoegen**.) Zorg ervoor dat de emulator is verbonden met het Internet.

>[AZURE.NOTE] Kennisgeving ontwerp in Android versie 5.0 en hoger vertegenwoordigt een belangrijke afwijking van die van eerdere versies. Zie [Android-meldingen](http://go.microsoft.com/fwlink/?LinkId=615880)voor meer informatie.


1. Van **hulpprogramma's**, klikt u op **Open Android Emulator Manager**, selecteer het apparaat en klik op **bewerken**.

    ![][18]

2. Selecteer **Google API's** in **doel**en klik vervolgens op **OK**.

    ![][19]

3. Op de bovenste werkbalk, klik op **uitvoeren**en selecteer uw app. Hiermee start u de emulator en de toepassing wordt uitgevoerd.

  De app het *registratie-id* opgehaald uit GCM en registreert bij de melding hub.

##<a name="send-notifications-from-your-backend"></a>Meldingen verzenden vanuit uw back-end


Ontvangst van meldingen in uw app door het verzenden van meldingen in de [Klassieke Portal Azure] via het tabblad foutopsporing op de hub melding zoals in het onderstaande scherm wordt weergegeven, kunt u testen.

![][30]


Push-meldingen worden normaal gesproken in een back-end services, zoals mobiele diensten of ASP.NET via een compatibele bibliotheek verzonden. U kunt ook de REST API rechtstreeks bericht om berichten te verzenden als een bibliotheek niet beschikbaar voor uw back-end is.

Hier volgt een lijst van andere zelfstudies die u controleren wilt voor het verzenden van meldingen:

- ASP.NET: Zie [Kennisgeving Hubs gebruik push-meldingen aan gebruikers].
- Azure melding Hubs Java SDK: Zie [Hubs melding van Java gebruiken](notification-hubs-java-push-notification-tutorial.md) voor het verzenden van meldingen van Java. Dit is getest in Eclips voor de ontwikkeling van Android.
- PHP: Zie [het gebruik van Hubs melding van PHP](notification-hubs-php-push-notification-tutorial.md).


In de volgende subsecties van de zelfstudie verzenden u meldingen met behulp van een console .NET app en met behulp van een mobiele service via een script van het knooppunt.

####<a name="optional-send-notifications-by-using-a-net-app"></a>(Optioneel) Berichten verzenden met behulp van een .NET-app

In deze sectie sturen we meldingen met behulp van een console .NET app

1. Maak een nieuwe Visual C# consoletoepassing:

    ![][20]

2. Klik op **Extra**in Visual Studio naar **NuGet Package Manager**en klik vervolgens op **Package Manager-Console**.

    Hiermee geeft de Package Manager-Console in Visual Studio.

3. Het **project standaard** ingesteld voor de nieuwe console application-project in het consolevenster met Package Manager en klik in het consolevenster met de volgende opdracht worden uitgevoerd:

        Install-Package Microsoft.Azure.NotificationHubs

    Hiermee wordt een verwijzing naar de Azure melding Hubs SDK met behulp van het <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">pakket Microsoft.Azure.Notification Hubs NuGet</a>toegevoegd.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Open het bestand Program.cs en voeg de volgende `using` instructie:

        using Microsoft.Azure.NotificationHubs;

5. In de `Program` klasse, voegt u de volgende methode. De tijdelijke aanduiding voor tekst met uw *DefaultFullSharedAccessSignature* verbinding string en hub naam bijwerken van het [Klassieke Azure-Portal].

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }

6. Voeg de volgende regels in de **Main** -methode:

         SendNotificationAsync();
         Console.ReadLine();

7. Druk op F5 voor het uitvoeren van de app. U ontvangt een melding in de app.

    ![][21]

####<a name="optional-send-notifications-by-using-a-mobile-service"></a>(Optioneel) Berichten verzenden met behulp van een mobiele service

1. Ga als volgt [aan de slag met mobiele Services].

1. Aanmelden bij de [Klassieke Portal Azure]en selecteer uw mobiele service.

2. Selecteer het tabblad **Taakplanner** op de voorgrond.

    ![][22]

3. Een nieuwe geplande taak maken, Voeg een naam en selecteer **op aanvraag**.

    ![][23]

4. Wanneer de taak is gemaakt, klikt u op de taaknaam van de. Klik vervolgens op het tabblad **Script** op de bovenste balk.

5. Voeg het volgende script binnen de scheduler-functie. Zorg ervoor dat de tijdelijke aanduidingen vervangen door de naam van de hub van uw melding en de verbindingsreeks voor *DefaultFullSharedAccessSignature* die u eerder hebt gekregen. Klik op **Opslaan**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );

6. Klik **Eenmaal uitvoeren** op de onderste balk. U ontvangt een melding toast.

##<a name="next-steps"></a>Volgende stappen

In het volgende eenvoudige voorbeeld u meldingen voor alle Android apparaten uitgezonden. Verwijzen naar de zelfstudie [Gebruiken kennisgeving Hubs op push-meldingen voor gebruikers]om specifieke doelpubliek. Als u wilt dat uw gebruikers door belangengroepen in segmenten, kunt u [Gebruik melding Hubs voor het verzenden van laatste nieuws]lezen. Meer informatie over het gebruik van Hubs kennisgeving in [Kennisgeving Hubs richtlijnen] en in de [Kennisgeving Hubs procedures voor Android].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Aan de slag met mobiele Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[Azure klassieke Portal]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Richtsnoeren voor kennisgeving Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[De procedures voor de kennisgeving Hubs voor Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Melding Hubs gebruiken voor push-meldingen aan gebruikers]: /manage/services/notification-hubs/notify-users-aspnet
[Melding Hubs gebruiken voor het verzenden van laatste nieuws]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Onderdeel van Google Cloud Messaging-Client]: http://components.xamarin.com/view/GCMClient/
[Azure onderdeel Messaging]: http://components.xamarin.com/view/azure-messaging

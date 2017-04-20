<properties
    pageTitle="Push-meldingen toevoegen aan uw app Xamarin.Forms | Microsoft Azure"
    description="Informatie over het Azure services via meerdere platforms push-meldingen verzenden naar uw apps Xamarin.Forms."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Push-meldingen toevoegen aan uw app Xamarin.Forms

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Overzicht

In deze zelfstudie voegt u de push-meldingen voor alle projecten als gevolg van de [Xamarin.Forms snel starten](app-service-mobile-xamarin-forms-get-started.md) zodat een push-bericht wordt verzonden naar alle clients met platforms telkens wanneer een record wordt ingevoegd.

Als u niet het project van de server gedownload snel starten, moet u het pakket push notification-extensie. Zie [werken met de .NET back-endserver SDK voor Azure mobiele toepassingen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) voor meer informatie.

##<a name="prerequisites"></a>Vereisten

* Voor iOS, moet u een [programma van Apple Developer-lidmaatschap](https://developer.apple.com/programs/ios/) en een fysiek i/o-apparaat omdat de [iOS simulator biedt geen ondersteuning voor push-meldingen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

##<a name="configure-hub"></a>Een Hub meldingen configureren

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Update van het serverproject om push-meldingen te verzenden

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]


##<a name="optional-configure-and-run-the-android-project"></a>(Optioneel) Configureren en uitvoeren van het Android project

Voltooi deze sectie als u de push-meldingen voor het project Xamarin.Forms Droid voor Android.


###<a name="enable-firebase-cloud-messaging-fcm"></a>Firebase-Cloud Messaging (FCM) inschakelen

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

###<a name="configure-the-mobile-app-backend-to-send-push-requests-using-fcm"></a>De backend mobiele App voor het verzenden van FCM met push-aanvragen configureren

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

###<a name="add-push-notifications-to-the-android-project"></a>Push-meldingen aan het Android project toevoegen

Met de back-end geconfigureerd met FCM, kunnen we onderdelen en codes op de client registreren bij FCM, registreren voor push-meldingen via de mobiele app-end met Azure melding Hubs en meldingen ontvangen toevoegen.

1. In het project **Droid** , klik met de rechtermuisknop op de map **Components** op **Krijg meer onderdelen...**, zoekt de component **Google Cloud Messaging-Client** en toe te voegen aan het project. Dit onderdeel biedt ondersteuning voor push-meldingen voor een project Xamarin Android.


2. Open het bestand MainActivity.cs en voeg de volgende instructie gebruiken aan het begin van het bestand:

        using Gcm.Client;

3. Voeg de volgende code de methode **OnCreate** na het aanroepen van **LoadApplication**:

        try
        {
            // Check to ensure everything's setup right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }


4. Een nieuwe helper-methode **CreateAndShowDialog** als volgt toevoegen:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }


5. De volgende code toevoegen aan de klasse **MainActivity** :

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    Dit beschrijft het huidige exemplaar van de **MainActivity** , zodat we op de hoofdthread UI uitvoeren kunnen.

6. Initialiseren van de `instance`, variabele aan het begin van de methode **OnCreate** als volgt.

        // Set the current instance of MainActivity.
        instance = this;

2. Een nieuwe klassebestand toevoegen aan de **Droid** -project met de naam `GcmService.cs`, en zorg ervoor dat de volgende instructies voor **Gebruik** aan het begin van het bestand aanwezig zijn:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;


9. Toevoegen aan het begin van het bestand de volgende machtigingen aanvragen na de overzichten **met** en voor de declaratie van de **naamruimte** .

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]

10. De volgende klassendefinitie toevoegen aan de naamruimte. 

        [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
        public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
        {
            public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
        }

    >[AZURE.NOTE]**< PROJECT_NUMBER >** vervangen door het projectnummer van uw dat u eerder hebt opgeschreven.   

11. Vervang de lege **GcmService** -klasse met de volgende code van de nieuwe ontvanger broadcast gebruikt:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }


12. De volgende code toevoegen aan de klasse **GcmService** die overschrijft de **OnRegistered** gebeurtenis-handler en wordt een methode voor het **registreren** .

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

        Note that this code uses the `messageParam` parameter in the template registration. 

13. Voeg de volgende code **OnMessage implementeert**: 

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    Deze binnenkomende berichten worden verwerkt en stuur ze naar de notification manager wordt weergegeven.

14. **GcmServiceBase** moet u de **OnUnRegistered** en **OnError** -handler methoden die u kunt als volgt implementeren:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

U bent nu gereed test push-meldingen in de app op een Android-apparaat of de emulator.

###<a name="test-push-notifications-in-your-android-app"></a>Test push-meldingen in de Android app

De eerste twee stappen zijn vereist, alleen bij het testen op een emulator.

1. Zorg ervoor dat u wilt implementeren of foutopsporing op een virtueel apparaat met Google APIs ingesteld als het doel, zoals hieronder wordt weergegeven in beheer van Android virtueel apparaat (AVD).

2. Een Google-account toevoegen aan het Android apparaat door te klikken op **Apps** > **Instellingen** > **account toevoegen**en volg de aanwijzingen voor het gebruik van een bestaande Google-account toevoegen aan het apparaat naar een nieuwe maken.

1. Klik met de rechtermuisknop op het project **Droid** in Visual Studio of Xamarin Studio en klikt u op **opstartproject**.

2. Druk op de knop **uitvoeren** voor het bouwen van het project en start de app op uw Android-apparaat of de emulator.

3. Typ een taak in de app, en klik vervolgens op het plusteken (**+**) pictogram.

4. Controleren of een kennisgeving is ontvangen wanneer een item is toegevoegd.


##<a name="optional-configure-and-run-the-ios-project"></a>(Optioneel) Configureren en uitvoeren van het project iOS

Deze sectie is voor het uitvoeren van het project Xamarin iOS voor iOS-apparaten. Als u niet met iOS apparaten werkt, kunt u deze sectie overslaan.

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]


####<a name="configure-the-notification-hub-for-apns"></a>De hub meldingen configureren voor APNS

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Vervolgens configureert u de instelling van de iOS-project in Xamarin Studio of Visual Studio.

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]


####<a name="add-push-notifications-to-your-ios-app"></a>Push-meldingen toevoegen aan uw iOS-app

1. Open in het project **iOS** AppDelegate.cs met de volgende instructie voor **Gebruik** toevoegen aan de bovenkant van het codebestand.

        using Newtonsoft.Json.Linq;

4. Voeg toe een vervanging voor de **RegisteredForRemoteNotifications** -gebeurtenis registreren voor meldingen in de klasse **AppDelegate** :

        public override void RegisteredForRemoteNotifications(UIApplication application, 
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }

5. In **AppDelegate**, het overschrijven van de gebeurtenis-handler **DidReceivedRemoteNotification** van de volgende items toevoegen:

        public override void DidReceiveRemoteNotification(UIApplication application, 
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Deze methode zorgt voor binnenkomende berichten terwijl de toepassing wordt uitgevoerd.

2. Voeg de volgende code de methode **FinishedLaunching** in de klasse **AppDelegate** : 

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Dit biedt ondersteuning voor externe meldingen en aanvragen voor push-registratie.

De app is nu bijgewerkt ter ondersteuning van push-meldingen.

####<a name="test-push-notifications-in-your-ios-app"></a>Test push-meldingen in de iOS-app

1. Klik met de rechtermuisknop op het project iOS en klik op **instellen als StartPp Project**.

2. Druk op de knop **uitvoeren** of **F5** in Visual Studio voor het bouwen van het project en de app starten in een iOS-apparaat en klik vervolgens op **OK** om te accepteren van push-meldingen.

    > [AZURE.NOTE] U moet expliciet push-meldingen accepteren van uw app. Deze aanvraag komt alleen voor de eerste keer dat de toepassing wordt uitgevoerd.

3. Typ een taak in de app, en klik vervolgens op het plusteken (**+**) pictogram.

4. Controleer of dat een bericht wordt ontvangen en klik vervolgens op **OK** om het bericht te sluiten.


##<a name="optional-configure-and-run-the-windows-projects"></a>(Optioneel) Configureren en uitvoeren van het Windows-projecten

Deze sectie is voor het uitvoeren van de Xamarin.Forms WinApp en WinPhone81 projecten voor Windows-apparaten. Deze stappen ook ondersteunen Universal Windows Platform (UWP)-projecten. Als u niet met Windows-apparaten werkt kunt u deze sectie overslaan.


####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Uw Windows-app voor push-meldingen registreren bij WNS

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]


####<a name="configure-the-notification-hub-for-wns"></a>De hub meldingen configureren voor WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]


####<a name="add-push-notifications-to-your-windows-app"></a>Push-meldingen toevoegen aan uw Windows-app

1. **App.xaml.cs** openen in Windows-project in Visual Studio en toevoegen met de volgende instructies voor **Gebruik** .

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Vervangen `<your_TodoItemManager_portable_class_namespace>` met de naamruimte van uw draagbare project met de `TodoItemManager` klasse.
 

2. Voeg de volgende **InitNotificationsAsync** -methode App.xaml.cs: 

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS = 
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Deze methode haalt de push notification kanaal en sjabloon Sjabloonmeldingen ontvangen van uw melding hub registreert. Een sjabloon melding die *messageParam ondersteunt* worden naar deze client bezorgd.

3. Werk in de App.xaml.cs, de definitie van de **OnLaunched** gebeurtenis-handler methode door het toevoegen van de `async` Fonetisch, voeg de volgende regel code toe aan het einde van de methode: 

        await InitNotificationsAsync();

    Dit zorgt ervoor dat de registratie van push melding wordt gemaakt of vernieuwd telkens wanneer de toepassing wordt gestart. Het is belangrijk om te kunnen garanderen dat het kanaal WNS push altijd actief is.  

4. Open het bestand **Package.appxmanifest** in de Solution Explorer van Visual Studio en **Toast staat** ingesteld op **Ja** onder **meldingen**.

5. De app bouwen en controleer of er geen fouten.  U client app dient nu te registreren voor de voor Sjabloonmeldingen van de mobiele App-end. Herhaal deze sectie voor elk Windows-project in de oplossing.


####<a name="test-push-notifications-in-your-windows-app"></a>Test push-meldingen in uw Windows-app

1. In Visual Studio, Windows-project met de rechtermuisknop op en klikt u op **opstartproject**.

2. Druk op de knop **uitvoeren** voor het bouwen van het project en de app start.

3. Typ een naam voor een nieuwe todoitem in de app, en klik op het plusteken (**+**) pictogram toe te voegen.

4. Controleren of een kennisgeving is ontvangen wanneer het item wordt toegevoegd.

##<a name="next-steps"></a>Volgende stappen

Meer informatie over push-meldingen:

* [Vaststellen van problemen met push notification](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Er zijn verschillende redenen waarom berichten kunnen ophalen neergezet of sluit niet af op apparaten. In dit onderwerp wordt beschreven hoe u te analyseren en de hoofdoorzaak van push notification storingen uitzoeken. 

Kunt u door te gaan op een van de volgende cursussen:

* [Verificatie toevoegen aan uw app](app-service-mobile-xamarin-forms-get-started-users.md)  
Informatie over het verifiëren van gebruikers van uw toepassing met een id-provider.

* [Off line synchronisatie van uw app inschakelen](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Informatie over het off line ondersteuning van uw app in de backend voor een mobiele App toevoegen. Off line synchronisatie kan eindgebruikers communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er geen netwerkverbinding is.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333


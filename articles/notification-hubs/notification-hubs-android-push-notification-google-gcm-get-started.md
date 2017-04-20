<properties
    pageTitle="Push-meldingen verzenden naar Android met Azure melding Hubs | Microsoft Azure"
    description="In deze zelfstudie leert u Azure melding Hubs gebruiken voor push-meldingen voor Android apparaten."
    services="notification-hubs"
    documentationCenter="android"
    keywords="push-meldingen push-bericht android push-bericht"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="07/05/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-android-with-azure-notification-hubs"></a>Push-meldingen verzenden naar Android met Azure melding Hubs

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Overzicht

> [AZURE.IMPORTANT] In dit onderwerp ziet u push-meldingen met Google Cloud Messaging (GCM). Als u gebruikmaakt van Google Firebase Cloud Messaging (FCM), Zie [push-meldingen verzenden naar Android met Azure melding Hubs en FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).

In deze zelfstudie wordt beschreven hoe u met Azure melding Hubs push-meldingen te verzenden naar een toepassing voor Android.
U maakt een lege Android app die push-meldingen ontvangen via Google Cloud Messaging (GCM).

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

De voltooide code voor deze zelfstudie kunt u downloaden van GitHub [hier](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStarted).


##<a name="prerequisites"></a>Vereisten

> [AZURE.IMPORTANT] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).

Naast een actieve Azure account hierboven vermeld, moet deze zelfstudie alleen de nieuwste versie van [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).

Voltooien van deze zelfstudie is een vereiste voor een andere melding Hubs zelfstudies voor Android apps.

##<a name="creating-a-project-that-supports-google-cloud-messaging"></a>Een project maken ondersteunt Google Cloud Messaging

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]


##<a name="configure-a-new-notification-hub"></a>Een nieuwe kennisgeving hub configureren


[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. in de **Instellingen** -blade Selecteer **Notification Services** en **Google (GCM)**. De API-sleutel en klik op **Opslaan**.

&emsp;&emsp;![Azure melding Hubs - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

De hub van uw melding is nu geconfigureerd voor het werken met GCM, en u hebt verbindingsreeksen zowel uw app voor het ontvangen en verzenden van push-meldingen registreren.

##<a id="connecting-app"></a>Uw app verbinding met de melding hub

###<a name="create-a-new-android-project"></a>Maak een nieuw project voor Android

1. Start een nieuwe Android Studio-project in Android Studio.

    ![Android Studio - nieuw project][13]

2. Kies de **Telefoon en Tablet** form factor en de **Minimum SDK** die u wilt ondersteunen. Klik vervolgens op **volgende**.

    ![Android Studio - project maken workflow][14]

3. **Lege activiteit** kiezen voor de belangrijkste activiteit, klik op **volgende**en klik vervolgens op **Voltooien**.

###<a name="add-google-play-services-to-the-project"></a>Google Play services toevoegen aan het project

[AZURE.INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

###<a name="adding-azure-notification-hubs-libraries"></a>Azure melding Hubs bibliotheken toevoegen


1. In de `Build.Gradle` voor de **app**bestand, voeg de volgende regels in de sectie **afhankelijkheden** .

        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'

2. De volgende opslagplaats toevoegen na de sectie **dependencies** .

        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-the-androidmanifestxml"></a>De AndroidManifest.xml wordt bijgewerkt.


1. Ter ondersteuning van GCM, moeten we een exemplaar-ID listener-service implementeren in onze code die wordt gebruikt voor het [verkrijgen van tokens voor registratie](https://developers.google.com/cloud-messaging/android/client#sample-register) [Van Google exemplaar-id-API](https://developers.google.com/instance-id/)gebruiken. In deze zelfstudie wordt we de klasse naam `MyInstanceIDService`. 
 
    De volgende servicedefinitie toevoegen aan het bestand AndroidManifest.xml binnen de `<application>` code. Vervang de `<your package>` tijdelijke aanduiding met de uw werkelijke naam weergegeven aan de bovenkant van de `AndroidManifest.xml` bestand.

        <service android:name="<your package>.MyInstanceIDService" android:exported="false">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID"/>
            </intent-filter>
        </service>


2. Nadat we onze registratietoken GCM van de API van exemplaar-ID hebt ontvangen, gebruiken we deze registreren [bij de melding Azure Hub](notification-hubs-push-notification-registration-management.md). We deze registratie zal ondersteunen in de achtergrond met behulp van een `IntentService` met de naam `RegistrationIntentService`. Deze service is ook verantwoordelijk voor het [vernieuwen van onze registratietoken GCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).
 
    De volgende servicedefinitie toevoegen aan het bestand AndroidManifest.xml binnen de `<application>` code. Vervang de `<your package>` tijdelijke aanduiding met de uw werkelijke naam weergegeven aan de bovenkant van de `AndroidManifest.xml` bestand. 

        <service
            android:name="<your package>.RegistrationIntentService"
            android:exported="false">
        </service>



3. Ook definiëren we een ontvanger als u wilt ontvangen. De volgende definitie voor de ontvanger binnen toevoegen aan het bestand AndroidManifest.xml de `<application>` code. Vervang de `<your package>` tijdelijke aanduiding met de uw werkelijke naam weergegeven aan de bovenkant van de `AndroidManifest.xml` bestand.

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>



4. Voeg de volgende nodig GCM verband houden machtigingen onder de `</application>` code. Vervang `<your package>` met de pakketnaam weergegeven aan de bovenkant van de `AndroidManifest.xml` bestand.

    Zie voor meer informatie over deze machtigingen [Setup een Client GCM-app voor Android](https://developers.google.com/cloud-messaging/android/client#manifest).

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

        <permission android:name="<your package>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
        <uses-permission android:name="<your package>.permission.C2D_MESSAGE"/>


### <a name="adding-code"></a>Code toe te voegen


1. Vouw in de weergave Project **app** > **src** > **belangrijkste** > **java**. Klik met de rechtermuisknop op de pakketmap onder **java**, klikt u op **Nieuw**en klik op **Java-klasse**. Toevoegen van een nieuwe klasse met de naam `NotificationSettings`. 

    ![Android Studio - nieuwe Java-klasse][6]

    Zorg ervoor dat deze drie tijdelijke aanduidingen in de volgende code voor de `NotificationSettings` klasse:
    * **SenderId**: het project dat u eerder in de [Google wolk Console](http://cloud.google.com/console).
    * **HubListenConnectionString**: de **DefaultListenAccessSignature** -verbindingsreeks voor de hub. Deze verbindingsreeks kunt u kopiëren door te klikken op **-Beleid** op het blad van de **Instellingen** van uw hub op de [Azure Portal].
    * **HubName**: de naam van uw hub-melding die wordt weergegeven in de blade hub in [Azure Portal]gebruiken.

    `NotificationSettings`code:

        public class NotificationSettings {
            public static String SenderId = "<Your project number>";
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Your default listen connection string>";
        }

2. Met behulp van de bovenstaande stappen toevoegen een andere nieuwe klasse met de naam `MyInstanceIDService`. Dit is onze implementatie van exemplaar-ID listener-service.

    De code voor deze klasse belt ons `IntentService` [de GCM-token vernieuwen](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) op de achtergrond.

        import android.content.Intent;
        import android.util.Log;
        import com.google.android.gms.iid.InstanceIDListenerService;
        
        
        public class MyInstanceIDService extends InstanceIDListenerService {
        
            private static final String TAG = "MyInstanceIDService";
        
            @Override
            public void onTokenRefresh() {
        
                Log.i(TAG, "Refreshing GCM Registration Token");
        
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


3. Een andere nieuwe klasse toevoegen aan uw project met de naam `RegistrationIntentService`. Dit is de uitvoering voor onze `IntentService` die [de GCM-token te vernieuwen](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) en te [registreren met de hub kennisgeving](notification-hubs-push-notification-registration-management.md)zal verwerken.

    Gebruik de volgende code voor deze klasse.

        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;
        
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.google.android.gms.iid.InstanceID;
        import com.microsoft.windowsazure.messaging.NotificationHub;
        
        public class RegistrationIntentService extends IntentService {
        
            private static final String TAG = "RegIntentService";
        
            private NotificationHub hub;
        
            public RegistrationIntentService() {
                super(TAG);
            }
        
            @Override
            protected void onHandleIntent(Intent intent) {      
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
        
                try {
                    InstanceID instanceID = InstanceID.getInstance(this);
                    String token = instanceID.getToken(NotificationSettings.SenderId,
                            GoogleCloudMessaging.INSTANCE_ID_SCOPE);        
                    Log.i(TAG, "Got GCM Registration Token: " + token);
        
                    // Storing the registration id that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if ((regID=sharedPreferences.getString("registrationID", null)) == null) {      
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.i(TAG, "Attempting to register with NH using token : " + token);

                        regID = hub.register(token).getRegistrationId();

                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1", "tag2").getRegistrationId();

                        resultString = "Registered Successfully - RegId : " + regID;
                        Log.i(TAG, resultString);       
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                    } else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete token refresh", e);
                    // If an exception happens while fetching the new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt the update at a later time.
                }
        
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }


        
4. In de `MainActivity` klasse, voeg de volgende `import` instructies boven de klassendeclaratie.

        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.google.android.gms.gcm.*;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;

5. De volgende persoonlijke leden toevoegen aan de bovenkant van de klasse. We gebruiken deze [controleren de beschikbaarheid van Google Play Services zoals aanbevolen door Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).

        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private GoogleCloudMessaging gcm;
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;

6. In de `MainActivity` klasse, voegt u de volgende methode om de beschikbaarheid van Google Play Services. 

        /**
         * Check the device to make sure it has the Google Play Services APK. If
         * it doesn't, display a dialog that allows users to download the APK from
         * the Google Play Store or enable it in the device's system settings.
         */
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }


7. In de `MainActivity` klasse, voeg de volgende code waarmee wordt gecontroleerd voor Google Play Services voordat u uw `IntentService` uw inschrijving GCM token ophalen en registreren met de hub van uw melding.

        public void registerWithNotificationHubs()
        {
            Log.i(TAG, " Registering with Notification Hubs");
    
            if (checkPlayServices()) {
                // Start IntentService to register this application with GCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }


8. In de `OnCreate` methode van de `MainActivity` klasse, voeg de volgende code om het registratieproces te starten wanneer de activiteit wordt gemaakt.

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }


9. Deze aanvullende methoden voor het toevoegen de `MainActivity` om te controleren of de staat en het rapport status app in de app.

        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
    
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
    
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
    
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
    
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }


10. De `ToastNotify` methode maakt gebruik van de *"Hello World"* `TextView` rapportstatus en meldingen permanent in de app. Voeg de volgende id voor dat besturingselement in activity_main.xml-indeling.

        android:id="@+id/text_hello"

11. We vervolgens toevoegen een subklasse voor de ontvanger die is gedefinieerd in de AndroidManifest.xml. Een andere nieuwe klasse toevoegen aan uw project met de naam `MyHandler`.

12. De volgende importinstructies toevoegen aan de bovenkant van `MyHandler.java`:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;

13. Voeg de volgende code voor de `MyHandler` maken van een subklasse van de klasse `com.microsoft.windowsazure.notifications.NotificationsHandler`.

    Deze code vervangt de `OnReceive` methode, zodat de handler verslag meldingen die worden ontvangen. De handler ook stuurt de push-bericht naar de Android notification manager met behulp van de `sendNotification()` methode. De `sendNotification()` methode moet worden uitgevoerd wanneer de toepassing wordt niet uitgevoerd en wordt een melding ontvangen.

        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
        
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
        
            private void sendNotification(String msg) {
        
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
        
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
        
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
        
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
        
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }


14. Klik op **Opbouwen**in Android Studio in het menu > **Project opnieuw** om ervoor te zorgen dat er geen fouten in uw code aanwezig zijn.

##<a name="sending-push-notifications"></a>Push-meldingen te verzenden

Push-meldingen ontvangen in uw app door deze te verzenden via de [Portal Azure] - Zoek de sectie **Probleemoplossing** in het blad hub, zoals hieronder wordt weergegeven, kunt u testen.

![Azure melding Hubs - Test verzenden](./media/notification-hubs-android-get-started/notification-hubs-test-send.png)

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-the-app"></a>(Optioneel) Push-meldingen verzenden direct vanuit de app.

Normaal gesproken zou u meldingen met behulp van een back endserver verzenden. Voor sommige gevallen kunt u push-meldingen direct van de clienttoepassing versturen. In dit gedeelte wordt uitgelegd hoe u meldingen wilt verzenden vanaf de client met de [Azure melding Hub REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).

1. Vouw in Android Studio Project-weergave **App** > **src** > **belangrijkste** > **res** > **lay-out**. Open de `activity_main.xml` bestandsindeling en klik op de **tekst** tab om de tekstinhoud van het bestand bijwerken. Werken met de onderstaande code waarmee nieuw toegevoegd `Button` en `EditText` besturingselementen voor het verzenden van push meldingen op de hub melding. Vlak voordat deze code toevoegen aan de onderkant, `</RelativeLayout>`.

        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />

        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />

2. Vouw in Android Studio Project-weergave **App** > **src** > **belangrijkste** > **res** > **waarden**. Open de `strings.xml` het bestand en voeg de string-waarden waarnaar wordt verwezen door de nieuwe `Button` en `EditText` besturingselementen. Toevoegen als u deze op de onderkant van het bestand, vlak voor `</resources>`.

        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>


3. In de `NotificationSetting.java` bestand, voeg de volgende instelling voor de `NotificationSettings` klasse.

    Update `HubFullAccess` met de **DefaultFullSharedAccessSignature** -verbindingsreeks voor de hub. Deze verbindingsreeks kan worden gekopieerd vanaf de [Portal Azure] **-Beleid** door op te klikken het blad van de **Instellingen** voor de hub van uw melding.

        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccess Connection string>";

4. In de `MainActivity.java` bestand, voeg de volgende `import` hierboven genoemde instructies de `MainActivity` klasse.

        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;

6. In de `MainActivity.java` het bestand, de volgende leden toevoegen aan de bovenkant van de `MainActivity` klasse.  

        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;

6. U moet een token Software toegang handtekening (SaS) voor het verifiëren van een POST-aanvraag om berichten te verzenden naar uw hub-melding maken. Dit wordt gedaan door de belangrijkste gegevens uit de verbindingsreeks parseren en vervolgens het token SaS maken zoals vermeld in de [Algemene begrippen](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API reference. De volgende code is een voorbeeld van de implementatie.

    In `MainActivity.java`, voegt u de volgende methode om de `MainActivity` klasse parseren van de verbindingsreeks.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * to parse the connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be the DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
    
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }


7. In `MainActivity.java`, voegt u de volgende methode om de `MainActivity` te maken van een SaS-verificatietoken klasse.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from the access key to authenticate a request.
         *
         * @param uri The unencoded resource URI string for this operation. The resource
         *            URI is the full URI of the Service Bus resource to which access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
    
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
    
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
    
                // Get an hmac_sha1 key from the raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
    
                // Get an hmac_sha1 Mac instance and initialize with the signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
    
                // Compute the hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
    
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
    
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
    
            return token;
        }



8. In `MainActivity.java`, voegt u de volgende methode om de `MainActivity` klasse verwerken het **Bericht verzenden** te klikken en de push-bericht verzenden naar de hub via de ingebouwde REST API.

        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added to the Authorization header on the POST request to the
         * notification hub. The text in the editTextNotificationMessage control
         * is added as the JSON body for the request to add a GCM message to the hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
    
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
    
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
    
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
    
                            // Authenticate the POST request with the SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
    
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
    
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //      "tag1 || tag2 || tag3");
    
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
    
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }

                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }



##<a name="testing-your-app"></a>Testen van uw app

####<a name="push-notifications-in-the-emulator"></a>Push-meldingen in de emulator

Als u wilt testen in de emulator een push-meldingen, zorg ervoor dat uw image emulator ondersteuning biedt voor de Google API die u hebt gekozen voor uw app. Als de afbeelding geen eigen Google APIs ondersteunt, worden er met de **SERVICE\_niet\_beschikbaar** uitzondering.

Zorg ervoor dat u uw Google-account hebt toegevoegd aan de actieve emulator onder **Instellingen**naast het bovenstaande > **rekeningen**. Anders wordt uw registreren bij GCM kan resulteren in de **verificatie\_mislukt** uitzondering.

####<a name="running-the-application"></a>De toepassing wordt uitgevoerd.

1. Start de app en merk op dat de registratie-ID voor een geslaagde registratie wordt gerapporteerd.

    ![Testen op Android - kanaal registratie][18]

2. Geef een melding moet worden verzonden naar alle Android-apparaten die zijn geregistreerd bij de hub.

    ![Testen op Android - bericht verzenden][19]

3. Drukt u op **melding verzenden**. Geeft alle apparaten die de app met een `AlertDialog` exemplaar met de push-melding. Apparaten die niet de app actief, maar eerder zijn geregistreerd voor push-meldingen ontvangt een melding in Android Notification Manager. Die kunnen worden bekeken door de lezer naar beneden in de linkerbovenhoek.

    ![Testen op Android - meldingen][21]

##<a name="next-steps"></a>Volgende stappen

Het is raadzaam de zelfstudie [Gebruiken kennisgeving Hubs op push-meldingen aan gebruikers] als volgende stap. Het wordt beschreven hoe u meldingen verzenden vanuit een ASP.NET backend tags gebruiken om specifieke gebruikers.

Als u wilt dat uw gebruikers door belangengroepen in segmenten, controleren van de zelfstudie [Gebruiken kennisgeving Hubs voor het laatste nieuws te verzenden] .

Zie voor meer algemene informatie over aanmelding Hubs, onze [Richtlijnen voor kennisgeving Hubs].

<!-- Images. -->
[6]: ./media/notification-hubs-android-get-started/notification-hub-android-new-class.png

[12]: ./media/notification-hubs-android-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-new-project.png
[14]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-choose-form-factor.png
[15]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app4.png
[16]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app5.png
[17]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app6.png

[18]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-registered.png
[19]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-set-message.png

[20]: ./media/notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-received-message.png
[22]: ./media/notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/notification-hubs-android-get-started/notification-hub-scheduler2.png
[29]: ./media/mobile-services-android-get-started-push/mobile-eclipse-import-Play-library.png

[30]: ./media/notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png

[31]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-add-ui.png


<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[Richtsnoeren voor kennisgeving Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Melding Hubs gebruiken voor push-meldingen aan gebruikers]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Melding Hubs gebruiken voor het verzenden van laatste nieuws]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure Portal]: https://portal.azure.com

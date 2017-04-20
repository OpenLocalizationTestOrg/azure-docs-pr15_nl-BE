1. Open het bestand in uw project **app** `AndroidManifest.xml`. Vervang in de code in de volgende twee stappen, _`**my_app_package**`_ met de naam van het pakket app voor uw project, is de waarde van de `package` kenmerk van de `manifest` code.

2. De volgende nieuwe machtigingen toevoegen na de bestaande `uses-permission` element:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Voeg de volgende code na de `application` code openen:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                        android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>


4. Open het bestand *ToDoActivity.java*en toevoegen van de volgende aanwijzing import:

        import com.microsoft.windowsazure.notifications.NotificationsManager;


5. De volgende persoonlijke variabele toevoegen aan de klasse: vervangen _`<PROJECT_NUMBER>`_ Project nummer toegewezen door Google aan uw app in de voorgaande procedure:

        public static final String SENDER_ID = "<PROJECT_NUMBER>";

6. Wijzig de definitie van de *MobileServiceClient* van **privé** in **openbare statische**, zodat er nu als volgt uitziet:

        public static MobileServiceClient mClient;

7. Vervolgens moet een nieuwe klasse meldingen toevoegen. Open in de Projectverkenner het **src** => **belangrijkste** => **java** knooppunten en met de rechtermuisknop op het knooppunt package-naam: klik op **Nieuw**en klik op **Java-klasse**.

8. Typ **de naam** in `MyHandler`, klikt u op **OK**.


    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)


9. Vervang de klassendeclaratie met in het bestand MyHandler

        public class MyHandler extends NotificationsHandler {


10. Toevoegen van de volgende importinstructies voor de `MyHandler` klasse:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;


11. Naast dit lid toevoegen aan de `MyHandler` klasse:

        public static final int NOTIFICATION_ID = 1;


12. In de `MyHandler` klasse, voeg de volgende code de methode **onRegistered** , die het apparaat met de mobiele service melding Hub registreert overschrijven.

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
            super.onRegistered(context, gcmRegistrationId);

            new AsyncTask<Void, Void, Void>() {

                protected Void doInBackground(Void... params) {
                    try {
                        ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                        return null;
                    }
                    catch(Exception e) {
                        // handle error             
                    }
                    return null;            
                }
            }.execute();
        }


13. In de `MyHandler` klasse, voeg de volgende code om het overschrijven van de methode **onReceive** , die ervoor zorgt de melding wordt weergegeven dat wanneer deze is ontvangen.

        @Override
        public void onReceive(Context context, Bundle bundle) {
                String msg = bundle.getString("message");

                PendingIntent contentIntent = PendingIntent.getActivity(context,
                        0, // requestCode
                        new Intent(context, ToDoActivity.class),
                        0); // flags

                Notification notification = new NotificationCompat.Builder(context)
                        .setSmallIcon(R.drawable.ic_launcher)
                        .setContentTitle("Notification Hub Demo")
                        .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                        .setContentText(msg)
                        .setContentIntent(contentIntent)
                        .build();

                NotificationManager notificationManager = (NotificationManager)
                        context.getSystemService(Context.NOTIFICATION_SERVICE);
                notificationManager.notify(NOTIFICATION_ID, notification);
        }


14. Terug in het bestand TodoActivity.java bijwerken met de methode **onCreate** van de klasse *ToDoActivity* melding handler klasse registreren. Zorg ervoor dat deze code toevoegen nadat de *MobileServiceClient* is gestart.


        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    De app is nu bijgewerkt ter ondersteuning van push-meldingen.

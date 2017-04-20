<properties
    pageTitle="Melding Hubs belangrijk nieuws zelfstudie - Android"
    description="Leren werken met Azure Service Bus melding Hubs recente nieuws om meldingen te verzenden naar apparaten met Android."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>


# <a name="use-notification-hubs-to-send-breaking-news"></a>Melding Hubs gebruiken voor het verzenden van laatste nieuws

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

##<a name="overview"></a>Overzicht

In dit onderwerp wordt beschreven hoe u Azure melding Hubs gebruiken voor het uitzenden van recente nieuws meldingen naar een Android app. Als alles klaar is, u kunnen registreren voor het breken van nieuwscategorieën waarin u geïnteresseerd bent, en alleen push meldingen ontvangen voor deze categorieën. Dit scenario is een gemeenschappelijk patroon voor veel toepassingen waar meldingen moeten worden verzonden naar groepen van gebruikers die eerder gedeclareerd belangstelling voor hen, zoals RSS-lezer, apps voor muziek, ventilatoren, enz.

Broadcast-scenario's zijn ingeschakeld door een of meer _tags_ bij het maken van een inschrijving in de kennisgeving hub. Wanneer berichten worden verzonden naar een tag, ontvangt alle apparaten die zijn geregistreerd voor de code de melding. Omdat codes gewoon tekenreeksen zijn, hoeven niet van tevoren worden ingericht. Raadpleeg [melding Hubs en Tag expressies](notification-hubs-tags-segment-push-message.md)voor meer informatie over tags.


##<a name="prerequisites"></a>Vereisten

Dit onderwerp is gebaseerd op de toepassing die u hebt gemaakt in het [aan de slag met Hubs melding][get-started]. Voordat u deze zelfstudie begint, moet reeds hebt voltooid [aan de slag met Hubs melding][get-started].

##<a name="add-category-selection-to-the-app"></a>Categorieselectie toevoegen aan de app.

De eerste stap is de gebruikersinterface-elementen toevoegen aan uw bestaande hoofdactiviteit waarmee de gebruiker kan de categorieën selecteren om te registreren. De categorieën die zijn geselecteerd door een gebruiker zijn op het apparaat opgeslagen. Wanneer de toepassing wordt gestart, wordt de apparaatregistratie van een in uw melding hub met geselecteerde categorieën als codes gemaakt.

1. Open het bestand res/layout/activity_main.xml en vervangen door de inhoud met de volgende:

        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">

                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>

2. Open het bestand res/values/strings.xml en voeg de volgende regels:

        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>

    De grafische indeling van de main_activity.xml ziet er nu als volgt:

    ![][A1]

3. Maak nu een klasse **meldingen** in hetzelfde pakket als de klasse **MainActivity** .

        import java.util.HashSet;
        import java.util.Set;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;

        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;

            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
        
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }

            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }

            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }

            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
        
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
        
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
        
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }

        }

    Deze klasse wordt de lokale opslag gebruikt voor het opslaan van de categorieën van het nieuws dat dit apparaat heeft ontvangen. Het bevat ook methoden te registreren voor deze categorieën.


4. In de klasse **MainActivity** private-velden voor **NotificationHub** en **GoogleCloudMessaging**verwijderen en toevoegen van een veld voor **meldingen**:

        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;

5. Verwijder vervolgens de initialisatie van de **hub** en de methode **registerWithNotificationHubs** in de **onCreate** -methode. Voeg vervolgens de volgende regels die een instantie van de klasse **meldingen** worden geïnitialiseerd. 


        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;
    
            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);
    
            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);
    
            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`en `HubListenConnectionString` moet al zijn ingesteld met de `<hub name>` en `<connection string with listen access>` tijdelijke aanduidingen met de naam van de hub van uw melding en de verbindingsreeks voor de *DefaultListenSharedAccessSignature* die u eerder hebt gekregen.

    > [AZURE.NOTE] Omdat de referenties die worden gedistribueerd met een client-app niet in het algemeen veilig zijn, moet u de sleutel voor het luisteren naar access alleen met uw app client distribueren. Access maakt de app te registreren voor meldingen, maar bestaande registraties kan niet worden gewijzigd luisteren en meldingen kunnen niet worden verzonden. De volledige toegang tot de sleutel wordt gebruikt in een beveiligde back-end-service voor het verzenden van meldingen en bestaande registraties wijzigen.


6. Voeg vervolgens de volgende invoer en `subscribe` methode voor het verwerken van de knop abonneren klikt u op de gebeurtenis:
        
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;

        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();

            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");

            notifications.storeCategoriesAndSubscribe(categories);
        }

    Deze methode maakt een lijst met categorieën en de **meldingen** klasse gebruikt de lijst opslaan in de lokale opslag en de corresponderende codes registreren met de hub van uw melding. Wanneer categorieën worden gewijzigd, wordt de registratie opnieuw gemaakt met de nieuwe categorieën.

Uw app kan nu een set categorieën worden opgeslagen in lokale opslag op het apparaat en registreren met de hub melding wanneer de gebruiker de selectie van categorieën wijzigt.

##<a name="register-for-notifications"></a>Aanmelden voor meldingen

Deze stappen registreren met de hub melding bij het opstarten met behulp van de categorieën die zijn opgeslagen in lokale opslag.

> [AZURE.NOTE] Omdat de registratie-id toegewezen door Google Cloud Messaging (GCM) op elk gewenst moment wijzigen kunt, dient u te registreren voor meldingen regelmatig om te voorkomen dat storingen melding. In dit voorbeeld registreert voor melding elke keer dat de toepassing wordt gestart. Voor toepassingen die vaak worden uitgevoerd, kunt meer dan één keer per dag, u waarschijnlijk overslaan registratie om bandbreedte te besparen als minder dan een dag is verstreken sinds de vorige registratie.


1. De volgende code toevoegen aan het einde van de **onCreate** -methode in de klasse **MainActivity** :

        notifications.subscribeToCategories(notifications.retrieveCategories());

    Dit zorgt ervoor dat telkens wanneer de toepassing wordt gestart wordt opgehaald van de categorieën van lokale opslag en een registeration voor deze categorieën vraagt. 

2. Werk de `onStart()` methode van de `MainActivity` klasse als volgt:

    @Overridebeveiligde void onStart() {super.onStart();      isVisible = true;

        Set<String> categories = notifications.retrieveCategories();

        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }

    Hiermee werkt u de voornaamste activiteit op basis van de status van eerder opgeslagen categorieën.

De app is nu voltooid en een set categorieën kan worden opgeslagen in de lokale opslag van apparaat gebruikt om u te registreren bij de hub melding wanneer de gebruiker de selectie van categorieën wijzigt. Vervolgens definiëren we een backend die categorie berichten naar deze toepassing verzenden kan.

##<a name="sending-tagged-notifications"></a>Gecodeerde berichten verzenden

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>De toepassing uitvoeren en meldingen genereren

1. De app bouwen in Android Studio en start op een apparaat of een emulator.

    Opmerking dat de UI-app biedt een aantal in-en uitschakelen waarmee u de categorieën wilt abonneren op kiezen.

2. Schakel een of meer categorieën in-of uitschakelen en klik op **abonneren**.

    De app wordt de geselecteerde categorieën omgezet in codes en vraagt om een nieuwe registratie voor de geselecteerde codes van de hub van de kennisgeving. De geregistreerde categorieën worden geretourneerd en weergegeven in een kennisgeving toast.

4. Een nieuwe melding sturen door het uitvoeren van de toepassing .NET-Console.  Ook tagged om Sjabloonmeldingen te verzenden op het tabblad foutopsporing van de hub van uw melding in de [Klassieke Azure-Portal].

    Meldingen voor de geselecteerde categorieën weergegeven als toast meldingen.

##<a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de laatste nieuws uitzending per categorie. Houd rekening met voltooien van een van de volgende zelfstudies die andere geavanceerde scenario's voor melding Hubs markeren:

+ [Melding Hubs gebruiken voor het uitzenden van gelokaliseerde laatste nieuws]

    Informatie over het uitbreiden van het laatste nieuws app zodat gelokaliseerde verzenden van meldingen.





<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Melding Hubs gebruiken voor het uitzenden van gelokaliseerde laatste nieuws]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Azure klassieke Portal]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591

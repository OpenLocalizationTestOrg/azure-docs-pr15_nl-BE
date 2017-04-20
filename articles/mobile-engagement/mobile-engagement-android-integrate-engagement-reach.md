<properties
    pageTitle="Integratie van Azure betrokkenheid bij de mobiele Android SDK"
    description="Meest recente updates en procedures voor de Android SDK voor Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-reach-on-android"></a>Het integreren van betrokkenheid bij de Reach op Android

> [AZURE.IMPORTANT] U moet de integratie procedure in de betrokkenheid bij de integratie van het op Android document voordat u deze handleiding volgen.

##<a name="standard-integration"></a>Standard-integratie

De SDK bereiken, moet de **Android Support library (v4)**.

De snelste manier om de bibliotheek toevoegen aan het project in **Eclips** is `Right click on your project -> Android Tools -> Add Support Library...`.

Als u geen Eclips gebruikt, kunt u de instructies lezen [hier].

Reach-bronbestanden kopiëren uit de SDK in uw project:

-   Kopieer de bestanden van de `res/layout` map wordt geleverd met de SDK in de `res/layout` map van uw toepassing.
-   Kopieer de bestanden van de `res/drawable` map wordt geleverd met de SDK in de `res/drawable` map van uw toepassing.

Bewerken uw `AndroidManifest.xml` bestand:

-   De volgende sectie toevoegen (tussen de `<application>` en `</application>` codes):

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
              <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
              </intent-filter>
            </receiver>

-   U moet deze machtiging voor systeemmeldingen die niet is geklikt tijdens het opstarten (anders ze op schijf worden bewaard, maar niet meer weergegeven, moet u echt deze).

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

-   Geeft een pictogram dat wordt gebruikt voor meldingen (beide in app en het systeem die) door te kopiëren en te bewerken in de volgende sectie (tussen de `<application>` en `</application>` codes):

            <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [AZURE.IMPORTANT] Deze sectie is **verplicht** als u van plan systeemmeldingen gebruiken bent bij het maken van campagnes bereiken. Android wordt voorkomen dat systeemmeldingen zonder pictogrammen worden weergegeven. Dus als u deze sectie overslaan, uw eindgebruikers worden niet kunnen ontvangen.

-   Als u campagnes met systeemmeldingen met grote afbeelding maakt, moet u de volgende machtigingen toevoegen (nadat de `</application>` tag) als ontbreken:

            <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
            <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

  -   Op Android M en als Android API niveau 23 of hoger, is bedoeld voor uw toepassing ``WRITE_EXTERNAL_STORAGE`` machtiging vereist gebruikersgoedkeuring. Lees [deze sectie](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

-   Voor systeemmeldingen kunt u ook opgeven in de campagne bereiken als het apparaat moet overgaan en/of trillen. Om te werken, u hebt om te controleren of u de volgende machtigingen gedeclareerd (nadat de `</application>` code):

            <uses-permission android:name="android.permission.VIBRATE" />

    Zonder deze machtiging kunnen Android wordt voorkomen dat systeemmeldingen worden weergegeven als u de ring of de vibrate optie in de campagne bereikt manager gecontroleerd.

-   Als u uw toepassing met behulp van **ProGuard** maken en fouten die zijn gerelateerd aan het Android Support library of het oppervlak van de betrokkenheid, voeg de volgende regels aan uw `proguard.cfg` bestand:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

## <a name="native-push"></a>Native Push

Nu u geconfigureerd bereik module, moet u systeemeigen push te kunnen ontvangen van de campagnes op het apparaat te configureren.

We ondersteunen twee services op Android:

  - Google-Play-apparaten: Gebruik [Google Cloud Messaging] door de gids [hoe GCM integreren met betrokkenheid bij de gids](mobile-engagement-android-gcm-integrate.md) .
  - Amazon-apparaten: Gebruik [Amazon apparaat Messaging] door de gids [hoe ADM integreren met betrokkenheid bij de gids](mobile-engagement-android-adm-integrate.md) .

Als zowel Amazon en Google Play-apparaten zijn mogelijk om alles in 1 AndroidManifest.xml/APK voor ontwikkeling. Maar bij het verzenden van Amazon, kan uw toepassing weigeren als ze GCM code vinden.

In dat geval moet u meerdere APKs gebruiken.

**De toepassing is gereed voor het ontvangen en weergeven van reach campagnes!**

##<a name="how-to-handle-data-push"></a>Het afhandelen van gegevens push

### <a name="integration"></a>Integratie

Als u wilt dat uw toepassing kunnen dit gereedschap duwt Reach gegevens ontvangen, hebt u voor het maken van een subklasse van `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` en de verwijzing in de `AndroidManifest.xml` bestand (tussen de `<application>` en/of `</application>` codes):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Vervolgens kunt u negeren de `onDataPushStringReceived` en `onDataPushBase64Received` callbacks. Hier volgt een voorbeeld:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Categorie

De categorieparameter is optioneel wanneer u een campagne Push-gegevens maken en Hiermee kunt dat u filtergegevens duwt. Dit is handig als u verschillende broadcast receivers verwerken van verschillende soorten gegevens gereedschap duwt, of als u verschillende soorten push van `Base64` gegevens en wilt u het type te identificeren voordat ze worden geparseerd.

### <a name="callbacks-return-parameter"></a>Callbacks retourparameter

Hier volgen enkele richtlijnen alleen goed verwerken de geretourneerde parameter van `onDataPushStringReceived` en `onDataPushBase64Received`:

-   Een broadcast-ontvanger weer `null` in het terugbellen als het weet niet hoe een push gegevens verwerken. U moet de categorie gebruiken om te bepalen of uw uitzending ontvanger de push gegevens of niet verwerken moet.
-   Een van de broadcast-ontvanger weer `true` in de retouraanroep als de push gegevens accepteert.
-   Een van de broadcast-ontvanger weer `false` in de retouraanroep als deze de push gegevens herkent maar verwijdert alle voor welke reden dan ook. Bijvoorbeeld `false` als in de ontvangen gegevens is ongeldig.
-   Als u een broadcast-ontvanger geeft `true` terwijl een ander met als `false` voor de dezelfde gegevens push, het gedrag is niet gedefinieerd, moet u dat nooit doen.

Het retourtype wordt alleen gebruikt voor de statistieken bereiken:

-   `Replied`wordt verhoogd als een van de ontvangers van broadcast geretourneerd als een `true` of `false`.
-   `Actioned`alleen als een van de ontvangers van broadcast geretourneerd wordt verhoogd `true`.

##<a name="how-to-customize-campaigns"></a>Het aanpassen van campagnes

Campagnes aanpassen, kunt u de indelingen die beschikbaar zijn in de SDK bereiken.

U moet alle id's die in de lay-outs en houden de typen weergaven die u gebruikt een id, met name voor tekst en afbeelding weergaven. Sommige weergaven worden alleen gebruikt om te verbergen of weergeven van gebieden, zodat hun type kan worden gewijzigd. Controleer de broncode als u wilt wijzigen van een weergave in de opgegeven lay-outs.

### <a name="notifications"></a>Meldingen

Er zijn twee soorten meldingen: meldingen van systeem en in de app die bestanden in verschillende indelingen gebruiken.

#### <a name="system-notifications"></a>Systeemmeldingen

Systeemmeldingen aanpassen, moet u gebruikmaken van de **categorieën**. U kunt naar [categorieën](#categories)gaan.

#### <a name="in-app-notifications"></a>In de app-meldingen

Een melding van een in-app is een weergave die dynamisch wordt toegevoegd aan de huidige activiteit gebruikersinterface dankzij de methode Android `addContentView()`. Dit heet een overlay melding. Melding overlays zijn ideaal voor een snelle integratie omdat ze niet vereisen dat u elke lay-out in uw toepassing kunt wijzigen.

Als u wilt het uiterlijk van uw melding overlays wijzigen, kunt u het bestand gewoon wijzigen `engagement_notification_area.xml` aan uw behoeften.

> [AZURE.NOTE] Het bestand `engagement_notification_overlay.xml` dat wordt gebruikt voor het maken van een overlay melding, waaronder het bestand `engagement_notification_area.xml`. U kunt ook aanpassen aan uw behoeften (bijvoorbeeld voor het plaatsen van het gebied met meldingen binnen de bedekking).

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Melding lay-out als onderdeel van de indeling van een activiteit opnemen

Overlays kunnen zijn ideaal voor een snelle integratie maar onhandig of neveneffecten hebben in speciale gevallen. Het overlay-systeem kan worden aangepast op het activiteitenniveau van, waardoor het gemakkelijk is om te voorkomen dat de neveneffecten voor speciale activiteiten.

U kunt onze indeling kennisgeving opnemen in uw bestaande lay-out dankzij de instructie Android **opnemen** . Het volgende is een voorbeeld van een gewijzigde `ListActivity` lay-out die net een `ListView`.

**Voor de integratie van betrokkenheid:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Na de integratie van betrokkenheid:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

In dit voorbeeld hebben we een bovenliggende container toegevoegd, omdat de oorspronkelijke indeling een lijstweergave als element van het hoogste niveau gebruikt. We hebben ook toegevoegd `android:layout_weight="1"` kunnen hieronder een lijstweergave geconfigureerd met een weergave toevoegen `android:layout_height="fill_parent"`.

De SDK voor het bereiken van betrokkenheid detecteert automatisch dat de indeling van het bericht is opgenomen in deze activiteit en wordt een overlay voor deze activiteit niet toevoegen.

> [AZURE.TIP] Als u een ListActivity in uw toepassing, voorkomt de bedekking van een zichtbaar bereik u reageren op de items in de lijstweergave hebt geklikt. Dit is een bekend probleem. U kunt dit probleem omzeilen stelt u de indeling van het bericht insluiten in de indeling van uw eigen activiteit zoals in het vorige voorbeeld.

##### <a name="disabling-application-notification-per-activity"></a>Kennisgeving per activiteit toepassing uitschakelen

Als u niet dat de overlay wilt wilt toevoegen aan uw activiteit en als u de indeling van het bericht niet in uw eigen lay-out opnemen, kunt u uitschakelen de overlay voor deze activiteit in de `AndroidManifest.xml` door het toevoegen van een `meta-data` sectie, zoals in het volgende voorbeeld:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a>Categorieën

Wanneer u de opgegeven lay-outs wijzigt, kunt u het uiterlijk van alle uw meldingen aanpassen. Categorieën kunnen u verschillende gerichte uiterlijk (eventueel gedrag) voor berichten definiëren. Een categorie kan worden opgegeven bij het maken van een campagne bereiken. Houd er rekening mee dat categorieën u ook aanpassen kunnen, aankondigingen en stemmingen, die verderop in dit document wordt beschreven.

Een categorie-handler voor de meldingen registreren, moet u een gesprek toevoegen wanneer de toepassing wordt geïnitialiseerd.

> [AZURE.IMPORTANT] Lees de waarschuwing over het kenmerk android proces: \<android sdk-betrokkenheid bij de procedure\> in de het Engagement integreren op Android onderwerp voordat u verdergaat.

In het volgende voorbeeld wordt ervan uitgegaan dat u de vorige waarschuwing wordt bevestigd en een subklasse van `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

De `MyNotifier` object is de implementatie van de handler voor de categorie aanmelding. Het is ofwel een implementatie van de `EngagementNotifier` -interface of een subklasse van de standaard uitvoering: `EngagementDefaultNotifier`.

Houd er rekening mee dat de kennisgever dezelfde opgedeeld in verschillende categorieën kan verwerken, kunt u deze als volgt registreren:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

Ter vervanging van de uitvoering van de categorie standaard, kunt u uw implementatie zoals registreren in het volgende voorbeeld:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

De huidige categorie gebruikt in een handler wordt doorgegeven als een parameter in de meeste methoden die u kunt negeren in `EngagementDefaultNotifier`.

Deze wordt doorgegeven als een `String` parameter of indirect in een `EngagementReachContent` object met een `getCategory()` methode.

Kunt u het meldingsproces maken de meeste methoden definitie op `EngagementDefaultNotifier`, voor meer geavanceerde aanpassingen gerust Kijk op de technische documentatie en de broncode.

##### <a name="in-app-notifications"></a>In de app-meldingen

Als u alleen gebruik van alternatieve indelingen voor een bepaalde categorie wilt, kunt u dit implementeren als volgt:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Voorbeeld van `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Zoals u ziet, is de overlay weergave-id andere dan de standaardlocatie. Het is belangrijk dat elke indeling een unieke id voor overlays gebruiken.

**Voorbeeld van `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Zoals u ziet, is de notification area weergave-id andere dan de standaardlocatie. Het is belangrijk dat elke indeling maakt gebruik van een unieke id voor de gebieden van kennisgeving.

Dit eenvoudige voorbeeld van categorie maakt een toepassing (of in de app) meldingen aan de bovenkant van het scherm weergegeven. Wij zijn de standaard-id's in het systeemvak zelf niet gewijzigd.

Als u wijzigen wilt, hebt u wijzigt de definitie van de `EngagementDefaultNotifier.prepareInAppArea` methode. Het is raadzaam om te zoeken op de technische documentatie en de broncode van `EngagementNotifier` en `EngagementDefaultNotifier` als u wilt dat dit niveau van geavanceerde aanpassingen.

##### <a name="system-notifications"></a>Systeemmeldingen

Door `EngagementDefaultNotifier`, kunt u negeren `onNotificationPrepared` de melding die is opgesteld door de standaardimplementatie wijzigen.

Bijvoorbeeld:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

In dit voorbeeld wordt de kennisgeving van een systeem voor een inhoud die wordt weergegeven als een voortdurende gebeurtenis wanneer de categorie 'continue' wordt gebruikt.

Als u wilt bouwen van de `Notification` object maken, kunt u terugkeren `false` en de oproep `notify` zelf op de `NotificationManager`. In dat geval is het belangrijk dat u een `contentIntent`, een `deleteIntent` en de bericht-id die wordt gebruikt door `EngagementReachReceiver`.

Hier volgt een voorbeeld van een dergelijke uitvoering:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice the call to get the right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Alleen aankondigingen van melding

Het beheer van de Klik in een kennisgeving alleen aankondiging kan worden aangepast door het overschrijven van `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` voor het wijzigen van de voorbereide `Intent`. Met deze methode kunt u de markeringen eenvoudig afstemmen.

Toevoegen van bijvoorbeeld de `SINGLE_TOP` vlag:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

Voor gebruikers van oudere betrokkenheid, houd er rekening mee dat systeemmeldingen zonder actie URL nu de toepassing start als het was op de achtergrond, zodat deze methode kan worden aangeroepen met een geboortekaartje zonder actie-URL. U moet rekening houden dat bij het aanpassen van de intentie.

U kunt ook implementeren `EngagementNotifier.executeNotifAnnouncementAction` maken.

##### <a name="notification-life-cycle"></a>Levenscyclus van melding

Als u de standaardcategorie, sommige levenscyclus-methoden worden aangeroepen op de `EngagementReachInteractiveContent` object geven statistieken weer en de status van de campagne:

-   Wanneer de melding wordt weergegeven in de toepassing of op de statusbalk plaatsen, de `displayNotification` methode is aangeroepen (waarvoor statistieken) door `EngagementReachAgent` als `handleNotification` geeft als resultaat `true`.
-   Als het bericht wordt gesloten, de `exitNotification` methode wordt aangeroepen, statistiek wordt gerapporteerd en volgende campagnes kunnen nu worden verwerkt.
-   Als u de melding klikt, `actionNotification` wordt aangeroepen, statistiek gemeld en wordt de bijbehorende intentie wordt gestart.

Als uw implementatie van `EngagementNotifier` het standaardgedrag omzeilt u moet deze methoden levenscyclus zelf bellen. De volgende voorbeelden ziet enkele gevallen waar het standaardgedrag wordt genegeerd:

-   Reiken niet `EngagementDefaultNotifier`, b.v. u categorie verwerking helemaal geïmplementeerd.
-   Voor systeemmeldingen, het overrode de `onNotificationPrepared` en u aangepast `contentIntent` of `deleteIntent` in de `Notification` object.
-   Voor meldingen-app, het overrode `prepareInAppArea`, moet u ten minste toewijzen `actionNotification` naar een van uw U.I bepaalt.

> [AZURE.NOTE] Als `handleNotification` genereert een uitzondering, de inhoud wordt verwijderd en `dropContent` wordt genoemd. Dit wordt gemeld in de statistieken en volgende campagnes kunnen nu worden verwerkt.

### <a name="announcements-and-polls"></a>Aankondigingen en polls

#### <a name="layouts"></a>Lay-outs

U kunt de `engagement_text_announcement.xml`, `engagement_web_announcement.xml` en `engagement_poll.xml` bestanden voor het aanpassen van tekst aankondigingen, web, aankondigingen en stemmingen.

Deze bestanden delen twee gangbare indelingen voor de titel en de knop Display. De lay-out voor de titel `engagement_content_title.xml` en het eponymous drawable-bestand wordt gebruikt voor de achtergrond. De lay-out voor de actie- en uitgiftemodules knoppen `engagement_button_bar.xml` en het eponymous drawable-bestand wordt gebruikt voor de achtergrond.

In een enquête, de indeling van de vraag en hun keuzes zijn dynamisch opgeblazen met verschillende keren de `engagement_question.xml` bestand lay-out voor de vragen en de `engagement_choice.xml` -bestand voor de opties.

#### <a name="categories"></a>Categorieën

##### <a name="alternate-layouts"></a>Alternatieve indelingen

Zoals mededelingen, kan de categorie van de campagne men hebben ook alternatieve indelingen voor uw aankondigingen en stemmingen.

Bijvoorbeeld om een categorie voor de aankondiging van een tekst hebt gemaakt, kunt u uitbreiden `EngagementTextAnnouncementActivity` en het verwijzen naar de `AndroidManifest.xml` bestand:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Houd er rekening mee dat de categorie van de uitvoerintentie filter wordt gebruikt om het verschil met de standaard aankondiging activiteit.

De SDK bereiken gebruikmaakt van de uitvoerintentie systeem om de juiste activiteit voor een specifieke categorie en valt terug op de standaardcategorie als de resolutie is mislukt.

Hebt u voor het implementeren van `MyCustomTextAnnouncementActivity`, als u alleen wilt wijzigen van de lay-out (maar behouden dezelfde weergave-id's), u hoeft alleen voor het definiëren van de klasse, zoals in het volgende voorbeeld:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class to use R.layout.my_text_announcement
              }
            }

Ter vervanging van het soort tekst, aankondigingen, gewoon vervangen door `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` door uw implementatie.

Web-aankondigingen en polls kunnen worden aangepast op een vergelijkbare manier.

Voor de webonderdelen aankondigingen kunt u uitbreiden `EngagementWebAnnouncementActivity` en verklaren de activiteit in de `AndroidManifest.xml` , zoals in het volgende voorbeeld:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in the intent is the data mime type -->
              </intent-filter>
            </activity>

U kunt uitbreiden voor polls `EngagementPollActivity` en verklaart de in de `AndroidManifest.xml` , zoals in het volgende voorbeeld:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Uitvoering geheel

Categorieën voor de activiteiten van de aankondiging (en poll) implementeren zonder uit te breiden van een van de `Engagement*Activity` klassen die door de SDK bereiken. Dit is bijvoorbeeld handig als u wilt definiëren een indeling die niet de dezelfde weergaven worden gebruikt als de standaard lay-outs.

Net als voor melding van geavanceerde aanpassingen, wordt het aanbevolen te kijken naar de broncode van de standaardimplementatie.

Hier volgen enkele dingen rekening houden: Reach de activiteit met een bepaalde bedoeling (overeenkomend met de bedoeling filter), plus een extra parameter die de content-ID wordt gestart.

U kunt dit doen voor het ophalen van de inhoud-object dat met de velden die u hebt opgegeven bij het maken van de campagne op de website:

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

Voor de statistiek, meld de inhoud wordt weergegeven in de `onResume` gebeurtenis:

            @Override
            protected void onResume()
            {
             /* Mark the content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Vervolgens moet u niet vergeten te bellen op `actionContent(this)` of `exitContent(this)` van het inhoud-object voordat de activiteit naar de achtergrond gaat.

Als u niet een `actionContent` of `exitContent`, statistieken niet verzonden (dat wil zeggen geen analytics van de campagne) en belangrijker nog, de volgende campagnes niet krijgt als het proces van de toepassing opnieuw wordt gestart.

Afdrukstand of andere wijzigingen in de configuratie kunnen maken de code lastig om te bepalen of de activiteit naar achtergrond gaat of niet, de standaardimplementatie wordt gecontroleerd of de inhoud wordt gemeld als de gebruiker de activiteit verlaat is afgesloten (door te drukken op `HOME` of `BACK`), maar niet als de afdrukstand verandert.

Hier is het interessante deel van de uitvoering:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have to check anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Zoals u ziet, als u de naam `actionContent(this)` en vervolgens de activiteit, is voltooid `exitContent(this)` veilig kan worden aangeroepen zonder effect.

[Hier]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud Messaging]:http://developer.android.com/guide/google/gcm/index.html
[Amazon apparaat Messaging]:https://developer.amazon.com/sdk/adm.html

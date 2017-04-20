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

#<a name="how-to-integrate-engagement-on-android"></a>Het integreren van betrokkenheid op Android

> [AZURE.SELECTOR]
- [Universele Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Silverlight voor Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Deze procedure beschrijft de eenvoudigste manier om de betrokkenheid van Analytics en controle functies in uw Android-toepassing activeren.

> [AZURE.IMPORTANT] Het minimumniveau van de Android SDK API moet 10 of hoger (Android 2.3.3 of hoger).

De volgende stappen zijn genoeg om het verslag van de logboeken die nodig zijn voor het berekenen van alle statistische gegevens over gebruikers, sessies, activiteiten, Crashes en Technicals wordt geactiveerd. Het verslag van de logboeken die nodig zijn voor het berekenen van andere statistieken, zoals gebeurtenissen, fouten en taken worden uitgevoerd handmatig met behulp van de API Engagement (Zie [het gebruik van geavanceerde mobiele aanstelling tagging API in uw Android](mobile-engagement-android-use-engagement-api.md) omdat deze statistieken afhankelijk van de toepassing zijn.

##<a name="embed-the-engagement-sdk-and-service-into-your-android-project"></a>De service en betrokkenheid bij de SDK insluiten in uw Android project

De Android SDK downloaden vanaf [hier](https://aka.ms/vq9mfn) Get `mobile-engagement-VERSION.jar` en plaatst u deze in de `libs` map van uw Android project (de libs map maken als u nog niet bestaat).

> [AZURE.IMPORTANT]
> Als u uw toepassingspakket met ProGuard bouwt, moet u sommige klassen behouden. U kunt de volgende configuratie-fragment:
>
>
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
            <methods>;
            }

Uw betrokkenheid bij de verbindingsreeks opgeven door het aanroepen van de volgende methode in de activiteit starten:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

De verbindingsreeks voor de toepassing wordt weergegeven op de Portal Azure.

-   Als ontbreekt, voegt u de volgende machtigingen voor Android (vóór de `<application>` code):

            <uses-permission android:name="android.permission.INTERNET"/>
            <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

-   De volgende sectie toevoegen (tussen de `<application>` en `</application>` codes):

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

-   Wijziging `<Your application name>` door de naam van uw toepassing.

> [AZURE.TIP] De `android:label` kenmerk kunt u ervoor kiezen de naam van de betrokkenheid bij de service zoals die wordt weergegeven voor de eindgebruikers in het scherm 'Services uitgevoerd' van hun telefoon. Het is raadzaam dit kenmerk instellen op `"<Your application name>Service"` (bv. `"AcmeFunGameService"`).

Geven de `android:process` kenmerk zorgt ervoor dat de betrokkenheid bij de service wordt uitgevoerd in een eigen proces (Engagement uitgevoerd in hetzelfde proces als uw toepassing uw hoofd/UI-thread mogelijk minder goed maakt).

> [AZURE.NOTE] Elke code die u plaatst in `Application.onCreate()` en andere callbacks van toepassing voor de processen van de toepassing, met inbegrip van de betrokkenheid bij de service wordt uitgevoerd. Deze wellicht ongewenste neveneffecten (zoals overbodige geheugentoewijzingen en threads in de betrokkenheid van proces, dubbele broadcast-ontvangers of services).

Als u overschrijven `Application.onCreate()`, is het raadzaam het volgende codefragment toevoegen aan het begin van de `Application.onCreate()` functie:

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

U kunt hetzelfde doen voor `Application.onTerminate()`, `Application.onLowMemory()` en `Application.onConfigurationChanged(...)`.

Kunt u uitbreiden `EngagementApplication` in plaats van het uitbreiden van `Application`: de callback `Application.onCreate()` voert het proces uit en roept `Application.onApplicationProcessCreate()` alleen als het huidige proces niet degene die de betrokkenheid bij de service, dezelfde regels voor de andere callbacks gelden.

##<a name="basic-reporting"></a>Eenvoudige rapportage

### <a name="recommended-method-overload-your-activity-classes"></a>Aanbevolen methode: overbelasten uw `Activity` klassen

Om te activeren in het rapport van de logboekbestanden die zijn vereist voor betrokkenheid bij de gebruikers, sessies, activiteiten, Crashes en technische statistieken te berekenen, u hoeft alleen te maken alle uw `*Activity` onderliggende klassen overnemen van de bijbehorende `Engagement*Activity` klassen (bv. Als uw oude activiteiten uitgebreid `ListActivity`, maken dit doorloopt `EngagementListActivity`).

**Zonder afspraak:**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**Met afspraak:**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [AZURE.IMPORTANT] Bij het gebruik van `EngagementListActivity` of `EngagementExpandableListActivity`, zorg ervoor dat alle aanroepen van `requestWindowFeature(...);` wordt uitgevoerd vóór de aanroep van `super.onCreate(...);`, anders een crash zal optreden.

U vindt deze klassen in de `src` map en deze kopiëren naar uw project. De klassen zijn ook in het **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternatieve methode: bel `startActivity()` en `endActivity()` handmatig

Als u niet kan of wil niet overbelasten uw `Activity` -klassen, in plaats daarvan starten en beëindigen van de activiteiten door het aanroepen van `EngagementAgent`van methoden direct.

> [AZURE.IMPORTANT] De Android SDK nooit roept de `endActivity()` methode, zelfs wanneer de toepassing wordt gesloten (op Android, toepassingen zijn eigenlijk nooit gesloten). Het is dus *sterk* aanbevolen om aan te roepen de `startActivity()` methode in de `onResume` terugbellen van *alle* activiteiten, en de `endActivity()` methode in de `onPause()` terugbellen van *alle* uw activiteiten. Dit is de enige manier om ervoor te zorgen dat sessies niet worden gelekt. Als een sessie wordt vermist, wordt betrokkenheid nooit verbroken door de service van de betrokkenheid van end (omdat de service verbonden blijft als een sessie in behandeling is).

Hier volgt een voorbeeld:

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

In dit voorbeeld lijkt veel op de `EngagementActivity` klasse en haar varianten, waarvan de broncode is beschikbaar in de `src` map.

##<a name="test"></a>Test

Controleer nu uw integratie of door uw mobiele app in een emulator of het apparaat en registratie van een sessie op het tabblad Monitor controleren.

De volgende secties zijn optioneel.

##<a name="location-reporting"></a>Locatie melden

Als u locaties moeten worden gerapporteerd, moet u een paar regels van configuratie toevoegen (tussen de `<application>` en `</application>` codes).

### <a name="lazy-area-location-reporting"></a>Lazy gebied locatie melden

Lazy gebied locatie reporting kunt melden van het land, regio en plaats die zijn gekoppeld aan apparaten. Dit type locatie melden wordt alleen gebruikt voor netwerklocaties (op basis van cel-ID of Wi-Fi). Het gebied van het apparaat is gemeld maximaal eenmaal per sessie. De GPS is nooit gebruikt en dit type locatie rapport heeft slechts weinig (en niet naar Nee zeggen) dus invloed op de accu.

Gerapporteerde gebieden worden gebruikt voor het berekenen van geografische statistieken over gebruikers, sessies, gebeurtenissen en fouten. Ze kunnen ook worden gebruikt als criterium in Reach campagnes.

Zodat lazy gebied locatie melden kunt u dit doen met behulp van de configuratie die eerder in deze procedure wordt vermeld:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Ook moet u de volgende machtigingen toevoegen als ontbreken:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Of u kunt blijven gebruiken ``ACCESS_FINE_LOCATION`` als u al in uw toepassing gebruikt.

### <a name="real-time-location-reporting"></a>Real-time melden van locatie

Rapportage van real time locatie kan rapporteren de breedte- en lengtewaarden gekoppeld aan apparaten. Dit type locatie melden wordt alleen netwerklocaties (op basis van cel-ID of Wi-Fi) en de rapportage is alleen actief wanneer de toepassing wordt uitgevoerd in de voorgrond (dat wil zeggen tijdens een sessie).

Locaties in real time worden *niet* gebruikt om statistieken te berekenen. Hun enige doel is dat het gebruik van real-time geo-fencing \<Reach-doelgroep-geofencing\> criterium in Reach campagnes.

Zodat real-time locatie melden kunt u dit doen met behulp van de configuratie die eerder in deze procedure wordt vermeld:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Ook moet u de volgende machtigingen toevoegen als ontbreken:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Of u kunt blijven gebruiken ``ACCESS_FINE_LOCATION`` als u al in uw toepassing gebruikt.

#### <a name="gps-based-reporting"></a>GPS gebaseerde rapportage

Real-time melden van locatie alleen standaard op basis van netwerklocaties. Gebruik het configuratieobject zodat het gebruik van op basis van GPS-locaties (die veel meer nauwkeurige):

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Ook moet u de volgende machtigingen toevoegen als ontbreken:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Achtergrond melden

Standaard is real-time melden van locatie alleen actief wanneer de toepassing wordt uitgevoerd in de voorgrond (dat wil zeggen tijdens een sessie). Gebruik het configuratieobject zodat de rapportage ook in achtergrond:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Wanneer de toepassing op de achtergrond, wordt alleen uitgevoerd netwerk op basis van locaties worden gerapporteerd, zelfs als u de GPS ingeschakeld.

Het rapport achtergrond locatie wordt gestopt als de gebruiker het apparaat opnieuw is opgestart, kunt u deze automatisch opnieuw wordt gestart tijdens de opstartprocedure manier toevoegen:

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

Ook moet u de volgende machtigingen toevoegen als ontbreken:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Machtigingen voor android M

Vanaf Android M, sommige machtigingen tijdens runtime worden beheerd en gebruikersgoedkeuring nodig heeft.

De runtime-machtigingen wordt voor nieuwe installaties van app standaard uitgeschakeld wanneer u Android API niveau 23. Anders zal deze worden standaard ingeschakeld.

De gebruiker kan in-/ uitschakelen die machtigingen in het menu apparaat instellingen. Het uitschakelen van de machtigingen in het systeemmenu van achtergrondprocessen van de toepassing beëindigd, dit is een systeem probleem en heeft geen invloed op de mogelijkheid om te ontvangen van push op achtergrond.

In de context van Mobile Engagement zijn de machtigingen die moeten worden goedgekeurd tijdens runtime:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`
- `WRITE_EXTERNAL_STORAGE`(alleen bij Android API niveau 23 voor deze toewijzing)

Externe opslag wordt alleen gebruikt voor Reach big picture-functie. Als u gebruikers deze waarin toestemming wordt gevraagd om storend, kunt u deze verwijderen als u deze alleen voor Mobile Engagement echter ten koste van grote afbeelding functie uit te schakelen.

Voor de functies van de locatie, moet u machtigingen aan de gebruiker een dialoogvenster standaard aanvragen. Als de gebruiker goedkeurt, moet u vertellen ``EngagementAgent`` die wijziging in aanmerking nemen in real-time (anders is de wijziging worden verwerkt wanneer de gebruiker de toepassing start).

Hier volgt een codevoorbeeld te gebruiken in een activiteit van uw toepassing machtigingen aanvragen en zendt het resultaat als positief naar ``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

##<a name="advanced-reporting"></a>Geavanceerde rapportage

Optioneel, als u rapporteren toepassing specifieke gebeurtenissen, fouten en taken wilt, moet u de betrokkenheid bij de API gebruiken via de methoden van de `EngagementAgent` klasse. Een object van deze klasse is opgehaald door het aanroepen van de `EngagementAgent.getInstance()` statische methode.

De betrokkenheid bij de API kunt u alle geavanceerde mogelijkheden van betrokkenheid van en wordt besproken in de manier waarop de betrokkenheid bij de API gebruiken op Android (en van de technische documentatie van de `EngagementAgent` klasse).

##<a name="advanced-configuration-in-androidmanifestxml"></a>Geavanceerde configuratie (in AndroidManifest.xml)

### <a name="wake-locks"></a>Wake-sloten

Als u wilt om ervoor te zorgen dat de statistieken worden verzonden in real-time als via Wi-Fi of wanneer het scherm uitgeschakeld is, voegt u de volgende optionele machtiging:

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Rapport van crash

Als u uitschakelen van foutenrapporten wilt, voegt u dit (tussen de `<application>` en `</application>` codes):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Burst-drempel

Standaard registreert de betrokkenheid bij de servicerapporten in realtime. Als de toepassing Logboeken zeer vaak wordt gemeld, is het beter in de buffer van de logboeken en om deze in één keer op een vaste tijd basis (dit is de 'burst-modus' genoemd) te melden. Hiertoe voegt u dit (tussen de `<application>` en `</application>` codes):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

De burst-modus iets langere levensduur, maar heeft een impact op de betrokkenheid bij de Monitor: duur van alle sessies en taken aan de drempelwaarde burst (sessies en dus korter zijn dan de drempelwaarde burst is mogelijk niet zichtbaar taken) worden afgerond. Het verdient aanbeveling een burst drempelwaarde niet meer dan 30000 (30s) gebruiken.

### <a name="session-timeout"></a>Time-out van sessie

Een sessie is afgelopen 10s standaard na het einde van de laatste activiteit (dit gebeurt meestal door te drukken op de toets Home of terug, door te stellen dat de telefoon inactief of door te gaan naar een andere toepassing). Dit is om te voorkomen dat een sessie-splitsing elke keer dat de gebruiker afsluiten en zeer snel terug te keren naar de toepassing (dit kan gebeuren wanneer hij een afbeelding, afhalen controleren een melding, enz.). U kunt deze parameter wijzigen. Hiertoe voegt u dit (tussen de `<application>` en `</application>` codes):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

##<a name="disable-log-reporting"></a>Melden van logboek uitschakelen

### <a name="using-a-method-call"></a>Met behulp van een aanroep van de methode

Als u wilt dat Engagement om te stoppen met het verzenden van Logboeken, kunt u bellen:

            EngagementAgent.getInstance(context).setEnabled(false);

Deze oproep is permanent: een bestand met voorkeursinstellingen voor gedeelde wordt gebruikt.

Als betrokkenheid actief is wanneer u deze functie aanroept, duurt 1 minuut om de service te stoppen. Maar het starten de service op de volgende keer dat Won't start u de toepassing.

Kunt u de rapportage opnieuw door te bellen met dezelfde functie logboek inschakelen `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integratie in uw eigen`PreferenceActivity`

In plaats van deze functie aanroept, kunt u deze instelling ook integreren in uw bestaande `PreferenceActivity`.

Betrokkenheid bij uw voorkeurenbestand (met de gewenste modus) te gebruiken kunt u configureren in het `AndroidManifest.xml` het bestand met `application meta-data`:

-   De `engagement:agent:settings:name` sleutel wordt gebruikt voor de naam van het bestand met gedeelde voorkeursinstellingen definiëren.
-   De `engagement:agent:settings:mode` sleutel wordt gebruikt voor het definiëren van de modus van het voorkeurenbestand voor gedeelde, moet u dezelfde modus als in de `PreferenceActivity`. De modus moet worden doorgegeven als een getal: als u een combinatie van vlaggen constant in uw code gebruikt, controleert u de totale waarde.

Betrokkenheid altijd gebruik van de `engagement:key` boolean sleutel in het bestand met voorkeursinstellingen voor het beheer van deze instelling.

In het volgende voorbeeld van `AndroidManifest.xml` de standaardwaarden:

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

U kunt toevoegen een `CheckBoxPreference` in de indeling van uw voorkeur zoals de volgende:

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094

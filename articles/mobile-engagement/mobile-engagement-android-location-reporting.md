<properties
    pageTitle="Locatie voor betrokkenheid bij de Azure mobiele Android SDK"
    description="Wordt beschreven hoe u de locatie voor Azure Mobile Engagement Android SDK"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Locatie voor betrokkenheid bij de Azure mobiele Android SDK

> [AZURE.SELECTOR]
- [Android](mobile-engagement-android-integrate-engagement.md)

Dit onderwerp wordt beschreven hoe u de locatie voor uw Android-toepassing.

## <a name="prerequisites"></a>Vereisten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Locatie melden

Als u locaties moeten worden gerapporteerd, moet u een paar regels van configuratie toevoegen (tussen de `<application>` en `</application>` codes).

### <a name="lazy-area-location-reporting"></a>Lazy gebied locatie melden

Lazy locatie voor de rapportage van het land, regio en plaats die is gekoppeld aan de apparaten kunnen melden. Dit type locatie melden wordt alleen gebruikt voor netwerklocaties (op basis van cel-ID of Wi-Fi). Het gebied van het apparaat is gemeld maximaal eenmaal per sessie. De GPS is nooit gebruikt en dit type rapport locatie heeft dus weinig effect op de accu.

Gerapporteerde gebieden worden gebruikt voor het berekenen van geografische statistieken over gebruikers, sessies, gebeurtenissen en fouten. Ze kunnen ook worden gebruikt als criterium in Reach campagnes.

U kunt lazy locatie voor de rapportage met de configuratie die eerder in deze procedure wordt vermeld:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

U moet ook een machtiging voor een locatie opgeven. Deze code maakt gebruik van ``COARSE`` machtiging:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Als uw app vereist, kunt u ``ACCESS_FINE_LOCATION`` in plaats daarvan.

### <a name="real-time-location-reporting"></a>Locatie real-time rapportage

Reporting real-time locatie kan rapporteren de breedte- en lengtewaarden apparaten gekoppeld. Dit type locatie melden alleen standaard netwerklocaties, op basis van cel-ID of Wi-Fi. De rapportage is alleen actief wanneer de toepassing wordt uitgevoerd in de voorgrond (bijvoorbeeld tijdens een sessie).

Real-time locaties worden *niet* gebruikt om statistieken te berekenen. Hun enige doel is dat het gebruik van real-time geo-fencing \<Reach-doelgroep-geofencing\> criterium in Reach campagnes.

Toevoegen zodat real-time locatie melden van een regel code aan waarin u de verbindingsreeks betrokkenheid bij de activiteit starten instellen. Het resultaat ziet er als volgt uit:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>GPS gebaseerde rapportage

Standaard locatie real-time rapportage gebruikt alleen locaties op basis van het netwerk. Gebruik het configuratieobject zodat het gebruik van GPS gebaseerde locaties die veel nauwkeuriger zijn:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Ook moet u de volgende machtigingen toevoegen als ontbreken:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Achtergrond melden

Standaard is locatie real-time rapportage alleen actief wanneer de toepassing wordt uitgevoerd in de voorgrond (bijvoorbeeld tijdens een sessie). Gebruik dit configuratieobject zodat de rapportage ook in achtergrond:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Wanneer de toepassing wordt uitgevoerd op de achtergrond, alleen op basis van het netwerk locaties worden gerapporteerd, zelfs als u de GPS ingeschakeld.

Als de gebruiker het apparaat opnieuw is opgestart, wordt het rapport achtergrond locatie gestopt. Als u automatisch opnieuw wordt gestart tijdens het opstarten, moet u deze code toevoegen.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

Ook moet u de volgende machtigingen toevoegen als ontbreken:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Machtigingen voor android M

Vanaf Android M, sommige machtigingen tijdens runtime worden beheerd en gebruikersgoedkeuring nodig.

Wanneer u Android API niveau 23, de runtime-machtigingen voor nieuwe installaties van app standaard uitgeschakeld. Anders zijn ze standaard ingeschakeld.

U kunt in-/ uitschakelen die machtigingen in het menu apparaat instellingen. De achtergrondprocessen van de toepassing die het gedrag van een systeem en heeft geen invloed op de mogelijkheid om te ontvangen van push op achtergrond uitschakelen, machtigingen in het systeemmenu worden beëindigd.

In de context van Mobile Engagement locatie melden, zijn de machtigingen die moeten worden goedgekeurd tijdens runtime:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`

Machtigingen van de gebruiker een dialoogvenster standaard aanvragen. Als de gebruiker goedkeurt, vertellen ``EngagementAgent`` te nemen die wijziging rekening in real-time. De wijziging wordt anders de volgende keer dat de gebruiker de toepassing start verwerkt.

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
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

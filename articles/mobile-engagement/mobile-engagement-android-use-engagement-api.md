<properties
    pageTitle="De betrokkenheid bij de API gebruiken op Android"
    description="Laatste Android SDK - aanstelling API gebruiken op Android"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="piyushjo;ricksal" />

#<a name="how-to-use-the-engagement-api-on-android"></a>De betrokkenheid bij de API gebruiken op Android

Dit document is een invoegtoepassing voor het [melden van geavanceerde opties voor Android Mobile Engagement SDK](mobile-engagement-android-advanced-reporting.md)-document. Het biedt in diepte details over het gebruik van de API Engagement voor aangifte van de statistieken van uw toepassing.

Houd er rekening mee dat als u wilt dat alleen de betrokkenheid van uw toepassing sessies, activiteiten, crashes en technische informatie, de eenvoudigste manier is om alle uw `Activity` onderliggende klassen overnemen van de bijbehorende `EngagementActivity` klasse.

Als u wilt doen, bijvoorbeeld als u wilt de toepassing specifieke gebeurtenissen, fouten en taken, of als u moet de activiteiten van uw toepassing op een andere manier dan de geïmplementeerd in het `EngagementActivity` klassen, moet u gebruikmaken van de API Engagement.

De betrokkenheid bij de API wordt geleverd door de `EngagementAgent` klasse. Een exemplaar van deze klasse kan worden opgehaald door het aanroepen van de `EngagementAgent.getInstance(Context)` statische methode (Houd er rekening mee dat de `EngagementAgent` object dat het resultaat is een singleton).

##<a name="engagement-concepts"></a>Betrokkenheid bij de concepten

De volgende onderdelen verfijnen de gemeenschappelijke [Concepten van Mobile Engagement](mobile-engagement-concepts.md)voor het Android-platform.

### <a name="session-and-activity"></a>`Session`en`Activity`

Als de gebruiker meer dan een paar seconden tussen twee *activiteiten*inactief blijft, is de volgorde van *activiteiten* gesplitst in twee afzonderlijke *sessies*. Deze enkele seconden 'time-out van sessie' genoemd.

Een *activiteit* wordt meestal veroorzaakt door een scherm van de toepassing, dat wil zeggen de *activiteit* wordt gestart wanneer het scherm wordt weergegeven en stopt wanneer het scherm is gesloten: dit is het geval wanneer de betrokkenheid bij de SDK is geïntegreerd met behulp van de `EngagementActivity` klassen.

Maar *activiteiten* kunnen ook handmatig met behulp van de API Engagement worden beheerd. Hiermee splitst een bepaald scherm in verschillende sub-delen voor meer informatie over het gebruik van dit scherm (bijvoorbeeld bekend hoe vaak en hoe lang de dialoogvensters in dit scherm worden gebruikt).

##<a name="reporting-activities"></a>Activiteiten melden

> [AZURE.IMPORTANT] U hoeft niet te rapport activiteiten, zoals beschreven in deze sectie als u de `EngagementActivity` klasse en haar varianten zoals uiteengezet in de betrokkenheid bij de integratie van het document voor Android.

### <a name="user-starts-a-new-activity"></a>Gebruiker start een nieuwe activiteit

            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

U moet bellen `startActivity()` elke keer dat de Gebruikerswijzigingen in activiteiten. De eerste aanroep van deze functie wordt een nieuwe gebruikerssessie gestart.

De beste plaats om deze functie is op elke activiteit `onResume` terugbellen.

### <a name="user-ends-his-current-activity"></a>Gebruiker eindigt zijn huidige activiteit

            EngagementAgent.getInstance(this).endActivity();

U moet bellen `endActivity()` ten minste één keer wanneer de gebruiker zijn laatste activiteit is. Dit meldt de SDK Engagement dat de gebruiker die momenteel niet actief is en dat de sessie van de gebruiker moet worden gesloten zodra de time-out voor sessies verlopen (als u `startActivity()` voordat de time-out voor sessies is verlopen, de sessie te worden hervat).

De beste plaats om deze functie is op elke activiteit `onPause` terugbellen.

##<a name="reporting-events"></a>Voor het melden van gebeurtenissen

### <a name="session-events"></a>Sessiegebeurtenissen

Sessiegebeurtenissen worden meestal gebruikt om de acties die worden uitgevoerd door een gebruiker tijdens de sessie zijn.

**Voorbeeld zonder extra gegevens:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Voorbeeld met extra gegevens:**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a>Zelfstandige gebeurtenissen

In tegenstelling tot de sessiegebeurtenissen, kunnen zelfstandige gebeurtenissen plaatsvinden buiten de context van een sessie.

**Voorbeeld:**

Stel dat u wilt rapport gebeurtenissen die optreden wanneer een broadcast-ontvanger wordt geactiveerd:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

##<a name="reporting-errors"></a>Fouten melden

### <a name="session-errors"></a>Sessie-fouten

Sessie-fouten worden meestal gebruikt om de invloed van de gebruiker tijdens de sessie zijn fouten.

**Voorbeeld:**

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a>Zelfstandige fouten

Zelfstandige fouten kunnen met sessie-fouten optreden buiten de context van een sessie.

**Voorbeeld:**

In het volgende voorbeeld ziet u hoe een fout gerapporteerd wanneer het geheugen laag op de telefoon wordt terwijl het toepassingsproces wordt uitgevoerd.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

##<a name="reporting-jobs"></a>Taken melden

### <a name="example"></a>Voorbeeld

Stel dat u wilt melden, de duur van de aanmelding:

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a>Fouten in rapport gedurende een project

Fouten kunnen worden gerelateerd aan een actieve taak in plaats van te wijten aan de huidige gebruikerssessie.

**Voorbeeld:**

Stel dat u wilt rapporteren, een fout die u tijdens het aanmelden proces:

[...] Public void aanmelding (Context context,...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a>Voor het melden van gebeurtenissen tijdens een project

Gebeurtenissen kunnen worden gerelateerd aan een actieve taak in plaats van te wijten aan de huidige gebruikerssessie.

**Voorbeeld:**

Stel dat we een sociaal netwerk hebben en we gebruiken een taak voor het rapport van de totale tijd gedurende welke de gebruiker is verbonden met de server. De gebruiker kan blijven in de achtergrond, zelfs wanneer hij een andere toepassing gebruikt of wanneer de telefoon in de slaapstand staat, zodat er geen sessie.

De gebruiker kan berichten ontvangen van zijn vrienden, dit is een taakgebeurtenis.

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

##<a name="extra-parameters"></a>Extra parameters

Willekeurige gegevens kunnen worden gekoppeld aan gebeurtenissen, fouten, activiteiten en taken.

Deze gegevens kan worden gestructureerd, de Android-bundel klasse wordt gebruikt (dat werkt als extra parameters in Android oogmerken). Houd er rekening mee dat een bundel matrices of een andere bundel instanties kan bevatten.

> [AZURE.IMPORTANT] Als u in een parcelable of serializable parameters, zorg ervoor dat hun `toString()` methode is geïmplementeerd als een leesbare tekenreeks als resultaat geven. Serializable klassen die tijdelijk geen velden bevatten die niet geserialiseerd worden brengt Android vastlopen wanneer u belt`bundle.putSerializable("key",value);`

> [AZURE.WARNING] Sparse matrices in extra parameters worden niet ondersteund, dat wil zeggen won't worden geserialiseerd als een matrix. U moet omzetten in standaard matrices voordat u deze in extra parameters.

### <a name="example"></a>Voorbeeld

            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Limieten

#### <a name="keys"></a>Toetsen

Elke sleutel in de `Bundle` moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Betekent dit dat de sleutels met ten minste één letter beginnen moeten, gevolgd door de letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte

Extra's zijn beperkt tot **1024** tekens per gesprek (eenmaal gecodeerd in JSON door de betrokkenheid bij de service).

In het vorige voorbeeld is het naar de server verzonden JSON 58 tekens:

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Toepassingsgegevens

Handmatig kunt u melden met behulp van het bijhouden van gegevens (of een andere toepassing specifieke gegevens) de `sendAppInfo()` functie.

Merk op dat deze informatie kan stapsgewijs worden verzonden: alleen de meest recente waarde voor een bepaalde sleutel worden opgeslagen voor een bepaald apparaat.

De bundel-klasse wordt gebruikt als gebeurtenis extra's, abstracte toepassingsgegevens, houd er rekening mee dat arrays of onderliggende bundels worden behandeld als een platte reeks (met JSON serialisatie).

### <a name="example"></a>Voorbeeld

Hier volgt een voorbeeld van code voor het verzenden van de gebruiker, geslacht en Geboortedatum:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Limieten

#### <a name="keys"></a>Toetsen

Elke sleutel in de `Bundle` moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Betekent dit dat de sleutels met ten minste één letter beginnen moeten, gevolgd door de letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte

Informatie over de toepassingen zijn beperkt tot **1024** tekens per gesprek (eenmaal gecodeerd in JSON door de betrokkenheid bij de service).

In het vorige voorbeeld is de JSON naar de server verzonden 44 tekens bevatten:

            {"expiration":"2016-12-07","status":"premium"}

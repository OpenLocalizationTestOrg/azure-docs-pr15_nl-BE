<properties
    pageTitle="Het gebruik van de API Engagement op iOS"
    description="Meest recente iOS SDK - het gebruik van de API Engagement op iOS"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-use-the-engagement-api-on-ios"></a>Het gebruik van de API Engagement op iOS

Dit document is een invoegtoepassing voor het document het Engagement op iOS integreren: bevat details over het gebruik van de API van de betrokkenheid van uw toepassing statistieken melden diepte.

Houd er rekening mee dat als u wilt dat alleen de betrokkenheid van uw toepassing sessies, activiteiten, crashes en technische informatie, de eenvoudigste manier is om alle uw aangepaste `UIViewController` objecten overgenomen van de corresponderende `EngagementViewController` klasse.

Als u wilt doen, bijvoorbeeld als u wilt de toepassing specifieke gebeurtenissen, fouten en taken, of als u moet de activiteiten van uw toepassing op een andere manier dan de geïmplementeerd in het `EngagementViewController` klassen, moet u gebruikmaken van de API Engagement.

De betrokkenheid bij de API wordt geleverd door de `EngagementAgent` klasse. Een exemplaar van deze klasse kan worden opgehaald door het aanroepen van de `[EngagementAgent shared]` statische methode (Houd er rekening mee dat de `EngagementAgent` object dat het resultaat is een singleton).

API-aanroepen voordat u de `EngagementAgent` object moet worden geïnitialiseerd door het aanroepen van de methode`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

##<a name="engagement-concepts"></a>Betrokkenheid bij de concepten

De volgende onderdelen verfijnen de algemene [Concepten van Mobile Engagement](mobile-engagement-concepts.md) voor het iOS platform.

### <a name="session-and-activity"></a>`Session`en`Activity`

Een *activiteit* wordt meestal veroorzaakt door een scherm van de toepassing, dat wil zeggen de *activiteit* wordt gestart wanneer het scherm wordt weergegeven en stopt wanneer het scherm is gesloten: dit is het geval wanneer de betrokkenheid bij de SDK is geïntegreerd met behulp van de `EngagementViewController` klassen.

Maar *activiteiten* kunnen ook handmatig met behulp van de API Engagement worden beheerd. Hiermee splitst een bepaald scherm in verschillende sub-delen voor meer informatie over het gebruik van dit scherm (bijvoorbeeld bekend hoe vaak en hoe lang de dialoogvensters in dit scherm worden gebruikt).

##<a name="reporting-activities"></a>Activiteiten melden

### <a name="user-starts-a-new-activity"></a>Gebruiker start een nieuwe activiteit

            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

U moet bellen `startActivity()` elke keer dat de Gebruikerswijzigingen in activiteiten. De eerste aanroep van deze functie wordt een nieuwe gebruikerssessie gestart.

### <a name="user-ends-his-current-activity"></a>Gebruiker eindigt zijn huidige activiteit

            [[EngagementAgent shared] endActivity];

> [AZURE.WARNING] U moet **nooit** deze functie aanroepen door uzelf, behalve als u wilt splitsen uw toepassing in meerdere sessies gebruikt: een aanroep van deze functie de huidige sessie onmiddellijk, dus uiteindelijk een daarop volgende oproep `startActivity()` een nieuwe sessie wilt starten. Deze functie wordt automatisch aangeroepen door de SDK wanneer uw toepassing wordt gesloten.

##<a name="reporting-events"></a>Voor het melden van gebeurtenissen

### <a name="session-events"></a>Sessiegebeurtenissen

Sessiegebeurtenissen worden meestal gebruikt om de acties die worden uitgevoerd door een gebruiker tijdens de sessie zijn.

**Voorbeeld zonder extra gegevens:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**Voorbeeld met extra gegevens:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a>Zelfstandige gebeurtenissen

In tegenstelling tot de sessiegebeurtenissen, kunnen zelfstandige gebeurtenissen buiten de context van een sessie worden gebruikt.

**Voorbeeld:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

##<a name="reporting-errors"></a>Fouten melden

### <a name="session-errors"></a>Sessie-fouten

Sessie-fouten worden meestal gebruikt om de invloed van de gebruiker tijdens de sessie zijn fouten.

**Voorbeeld:**

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a>Zelfstandige fouten

Met sessie-fouten kunnen zelfstandige fouten worden gebruikt buiten de context van een sessie.

**Voorbeeld:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

##<a name="reporting-jobs"></a>Taken melden

**Voorbeeld:**

Stel dat u wilt melden, de duur van de aanmelding:

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a>Fouten in rapport gedurende een project

Fouten kunnen worden gerelateerd aan een actieve taak in plaats van te wijten aan de huidige gebruikerssessie.

**Voorbeeld:**

Stel dat u wilt een fout gemeld tijdens de aanmelding:

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a>Gebeurtenissen die tijdens een project

Gebeurtenissen kunnen worden gerelateerd aan een actieve taak in plaats van te wijten aan de huidige gebruikerssessie.

**Voorbeeld:**

Stel dat we een sociaal netwerk hebben en we gebruiken een taak voor het rapport van de totale tijd gedurende welke de gebruiker is verbonden met de server. De gebruiker kan berichten ontvangen van zijn vrienden, dit is een taakgebeurtenis.

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

##<a name="extra-parameters"></a>Extra parameters

Willekeurige gegevens kunnen worden gekoppeld aan gebeurtenissen, fouten, activiteiten en taken.

Deze gegevens kan worden gestructureerd, van iOS NSDictionary klasse wordt gebruikt.

Extra's kunnen bevatten `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` of andere `NSDictionary` exemplaren.

> [AZURE.NOTE] De extra parameter wordt omgezet in JSON. Als u wilt dat andere objecten dan de hierboven beschreven doorgeeft, moet u de volgende methode in de klasse implementeren:
>
             -(NSString*)JSONRepresentation;
>
> De methode moet een JSON representatie van het object te retourneren.

### <a name="example"></a>Voorbeeld

    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Limieten

#### <a name="keys"></a>Toetsen

Elke sleutel in de `NSDictionary` moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Betekent dit dat de sleutels met ten minste één letter beginnen moeten, gevolgd door de letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte

Extra's zijn beperkt tot **1024** tekens per gesprek (eenmaal gecodeerd in JSON door de betrokkenheid bij de agent).

In het vorige voorbeeld is het naar de server verzonden JSON 58 tekens:

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Toepassingsgegevens

Handmatig kunt u melden met behulp van het bijhouden van gegevens (of een andere toepassing specifieke gegevens) de `sendAppInfo:` functie.

Merk op dat deze informatie kan stapsgewijs worden verzonden: alleen de meest recente waarde voor een bepaalde sleutel worden opgeslagen voor een bepaald apparaat.

Gebeurtenis extra's, zoals de `NSDictionary` klasse wordt gebruikt voor de toepassingsinformatie als abstract, houd er rekening mee dat matrices of onderliggende woordenboeken worden behandeld als een platte reeks (met JSON serialisatie).

**Voorbeeld:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Limieten

#### <a name="keys"></a>Toetsen

Elke sleutel in de `NSDictionary` moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Betekent dit dat de sleutels met ten minste één letter beginnen moeten, gevolgd door de letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte

Informatie over de toepassingen zijn beperkt tot **1024** tekens per gesprek (eenmaal gecodeerd in JSON door de betrokkenheid bij de agent).

In het vorige voorbeeld is de JSON naar de server verzonden 44 tekens bevatten:

    {"birthdate":"1983-12-07","gender":"female"}
